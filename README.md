# C Programming notes

## Variables and constants

### Literals

A character literal is created by enclosing a single character inside single
quotation marks i.e., `'a'`, `'m'`, `'F'`.

A string literal is a sequence of characters enclosed in double-quote marks
i.e. `"hello"`, `"How are you"` etc.

### Constants

Defining constants can be achieved with following statement i.e. `constant double
PI = 3.14`

### Data types

Data types in C can be summarized by the following table,

| Type                   | Size (bytes) | Format specifier |
|------------------------|--------------|------------------|
| int                    | 4            | `%d`, `%i`       |
| char                   | 1            | `%c`             |
| float                  | 4            | `%f`             |
| double                 | 8            | `%lf`            |
| short int              | 2            | `%hd`            |
| unsigned int           | 4            | `%u`             |
| long int               | 8            | `%ld`, `%li`     |
| long long int          | 8            | `%lld`, `%lli`   |
| unsigned long int      | 8            | `%lu`            |
| unsigned long long int | 8            | `%llu`           |
| signed char            | 1            | `%c`             |
| unsigned char          | 1            | `%c`             |
| long double            | 16           | `%Lf`            |

## C input / output

### Reading input from STDIN

You can read a number from stdin by following code,

```c
int temp;
printf("Enter a integer: ");
scanf("%d", &temp);
printf("\nYou entered: %d");
```

Please note that `&temp` is used since the function stores the value at the
address of temp.

## Control flow

### Break

A `break` statement ends a loop immediately when it is encountered.

### Continue

A `continue` statement skips the current iteration of the loop and continues
with the next iteration.

## Scope / Storage classes

### Local scope

Local variables exists only inside the block in which it is declared e.g.,

```c
for (int i = 0; i <= 5; i++)
printf("%d\n", i);

printf("%d\n",i); // <----- ERROR
```

### Global scope

Variables that are declared outside of all functions are known as external or
global variables. They are accessible from any function inside the program. You
can defined them anywhere outside a function.

### Static variables

The value of a static variable persist until the end of the whole program.
Please see following example.

```c
#include <stdio.h>
void display();

int main()
{
display();
display();
}
void display()
{
static int c = 1;
c += 5;
printf("%d  ",c);
}
```

gives the output

`6 11`

## Arrays

### General

Definition of a variable that can store 100 integers: `int data[100];`

You can initialize a array during declaration according to: `int mark[5] = {1, 2,
3, 4, 5};`

Initiation of an 2D array:

```c
int c[3][3] = {
{1, 2, 3},
{4, 5, 6},
{7, 8, 9}
};
```

You don't need to  specify the array size at initiation e.g. `int ageArray[] =
{1, 4, 38, 42}:`

You can determine the size of a array with `int len = sizeof(array) /
sizeof(array[0]);`. Please note that you cannot do this if you pass the array as
an argument into a function. The reason to this is that the following code will
return the length of the pointer to the array and not the array itself.

Following code reflects how pointers work in a nutshell:

```c
int* pc, c;
c = 5;
pc = &c;
*pc = 1;
printf("%d", *pc);  // Output: 1
printf("%d", c);    // Output: 1

```

You can display addresses of variables with the following code:

```c
#include <stdio.h>

int main() {
int *ptr;
int c = 5;

ptr = &c;

printf("Value of c: %d\n",c);
printf("Value of *ptr: %d\n",*ptr);

printf("Address of c: %p\n",&c);
printf("Address of ptr: %p\n",ptr);

return 0;

}
```

Output:

```bash
Value of c: 5
Value of *ptr: 5

Address of c: 0x7ffc87cedbbc
Address of ptr: 0x7ffc87cedbbc
```

### Relationships between arrays and pointers

```c
#include <stdio.h>

int main() {
int x[5] = {1, 2, 3, 4, 5};
printf("--------------------------\n\n");
printf("Address of x = %p\n", x);
printf("Value of *x = %d\n", *x);
printf("\n\n--------------------------\n\n");
for (int i = 0; i < 5; i++) {
printf("Address of &x[%d] = %p\n", i, &x[i]);
printf("Address of (x + %d) = %p\n", i, (x + i));

printf("Value of x[%d] = %d\n", i, x[i]);
printf("Value of *(x + %d) = %d\n", i, *(x + i));
printf("\n\n--------------------------\n\n");
}

return 0;
}
```

Gives the output:

```bash
--------------------------

Address of x = 0x7ffd621d9f80
Value of *x = 1

--------------------------

Address of &x[0] = 0x7ffd621d9f80
Address of (x + 0) = 0x7ffd621d9f80

Value of x[0] = 1
Value of *(x + 0) = 1

--------------------------

Address of &x[1] = 0x7ffd621d9f84
Address of (x + 1) = 0x7ffd621d9f84

Value of x[1] = 2
Value of *(x + 1) = 2

--------------------------

Address of &x[2] = 0x7ffd621d9f88
Address of (x + 2) = 0x7ffd621d9f88

Value of x[2] = 3
Value of *(x + 2) = 3

--------------------------

Address of &x[3] = 0x7ffd621d9f8c
Address of (x + 3) = 0x7ffd621d9f8c

Value of x[3] = 4
Value of *(x + 3) = 4

--------------------------

Address of &x[4] = 0x7ffd621d9f90
Address of (x + 4) = 0x7ffd621d9f90

Value of x[4] = 5
Value of *(x + 4) = 5

--------------------------
```

There is a difference of 4 bytes between two consecutive elements of array x.
It is because the size of int is 4 bytes.

Notice that, the address of &x[0] and x is the same. It's because the variable
name x points to the first element of the array.

Lets look at another example

```c
#include <stdio.h>
int main() {
int x[5] = {1, 2, 3, 4, 5};
int* ptr;

// ptr is assigned the address of the third element
ptr = &x[2];

printf("*ptr = %d \n", *ptr);   // 3
printf("*(ptr+1) = %d \n", *(ptr+1)); // 4
printf("*(ptr-1) = %d", *(ptr-1));  // 2

return 0;
}
```

Gives the output:

```bash
*ptr = 3
*(ptr+1) = 4
*(ptr-1) = 2
```

## Pointers

### Introduction

- A pointer is a variable that stores the address of a memory location.

### Pointers: Array vs. pointer representation

You can access pointer values according to a pointer style as well according to
an array style. Please see example below.

```c
char *names[] = {"Miller", "Jones", "Anderson"};
printf("%c\n", *(*(names + 1) + 2)); // n
printf("%c\n", names[1][2]);         // n
```

### Displaying pointer values or addresses in general

| Specifier | Meaning                                                       |
|-----------|---------------------------------------------------------------|
| `%x`      | Displays the value of the pointer (address) as an hex number  |
| `%o`      | Displays the value as an octal number                         |
| `%p`      | Displays the value in an implementation specific manner (hex) |

Please use `%p` when displaying addresses.

### Dereferencing a pointer using the indirection operator

The indirection operator * returns the value pointed to by a pointer variable.
This is frequently referred to as dereferencing a pointer. Please see example,

```c
int num = 5;
int *ptr = &num;

printf("%d\n", *ptr); // 5
```

### The concept of NULL

When `NULL` is assigned to a pointer, it means that the pointer does not point
to anything.

`NULL` is generally in `<stdlib.h>` defined as the following macro `#define NULL
((void *)0)`. i.e. it is a constant integer zero cast to a pointer of void.

We can test if an pointer is not NULL in the following way:

```c
int *ptr = (int*)malloc(sizeof(int));

if (ptr)
// NOT NULL
else
// NULL
```

### Understanding `size_t`

- The type `size_t` represents the maximum size any object can be in C.
- It is an unsigned integer as negative numbers doesn't make any sense in this
context.
- Its purpose is to provide a portable means of declaring a size consistent
with the addressable area of memory available on the system.
- It is good practice to use `size_t` when declaring variables for sizes such as
the number of characters and array indexes. It should also be used for loop
counters, indexing to arrays and sometimes pointer arithmetic.

Its definition can be found in `<stdlib.h>` as,

```c
#ifndef __SIZE_T
#define __SIZE_T
typedef unsigned int size_t;
#endif
```

- Its size will be 32 bits in length on 32-bit systems and 64 bits on 64-bit
systems.

### Pointer arithmetic

#### Addition

- When am integer is added to a pointer, the amount added is the product of the
integer times the number of bytes of the underlying datatype.

Please see following example,

```c
#include <stdio.h>

int main() {
  int numbers[] = {1, 2, 3, 4};
  int *ptr = numbers; // ptr points to the first element in the array numbers

  /*
  * Please note that the address that the pointer points to (the value of the
  * pointer) is increasing with 1 x sizeof(int) i.e. 1 x 4 while the element
  * it points to in numbers increases with 1.
  */
  printf("ptr: %p  --- *ptr: %d\n", ptr, *ptr); // ptr: 0x7ffeeb1ce100 *ptr: 1
  ptr++;
  printf("ptr: %p  --- *ptr: %d\n", ptr, *ptr); // ptr: 0x7ffeeb1ce104 *ptr: 2
  ptr++;
  printf("ptr: %p  --- *ptr: %d\n", ptr, *ptr); // ptr: 0x7ffeeb1ce108 *ptr: 3
  ptr++;
  printf("ptr: %p  --- *ptr: %d\n", ptr, *ptr); // ptr: 0x7ffeeb1ce10c *ptr: 4

  return 0;
}
```

#### Subtraction

Subtraction can also occur in the same way as addition. Please see following
example.

```c

#include <stdio.h>

int main() {
int numbers[] = {1, 2, 3, 4};
int *ptr = numbers + (4 - 1);

/*
* Please note that the address that the pointer points to (the value of the
* pointer) is decreasing with 1 x sizeof(int) i.e. 1 x 4 while the element
* it points to in numbers increases with 1.
*/
printf("ptr: %p  --- *ptr: %d\n", ptr, *ptr); // ptr: 0x7ffc7078f4dc *ptr: 4
ptr--;
printf("ptr: %p  --- *ptr: %d\n", ptr, *ptr); // ptr: 0x7ffc7078f4d8 *ptr: 3
ptr--;
printf("ptr: %p  --- *ptr: %d\n", ptr, *ptr); // ptr: 0x7ffc7078f4d4 *ptr: 2
ptr--;
printf("ptr: %p  --- *ptr: %d\n", ptr, *ptr); // ptr: 0x7ffc7078f4d0 *ptr: 1

return 0;
}
```

#### Subtraction of pointers

- When one pointer is subtracted from another, we get the difference between
their addresses.
- This is helpful for determining the order of elements in an array.
- The difference between pointers is the number of units by which they differ.

Please see example

```c

#include <stdio.h>

int main() {
int numbers[] = {1, 2, 3, 4};
int *ptr0 = numbers;
int *ptr1 = numbers + 1;
int *ptr2 = numbers + 2;
int *ptr3 = numbers + 3;

printf("ptr3 - ptr0 = %ld\n", ptr3 - ptr0); // 3
printf("ptr3 - ptr1 = %ld\n", ptr3 - ptr1); // 2
printf("ptr3 - ptr2 = %ld\n", ptr3 - ptr2); // 1
printf("ptr3 - ptr3 = %ld\n", ptr3 - ptr3); // 0

printf("-----------------------\n");
printf("ptr2 - ptr0 = %ld\n", ptr2 - ptr0); // 2
printf("ptr2 - ptr1 = %ld\n", ptr2 - ptr1); // 1
printf("ptr2 - ptr2 = %ld\n", ptr2 - ptr2); // 0
printf("ptr2 - ptr3 = %ld\n", ptr2 - ptr3); // -1

printf("-----------------------\n");

printf("ptr1 - ptr0 = %ld\n", ptr1 - ptr0); // 1
printf("ptr1 - ptr1 = %ld\n", ptr1 - ptr1); // 0
printf("ptr1 - ptr2 = %ld\n", ptr1 - ptr2); // -1
printf("ptr1 - ptr3 = %ld\n", ptr1 - ptr3); // -2

printf("-----------------------\n");

printf("ptr0 - ptr0 = %ld\n", ptr0 - ptr0); // 0
printf("ptr0 - ptr1 = %ld\n", ptr0 - ptr1); // -1
printf("ptr0 - ptr2 = %ld\n", ptr0 - ptr2); // -2
printf("ptr0 - ptr3 = %ld\n", ptr0 - ptr3); // -3
}
```

### Common uses for pointers

#### Multiple levels of indirection

Pointers can use different levels of indirection. It is not uncommon to see a
variable declared as a pointer to a pointer, sometimes called a double pointer.

Please see following example that uses three different types of arrays.

- First array is an array of strings that are used to hold book titles.
- The second array has the purpose to maintain a list of the best books.
- The third array has the purpose to maintain a list of english books.

The second and third arrays will instead of holding copies of the titles (which
is inefficient), they will hold the address of a title in the title array. Both
arrays will need to be declared as a pointer to pointer to a char. The arrays
elements will hold the addresses to the title arrays elements.

This method will avoid to have duplicate memory for each title and results ina
single results in a single location for titles. If a title needs to be changed,
then the change will only need to have be performed in one location.

##### Example

```c
#include <stddef.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main() {

  char *bookTitles[] = {"A tale of two cities",
  "Withering heights",
  "Don Quixote",
  "Odyssey",
  "Moby-Dick",
  "Hamlet",
  "Gulliver's Travels"};

  char **bestThreeBooks[3];
  bestThreeBooks[0] = &bookTitles[0];
  bestThreeBooks[1] = &bookTitles[3];
  bestThreeBooks[2] = &bookTitles[5];

  printf("%p\n", bestThreeBooks[0]);
  printf("%p\n", &bookTitles[0]);

  char **englishBooks[5];
  englishBooks[0] = &bookTitles[0];
  englishBooks[1] = &bookTitles[1];
  englishBooks[2] = &bookTitles[5];
  englishBooks[3] = &bookTitles[6];

  printf("Top 3 books\n");
  printf("--------------\n");

  for (size_t i = 0; i < 3; i++)
    printf("Best book %zu: %s\n", i, *bestThreeBooks[i]);

  printf("------------\n");

  printf("English books\n");
  printf("--------------\n");

  for (size_t i = 0; i < 4; i++)
    printf("English book %zu: %s\n", i, *englishBooks[i]);

  printf("------------\n");

return 0;
}
```

Output:

```bash
0x7ffdbc50ab90
0x7ffdbc50ab90
Top 3 books
--------------
Best book 0: A tale of two cities
Best book 1: Odyssey
Best book 2: Hamlet
------------
English books
--------------
English book 0: A tale of two cities
English book 1: Withering heights
English book 2: Hamlet
English book 3: Gulliver's Travels
------------
```

More examples:

##### Example 1

```c
#include <stddef.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main() {

char *bookTitles[] = {"A tale of two cities",
"Withering heights",
"Don Quixote",
"Odyssey",
"Moby-Dick",
"Hamlet",
"Gulliver's Travels"};

char *title = *(bookTitles + 0);
printf("title + 0: %s\n", title + 0);
printf("title + 1: %s\n", title + 1);
printf("title + 2: %s\n", title + 2);
printf("title + 3: %s\n", title + 3);
printf("title + 4: %s\n", title + 4);
printf("title + 5: %s\n", title + 5);

return 0;
}
```

prints:

```bash
title + 0: A tale of two cities
title + 1:  tale of two cities
title + 2: tale of two cities
title + 3: ale of two cities
title + 4: le of two cities
title + 5: e of two cities
```

##### Example 2

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main() {

char *bookTitles[] = {"A tale of two cioties",
"Wuthering heights",
"Don Quixote",
"Odyssey",
"Moby-Dick",
"Hamlet",
"Gulliver's Travels"};

char *title = *(bookTitles + 1);
printf("title + 0: %s\n", title + 0);
printf("title + 1: %s\n", title + 1);
printf("title + 2: %s\n", title + 2);
printf("title + 3: %s\n", title + 3);
printf("title + 4: %s\n", title + 4);
printf("title + 5: %s\n", title + 5);

return 0;
}
```

Output:

```bash
title + 0: Wuthering heights
title + 1: uthering heights
title + 2: thering heights
title + 3: hering heights
title + 4: ering heights
title + 5: ring heights
```

##### Example 3

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main() {

char *bookTitles[] = {"A tale of two cities",
"Wuthering heights",
"Don Quixote",
"Odyssey",
"Moby-Dick",
"Hamlet",
"Gulliver's Travels"};

char *title = bookTitles[0];
printf("title: %s\n", title);
printf("title[0]: %c\n", title[0]);
printf("title[1]: %c\n", title[1]);
printf("title[2]: %c\n", title[2]);
printf("title[3]: %c\n", title[3]);
printf("title[4]: %c\n", title[4]);
printf("title[5]: %c\n", title[5]);

return 0;
}
```

Output:

```bash
title: A tale of two cities
title[0]: A
title[1]:
title[2]: t
title[3]: a
title[4]: l
title[5]: e
```

##### Example 4

```c
#include <stddef.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main() {

char *bookTitles[] = {"A tale of two cities",
"Wuthering heights",
"Don Quixote",
"Odyssey",
"Moby-Dick",
"Hamlet",
"Gulliver's Travels"};

/*
     * Array of seven pointers gives the size: 7 x sizeof(char*) = 7 x 8 = 56.
     * i.e. the size of a char* pointer on a 64 bit system is 64 bit i.e. 8
     * byte.
     */

     printf("sizeof(bookTitles): %lu\n", sizeof(bookTitles));

     /*
     * Since bookTitles is an array of pointers, one certain element contains
     * only one pointer. i.e. the size of an element is then 1 x sizeof(char*)
     * i.e. 1 x 8 = 8.
     */

     printf("sizeof(bookTitles[0]): %lu\n", sizeof(bookTitles[0]));

     /*
     * To get the number of books in bookTitles, we simply divide
     * sizeof(bookTitles) with the sizeof(bookTitles[0]).
     */

     int nrBooks = sizeof(bookTitles) / sizeof(bookTitles[0]); // 7

     char **topThreeBooks = calloc(3, sizeof(char *));

     topThreeBooks[0] = calloc(strlen(bookTitles[0]), sizeof(char));
     strcpy(topThreeBooks[0], bookTitles[0]);

     topThreeBooks[1] = calloc(strlen(bookTitles[3]), sizeof(char));
     strcpy(topThreeBooks[1], bookTitles[3]);

     topThreeBooks[2] = calloc(strlen(bookTitles[5]), sizeof(char));
     strcpy(topThreeBooks[2], bookTitles[5]);

     printf("topThreeBooks[0]: %s\n", topThreeBooks[0]);
     printf("topThreeBooks[1]: %s\n", topThreeBooks[1]);
     printf("topThreeBooks[2]: %s\n", topThreeBooks[2]);

     return 0;
     }
     ```

     Output:

     ```bash
     sizeof(bookTitles): 56
     sizeof(bookTitles[0]): 8
     topThreeBooks[0]: A tale of two cities
     topThreeBooks[1]: Odyssey
     topThreeBooks[2]: Hamlet
```

##### Example 5

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int wordsinsentence(char **x) {
int w = 0;
while (*x) {
w += 1;
x++;
}
return w;
}

int wordsinmono(char ***x) {
int w = 0;
while (*x) {
w += wordsinsentence(*x);
x++;
}
return w;
}

int wordsinbio(char ****x) {
int w = 0;
while (*x) {
w += wordsinmono(*x);
x++;
}
return w;
}

int wordsinlib(char *****x) {
int w = 0;
while (*x) {
w += wordsinbio(*x);
x++;
}
return w;
}

int wordsinlol(char ******x) {
int w = 0;
while (*x) {
w += wordsinlib(*x);
x++;
}
return w;
}

int main(void) {
char *word;
char **sentence;
char ***monologue;
char ****biography;
char *****biolibrary;
char ******lol;

// fill data structure
word = malloc(4 * sizeof(*word)); // assume it worked
strcpy(word, "foo");

sentence = malloc(4 * sizeof(*sentence)); // assume it worked
sentence[0] = word;
sentence[1] = word;
sentence[2] = word;
sentence[3] = NULL;

monologue = malloc(4 * sizeof(*monologue)); // assume it worked
monologue[0] = sentence;
monologue[1] = sentence;
monologue[2] = sentence;
monologue[3] = NULL;

biography = malloc(4 * sizeof *(biography)); // assume it worked
biography[0] = monologue;
biography[1] = monologue;
biography[2] = monologue;
biography[3] = NULL;

biolibrary = malloc(4 * sizeof(*biolibrary)); // assume it worked
biolibrary[0] = biography;
biolibrary[1] = biography;
biolibrary[2] = biography;
biolibrary[3] = NULL;

lol = malloc(4 * sizeof(*lol)); // assume it worked
lol[0] = biolibrary;
lol[1] = biolibrary;
lol[2] = biolibrary;
lol[3] = NULL;

printf("total words in my lol: %d\n", wordsinlol(lol));

free(lol);
free(biolibrary);
free(biography);
free(monologue);
free(sentence);
free(word);
}
```

Output:

```bash
total words in my lol: 243
```

##### Example 6

```c
#include <stddef.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main() {

    char *bookTitles[] = {"A tale of two cities",
                          "Withering heights",
                          "Don Quixote",
                          "Odyssey",
                          "Moby-Dick",
                          "Hamlet",
                          "Gulliver's Travels"};

    /*
    If you want to retrieve values from an array from another array, you need to create a double pointer
    or else your code becomes inefficient and you are about to copy the data to a new adresses.
    With a double pointers, you are basically not creating any new data besides the pointers
    i.e. you need to create *(*bestBooks[]) and use that for pointing to the values in char bookTitles[].
    */

    char *testBestBooks[3];

    testBestBooks[0] = bookTitles[1]; // Will copy data to a new adress - inefficient
    testBestBooks[1] = bookTitles[5]; // Will copy data to a new adress - inefficient
    testBestBooks[2] = bookTitles[6]; // Will copy data to a new adress - inefficient


    char **bestBooks[3];

    bestBooks[0] = &bookTitles[1]; // Will create a pointer that points to the adress of bookTitles[] - EFFICIENT since your are not creating any extra data (besides from the new pointers)
    bestBooks[1] = &bookTitles[5]; // Will create a pointer that points to the adress of bookTitles[] - EFFICIENT since your are not creating any extra data (besides from the new pointers)
    bestBooks[2] = &bookTitles[6]; // Will create a pointer that points to the adress of bookTitles[] - EFFICIENT since your are not creating any extra data (besides from the new pointers)


    printf("Addresses of best books using char *bookTitles[]\n");
    printf("-----------------------------------------------\n");
    printf("bookTitles[1] = %s - Address (&bookTitles[1] = %p)\n", bookTitles[1],
           &bookTitles[1]);
    printf("bookTitles[5] = %s - Address (&bookTitles[5] = %p)\n", bookTitles[5],
           &bookTitles[5]);
    printf("bookTitles[6] = %s - Address (&bookTitles[6] = %p)\n", bookTitles[6],
           &bookTitles[6]);
    printf("\n");

    printf("Addresses of best books using char *testBestBooks[]\n");
    printf("-----------------------------------------------\n");

    for (int i = 0; i < 3; i++) {
      printf("Value: testBestBooks[%d] => %s - Address: (&testBestBooks[%d] => %p)\n", i, testBestBooks[i], i, &testBestBooks[i]); // PROOF: These adresses will not be the same as &bookTitles - Thereby, this provides proof that data is copied to a new adress
    }

    printf("\n");

    printf("Addresses of best books using char **bestBooks[]\n");
    printf("-----------------------------------------------\n");
    for(int i = 0; i <= 3; i++) {
      printf("Value: *bestBooks[%d] => %s\n",i,*bestBooks[i]);
      printf("Adress (where pointer points): bestBooks[%d] => %p\n", i, bestBooks[i]); // This one points to bookTitles i.e. No data has been copied.
      printf("Adress of pointer: &bestBooks[%d] => %p\n",i, &bestBooks[i]);
      printf("-----------------------------------------------\n");
    }

    return 0;
}
```

Output:

```
Addresses of best books using char *bookTitles[]
-----------------------------------------------
bookTitles[1] = Withering heights - Address (&bookTitles[1] = 0x7ffe0b706dc8)
bookTitles[5] = Hamlet - Address (&bookTitles[5] = 0x7ffe0b706de8)
bookTitles[6] = Gulliver's Travels - Address (&bookTitles[6] = 0x7ffe0b706df0)

Addresses of best books using char *testBestBooks[]
-----------------------------------------------
Value: testBestBooks[0] => Withering heights - Address: (&testBestBooks[0] => 0x7ffe0b706d80)
Value: testBestBooks[1] => Hamlet - Address: (&testBestBooks[1] => 0x7ffe0b706d88)
Value: testBestBooks[2] => Gulliver's Travels - Address: (&testBestBooks[2] => 0x7ffe0b706d90)

Addresses of best books using char **bestBooks[]
-----------------------------------------------
Value: *bestBooks[0] => Withering heights
Adress (where pointer points): bestBooks[0] => 0x7ffe0b706dc8
Adress of pointer: &bestBooks[0] => 0x7ffe0b706da0
-----------------------------------------------
Value: *bestBooks[1] => Hamlet
Adress (where pointer points): bestBooks[1] => 0x7ffe0b706de8
Adress of pointer: &bestBooks[1] => 0x7ffe0b706da8
-----------------------------------------------
Value: *bestBooks[2] => Gulliver's Travels
Adress (where pointer points): bestBooks[2] => 0x7ffe0b706df0
Adress of pointer: &bestBooks[2] => 0x7ffe0b706db0
-----------------------------------------------
```

#### Constant pointers

##### Pointer to a constant

Please see following example,

```c
#include <stdio.h>

int main() {
const int num = 5;
int limit = 500;
const int *ptr = &num;
printf("*ptr : %d\n", *ptr); // 5
ptr = &limit;
printf("*ptr : %d\n", *ptr); // 500
// ptr = 10; <---- ERROR!
return 0;
}
```

- A pointer can be used to point to a constant. This means that the type of the
pointer needs to be `const int *ptr`. Please compare this with `const *int ptr`
which is basically a constant pointer.
- A pointer to a constant (`const int *ptr`) can be dereferenced nut cannot be
used to modify the value it is referencing. i.e. `printf("%d\n", *ptr)` is fine
but `*ptr = 10` gives an error.
- The pointer value is not constant i.e. the pointer can be changed to reference
another constant integer or simply an integer.
- `ptr` can be assigned to point to different constant integers.
- `ptr` can be assigned to point to different nonconstant integers.
- `ptr` can be dereferenced for reading purposes.
- `ptr` cannot be dereferenced to change what it points to.

##### Constant pointer to nonconstants

```c
int num = 5;
int limit = 500;
int *const cpi = &num;
*cpi = limit; // OK!
*cpi = 700; // OK!
// cpi = &limit; <---- ERROR!
```

With the declaration above,

- `ptr` must be initialized to a nonconstant variable.
- `ptr` cannot be modified.
- The data pointed to by `ptr` can be modified.

### Dynamic memory management i C

- Languages such as C support dynamic memory allocation management where objects
are allocated memory from the heap.

#### Dynamic memory allocation

- Basic Steps of memory allocation i C:

1. Use `malloc()` / `calloc()` / `realloc()` to allocate memory
2. Use the allocated memory to support the application
3. Deallocate the memory using the `free()` function.

Example (Allocating memory for one int):

```c
int *ptr = (int*)malloc(1 * sizeof(int));
if (ptr == NULL) {
fprintf(stderr, "Failed to allocate memory\n");
exit(0);
}
*ptr = 5;
printf("*ptr: %d\n", *ptr);
free(ptr);
```

- `malloc` specifies the number of bytes to allocate.
- If succesful, it returns a pointer to memory allocated on the heap.
- if it fails, it returns a null pointer.

Please note, a common practise is to always also assign `NULL` to a freed
pointer.

##### Memory leaks

- A memory leak occurs when allocated memory is never used again but is not
freed. This can happen when:

1. Memory adress is lost.
2. The `free` function is never invoked though is should be.

- A problem with memory leaks is that memory cannot be reclaimed and used later.
- The amount of memory available to the heap manager is decreased.
- If memory is repeatedly allocated and then lost, then the program may
terminate when more memory is needed but `malloc` cannot allocate it because it
ran out of memory.

In the example below, variable `chunck` is assigned memory from the heap. For
every loop, memory is not freed before another block of memory is assigned to
it. Eventually, the application will run out of memory and terminate abnormally.

```c
char *chunck;

while (1) {
chunck = (char*)malloc(1000000);
}
```

###### Losing an adress

One example of loosing an adress is when a pointer is reassigned to a new
adress. Please see following example.

```c
int *ptr = (int*)malloc(sizeof(int));
*ptr = 5;
ptr = (int*)malloc(sizeof(int));
```

The main issue with the above code is that the first memory blocks adress is
lost and the memory is not freed.

A second example allocates memory for a string, initializes it, and then
displays the string character by character.

```c
char *name = (char*)malloc(strlen("Susan") + 1);
strcpy(name, "Susan");
while (*name != 0) {
printf("%c", *name);
name++
}
```

The code above increments the pointer `name` by one for each iteration. At the
end, `name` is left pointing to the strings `NUL` termination character and the
allocated memorys starting adress has been lost.

#### Dynamic memory allocation functions

| Function  | Description                                              |
|-----------|----------------------------------------------------------|
| `malloc`  | Allocates memory from the heap                           |
| `realloc` | Reallocates memory based on a previosleu allocated block |
| `calloc`  | Allocates and zeros out memory from the heap             |
| `free`    | Returns a block of memory to the heap                    |

- Dynamic memory is allocated  from the heap
- With successive memory calls, there is o guarantee regarding the order of the
memory or the continuity of memory allocated
- The memory allocated will be aligned according to pointers data type i.e. a
four-byte integer would be allocated on an adress boundarey evenly divisible by
four.
- The adress returned by the heap manager will contain the lowest byte's adress.

##### Malloc

- `malloc` allocates a block of memory from the heap.
- The number of bytes allocated is specified by its single argument.
- Return type is a pointer to `void`
- If memory is not available, `NULL` is returned
- The function does not clear or otherwise modify the memory, thus the contents
of memory should be treated as if it contained garbage.
- Use case: `int *ptr = (int*)sizeof(1 x sizeof(int))` <-- Allocates memory for
1 int.

The following steps are performed when `malloc` function is executed:

1. Memory is allocated from the heap
2. The memory is not modified or otherwise cleared.
3. The first byte's adress is returned.

Good practise for `malloc`:

```c
int *ptr = (int*)malloc(sizeof(int));
if (ptr != NULL)
// Pointer is good.
else
// Pointer is bad.
```

###### To cast or not cast

Since a pointer to void can be assigned to any other pointer type, explicit
casting is no longer required. Some developers considers explicit casts to be a
good practise because:

1. They document the intation of the malloc function.
2. They make the code compatible with C++ (or earlier C complilers), which
require explicit casts.

###### Failing to allocate memory

- If you declare a pointer but fail to allocate memory to the adress it points
to before using it, that memory will usually contain garbage. As in the example
below. Before writing data with a pointer, memory needs first to be allocated.

```c
char *name;
printf("Enter your name: ");
scanf("%s", name); // <--- ERROR! Memory not allocated
```

###### Not using the right size for the malloc function

A common error is following:

```c
const int NUMBER_OF_DOUBLES = 10;
double *ptr = (double*)malloc(NUMBER_OF_DOUBLES);
/* Line above has an error. It only allocates 10 bytes while the size of an
double is 8 byte.
*/
```

Please note, in the code above only 10 bytes is allocated which can store only 1
double (size (8 bytes))

Please note, in the code above only 10 bytes is allocated which can store only 1
double (size (8 bytes)). If you want to allocate memory for 10 doubles, you will
need to consider the size of a double.

```c
const int NUMBER_OF_DOUBLES = 10;
double *ptr = (double*)malloc(NUMBER_OF_DOUBLES * sizeof(double));
```

###### Determing the amount of memory allocated

When `malloc` executes, it is supposed to allocate the amount of memory
requested and then return the memorys adress.

##### Using the `calloc` function

- The `calloc` function will allocate and clear memory  at the same time. Its
prototype follows: `void *calloc(size_t numElements, size_t elementSize)`.

- A pointer is returned to the first byte of memory. If the function is unable
to allocate memory, `NULL` is returned.

- `ptr` is allocated 20 bytes, all contained zeros: `int *ptr = calloc(5,
sizeof(int));`

Instead of usign `calloc`, the `malloc` function along with the memset function
can be used to achieve same results. Please see example below,

```c
int *ptr = malloc(5 * sizeof(int));
memset(ptr, 0, 5 * sizeof(int));
```

- Use `calloc` when memory needs to be zeroed out.
- Execution of `calloc` may take longer than `malloc`.

##### Using the `realloc` function

- It may be necessary to increase or decrease the amount of memory allocated to
a pointer. A normal use case for this is a variable sized array.
- Prototype: `void *realloc(void *ptr, size_t size);`
- Returns a pointer to a block of memory.
- Two arguments:
  - Pointer to the original block.
  - Requested block size.
- If size is less than what is currently allocated, the excess memory is
returned to the heap. There is no guarantee that the excess memory is cleared.
- If the size is greater than what is currently allocated, then if possible, the
memory will be allocated from the region immeditely following the current
allocation. Otherwise, memory is allocated from a different region of the heap
and the old memory is copied to the new region.

#### Deallocating memory using the free function

- Dynamic memory allocation: Programmer is able to return memory when it is no
longer needed and thereby, freeing up its use for other.
- Prototype `void free(void *ptr);`
- The pointer argument should contain the adress of memory allocated by a
"`malloc` type function".
- The memory is returned to the heap.
- While pointer may still be pointed to the region, always assume it contains
garbage.
- Region may be reallocated later and populated with different data.

Example:

```c
int *pi = (int*)malloc(sizeof(int));
*pi = 5;
free(pi);
```

Please note that even `pi` is freed, it still contain the previous adress.
hence, `pi` has become a **dangling pointer**.

Please note that the following code has undefined behavior and should be
avoided meaning that you can only use `free()` on pointers created from `malloc`
type of functions.

```c
int num = 5;
int *ptr = &num;
free(ptr);
```

##### Assigning `NULL` to a freed pointer to adress dangling pointer

- Pointers can cause problems even they are freed.
- If we try to dereference a freed pointer, its behavior is undefined and
should be avoided.
- As a result, programmers will explicitly assign `NULL` to a pointer to
designate the pointer as invalid.
- Subsequent use of such pointer will result in an runtime exception.

```c
int *ptr = (int *)malloc(sizeof(int));
*ptr = 5;
free(ptr)
ptr = NULL;
```

##### Double free

Example:

```c
int *ptr = (int*)malloc(sizeof(int));
*ptr = 5;
int *ptr2 = ptr;

free(ptr);
free(ptr2);
```

- Heap managers don't attempt to detect if the same memory has been freed twice.
- This normally results in a corrupted heap and program termination.
- There is no reason to free the same memory twice.
- It has been suggested that the `free` function should assign `NULL` to its
pointer when it returns. But since pointers are passed by value, the `free`
function is unable to explicitly assign `NULL` to the pointer.

#### Dangling pointers

If a pointer still references the original memory after it has been freed, it is
a called a **dangling pointer**.

This can result in different problems:

- Unpredictable behavior if the memory is accessed.
- Segmentation faults when the memory is no longer accessible.
- Potential security risks.

Dangling pointers often result from:

- Memory is accesses when freed.
- A pointer is returned to an automatic variable ina previous function call.

#### Dangling pointer examples

##### Dangling pointer example 1

```c
int *ptr = (int*)malloc(sizeof(ptr));
*ptr = 5;
printf("*ptr = %d\n", *ptr);
free(ptr);
*ptr = 10;
```

##### Dangling pointer example 2

```c
int *ptr = (int*)malloc(sizeof(int));
*p1 = 5;
int *ptr2 = ptr;
free(ptr);
*ptr2 = 10; // Dangling pointer
```

##### Dangling pointer example 3

Often, dangling block can occur when using block statements as below:

```c
int *ptr;
...
{
int temp = 5;
ptr = &temp;
}
// ptr is now a Dangling pointer.
```

- Most compilers treats a block statement as a stack frame.
- The variable temp was allocated on the stack frame and subsequently popped off
the stack when the block statement was exited.
- Pointer `ptr` is now left pointing to a region of memory that may eventually
be overridden.

#### Dealing with dangling pointers

Examples:

- Setting a pointer to `NULL` after freeing it.
- Writing a special `free()` function.
- Use third party tools to detect memory leaks and dangling pointers.
- `assert` macro can also be useful.

#### Dynamic memory allocation technologies

##### Garbage collection in C

- When memory is no longer needed, it is collected and made available for use
later in the program.
- The deallocated memory si referred to as garbage, hence, the term garbage
collection refers to the processing of this information.

### Pointers and functions

- Pointers allow data to be passed and modified by a function.
- When pointers can also hold the adress of a function, the provide means to
dynamically control a programs execution flow.
- When a function is invoked, its stack frame is created and then pushed onto
the program stack. When the function returns, its stack frame is popped off of
the program stack.

Working with functions, there are two areas ehre pointers become useful.

1. **Passing a pointer to a function** - This allows the function to modify data
references by the pointer and to pass blocks of information more efficiently.
2. **Declaring a pointer to a function** - Controls the execution flow of a
program.

#### Program stack and heap

##### Program stack

- The program stack is an area of memory that supports the execution of
functions and is normally shared with the heap.
- The program stack tends to occupy the lower parts of the region, whole the
heap uses the upper part.
- The program stack holds stack frames, sometimes called activation records or
activation frames. Stack frames hold the parameters and local variables orf a
function.
- When a function is called, its stack frame are pushed onto the stack and the
stack grows "upward". When a function terminates, its stack frame is popped off
the program stack
- When a function is called, its stack frame are pushed onto the stack and the
stack grows "upward". When a function terminates, its stack frame is popped off
the program stack

#### Passing and returning by pointer

- Passing pointers allows the reference object to be accessible in multiple
functions without making the object global. This means that only those
functions that need access to the object will get this access and that the
object does not need to be duplicated.
- Of data needs to be modified in a function, it needs to be passed by a
pointer. We can pass data by a pointer and prohibit it from being modified by
passing it as a pointer to an constant.
- Parameters, including pointers are passed by value. That is, a copy of the
argument is passed to the function. Passing a pointer to an argument can be
efficient when dealing with large data structures.
- Passing a pointer to the object means that the object does not have to be
copied, and we can access the object though the pointer.

##### Passing data using a pointer

Following code works since code is passed by pointer.

```c
void swapWithPointer(int *num1, int *num2) {
int temp;
temp = *num1;
*num1 = *num2;
*num2 = temp;
}

int main() {

int n1 = 5;
int n2 = 10;
swapWithPointer(&n1, &n2);
return 0;
}
```

##### Passing data by value

Please note that following code will not work. It will not swap any values since
data is passed by value and not pointer.

```c
void swap(int num1, int num2) {
int temp;
temp = num1;
num1 = num2;
num2 = temp;
}

int main() {

int n1 = 5;
int n2 = 10;
swapWithPointer(n1, n2); // <---- WILL NOT WORK!
return 0;
}
```

##### Returning a pointer

```c
int *allocateArray(int size, int value) {

int *arr = (int*)malloc(size * sizeof(int));
for (size_t i = 0; i < size; i++)
arr[i] = value;

return arr;
}

int main() {
int * vector = allocateArray(10, 0);
for (size_t i = 0; i < 10; i++)
printf("%d\n", vector[i]);

return 0;
}
```

##### Pointers to local data

- Returning a pointer to local data is an easy mistake to make if you don't
understand how the program stack works. See example below,

```c
int *allocateArray(int size, int value) {
int arr[size];
for (size_t i = 0; i < size; i++)
arr[i] = value;

return arr;
}

int main() {
int * vector = allocateArray(10, 0);
for (size_t i = 0; i < 10; i++)
printf("%d\n", vector[i]);

return 0;
}
```

- The mistake here is that the adress of the array is no longer valid once the
function returns because the functions stack frame is popped off the stack.
- While ach array element may still contain its value, these values may be
overwritten if another function is called.

One solution could be to define the array `arr` as static. Please see code
below:

```c
#include <stddef.h>
#include <stdio.h>

int *allocateArray(int size, int value);

int main() {

int *vector = allocateArray(10, 45);

for (size_t i = 0; i < 5; i++)
printf("vector[%zu] = %d\n", i, vector[i]);

vector = allocateArray(20, 100);

for (size_t i = 0; i < 5; i++)
printf("vector[%zu] = %d\n", i, vector[i]);

vector[0] = 200;
vector[1] = 300;
vector[2] = 400;
vector[3] = 500;
vector[4] = 600;

for (size_t i = 0; i < 5; i++)
printf("vector[%zu] = %d\n", i, vector[i]);

return 0;
}

int *allocateArray(int size, int value) {
// PLEASE NOTE: CANNOT USE SIZE WHEN USING STATIC. SIZE OF ARRAY NEED TO BE
// CONSTANT
static int arr[5];
for (size_t i = 0; i < 5; i++)
arr[i] = value;

return arr;
}
```

- Please note that every time `allocateArray` function is called, it will reuse
the array. This effectively invalidates any previous calls to the function.
- Static array must be declared of fixed size. This will limit the functions
ability toi handle various array sizes.

##### Passing `NULL` pointers

- When a pointer is passed to a function, it is always good practice to verify
it is not NULL before using it.

```c
int *allocateArray(int *arr, int size, int value) {
if (arr != NULL) {
for (size_t i = 0; i < size; i++)
arr[i] = value;
}
return arr;
}
```

##### Passing a pointer to a pointer

- When a pointer is passed to a function, it is passed by value.
- If we want to modify the original pointer and not the copy of the pointer, we
need to pass a pointer to a pointer.

```c
void allocateArray(int **arr, int size, int value) {
*arr = (int *)malloc(size * sizeof(int *));
if (*arr != NULL) {
for (int i = 0; i < size; i++)
*(*arr + i) = value;
}
}

int main() {
int *vector = NULL;
allocateArray(&vector, 10, 10);
return 0;
}
```

##### Writing your own `free` function

Problems with the original `free` function:

- Does not check the pointer passed to see wether it is NULL.
- Does not set the pointer to NULL before it returns.

```c
void saferFree(void **p) {
if (p != NULL && *pp != NULL) {
free(*p);
*p = NULL;
}
}
```

- Please note that its parameter is declared to `void`.
- Using a pointer to pointer allows us to modify the pointer passed.
- Using the void type allows all types of pointers to be passed.
- A warning is recieved if we dont explicitly cast the pointer to type `void`.
This can be accomplioshed with a macro as below:

```c
#define safeFree(p) saferFree((void)**&(p))
```

Running it in the following format gives:

```c
#include <stdio.h>
#include <stdlib.h>

#define safeFree(p) saferFree((void **)&(p))

void saferFree(void **ptr);

int main() {
int *ptr = (int *)malloc(sizeof(int));
*ptr = 5;
printf("%p\n", ptr);
safeFree(ptr);
printf("%p\n", ptr);

return 0;
}

void saferFree(void **ptr) {
if (ptr != NULL && *ptr != NULL) {
free(*ptr);
*ptr = NULL;
}
}
```

With the output:

```bash
0x5637b308c2a0
(nil)
```

#### Function pointers

- A function pointer is a pointer that holds the adress of a function.

##### Declaring function pointer

Prototypes:

`void (*foo)();` \
`int (*fi)(double);` \
`void (*f2)(char*);` \
`double* (*f3)(int, int)`

- Please do not confuse these with functions that pass a pointer.

##### Using a function pointer

```c
#include <stdio.h>

int square(int num) { return num * num; }

int main() {
int (*fptr1)(int); // Function pointer
int n = 5;
fptr1 = square; // fptr1 = &square is not needed.
printf("%d squared = %d\n", n, fptr1(n));
return 0;
}
```

Output:

```bash
5 squared = 25
```

It is also convenient to use `typedef` in this case. See below.

```c
#include <stdio.h>

typedef int (*funcPtr)(int);

int square(int num) { return num * num; }

int main() {
funcPtr fptr1;
int n = 5;
fptr1 = square; // fptr1 = &square is not needed.
printf("%d squared = %d\n", n, fptr1(n));
return 0;
}
```

##### Passing function pointers

```c
#include <stdio.h>

typedef int (*functionPtr)(int, int);

int add(int n1, int n2) { return n1 + n2; }
int subtract(int n1, int n2) { return n1 - n2; }
int multiply(int n1, int n2) { return n1 * n2; }
int compute(functionPtr operation, int n1, int n2) { return operation(n1, n2); }

int main() {
int n1 = 10;
int n2 = 5;

printf("%d + %d = %d\n", n1, n2, compute(&add, n1, n2)); // & is not needed
printf("%d - %d = %d\n", n1, n2, compute(&subtract, n1, n2));
printf("%d * %d = %d\n", n1, n2, compute(&multiply, n1, n2));

return 0;
}
```

Output:

```bash
10 + 5 = 15
10 - 5 = 5
10 * 5 = 50
```

##### Returning function pointers

- Returning a function pointer requires declraring the functions return type as
a function pointer.

```c
#include <stdio.h>

typedef int (*functionPtr)(int, int);

int add(int n1, int n2) { return n1 + n2; }
int subtract(int n1, int n2) { return n1 - n2; }
int multiply(int n1, int n2) { return n1 * n2; }
int compute(functionPtr operation, int n1, int n2) { return operation(n1, n2); }

functionPtr select(char opcode) {
switch (opcode) {
case '+':
return &add;
case '-':
return &subtract;
default:
return &multiply;
}
}

int evaluate(char opcode, int n1, int n2) {
functionPtr operation = select(opcode);
return operation(n1, n2);
}

int main() {
int n1 = 10;
int n2 = 5;

printf("%d + %d = %d\n", n1, n2, evaluate('+', n1, n2));
printf("%d - %d = %d\n", n1, n2, evaluate('-', n1, n2));
printf("%d * %d = %d\n", n1, n2, evaluate('*', n1, n2));

return 0;
}
```

Output:

```bash
10 + 5 = 15
10 - 5 = 5
10 * 5 = 50
```

##### Using an array of function pointers

```c
#include <stdio.h>

typedef int (*operation)(int, int);
operation operations[128] = {NULL};
typedef int (*functionPointer)(int, int);

int add(int n1, int n2) { return n1 + n2; }
int subtract(int n1, int n2) { return n1 - n2; }
int multiply(int n1, int n2) { return n1 * n2; }

int evaluate(char opcode, int n1, int n2) {
functionPointer operation;
operation = operations[opcode];
return operation(n1, n2);
}

void initializeOperationsArray() {
operations['+'] = &add;
operations['-'] = &subtract;
operations['*'] = &multiply;
}

int main() {
int n1 = 10;
int n2 = 5;

initializeOperationsArray();
printf("%d + %d = %d\n", n1, n2, evaluate('+', n1, n2));
printf("%d - %d = %d\n", n1, n2, evaluate('-', n1, n2));
printf("%d * %d = %d\n", n1, n2, evaluate('*', n1, n2));

return 0;
}
```

Same output as above.

##### Comparing function pointers

Following code is totally valid:

```c
functionPointer fptr1 = add;

if (fptr1 == add)
//
else
///
}
```

### Pointers and arrays

#### A quick review of arrays

- An array is a contiguous collection of homogenous elements that can be accessed
using an index.
- Contiguous: Elements of an array are adjacent to one another in memory.
- Homogenous: All elements are of same type.

##### One dimensional arrays

- An array has no information about the number of elements it contains.
- The array is simply references to a block of memory.
- You can use the `sizeof()` operator with an array to determine how many bytes
are allocated to the array.
- You can use the following code to determine the length of an array:
`sizeof(array) / sizeof(array[0]);`
- Declaration: `int array[5] = {1, 2, 3, 4, 5};`

##### Two dimensional arrays

- Two dimensional array needs to be mapped to one-dimensional adress space of
main memory.
- A two dimensional array is treated as an array of arrays. When we are using
the array with only one subscript, we get the pointer to the corresponding row.

Please see following examples:

```c
#include <stdio.h>

int main() {
int matrix[2][3] = {{1, 2, 3}, {4, 5, 6}};
for (int i = 0; i < 2; i++)
printf("&matrix[%d]: %p - sizeof(matrix[%d]): %lu\n", i, &matrix[i], i,
sizeof(matrix[i]));
return 0;
}
```

Output:

```bash
&matrix[0]: 0x7ffebbb02390 - sizeof(matrix[0]): 12
&matrix[1]: 0x7ffebbb0239c - sizeof(matrix[1]): 12
```

Another example:

```c
#include <stdio.h>

int main() {
int matrix[2][3] = {{1, 2, 3}, {4, 5, 6}};
for (int i = 0; i < 2; i++)
for (int j = 0; j < 3; j++)
printf("&matrix[%d][%d]: %p - sizeof(matrix[%d][%d]): %lu\n", i, j,
&matrix[i][j], i, j, sizeof(matrix[i]));
return 0;
}
```

Output:

```bash
&matrix[0][0]: 0x7fffddbc8180 - sizeof(matrix[0][0]): 4
&matrix[0][1]: 0x7fffddbc8184 - sizeof(matrix[0][1]): 4
&matrix[0][2]: 0x7fffddbc8188 - sizeof(matrix[0][2]): 4
&matrix[1][0]: 0x7fffddbc818c - sizeof(matrix[1][0]): 4
&matrix[1][1]: 0x7fffddbc8190 - sizeof(matrix[1][1]): 4
&matrix[1][2]: 0x7fffddbc8194 - sizeof(matrix[1][2]): 4
```

#### Pointer notation and arrays

Please note the following pointer / vector notation:

```c
#include <stdio.h>

int main() {
int vector[] = {1, 2, 3, 4, 5};
int *ptr = vector;

for (int i = 0; i < sizeof(vector) / sizeof(int); i++) {
printf("vector + %d = %p, &vector[%d] = %p, ptr + %d = %p, &ptr[%d] = "
"%p (ALL THE SAME)\n",
i, vector + i, i, &vector[i], i, ptr + i, i, &ptr[i]);
}

printf("-------------------------\n");

for (int i = 0; i < sizeof(vector) / sizeof(int); i++) {
printf(
"*(vector + %d) = %d, vector[%d] = %d, *(ptr + %d) = %d, ptr[%d] = "
"%d (ALL THE SAME)\n",
i, *(vector + i), i, vector[i], i, *(ptr + i), i, ptr[i]);
}

return 0;
}
```

Output:

```bash
vector + 0 = 0x7ffd749126d0, &vector[0] = 0x7ffd749126d0, ptr + 0 = 0x7ffd749126d0, &ptr[0] = 0x7ffd749126d0 (ALL THE SAME)
vector + 1 = 0x7ffd749126d4, &vector[1] = 0x7ffd749126d4, ptr + 1 = 0x7ffd749126d4, &ptr[1] = 0x7ffd749126d4 (ALL THE SAME)
vector + 2 = 0x7ffd749126d8, &vector[2] = 0x7ffd749126d8, ptr + 2 = 0x7ffd749126d8, &ptr[2] = 0x7ffd749126d8 (ALL THE SAME)
vector + 3 = 0x7ffd749126dc, &vector[3] = 0x7ffd749126dc, ptr + 3 = 0x7ffd749126dc, &ptr[3] = 0x7ffd749126dc (ALL THE SAME)
vector + 4 = 0x7ffd749126e0, &vector[4] = 0x7ffd749126e0, ptr + 4 = 0x7ffd749126e0, &ptr[4] = 0x7ffd749126e0 (ALL THE SAME)
-------------------------
*(vector + 0) = 1, vector[0] = 1, *(ptr + 0) = 1, ptr[0] = 1 (ALL THE SAME)
*(vector + 1) = 2, vector[1] = 2, *(ptr + 1) = 2, ptr[1] = 2 (ALL THE SAME)
*(vector + 2) = 3, vector[2] = 3, *(ptr + 2) = 3, ptr[2] = 3 (ALL THE SAME)
*(vector + 3) = 4, vector[3] = 4, *(ptr + 3) = 4, ptr[3] = 4 (ALL THE SAME)
*(vector + 4) = 5, vector[4] = 5, *(ptr + 4) = 5, ptr[4] = 5 (ALL THE SAME)
```

##### Difference between arrays and pointers

Example:

```c
#include <stdio.h>

int main() {
int vector[] = {1, 2, 3, 4, 5};
int *ptr = vector;

printf("BEFORE\n");
printf("---------------------\n");
printf("[");
for (int i = 0; i < sizeof(vector) / sizeof(vector[0]); i++)
printf("%d ", ptr[i]);
printf("]\n");

// Multiply every element with 5:
int value = 5;
for (int i = 0; i < sizeof(vector) / sizeof(vector[0]); i++)
*ptr++ *= value;

ptr = ptr - 5;

/*
     * PLEASE NOTE: YOU WOULD NOT BE ABLE TO DO vector = vector - 5 SINCE vector
     * is an array. YOU CAN ONLY DO THIS WITH POINTERS.
     */

     printf("AFTER\n");
     printf("---------------------\n");

     printf("[");
     for (int i = 0; i < sizeof(vector) / sizeof(vector[0]); i++)
     printf("%d ", ptr[i]);
     printf("]\n");

     return 0;
     }
     ```

     Output:

     ```bash
     BEFORE
     ---------------------
     [1 2 3 4 5 ]
     AFTER
     ---------------------
     [5 10 15 20 25 ]
     ```

#### Using malloc to create one-dimensional arrays

Example

```c
int *ptr = (int*)malloc(5 * sizeof(int));
for (int i = 0; i < 5; i++)
ptr[i] = i + 1; // SAME AS *(ptr + i) = i + 1;
```

#### Using `realloc()` function to resize an array

- The `realloc` function can be used to decrease the amount of space used by a
pointer.

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

char *trim(char *phrase) {
char *old = phrase;
char *new = phrase;

while (*old == ' ')
old++;

while (*old) {
/*printf("new: %c\n", *new);*/
/*printf("old: %c\n", *old);*/
printf("phrase: %s\n", phrase);
*(new ++) = *(old++);
}

printf("phrase: %s\n", phrase);
*new = 0;
printf("phrase: %s\n", phrase);
return (char *)realloc(phrase, strlen(phrase) + 1);
}

int main() {
char *buffer = (char *)malloc(strlen(" cat") + 1);
strcpy(buffer, " cat");
printf("phrase: %s\n", trim(buffer));
return 0;
}
```

Output:

```bash
phrase:  cat
phrase: ccat
phrase: caat
phrase: catt
phrase: cat
```

#### Passing a one-dimensional array

- When a one dimensional array is passed to a function, the arrays adress is
passed by value.
- This makes the transfer of information more efficient since we are not passing
the entire array and having to allocate memory in the stack for it.
- This means that the arrays size needs to be passed as an argument to the
function. If we don't, all we then have in the function is the adress to the
beginning of an array with no indication of its size (unless string array - then
the end can be found by searching for an '\0'.

##### Array notation

```c
void displayArray(int arr[], int size) {
for (int i = 0; i < size; i++)
printf("arr[%d]: %d\n", i, arr[i]);
}

int main() {
int vector[] = {1, 2, 3, 4, 5};
displayArray(vector, sizeof(vector) / sizeof(vector[0]));
return 0;
}
```

##### Pointer notation

```c
void displayArray(int *arr, int size) {
for (int i = 0; i < size; i++)
printf("%d\n", arr[i]); // Could also be written as: *(arr + i)
}
```

#### Using a one-dimensional array of pointers

Example:

```c
int *arr[5]; // array of 5 pointers
for (int i = 0; i < 5; i++) {
arr[i] = (int*)malloc(sizeof(int)); // *(arr + i) = (int*)malloc(sizeof(int));
*arr[i] = i; // **(arr + i) = i
}
```

Please see the following table of values of `arr`:

| Expression             | Value                                              |
|------------------------|----------------------------------------------------|
| `*arr[0]`              | 0                                                  |
| `**arr => **(arr + 0)` | 0                                                  |
| `**(arr + 1)`          | 1                                                  |
| `arr[0][0]`            | 0                                                  |
| `arr[3][0]`            | 3                                                  |
| `arr[3][1]`            | N/A - pointer `arr[3]` only has 1 element i.e. `0` |

#### Pointers and multidimensional arrays

```c
#include <stdio.h>

int main() {
int matrix[2][5] = {{1, 2, 3, 4, 5}, {6, 7, 8, 9, 10}};
int rows = sizeof(matrix) / sizeof(matrix[0]);
int columns = sizeof(matrix[0]) / sizeof(matrix[0][0]);

printf("sizeof(matrix): %lu\n", sizeof(matrix));          // 2 * 5 * 4 (int)
printf("sizeof(matrix[0]): %lu\n", sizeof(matrix[0]));    // 5 * 4 (int)
printf("sizeof(matrix[0][0]): %lu\n", sizeof(matrix[0])); // 4 (int)
printf("Number of rows: %d\n", rows);                     // 40 / 20 = 2
printf("Number of columns: %d\n", columns);               // 20 / 4 = 5

int(*ptrMatrix)[5] = matrix;

for (int i = 0; i < rows; i++)
for (int j = 0; j < columns; j++)
printf("matrix[%d][%d] Address: %p Value: %d\n", i, j,
&matrix[i][j], matrix[i][j]);

printf("-------------------------------\n");

for (int i = 0; i < rows; i++)
for (int j = 0; j < columns; j++)
printf("ptrMatrix[%d][%d] Address: %p Value: %d\n", i, j,
&ptrMatrix[i][j], matrix[i][j]);

return 0;
}
```

Output:

```bash
sizeof(matrix): 40
sizeof(matrix[0]): 20
sizeof(matrix[0][0]): 20
Number of rows: 2
Number of columns: 5
matrix[0][0] Address: 0x7ffd77649b80 Value: 1
matrix[0][1] Address: 0x7ffd77649b84 Value: 2
matrix[0][2] Address: 0x7ffd77649b88 Value: 3
matrix[0][3] Address: 0x7ffd77649b8c Value: 4
matrix[0][4] Address: 0x7ffd77649b90 Value: 5
matrix[1][0] Address: 0x7ffd77649b94 Value: 6
matrix[1][1] Address: 0x7ffd77649b98 Value: 7
matrix[1][2] Address: 0x7ffd77649b9c Value: 8
matrix[1][3] Address: 0x7ffd77649ba0 Value: 9
matrix[1][4] Address: 0x7ffd77649ba4 Value: 10
-------------------------------
ptrMatrix[0][0] Address: 0x7ffd77649b80 Value: 1
ptrMatrix[0][1] Address: 0x7ffd77649b84 Value: 2
ptrMatrix[0][2] Address: 0x7ffd77649b88 Value: 3
ptrMatrix[0][3] Address: 0x7ffd77649b8c Value: 4
ptrMatrix[0][4] Address: 0x7ffd77649b90 Value: 5
ptrMatrix[1][0] Address: 0x7ffd77649b94 Value: 6
ptrMatrix[1][1] Address: 0x7ffd77649b98 Value: 7
ptrMatrix[1][2] Address: 0x7ffd77649b9c Value: 8
ptrMatrix[1][3] Address: 0x7ffd77649ba0 Value: 9
ptrMatrix[1][4] Address: 0x7ffd77649ba4 Value: 10
```

- Please note that the array is stored in row-order i.e. first row is stored
sequentially in memory followed by a second row.
- A pointer can be declared to this matrix by `int (*ptrMatrix)[5] = matrix`.
- The expression `(*ptrMatrix)` declares a pointer to an array. It means that it
is defined as a pointer to a two dimensional array of integers with five
elements per column. If we had left the parentheses of i.e. `int* ptrMatrix[5]`,
we would have instead declared a five-element array of pointers to integers.

**It is very important to note that a two dimensional array notation can be
interpreted as: `arr[i][j] -> adress of arr + (i * size of row) + (j * size of
element)`**

#### Passing a multidimensional array

- When passing a multidimensional array you need to consider:
  - Whether to use array or pointer notation.
  - Arrays shape - The number and size of its dimensions.

- Prototype 1: `void display2DArray(int arr[][5], int rows)` (implicit
declaration of a pointer to an array)
- Prototype 2: `void display2DArray(int (*arr)[5], int rows)` (explicit
declaration of an pointer)
- Prototype 3: `void display2DArray(int *arr, int rows, int cols)`
- **Erroneous prototype:** `void display2DArray(int *arr[5], int rows)` - This
will generate a syntax error since the array passed is assumed to be a five
element array of pointers to integers.

In prototypes 1 and 2, the number of columns are specified. This is needed
because the compiler needs to know the number of elements in eah row. If this
information is not passed, then it is unable to evaluate expressions such as
`arr[0][3]`.

##### Example 1 - Passing a 2D array

```c
#include <stdio.h>

void display2DArray(int arr[][5], int rows) {
for (int i = 0; i < rows; i++)
for (int j = 0; j < 5; j++)
printf("arr[%d][%d] = %d\n", i, j, arr[i][j]);
}

int main() {
int matrix[2][5] = {{1, 2, 3, 4, 5}, {6, 7, 8, 9, 10}};
display2DArray(matrix, 2);
return 0;
}
```

##### Example 2 - Passing a 2D array

```c
#include <stdio.h>

void display2DArray(int *arr, int rows, int cols) {
for (int i = 0; i < rows; i++)
for (int j = 0; j < 5; j++)
printf("%d\n", *(arr + (i * cols) + j));
}

int main() {
int matrix[2][5] = {{1, 2, 3, 4, 5}, {6, 7, 8, 9, 10}};
display2DArray(&matrix[0][0], 2, 5);
return 0;
}
```

- The `printf` statement calculates the adress of each element by adding to
`arr` the number of elements in the previous row, and then adding `j` to specify
the column.
- To invoke the function, we need to pass the first elements adress. This can be
achieved by `&matrix[0][0]`. Passing only `matrix` could work but will most
likely give a warning.
- Please note that within the function `display2DArray`, we cannot use array
subscripts of type `arr[i][j]`. This is not possible to because the pointer is
not declared as a two-dimensional array. However, following notation is possible
`(arr + i)[j]` i.e. `printf("%d\n" arr[i][j])` will give an error while
`printf("%d\n", (arr + i)[j])` works fine.

##### Example 3 - Passing a 3D array

- When passing an array with more than 2 dimensions, all but the size of the
first dimension needs to be specified. See example below.

Example:

```c
#include <stdio.h>

void display3DArray(int (*arr)[2][4], int rows) {
for (int i = 0; i < rows; i++)
for (int j = 0; j < 2; j++)
for (int k = 0; k < 4; k++)
printf("%d\n", arr[i][j][k]);
}

int main() {
int arr3D[3][2][4] = {{{1, 2, 3, 4}, {5, 6, 7, 8}},
{{9, 10, 11, 12.}, {13, 14, 15, 16}},
{{17, 18, 19, 20}, {21, 22, 23, 24}}};
display3DArray(arr3D, 3);
return 0;
}
```

#### Dynamically allocating a two-dimensional array

Issues when dynamically allocating memory for a two dimensional array,
including:

- Whether the array elements need to be contiguous
- Whether the array is jagged
- Please note that memory is allocated contiguously when a two dimensional array
is declared as follows `int matrix[2][5] = {...}`. However, when using `malloc`
to create a two dimensional array, there are variations in how memory can be
allocated.
- Since a two dimensional array can be treated as an array of arrays, there is
no reason the "inner" array need to be contiguous.
- Whether or not memory is contiguous can affect other operations, such as
copying a block of memory. Multiple copies may be required if the memory is not
contiguous.

##### Allocating potentially noncontiguous memory

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
int rows = 2;
int columns = 5;

int **matrix = (int **)malloc(rows * sizeof(int *));

for (int i = 0; i < rows; i++)
matrix[i] = (int *)malloc(columns * sizeof(int));

for (int i = 0; i < rows; i++)
for (int j = 0; j < columns; j++)
printf("&matrix[%d][%d] = %p\n", i, j, &matrix[i][j]);

return 0;
}
```

Output:

```bash
&matrix[0][0] = 0x5595736a82c0
&matrix[0][1] = 0x5595736a82c4
&matrix[0][2] = 0x5595736a82c8
&matrix[0][3] = 0x5595736a82cc
&matrix[0][4] = 0x5595736a82d0
&matrix[1][0] = 0x5595736a82e0 <----- JUMP IN MEMORY LOCATION
&matrix[1][1] = 0x5595736a82e4
&matrix[1][2] = 0x5595736a82e8
&matrix[1][3] = 0x5595736a82ec
&matrix[1][4] = 0x5595736a82f0
```

##### Allocating contiguous memory

###### Contiguous memory: Technique 1

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
int rows = 2;
int columns = 5;

int **matrix = (int **)malloc(rows * sizeof(int *));
matrix[0] = (int *)malloc(rows * columns * sizeof(int));

for (int i = 0; i < rows; i++)
matrix[i] = matrix[0] + i * columns;

for (int i = 0; i < rows; i++)
for (int j = 0; j < columns; j++)
printf("&matrix[%d][%d] = %p\n", i, j, &matrix[i][j]);

return 0;
}
```

Output:

```bash
&matrix[0][0] = 0x55fae8db42c0
&matrix[0][1] = 0x55fae8db42c4
&matrix[0][2] = 0x55fae8db42c8
&matrix[0][3] = 0x55fae8db42cc
&matrix[0][4] = 0x55fae8db42d0
&matrix[1][0] = 0x55fae8db42d4
&matrix[1][1] = 0x55fae8db42d8
&matrix[1][2] = 0x55fae8db42dc
&matrix[1][3] = 0x55fae8db42e0
&matrix[1][4] = 0x55fae8db42e4
```

###### Contiguous memory: Technique 2

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
int rows = 2;
int columns = 5;

int *matrix = (int *)malloc(rows * columns * sizeof(int *));

for (int i = 0; i < rows; i++)
for (int j = 0; j < columns; j++) {
*(matrix + (i * columns) + j) = i * j;
printf("*(matrix + (%d * %d) + %d) = %d => (matrix + (%d * %d) + "
"%d) = %p\n",
i, columns, j, *(matrix + (i * columns) + j), i, columns, j,
(matrix + (i * columns) + j));
}

return 0;
}
```

Output:

```bash
*(matrix + (0 * 5) + 0) = 0 => (matrix + (0 * 5) + 0) = 0x5596718782a0
*(matrix + (0 * 5) + 1) = 0 => (matrix + (0 * 5) + 1) = 0x5596718782a4
*(matrix + (0 * 5) + 2) = 0 => (matrix + (0 * 5) + 2) = 0x5596718782a8
*(matrix + (0 * 5) + 3) = 0 => (matrix + (0 * 5) + 3) = 0x5596718782ac
*(matrix + (0 * 5) + 4) = 0 => (matrix + (0 * 5) + 4) = 0x5596718782b0
*(matrix + (1 * 5) + 0) = 0 => (matrix + (1 * 5) + 0) = 0x5596718782b4
*(matrix + (1 * 5) + 1) = 1 => (matrix + (1 * 5) + 1) = 0x5596718782b8
*(matrix + (1 * 5) + 2) = 2 => (matrix + (1 * 5) + 2) = 0x5596718782bc
*(matrix + (1 * 5) + 3) = 3 => (matrix + (1 * 5) + 3) = 0x5596718782c0
*(matrix + (1 * 5) + 4) = 4 => (matrix + (1 * 5) + 4) = 0x5596718782c4
```

### Pointers and strings

#### String fundamentals



### Pointers and memory

When a C program is complied, it works with three types of memory:

- **Static / Global**
  - Static / Global variables are allocated when the program starts and remain in
      existence until the program terminates.
  - All functions have acess to global variables.
  - The scope of static variables is restricted to their defining function.

- **Automatic**
  - Automatic variables are declared within a function and are created when the
      function's called.
  - Their scope is restricted to the function.
  - Their lifetime is limited to the time the function is executing.

- **Dynamic**
  - Memory is allocated from the heap and can be released when necessary.
  - A pointer references to the allocated memory (adress of the first byte of
      the allocated memory)
  - Scope is limited to the pointer that references the memory. It exists until
      it is released.

      |           | Scope                        | Lifetime                        |
      |-----------|------------------------------|---------------------------------|
      | Global    | The entire file              | The lifetime of the application |
      | Static    | The function where declrared | The lifetime of the application |
      | Automatic | The function where declared  | While the function executes     |
      | Dynamic   | Determined by pointer        | Until memory is freed           |

### Memory allocation

`malloc()` - Function that reserves a block of memory of the specified number of
bytes and it returns a pointer of `void` that can be casted into pointers of any
form.

syntax is `float *ptr = (float*)malloc(100 * sizeof(float))`. This means that
the function `malloc()` allocates 400 bytes of memory, this due to the reason
that a float has the size of 4 bytes. Also, the pointer `ptr` points / holds the
adress of the first byte in the allocated memory.

`calloc()` - contigous allocation - `calloc()` allocates memory and initializes
all the bits to zero, whereas, `malloc()` allocates memory and leaves the memory
uninitialized. Syntax: `float* ptr = (float*)calloc(25,sizeof(float))`.

`free()` - Release space i.e. `free(ptr)`.

`realloc()` - reallocates space with syntax: `ptr = realloc(ptr, 100 *
sizeof(float))` where `100 * sizeof(float) = 400` is the new memory size
allocated to pointer `ptr`.

If you allocate memory to a pointer. You must check that if there was enough
memory available i.e.

```c
char* str = (char*)malloc(100 * sizeof(char));

if (str == NULL)
printf("Out of memory\n");
```

### Pointers and functions

#### Single pointers

What is the output of following code,

```c
void fun(int* ptr) {
static int q = 10;
ptr = &q;
}

int main() {
int r = 20;
int* p = &r;
fun(p);
printf("*p = %d\n", *p);
return 0;
}
```

Output,

```bash
*p = 20
```

Inside the function `fun()`, `ptr` becomes a copy of the pointer `p`. If we
redirect the pointer `ptr` to point in a another direction, it wont change where
the original pointer `p` is pointing.

#### Double pointers

What is the poutput of the following function,

```c
#include <stdio.h>

void fun(int** ptr) {
static int anotherNumber = 20;
*ptr = &anotherNumber;
}

int main() {
int number = 10;
int* ptr = &number;
fun(&ptr);
printf("*ptr = %d\n", *ptr);
return 0;
}
```

Output:

`*ptr = 20`

### Relationships between double pointers, pointers and variables

#### Example 1 (double pointers)

![Relationships between pointers and variables](img/double_pointers_c.png)

As the pictures describes, following pointer relationship is,

```c
int x = 5;
int* y = &x;
int** z = &y;
```

This means that,

`**z`, `*y` and `x` returns number `5`.

`*z`, `y` points to the same position in memory i.e. the location where `x` is
stored (`&x`). So for example, if you point `*z` or `y` in another direction.
The other value will be affected.

#### Example 2 (double pointers)

```c
#include <stdio.h>

int main() {
int box = 5;
int *ptr = &box;
int **dPtr = &ptr;

printf("box holds: %d (box)\n", box);
printf("box lives at: %p (&box)\n", &box);
printf("ptr points to adress: %p (ptr)\n", ptr);
printf("The thing that ptr points to has the value: %d (*ptr)\n", *ptr);
printf("ptr lives at: %p (&ptr)\n", &ptr);
printf("dPtr points to: %p (dPtr)\n", dPtr);
printf("The thing that dPtr points to has the value %p (*dPtr)\n", *dPtr);
printf("the ptr that dPtr points to, points to an int with the value: %d "
"(**dPtr)\n",
**dPtr);
printf("dPtr lives at: %p (&dPtr)\n", &dPtr);

printf(
"\n\nThing:      dPtr                         ptr                     "
"    "
"box\n");
printf("Values:     [%p] (dPtr)----->[%p] (ptr)----->[%d] (box)\n", dPtr,
ptr, box);
printf("Adresses:   [%p] (&dPtr)     [%p] (&ptr)     [%p] (&box)\n", &dPtr,
&ptr, &box);

return 0;
}
```

gives the output:

```bash
box holds: 5 (box)
box lives at: 0x7ffcc42accf4 (&box)
ptr points to adress: 0x7ffcc42accf4 (ptr)
The thing that ptr points to has the value: 5 (*ptr)
ptr lives at: 0x7ffcc42accf8 (&ptr)
dPtr points to: 0x7ffcc42accf8 (dPtr)
The thing that dPtr points to has the value 0x7ffcc42accf4 (*dPtr)
the ptr that dPtr points to, points to an int with the value: 5 (**dPtr)
dPtr lives at: 0x7ffcc42acd00 (&dPtr)

Thing:      dPtr                         ptr                         box
Values:     [0x7ffcc42accf8] (dPtr)----->[0x7ffcc42accf4] (ptr)----->[5] (box)
<!-- markdownlint-disable-next-line -->
Adresses:   [0x7ffcc42acd00] (&dPtr)     [0x7ffcc42accf8] (&ptr)     [0x7ffcc42accf4] (&box)
```

### More double pointers

In C arrays always allocate memory on the stack, thus a function can't return a
(non-static) array due to the fact that memory allocated on the stack gets freed
automatically when the execution reaches the end of the current block. That's
really annoying when you want to deal with two-dimensional arrays (i.e. matrices)
and implement a few functions that can alter and return matrices. To achieve this,
you could use a pointer-to-pointer to implement a matrix with dynamically
allocated memory.

The double-pointer-to-double-pointer A points to the first element `A[0]` of a
memory block whose elements are double-pointers itself. You can imagine these
double-pointers as the rows of the matrix. That's the reason why every
double-pointer allocates memory for num_cols elements of type double. Furthermore
`A[i]` points to the i-th row, i.e. `A[i]` points to `A[i][0]` and that's just the
first double-element of the memory block for the i-th row. Finally, you can
access the element in the i-th row and j-th column easily with `A[i][j]`.

```bash
double**       double*           double
-------------       ---------------------------------------------------------
A ------> |   A[0]    | ----> | A[0][0] | A[0][1] | A[0][2] | ........ | A[0][cols-1] |
| --------- | ---------------------------------------------------------
| A[1]      | ---->                                                     | A[1][0]      | A[1][1]      | A[1][2] | ........          | A[1][cols-1] |
| --------- | ---------------------------------------------------------
| .         | .
| .         | .
| .         | .
| --------- | ---------------------------------------------------------
| A[i]      | ---->                                                     | A[i][0]      | A[i][1]      | A[i][2] | ........          | A[i][cols-1] |
| --------- | ---------------------------------------------------------
| .         | .
| .         | .
| .         | .
| --------- | ---------------------------------------------------------
| A[rows-1] | ---->                                                     | A[rows-1][0] | A[rows-1][1] | ...     | A[rows-1][cols-1] |
-------------       ---------------------------------------------------------
```

```c
#include <stdio.h>
#include <stdlib.h>

double **init_matrix(int num_rows, int num_cols);
void randn_fill_matrix(double **matrix, int rows, int cols);
void free_matrix(double **matrix, int rows, int cols);
void print_matrix(double **matrix, int rows, int cols);

int main() {
int rows = 3, cols = 3;
double **A = init_matrix(rows, cols);
randn_fill_matrix(A, rows, cols);
print_matrix(A, rows, cols);
free_matrix(A, rows, cols);
return 0;
}

double **init_matrix(int rows, int cols) {
double **matrix = calloc(rows, sizeof(double *));

if (matrix == NULL)
return NULL;

for (int i = 0; i < rows; i++) {
matrix[i] = calloc(cols, sizeof(double));

if (matrix[i] == NULL) {
for (int j = 0; j < i; j++)
free(matrix[j]);

free(matrix);
return NULL;
}
}
return matrix;
}

void randn_fill_matrix(double **matrix, int rows, int cols) {
for (int i = 0; i < rows; i++)
for (int j = 0; j < cols; j++)
matrix[i][j] = (double)(rand()) / RAND_MAX;
}

void free_matrix(double **matrix, int rows, int cols) {
for (int i = 0; i < rows; i++)
free(matrix[i]);
free(matrix);
}

void print_matrix(double **matrix, int rows, int cols) {
for (int i = 0; i < rows; i++) {
for (int j = 0; j < cols; j++)
printf(" %- f ", matrix[i][j]);
printf("\n");
}
}
```

## Strings

### Initization of strings

You can init strings in the following ways:

`char c[] = "abcd";`

or

`char c[] = {'a', 'b', 'c', 'd', '\0'};`

**Never do** `char c[5] = "abcd";`

Also following code is errourness:

`char c[50];`
`c = "C programming";`

You should instead use `strcpy()` to copy the string to the variable `c`.

### Read strings from stdin

`scanf` reads only a word or a number. As soon as you hit space. `scanf()` stops
to read. Its better then to use `fgets()` since it reads a line. You can then
use `puts()` to display the string.

### Strings and pointers

Folling code displays the relationship between strings and pointers:

```c
#include <stdio.h>

int main(void) {
char name[] = "Harry Potter";

printf("%c", *name);     // Output: H
printf("%c", *(name+1));   // Output: a
printf("%c", *(name+7));   // Output: o

char *namePtr;

namePtr = name;
printf("%c", *namePtr);     // Output: H
printf("%c", *(namePtr+1));   // Output: a
printf("%c", *(namePtr+7));   // Output: o
}
```

### Commonly used string functions

`strlen()` - Calculates the length of a string.
`strcpy()` - Copies a string to another.
`strcmp()` - Compares to strings.
`strcat`   - Concatenates two strings.

## Structs

### Initization of a struct variable

```c
struct Person {
char name[50];
int cityNumber;
float salary;
}

int main() {
// Initization of data type Person.
struct Person person1;

// You can declare a pointer pointing to the person 1.
struct Person *person1Ptr;
Person1Ptr = &person1;

person1.name = "Mikael";
person1.cityNumber = 100;
person1.salary = 100000;

// Inititization of a Person pointer and allocate memory to it.
struct Person* person2;
person2 = (struct Person*)malloc(sizeof(struct Person));
person2->name = "Jenny";
person2->cityNumber = 101;
person2->salary = 500000;
return 0;
}
```

### typedef

We use the typedef keyword to create an alias name for data types. It is
commonly used with structures to simplify the syntax of declaring variables.
Please see the examples:

```c
typedef struct Distance{
int feet;
float inch;
} distances;

int main() {
distances d1, d2;
}
```

or

```c
typedef struct {
int feet;
float inch;
} distances;

int main() {
distances d1, d2;
}
```

## Stack vs. heap

[Stack vs. Heap on Stackoverflow](
<https://stackoverflow.com/questions/79923/what-and-where-are-the-stack-and-heap>
)
