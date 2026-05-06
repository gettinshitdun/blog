We write code to tell these complex machines that we call computers to do what we want it to do.

## What is code
code is English gibberish which when written in a particular way actually makes sense to this computer to precisely know what is to be done.

## Why is it important to write it
Code like i said earlier tells computers to do stuff, now the computer is highly complex machines capable to do a lot of hard task but needs direction and that's where code comes in.
So code enables us to use this computers for our use case

# What's a computer
Nice question we have been talking about this machine a lot which we call the computer, well a computer is a machine which is made to solve large problems and is very versatile, originally it started with solving a complex mathematical problems but as time passes and the technology itself evolved its uses also became more and more versatile more people realised that the hardware that the computer has a lot of capabilities hence to adjust to this versatile nature we essentially need a way to talk with hardware.
Now the issue is that this hardware only understands two states, two values on or off, so if we want to interact with them we need to be on the same level as it hence this is where binary comes in, so we now need a way to talk in binary 0's and 1's. We need to make instructions in binary.

## Why binary why not a range of values
Actually the answer is simplicity, as i introduce more values in the system the hardware needs to be adjusted not only that but also hardware now needs to add complex circuits to add that and then comes the bane of it all Noise. If currently noise is introduced in the circuits there is not big of a problem as it will take a lot of noise to actually disrupt an off value to an on value or vice-versa for example in a range of 0-5V i consider 0V as binary 0 and 5V as binary 1 and even if noise comes in it would at max make 0V as 3.3V still in that case we know there are only two valid inputs so we would round it down to 0V.
But if i introduce another value in between them so now the range would be 0V, 2.5V, 5V, now there would be an issue to recognize this noise as this would induce variability. Hence we keep it simple 0 and 1

## Why can't humans write in binary then
Well actually humans used to, but turns out it's extremely hard, also different architecture may have different implementations so for one architecture it may be "0011" but for other it may be "1010" So it's really really hard for humans to remember all this. And even within the same architecture in different versions this could change so the architecture developers themselves introduced an abstraction language, we call it assembly.
An assembly language actually solves a lot of issues that a programmer had now it's somewhat english, here's an example for bubbe sort in assembly language for the intel 8086 architecture

src:  https://github.com/AllAlgorithms/assembly/blob/master/algorithms/sorting/BubbleSort.asm

``` asm
iclude'emu8086.inc'

org 100h 
.data

array  db 9,6,5,4,3,2,1
count  dw 7

.code

    mov cx,count      
    dec cx               ; outer loop iteration count

nextscan:                ; do {    // outer loop
    mov bx,cx
    mov si,0 

nextcomp:

    mov al,array[si]
    mov dl,array[si+1]
    cmp al,dl

    jnc noswap 

    mov array[si],dl
    mov array[si+1],al

noswap: 
    inc si
    dec bx
    jnz nextcomp

    loop nextscan       ; } while(--cx);



;;; this  loop to display  elements on the screen

    mov cx,7
    mov si,0

print:

    Mov al,array[si]  
    Add al,30h
    Mov ah,0eh
    Int  10h 
    MOV AH,2
    Mov DL , ' '
    INT 21H
    inc si
    Loop print

    ret 
```

even if you don't know assembly at all but this is way better than 0's and 1's right.
With this humans were capable of writing a lot of code, now the next issue comes in. every architecture has a different assembly language, so even if the algorithm or the work that needs to be done on the high level is same but the actual code is different, so a newer layer of abstraction was introduced and this is what we now call a code
With the help of various tools we managed to create some altered programming language that could actually handle the architecture assembly all on it own and the user just has to write this language and this was a great breakthrough
Now the same code could be written in the language C (one of the first languages created)

src: https://www.programiz.com/dsa/bubble-sort

``` c
// Bubble sort in C

#include <stdio.h>

// perform the bubble sort
void bubbleSort(int array[], int size) {

  // loop to access each array element
  for (int step = 0; step < size - 1; ++step) {
      
    // loop to compare array elements
    for (int i = 0; i < size - step - 1; ++i) {
      
      // compare two adjacent elements
      // change > to < to sort in descending order
      if (array[i] > array[i + 1]) {
        
        // swapping occurs if elements
        // are not in the intended order
        int temp = array[i];
        array[i] = array[i + 1];
        array[i + 1] = temp;
      }
    }
  }
}

// print array
void printArray(int array[], int size) {
  for (int i = 0; i < size; ++i) {
    printf("%d  ", array[i]);
  }
  printf("\n");
}

int main() {
  int data[] = {-2, 45, 0, 11, -9};
  
  // find the array's length
  int size = sizeof(data) / sizeof(data[0]);

  bubbleSort(data, size);
  
  printf("Sorted Array in Ascending Order:\n");
  printArray(data, size);
}
```

So with this abstraction a natural question should arise, how actually this occurred what made us take this leap.
So this thing which now converts the programming language to it's binary is done by a program called the "compiler".
this time we will consider this program as a black box which has the power to convert the language into it's binary. But if you are actually interested currently my part time hobby is to work on a compiler myself so i could help you to understand it, but that's a topic for another day.

## Why english cannot be our code?

This is another great question, with this much advancements that we have why can't we still make another abstraction which allows us to write english.
Well the answer is english is just very complex, out computer as much behemoth it is it's still a complex highly logical machine if we do not tell it exactly what to do and how to do it would fail miserably over the years we have reached good abstraction but not would never be able to reach to our regular language, why?
Because english is non-sensical from a logic perspective how will computer know about "this", "that", "these", "where", "here" keywords until it knows the context. yes we could say that AI is bridging the gap but still code is still written in that highly methodical language which has very strict instructions

## How is code affecting the state of the computer
Like i said earlier code when converts to 0's and 1's the computer's hardware is built this way that it could interpret those 0's and 1's and do the works or what we say in computer language perform the computer instruction what it's meant to do, Now if your question is how does a computer do that, then we would have to dive deeper into an interesting topic called Computer architecture which would then explain how the cpu would interpret 0's and 1's and what would happen if we get this instruction from a hardware perspective.
Work in progress
## If that's all what code do, then why do we need so many of these programming languages?
Well the answer is simply convenience and specificity, as languages can be made by developers themselves some just make it a hobby to create their own, part of like different parts of the world has different human languages to communicate in.
The other reason is some languages are made in such a way to ease some process, there's probably a reason that we use JavaScript of web browsers stuff, probably because someone who though about it made it to be used as language of the web hence it includes support for more browser oriented workings, similarly c++ is is used for trading why because that's what the use case trading community found it for as compared to other languages.
Different languages handle things differently example c, c++ like languages do not handle garbage collection the memory you allocated needs to be deallocated and some languages handle it themselves without the need of user intervention this probably comes at a cost of performance but the developer who is using this language does not need control over deallocation and is ready to bear this cost of performance. 