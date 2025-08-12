---
title: "X86 Assembly and C"
author: "hdcolin"
date: "2025-08-11"
summary: "Experimenting with calling X86 asm from a C program, inspired by vintage computing"
description: "Experiments with linking X86 asm and C"
readTime: false
autonumber: true
math: true
tags: ["c", "programming"]
showTags: false
hideBackToTop: false
---

I recently visited the The National Museum of Computing" - https://www.tnmoc.org/ in Milton Keynes UK. The museum contains the largest collection of working historic computers.

The museum showcases the early code-breaking computers used during the war to crack the Enigma code, all the way through to the large mainframes of the 60’s and 70’s. Some of these mainframes have only recently been decommissioned from running power stations.

It was interesting to see the adaptations that had been made to interact with these machines as modern computing had progressed. Historically they were programmed with a punched paper tape reel, however modern advancements introduced emulated paper tape programs that tricked the mainframe into thinking it had been fed a paper tape. Allowing the systems to stay (kind of) relevant.

Whilst wondering around the museum, it dawned on me how much modern computing has progressed. I could see how old computers really were “tools” compared to the glossy systems that we have today with complex graphical operating systems that can be used as much for recreation as well as work.

I felt inspired to see if I could interact with the low-level interfaces of the modern computer that I use for work, in a simmilar way that the technicians of the past would have.

Though not strictly related to film and TV post production; I often find myself experimenting with these side projects with the aim to learn what the tools are, how they work and if I can apply them to the areas of post-production or camera related topics that I am interested in. Either making tools to support and develop processes in the business, to learn something new, or just for fun.

I have a general understanding of C programming and for this experiment I chose to create a simple function in x86 assembler and call it from some C code. The function just returns the number that has been passed in as an argument. The C code, then prints out the result.

```
global my_function

section .text
my_function:
  push rbp
  mov rbp, rsp

  mov rax, rdi

  mov rsp, rbp
  pop rbp
  ret
```

To break this down:

`global my_function`

Declares the exported function name that will be called from the C code.

`section .text`

This defines the area of the code that the compiler uses to correctly place our function into the compiled binary. 

```
push rbp
mov rbp, rsp
```

This next section is within the definition of the function. It is known as the function prologue and is used to correctly handle the stack frame, ensuring that the function has its own frame for storing local variables and we can correctly return to the main program on return.

The same can be seen in the following code where we restore the pointer to the calling functions stack frame and return.

The single line of code mov rax, rdi moves the calling argument into the rax register that is used to handle return values.

The corresponding C code and output can be seen below. I’m no computer science assembly expert, but this felt like a rewarding project that I can develop further and eventually apply the techniques to industry specific challenges and problems.

```c
#include <stdio.h>

extern int my_function(int args);

int main() {
  int result = my_function(42);
  printf("%d\n",result);
  return 0;
}

OUTPUT:
42
```

I think that it's worth noting that this is purely experimental and for my own learning, to share the concept. I'm of the understanding that modern compilers are very advanced, and are able to generate optimised assembly far quicker than it would take to write it manually. 
