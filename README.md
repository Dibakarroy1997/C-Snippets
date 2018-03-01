#### This repository contains few code snippets for C language which I thing might worth knowing.

***

* **Swap 2 variables without using temporary variable**

Using '+' and '-':

```C
a = a + b;
b = a - b;
a = a - b;
```

OR

Using '\*' and '/':

```C
a = a * b;
b = a / b;
a = a / b;
```

OR

Using ^ (ex-or):

```C
a = a ^ b;
b = a ^ b;
a = a ^ b;
```

***

* **Implementation of strlen**

```C
int impStrlen (char *str) {

	// pointer to the starting address of str
	char *temp= str ;
	while (*temp++)
		// You can print value of temp to get an idea of what is happening
		//printf("%d\n", temp);
		;

	return (temp - str - 1 );
}
```

***

* **Check if string is palindrome or not**

```C
int palindromeCheck (char *str){
	char *p1 = str; // pointer pointing to starting of the string
	char *p2 = str + strlen (str) - 1; // pointer pointing to ending of the string

	while (p1 < p2){
		if (*p1++ != *p2--)
			return 0;		// not a palindrome
	}

	return 1;			// it is a palindrome
}
```

***

* **Set or reset a specific bit in a given variable**

e.g. 

8 -> 1000

we want to set 0th position so that it become 1001 (i.e. 9)

3 -> 0011

we want to reset 0th position so that it becomes 0010 (i.e. 2)

```C
#include <stdio.h>

// We can do a bitwise AND (&) or OR (|) to reset or set the bit
#define BITWISE_OP(x) (0x1 << x)

int set(int val, int pos){
    return (val |= BITWISE_OP(pos)) ;
}

int reset(int val, int pos) {
    return (val &= ~BITWISE_OP(pos)) ;
}

int main (){
  
    printf("%d\n", set(8,0));
    printf("%d\n", reset(3,0));

  return 0;
}
```

Output is:

9

2

***

* **We can use bitwise operators in many situation**

Rewriting **int c = a \* 16 + b / 2;**

```C
int c = (a << 4) + (b >> 1)
```

***

* **Complement of integer without directly using** **~**

refer to this: https://stackoverflow.com/questions/791328/how-does-the-bitwise-complement-operator-work

```C
#include <stdio.h>

void complement(int i) {
    printf( "Method 1: %d\n", i^(~0));
    printf( "Method 2: %d\n" , -(i+1)) ;
}

int main (){
    complement(5);
    return 0;
}
```

***

* **Multiline macros**

It is best practice to set a pointer to NULL after freeing it. We can use a macros for this. Below is the code for the same. 

```C
#include <stdio.h>
#include <stdlib.h>
#define freeNULL(p) {free(p); p = NULL; printf("FIN");}

int main (){
    
    int *ptr = (int *) malloc( 1 * sizeof(int *) );
    
    if(1)
        freeNULL(ptr);
    else
        printf("Nothing to do here...");
        
    return(0);
}
```

But this code gives an error message:

```
In function 'main':
12:5: error: 'else' without a previous 'if'
     else
     ^~~~
```

why so?

The reson is that after puting the macros the if statement becomes something like this:

```C
if(1){
	free(p); 
	p = NULL; 
    printf("FIN");
};
```

The semicolon at the end cause the error. The Best way to overcome this is to use a do-while(0) loop. This loop runs only once, and after using this loop the if statement becomes something like this:

```C
if(1)
do {
	free(p); 
	p = NULL; 
    printf("FIN");
}while(0);
```

So our program looks like:

```C
#include <stdio.h>
#include <stdlib.h>
#define freeNULL(p) do{free(p); p = NULL; printf("FIN");}while(0)

int main (){
    
    int *ptr = (int *) malloc( 1 * sizeof(int *) );
    
    if(1)
        freeNULL(ptr);
    else
        printf("Nothing to do here...");
        
    return(0);
}
```

***

* **Find size of a variable without using** _sizeof_ operator**



```C
#include <stdio.h>
#define mySizeof(x) (((x *) 0) + 1)

int main(){
    printf("%zd\n", mySizeof(char));
    printf("%zd\n", mySizeof(int));
    printf("%zd\n", mySizeof(float));
    printf("%zd\n", mySizeof(double));
}
```

***

* **Swap two nibbles in a byte**

before swap:

18 -> 0001 0010

after swap:

0010 0001 -> 33 

```C
#include <stdio.h>
#define swap(x) (((x & 0xF) << 4 ) | ((x&0xF0) >> 4))

int main(){
    printf("%d\n", swap(18));
}
```

***

* **Find middle node in a singly linked list**

The idea is to take two pointers pointing to head. Now increament 1st pointer by 1 and 2nd pointer by 2 in each iteration. When the 2nd pointer reach the end of the linked list then the first pointer is pointing to middle node.

```C
Node *middleNode(Node *head){
    Node *p1 = head, *p2 = head;

    while(p2 != 0){
        p1 = p1->next;
        p2 = (p2->next) ? p2->next->next : 0;
    }

    return p1;
}
```

***

* **Check if the number is power of 2**

Any number which is power of 2  and the number 1 less returns 0 when we apply a bitwise AND (&) operation.

e.g. 

8 -> 1000

7 -> 0111

and (1000 & 0111) = 0000

```C
checkIfPowerOf2(int n){
    return(!(n & (n-1)));
}
```

***

* **Delete a node in a linked list, pointer to which is given**

```C
void deleteNode(Node *node){
	// traverse the list till the end keep on copying the next node to the current node
	while(node != NULL){
		node.data = node->next.data;
		node = node->next;
	}
}
```

This way the time complexity is O(n). Another way of doing this is just copy the next node detail in the current node and free the next node. But in this way you need to make sure that a next node is present in the list. This way the time complexity is O(1).

```C
void deleteNode(Node *node){
	if(node->next != NULL){
		// copy data of next node
		node.data = node->next.data;
		// get pointer to the next node
		Node *freeNode = node->next;
		// copy pointer to the next to next node
		node->next = node->next->next;
		// free next node
		free(freeNode);
	}
}
```

***

* **How would you know whether your system is Big-endian or Little-endian**

The order in which the variable is stored in machine referred to as endian.

Sequence order -> big endian

Reverse order -> little endian

Eg. take hex number 0x0304

Machine A stores 2 bytes at a time and the sequence of store is: 0x03 followed by 0x04

Machine B stores 2 bytes at a time and the sequence of store is: 0x04 followed by 0x03

Therefore 

machine A -> big endian

machine B -> little endian

```C
#include <stdio.h>

int main(){
    
    // This is a very nice example to demostrate the use of union
    // In union all member shares same memory location
    union{
        short value;
        char store[sizeof(short)];
    } endianCheck;
    
    endianCheck.value = 0x0304;
    
    // Here endianCheck share same memory location as of value
    if(endianCheck.store[0] == 0x03 && endianCheck.store[sizeof(short) - 1] == 0x04)
        printf("big-endian system");
    else
        printf("little-endian system");

    return 0;
}
```

***

* **Find if a number is power of two**

The logic is very straight forward.

2 => 0010; 2-1 (i.e. 1) => 0001; 2 & 1 => 0010 & 0001 = 0000
4 => 0100; 4-1 (i.e. 3) => 0011; 4 & 3 => 0100 & 0011 = 0000
8 => 1000; 8-1 (i.e. 7) => 0111; 8 & 7 => 1000 & 0111 = 0000
...

```C
int isPowerOf2(int num){
	return(!(num & (num -1)));
}
```

***

* **Very interesting code. Print binary representation of any number. Let's do it recursively.**

```C
#include <stdio.h>

void decimalToBinary(int num){
	if(num == 0)
		return;
	decimalToBinary(num >> 1);
	(num & 01) ? printf("1") : printf("0");

}

int main(){
    
    decimalToBinary(965);

    return 0;
}
```

Output: 1111000101

***