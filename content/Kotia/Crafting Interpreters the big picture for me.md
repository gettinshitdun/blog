We will implement the language two times, one in java then the other one in C
For "jlox" we are meant to understand the fundamental concepts of the compiling stages and the compiler itself hence we will write the most understandable code and not think about optimizations all that will be handled in "clox"

Two concepts we encounter
1. Self hosting -> the compiler and the source file are both being written in the same language, using gcc (written in cpp) to compile cpp
2. BootStrapping -> to enablea language to self host itself we need to write another compiler for out language and use that to compile our first version then we can use the compiled version of our own compiler in the future to self host and the original compiler can be discarded

