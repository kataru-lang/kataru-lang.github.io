# FAQ

## Adoption
### How to mark a passage as visited?
Check out <a href="#/concepts/logic?id=onenter">onEnter</a> or <a href="#/concepts/logic?id=onexit">onExit.</a>

### How to end dialogue prematurely?
Create a global passage that only <a href="#/syntax?id=return">returns</a>. Call this whenever you have this use case.

```yml
Passage:
  - choices:
        Hi!: Hi
        Bye: EndDialogue

EndDialogue:
  - return:
```

### How to keep track of multiple conversations?
If you want to make something like a messaging app where you're juggling multiple conversations and are at different line numbers for all of them, you could do something like:
```csharp
// Called when clicking on a user to see your messages with them
public void OnSwitchingToChatUser(string name){
  Runner.SaveSnapshot(previousName);
  // Load UI, instantiate previously cached text messages, etc
  Runner.LoadSnapshot(name);
}
```

---

## Technical
### When I type yes/*/etc, I get a syntax error?
A limitation of the YML language is that it throws unexpected errors sometimes on things like 'yes' because it interprets that as a special value. To get around this, just wrap the troublesome sequence in double quotes.

### Auto-formatting long lines looks weird?
If you're using Prettier and VSCode, in settings, try "prettier.printWidth": 120.

