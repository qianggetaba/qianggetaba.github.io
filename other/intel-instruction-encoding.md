测试 汇编指令格式

AT&T
test.s

    .global _start
    _start:
    .byte 0,0,0,0,0,0

可以直接写一条mov指令

    mov $0x34,%al
或者直接写指令编码

    .byte 0xb0,0x35
然后编译看反汇编

    gcc -m32 -c test.s | objdump -d test.o
    gcc -m32 -c test.s | objdump -d -D -Maddr16,data16 test.o   16bit
windows下nasm

    nasm -f bin test.asm & ndisasm -b 16 test

寄存器顺序表示值，就是modR/M表里的寄存器顺序值

    0    1    2    3    4    5    6     7
    EAX  ECX  EDX  EBX  ESP  EBP  ESI   EDI
    0    1    2    3    4    5    6     7
    AL   CL   DL   BL   AH   CH   DH    BH
    
    B8 78 56 34 12  MOV EAX, 0X12345678 ; B9就是MOV ECX,以此类推
    B0 35           MOV AL,  0X35       ; B1就是MOV CL
    90              NOP                 ; 可以看做XCHG EAX, EAX 交换内容, 91就是XCHG EAX, ECX
    40              INC EAX             ;  41是
    50              PUSH EAX            ;
intel手册

指令格式主要在volume2--2.1

volume2--appendix A opcode map,A.3 ONE, TWO, AND THREE-BYTE OPCODE MAPS，单双三字节指令表

EB代表1字节内存，GB代表8位寄存器,GV代表32或8位寄存器,EV代表4字节内存，IB代表1字节立即数，ID代表4字节立即数，Ib立即数
这些缩写在volume2--appendix A opcode map,A.2.1 Codes for Addressing Method，每个字母代表的含义
ev又可以表示为r/m32

行号列号组成一个字节，两位16进制数表示一条指令
06  push es  第0行第6列  ，右上角i64,invalid 64位不能用

add eax, eax 16位下66 01 C0;  66表示使用32位寄存器长度，ax-->eax, 01是单指令表的第0行第1列，类型是ADD Ev Gv，在16位下就是ADD r/m16, r16
                              加66前缀后，变为类型ADD r/m32, r32，在Table 2-1. 16-Bit Addressing Forms with the ModR/M Byte表中就是c0
                              c0 划分modR/M格式  11  000  000,11表示两个参数都是寄存器，第一个000就是第一个参数的寄存器值


add    %al,(%bx,%si)  <>   add    BYTE PTR [bx+si],al

ndisasm -b {16|32|64} filename