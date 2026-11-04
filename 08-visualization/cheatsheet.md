# Module 8 Cheatsheet

## Structure

```python
import matplotlib.pyplot as plt

fig, ax = plt.subplots(figsize=(7, 4))                        # one panel
fig, axes = plt.subplots(2, 3, sharex=True, sharey=True)     # grid of panels (array of Axes)
ax.set_title("A title that states the finding")
ax.set_xlabel("Body mass (g)"); ax.set_ylabel("Count")
ax.legend(loc="upper left")
fig.suptitle("...", x=0.01, ha="left")
fig.tight_layout()
plt.show()
```

Functions that draw should **return `fig, ax`**, not call `plt.show()`.

## Marks

```python
ax.plot(x, y, color=PALETTE[0], linewidth=1.6, label="train")
ax.scatter(x, y, color=c, marker="o", s=26, edgecolors=SURFACE, linewidths=0.6)
ax.hist(values, bins=edges, color=c, edgecolor=SURFACE, linewidth=1)
ax.barh(labels, values, color=PALETTE[0], height=0.55)
ax.imshow(matrix, cmap=SEQUENTIAL)                  # heatmap
ax.contourf(gx, gy, regions, levels=..., alpha=0.18)
ax.fill_between(x, low, high, alpha=0.12, linewidth=0)
ax.axhline(100, color=AXIS); ax.axvline(best, color=MUTED)
ax.hexbin(x, y, gridsize=35, cmap=SEQUENTIAL, mincnt=1)
```

## Text and annotation

```python
ax.text(x, y, "label", ha="center", va="center", color=INK_2)
ax.text(0.99, 0.9, "corner", transform=ax.transAxes, ha="right")   # axes coordinates
ax.annotate("chosen", xy=(x, y), xytext=(40, -20), textcoords="offset points",
            arrowprops={"arrowstyle": "-", "color": MUTED})
fig.text(0.01, 0.01, "Data: ...", color=MUTED, fontsize=8)          # source note
```

## Axes

```python
ax.set_xlim(0, top)                     # bars start at 0
ax.set_yscale("log")
ax.set_yticks([0.03, 0.1, 0.3]); ax.yaxis.set_major_formatter(FuncFormatter(lambda v, _: f"{v:g}"))
ax.set_xticks(range(n), labels, rotation=30, ha="right")
ax.grid(axis="y", visible=False)
locator = mdates.AutoDateLocator()
ax.xaxis.set_major_locator(locator)
ax.xaxis.set_major_formatter(mdates.ConciseDateFormatter(locator))
```

## Colour: four jobs

| job | use | rule |
|---|---|---|
| identity | `PALETTE[i]` | fixed order; colour follows the entity, not its position |
| magnitude | `SEQUENTIAL` | one hue, light to dark |
| polarity | `DIVERGING`, `vmin=-1, vmax=1` | two opposite hues, grey midpoint |
| state | reserved colours | always with a label or icon |

Never a rainbow. Never colour alone: add markers, labels or a legend. Text in ink colours, not series colours.

## Which chart?

| question | chart |
|---|---|
| distribution of one variable | histogram (try several bin counts), box plot, violin |
| relationship between two | scatter (colour + marker by group) |
| groups on one number | sorted bar chart, starting at 0 |
| change over time | line, with a rolling average |
| two measures, different units | index both to 100, **one** axis |
| many pairs | correlation heatmap, pair plot |
| one number | a sentence, not a chart |

## Small multiples

```python
edges = np.linspace(values.min(), values.max(), bins + 1)   # same bins in every panel
fig, axes = plt.subplots(n, 1, sharex=True, sharey=True)
```

## Overplotting

`alpha=0.15` and small markers, or `hexbin`.

## Seaborn

```python
import seaborn as sns
sns.histplot(data=df, x="col", hue="group", hue_order=ORDER, palette=COLOURS, ax=ax)
sns.violinplot(data=df, x="value", y="group", order=ORDER, hue="group", hue_order=ORDER,
               palette=COLOURS, legend=False, ax=ax)
sns.pairplot(df.sort_values("group"), vars=[...], hue="group", hue_order=ORDER, palette=COLOURS)
```

Always set readable axis labels afterwards (Seaborn uses column names).

## Multi-panel reports

```python
from matplotlib.gridspec import GridSpec
fig = plt.figure(figsize=(13, 8.5))
gs = GridSpec(2, 3, figure=fig, hspace=0.45, wspace=0.3)
ax = fig.add_subplot(gs[0, 0])          # gs[0, 1:] spans columns
```

## Saving

```python
fig.savefig(path.with_suffix(".png"), bbox_inches="tight")   # PNG for slides
fig.savefig(path.with_suffix(".svg"), bbox_inches="tight")   # SVG for documents and web
plt.close(fig)                                                # in loops, always
```

## Honest-chart checklist

- Title states the finding; axes have labels with units
- One y-axis; bars start at zero
- Same bins and shared axes across small multiples
- Fixed colour scale for correlations (`vmin=-1, vmax=1`)
- Split by group before trusting a pooled trend (Simpson's paradox)
- Colour follows the entity; never colour alone
- Source note, and alt text that describes the findings
