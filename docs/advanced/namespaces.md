# Namespaces

Namespaces are declared in the config. Each file can only have one namespace. Multiple files can have the same namespace and will get merged when compiled.

```yaml
---
namespace: global
---
```

## Referencing other namespaces

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
  - Anyone home?
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

## `global` namespace

If a file's namespace is set to `global`, all state/characters/commands/passages in that file can be referenced by outside namespaces without needing to prefix it with `global:`.

```yaml
# Global.yml
---
namespace: global

characters:
  - June:
---
EndDialogue: 
  - return:
```

```yaml
# MaysWorld.yml
---
namespace: MaysWorld

characters:
  - May:
---
PassageB:
  - May: Hi June!
  - choices:
      Hi.: PassageHi
      ...: EndDialogue

PassageHi:
  - June: Hey.
```