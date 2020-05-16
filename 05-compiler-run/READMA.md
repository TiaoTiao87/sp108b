# Compiler

main.c => lexer.c
          compiler.c
          ir.c       => irvm.c

test mode
```
a = 3;
b = 5;
if (a > b)
  t = a;
else
  t = b;
```

```
=======irDump()==========
00: t1 = 3
01: a = t1
02: t1 = 5
03: b = t1
04: t2 = a
05: t3 = b
06: t4 = t2 > t3
07: ifnot t4 goto L1
08: t1 = a
09: t = t1
10: if t4 goto L2
11: (L1)
12: t1 = b
13: t = t1
14: (L2)
===================irRun()=======================
00: t1 = 3 (3)
01: a = t1 (3)
02: t1 = 5 (5)
03: b = t1 (5)
04: t2 = a (3)
05: t3 = b (5)
06: t4 = t2 > t3 (0)
07: ifnot t4 (0) goto L1 (11)
11: (L1) (11)
12: t1 = b (5)
13: t = t1 (5)
14: (L2) (14)
```
