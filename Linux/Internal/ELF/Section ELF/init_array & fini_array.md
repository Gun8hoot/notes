- Cette section est un tableau qui va contenir l'addresse des fonction **constructor** et **destructor** qui seront lancer avant ou apres le `main()`
```c
#include <stdio.h>

void before_main(void) __attribute__((constructor));
void after_main(void) __attribute__((destructor));

void before_main(void)
{
	printf("before the main\n");
}

void after_main(void)
{
	printf("after the main\n");
}

int main (void)
{
	printf("Is the main\n");
}
```
- Lorsque l'on utilise un deassembler sur notre binaire il nous dit que la fonction before_main() est a l'addresse 0x1139 et after_main() est a l'addresse 0x114F
```
0000000000001139 <before_main>:
    1139:	55                   	push   rbp
    113a:	48 89 e5             	mov    rbp,rsp
    113d:	48 8d 05 c0 0e 00 00 	lea    rax,[rip+0xec0]        # 2004 <_IO_stdin_used+0x4>
    1144:	48 89 c7             	mov    rdi,rax
    1147:	e8 e4 fe ff ff       	call   1030 <puts@plt>
    114c:	90                   	nop
    114d:	5d                   	pop    rbp
    114e:	c3                   	ret

000000000000114f <after_main>:
    114f:	55                   	push   rbp
    1150:	48 89 e5             	mov    rbp,rsp
    1153:	48 8d 05 ba 0e 00 00 	lea    rax,[rip+0xeba]        # 2014 <_IO_stdin_used+0x14>
    115a:	48 89 c7             	mov    rdi,rax
    115d:	e8 ce fe ff ff       	call   1030 <puts@plt>
    1162:	90                   	nop
    1163:	5d                   	pop    rbp
    1164:	c3                   	ret
```
- Si on l'on regarde la section .init_array et .fini_array, nous pouvons remarquer que les deux addresse eu avec objdump sont bien presente dans notre **.init_array** et **.fini_array**
```
Hex dump of section '.init_array':
  0x00003dc0 30110000 00000000 >>3911<<0000 00000000 0.......9.......

Hex dump of section '.fini_array':
  0x00003dd0 e0100000 00000000 >>4f11<<0000 00000000 ........O.......
```
- A noter que `.init_array` et `.fini_array` sont de type **INIT/FINI_ARRAY** ce qui ne leurs permette pas de contenir du code comme les sections `.init` ou `.fini`  qui eux sont de type **PROGBITS**