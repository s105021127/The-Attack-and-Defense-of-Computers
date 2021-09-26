# FromSourceToExecutive

# 1.Linux C 程式的編譯與運行:
## 範例
```
// helloCTFer.c
#include <stdio.h>

int main()
{
   printf("Hello CTFer\n ”);
   return 0;
}
```
### Kali 實作
- 用leafpad寫c code
![image](https://user-images.githubusercontent.com/22366572/134798466-e78bc372-c6b2-46df-889f-8aabd7d37257.png)

## Unumtu 18.04 LTS/Kali Linux

| (1) 編譯 | (2)執行 |(3)檢查執行檔檔案格式|
|-----------------------|-------------|-------------|
| gcc helloCTFer.c   ==>  產生a.out執行檔 |  ./a.out | file ./a.out|
|gcc helloCTFer.c -o helloCTFer |./helloCTFer   | file ./helloCTFer   |
|gcc helloCTFer.c -o helloCTFer.exe | ./helloCTFer.exe | file ./helloCTFer.exe|

- strings ./helloCTFer
- hexdump -C helloCTFer.exe


- [ELF File magic](https://unix.stackexchange.com/questions/153352/what-is-elf-magic)
- hexdump 的用法
```
usage: hexdump [-bcCdovx] [-e fmt] [-f fmt_file] [-n length]
               [-s skip] [file ...]
       hd      [-bcdovx]  [-e fmt] [-f fmt_file] [-n length]
               [-s skip] [file ...]

hexdump還有很多的用法，具體可以參看man hexdump
https://man7.org/linux/man-pages/man1/hexdump.1.html

https://man.linuxde.net/hexdump
https://www.itread01.com/article/1513751613.html
https://www.itread01.com/p/1385440.html
```
### Kali 實作

- strings test.exe
![image](https://user-images.githubusercontent.com/22366572/134800003-d783887c-7725-4e7d-a3a0-3c4a19f753bd.png)

- hexdump test.exe (產生16位元訊息)
![image](https://user-images.githubusercontent.com/22366572/134799588-7d07a932-2987-4ebd-81d3-8e10c68e0768.png)

- hexdump -C test.exe
![image](https://user-images.githubusercontent.com/22366572/134799630-34d419b1-fb5c-4b1a-a4e1-5a588936852a.png)

- ELF File magic
```
readelf -h /bin/ls
ELF Header:
  Magic:   7f 45 4c 46 02 01 01 00 00 00 00 00 00 00 00 00 
  Class:                             ELF64
  Data:                              2's complement, little endian
  Version:                           1 (current)
  OS/ABI:                            UNIX - System V
  ABI Version:                       0
  Type:                              DYN (Shared object file)
  Machine:                           Advanced Micro Devices X86-64
  Version:                           0x1
  Entry point address:               0x6160
  Start of program headers:          64 (bytes into file)
  Start of section headers:          145256 (bytes into file)
  Flags:                             0x0
  Size of this header:               64 (bytes)
  Size of program headers:           56 (bytes)
  Number of program headers:         11
  Size of section headers:           64 (bytes)
  Number of section headers:         30
  Section header string table index: 29
```

# 2_Linux C 程式的編譯與運行:

### 編譯的各階段

![CompilationProcess.png](CompilationProcess.png)

### 預處理preprocessing
- Includes the files containing definitions (include/header files) into the source files, as specified by the #include keyword.
- Converts the values specified by using #define statements into the constants.
- Converts the macro definitions into code at the variety of locations in which the macros are invoked.
- Conditionally includes or excludes certain parts of the code, based on the position of #if, #elif, and #endif directives.

### 編譯compilation
- 有許多工作  Lexical analysis,Semantic analysis  
- 產生組合語言程式

### 彙編assembly 
- convert the standard language constructs into the constructs specific to the actual CPU instruction set
- 最後產生object file
- The assembly instructions (written in human-readable ASCII code) are at this stage converted into the binary values of the corresponding machine instructions (opcodes) and written to the specific locations in the object file(s)
- Object File性質

### 連結linking
- relocation 重定位
- Resolving References

原始程式碼 ==> helloCTFer.c

| PHASE |command|
|-----------------------|-------------|
|預處理preprocessing|gcc –E helloCTFer.c –o helloCTFer.i |
|編譯compilation |gcc –S helloCTFer.i  -o helloCTFer.s |
|彙編assembly  |gcc -c helloCTFer.s -o helloCTFer.o |
|連結linking | gcc  helloCTFer.o -o helloCTFer  |

```
gcc helloCTFer.c -o hello -save-temps --verbose

編譯選項:
"-save-temps" ==> 將編譯過程中生成的中間檔案保存下來
"--verbose" ==> 查看GCC 編譯的詳細工作流程，
```

### Kali 實作

- (法1) 用 gcc –E helloCTFer.c –o helloCTFer.i / gcc –S helloCTFer.i  -o helloCTFer.s / gcc -c helloCTFer.s -o helloCTFer.o / gcc  helloCTFer.o -o helloCTFer 分別建立file
- (法2) 用 gcc helloCTFer.c -o hello -save-temps --verbose 一次建立好所有檔案

![image](https://user-images.githubusercontent.com/22366572/134799105-d0737a60-fbb5-4ee4-8621-6c1b62088195.png)



## 3_組語程式格式:[AT&T Syntax]  VS  [Intel Syntax]
```
gcc helloCTFer.c -o helloCTFer.s
gcc -S -masm=AT&T  my_XXX.c -o my_XXX.s

gcc -S -masm=intel my_XXX.c -o my_XXX.s 

gcc -S -masm=intel my_XXX.c -o my_XXX.s  -fno-asynchronous-unwind-tables
```
### Kali 實作
- gcc -S -masm=AT&T  my_XXX.c -o my_XXX.s (gcc –S helloCTFer.i  -o helloCTFer.s 預設就是AT&T)
```
cat test.s                             1 ⨯
        .file   "test.c"
        .text
        .section        .rodata
.LC0:
        .string "Hello CTF"
        .text
        .globl  main
        .type   main, @function
main:
.LFB0:
        .cfi_startproc
        pushq   %rbp
        .cfi_def_cfa_offset 16
        .cfi_offset 6, -16
        movq    %rsp, %rbp
        .cfi_def_cfa_register 6
        leaq    .LC0(%rip), %rdi
        call    puts@PLT
        movl    $0, %eax
        popq    %rbp
        .cfi_def_cfa 7, 8
        ret
        .cfi_endproc
.LFE0:
        .size   main, .-main
        .ident  "GCC: (Debian 10.2.1-6) 10.2.1 20210110"
        .section        .note.GNU-stack,"",@progbits
```

- gcc -S -masm=intel my_XXX.c -o my_XXX.s (Intel Syntax，注意:"S"一定要是大寫，小寫會錯掉)
```
cat test_intel_1.s                     1 ⨯
        .file   "test.c"
        .intel_syntax noprefix
        .text
        .section        .rodata
.LC0:
        .string "Hello CTF"
        .text
        .globl  main
        .type   main, @function
main:
.LFB0:
        .cfi_startproc
        push    rbp
        .cfi_def_cfa_offset 16
        .cfi_offset 6, -16
        mov     rbp, rsp
        .cfi_def_cfa_register 6
        lea     rdi, .LC0[rip]
        call    puts@PLT
        mov     eax, 0
        pop     rbp
        .cfi_def_cfa 7, 8
        ret
        .cfi_endproc
.LFE0:
        .size   main, .-main
        .ident  "GCC: (Debian 10.2.1-6) 10.2.1 20210110"
        .section        .note.GNU-stack,"",@progbits
```

- gcc -S -masm=intel my_XXX.c -o my_XXX.s  -fno-asynchronous-unwind-tables (輸出比起上面Intel指令更有人性化)
```
cat test_intel_2.s
        .file   "test.c"
        .intel_syntax noprefix
        .text
        .section        .rodata
.LC0:
        .string "Hello CTF"
        .text
        .globl  main
        .type   main, @function
main:
        push    rbp
        mov     rbp, rsp
        lea     rdi, .LC0[rip]
        call    puts@PLT
        mov     eax, 0
        pop     rbp
        ret
        .size   main, .-main
        .ident  "GCC: (Debian 10.2.1-6) 10.2.1 20210110"
        .section        .note.GNU-stack,"",@progbits

```

## 4_Reversing using objdump

```
objdump逆向成組語  執行檔==> 組合語言

objdump -S -M intel objfile | less

objdump -S -M intel objfile | grep  XXXX

objdump -S -j .text -M intel helloCTFer

objdump -S -j .text -M intel helloCTFer --no-show-raw-insn  
```

### Kali 實作
用 objdump 把 test 轉回 test.s (Intel Version)
- objdump -S -M intel objfile (看完整的section) [可以跟原本的test.s比較一下]
```
objdump -S -M intel test   

test:     file format elf64-x86-64


Disassembly of section .init:

0000000000001000 <_init>:
    1000:       48 83 ec 08             sub    rsp,0x8
    1004:       48 8b 05 dd 2f 00 00    mov    rax,QWORD PTR [rip+0x2fdd]        # 3fe8 <__gmon_start__>
    100b:       48 85 c0                test   rax,rax
    100e:       74 02                   je     1012 <_init+0x12>
    1010:       ff d0                   call   rax
    1012:       48 83 c4 08             add    rsp,0x8
    1016:       c3                      ret    

Disassembly of section .plt:

0000000000001020 <.plt>:
    1020:       ff 35 e2 2f 00 00       push   QWORD PTR [rip+0x2fe2]        # 4008 <_GLOBAL_OFFSET_TABLE_+0x8>
    1026:       ff 25 e4 2f 00 00       jmp    QWORD PTR [rip+0x2fe4]        # 4010 <_GLOBAL_OFFSET_TABLE_+0x10>
    102c:       0f 1f 40 00             nop    DWORD PTR [rax+0x0]

0000000000001030 <puts@plt>:
    1030:       ff 25 e2 2f 00 00       jmp    QWORD PTR [rip+0x2fe2]        # 4018 <puts@GLIBC_2.2.5>
    1036:       68 00 00 00 00          push   0x0
    103b:       e9 e0 ff ff ff          jmp    1020 <.plt>

Disassembly of section .plt.got:

0000000000001040 <__cxa_finalize@plt>:
    1040:       ff 25 b2 2f 00 00       jmp    QWORD PTR [rip+0x2fb2]        # 3ff8 <__cxa_finalize@GLIBC_2.2.5>
    1046:       66 90                   xchg   ax,ax

Disassembly of section .text:

0000000000001050 <_start>:
    1050:       31 ed                   xor    ebp,ebp
    1052:       49 89 d1                mov    r9,rdx
    1055:       5e                      pop    rsi
    1056:       48 89 e2                mov    rdx,rsp
    1059:       48 83 e4 f0             and    rsp,0xfffffffffffffff0
    105d:       50                      push   rax
    105e:       54                      push   rsp
    105f:       4c 8d 05 4a 01 00 00    lea    r8,[rip+0x14a]        # 11b0 <__libc_csu_fini>
    1066:       48 8d 0d e3 00 00 00    lea    rcx,[rip+0xe3]        # 1150 <__libc_csu_init>
    106d:       48 8d 3d c1 00 00 00    lea    rdi,[rip+0xc1]        # 1135 <main>
    1074:       ff 15 66 2f 00 00       call   QWORD PTR [rip+0x2f66]        # 3fe0 <__libc_start_main@GLIBC_2.2.5>
    107a:       f4                      hlt    
    107b:       0f 1f 44 00 00          nop    DWORD PTR [rax+rax*1+0x0]

0000000000001080 <deregister_tm_clones>:
    1080:       48 8d 3d a9 2f 00 00    lea    rdi,[rip+0x2fa9]        # 4030 <__TMC_END__>
    1087:       48 8d 05 a2 2f 00 00    lea    rax,[rip+0x2fa2]        # 4030 <__TMC_END__>
    108e:       48 39 f8                cmp    rax,rdi
    1091:       74 15                   je     10a8 <deregister_tm_clones+0x28>
    1093:       48 8b 05 3e 2f 00 00    mov    rax,QWORD PTR [rip+0x2f3e]        # 3fd8 <_ITM_deregisterTMCloneTable>
    109a:       48 85 c0                test   rax,rax
    109d:       74 09                   je     10a8 <deregister_tm_clones+0x28>
    109f:       ff e0                   jmp    rax
    10a1:       0f 1f 80 00 00 00 00    nop    DWORD PTR [rax+0x0]
    10a8:       c3                      ret    
    10a9:       0f 1f 80 00 00 00 00    nop    DWORD PTR [rax+0x0]

00000000000010b0 <register_tm_clones>:
    10b0:       48 8d 3d 79 2f 00 00    lea    rdi,[rip+0x2f79]        # 4030 <__TMC_END__>
    10b7:       48 8d 35 72 2f 00 00    lea    rsi,[rip+0x2f72]        # 4030 <__TMC_END__>
    10be:       48 29 fe                sub    rsi,rdi
    10c1:       48 89 f0                mov    rax,rsi
    10c4:       48 c1 ee 3f             shr    rsi,0x3f
    10c8:       48 c1 f8 03             sar    rax,0x3
    10cc:       48 01 c6                add    rsi,rax
    10cf:       48 d1 fe                sar    rsi,1
    10d2:       74 14                   je     10e8 <register_tm_clones+0x38>
    10d4:       48 8b 05 15 2f 00 00    mov    rax,QWORD PTR [rip+0x2f15]        # 3ff0 <_ITM_registerTMCloneTable>
    10db:       48 85 c0                test   rax,rax
    10de:       74 08                   je     10e8 <register_tm_clones+0x38>
    10e0:       ff e0                   jmp    rax
    10e2:       66 0f 1f 44 00 00       nop    WORD PTR [rax+rax*1+0x0]
    10e8:       c3                      ret    
    10e9:       0f 1f 80 00 00 00 00    nop    DWORD PTR [rax+0x0]

00000000000010f0 <__do_global_dtors_aux>:
    10f0:       80 3d 39 2f 00 00 00    cmp    BYTE PTR [rip+0x2f39],0x0        # 4030 <__TMC_END__>
    10f7:       75 2f                   jne    1128 <__do_global_dtors_aux+0x38>
    10f9:       55                      push   rbp
    10fa:       48 83 3d f6 2e 00 00    cmp    QWORD PTR [rip+0x2ef6],0x0        # 3ff8 <__cxa_finalize@GLIBC_2.2.5>
    1101:       00 
    1102:       48 89 e5                mov    rbp,rsp
    1105:       74 0c                   je     1113 <__do_global_dtors_aux+0x23>
    1107:       48 8b 3d 1a 2f 00 00    mov    rdi,QWORD PTR [rip+0x2f1a]        # 4028 <__dso_handle>
    110e:       e8 2d ff ff ff          call   1040 <__cxa_finalize@plt>
    1113:       e8 68 ff ff ff          call   1080 <deregister_tm_clones>
    1118:       c6 05 11 2f 00 00 01    mov    BYTE PTR [rip+0x2f11],0x1        # 4030 <__TMC_END__>
    111f:       5d                      pop    rbp
    1120:       c3                      ret    
    1121:       0f 1f 80 00 00 00 00    nop    DWORD PTR [rax+0x0]
    1128:       c3                      ret    
    1129:       0f 1f 80 00 00 00 00    nop    DWORD PTR [rax+0x0]

0000000000001130 <frame_dummy>:
    1130:       e9 7b ff ff ff          jmp    10b0 <register_tm_clones>

0000000000001135 <main>:
    1135:       55                      push   rbp
    1136:       48 89 e5                mov    rbp,rsp
    1139:       48 8d 3d c4 0e 00 00    lea    rdi,[rip+0xec4]        # 2004 <_IO_stdin_used+0x4>
    1140:       e8 eb fe ff ff          call   1030 <puts@plt>
    1145:       b8 00 00 00 00          mov    eax,0x0
    114a:       5d                      pop    rbp
    114b:       c3                      ret    
    114c:       0f 1f 40 00             nop    DWORD PTR [rax+0x0]

0000000000001150 <__libc_csu_init>:
    1150:       41 57                   push   r15
    1152:       4c 8d 3d 8f 2c 00 00    lea    r15,[rip+0x2c8f]        # 3de8 <__frame_dummy_init_array_entry>
    1159:       41 56                   push   r14
    115b:       49 89 d6                mov    r14,rdx
    115e:       41 55                   push   r13
    1160:       49 89 f5                mov    r13,rsi
    1163:       41 54                   push   r12
    1165:       41 89 fc                mov    r12d,edi
    1168:       55                      push   rbp
    1169:       48 8d 2d 80 2c 00 00    lea    rbp,[rip+0x2c80]        # 3df0 <__do_global_dtors_aux_fini_array_entry>
    1170:       53                      push   rbx
    1171:       4c 29 fd                sub    rbp,r15
    1174:       48 83 ec 08             sub    rsp,0x8
    1178:       e8 83 fe ff ff          call   1000 <_init>
    117d:       48 c1 fd 03             sar    rbp,0x3
    1181:       74 1b                   je     119e <__libc_csu_init+0x4e>
    1183:       31 db                   xor    ebx,ebx
    1185:       0f 1f 00                nop    DWORD PTR [rax]
    1188:       4c 89 f2                mov    rdx,r14
    118b:       4c 89 ee                mov    rsi,r13
    118e:       44 89 e7                mov    edi,r12d
    1191:       41 ff 14 df             call   QWORD PTR [r15+rbx*8]
    1195:       48 83 c3 01             add    rbx,0x1
    1199:       48 39 dd                cmp    rbp,rbx
    119c:       75 ea                   jne    1188 <__libc_csu_init+0x38>
    119e:       48 83 c4 08             add    rsp,0x8
    11a2:       5b                      pop    rbx
    11a3:       5d                      pop    rbp
    11a4:       41 5c                   pop    r12
    11a6:       41 5d                   pop    r13
    11a8:       41 5e                   pop    r14
    11aa:       41 5f                   pop    r15
    11ac:       c3                      ret    
    11ad:       0f 1f 00                nop    DWORD PTR [rax]

00000000000011b0 <__libc_csu_fini>:
    11b0:       c3                      ret    

Disassembly of section .fini:

00000000000011b4 <_fini>:
    11b4:       48 83 ec 08             sub    rsp,0x8
    11b8:       48 83 c4 08             add    rsp,0x8
    11bc:       c3                      ret    
```

[原test.s]
```
cat test.s        
        .file   "test.c"
        .text
        .section        .rodata
.LC0:
        .string "Hello CTF"
        .text
        .globl  main
        .type   main, @function
main:
.LFB0:
        .cfi_startproc
        pushq   %rbp
        .cfi_def_cfa_offset 16
        .cfi_offset 6, -16
        movq    %rsp, %rbp
        .cfi_def_cfa_register 6
        leaq    .LC0(%rip), %rdi
        call    puts@PLT
        movl    $0, %eax
        popq    %rbp
        .cfi_def_cfa 7, 8
        ret
        .cfi_endproc
.LFE0:
        .size   main, .-main
        .ident  "GCC: (Debian 10.2.1-6) 10.2.1 20210110"
        .section        .note.GNU-stack,"",@progbits

```


- objdump -S -j .text -M intel helloCTFer (只看某個section)
```
objdump -S -j .text -M intel test      

test:     file format elf64-x86-64


Disassembly of section .text:

0000000000001050 <_start>:
    1050:       31 ed                   xor    ebp,ebp
    1052:       49 89 d1                mov    r9,rdx
    1055:       5e                      pop    rsi
    1056:       48 89 e2                mov    rdx,rsp
    1059:       48 83 e4 f0             and    rsp,0xfffffffffffffff0
    105d:       50                      push   rax
    105e:       54                      push   rsp
    105f:       4c 8d 05 4a 01 00 00    lea    r8,[rip+0x14a]        # 11b0 <__libc_csu_fini>
    1066:       48 8d 0d e3 00 00 00    lea    rcx,[rip+0xe3]        # 1150 <__libc_csu_init>
    106d:       48 8d 3d c1 00 00 00    lea    rdi,[rip+0xc1]        # 1135 <main>
    1074:       ff 15 66 2f 00 00       call   QWORD PTR [rip+0x2f66]        # 3fe0 <__libc_start_main@GLIBC_2.2.5>
    107a:       f4                      hlt    
    107b:       0f 1f 44 00 00          nop    DWORD PTR [rax+rax*1+0x0]

0000000000001080 <deregister_tm_clones>:
    1080:       48 8d 3d a9 2f 00 00    lea    rdi,[rip+0x2fa9]        # 4030 <__TMC_END__>
    1087:       48 8d 05 a2 2f 00 00    lea    rax,[rip+0x2fa2]        # 4030 <__TMC_END__>
    108e:       48 39 f8                cmp    rax,rdi
    1091:       74 15                   je     10a8 <deregister_tm_clones+0x28>
    1093:       48 8b 05 3e 2f 00 00    mov    rax,QWORD PTR [rip+0x2f3e]        # 3fd8 <_ITM_deregisterTMCloneTable>
    109a:       48 85 c0                test   rax,rax
    109d:       74 09                   je     10a8 <deregister_tm_clones+0x28>
    109f:       ff e0                   jmp    rax
    10a1:       0f 1f 80 00 00 00 00    nop    DWORD PTR [rax+0x0]
    10a8:       c3                      ret    
    10a9:       0f 1f 80 00 00 00 00    nop    DWORD PTR [rax+0x0]

00000000000010b0 <register_tm_clones>:
    10b0:       48 8d 3d 79 2f 00 00    lea    rdi,[rip+0x2f79]        # 4030 <__TMC_END__>
    10b7:       48 8d 35 72 2f 00 00    lea    rsi,[rip+0x2f72]        # 4030 <__TMC_END__>
    10be:       48 29 fe                sub    rsi,rdi
    10c1:       48 89 f0                mov    rax,rsi
    10c4:       48 c1 ee 3f             shr    rsi,0x3f
    10c8:       48 c1 f8 03             sar    rax,0x3
    10cc:       48 01 c6                add    rsi,rax
    10cf:       48 d1 fe                sar    rsi,1
    10d2:       74 14                   je     10e8 <register_tm_clones+0x38>
    10d4:       48 8b 05 15 2f 00 00    mov    rax,QWORD PTR [rip+0x2f15]        # 3ff0 <_ITM_registerTMCloneTable>
    10db:       48 85 c0                test   rax,rax
    10de:       74 08                   je     10e8 <register_tm_clones+0x38>
    10e0:       ff e0                   jmp    rax
    10e2:       66 0f 1f 44 00 00       nop    WORD PTR [rax+rax*1+0x0]
    10e8:       c3                      ret    
    10e9:       0f 1f 80 00 00 00 00    nop    DWORD PTR [rax+0x0]

00000000000010f0 <__do_global_dtors_aux>:
    10f0:       80 3d 39 2f 00 00 00    cmp    BYTE PTR [rip+0x2f39],0x0        # 4030 <__TMC_END__>
    10f7:       75 2f                   jne    1128 <__do_global_dtors_aux+0x38>
    10f9:       55                      push   rbp
    10fa:       48 83 3d f6 2e 00 00    cmp    QWORD PTR [rip+0x2ef6],0x0        # 3ff8 <__cxa_finalize@GLIBC_2.2.5>
    1101:       00 
    1102:       48 89 e5                mov    rbp,rsp
    1105:       74 0c                   je     1113 <__do_global_dtors_aux+0x23>
    1107:       48 8b 3d 1a 2f 00 00    mov    rdi,QWORD PTR [rip+0x2f1a]        # 4028 <__dso_handle>
    110e:       e8 2d ff ff ff          call   1040 <__cxa_finalize@plt>
    1113:       e8 68 ff ff ff          call   1080 <deregister_tm_clones>
    1118:       c6 05 11 2f 00 00 01    mov    BYTE PTR [rip+0x2f11],0x1        # 4030 <__TMC_END__>
    111f:       5d                      pop    rbp
    1120:       c3                      ret    
    1121:       0f 1f 80 00 00 00 00    nop    DWORD PTR [rax+0x0]
    1128:       c3                      ret    
    1129:       0f 1f 80 00 00 00 00    nop    DWORD PTR [rax+0x0]

0000000000001130 <frame_dummy>:
    1130:       e9 7b ff ff ff          jmp    10b0 <register_tm_clones>

0000000000001135 <main>:
    1135:       55                      push   rbp
    1136:       48 89 e5                mov    rbp,rsp
    1139:       48 8d 3d c4 0e 00 00    lea    rdi,[rip+0xec4]        # 2004 <_IO_stdin_used+0x4>
    1140:       e8 eb fe ff ff          call   1030 <puts@plt>
    1145:       b8 00 00 00 00          mov    eax,0x0
    114a:       5d                      pop    rbp
    114b:       c3                      ret    
    114c:       0f 1f 40 00             nop    DWORD PTR [rax+0x0]

0000000000001150 <__libc_csu_init>:
    1150:       41 57                   push   r15
    1152:       4c 8d 3d 8f 2c 00 00    lea    r15,[rip+0x2c8f]        # 3de8 <__frame_dummy_init_array_entry>
    1159:       41 56                   push   r14
    115b:       49 89 d6                mov    r14,rdx
    115e:       41 55                   push   r13
    1160:       49 89 f5                mov    r13,rsi
    1163:       41 54                   push   r12
    1165:       41 89 fc                mov    r12d,edi
    1168:       55                      push   rbp
    1169:       48 8d 2d 80 2c 00 00    lea    rbp,[rip+0x2c80]        # 3df0 <__do_global_dtors_aux_fini_array_entry>
    1170:       53                      push   rbx
    1171:       4c 29 fd                sub    rbp,r15
    1174:       48 83 ec 08             sub    rsp,0x8
    1178:       e8 83 fe ff ff          call   1000 <_init>
    117d:       48 c1 fd 03             sar    rbp,0x3
    1181:       74 1b                   je     119e <__libc_csu_init+0x4e>
    1183:       31 db                   xor    ebx,ebx
    1185:       0f 1f 00                nop    DWORD PTR [rax]
    1188:       4c 89 f2                mov    rdx,r14
    118b:       4c 89 ee                mov    rsi,r13
    118e:       44 89 e7                mov    edi,r12d
    1191:       41 ff 14 df             call   QWORD PTR [r15+rbx*8]
    1195:       48 83 c3 01             add    rbx,0x1
    1199:       48 39 dd                cmp    rbp,rbx
    119c:       75 ea                   jne    1188 <__libc_csu_init+0x38>
    119e:       48 83 c4 08             add    rsp,0x8
    11a2:       5b                      pop    rbx
    11a3:       5d                      pop    rbp
    11a4:       41 5c                   pop    r12
    11a6:       41 5d                   pop    r13
    11a8:       41 5e                   pop    r14
    11aa:       41 5f                   pop    r15
    11ac:       c3                      ret    
    11ad:       0f 1f 00                nop    DWORD PTR [rax]

00000000000011b0 <__libc_csu_fini>:
    11b0:       c3                      ret    
```

- objdump -S -j .text -M intel helloCTFer --no-show-raw-insn (不要秀機器碼)
```
objdump -S -j .text -M intel test --no-show-raw-insn  

test:     file format elf64-x86-64


Disassembly of section .text:

0000000000001050 <_start>:
    1050:       xor    ebp,ebp
    1052:       mov    r9,rdx
    1055:       pop    rsi
    1056:       mov    rdx,rsp
    1059:       and    rsp,0xfffffffffffffff0
    105d:       push   rax
    105e:       push   rsp
    105f:       lea    r8,[rip+0x14a]        # 11b0 <__libc_csu_fini>
    1066:       lea    rcx,[rip+0xe3]        # 1150 <__libc_csu_init>
    106d:       lea    rdi,[rip+0xc1]        # 1135 <main>
    1074:       call   QWORD PTR [rip+0x2f66]        # 3fe0 <__libc_start_main@GLIBC_2.2.5>
    107a:       hlt    
    107b:       nop    DWORD PTR [rax+rax*1+0x0]

0000000000001080 <deregister_tm_clones>:
    1080:       lea    rdi,[rip+0x2fa9]        # 4030 <__TMC_END__>
    1087:       lea    rax,[rip+0x2fa2]        # 4030 <__TMC_END__>
    108e:       cmp    rax,rdi
    1091:       je     10a8 <deregister_tm_clones+0x28>
    1093:       mov    rax,QWORD PTR [rip+0x2f3e]        # 3fd8 <_ITM_deregisterTMCloneTable>
    109a:       test   rax,rax
    109d:       je     10a8 <deregister_tm_clones+0x28>
    109f:       jmp    rax
    10a1:       nop    DWORD PTR [rax+0x0]
    10a8:       ret    
    10a9:       nop    DWORD PTR [rax+0x0]

00000000000010b0 <register_tm_clones>:
    10b0:       lea    rdi,[rip+0x2f79]        # 4030 <__TMC_END__>
    10b7:       lea    rsi,[rip+0x2f72]        # 4030 <__TMC_END__>
    10be:       sub    rsi,rdi
    10c1:       mov    rax,rsi
    10c4:       shr    rsi,0x3f
    10c8:       sar    rax,0x3
    10cc:       add    rsi,rax
    10cf:       sar    rsi,1
    10d2:       je     10e8 <register_tm_clones+0x38>
    10d4:       mov    rax,QWORD PTR [rip+0x2f15]        # 3ff0 <_ITM_registerTMCloneTable>
    10db:       test   rax,rax
    10de:       je     10e8 <register_tm_clones+0x38>
    10e0:       jmp    rax
    10e2:       nop    WORD PTR [rax+rax*1+0x0]
    10e8:       ret    
    10e9:       nop    DWORD PTR [rax+0x0]

00000000000010f0 <__do_global_dtors_aux>:
    10f0:       cmp    BYTE PTR [rip+0x2f39],0x0        # 4030 <__TMC_END__>
    10f7:       jne    1128 <__do_global_dtors_aux+0x38>
    10f9:       push   rbp
    10fa:       cmp    QWORD PTR [rip+0x2ef6],0x0        # 3ff8 <__cxa_finalize@GLIBC_2.2.5>
    1102:       mov    rbp,rsp
    1105:       je     1113 <__do_global_dtors_aux+0x23>
    1107:       mov    rdi,QWORD PTR [rip+0x2f1a]        # 4028 <__dso_handle>
    110e:       call   1040 <__cxa_finalize@plt>
    1113:       call   1080 <deregister_tm_clones>
    1118:       mov    BYTE PTR [rip+0x2f11],0x1        # 4030 <__TMC_END__>
    111f:       pop    rbp
    1120:       ret    
    1121:       nop    DWORD PTR [rax+0x0]
    1128:       ret    
    1129:       nop    DWORD PTR [rax+0x0]

0000000000001130 <frame_dummy>:
    1130:       jmp    10b0 <register_tm_clones>

0000000000001135 <main>:
    1135:       push   rbp
    1136:       mov    rbp,rsp
    1139:       lea    rdi,[rip+0xec4]        # 2004 <_IO_stdin_used+0x4>
    1140:       call   1030 <puts@plt>
    1145:       mov    eax,0x0
    114a:       pop    rbp
    114b:       ret    
    114c:       nop    DWORD PTR [rax+0x0]

0000000000001150 <__libc_csu_init>:
    1150:       push   r15
    1152:       lea    r15,[rip+0x2c8f]        # 3de8 <__frame_dummy_init_array_entry>
    1159:       push   r14
    115b:       mov    r14,rdx
    115e:       push   r13
    1160:       mov    r13,rsi
    1163:       push   r12
    1165:       mov    r12d,edi
    1168:       push   rbp
    1169:       lea    rbp,[rip+0x2c80]        # 3df0 <__do_global_dtors_aux_fini_array_entry>
    1170:       push   rbx
    1171:       sub    rbp,r15
    1174:       sub    rsp,0x8
    1178:       call   1000 <_init>
    117d:       sar    rbp,0x3
    1181:       je     119e <__libc_csu_init+0x4e>
    1183:       xor    ebx,ebx
    1185:       nop    DWORD PTR [rax]
    1188:       mov    rdx,r14
    118b:       mov    rsi,r13
    118e:       mov    edi,r12d
    1191:       call   QWORD PTR [r15+rbx*8]
    1195:       add    rbx,0x1
    1199:       cmp    rbp,rbx
    119c:       jne    1188 <__libc_csu_init+0x38>
    119e:       add    rsp,0x8
    11a2:       pop    rbx
    11a3:       pop    rbp
    11a4:       pop    r12
    11a6:       pop    r13
    11a8:       pop    r14
    11aa:       pop    r15
    11ac:       ret    
    11ad:       nop    DWORD PTR [rax]

00000000000011b0 <__libc_csu_fini>:
    11b0:       ret    
```

## `[延伸閱讀|read-around|Further reading|Weiterlesen|Lectures complémentaires]`

- Advanced C and C++ Compiling by Milan Stevanovic (Apress, 2014).
  - [Github](https://github.com/Apress/adv-c-cpp-compiling)
  - Chapter 2: Simple Program Lifetime Stages 
  - 導讀: 使用底下程式練習compilation process的流程 

function.h
```c
#pragma once
#define FIRST_OPTION
#ifdef FIRST_OPTION
#define MULTIPLIER (3.0)
#else
#define MULTIPLIER (2.0)
#endif
float add_and_multiply(float x, float y);
```

function.c
```c
int nCompletionStatus = 0;

float add(float x, float y)
{
   float z = x + y;
   return z;
}

float add_and_multiply(float x, float y)
{
   float z = add(x,y);
   z *= MULTIPLIER;
   return z;
}
```

main.c
```c
#include "function.h"

extern int nCompletionStatus = 0;

int main(int argc, char* argv[])
{
   float x = 1.0;
   float y = 5.0;
   float z;
   z = add_and_multiply(x,y);
   nCompletionStatus = 1;
   return 0;
}
```


