

- llc test.bc -o test.s

```assembly
	.text
	.file	"test.ll"
	.globl	mult                    # -- Begin function mult
	.p2align	4, 0x90
	.type	mult,@function
mult:                                   # @mult
	.cfi_startproc
# %bb.0:
	movl	%edi, %eax
	imull	%esi, %eax
	retq
.Lfunc_end0:
	.size	mult, .Lfunc_end0-mult
	.cfi_endproc
                                        # -- End function
	.section	".note.GNU-stack","",@progbits
```



- clang -S test.bc -o test.s -fomit-frame-pointer

```assembly
	.text
	.file	"test.ll"
	.globl	mult                    # -- Begin function mult
	.p2align	4, 0x90
	.type	mult,@function
mult:                                   # @mult
	.cfi_startproc
# %bb.0:
	imull	%esi, %edi
	movl	%edi, %eax
	retq
.Lfunc_end0:
	.size	mult, .Lfunc_end0-mult
	.cfi_endproc
                                        # -- End function
	.section	".note.GNU-stack","",@progbits
	.addrsig
```

