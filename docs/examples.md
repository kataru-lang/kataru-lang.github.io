# Examples <!-- {docsify-ignore-all} -->

## Unity-Kataru

### Unity Kataru Demo 

[(Github)](https://github.com/kataru-lang/unity-kataru-demo)

<img src="https://github.com/kataru-lang/unity-kataru-demo/raw/main/Screenshot2.png" height="500">

### Ribbons

<iframe width="560 !important" height="315" src="https://www.youtube.com/embed/cM8g9otb2A8" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Mirror rorriM

<iframe width="560 !important" height="315" src="https://www.youtube.com/embed/GpKFAkyGaVY" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Kataru

See [kataru-lang/kataru/examples/simple](https://github.com/kataru-lang/kataru/tree/main/examples/simple) for a minimal example running the engine in the terminal.


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

  $character.SetAnimatorTrigger:
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
  - May.SetAnimatorTrigger: [ "drinkcoffee" ]
  - call: End

NoCoffee:
  - May: No thanks.
  - Wait: { duration: 1 }
  - June: Want to end this story?
  - call: End

End:
  - May: The end!
```