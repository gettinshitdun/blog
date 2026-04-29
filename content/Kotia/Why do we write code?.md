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
Work in progress
## If that's all what code do, then why do we need so many of these programming languages?
Work in progress
-----------------------------------------------x-------------------------------------------------x-------------------------------------------x

In this day and age of AI, can we answer this simple question, Why do we write code?

We write code to tell computers to do what we want it to do 
for example If i need to interact with my smartphone essentially looking up a notification, we have baked this into code that when user swipes this way a notification if exists show it to user.
# Why do we need code in the first place?
That's a great question, why can't we just make our computers in this way that it understands English, well that's not a good.
It seems  AI (apparently) is doing just that, it lets humans like us who doesn't know code in any way interact with computers through plain language.
But the question remains, because even though I could tell AI to tell the computer to do this. 
AI too actually codes it, so essentially the superpower is not the computer or the AI but actually "code". 
So code let's us do whatever we want
Again the question remains why. Because actually code is not ambiguous at all. Computers are really just a large logic machine you tell it this is how you should react at this and it would exactly do the same thing. The problem with English is that it's ambiguous not to us because we have been practicing it daily so we kinda understand the meaning of "this", "that", "where", "here" and maybe dozen more. 
But what about the logic machine? Actually it needs really really strict instructions and this is where code comes in.

Now the next question is how do computers do that. What really happens when we write code.

# Computer and the code
Now the next question is how do computers do that. What really happens when we write code.

Now to understand this we have two aspects the computer and the code itself
For simplicity i will tell you that the code is just a magic language which somehow gets converted to something we call binary (binary means two i.e. 0's and 1's)
So essentially whatever the code is written somehow gets converted into these 0's and 1's

Now the question is who does this?
What's this magical thing which converts code into binary and that too not jargon binary a stream of 0's and 1's which actually is meaningful for the computer.
This magical thing is another program called the compiler, the compiler is responsible for converting the code into it's correct form of 0's and 1's.
Now understanding how does a compiler do this is a big task to in itself, Hence i am planning to create another blog which explores the inner workings of the compiler.
But i think for now we could consider the compiler as a magic program which would give us the right value of 0's and 1's

So with this let's move on to now the second piece of the puzzle,  The "computer" so the question is that with the compiler doing it's job, the computer actually understands those 0's and 1's. How is that possible?

So let's pose another question Why 0's and 1's why not just plain code?

# Why do we need binary system
Well there are multitude of reasons why and we will explore some of them
1. Computers are electronic machines and the electronics are always made in such a way that they understand 0's and 1's easily, well it's just either on or off all electronic components works on this theory example vacumm tubes, even when there wasn't a computer still for mechanical machines we used punch cards which again comes back to 0's and 1's.
2. Binary is much more reliable as compared any other thing, we would never need a whole scale of values for example take electrical signals here! Let's assume that the range of my electrical signals is 0-5V. And now I define different numbers as values so maybe 0.5V, 1V, 1.5V, 2V, 2.5V, 3V, 3.5V, 4V, 4.5V, 5V assume all of these values so now the processor needs a very accurate way to measure this signal to get the correct value, and if by any chance there seems to be some noise added in the system there is a very high chance that the value calculated is wrong so now i have to make very very accurate systems to detect values, that is easy to make but also i have to create wires or ways for the electricity to travel very reliably is this possible, no not at all cause noise by default is introduced by nature and we can't do anything about it. Hence it's way better to reduce the values into two 0V, 5V anything in between could be rounded off to these two values and this makes the signal way more reliable hence binary is the way 
3. This one is something which i recognized while i was researching, Many mathematical operations are actually very easy to do in binary.
4. Now you may be asking that now we could probably innovate and bring in another system to help us be efficient, the answer is yes but the truth is it's far more easier as the systems are already built for us, why do we need to reinvent the wheel when there are already things made upon it example logic gates, transistors they are a beauty in research and we need to abandon it that seems wrong

Ok we get it, computer need binary but now we pose another question if that's so. if the only thing they need is binary values then why the heck computer has this much components?
Why do we need a RAM, SSD, CPU, Motherboard, and tons of other stuff.  Why's that only one wire isnt't enough

# The Computer and its components
The reason is binary itself, the more simpler the input the more complex circuits are needed to be made to decode these inputs
Yes we can make everything into one, we have always been told that the CPU is the brain of the computer so we kinda can put everything we need into this CPU and call it a day, but is that viable for us, From an engineering perspective we say that if some tasks that the CPU can do be given some other component, maybe this will make our CPU faster and then overall computer faster that's what we need right.
Hence some components originated that way, some just originated because of the limitation of how the CPU was made, CPU was never made to store data so we would always need
It all started with the von neuman architecture, but there also exists something called a harvard architecture.
So there are two types of architecture, architecture -> the way a computer is built from pieces.
1. Von neumann architecture says that we have a central processing unit which has the control unit as well the arithmetic logic unit. We also have a memory unit where the data and the instructions are stored
   
   ![[Pasted image 20260422193130.png]]

	Which is then used for and by both input and output device. This is one of the first architectures ever built hence was used for the initial computers 

2. Harvard Architecture



```
Why do we write code?
	What is code?
	Why is it important to write it
	
	What is a computer, why do we need it
	What's machine?
	
	What's binary
	Why  can't humans do binary
	How does code gets coverted to 0's and 1's
	Why code cannot be english
	
	How is code affecting the state of this computer
	If that's all what code do then why do we need so many of it?
```