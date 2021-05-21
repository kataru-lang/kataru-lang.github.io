# Examples <!-- {docsify-ignore-all} -->

## Unity-Kataru

Handles dialogue and story state in Mirror rorriM (currently in progress).
<iframe width="560 !important" height="315" src="https://www.youtube.com/embed/WRydvrfUo7c" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Kataru

See [kataru-lang/kataru/examples/simple]([./examples/simple](https://github.com/kataru-lang/kataru/tree/main/examples/simple)) for a minimal example running the engine in the terminal.


---

## Basic Template

Feel free to copy the below template into any empty .yml file to get started.

```yml
---
# Define what namespace this file is in.
namespace: global

state: 
  coffee: 0
  $passage.completed: 0

# Configure the scene. List your characters, commands, etc.
characters:
  May:
  June: 

commands:
  Wait:
    duration: 0.3

  $character.PlaySfx:
    clip: ""

onExit:
  set:
    $passage.completed +: 1
---

Start:
  - May: Welcome to my story!
  - June: Want a coffee?
  - choices:
      Yes: YesCoffee
      No: NoCoffee

YesCoffee:
  - May: Yeah, thanks!
  - set:
        $coffee +: 1
  - May.PlaySfx: [ "HappyShout" ]
  - call: End

NoCoffee:
  - May: No thanks.
  - Wait: { duration: 1 }
  - June: Want to end this story?
  - call: End

End:
  - May: The end!
```