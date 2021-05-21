# Logic

## Commands
Commands perform functionality outside of dialogue or choices. There's multiple ways to call them, as seen below.

```yaml
commands:
  FadeToBlack:
    duration: 0.5

  $character.WalkTo:
    destination: ""
---
Passage:
  - FadeToBlack: {} # Call with default parameters.
  - FadeToBlack: [] # Alternative way to do the above.
  - FadeToBlack: { duration: 1 } # Call with named parameters.
  - FadeToBlack: [ 1 ] # Call with positional parameters.
  - $May.WalkTo: [ "June" ] # Call character-specific command.
```

### Declaring commands
Commands must be declared in the config under `commands` before being used. 

```yaml
---
commands: 
  Command1:

  Command2:
    param1: 0
    param2: ""

  $character.Command1:
    param1: false
---
Passage: 
  - Command1: []
```

### Character-specific commands
To call the same command on multiple different instances, utilize character-specific commands. For example, if we have a `TurnOffLight` command and multiple lights we want to call this on, we could do the following:

```yaml
---
characters:
  LightA:
  LightB:
  May:  

commands: 
  $character.TurnOffLight:
---
PassageA:
  - $LightA.TurnOffLight: []
  - May: Who turned off that light!?
  - $LightB.TurnOffLight: []
  - May: I can't see!
```

Character-specific commands can be used on characters that speak as well. For example, say we have a couple of characters who should look happy or sad at the right moments. We could do this:

```yaml
---
characters:
  May:
  August:

commands: 
  $character.SetAnimatorTrigger:
    trigger: ""
---
AugustComesHome:
  - $August.SetAnimatorTrigger: ["sad"]
  - August: Work went late; sorry I missed dinner.
  - $May.SetAnimatorTrigger: ["happy"]
  - May: It's okay! 
  - I'm just glad you're home!
```
Ultimately, characters are flexible, and combined with commands, can minimize code redundancy.

### Built-in commands
Some commands that are expected to be used a lot are pre-built into Kataru.
#### `call`
Jumps to another passage.
```yml
Test:
  - May: Loading 0%...
  - call: Test2
  - May: Loading 100%!

Test2:
  - May: Loading 50%...
```
#### `return`
Force exit out of a passage.
```yml
Test:
  - return:
  - May: This will never get called.
```

### Registering external commands
To learn how to register your own functions as commands, read the <a href="#/api/unity?id=general-steps-before-registering">Unity-Kataru</a> or <a href="#/api/kataru?id=registering-commands">Kataru documentation</a>.

## Events
Each config can optionally listen to per-passage events. These events will raise on every passage within the config's namespace.

### `onEnter`
This event is called upon entering a passage. 
```yaml
---
state:
  $passage.visited: 0

onEnter: 
  set:
    $passage.visited +: 1
---
Test:
  - if $Test.visited == 0: # This will never get called because $Test.visited will have already incremented before we evaluate this condition
      - return: 
    elif $Test.visited == 1: # This will get called the first time we visit Test
      - return:
    else:
      - return:
```
### `onExit`
This event is called upon exiting a passage.
```yaml
---
state:
  $passage.completed: 0

onExit: 
  set:
    $passage.completed +: 1
---
Test:
  - if $Test.completed == 0: # This will get called the first time we visit Test
      - return: 
    elif $Test.completed == 1: 
      - return:
    else:
      - return:
```
---