# Syntax Reference
This is a quick syntax reference guide. For examples on how to use it, see [Examples](examples.md).


Kataru files have a config and passages. The config contains meta information like state variables and character names. Passages contain multiple lines of dialogue, choices, or commands.

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

### `namespace`

Each file can only have one namespace. Multiple files can have the same namespace and will get merged when compiled.

```yaml
---
namespace: global
---
```

#### Referencing other namespaces

When wanting to reference a non-global namespace's passage/state/character:
- If referencing outside that namespace, prefix the reference with `NAMESPACE_NAME:`
- If the reference is called within that namespace, forgo prefixing

Here's an example (note that they're separate files):

```yaml
# JunesWorld.yml
---
namespace: JunesWorld

characters:
  - June:
---
PassageA:
  - June: Hi.
```

```yaml
# MaysWorld.yml
---
namespace: MaysWorld

characters:
  - May:
---
PassageB:
  - May: June, are you there?
  - call: JunesWorld:PassageA
  - JunesWorld:June: Yeah, I'm here.
```

#### `global`

If a file's namespace is set to `global`, all state/characters/commands/passages in that file can be referenced by outside namespaces without needing to prefix it with `global:`.

### `state`

Variables must be declared under `state` before being used.
Read more on variables <a href="#/syntax?id=variables">here</a>.

```yaml
---
state: 
  var1: 0
  var2: ""
  var3: false
---
```

#### Variable expansion (`$passage` and `$character`)

If you want variables defined for each character or each passage, you should use an _expansion keyword_ in your state declaration.
`$character` and `$passage` are both expansion keywords that generate state variables for each corresponding entity in your namespace.

In example, if your story has characters `Alice`, `Bob`, and `Charlie` in the current namespace, defining state as

```yaml
state:
  $character.happy: true
```

is equivalent to writing:

```yaml
state:
  Alice.happy: true
  Bob.happy: true
  Charlie.happy: true
```

Expansion keywords are useful for creating shared behavior for all passages, such as keeping track of how often each passage has been visited (see TODO example for how to implement this).

#### `$passage`
The keyword `$passage` will be replaced by 
### `characters`

### `commands`
#### `$character`


### Built-in Events

#### `onEnter`

#### `onExit`

---

## Passages

Passage names can't contain spaces.

Any dialogue line that contains only a YAML string is interpretated as narration text.
YAML supports different ways of multi-lining strings, including the `|` operator for retaining white space and `>-` operator for ignoring white space.

```yaml
---
# No config required for this scene.
---
Passage:
  # A single text block can have many lines using `|`.
  - |
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

---
### Line Types

Every line in a passage will be interpreted differently depending on how it's formatted.
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

#### Dialogue

Dialogue lines are maps of configured character names to their speech text.

```yaml
---
# Define your character(s)
characters:
  Alice:

---
Start:
  - Alice: Welcome to my story!
```

#### Choice

Choices can have a timeout.

#### Command

Commands are delineated by `[]`, and are recognized by the YAML parser as arrays.
Any dialogue lines that are arrays are viewed as list of commands to be executed by the client.

There are a few <a href="#/syntax?id=built-in-commands">built-in commands</a>. To learn how to use outside commands, read the <a href="#/documentation/unity?id=registering-commands">Unity-Kataru</a> or <a href="#/documentation/kataru?id=registering-commands">Kataru documentation</a>.

```yaml
# Define commands and their default parameters.
cmds:
  ClearScreen: { duration: 0 }
---
# Call them in a passage
Passage:
  - ClearScreen: {} # Call with default parameters.
  - ClearScreen: { duration: 1 } # Call with overriden parameters.
  - ClearScreen: { 1 } # Call with positional parameters
```

Two ways to write commands:
```yaml
Passage:
  - Foo: { bar: a } # Call with named parameters.
  - Foo: [ a ] # Call without.
```

To call local commands

#### Input Command

---
### Variables
Variables get declared in the `state` config. Variable names can contain . and alphanumberic characters. 
#### Conditionals

An `if` statement opens a conditional. Either the end of the passage or a new passage line closes it. 
- If statements comparing numeric values use `==`, `!=`, `<`, `>`, `=<`, `>=`. 
- If statements comparing string values use `==`, `!=`.
- If statements comparing boolean values use `if $var`, `if not $var`, `==`, `!=`.

```yml
---
state:
  $passage.completed: 0
  $grumpy: false

onExit:
  set:
    $passage.completed +: 1
---
JuneTalk:
  - if JuneTalk > 2:
      - June: I'm tired of talking to you!
    elif JuneTalk == 1:
      - if not $grumpy:
        - May: How you doin?
        - June: Gah.
    else:
      - May: Poke.
  - May: I'm bored!
```

#### `set`

To set state in a story, use the `set: {}` command.
```yml
Passage: 
  - set:
      $june: 1
      $may: $june
```

#### Inline Expressions
Kataru supports any level of inline expressions within dialogue lines, conditionals, and `set`.
```yml
Passage:
  - if $june < $may * 5 + 1:
      June: Test ${june * 4.2}
      set:
        $june +: $may * 5
        $may: (5 + 5) / 10
```
---

## Built-in Commands
Some commands that are expected to be used a lot are pre-built into Kataru.
### `call`
Jump to another passage.
```yml
Test:
  - May: Starting a passage!
  - call: Test2
  - May: I finished the passage!
```
### `return`
Force exit out of a passage.
```yml
Test:
  - return:
  - May: Aw, this'll never get called.
```