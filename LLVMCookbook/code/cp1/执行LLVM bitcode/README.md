



```sh
root@satisfying-fowl:/home/ubuntu# ls
2         linux-4.17-rc2         multiply.c   multiply1.ll  test.bc  test.s    test1.c   test2.bc  test2.ll
app.yaml  linux-4.17-rc2.tar.gz  multiply.ll  output.bc     test.ll  test1.bc  test1.ll  test2.c
root@satisfying-fowl:/home/ubuntu# 
root@satisfying-fowl:/home/ubuntu# pwd
/home/ubuntu
root@satisfying-fowl:/home/ubuntu# lli output.bc
number is 10
root@satisfying-fowl:/home/ubuntu# 
```

工作原理
lli工具命令执行LLVM bitcode格式程序，它使用LLVM bitcode格式作为输入 并且使用即时编译器(JIT)执行。当然，如果当前的架构不存在JIT编译器，会 用解释器执行。
如果lli能够采用JIT编译器，那么它能高效地使用所有代码生成器参数，如 llc。