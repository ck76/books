



- llvm-dis test.bc -o test2.ll

```
BC??5b
      0$JY?fM??O
                Q?L!
                    ?
                     !?#?A?I29??
                                ?b?
                                   EB?
                                      Bd2K
22?Hp?!#D??A?d? CF? ?22?*(*?1|?\? ??? 
                                      2" d??!???!ㄡ?L??
                                                       ?dLs`P???!
                                                                 yL?4??2???$???????f=?C8?ÌB?yxs?q
                                                                                                 ???3
                                                                                                     B??Ρf0=?C8??=?C=?=?x?tpyH?ppzpvx?p ????0n0???P3??!?!?af0?;??;?C9?<??<?;??v`{h7h?rh7??p??p`v(v?vx?w???q?r??y??,???????0bȡ?̡??a?!ā?a֐C9?C9?C9?C9??8?C8?;??/??<??;?;??
                                                                                                          ?i?pX?rp?thx`?t?t???S??P??@? ?P3 (???A?!܁?????fQ8?C:??;?P$v`{h7`?wxx?QL???P3j?a?!??~??!?aT??8??;?C=?C9??<?C;??;?Ì?
?y??w?z(r??\????P?0#??A?????fH;??=???8?C9??<??9??;?<?H?v`?qX????`??? ?0? ?Pn?0?0?????P?0C??!???a?aF???8?;??/?C:?C:?C:?C:?C>?

?A???!?!??4?`?P? ?@? ?P???<??;?;?=??<?C8??a 
l&q 2"??]
         ?mult10.0.0test.ll
```



- 生成的ll

```c
; ModuleID = 'test.bc'
source_filename = "test.ll"

define i32 @mult(i32 %a, i32 %b) {
  %1 = mul nsw i32 %a, %b
  ret i32 %1
}
```

