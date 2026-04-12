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


the c-lexer
```c
/* Lex a token into pfile->cur_token, which is also incremented, to

get diagnostics pointing to the correct location.

  

Does not handle issues such as token lookahead, multiple-include

optimization, directives, skipping etc. This function is only

suitable for use by _cpp_lex_token, and in special cases like

lex_expansion_token which doesn't care for any of these issues.

  

When meeting a newline, returns CPP_EOF if parsing a directive,

otherwise returns to the start of the token buffer if permissible.

Returns the location of the lexed token. */

cpp_token *

_cpp_lex_direct (cpp_reader *pfile)

{

cppchar_t c = 0;

cpp_buffer *buffer;

const unsigned char *comment_start;

bool fallthrough_comment = false;

cpp_token *result = pfile->cur_token++;

  

fresh_line:

result->flags = 0;

buffer = pfile->buffer;

if (buffer->need_line)

{

if (pfile->state.in_deferred_pragma)

{

/* This can happen in cases like:

#define loop(x) whatever

#pragma omp loop

where when trying to expand loop we need to peek

next token after loop, but aren't still in_deferred_pragma

mode but are in in_directive mode, so buffer->need_line

is set, a CPP_EOF is peeked. */

result->type = CPP_PRAGMA_EOL;

pfile->state.in_deferred_pragma = false;

if (!pfile->state.pragma_allow_expansion)

pfile->state.prevent_expansion--;

result->src_loc = pfile->line_table->highest_line;

return result;

}

if (!_cpp_get_fresh_line (pfile))

{

result->type = CPP_EOF;

/* Not a real EOF in a directive or arg parsing -- we refuse

to advance to the next file now, and will once we're out

of those modes. */

if (!pfile->state.in_directive && !pfile->state.parsing_args)

{

/* Tell the compiler the line number of the EOF token. */

result->src_loc = pfile->line_table->highest_line;

result->flags = BOL;

/* Now pop the buffer that _cpp_get_fresh_line did not. */

_cpp_pop_buffer (pfile);

}

else if (c == 0)

result->src_loc = pfile->line_table->highest_line;

return result;

}

if (buffer != pfile->buffer)

fallthrough_comment = false;

if (!pfile->keep_tokens)

{

pfile->cur_run = &pfile->base_run;

result = pfile->base_run.base;

pfile->cur_token = result + 1;

}

result->flags = BOL;

if (pfile->state.parsing_args == 2)

result->flags |= PREV_WHITE;

}

buffer = pfile->buffer;

update_tokens_line:

result->src_loc = pfile->line_table->highest_line;

  

skipped_white:

if (buffer->cur >= buffer->notes[buffer->cur_note].pos

&& !pfile->overlaid_buffer)

{

_cpp_process_line_notes (pfile, false);

result->src_loc = pfile->line_table->highest_line;

}

c = *buffer->cur++;

  

if (pfile->forced_token_location)

result->src_loc = pfile->forced_token_location;

else

result->src_loc = linemap_position_for_column (pfile->line_table,

CPP_BUF_COLUMN (buffer, buffer->cur));

  

switch (c)

{

case ' ': case '\t': case '\f': case '\v': case '\0':

result->flags |= PREV_WHITE;

skip_whitespace (pfile, c);

goto skipped_white;

  

case '\n':

/* Increment the line, unless this is the last line ... */

if (buffer->cur < buffer->rlimit

/* ... or this is a #include, (where _cpp_stack_file needs to

unwind by one line) ... */

|| (pfile->state.in_directive > 1

/* ... except traditional-cpp increments this elsewhere. */

&& !CPP_OPTION (pfile, traditional)))

CPP_INCREMENT_LINE (pfile, 0);

buffer->need_line = true;

if (pfile->state.in_deferred_pragma)

{

/* Produce the PRAGMA_EOL on this line. File reading

ensures there is always a \n at end of the buffer, thus

in a deferred pragma we always see CPP_PRAGMA_EOL before

any CPP_EOF. */

result->type = CPP_PRAGMA_EOL;

result->flags &= ~PREV_WHITE;

pfile->state.in_deferred_pragma = false;

if (!pfile->state.pragma_allow_expansion)

pfile->state.prevent_expansion--;

return result;

}

goto fresh_line;

  

case '0': case '1': case '2': case '3': case '4':

case '5': case '6': case '7': case '8': case '9':

{

struct normalize_state nst = INITIAL_NORMALIZE_STATE;

result->type = CPP_NUMBER;

lex_number (pfile, &result->val.str, &nst);

warn_about_normalization (pfile, result, &nst, false);

break;

}

  

case 'L':

case 'u':

case 'U':

case 'R':

/* 'L', 'u', 'U', 'u8' or 'R' may introduce wide characters,

wide strings or raw strings. */

if (c == 'L' || CPP_OPTION (pfile, rliterals)

|| (c != 'R' && CPP_OPTION (pfile, uliterals)))

{

if ((*buffer->cur == '\'' && c != 'R')

|| *buffer->cur == '"'

|| (*buffer->cur == 'R'

&& c != 'R'

&& buffer->cur[1] == '"'

&& CPP_OPTION (pfile, rliterals))

|| (*buffer->cur == '8'

&& c == 'u'

&& ((buffer->cur[1] == '"' || (buffer->cur[1] == '\''

&& CPP_OPTION (pfile, utf8_char_literals)))

|| (buffer->cur[1] == 'R' && buffer->cur[2] == '"'

&& CPP_OPTION (pfile, rliterals)))))

{

lex_string (pfile, result, buffer->cur - 1);

break;

}

}

/* Fall through. */

  

case '_':

case 'a': case 'b': case 'c': case 'd': case 'e': case 'f':

case 'g': case 'h': case 'i': case 'j': case 'k': case 'l':

case 'm': case 'n': case 'o': case 'p': case 'q': case 'r':

case 's': case 't': case 'v': case 'w': case 'x':

case 'y': case 'z':

case 'A': case 'B': case 'C': case 'D': case 'E': case 'F':

case 'G': case 'H': case 'I': case 'J': case 'K':

case 'M': case 'N': case 'O': case 'P': case 'Q':

case 'S': case 'T': case 'V': case 'W': case 'X':

case 'Y': case 'Z':

result->type = CPP_NAME;

{

struct normalize_state nst = INITIAL_NORMALIZE_STATE;

const auto node = lex_identifier (pfile, buffer->cur - 1, false, &nst,

&result->val.node.spelling);

result->val.node.node = node;

identifier_diagnostics_on_lex (pfile, node);

warn_about_normalization (pfile, result, &nst, true);

}

  

/* Convert named operators to their proper types. */

if (result->val.node.node->flags & NODE_OPERATOR)

{

result->flags |= NAMED_OP;

result->type = (enum cpp_ttype) result->val.node.node->directive_index;

}

  

/* Signal FALLTHROUGH comment followed by another token. */

if (fallthrough_comment)

result->flags |= PREV_FALLTHROUGH;

break;

  

case '\'':

case '"':

lex_string (pfile, result, buffer->cur - 1);

break;

  

case '/':

/* A potential block or line comment. */

comment_start = buffer->cur;

c = *buffer->cur;

  

if (c == '*')

{

if (_cpp_skip_block_comment (pfile))

cpp_error (pfile, CPP_DL_ERROR, "unterminated comment");

}

else if (c == '/' && ! CPP_OPTION (pfile, traditional))

{

/* Don't warn for system headers. */

if (_cpp_in_system_header (pfile))

;

/* Warn about comments if pedantically GNUC89, and not

in system headers. */

else if (CPP_OPTION (pfile, lang) == CLK_GNUC89

&& CPP_PEDANTIC (pfile)

&& ! buffer->warned_cplusplus_comments)

{

if (cpp_pedwarning (pfile, CPP_W_PEDANTIC,

"C++ style comments are not allowed "

"in ISO C90"))

cpp_error (pfile, CPP_DL_NOTE,

"(this will be reported only once per input file)");

buffer->warned_cplusplus_comments = 1;

}

/* Or if specifically desired via -Wc90-c99-compat. */

else if (CPP_OPTION (pfile, cpp_warn_c90_c99_compat) > 0

&& ! CPP_OPTION (pfile, cplusplus)

&& ! buffer->warned_cplusplus_comments)

{

if (cpp_error (pfile, CPP_DL_WARNING,

"C++ style comments are incompatible with C90"))

cpp_error (pfile, CPP_DL_NOTE,

"(this will be reported only once per input file)");

buffer->warned_cplusplus_comments = 1;

}

/* In C89/C94, C++ style comments are forbidden. */

else if ((CPP_OPTION (pfile, lang) == CLK_STDC89

|| CPP_OPTION (pfile, lang) == CLK_STDC94))

{

/* But don't be confused about valid code such as

- // immediately followed by *,

- // in a preprocessing directive,

- // in an #if 0 block. */

if (buffer->cur[1] == '*'

|| pfile->state.in_directive

|| pfile->state.skipping)

{

result->type = CPP_DIV;

break;

}

else if (! buffer->warned_cplusplus_comments)

{

if (cpp_error (pfile, CPP_DL_ERROR,

"C++ style comments are not allowed in "

"ISO C90"))

cpp_error (pfile, CPP_DL_NOTE,

"(this will be reported only once per input "

"file)");

buffer->warned_cplusplus_comments = 1;

}

}

if (skip_line_comment (pfile) && CPP_OPTION (pfile, warn_comments))

cpp_warning (pfile, CPP_W_COMMENTS, "multi-line comment");

}

else if (c == '=')

{

buffer->cur++;

result->type = CPP_DIV_EQ;

break;

}

else

{

result->type = CPP_DIV;

break;

}

  

if (fallthrough_comment_p (pfile, comment_start))

fallthrough_comment = true;

  

if (pfile->cb.comment)

{

size_t len = pfile->buffer->cur - comment_start;

pfile->cb.comment (pfile, result->src_loc, comment_start - 1,

len + 1);

}

  

if (!pfile->state.save_comments)

{

result->flags |= PREV_WHITE;

goto update_tokens_line;

}

  

if (fallthrough_comment)

result->flags |= PREV_FALLTHROUGH;

  

/* Save the comment as a token in its own right. */

save_comment (pfile, result, comment_start, c);

break;

  

case '<':

if (pfile->state.angled_headers)

{

lex_string (pfile, result, buffer->cur - 1);

if (result->type != CPP_LESS)

break;

}

  

result->type = CPP_LESS;

if (*buffer->cur == '=')

{

buffer->cur++, result->type = CPP_LESS_EQ;

if (*buffer->cur == '>'

&& CPP_OPTION (pfile, cplusplus)

&& CPP_OPTION (pfile, lang) >= CLK_GNUCXX20)

buffer->cur++, result->type = CPP_SPACESHIP;

}

else if (*buffer->cur == '<')

{

buffer->cur++;

IF_NEXT_IS ('=', CPP_LSHIFT_EQ, CPP_LSHIFT);

}

else if (CPP_OPTION (pfile, digraphs))

{

if (*buffer->cur == ':')

{

/* C++11 [2.5/3 lex.pptoken], "Otherwise, if the next

three characters are <:: and the subsequent character

is neither : nor >, the < is treated as a preprocessor

token by itself". */

if (CPP_OPTION (pfile, cplusplus)

&& CPP_OPTION (pfile, lang) != CLK_CXX98

&& CPP_OPTION (pfile, lang) != CLK_GNUCXX

&& buffer->cur[1] == ':'

&& buffer->cur[2] != ':' && buffer->cur[2] != '>')

break;

  

buffer->cur++;

result->flags |= DIGRAPH;

result->type = CPP_OPEN_SQUARE;

}

else if (*buffer->cur == '%')

{

buffer->cur++;

result->flags |= DIGRAPH;

result->type = CPP_OPEN_BRACE;

}

}

break;

  

case '>':

result->type = CPP_GREATER;

if (*buffer->cur == '=')

buffer->cur++, result->type = CPP_GREATER_EQ;

else if (*buffer->cur == '>')

{

buffer->cur++;

IF_NEXT_IS ('=', CPP_RSHIFT_EQ, CPP_RSHIFT);

}

break;

  

case '%':

result->type = CPP_MOD;

if (*buffer->cur == '=')

buffer->cur++, result->type = CPP_MOD_EQ;

else if (CPP_OPTION (pfile, digraphs))

{

if (*buffer->cur == ':')

{

buffer->cur++;

result->flags |= DIGRAPH;

result->type = CPP_HASH;

if (*buffer->cur == '%' && buffer->cur[1] == ':')

buffer->cur += 2, result->type = CPP_PASTE, result->val.token_no = 0;

}

else if (*buffer->cur == '>')

{

buffer->cur++;

result->flags |= DIGRAPH;

result->type = CPP_CLOSE_BRACE;

}

}

break;

  

case '.':

result->type = CPP_DOT;

if (ISDIGIT (*buffer->cur))

{

struct normalize_state nst = INITIAL_NORMALIZE_STATE;

result->type = CPP_NUMBER;

lex_number (pfile, &result->val.str, &nst);

warn_about_normalization (pfile, result, &nst, false);

}

else if (*buffer->cur == '.' && buffer->cur[1] == '.')

buffer->cur += 2, result->type = CPP_ELLIPSIS;

else if (*buffer->cur == '*' && CPP_OPTION (pfile, cplusplus))

buffer->cur++, result->type = CPP_DOT_STAR;

break;

  

case '+':

result->type = CPP_PLUS;

if (*buffer->cur == '+')

buffer->cur++, result->type = CPP_PLUS_PLUS;

else if (*buffer->cur == '=')

buffer->cur++, result->type = CPP_PLUS_EQ;

break;

  

case '-':

result->type = CPP_MINUS;

if (*buffer->cur == '>')

{

buffer->cur++;

result->type = CPP_DEREF;

if (*buffer->cur == '*' && CPP_OPTION (pfile, cplusplus))

buffer->cur++, result->type = CPP_DEREF_STAR;

}

else if (*buffer->cur == '-')

buffer->cur++, result->type = CPP_MINUS_MINUS;

else if (*buffer->cur == '=')

buffer->cur++, result->type = CPP_MINUS_EQ;

break;

  

case '&':

result->type = CPP_AND;

if (*buffer->cur == '&')

buffer->cur++, result->type = CPP_AND_AND;

else if (*buffer->cur == '=')

buffer->cur++, result->type = CPP_AND_EQ;

break;

  

case '|':

result->type = CPP_OR;

if (*buffer->cur == '|')

buffer->cur++, result->type = CPP_OR_OR;

else if (*buffer->cur == '=')

buffer->cur++, result->type = CPP_OR_EQ;

break;

  

case ':':

result->type = CPP_COLON;

if (*buffer->cur == ':')

{

if (CPP_OPTION (pfile, scope))

buffer->cur++, result->type = CPP_SCOPE;

else

result->flags |= COLON_SCOPE;

}

else if (*buffer->cur == '>' && CPP_OPTION (pfile, digraphs))

{

buffer->cur++;

result->flags |= DIGRAPH;

result->type = CPP_CLOSE_SQUARE;

}

break;

  

case '*': IF_NEXT_IS ('=', CPP_MULT_EQ, CPP_MULT); break;

case '=': IF_NEXT_IS ('=', CPP_EQ_EQ, CPP_EQ); break;

case '!': IF_NEXT_IS ('=', CPP_NOT_EQ, CPP_NOT); break;

case '^': IF_NEXT_IS ('=', CPP_XOR_EQ, CPP_XOR); break;

case '#': IF_NEXT_IS ('#', CPP_PASTE, CPP_HASH); result->val.token_no = 0; break;

  

case '?': result->type = CPP_QUERY; break;

case '~': result->type = CPP_COMPL; break;

case ',': result->type = CPP_COMMA; break;

case '(': result->type = CPP_OPEN_PAREN; break;

case ')': result->type = CPP_CLOSE_PAREN; break;

case '[': result->type = CPP_OPEN_SQUARE; break;

case ']': result->type = CPP_CLOSE_SQUARE; break;

case '{': result->type = CPP_OPEN_BRACE; break;

case '}': result->type = CPP_CLOSE_BRACE; break;

case ';': result->type = CPP_SEMICOLON; break;

  

/* @ is a punctuator in Objective-C. */

case '@': result->type = CPP_ATSIGN; break;

  

default:

{

const uchar *base = --buffer->cur;

static int no_warn_cnt;

  

/* Check for an extended identifier ($ or UCN or UTF-8). */

struct normalize_state nst = INITIAL_NORMALIZE_STATE;

if (forms_identifier_p (pfile, true, &nst))

{

result->type = CPP_NAME;

const auto node = lex_identifier (pfile, base, true, &nst,

&result->val.node.spelling);

result->val.node.node = node;

identifier_diagnostics_on_lex (pfile, node);

warn_about_normalization (pfile, result, &nst, true);

break;

}

  

/* Otherwise this will form a CPP_OTHER token. Parse valid UTF-8 as a

single token. */

buffer->cur++;

if (c >= utf8_signifier)

{

const uchar *pstr = base;

cppchar_t s;

if (_cpp_valid_utf8 (pfile, &pstr, buffer->rlimit, 0, NULL, &s))

{

if (s > UCS_LIMIT && CPP_OPTION (pfile, cpp_warn_invalid_utf8))

{

buffer->cur = base;

_cpp_warn_invalid_utf8 (pfile);

}

buffer->cur = pstr;

}

else if (CPP_OPTION (pfile, cpp_warn_invalid_utf8))

{

buffer->cur = base;

const uchar *end = _cpp_warn_invalid_utf8 (pfile);

buffer->cur = base + 1;

no_warn_cnt = end - buffer->cur;

}

}

else if (c >= utf8_continuation

&& CPP_OPTION (pfile, cpp_warn_invalid_utf8))

{

if (no_warn_cnt)

--no_warn_cnt;

else

{

buffer->cur = base;

_cpp_warn_invalid_utf8 (pfile);

buffer->cur = base + 1;

}

}

create_literal (pfile, result, base, buffer->cur - base, CPP_OTHER);

break;

}

  

}

  

/* Potentially convert the location of the token to a range. */

if (result->src_loc >= RESERVED_LOCATION_COUNT

&& result->type != CPP_EOF)

{

/* Ensure that any line notes are processed, so that we have the

correct physical line/column for the end-point of the token even

when a logical line is split via one or more backslashes. */

if (buffer->cur >= buffer->notes[buffer->cur_note].pos

&& !pfile->overlaid_buffer)

_cpp_process_line_notes (pfile, false);

  

source_range tok_range;

tok_range.m_start = result->src_loc;

tok_range.m_finish

= linemap_position_for_column (pfile->line_table,

CPP_BUF_COLUMN (buffer, buffer->cur));

  

result->src_loc

= pfile->line_table->get_or_create_combined_loc (result->src_loc,

tok_range, nullptr, 0);

}

  

return result;

}
```
Now that lexing is done how does this lexer converts it into a tree


cpython -> https://devguide.python.org/internals/c
