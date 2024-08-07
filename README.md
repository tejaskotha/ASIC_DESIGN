# ASIC_DESIGN
## Lab-1
### Description
In this lab we compile a simple C program using GCC and RISCV compliers.Then we compare the outputs
### Tasks
1.  Writing a simple C program to calculate sum of n numbers and execute the program using GCC.
2.  Execute the same C program using RISCV compiler.
3.  Compare the results.
### Procedure
1. Create a new file(sum1ton.c) in home directory.(Run the command 'cd' before creating the file to make sure you are in home directory).
2. We use leafpad as texteditor to write the code.
   ### Code
   ```c
   #include <stdio.h>
   int main(){
     int i,sum=0,n=100;
     for(i=1;i<=n;i++){
       sum+=i;
     }
     printf("Sum of numbers from 1 to %d is %d\n",n,sum);
     return 0;
   }
   ### To compile the code using GCC
   ```c
   gcc sum1ton.c
   
