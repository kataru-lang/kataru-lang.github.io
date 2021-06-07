Documentation for the [Unity-Kataru](https://github.com/kataru-lang/unity-kataru) repository, which acts as a middle-man between Unity and Kataru. 

## Kataru Settings
Kataru Settings can be accessed via Project Settings/Kataru Settings or Kataru Settings.asset in the Assets folder. There are a couple important things to note:

- All files written in Kataru that will be compiled into the final story should be in the "Story Path".
- The "Bookmark Path" and "Target Path" are relative to StreamingAssets.
- The "Save Path" is relative to Application.persistentDataPath.
- Lastly, Kataru Settings.asset must stay at root level in a Resources folder.

As long as the above is followed, feel free to move files around.

## Inheriting from Handler
The `Handler` class automatically manages subscription and disposal of listeners to the Runner.
All MonoBehaviors that need to access data from Kataru should inherit from Handler and register their methods using `CommandHandler` and `CharacterHandler` attributes.

### General steps before registering
1. Inherit class from `Handler`
2. Declare the character or command in the relevant config

### Registering characters
To register a character:
1. Override the `Name` property to return the character's name
2. Tag the function with the attribute `[CharacterHandler]`

An example can be found [here](https://github.com/kataru-lang/unity-kataru-demo/blob/main/Assets/Scripts/Kataru/KataruSpeaker.cs).

### Registering commands
To register your own c-sharp method as a command, tag it with the attribute `[CommandHandler]`.

To register character commands:
1. Override the `Name` property to return the character's name
3. In the `CommandHandler` attribute, add `local: true` as a parameter.

To manually declare when the next Kataru line should be run:
1. In the `CommandHandler` attribute, add `autoNext: false` as a parameter.
2. Call `Runner.Next` or `Runner.DelayedNext` to declare when to trigger the next line.

An example can be found [here](https://github.com/kataru-lang/unity-kataru-demo/blob/main/Assets/Scripts/Kataru/KataruScene.cs).

## Class Runner

### Properties

#### isRunning => Bool
Is a passage being currently run or not?
```csharp
public static bool isRunning = false;
```

### Methods

#### Init()
Initialize the Runner. This must be called before any other method is called.
```csharp
public static void Init()
```

#### Save()
Save the bookmark to the save path determined by Kataru Settings.
```csharp
public static void Save()
```

#### SaveExists() => Bool
Return true if a save file exists at the save path.
```csharp
public static bool SaveExists()
```

#### DeleteSave()
Delete the save file at the save path if it exists.
```csharp
public static void DeleteSave()
```

#### Load()
Load bookmark from the save path if a save exists.
```csharp
public static void Load()
```

#### SaveSnapshot(String)
Save progress on a separate stack keyed with `name`. This makes it possible to keep track of progress in multiple different conversations.
```csharp
public static void SaveSnapshot(string name)
```

#### LoadSnapshot(String)
Load progress from stack keyed with `name`.
```csharp
public static void LoadSnapshot(string name)
```

#### SetState(String, String|Double|Bool)
Set the state variable with the given key to the given value.
```csharp
public static void SetState(string key, string value)
public static void SetState(string key, double value)
public static void SetState(string key, bool value)
```

#### GetPassage() => String
Get the current passage name. Returns empty if no passage running.
```csharp
public static string GetPassage()
```

#### RunPassage(String)
Run the given passage.
```csharp
public static void RunPassage(string passage)
```

#### RunPassageUntilChoice(String)
Run a passage until a choice is encountered.
```csharp
public static void RunPassageUntilChoice(string passage)
```

#### Next(String?) => LineTag
Tell Kataru to run the next passage line.
```csharp
public static LineTag Next(string input = "")
```

#### DelayedNext(Float|Func<Bool>, String?) => IEnumerator
Call `Next` after a given amount of seconds or a given predicate returns true.
```csharp
public static IEnumerator DelayedNext(float seconds, string input = "")
public static IEnumerator DelayedNext(Func<bool> predicate, string input = "")
```

### Events

#### OnLine(LineTag)
Called every time a passage line is run.
```csharp
public static event Action<LineTag> OnLine;
```

#### OnChoices(Choices)
Called every time a choice is encountered.
```csharp
public static event Action<Choices> OnChoices;
```

#### OnDialogueEnd()
Called when Runner is finished running. Use this for any cleanup like hiding dialogue bubbles, etc.
```csharp
public static event Action OnDialogueEnd;
```

#### OnInputCommand(InputCommand)
Called when an input command is encountered.
```csharp
public static event Action<InputCommand> OnInputCommand;
```
