# Attributes

Attributes are declared in the config, then used in dialogue lines like rich text tags. They're a great way to do multiple things in a dialogue line succinctly.

## Declaring in config

Declare that an attribute should be parsed by Kataru like the following:

```yaml
---
namespace: global

# Example usage of the following attributes:
# Hina: <emote=sad/><sfx=hey/>Hey!<impulse=3/>
# Sakura: Oh. <size=20>Hina...</size>
attributes:
  emote: happy
  sfx: hey
  impulse: 4
  size: 2
---
```

## Macros

Macros are attributes that contain other attributes with specific values, very similar to TextMeshPro's styles.

For example, given the following config:

```yaml
---
namespace: global

attributes:
    happy:
        emote: happy
        sfx: yay
        impulse: 2
  
---
```

The dialogue line:
```yaml
Hina: <happy/>I can't believe it!
```
Would be parsed as:
```yaml
Hina: <emote=happy/><sfx=yay/><impulse=2/>I can't believe it!
```


Note that you don't need to declare attributes within macros outside of the macro, unless you plan on using it outside of the macro.


## Referencing in script

To parse the attributes given to you in script, you could do something like the following:

```yaml
---
namespace: global

attributes:
    happy:
        emote: happy
        sfx: yay
        impulse: 2
---

Start:
  - May: <happy/>June, are you there?
```

```csharp
 private void SayLine(Kataru.Dialogue dialogue)
{
    // ...Do dialogue things
    ProcessAttributes(dialogue);
}

void ProcessAttributes(Kataru.Dialogue dialogue)
{
    foreach (var attribute in dialogue.attributes)
    {
        if (attribute.parameters.TryGetValue("sfx", out object
            sfx))
        {
            Debug.Log("found sfx attribute!");
            string sfxVal = (string)sfx; // need to convert the object into expected type
        }
        if (attribute.parameters.TryGetValue("emote", out object emote))
        {
            Debug.Log("found emote attribute!");
            string emoteVal = (string)emote;
        }
        if (attribute.parameters.TryGetValue("impulse", out object impulse))
        {
            Debug.Log("found impulse attribute!");
            double impulseVal = (double)impulse;
        }
        Debug.Log($"{attribute.start} {attribute.end}"); // do with this what you will
    }
}
```
