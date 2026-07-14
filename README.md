# LuauExecutor

A simple wrapper for [LuauDispatcher](https://goobismoobis.github.io/LuauDispatcher/) and [LuauParser](https://vantoanvh.github.io/LuauParser/index.html) that is 10% faster than [vLuau](https://github.com/kosuke14/vLuau) overall.

See both of their documentation websites (hyperlinked above) for info on them specifically.

If you're interested in the exact speed comparisons between LuauExecutor and vLuau check out the [LuauDispatcher Repo](https://github.com/GoobisMoobis/LuauDispatcher)

# Usage

```luau
local LuauExecutor = require(path.to.LuauExecutor)
LuauExecutor(code, options, ...)
```

## Other usage

Types:
- [AbstractSyntaxTree](https://en.wikipedia.org/wiki/Abstract_syntax_tree) (Created by LuauParser based on your code, executed by LuauDispatcher)
- [RuntimeOptions](https://goobismoobis.github.io/LuauDispatcher/api/LuauDispatcher#RuntimeOptions) (Options for when the code runs)
- [ParseOptions](https://vantoanvh.github.io/LuauParser/index.html?view=getting-started) (Options for how the AbstractSyntaxTree is produced)
- Options
	```luau
	Options = {
		ParseOptions: ParseOptions?,
		RuntimeOptions: RuntimeOptions?
	}
 	```
 - [Completion](https://goobismoobis.github.io/LuauDispatcher/api/LuauDispatcher#Completion) (Note that running LuauExecutor() returns one of these)

<hr>

```luau
LuauExecutor:RunCode(code: string, Options: Options?, ...): Completion -- Alternate to running LuauExecutor()
LuauExecutor:Parse(code: string, options: ParseOptions): AbstractSyntaxTree -- Runs LuauParser manually
LuauExecutor:Run(tree: AbstractSyntaxTree, options: RuntimeOptions?, ...): Completion -- Runs LuauDispatcher manually

-- These next few all return connections, but they aren't RbxScriptConnections.
-- LuauDispatcher uses a library called FastSignal for it's connections.
-- You don't need to worry, they still have all the same methods, such as :Disconnect()
LuauExecutor:OnOutput(callback: (...any) -> ()) -- Fires when something is printed
LuauExecutor:OnError(callback: (...any) -> ()) -- Fires when something errors
LuauExecutor:OnWarning(callback: (...any) -> ()) -- Fires when something calls warn
LuauExecutor:OnStatusChange(callback: (boolean) -> ()) -- Fires when the executor begins or stops code execution
LuauExecutor:OnUnhandledReturn(callback: (...any) -> ()) -- Fires when a script returns at the top level (essentially returning a value to nothing). Example: LuauExecutor("return 5" {})
LuauExecutor:WhenLuauRan(callback: (success: boolean, tree: AbstractSyntaxTree) -> ()) -- Fired whenever luau is ran (wow)

LuauExecutor:GetRunningCount(): number -- How many Dispatchers are currently running
```

# Options

```luau
local options = {
  ParseOptions = {
    allowDeclarationSyntax: boolean?,
	  captureComments: boolean?,
	  noErrorLimit: boolean?,
    parseFragment: {
  		localMap: { [string]: AstLocal? },
  		localStack: { AstLocal? },
  		resumePosition: Position,
  	}?,
	  storeCstData: boolean?,
  }
  RuntimeOptions = {
    rawGlobalEnabled: boolean?,
  	overrideCertainGlobalsAtRuntime: boolean?,
  	noEnv: boolean?,
  	instructionLimit: number?,
  	allowEnvironmentModifications: boolean?,
  	allowConsoleOutput: boolean?,
  }
}
```

Documentation for [RuntimeOptions](https://goobismoobis.github.io/LuauDispatcher/api/LuauDispatcher#RuntimeOptions)

Documentation for [ParseOptions](https://vantoanvh.github.io/LuauParser/index.html?view=getting-started)

# Varargs

Varargs are a way for you to give running scripts values they otherwise might not have access to unless you were to modify LuauExecutor.Dispatcher.globals

Modified example from the LuauDispatcher docs:

```luau
LuauExecutor([[
  local msg1, msg2 = ...
  print(msg1.." "..msg2)
]], {}, "Hello", "World")
```
This will print `Hello World`
