Download Link: https://assignmentchef.com/product/solved-cs-3844-computer-organization-lab-07
<br>
This lab covers loops and string instructions. You will be stepping through some code using some string instructions and writing code to do a loop.




First, examine the code in Lab7.cpp. at the top of the file, you will find “<strong>#define</strong><strong> STRLEN_TEST</strong>” which is choosing the code that uses the C strlen library function. There are two global character arrays defined, n and q. The two prototypes for the solution functions are included – you will get those files later. FUNC1_Student and FUNC2_Student are ready for your modification. In this lab, you will write code for FUNC1_Student. In Lab #8, you will write code for FUNC2_Student.




Next is FUNC0 which takes a function pointer as an argument. The program as posted, uses the address of the C library function “strlen” here. You are going to write assembly code functionally equivalent to strlen and put that in FUNC1_Student.




Set a BreakPoint (BP) on the “return 0” at the end of main and on the “return len” at the end of FUNC0.




Run the program inside the debugger and view the output window. You should see the following: “FUNC: Length = 14.” Then click the green arrow and continue to the end. Your output window should now have this, “NewString: Exams are fun!  But this lab sux!” displayed after the first statement.




This program calls FUNC0 with the address of a function that gets the length of the global string “n.” Then it runs the assembly code which concatenates “n” with string “q.” Continue until the program ends.




Add a BP to this statement in main “<strong>phraseLength = FUNC0( (DWORD *)  strlen );</strong>”, run the program, and view the disassembly. Note: If you are not seeing the machine code values in the disassembly window, right-click and select “Show Code Bytes.”




We are passing the address of strlen into FUNC0 and typecasting it as a DWORD pointer. The reason for this is simple. If we passed an actual function pointer, we’d have to define FUNC0 as taking a pointer to a function with a <em><u>specific prototype</u></em> and the prototype for strlen is different than the prototype for FUNC1_Student and FUNC2_Student. This way, we can pass any function address.







<ol>

 <li>Single-step into the call to FUNC0, continue past the extra jmp, and get to the push ebp in FUNC0. Then step until you get to “push ebx” and stop there. In a memory window, type in ebp. Note, some values may vary so use my stack below to answer the question.  EBP = 0x18FED4.</li>

</ol>




<strong>0x0018FED4  34 ff 18 00 d3 d6 42 00 f9 b6 42 00 00 00 00 00  4ÿ..ÓÖB.ù¶B…..</strong>

<strong>0x0018FEE4  00 00 00 00 00 e0 fd 7e e0 21 43 00 d9 21 43 00  …..àý~à!C.Ù!C.</strong>




<ol>

 <li>Looking at my stack, what is the value of the prior EBP.</li>

</ol>




<ol>

 <li>What is the return address?</li>

</ol>




<ol>

 <li>What is the address of strlen?</li>

</ol>

<strong> </strong>




<ol start="2">

 <li>Step to the “<strong>0042D680 8D 4D FC lea  ecx,[len]</strong>” instruction and note the value in ECX before executing it. Now, execute the instruction. What is the hex value in ECX? The description would be the address of the local variable “len” which is at displacement -4 in EBP.</li>

</ol>




<ol start="3">

 <li>Single step to the call instruction “<strong>0042D68B FF 55 08 call dword ptr [funcAddress].</strong>” It is actually a call [ebp+8] which means it will add 8 to EBP, use that address to retrieve the address to call. Before taking the call, what is the destination address?</li>

</ol>




<ol start="4">

 <li>Since this is a call to strlen, no need to step into it. Step over it to the “<strong>mov dword ptr [len], eax</strong>” instruction. What is this instruction doing? NOTE: We only do this for strlen, your function will populate len directly.</li>

</ol>




<ol start="5">

 <li>What is the length of the string?</li>

</ol>




<ol start="6">

 <li>Type “&amp;len” in a memory window. The current value is zero since it was initialized. Step to the next instruction and see the red 0e in the memory window as eax was written. What is the offset of the local variable “len” from EBP?</li>

</ol>




<ol start="7">

 <li>The next few instructions call printf, step over all that until you get to “<strong>mov eax,dword ptr [len]</strong>.” Why are we doing this?</li>

</ol>




Step over the next call to printf and all the way until you are out and back in main. Should be on an “<strong>add esp,4</strong>” instruction. Single step up to the “<strong>push 400h</strong>” instruction.  Since you have the source code, you should be able to easily determine what this part of the code does. On the next exam, you could be required to write the C code for something like this.




<strong>0042D6D9 68 00 04 00 00   push        400h </strong>

<strong>0042D6DE E8 1B E0 FF FF   call        @ILT+1785(_malloc) (42B6FEh) </strong>

<strong>0042D6E3 83 C4 04         add         esp,4 </strong>

<strong>0042D6E6 89 45 FC         mov         dword ptr [ptrNewString],eax </strong>

<strong>0042D6E9 83 7D FC 00      cmp         dword ptr [ptrNewString],0 </strong>

<strong>0042D6ED 75 14            jne         main+43h (42D703h) </strong>

<strong> </strong>

<strong>0042D6EF 68 98 2C 48 00   push        offset string “Error – Could not allocate 1024 ” </strong>

<strong>0042D6F4 E8 19 EA FF FF   call        @ILT+4365(_printf) (42C112h) </strong>

<strong>0042D6F9 83 C4 04         add         esp,4 </strong>

<strong>0042D6FC 6A FF            push        0FFFFFFFFh </strong>

<strong>0042D6FE E8 31 EB FF FF   call        @ILT+4655(_exit) (42C234h)</strong>




<ol start="8">

 <li>Step over all of the instructions until you get past the call to _exit. Stop on instruction “<strong>mov edi,dword ptr [ptrNewString]</strong>” EDI gets the address of the newly allocated memory – this is heap memory – which is the <strong><em>destination</em></strong> for the string “n.” ESI gets the <strong><em>source</em></strong> Step to the “loads” (load string) instruction. What are the values of EDI, ESI, and ah?</li>

</ol>




<ol start="9">

 <li>You should be familiar with the string instructions from the narrations and slides. If you skipped all that you are going to have trouble with this – I recommend that you watch the narrations and review the slides before you continue. After executing lods/stos what value do you expect to be in EDI/ESI/ah/al? For each of those registers, explain why the value of each is what it is.</li>

</ol>




<ol start="10">

 <li>“<strong>Test al,al</strong>” will not set the zero flag at this point, so jne will go to REPEAT_OP. How many times will this loop and why is it that specific number?</li>

</ol>




<ol start="11">

 <li>Set a BP on the “<strong>test ah,ah</strong>” instruction. Hit continue. In a memory window, get the contents of the allocated memory and show it here. Address may vary, but data should match solutions.</li>

</ol>




<ol start="12">

 <li>Based on the value of AH, will the jne be taken?</li>

</ol>




<ol start="13">

 <li>Single step to the “<strong>jmp REPEAT_OP</strong>” instruction. We inc AH to set a flag so this will not be repeated. We load the effective address of “q” into ESI. We are going to concatenate “n” and “q.” Next we “<strong>dec EDI</strong>” – why is that? Hint: look at EDI -2 in a memory window.</li>

</ol>




<ol start="14">

 <li>Continue again to the “<strong>test ah,ah</strong>” instruction. Type “ptrNewString” in a memory window. How long is the new string in bytes? Once you know, continue and exit the program.</li>

</ol>




<ol start="15">

 <li>Now, to complete the lab, comment out “<strong>#define</strong> <strong>STRLEN_TEST</strong>” and uncomment the source “<strong>phraseLength = FUNC0( (DWORD *) FUNC1_Student);</strong>” in main. You need to write an assembly routine to do a strlen EXCEPT the result will be stored in the 2<sup>nd</sup> parameter rather than the return value. For lab #7 do not use string instructions, that will be lab #8.</li>

</ol>




If your code is correct, your output will look exactly the same as the one when passing strlen.