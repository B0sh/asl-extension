# Snippets

The following are custom code snippets currently supported by this extension.  
These are merely examples. The variable types and names can be modified by the user upon use of a snippet.  
For a complete overview of all supported snippets, view [../snippets](../master/snippets).

---
## State
#### `state`

*Describes the process the script should connect to.*
```cs
state("{1:process}")
{
	{0}
}
```

---
#### `stv`, `state_version`

*Describes the process the script should connect to, along with a version identifier.*
```cs
state("{1:process}", "{2:version}")
{
	{0}
}
```

---
#### `sptr`, `state_pointer`

*Creates a pointer path in the state descriptor to be used within the script.*  
*When using Cheat Engine, be sure to enter the pointer's offsets from the bottom upwards.*
```cs
{1:int} {2:name} : "{3:module}", {4:baseOffset}, {5:offsets}; // {6:description}
```



---
## Startup Section
#### `su`, `startup`

*This action runs when the script is first loaded.*  
*This includes saving the current .asl file, where the script is reconnected.*
```cs
startup
{
	// vars.Log = (Action<object>)((output) => print("[{1:Game} ASL] " + output));

	{0}
}
```

---
#### `addsetting`

*Creates an entry to be added to the Auto Splitter's settings.*
```cs
settings.Add("{1:id}", {2:true}, "{3:description}");
```

---
#### `addsc`, `add_setting_child`

*Creates an entry to be added to the Auto Splitter's settings.*  
*The setting will be a child to a parent settings entry.*
```cs
settings.Add("{1:id}", {2:true}, "{3:description}", "{4:parent}");
```

---
#### `tt`, `tooltip`

*Creates a tooltip to assign to a setting.*
```cs
settings.SetToolTip("{1:id}", "{2:tooltip}");
```



---
## On Timer Event
#### `onStart`

*This action runs when the timer is started, independent from whether the script or the user did it.*
```cs
onStart
{
	{0}
}
```

---
#### `onSplit`

*This action runs when a split is made, independent from whether the script or the user did it.*
```cs
onSplit
{
	{0}
}
```

---
#### `onReset`

*This action runs when the timer is reset, independent from whether the script or the user did it.*
```cs
onReset
{
	{0}
}
```



---
## Script Blocks
#### `init`

*This action runs when the process has been found according to at least one of the state descriptors.*
```cs
init
{
	{0}
}
```

---
#### `update`

*This action runs first on every iteration.*  
*Explicitly returning false disables all other actions.*
```cs
update
{
	{0}
}
```

---
#### `start`

*Returning true in this action will start the timer and run the onStart action.*  
*This action will only run when the timer is not running.*
```cs
start
{
	return {0:condition};
}
```

---
#### `split`

*Returning true in this action will trigger a split and run the onSplit action.*  
*This action will only run when the timer is running.*
```cs
split
{
	return {0:condition};
}
```

---
#### `reset`

*Returning true in this action will reset the timer and run the onReset action.*  
*This action will only run when the timer is running.*
```cs
reset
{
	return {0:condition};
}
```

---
#### `gt`, `gameTime`

*Returning a TimeSpan object in this action will set the timer's game time to the return value.*  
*When using this block, it is usually recommended to also return true in isLoading.*
```cs
gameTime
{
	return TimeSpan.FromSeconds({0});
}
```

---
#### `il`, `isLoading`

*Returning true in this action will pause the timer's game time.*
```cs
isLoading
{
	return {0:condition};
}
```

---
#### `exit`

*This action is run when the currently attached process exits.*
```cs
exit
{
	{0}
}
```

---
#### `sd`, `shutdown`

*This action is run when the script is entirely disconnected from LiveSplit.*  
*Examples include when the Auto Splitter is disabled, LiveSplit exits, the script path is changed, or the script is reloaded.*
```cs
shutdown
{
	{0}
}
```



---
## Useful Snippets
#### `cw`, `print`

*Prints the specified string to the output.*  
*Use TraceSpy or DebugView to read the output.*
```cs
print("{0}");
```

---
#### `log`

*Uses vars.Log to print any type of object to the output.*  
*Use TraceSpy or DebugView to read the output.*
```cs
vars.Log({0});
```

---
#### `rptr`, `readptr`, `readpointer`

*Reads an address at another specified address.*
```cs
game.ReadPointer({1:address})
```

---
#### `rv`, `readvalue`

*Reads a specified type value at a specified address.*
```cs
game.ReadValue<{1:int}>({2:address})
```

---
#### `rstr`, `readstring`

*Reads a string at a specified address.*
```cs
game.ReadString({1:address}, {2:length})
```

---
#### `scanpages`, `scanmemorypages`

*Creates a foreach loop over all MemoryPages currently loaded by the process, scanning a specified SigScanTarget on each of them.*
```cs
var {1:result} = IntPtr.Zero;
foreach (var page in game.MemoryPages(all: {2:false}))
{
	var scanner = new SignatureScanner(game, page.BaseAddress, (int)(page.RegionSize));
	if (({1} = scanner.Scan({3:SigScanTarget})) != IntPtr.Zero)
		break;
}
```



---
## General Objects
#### `dp`, `deep_pointer`

*Creates a DeepPointer object, often used when working with MemoryWatchers or more sophisticated memory-management.*
```cs
new DeepPointer({0:pointerpath})
```

---
#### `mw`, `memwatcher`, `memorywatcher`

*Creates a MemoryWatcher object, often used when building pointers manually.*  
*A MemoryWatcher's first parameter is either an IntPtr or a DeepPointer object.*
```cs
new MemoryWatcher<{1:int}>({0:IntPtr or DeepPointer})
```

---
#### `mwn`, `memwatcher_named`, `memorywatcher_named`

*Creates a MemoryWatcher object, often used when building pointers manually.*  
*A MemoryWatcher's first parameter is either an IntPtr or a DeepPointer object.*  
*Useful with MemoryWatcherLists.*
```cs
new MemoryWatcher<{1:int}>({0:IntPtr or DeepPointer}) { Name = "{2:name}" }
```

---
#### `sw`, `stringwatcher`

*Creates a StringWatcher object, often used when building pointers manually.*  
*A StringWatcher's first parameter is either an IntPtr or a DeepPointer object.*
```cs
new StringWatcher({0:IntPtr or DeepPointer}, {1:length})
```

---
#### `swn`, `stringwatcher_named`

*Creates a StringWatcher object, often used when building pointers manually.*  
*A StringWatcher's first parameter is either an IntPtr or a DeepPointer object.*  
*Useful with MemoryWatcherLists.*
```cs
new StringWatcher({0:IntPtr or DeepPointer}, {1:length}) { Name = "{2:name}" }
```

---
#### `mwl`, `memwatcherlist`, `memorywatcherlist`

*Creates a list of MemoryWatchers.*
```cs
// To access a watcher, each one must be given a name.
vars.{1:Watchers} = new MemoryWatcherList
{
	{0}
};
```

---
#### `scn`, `scanner`, `signaturescanner`

*Creates a SignatureScanner used to scan SigScanTargets.*
```cs
new SignatureScanner({1:game}, {2:module}.BaseAddress, {2}.ModuleMemorySize);
```

---
#### `trg`, `target`, `sigscantarget`

*Creates a SigScanTarget to scan.*
```cs
new SigScanTarget({1:0}, {2:pattern});
```

---
#### `trgf_x64`, `target_onfound_64bit`, `sigscantarget_onfound_64bit`

*Creates a SigScanTarget to scan.*  
*The OnFound function gets executed when the target was successfully found.*
```cs
new SigScanTarget({1:0}, {2:pattern})
{ OnFound = (process, scanner, address) => address + 0x4 + process.ReadValue<int>(address) };
```

---
#### `trgf_x86`, `target_onfound_32bit`, `sigscantarget_onfound_32bit`

*Creates a SigScanTarget to scan.*  
*The OnFound function gets executed when the target was successfully found.*
```cs
new SigScanTarget({1:0}, {2:pattern})
{ OnFound = (process, scanner, address) => process.ReadPointer(address) };
```

---
#### `tm`, `timer_model`

*Creates a TimerModel object reflecting the current timer's state.*  
*Often used for unusual workarounds.*
```cs
vars.{1:TimerModel} = new TimerModel { CurrentState = timer };
```



---
## Threading
#### `cts`, `cancel_source`

*A CancellationTokenSource to be used when working with Threads or Tasks.*
```cs
vars.{1:CancelSource} = new CancellationTokenSource();
```

---
#### `thread`

*Creates and starts a Thread that runs independently of the LiveSplit process, not blocking the UI thread.*
```cs
// WARNING: if an error is thrown in the thread, LiveSplit crashes!
// Threads which have an indeterminate runtime require a way to be canceled,
// otherwise the thread will continue running indefinitely.
// This could be the case when using infinite loops or looking for specific objects.
// Call Cancel() on a CancellationTokenSource when exit{} or shutdown{} run and use
// CancellationTokenSource.Token.IsCancellationRequested to exit a loop.

vars.{1:IsCompleted} = false;
new Thread(() =>
{
	// Thread.Sleep(1000);

	{0}

	vars.{1} = true;
}).Start();
```

---
#### `thcs`, `thread_cts`

*Creates and starts a Thread that runs independently of the LiveSplit process, not blocking the UI thread.*  
*The CancellationToken should be used to exit all logic in the thread.*
```cs
// WARNING: if an error is thrown in the thread, LiveSplit crashes!
// Threads which have an indeterminate runtime require a way to be canceled,
// otherwise the thread will continue running indefinitely.
// This could be the case when using infinite loops or looking for specific objects.
// Call Cancel() on the CancellationTokenSource when exit{} or shutdown{} run and use
// token.IsCancellationRequested to exit a loop.

vars.{1:IsCompleted} = false;
vars.{2:CancelSource} = new CancellationTokenSource();
new Thread(() =>
{
	// Thread.Sleep(1000);
	// var token = vars.{2}.Token;

	{0}

	vars.{1} = true;
}).Start();
```

---
#### `task`

*Starts a task that runs asynchronously to the LiveSplit process, not blocking the UI thread.*
```cs
// WARNING: if an error is thrown in the task, LiveSplit crashes!
// Tasks which have an indeterminate runtime require a way to be canceled,
// otherwise the task will continue running indefinitely.
// This could be the case when using infinite loops or looking for specific objects.
// Call Cancel() on a CancellationTokenSource when exit{} or shutdown{} run and pass
// the CancellationTokenSource.Token object to the Task.

vars.{1:IsCompleted} = false;
System.Threading.Tasks.Task.Run(async () =>
{
	// await System.Threading.Tasks.Task.Delay(1000);

	{0}

	vars.{1} = true;
}).Start();
```

---
#### `tcs`, `task_cts`

*Creates a Task that runs asynchronously to the LiveSplit process, not blocking the UI thread.*  
*The Task takes a CancellationToken, stopping the task when it is canceled.*
```cs
// WARNING: if an error is thrown in the task, LiveSplit crashes!
// Tasks which have an indeterminate runtime require a way to be canceled,
// otherwise the task will continue running indefinitely.
// This could be the case when using infinite loops or looking for specific objects.
// Call Cancel() on the CancellationTokenSource when exit{} or shutdown{} run and pass
// the CancellationTokenSource.Token object to the Task.

vars.{1:IsCompleted} = false;
vars.{2:CancelSource} = new CancellationTokenSource();
System.Threading.Tasks.Task.Run((Action)(async () =>
{
	// await System.Threading.Tasks.Task.Delay(1000);
	// var token = vars.{2}.Token;

	{0}

	vars.{1} = true;
}), vars.{2}.Token);
```