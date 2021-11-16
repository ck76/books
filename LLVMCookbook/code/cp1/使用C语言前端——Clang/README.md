





$ clang test.c
$ ./a.out
hello world



- 3. 除此之外，Clang也能用作预处理器，只需要增加-E参数。下面的样例 中，C语言代码中用#define定义了一个宏MAX，其值为100，用来作为即将创建 的数组的长度:

```c
root@satisfying-fowl:/home/ubuntu# vim macro.c
root@satisfying-fowl:/home/ubuntu# clang macro.c -E 
# 1 "macro.c"
# 1 "<built-in>" 1
# 1 "<built-in>" 3
# 341 "<built-in>" 3
# 1 "<command line>" 1
# 1 "<built-in>" 2
# 1 "macro.c" 2


void func() {
 int a[100];
}
root@satisfying-fowl:/home/ubuntu# 
```



- 你能用下面的命令打印前面样例中test.c文件的抽象语法树，在标准输出 中输出结果
  - 这里的-cc1参数保证了只运行编译器前端，而不是编译器驱动，它输出了 test.c文件代码的AST。

```c
root@satisfying-fowl:/home/ubuntu# cat test.c
#include<stdio.h>
int main() {
printf("hello world\n");
return 0; }
```

- clang -cc1 test.c -ast-dump

```c
test.c:1:9: fatal error: 'stdio.h' file not found
#include<stdio.h>
        ^~~~~~~~~
TranslationUnitDecl 0xbda788 <<invalid sloc>> <invalid sloc>
|-TypedefDecl 0xbdb020 <<invalid sloc>> <invalid sloc> implicit __int128_t '__int128'
| `-BuiltinType 0xbdad20 '__int128'
|-TypedefDecl 0xbdb090 <<invalid sloc>> <invalid sloc> implicit __uint128_t 'unsigned __int128'
| `-BuiltinType 0xbdad40 'unsigned __int128'
|-TypedefDecl 0xbdb398 <<invalid sloc>> <invalid sloc> implicit __NSConstantString 'struct __NSConstantString_tag'
| `-RecordType 0xbdb170 'struct __NSConstantString_tag'
|   `-Record 0xbdb0e8 '__NSConstantString_tag'
|-TypedefDecl 0xbdb430 <<invalid sloc>> <invalid sloc> implicit __builtin_ms_va_list 'char *'
| `-PointerType 0xbdb3f0 'char *'
|   `-BuiltinType 0xbda820 'char'
|-TypedefDecl 0xbdb728 <<invalid sloc>> <invalid sloc> implicit __builtin_va_list 'struct __va_list_tag [1]'
| `-ConstantArrayType 0xbdb6d0 'struct __va_list_tag [1]' 1 
|   `-RecordType 0xbdb510 'struct __va_list_tag'
|     `-Record 0xbdb488 '__va_list_tag'
|-FunctionDecl 0xc393c0 <test.c:2:1, line:4:11> line:2:5 main 'int ()'
| `-CompoundStmt 0xc397c8 <col:12, line:4:11>
|   |-CallExpr 0xc39740 <line:3:1, col:23> 'int'
|   | |-ImplicitCastExpr 0xc39728 <col:1> 'int (*)(const char *, ...)' <FunctionToPointerDecay>
|   | | `-DeclRefExpr 0xc39660 <col:1> 'int (const char *, ...)' Function 0xc394e8 'printf' 'int (const char *, ...)'
|   | `-ImplicitCastExpr 0xc39780 <col:8> 'const char *' <NoOp>
|   |   `-ImplicitCastExpr 0xc39768 <col:8> 'char *' <ArrayToPointerDecay>
|   |     `-StringLiteral 0xc396b8 <col:8> 'char [13]' lvalue "hello world\n"
|   `-ReturnStmt 0xc397b8 <line:4:1, col:8>
|     `-IntegerLiteral 0xc39798 <col:8> 'int' 0
`-FunctionDecl 0xc394e8 <line:3:1> col:1 implicit used printf 'int (const char *, ...)' extern
  |-ParmVarDecl 0xc39588 <<invalid sloc>> <invalid sloc> 'const char *'
  `-FormatAttr 0xc395f8 <col:1> Implicit printf 1 2
1 error generated.
root@satisfying-fowl:/home/ubuntu# 
```



- 你也可以用下面的命令来为前面样例中的test.c文件生成LLVM汇编码

clang test.c -S -emit-llvm -o -

```c
; ModuleID = 'test.c'
source_filename = "test.c"
target datalayout = "e-m:e-p270:32:32-p271:32:32-p272:64:64-i64:64-f80:128-n8:16:32:64-S128"
target triple = "x86_64-pc-linux-gnu"

@.str = private unnamed_addr constant [13 x i8] c"hello world\0A\00", align 1

; Function Attrs: noinline nounwind optnone uwtable
define dso_local i32 @main() #0 {
  %1 = alloca i32, align 4
  store i32 0, i32* %1, align 4
  %2 = call i32 (i8*, ...) @printf(i8* getelementptr inbounds ([13 x i8], [13 x i8]* @.str, i64 0, i64 0))
  ret i32 0
}

declare dso_local i32 @printf(i8*, ...) #1

attributes #0 = { noinline nounwind optnone uwtable "correctly-rounded-divide-sqrt-fp-math"="false" "disable-tail-calls"="false" "frame-pointer"="all" "less-precise-fpmad"="false" "min-legal-vector-width"="0" "no-infs-fp-math"="false" "no-jump-tables"="false" "no-nans-fp-math"="false" "no-signed-zeros-fp-math"="false" "no-trapping-math"="false" "stack-protector-buffer-size"="8" "target-cpu"="x86-64" "target-features"="+cx8,+fxsr,+mmx,+sse,+sse2,+x87" "unsafe-fp-math"="false" "use-soft-float"="false" }
attributes #1 = { "correctly-rounded-divide-sqrt-fp-math"="false" "disable-tail-calls"="false" "frame-pointer"="all" "less-precise-fpmad"="false" "no-infs-fp-math"="false" "no-nans-fp-math"="false" "no-signed-zeros-fp-math"="false" "no-trapping-math"="false" "stack-protector-buffer-size"="8" "target-cpu"="x86-64" "target-features"="+cx8,+fxsr,+mmx,+sse,+sse2,+x87" "unsafe-fp-math"="false" "use-soft-float"="false" }

!llvm.module.flags = !{!0}
!llvm.ident = !{!1}

!0 = !{i32 1, !"wchar_size", i32 4}
!1 = !{!"clang version 10.0.0-4ubuntu1 "}
```



- 为了得到用于相同test.c测试码的机器码，需要给Clang传递-S参数。如果 你使用了-o -参数，则会在标准输出中输出结果:

clang -S test.c -o-

```c
	.text
	.file	"test.c"
	.globl	main                    # -- Begin function main
	.p2align	4, 0x90
	.type	main,@function
main:                                   # @main
	.cfi_startproc
# %bb.0:
	pushq	%rbp
	.cfi_def_cfa_offset 16
	.cfi_offset %rbp, -16
	movq	%rsp, %rbp
	.cfi_def_cfa_register %rbp
	subq	$16, %rsp
	movl	$0, -4(%rbp)
	movabsq	$.L.str, %rdi
	movb	$0, %al
	callq	printf
	xorl	%ecx, %ecx
	movl	%eax, -8(%rbp)          # 4-byte Spill
	movl	%ecx, %eax
	addq	$16, %rsp
	popq	%rbp
	.cfi_def_cfa %rsp, 8
	retq
.Lfunc_end0:
	.size	main, .Lfunc_end0-main
	.cfi_endproc
                                        # -- End function
	.type	.L.str,@object          # @.str
	.section	.rodata.str1.1,"aMS",@progbits,1
.L.str:
	.asciz	"hello world\n"
	.size	.L.str, 13

	.ident	"clang version 10.0.0-4ubuntu1 "
	.section	".note.GNU-stack","",@progbits
	.addrsig
	.addrsig_sym printf
```



- 在只使用-S参数的情况下，编译器会在代码生成的过程中产生机器码。这 里使用了-o –参数，因此执行命令后机器码直接输出到了标准输出。





