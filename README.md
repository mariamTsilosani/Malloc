# Malloc

## Mariam Tsilosani

### Design

###### The Malloc program was built to imitate the behaviour of Malloc and how it distributes memory, except instead of using memory from the heap it uses a character array allocated on the stack. It is designed with the hope of reducing fragmentation as much as possible. One solution to the fragmentation is to use three separate blocks and allocate memory from them based on the sizes of the memory being asked for when the malloc() function is called. This will reduce fragmentation by keeping the smaller and larger chunks of memory in different arrays.

### Implementation

###### The Malloc uses a linked list of memEntry structs to manage how much memory is allocated and how much there is remaining etc. It has a lot of special features to improve it's usability and flexibility for different edge cases.

          1. Leak detection and reporting. I used atexit(function pointer) to go through each array and see if there are any pointers
          left that have not been freed, if said pointer exists it prints out an error message and the LINE and FILE the pointer was
          allocated at.
          
          2.	calloc(size_t nmemb , size_t size ) - Calloc() allocates new memory and zeroes it out before returning it back to the user.
          
          3.	realloc(void *ptr, size_t size) - Realloc() If the new size is smaller than pointer size it shrinks the pointer size and gives the left over size back to the array. If the new size is bigger than the pointer size it checks if the memory around the received pointer is free, if it is then it combines that memory with the memory pointer was pointing to, else, it frees the pointer and goes to find more memory in the arrays.
          
          4.	Corruption detection - I use a hex number that every memEntry struct has at the beginning, every-time a pointer is received back I check if the hex number is still there, if its not then it means it has been overwritten, corruption has been detected, the program exits.
          
          5.	Freeing non Heap Variables with free(void *ptr) will give an error message if the variable being freed has not been allocated on the heap.
          
          6.	free()ing some pointer that was not returned by previous malloc()/realloc() call. A little harder. Somebody has done some bogus pointer arithmetic and tries to return an altered pointer to somewhere in the heap.
          
          7.	Fragmentation reduction - There are three separate arrays as a means to reduce the amount of fragmentation because I can keep similarly sized blocks together.
          
### Usage

#### The API for the malloc family of functions is below.

##### Malloc comes with a very extensive Makefile that allows you to test individual functions in Main.c by using Conditional Compilation. You can test each specific function using make <option>. Where the options are the following.

          1. make will compile main.c and create an object file called my_malloc that has linked together malloc.c and malloc.h
          
          2. make run will compile and run main without debugging flags or any additional debugging information. This means that main can be edited with additional test-cases if desired and can then be very quickly observed by using the command again.
          
          3. make debug will compile and run main.c with all debugging flags enables that will test all functions extensively.
          
          4. other <options> include items such as make call which will compile main with only the test cases for calloc() and run them. The other options are reall, leak, and corrupt.
