

- DragonEgg是一个GCC插件，它使得GCC能够使用LLVM优化器和代码生成 器来取代GCC自己的优化器和代码生成器。

- 你需要GCC 4.5及以上版本，目标机器为x86-32/x86-64以及ARM处理器。 当然，也需要下载DragonEgg源码并构建dragonegg.so动态链接库文件。

```c
#include<stdio.h>
int main() {
printf("hello world");
}
```



- gcc testprog.c -S -o1 -o -

```c
	.section	__TEXT,__text,regular,pure_instructions
	.build_version macos, 11, 0	sdk_version 11, 3
	.globl	_main                           ## -- Begin function main
	.p2align	4, 0x90
_main:                                  ## @main
	.cfi_startproc
## %bb.0:
	pushq	%rbp
	.cfi_def_cfa_offset 16
	.cfi_offset %rbp, -16
	movq	%rsp, %rbp
	.cfi_def_cfa_register %rbp
	subq	$16, %rsp
	leaq	L_.str(%rip), %rdi
	movb	$0, %al
	callq	_printf
	xorl	%ecx, %ecx
	movl	%eax, -4(%rbp)                  ## 4-byte Spill
	movl	%ecx, %eax
	addq	$16, %rsp
	popq	%rbp
	retq
	.cfi_endproc
                                        ## -- End function
	.section	__TEXT,__cstring,cstring_literals
L_.str:                                 ## @.str
	.asciz	"hello world"

.subsections_via_symbols
```





```c
3. 在gcc命令行中使用-fplugin=path/dragonegg.so参数，来让GCC调用LLVM 的优化器和代码生成器:
$ gcc testprog.c -S -O1 -o - -fplugin=./dragonegg.so
  .file " testprog.c"
# Start of file scope inline assembly
  .ident "GCC: (GNU) 4.5.0 20090928 (experimental) LLVM:
82450:82981"
# End of file scope inline assembly
  .text
  .align 16
  .globl main
  .type main,@function
main:
  subq $8, %rsp
  movl $.L.str, %edi
  call puts
  xorl %eax, %eax
  addq $8, %rsp
  ret
  .size main, .-main
  .type .L.str,@object
  .section
  .rodata.str1.1,"aMS",@progbits,1
.L.str:
  .asciz “Hello world!”
  .size .L.str, 13
.section .note.GNU-stack,"",@progbits

```

