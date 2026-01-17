		I have always been fascinated by programming languages, and this has increased since the last year cause i have been following this book [[www.craftinginterpreters.com]] This book has been the source of all my compiler knowledge with practical, if you haven't checked it out, it's one of the best sources for creating your own language. \
I have been working on a language with the help of the aforementioned book and now i would give my honest try to give some of the knowledge i gained back. 


TODO : How I got into compilers & programming languages myself

First we will try to understand a very high level architecture which all languages go through to become a working usable programming language
Every language is different but the overall architecture for their compilers/ interpreters are the same
My goal is not to bore you guys with the textbook of compiler design but rather we will try to explore different languages codebase to see how they handle this compiler design concept
I think we could say the low level system design for the language's compiler
What languages we will explore - currently i have C/C++ , Java, Python in mind we will start with C++ and then move with others
Nevertheless back to our topic what's the high level architecture for each compiler.

But before that 
## Do we know what a compiler really is?
Essentially a compiler is just a program which will convert the text into instructions that our computer could understand. We all know this by now computer only know 0's and 1's so from their perspective whatever we write is just gibberish.
So A compiler could also be called as a program which converts the text to another program So this is how i would visualize a compiler.
It's a program which would convert a source language into a target language.
So basically a drawing for the black box

Each compiler has a front end and the backend
We will start with the front end of the compiler
Now the first thing in the front end is what we call is the Lexer, 
A lexer is used to convert the text into tokens, why do we have to do this, well the classic point is that text could be of different varying sizes, if it was a number its just "1", but what about an string ""Hello, World!"" or just a variable name or a keyword we don't know we can't just check everytime that
if f&o&r is written then it's a keyword or a very big identifier name to check this
This is handled by our first pipeline journey Called the scanner
The scanner work is just to convert all the text it's inputted into a stream of tokens, so a stream of tokens could also be said as an array of tokens
Why tokens just to create a common ground for the next pipeline to work well now the next pipeline won't have to care about what is written it works on something else, to convert text into tokens is the work of the Lexer.
So how is this written in C++ let's explore this together
Remember we are not caring about why a programming language is made in this way but rather how is it's compiler (in this case g++) handles the programming language
So rather than exploring C++ language we will rather explore it's compiler ie g++
So let's come back to our original question 

### How does G++ handle lexing
Let's now take a look at it's code 

### **GOAL -> Check How G++ which is a part of GNU Compiler Collection handles the frontend of the C/C++ language**

Frontend means till GIMPLE we will first handle the lexer of the g++/GCC

### **SUB GOAL -> How does lexing works in g++/GCC**

For this to work we would need to first step though the gcc code itself, this is not available to the user as we download just the binary of gcc when we download it using linux getutils...

#### GOAL -> Create a debugable gcc program which when run though our code we would be able to debug through gcc or something else

https://gcc.gnu.org/onlinedocs/gcc/Debugging-Options.html
https://gcc.gnu.org/wiki/DebuggingGCC
https://www.youtube.com/watch?v=yEySjvC4lSI
Tried to run this command but build failed,
Installed prerequisites

use install_prerequisites executable and then run again the command
OK, so finally we were able to install gcc with our gcc named gcc-kotia, so let's see how we did it
1. Download the gcc source using git clone 
2. We now need to checkout the source code with the tag we want, i wanted latest release ie 15, so I think i did git checkout release/gcc-15 or something like that and then fetch it, we need to look this up
3. create a objdir outside of the source dir
4. switch to objdir and now configure our gcc to do that from the objdir run the configure command with the following tags
5. ``` sh
   ./../gcc/configure \      
		--disable-analyzer \
        --disable-bootstrap \
        --disable-cet \
        --disable-default-pie \
        --disable-default-ssp \
        --disable-fixincludes \
        --disable-gcov \
        --disable-libada \
        --disable-libgomp \
        --disable-libitm \
        --disable-libquadmath \
        --disable-libsanitizer \
        --disable-libssp \
        --disable-libstdcxx-pch \
        --disable-libvtv \
        --disable-lto \
        --disable-multilib \
        --disable-nls \
        --disable-objc-gc \
        --disable-systemtap \
        --disable-werror \
        --enable-languages=c,c++ \
        --without-libatomic \
        --without-libbacktrace \
        --without-isl \
        --without-zstd \
        --with-system-zlib \
        STAGE1_C{,XX}FLAGS="-O0 -g" \
		--prefix=$HOME/Development/G++/gcc-build/v0/ \
		--program-suffix=-kotia

   ```
6. These flags we got from the link https://gcc.gnu.org/wiki/DebuggingGCC these flags to specifically reduce time and debug our program, all of the flags can't be explained as i don't know them either but 2 i added was --prefix= this shows the path to which gcc would be instaled, --program-suffix would add this suffix to the program when required
7. with this in our objdir we would have three files, config, config.status, Makefile
8. run make install and it would be built in the specified directory

## Time to try this program to see how it works 
For this i think we would need a book now on how gcc internals work and which file to go through
Like always a step closed
So run gdb in tui while running our own program with the arguments, i was doing it wrong then i realised we cant use our gcc to compile and then use gdb on the outpu file but rather we want to gdb onto this compilation process

``` sh

gdb --tui --args ./../v0/bin/gcc-kotia hello.c -o hello-kotia

```


https://www.cse.iitb.ac.in/grc/gcc-workshop-13/index.php?page=slides
https://sensperiodit.wordpress.com/wp-content/uploads/2011/04/hagen-the-definitive-guide-to-gcc-2e-apress-2006.pdf
https://en.wikibooks.org/wiki/GNU_C_Compiler_Internals/GNU_C_Compiler_Architecture

Tried to use the rr debugger a kind of a time travel debugger but didn't work, still struggling to find the right file but a bit of progress this file i found shows the control flow 
https://www.cse.iitb.ac.in/~uday/courses/cs715-09/gcc-code-view.pdf

Current control flow which we understood
main -> d.main->do_spec_on_infiles (this function probably calls cc1) ->
From then i am unable to understand what gets called
do_spec, do_spec_2, do_spec_1

driver::main -> 3 child processes executes
Q How did we reached toplev main of cc1 through this
as soon as we used follow-fork-mode child we got into the toplev::main function of cc1, now I will give time to recognize how did this happen which function actually calls this function
so this is what i understood so far, gcc is the driver file which then calls upon the respective binary which handles the front end as well as the backend and then it gets handed back to gcc driver program to run the linker
for the generated compiler is cc1, so we now want to focus on cc1, we could focus on the code but currently I do not understand that maybe gdb is having a bit of problems to go through the code for cc1, So for now we will divide our time into two parts
1. How does gcc call cc1
2. How does cc1 handles the parsing 

https://www.cse.iitb.ac.in/grc/slides/cgotut-gcc/topic4-module-bindings.pdf

https://www.cse.iitb.ac.in/grc/index.php?page=docs
https://www.cse.iitb.ac.in/grc/slides/

driver::main -> 3 child processes executes
	do_spec_on_infiles -> 2
		do_spec -> 2
			do_spec_2 -> 1
			execute -> 1
	maybe_run_linker -> 1

![[do_spec.png]]

What are tracepoints in gdb
Wasn't able to understand much still has to understand both the aspects mentioned above

Still don't know will this help me in understanding how is the function executing, don't know but this will take some time cause a lot of stepping has to be done

```sh
set print elements 0
```

![[cc1.png]]

Good progress today
We got the whole method from which cc1 is being called, it was exactly as the ppt said but it was nice to see it 
driver::main ->  do_spec_on_infiles -> do_spec -> do_spec_2 -> do_spec_1* <-> handle_braces* ---> execute -> pex_run -> pex_run_in_environment

After this we got stuck on when the child process is forked it immediately exitsas being done, gdb does not get enough time to get into and give execution to it, it just immediately runs, hence when we got into child program we need to start it again hence it forgets all it's flags therefore gets stuck in stdin mode we recognized this with the help of gemini & the hint for linux/read.c file which i thought that when i press enter it would we written in stdin but no actually when we press ctrl+D then it gives the stdin / program that we have reached the EOF (end of file)
so using this method it worked but not the way we wanted it to be i.e. from gcc

So now we needed to figure out to debug the child process i.e. cc1

Luckily gemini helped again it showed on how to put breakpoints in function and i followwed what i learnt previously that write the filename:function to put a breakpoint in a different program
```sh
set follow-fork-mode child
break toplev::main
```
 now this triggerred gdb to say that
 
 "no source file named /home/kotia/Development/G++/gcc/gcc/main.cc:"
 Make breakpoint pending on future shared libraries y or n
 
 I thought that it would breakpoint whenver a library loads, but no i think what it does is whenever the corresponding library loads for that file it would analyze the code and put breakpoint in that featured place, so good thing, of my god i love it
 We would now explore how did cc1 parse and i belive it would be underwhelming, but let's see
 Also realized how good of that slides are, will follow them more
 www.airs.com/dnovillo/200711-GCC-Internals/

### **GOAL -> See how does cc1 lexes the source code**
lexes -> create a string into token

toplev:main->do_compile->compile_file
step langhooks.parse_file()
c-opts.cc : c_common_parse_file 
	c-parser.cc : c_parse_file
		c_parser_translation_unit (the_parser);

c-parser.cc :  c_lex_one_token
c-lex.cc : c_lex_with_flags
c-lex.cc : get_token
macro.cc : cpp_get_token_with_location
macro.cc : cpp_get_token_1
lex.cc : _cpp_lex_token
lex.cc : _cpp_lex_direct

c = 32 (\space)-> skip_whitespace
c = 95 (_)->  lex_identifier

try reverse debugging in gdb
https://gcc.gnu.org/onlinedocs/cppinternals/
explore time travel debuggers
Explore undo, rr, gbd reverse debugging, Mon, Wed, Fri
Parallely explore the lexing through plain gdb Tue, Thurs, Sat

ran 
gcc -E hello.c 
this returns the preprocessor output and this is quite large at the end our code is getting parsed, this honestly is a lot, i think we would need time travel debugger
Also i believe we have reached the point we were looking for i.e. the file for the lexing of c code
i.e. c-famile/c-lex.cc => need to confirm if this is the only file for this






cpython -> https://devguide.python.org/internals/c
