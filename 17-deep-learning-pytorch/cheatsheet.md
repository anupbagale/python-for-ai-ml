# Module 17 Cheatsheet

## Tensors

```python
import torch
from torch import nn

x = torch.tensor([[1.0, 2.0], [3.0, 4.0]])      # copies the data; float32 by default for floats
x.shape, x.dtype, x.device
torch.zeros(3, 4), torch.ones(3), torch.randn(2, 5), torch.arange(10)
x.reshape(4), x.T, x @ x, x.sum(dim=0), x.mean(dim=1, keepdim=True)
torch.from_numpy(array)                          # shares memory with the array
t.numpy()                                        # shares memory too (CPU tensors without gradients)
t.item()                                         # one-element tensor -> Python number
```

- Networks use `torch.float32`; class labels must be `torch.long`.
- Integer tensors can't have gradients.

## Autograd

```python
w = torch.tensor(2.0, requires_grad=True)
loss = (w * 3 - 1) ** 2
loss.backward()                                  # fills w.grad
w.grad                                           # d loss / d w
w.grad.zero_()                                   # gradients ADD UP across backward() calls

with torch.no_grad():                            # don't record operations (evaluation, manual updates)
    w -= 0.1 * w.grad
t.detach()                                       # same values, cut off from the graph
```

## Building a network

```python
class MLP(nn.Module):
    def __init__(self, n_inputs, n_hidden, n_outputs):
        super().__init__()
        self.net = nn.Sequential(
            nn.Linear(n_inputs, n_hidden), nn.ReLU(),
            nn.Linear(n_hidden, n_hidden), nn.ReLU(),
            nn.Linear(n_hidden, n_outputs),       # raw logits: no softmax at the end
        )

    def forward(self, x):
        return self.net(x)

sum(p.numel() for p in model.parameters())       # number of parameters
```

- Layers kept in a plain Python list aren't registered: use `nn.Sequential` or `nn.ModuleList`.

## Data

```python
from torch.utils.data import DataLoader, TensorDataset

train_loader = DataLoader(TensorDataset(X_train, y_train), batch_size=64, shuffle=True)
valid_loader = DataLoader(TensorDataset(X_valid, y_valid), batch_size=256)
```

- Shuffle the training data, not the validation data. Fit any scaling on the training set only.
- Your own data: a class with `__len__` and `__getitem__` (a `Dataset`).

## Training and evaluation

```python
def train_epoch(model, loader, loss_fn, optimizer):
    model.train()
    total, count = 0.0, 0
    for xb, yb in loader:
        optimizer.zero_grad()
        loss = loss_fn(model(xb), yb)
        loss.backward()
        optimizer.step()
        total += loss.item() * len(xb)           # .item(), and weight by batch size
        count += len(xb)
    return total / count


@torch.no_grad()
def evaluate(model, loader, loss_fn):
    model.eval()                                 # dropout off, batch norm uses running statistics
    total, correct, count = 0.0, 0, 0
    for xb, yb in loader:
        logits = model(xb)
        total += loss_fn(logits, yb).item() * len(xb)
        correct += (logits.argmax(dim=1) == yb).sum().item()
        count += len(xb)
    return total / count, correct / count
```

| task | last layer | loss |
|---|---|---|
| classification, k classes | `nn.Linear(h, k)`, logits | `nn.CrossEntropyLoss()` (takes logits and class numbers) |
| binary classification | `nn.Linear(h, 1)`, one logit | `nn.BCEWithLogitsLoss()` |
| regression | `nn.Linear(h, 1)` | `nn.MSELoss()`; watch the shapes: `(n, 1)` vs `(n,)` broadcasts silently |

## Optimisers

```python
torch.optim.SGD(model.parameters(), lr=0.1, momentum=0.9)
torch.optim.Adam(model.parameters(), lr=1e-3)                         # a good default
torch.optim.AdamW(model.parameters(), lr=1e-3, weight_decay=0.01)     # Adam with proper weight decay
```

- Momentum: `v = momentum * v + g`, then `w -= lr * v`.
- Adam: running averages of the gradient and the squared gradient, bias-corrected; each weight moves by about `lr` per step.
- The learning rate is the most important setting. Too high: the loss jumps around or becomes `nan`. Too low: slow progress.

## Initialisation and normalisation

- PyTorch's default initialisation is fine for most networks. For deep ReLU networks: `nn.init.kaiming_normal_(w, nonlinearity="relu")`.
- Too small: the signal shrinks to zero layer by layer. Too large: it explodes.
- `nn.BatchNorm1d(n)` after a linear layer (`nn.BatchNorm2d` for images): normalises each feature over the batch, then rescales with learnt `gamma` and `beta`. Running statistics are used in `eval()` mode.
- `nn.LayerNorm(n)`: normalises over the features of each example instead; used in transformers (Module 19).

## Regularisation

| tool | code |
|---|---|
| dropout | `nn.Dropout(0.3)` after an activation; active only in `train()` mode |
| weight decay | `AdamW(..., weight_decay=0.01)` |
| early stopping | keep the weights from the epoch with the lowest validation loss |
| more data | real data first; data augmentation for images (Module 18) |

- Overfitting: training loss falls while validation loss rises. Validation loss usually shows it before validation accuracy does.
- With dropout, the training loss can sit above the validation loss. That's expected.

## Learning-rate schedules

```python
scheduler = torch.optim.lr_scheduler.CosineAnnealingLR(optimizer, T_max=epochs)        # step once per epoch
scheduler = torch.optim.lr_scheduler.OneCycleLR(optimizer, max_lr=3e-3, epochs=epochs,
                                                steps_per_epoch=len(train_loader))      # step after EVERY batch
scheduler = torch.optim.lr_scheduler.LambdaLR(optimizer, lambda step: factor(step))     # any schedule you like
scheduler.step()                                                                         # after optimizer.step()
```

- Warmup then decay (cosine or one-cycle) is the usual recipe for large networks.

## Devices

```python
device = torch.device("cuda" if torch.cuda.is_available() else ("mps" if torch.backends.mps.is_available() else "cpu"))
model = model.to(device)
xb, yb = xb.to(device), yb.to(device)            # model and data on the same device
```

- In Colab: Runtime > Change runtime type > GPU. GPUs help most for large models and large batches.

## Debugging checklist

1. **Initial loss.** For k balanced classes it should be close to `ln(k)` (2.30 for 10 classes).
2. **Overfit one small batch.** A correct model and loop reach near-zero loss on it. If not, there's a bug.
3. **Watch gradient norms.** Vanishing, exploding or `nan` point at initialisation, normalisation or the learning rate. Safety net: `torch.nn.utils.clip_grad_norm_(model.parameters(), 1.0)` between `backward()` and `step()`.
4. **Classic bugs:** missing `zero_grad()`; softmax before `CrossEntropyLoss`; forgetting `model.eval()`; shape `(n, 1)` vs `(n,)`; labels as floats; inputs not scaled; data and labels shuffled separately.

## Saving and loading

```python
torch.save(model.state_dict(), "model.pt")
model = MLP(64, 128, 10)                         # rebuild the architecture first
model.load_state_dict(torch.load("model.pt", weights_only=True))
model.eval()
```

- Save the preprocessing (scaling values, class names) and the settings needed to rebuild the model alongside the weights.
- Never load a model file from an untrusted source without `weights_only=True`.
