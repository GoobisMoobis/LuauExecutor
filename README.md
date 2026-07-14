# LuauExecutor

A simple wrapper for [LuauDispatcher](https://goobismoobis.github.io/LuauDispatcher/) and [LuauParser](https://vantoanvh.github.io/LuauParser/index.html) that is 10% faster than [vLuau](https://github.com/kosuke14/vLuau) overall.

See both of their documentation websites (hyperlinked above) for info on them specifically.

If you're interested in the exact speed comparisons between LuauExecutor and vLuau check out the [LuauDispatcher Repo](https://github.com/GoobisMoobis/LuauDispatcher)

# Usage

```luau
local LuauExecutor = require(path.to.LuauExecutor)
LuauExecutor(code, options, ...)
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
