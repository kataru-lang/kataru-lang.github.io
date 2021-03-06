# State

State is the map of all variables used in the story.

## Declaring state

Variables must be declared in the config under `state` before being used. Variable names can contain . and alphanumeric characters. Variable types can be of double, string, or boolean.

```yaml
---
state: 
  var1: 0
  var2: ""
  var3: false
---
```
### Variable expansion (`$passage` and `$character`)

If you want variables defined for each character or each passage, you should use an _expansion keyword_ in your state declaration.
`$character` and `$passage` are both expansion keywords that generate state variables for each corresponding entity in your namespace.

In example, if your story has characters `June`, `May`, and `August` in the current namespace, defining state as

```yaml
state:
  $character.happy: true
```

is equivalent to writing:

```yaml
state:
  June.happy: true
  May.happy: true
  August.happy: true
```

Expansion keywords are useful for creating shared behavior for all passages, such as keeping track of how often each passage has been visited (for more on how you might implement this, see <a href="#/concepts/logic?id=events">events</a>).


## Setting state
To set state in a story, use the `set: {}` command.
```yml
Passage: 
  # Block syntax
  - set:
      $june: 1
      $may: $june
  # Alternatively, you can use flow syntax
  - set: {$june: 2, $may: 4}
```

## Expressions
Kataru supports any level of inline expressions within dialogue lines, conditionals, and `set`.
```yml
Passage:
  - if $june < $may * 5 + 1:
      - June: Test {$june * 4.2}
      - Test 2 $june 
      - set:
        $june +: $may * 5
        $may: (5 + 5) / 10
```
## Conditionals
An `if` statement opens a conditional. Either the end of the passage or a new passage line closes it. 

- If statements comparing numeric values use `==`, `!=`, `<`, `>`, `=<`, `>=`. 
- If statements comparing string values use `==`, `!=`.
- If statements comparing boolean values use `if $var`, `if not $var`, `==`, `!=`.

In the below example, note that `elif` and `else` are within the same passage line; aka, they do not have `-` before them.
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
        - May: How you doing?
        - June: Gah.
    else:
      - May: Poke.
  - May: I'm bored!
```

## Debugging state
If you'd like to debug state while playing, switch the Save Path in Kataru Settings from .bin to .yml like so:
![Change save path in Kataru Settings](DebugState.png)

Then view and/or edit the save file created in that Save Path, relative to Application.persistentDataPath.

A save file will only be located there once <a href="#/api/unity?id=save">`Save()`</a> has been called.

> [!WARNING]
> After changing .bin to .yml for debugging purposes, make sure to change it back before building for players since any player could just open a .yml file and change it if they wanted to.