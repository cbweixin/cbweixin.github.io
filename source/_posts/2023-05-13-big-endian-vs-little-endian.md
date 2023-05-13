title: big-endian-vs-little-endian
date: 2023-05-13 14:21:06
categories: programming
tags:
- c/c++
- network
---

# big endian, little endian and network byte order
Big-endian and little-endian are two byte order formats that define the order of bytes in a multi-byte data type, such as a word, double word, or longword. 
 
In a big-endian system, the most significant byte (MSB) of a multi-byte value is stored first, while the least significant byte (LSB) is stored last. On the other hand, in a little-endian system, the LSB is stored first and the MSB is stored last. 
 
check the graph below: ![image](https://github-production-user-asset-6210df.s3.amazonaws.com/1326906/238157312-8a060263-e124-4a53-a0ac-3d90d1ee7c5a.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20230513%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20230513T213325Z&X-Amz-Expires=300&X-Amz-Signature=7c6175beb25ec90cb06143fce050d8bade744a7e4ddb57f442fb360ce173ae9b&X-Amz-SignedHeaders=host&actor_id=1326906&key_id=0&repo_id=109354703)

Network byte order refers to the standard byte order used in networking protocols. It is also known as big-endian byte order, where the most significant byte (MSB) of a multi-byte value is stored first, while the least significant byte (LSB) is stored last. This order is used for transmitting data across different computer architectures and operating systems, ensuring that the data is interpreted correctly by the receiving system. 
 
we can use some c function to show the differences. 

- `htons`, `h` stands for host, `n` stands for network, `s` stands for short
- `htonl`, same above, `l` means long here. 
those 2 functions try to convert short/long type data from host byte order to network byte order
we also have
- ntohs
- ntohl
those do the reverse. 

here is an example code:
```c
#include <netinet/in.h>
#include <stdio.h>

int main(int argc, char *argv[]){
    unsigned short host_port = 0x1234;
    unsigned short net_port;
    unsigned long host_addr=0x12345678;
    unsigned long net_addr;

    net_port=htons(host_port);
    net_addr= htonl(host_addr);

    printf("Host ordered port : %#x \n", host_port);
    printf("[0]:0x%x\n", *((char *)&host_port + 0));
    printf("[1]:0x%x\n", *((char *)&host_port + 1));

    printf("network ordered port : %#x \n", net_port);
    printf("[0]:0x%x\n", *((char *)&net_port + 0));
    printf("[1]:0x%x\n", *((char *)&net_port + 1));

    printf("Host ordered addr : %#lx \n", host_addr);
    printf("[0]:0x%x\n", *((char *)&host_addr + 0));
    printf("[1]:0x%x\n", *((char *)&host_addr + 1));
    printf("[2]:0x%x\n", *((char *)&host_addr + 2));
    printf("[3]:0x%x\n", *((char *)&host_addr + 3));


    printf("network ordered addr : %#lx \n", net_addr);
    printf("[0]:0x%x\n", *((char *)&net_addr + 0));
    printf("[1]:0x%x\n", *((char *)&net_addr + 1));
    printf("[2]:0x%x\n", *((char *)&net_addr + 2));
    printf("[3]:0x%x\n", *((char *)&net_addr + 3));
}
```
output:
```shell
gabriel@gabriel-HP-ProDesk-600-G2-SFF:~/weixin-github/tcpip-networking/hello-server$ ./conv 
Host ordered port : 0x1234 
[0]:0x34
[1]:0x12
network ordered port : 0x3412 
[0]:0x12
[1]:0x34
Host ordered addr : 0x12345678 
[0]:0x78
[1]:0x56
[2]:0x34
[3]:0x12
network ordered addr : 0x78563412 
[0]:0x12
[1]:0x34
[2]:0x56
[3]:0x78
```



