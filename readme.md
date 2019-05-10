HW03
===
This is the hw03 sample. Please follow the steps below.

# Build the Sample Program

1. Fork this repo to your own github account.

2. Clone the repo that you just forked.

3. Under the hw03 dir, use:

	* `make` to build.

	* `make clean` to clean the ouput files.

4. Extract `gnu-mcu-eclipse-qemu.zip` into hw03 dir. Under the path of hw03, start emulation with `make qemu`.

	See [Lecture 02 ─ Emulation with QEMU] for more details.

5. The sample is a minimal program for ARM Cortex-M4 devices, which enters `while(1);` after reset. Use gdb to get more details.

	See [ESEmbedded_HW02_Example] for knowing how to do the observation and how to use markdown for taking notes.

# Build Your Own Program

1. Edit main.c.

2. Make and run like the steps above.

3. Please avoid using hardware dependent C Standard library functions like `printf`, `malloc`, etc.

# HW03 Requirements

1. How do C functions pass and return parameters? Please describe the related standard used by the Application Binary Interface (ABI) for the ARM architecture.

2. Modify main.c to observe what you found.

3. You have to state how you designed the observation (code), and how you performed it.

	Just like how you did in HW02.

3. If there are any official data that define the rules, you can also use them as references.

4. Push your repo to your github. (Use .gitignore to exclude the output files like object files or executable files and the qemu bin folder)

[Lecture 02 ─ Emulation with QEMU]: http://www.nc.es.ncku.edu.tw/course/embedded/02/#Emulation-with-QEMU
[ESEmbedded_HW02_Example]: https://github.com/vwxyzjimmy/ESEmbedded_HW02_Example

--------------------

- [x] **If you volunteer to give the presentation next week, check this.**

--------------------

#Question
How do C functions pass and return parameters?
* Modify main.c to observe what I found.
```
int plus(int a,int b){return a+b;}
 
void reset_handler(void)  
{ 
    int a=1,b=2;
    int c,d,e;  
    c=plus(a,b);  
    d=plus(c,10); 
    e=20;
 
    while(1)
	;  
}
```
* qemu objdump results
```
Disassembly of section .mytext:

00000000 <plus-0x8>:
   0:	20000100 	andcs	r0, r0, r0, lsl #2
   4:	00000025 	andeq	r0, r0, r5, lsr #32

00000008 <plus>:
   8:	b480      	push	{r7}
   a:	b083      	sub	sp, #12
   c:	af00      	add	r7, sp, #0
   e:	6078      	str	r0, [r7, #4]
  10:	6039      	str	r1, [r7, #0]
  12:	687a      	ldr	r2, [r7, #4]
  14:	683b      	ldr	r3, [r7, #0]
  16:	4413      	add	r3, r2
  18:	4618      	mov	r0, r3
  1a:	370c      	adds	r7, #12
  1c:	46bd      	mov	sp, r7
  1e:	f85d 7b04 	ldr.w	r7, [sp], #4
  22:	4770      	bx	lr

00000024 <reset_handler>:
  24:	b580      	push	{r7, lr}
  26:	b086      	sub	sp, #24
  28:	af00      	add	r7, sp, #0
  2a:	2301      	movs	r3, #1
  2c:	617b      	str	r3, [r7, #20]
  2e:	2302      	movs	r3, #2
  30:	613b      	str	r3, [r7, #16]
  32:	6978      	ldr	r0, [r7, #20]
  34:	6939      	ldr	r1, [r7, #16]
  36:	f7ff ffe7 	bl	8 <plus>
  3a:	60f8      	str	r0, [r7, #12]
  3c:	68f8      	ldr	r0, [r7, #12]
  3e:	210a      	movs	r1, #10
  40:	f7ff ffe2 	bl	8 <plus>
  44:	60b8      	str	r0, [r7, #8]
  46:	2314      	movs	r3, #20
  48:	607b      	str	r3, [r7, #4]
  4a:	e7fe      	b.n	4a <reset_handler+0x26>

Disassembly of section .comment:

00000000 <.comment>:
   0:	3a434347 	bcc	10d0d24 <reset_handler+0x10d0d00>
   4:	35312820 	ldrcc	r2, [r1, #-2080]!	; 0xfffff7e0
   8:	392e343a 	stmdbcc	lr!, {r1, r3, r4, r5, sl, ip, sp}
   c:	732b332e 			; <UNDEFINED> instruction: 0x732b332e
  10:	33326e76 	teqcc	r2, #1888	; 0x760
  14:	37373131 			; <UNDEFINED> instruction: 0x37373131
  18:	2029312d 	eorcs	r3, r9, sp, lsr #2
  1c:	2e392e34 	mrccs	14, 1, r2, cr9, cr4, {1}
  20:	30322033 	eorscc	r2, r2, r3, lsr r0
  24:	35303531 	ldrcc	r3, [r0, #-1329]!	; 0xfffffacf
  28:	28203932 	stmdacs	r0!, {r1, r4, r5, r8, fp, ip, sp}
  2c:	72657270 	rsbvc	r7, r5, #112, 4
  30:	61656c65 	cmnvs	r5, r5, ror #24
  34:	00296573 	eoreq	r6, r9, r3, ror r5

Disassembly of section .ARM.attributes:

00000000 <.ARM.attributes>:
   0:	00003041 	andeq	r3, r0, r1, asr #32
   4:	61656100 	cmnvs	r5, r0, lsl #2
   8:	01006962 	tsteq	r0, r2, ror #18
   c:	00000026 	andeq	r0, r0, r6, lsr #32
  10:	726f4305 	rsbvc	r4, pc, #335544320	; 0x14000000
  14:	2d786574 	cfldr64cs	mvdx6, [r8, #-464]!	; 0xfffffe30
  18:	0600344d 	streq	r3, [r0], -sp, asr #8
  1c:	094d070d 	stmdbeq	sp, {r0, r2, r3, r8, r9, sl}^
  20:	14041202 	strne	r1, [r4], #-514	; 0xfffffdfe
  24:	17011501 	strne	r1, [r1, -r1, lsl #10]
  28:	1a011803 	bne	4603c <reset_handler+0x46018>
  2c:	22061e01 	andcs	r1, r6, #1, 28
  30:	Address 0x0000000000000030 is out of bounds.
```
* When we call the function including main.c and subroutine, we can see the register {7} pushed into stack space in the begining.
```
  00000024 <reset_handler>:
  24:	b580      	push	{r7, lr}
```
```
  00000008 <plus>:
  8:	b480      	push	{r7}
```
* Also, We can see r0~r3 process variables by storing, loading, moving, and adding in every function.
```
  2a:	2301      	movs	r3, #1
  2c:	617b      	str	r3, [r7, #20]
  2e:	2302      	movs	r3, #2
  30:	613b      	str	r3, [r7, #16]
  32:	6978      	ldr	r0, [r7, #20]
  34:	6939      	ldr	r1, [r7, #16]
```
```
  e:	6078      	str	r0, [r7, #4]
  10:	6039      	str	r1, [r7, #0]
  12:	687a      	ldr	r2, [r7, #4]
  14:	683b      	ldr	r3, [r7, #0]
  16:	4413      	add	r3, r2
  18:	4618      	mov	r0, r3
```
* In subroutine, we get our results from moving r7 to sp, and then send back to main function by instuction bx.
```
  1a:	370c      	adds	r7, #12
  1c:	46bd      	mov	sp, r7
  1e:	f85d 7b04 	ldr.w	r7, [sp], #4
  22:	4770      	bx	lr
```
#Conclusion
* Reference from Procedure Call Standard for the ARM® Architecture. It describes each functions of registers.
![image](https://github.com/P86071244/ESEmbedded_HW03/blob/master/AAPCS%20core%20registers.jpg)
* r0~r3 are used to pass argument values into subroutine and to return a result value from a function.
![image](https://github.com/P86071244/ESEmbedded_HW03/blob/master/r0r1r2r3.jpg)
