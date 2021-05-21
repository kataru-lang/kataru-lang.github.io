Documentation for the [Kataru](https://github.com/Katsutoshii/kataru) repository.

## Understanding the `Bookmark`

Kataru keeps track of your position in a story via a `Bookmark`.
For the simplest stories, this is as simple as keeping track of your current line number.
But nonlinear stories with true agency need to evolve based on the decisions the user made in the past.
Kataru keeps track of the state of the story inside of the `Bookmark` as well.

## Dialogue Runner

The `Runner` class only needs a `passage` name and a `line` number to keep its cursor in the story.

Tree-like structures such as `if` and `else` statements are flattened.

In example,

```yaml
Passage:
  - Line 1
  - if x > 2:
      - Line 3
    else:
      - Line 5
  - Line 6
```

Will be flattened to

```yaml
Passage:
  - Line 1
  - if x > 2 jump to Line 3, else Jump to Line 5
  - Line 3
  - Jump to Line 6
  - Line 5
  - Line 6
```

## Registering Commands

To be completed.