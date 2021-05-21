# The Basics
Kataru files have a config and passages. The config contains meta information while passages contain multiple lines of dialogue, choices, or commands.

A Kataru file is formatted like this:
```yaml
---
# Config
---
# Passages
```

## Config
The config can contain:
- `namespace`
- `state` declaration
- `characters` declaration
- `commands` declaration
- Built-in events


## Passages
A passage consists of a name and lines. 

Passage names can't contain spaces.

Passage lines can be interpreted as either a Dialogue, Choice, Command, or Input Command, depending on how they're formatted.

```yaml
# A quick overview of possible line types.
Passage:
  - May: Hi! # Dialogue
  - choices: # Choice
      Hi!: Hi
      ...: DotDotDot
  - $May.SetAnimatorTrigger: [dance] # Command
  - input: # Input Command
      $name: What's your name?
```

Multi-line strings in a passage line are possible via the `|-` operator for retaining white space or the `>-` operator for ignoring white space.

```yaml
Passage:
  # A single text block can have many lines using `|`.
  - |-
    Welcome to Kataru!
    「カタル」へようこそ!
    Kataru is a dialogue engine built completely on top of YAML with a focus on ease of implementation and simplicity of writing.

  # If you want to ignore whitespace, use `>-`.
  - >-
    Alice walks into the room,
    her footsteps echoing through the halls.

  # Single line text requires no operator.
  - A simple, single line of text.
```
