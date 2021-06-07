# Dialogue

## Characters
Characters either key to dialogue lines or run <a href="#/concepts/logic?id=character-commands">character commands</a>.

Characters must be declared in the config under `characters` before being used. 

If you are a Unity user, you must also register your characters for them to actually receive lines client-side. See <a href="#/api/unity?id=general-steps-before-registering">Registering characters.</a>

```yaml
---
characters: 
  # Characters that might speak and/or run commands
  June:
  May:
  August:

  # Characters that might run commands
  GlobalLight:
  TriggerArea:
---
Passage:
  June: Hi!
  May: Hi there.
  $GlobalLight.TurnOn: []
```

## Dialogue lines
Dialogue lines are passage lines, optionally prefixed with a character name. Behind the scenes, they're interpreted as maps of character names to their speech text.

If there is no character specified, Kataru will assume the speaker is the one who last spoke.

```yaml
Start:
  - May: Welcome to my story!
  - My name is May.
  - >-
      Here is a long piece of text. It's 
      really really really really long!
```


## Choices
Choices are maps of choice text to passage names.

Choices can optionally have a timeout. Implementing timeout behavior is the client's responsibility.

```yaml
Walk:
  - May: Let's walk around.
  - choices:
      Sure!: WalkSure
      Maybe later? I'm not feeling well...: WalkLater
    timeout: 1

WalkSure:
  - May: Great!

WalkLater:
  - return:
```

## Input

An input command is a special type of <a href="#/concepts/logic?id=commands">command</a> that prompts the user for input.

Input commands can optionally have a timeout. Implementing timeout behavior is the client's responsibility.

```yaml
Passage:
  - input: 
      $name: What's your name?
    timeout: 1
```