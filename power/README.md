# Power

## 執行
```
PS C:\Users\teemo\Desktop\系統程式\sp108b\homework\power> gcc .\PowerJitCall.c -o power
PS C:\Users\teemo\Desktop\系統程式\sp108b\homework\power> ./power
power(2, 3)=8
```

## 原理

```
PS C:\Users\teemo\Desktop\系統程式\sp108b\homework\power> gcc -c power.c
PS C:\Users\teemo\Desktop\系統程式\sp108b\homework\power> objdump -d power.o

power.o:     file format pe-i386

Disassembly of section .text:

00000000 <_power>:
   0:   55                      push   %ebp
   1:   89 e5                   mov    %esp,%ebp
   3:   83 ec 10                sub    $0x10,%esp
   6:   c7 45 fc 01 00 00 00    movl   $0x1,-0x4(%ebp)
   d:   8b 45 0c                mov    0xc(%ebp),%eax
  15:   8b 45 fc                mov    -0x4(%ebp),%eax
  1f:   83 6d f8 01             subl   $0x1,-0x8(%ebp)
  23:   83 7d f8 00             cmpl   $0x0,-0x8(%ebp)
  27:   7f ec                   jg     15 <_power+0x15>
  29:   8b 45 fc                mov    -0x4(%ebp),%eax
  2c:   c9                      leave
  2e:   90                      nop
  2f:   90                      nop
```
