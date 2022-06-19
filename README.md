# js-hard-parts-v2
Notes from the FEM JS - The hard parts v2 course.

Functions and Callbacks
- There is only one thread, which means only one thread of execution runs at a time. Either the global execution or the local function execution. 
- The callstack stores the function that is running currently
- At the bottom of the call stack is the global(), which starts running once the function finishes and is removed. 
- A function finishes as soon as it sees the return keyword. 
- Every context has a thread of execution, functions, global context.  
- A new function means a new execution context
