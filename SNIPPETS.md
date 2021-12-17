# Snippets

The following are all code snippets currently supported by this extension.  
These are merely examples. The variable types and names can be modified by the user upon use of a snippet.

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
#### `csett`, `create_setting`

*Creates an entry to be added to the Auto Splitter's settings.*
```cs
settings.Add("{1:id}", {2:true}, "{3:description}");
```

---
#### `csettp`, `create_setting_parent`

*Creates an entry to be added to the Auto Splitter's settings.*
```cs
settings.Add("{1:id}", {2:true}, "{3:description}", "{4:parent}");
```

---
#### `tt`, `tooltip`

*Creates a tooltip to assign to a setting.*
```cs
settings.SetToolTip("{1:id}", "{2:tooltip}");
```



## OnTimerEvent
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



## Script Blocks
#### `init`

*This action runs when the game has been found according to at least one of the state descriptors.*
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
#### `updc`, `update_check`

*This action runs first on every iteration.*  
*Explicitly returning false disables all other actions.*  
*Check whether a Task or Thread is still running; if so, don't let any logic continue.*
```cs
update
{
	// Use this check when making use of multi-threading or tasks.
	// The script will not run until the vars is set to true.
	if (!vars.{1:IsCompleted}) return false;

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
#### `exitc`, `exit_cancel`

*This action is run when the currently attached process exits.*  
*A specified CancellationTokenSource object can be canceled.*
```cs
exit
{
	// Use this when making use of multi-threading or tasks.
	// The thread or task should stop running when the process exits.
	vars.{1:CancelSource}.Cancel();{0}
}
```

---
#### `sd`, `shutdown`

*This action is run when the script is entirely disconnected from LiveSplit.*  
*Examples include when the Auto Splitter is disabled, LiveSplit exits, the script path is changed, or the script is reloaded*
```cs
shutdown
{
	{0}
}
```

---
#### `sdc`, `shutdown_cancel`

*This action is run when the script is entirely disconnected from LiveSplit.*  
*Examples include when the Auto Splitter is disabled, LiveSplit exits, the script path is changed, or the script is reloaded*  
*A specified CancellationTokenSource object can be canceled.*
```cs
shutdown
{
	// Use this when making use of multi-threading or tasks.
	// The thread or task should stop running when the process exits.
	vars.{1:CancelSource}.Cancel();{0}
}
```



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
game.ReadPointer<{1:int}>({2:address})
```

---
#### `rstr`, `readstring`

*Reads a string at a specified address.*
```cs
game.ReadString({1:address}, {2:length})
```



## General Objects
#### `dp`, `deeppointer`

*Creates a DeepPointer object, often used when working with MemoryWatchers or more sophisticated memory-management.*
```cs
new DeepPointer({0:pointerpath});
```

---
#### `mw`, `memwatcher`, `memorywatcher`

*Creates a MemoryWatcher object, often used when building pointers manually.*  
*A MemoryWatcher's first parameter is either an IntPtr or a DeepPointer object.*
```cs
new MemoryWatcher<{1:int}>({0:pointer});
```

---
#### `mwn`, `memwatcher_named`, `memorywatcher_named`

*Creates a MemoryWatcher object, often used when building pointers manually.*  
*A MemoryWatcher's first parameter is either an IntPtr or a DeepPointer object.*  
*Useful with MemoryWatcherLists.*
```cs
new MemoryWatcher<{1:int}>({0:pointer}) { Name = "{2:name}" };
```

---
#### `sw`, `stringwatcher`

*Creates a StringWatcher object, often used when building pointers manually.*  
*A StringWatcher's first parameter is either an IntPtr or a DeepPointer object.*
```cs
new StringWatcher({0:pointer}, {1:length});
```

---
#### `swn`, `stringwatcher_named`

*Creates a StringWatcher object, often used when building pointers manually.*  
*A StringWatcher's first parameter is either an IntPtr or a DeepPointer object.*  
*Useful with MemoryWatcherLists.*
```cs
new StringWatcher({0:pointer}, {1:length}) { Name = "{2:name}" };
```

---
#### `mwl`, `memwatcherlist`, `memorywatcherlist`

*Creates a list of MemoryWatchers or StringWatchers.*  
*Use MemoryWatcherList[name] to access a watcher by its name.*
```cs
// To access a watcher, each one must be given a name.
vars.{1:Watchers} = new MemoryWatcherList
{
	{0}
};
```

---
#### `tm`, `timermodel`

*Creates a TimerModel object reflecting the current timer's state.*  
*Often used for unusual workarounds.*
```cs
vars.{1:TimerModel} = new TimerModel { CurrentState = timer };
```



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
// Threads which have an indeterminate runtime require a way to be canceled,
// otherwise the thread will continue running indefinitely.
// This could be the case when using infinite loops or looking for specific objects.
// Call Cancel() on a CancellationTokenSource when exit{} or shutdown{} run and use
// CancellationTokenSource.Token.IsCancellationRequested to exit a loop.

vars.{1:IsCompleted} = false;
vars.{2:Thread} = new Thread(() =>
{
	// Thread.Sleep(1000);

	{0}

	vars.{1} = true;
});

vars.{2}.Start();
```

---
#### `cth`, `cthread`

*Creates and starts a Thread that runs independently of the LiveSplit process, not blocking the UI thread.*  
*The CancellationToken should be used to exit all logic in the thread.*
```cs
// Threads which have an indeterminate runtime require a way to be canceled,
// otherwise the thread will continue running indefinitely.
// This could be the case when using infinite loops or looking for specific objects.
// Call Cancel() on the CancellationTokenSource when exit{} or shutdown{} run and use
// CancellationTokenSource.Token.IsCancellationRequested to exit a loop.

vars.{1:IsCompleted} = false;
vars.{2:CancelSource} = new CancellationTokenSource();
vars.{3:Thread} = new Thread(() =>
{
	// Thread.Sleep(1000);
	// var token = vars.${2}.Token;

	{0}

	vars.{2} = true;
});

vars.{3}.Start();
```

---
#### `task`

*Starts a task that runs asynchronously to the LiveSplit process, not blocking the UI thread.*
```cs
// Tasks which have an indeterminate runtime require a way to be canceled,
// otherwise the task will continue running until LiveSplit is closed.
// This could be the case when using infinite loops or looking for specific objects.
// Call Cancel() on a CancellationTokenSource when exit{} or shutdown{} run and pass
// the CancellationTokenSource.Token object to the Task.

vars.{1:IsCompleted} = false;
new System.Threading.Tasks.Task(async () =>
{
	// await System.Threading.Tasks.Task.Delay(1000);

	{0}

	vars.{1} = true;
}).Start();
```

---
#### `tcs`, `task_cts`

*A Task that runs asynchronously to the LiveSplit process, not blocking the UI thread.*  
*The Task takes a CancellationToken, stopping the task when it is canceled.*
```cs
// Tasks which have an indeterminate runtime require a way to be canceled,
// otherwise the task will continue running until LiveSplit is closed.
// This could be the case when using infinite loops or looking for specific objects.
// Call Cancel() on the CancellationTokenSource when exit{} or shutdown{} run and pass
// the CancellationTokenSource.Token object to the Task.

vars.{1:IsCompleted} = false;
vars.{2:CancelSource} = new CancellationTokenSource();
System.Threading.Tasks.Task.Run(async () =>
{
	// await System.Threading.Tasks.Task.Delay(1000);

	{0}

	vars.{1} = true;
}, vars.{2}.Token);
```