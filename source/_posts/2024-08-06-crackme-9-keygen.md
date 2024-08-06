title: crackme-9-keygen
date: 2024-08-06 09:03:08
categories: programming
tags: asm
---

# keygen for crackme 9
record how I wrote a keygen for crackme 9th question.

## crackme 9 UI
looks as such
![image1](/images/crackme9_1.png)

input user name and password, then click on `check`. 

## decompile exe file and check source
here is the decompiled code ![image2](/images/crackme9_2.png)
at memory address `004013121` , there is a `je` jump, so the brute force way is to change it to `jnz` or `nop`, then it would be cracked successfully. that's easy. but the question is we still don't know how the key got calcualted, ie, how string1 `Bon-xx-xx-xx` generated. that's our target.

### username processing
see logic as such: ![username process](/images/crackme9_3.png)
```asm
00401208  /$  68 F8DC4000   push keygenme.0040DCF8                   ; /String = ""
0040120D  |.  E8 80010000   call <jmp.&kernel32.lstrlenA>            ; \lstrlenA
00401212  |.  A3 86DC4000   mov dword ptr ds:[0x40DC86],eax
00401217  |.  833D 86DC4000>cmp dword ptr ds:[0x40DC86],0x4
0040121E  |.  0F8C 29010000 jl keygenme.0040134D
00401224  |.  833D 86DC4000>cmp dword ptr ds:[0x40DC86],0x32
0040122B  |.  0F8F 1C010000 jg keygenme.0040134D

```
`keygenme.0040DCF8` has the username , push it to stack, then call `strnlen` to check lenght. 
pseudo code as such:
```python
s = username
l = len(s)
if (l < 4 ) {}
if (l > 32) {}
```
then 
```asm
00401231  |.  33C0          xor eax,eax
00401233  |.  33DB          xor ebx,ebx
00401235  |.  33C9          xor ecx,ecx                              ;  keygenme.<ModuleEntryPoint>
00401237  |.  BF F8DC4000   mov edi,keygenme.0040DCF8
0040123C  |.  8B15 86DC4000 mov edx,dword ptr ds:[0x40DC86]          ;  put username length to edx
```
1,2,3 clear all registers, then put username to edi , put lenght to edx , ebx is 0 right now
continue
```asm
00401242  |> /0FB60439      /movzx eax,byte ptr ds:[ecx+edi]         ;  loop every char of the username
00401246  |. |83E8 19       |sub eax,0x19                            ;  minus 19 from the char
00401249  |. |2BD8          |sub ebx,eax
0040124B  |. |41            |inc ecx                                 ;  i++
0040124C  |. |3BCA          |cmp ecx,edx                             ;  if i == username.length
0040124E  |.^\75 F2         \jnz short keygenme.00401242
```
pesudo code
```python
ebx = 0
for c in username:
    eax = c
    eax -= 0x19
    ebx -= c
```
then
```asm
00401250  |.  53            push ebx                                 ; /<%lX> = 0x3E0000
00401251  |.  68 F8DB4000   push keygenme.0040DBF8                   ; |%lX
00401256  |.  68 F8E04000   push keygenme.0040E0F8                   ; |s = keygenme.0040E0F8
0040125B  |.  E8 38010000   call <jmp.&user32.wsprintfA>             ; \wsprintfA
```
print out the hex representation of ebx
```c
s = printf("%lx", ebx)
```
formated string would be stored in keygenme.0040E0F8
then the stack looks like:
```
+-------------------------+
| 0x0040E0F8 (buffer)     | <- esp + 8
+-------------------------+
| 0x0040DBF8 (format)     | <- esp + 4
+-------------------------+
| value in ebx            | <- esp
+-------------------------+
```
then
```asm
00401260  |.  83C4 0C       add esp,0xC
00401263  |.  33C0          xor eax,eax
00401265  |.  33D2          xor edx,edx                              ;  keygenme.<ModuleEntryPoint>
00401267  |.  33C9          xor ecx,ecx                              ;  keygenme.<ModuleEntryPoint>
```
since push 3 variables to stack, 3*4 = 12, line 1 expand the stack by 12, then clear all registers.
```
00401269  |.  03C3          add eax,ebx
0040126B  |.  0FAFC3        imul eax,ebx
0040126E  |.  03C8          add ecx,eax
00401270  |.  2BD3          sub edx,ebx
00401272  |.  33D0          xor edx,eax
00401274  |.  0FAFD8        imul ebx,eax
```
code:
```asm
eax = edx = ecx = 0
eax = ebx
eax *= ebx
ecx += eax
edx -= ebx
edx |= eax
ebx *= eax
```
continue
```asm
00401277  |.  53            push ebx                                 ; /<%lX> = 0x3E0000
00401278  |.  68 F8DB4000   push keygenme.0040DBF8                   ; |%lX
0040127D  |.  68 F8E14000   push keygenme.0040E1F8                   ; |s = keygenme.0040E1F8
00401282  |.  E8 11010000   call <jmp.&user32.wsprintfA>             ; \wsprintfA
```
same, print out ebx
continue
```asm
00401287  |.  83C4 0C       add esp,0xC
0040128A  |.  33C0          xor eax,eax
0040128C  |.  33DB          xor ebx,ebx
0040128E  |.  33D2          xor edx,edx                              ;  keygenme.<ModuleEntryPoint>
00401290  |.  33C9          xor ecx,ecx                              ;  keygenme.<ModuleEntryPoint>
00401292  |.  B8 F8E04000   mov eax,keygenme.0040E0F8
00401297  |.  03D8          add ebx,eax
00401299  |.  33CB          xor ecx,ebx
0040129B  |.  0FAFCB        imul ecx,ebx
0040129E  |.  2BC8          sub ecx,eax
```
expand stack by 12, the keep calculating
code:
```python
eax = ebx = ecx = edx = 0
eax = s # see previous code
ebx += eax
ecx |= ebx
ecx *= ebx
ecx -= eax
```
then
```asm
004012A0  |.  51            push ecx                                 ; /<%lX> = 0x401000
004012A1  |.  68 F8DB4000   push keygenme.0040DBF8                   ; |%lX
004012A6  |.  68 F8E24000   push keygenme.0040E2F8                   ; |s = keygenme.0040E2F8
004012AB  |.  E8 E8000000   call <jmp.&user32.wsprintfA>             ; \wsprintfA
```
same, print out hex of ecx and store it in 0040E2F8
continue
```asm
004012B0  |.  83C4 0C       add esp,0xC
004012B3  |.  68 FCDB4000   push keygenme.0040DBFC                   ; /Bon-
004012B8  |.  68 F8DD4000   push keygenme.0040DDF8                   ; |s = keygenme.0040DDF8
004012BD  |.  E8 D6000000   call <jmp.&user32.wsprintfA>             ; \wsprintfA
004012C2  |.  83C4 08       add esp,0x8
004012C5  |.  68 F8E04000   push keygenme.0040E0F8                   ; /StringToAdd = ""
004012CA  |.  68 F8DD4000   push keygenme.0040DDF8                   ; |ConcatString = ""
004012CF  |.  E8 B2000000   call <jmp.&kernel32.lstrcatA>            ; \lstrcatA
004012D4  |.  68 01DC4000   push keygenme.0040DC01                   ; /-
004012D9  |.  68 F8DD4000   push keygenme.0040DDF8                   ; |ConcatString = ""
004012DE  |.  E8 A3000000   call <jmp.&kernel32.lstrcatA>            ; \lstrcatA
004012E3  |.  68 F8E14000   push keygenme.0040E1F8                   ; /StringToAdd = ""
004012E8  |.  68 F8DD4000   push keygenme.0040DDF8                   ; |ConcatString = ""
004012ED  |.  E8 94000000   call <jmp.&kernel32.lstrcatA>            ; \lstrcatA
004012F2  |.  68 01DC4000   push keygenme.0040DC01                   ; /-
004012F7  |.  68 F8DD4000   push keygenme.0040DDF8                   ; |ConcatString = ""
004012FC  |.  E8 85000000   call <jmp.&kernel32.lstrcatA>            ; \lstrcatA
00401301  |.  68 F8E24000   push keygenme.0040E2F8                   ; /StringToAdd = ""
00401306  |.  68 F8DD4000   push keygenme.0040DDF8                   ; |ConcatString = ""
0040130B  |.  E8 76000000   call <jmp.&kernel32.lstrcatA>            ; \lstrcatA
00401310  |.  B8 F8DD4000   mov eax,keygenme.0040DDF8
00401315  |.  BB F8DE4000   mov ebx,keygenme.0040DEF8
0040131A  |.  53            push ebx                                 ; /String2 = ""
0040131B  |.  50            push eax                                 ; |String1 = "?"
0040131C  |.  E8 6B000000   call <jmp.&kernel32.lstrcmpA>            ; \lstrcmpA
00401321  |.  74 15         je short keygenme.00401338               ;  爆破点！！！

```
then keep concating ,  keygenme.0040DDF8   is ConcatString = "", 
```python
ConcatString = ""
ConcatString += `Bon-`
ConcatString += s1 # E0F8, see previous
ConcatString += '-' # keygenme.0040DC01   is `-`
ConcatString += s2 # E1F8
ConcatString += '-'
ConcatString += s3 # E2F8
if ConcatSting == key_you_typed:
    you hacked
else:
    you failed
```
## write keygen
based on the static analysis of asm code, we can write keygen
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main( )
{
    char username[100];
    printf("Enter username: ");
    scanf("%99s", username);
    printf("you entered: %s\n", username);
    size_t len = strlen(username);
    if(len < 4)
    {
        printf("username is too short");
        return 1;
    }
    else if(len > 32)
    {
        printf("username is too long");
        return 1;
    }
    int32_t ebx = 0;
    for(size_t i = 0; i < len; i++)
    {
        int c = username[i];
        c -= 0x19;
        ebx -= c;
    }


    char buffer1[100];
    sprintf(buffer1, "%x", ebx);

    int32_t eax =0, ecx = 0;
    eax += ebx;
    eax *= ebx;
    ebx *= eax;

    char buffer2[100];
    sprintf(buffer2, "%x", ebx);

    ebx = ecx = 0;
    // hardcoded value
    eax = 0x40E0F8;
    ebx += eax;
    ecx ^= ebx;
    ecx *= ebx;
    ecx -= eax;


    char buffer3[100];
    sprintf(buffer3, "%x", ecx);

    char code[100] = "Bon-";
    strcat(code, buffer1);
    strcat(code, "-");
    strcat(code, buffer2);
    strcat(code, "-");
    strcat(code, buffer3);

    printf("keygen code: %s\n",code);
    return 0;

}
```
run it 
```shell
~/github/algorithm-practice/crackme (master ✗) ./crack9
Enter username: testabc
you entered: testabc
keygen code: Bon-fffffdc9-f5229019-41720f48
```
works perfect. 














