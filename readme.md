# ActiveScript for AutoHotkey v1.1

Provides an interface to Active Scripting languages like VBScript and JScript, without relying on Microsoft's ScriptControl, which is not available to 64-bit programs.

**License:** Use, modify and redistribute without limitation, but at your own risk.

## Usage

Save `ActiveScript.ahk` and `JsRT.ahk` (if needed) in a [Lib folder](http://ahkscript.org/docs/Functions.htm#lib).

### ActiveScript

Supports JScript, VBScript and possibly other scripting engines which are registered with COM and implement the IActiveScript interface.

```AutoHotkey
    #Include <ActiveScript>

    script := new ActiveScript("JScript")
    script := new ActiveScript("VBScript")
```

More examples are included in the Example\*.ahk files.

### JsRT

Supports JavaScript as implemented in IE11 or Edge (Windows 10).

```AutoHotkey
	#Include <ActiveScript>
	#Include <JsRT>
	
	script := new JsRT.IE  ; IE11 feature set.
	script := new JsRT.Edge  ; Edge feature set.
```

More examples are included in Example_JsRT.ahk.

## Methods

### Eval

Evaluate an expression and return the result.

    Result := script.Eval(Code)

### Exec

Execute script code.

    script.Exec(Code)

### AddObject

Add an object to the global namespace of the script.

    script.AddObject(Name, DispObj, AddMembers := false)

*Name* is required and must be unique.

If *AddMembers* is true, the object's methods and properties will be added to the script's global namespace instead of the object itself. If omitted, it defaults to *false*.

*DispObj* must be either an object which implements the IDispatch interface, passed either via a ComObject wrapper or by address. Can be an AutoHotkey object if running on AutoHotkey v1.1.17 or later.

Evaluating code with *Eval* or *Exec* may also add global variables and functions.

With AutoHotkey v1.1.17 or later, `script[Name] := DispObj` will usually have the same effect if *AddMembers* is false or omitted.

**JsRT:** *AddMembers* must be false. *DispObj* can be any value, and will be added as is. Do not pass a pointer, or it will be added as a number.

### Anything else

To call functions or retrieve or set variables defined in the script,  use normal object notation on the ActiveScript object.  For example:

    result := script.MyFunc()
    value := script.globalvar
    script.globalvar := value

New VBScript variables cannot be created this way. New JScript variables can be created this way only on AutoHotkey v1.1.18 and later.

New variables can be created by declaring them in script with Exec() or Eval().

The hosted script can be given access to AutoHotkey functions by using the `Func()` function:

```AutoHotkey
    script.alert := Func("alert")
	alert(message) {
		MsgBox 48, Message from script, %message%
	}
```