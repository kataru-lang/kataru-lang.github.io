# How Would I...

## Mark a passage as visited?

Check out <a href="#/syntax?id=onenter">onEnter</a> or <a href="#/syntax?id=onexit">onExit.</a>

## End dialogue prematurely?

Create a global passage that only <a href="#/syntax?id=return">returns</a>. Call this whenever you have this use case.

```yml
Passage:
  - choices:
        Hi!: Hi
        Bye: EndDialogue

EndDialogue:
  - return:
```

## Type yes/*/some special string sequence without throwing a syntax error?
A limitation of the YML language is that it throws unexpected errors sometimes on things like 'yes' because it interprets that as a special value. To get around this, just wrap the troublesome sequence in double quotes.