## Let's start
The we are head first diving into the topic, i would like to digress into the start to tell you on how did i got motivated to learn about compiler design or mainly how are programming languages are made. It all came down to a widely popular language "rust". One day i was just wondering on how does languages get compiled to machine code and all and one question hit me.
gcc is used to compile C programs, g++ for C++ programs, how about this popular language rust
I am not able to find the exact google page which i saw the answer from so here's the google AI summary

![rustc](rustc.png)


So I got interested that the compiler rustc is written in which programming language, turns out rustc is written in rust and this fact blew my mind, thinking how's that possible, then i researched a bit more now another question arose In which programming language GCC written in i thought as it's used to compile C programs it's probably written in Assembly but boy was i wrong, it's actually written in C++ and now my brain is fucked how is all this possible?

If GCC is written in C++ or even C to compile C programs isn't it bizarre, Just think about it isn't this disturbing to wrap your brain around the fact that if C is used to compile C then how was it compiled when the language was in it's initial phases cause at that time you do not have the fully built language yet, so a classic chicken and egg problem Using C to compile C is just something that I can't digest and that's where the answer comes in
Two new words came into the picture

Self-Hosting -> Ultimately we want that our language's compiler to compile its own code, why such that it's not dependent on any other language
Bootstrapping -> This is the first process to self-host a language
