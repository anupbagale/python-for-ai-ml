# Data

## Tiny Shakespeare (lesson)

`tiny_shakespeare.txt` (1.1 MB) is about 40,000 lines from Shakespeare's plays joined into one plain text file: 1,115,394 characters, using 65 different characters (letters, digits, punctuation, space and new line). Each speech is written as the speaker's name in capitals followed by a colon, then the lines, with a blank line between speeches.

The lesson uses the first 90% of the file for training and the last 10% for validation.

**Source.** Collected by Andrej Karpathy for his char-rnn project (2015): https://github.com/karpathy/char-rnn, file `data/tinyshakespeare/input.txt`, in a repository released under the MIT licence. Shakespeare's plays themselves are in the public domain.

The notebook reads the copy in this folder, or downloads it from this repository on GitHub if the folder isn't there (for example on Colab).

## Sums (mini project)

The mini project makes its own data: random pairs of 5-digit numbers and their sums, generated with a fixed random seed. Nothing is downloaded.
