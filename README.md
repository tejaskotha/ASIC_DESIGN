# ASIC_DESIGN
<details>
 <summary><strong>Lab-1</strong></summary>
 
 ### Description
   In this lab we compile a simple C program using GCC and RISCV compliers.
 ### Tasks
1.  Writing a simple C program to calculate sum of n numbers and execute the program using GCC.
2.  Execute the same C program using RISCV compiler and compare the results.
## Procedure
 ### Task-1
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
   ```
   ### To compile the code using GCC
   ```
   gcc sum1ton.c
   ```

   ### To Run the code
   ```
   ./a.out
   ```
   ### Output
   ![Output of the code](images/gcc_op.jpg)
### Task-2 : Compliling and verifying the same C code using RISC-V compiler
  1. Compile the code using RISV using the below command
     ```
     riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
     ```
  2. To run the code
     ```
     riscv64-unknown-elf-objdump -d sum1ton.o | less
     ```
  3. Output
     ![Ouput of RISCV](images/riscv_op.jpg)
  ### Explanation of the Output:
  - 0000000000010184 is the address where the main function starts.
  - To known the number of instructions it takes to execute we subtract 0000000000010184(which is the next function) from 00000000000101c0 and divide it by 4(Since each instruction takes 4 bytes).
  - In this case it take 15 instructions.
  - We can reduce the number of instructions it takes by compiling the code using the below command.
     ```
     riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
     ```
     ![Output of riscv](images/riscvfast_op.jpg)
    - Run the code using the same code as above.
    - In the above case it is taking only 12 instructions to execute.

</details>


<details>
 <summary><strong>Lab-2</strong></summary>

  ### Description
  Comparing the GCC compiled output with RISCV compiled output
  ### Procedure
  1. Compile the code using the below command.
     ```
     riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
     ```
  2. Run the code
     ```
     spike pk sum1ton.o
     ```
  3. Output
     ![Output of riscv](images/3.png)
  ### Debugging the Code
  - We debug the code using the below command
    ```
    spike -d pk sum1ton.o
    ```
  - First run the RISCV code(`riscv64-unknown-elf-objdump -d sum1ton.o | less`).We can get the location of main function from where we can debug the code line by line.
    #### Output
    ![Output](images/4.png)
    
  - Use the below command to take pc to the main function .(100b0 is the starting point of the main function).
    ```
    until pc 0 100b0
    ```
  - We can know the initial value of a2 before executing the instruction and obsereve the change in the value of a2 after executing the instruction.
  - To know the value of a2 use the below command.Press enter the run the following instructions.
     ```
     reg 0 a2
     ```
     ![Output](images/5.png)
  - lui - Load Upper immediate.
  - We can observe the value of a2 is changed.
  - Similarily, we can also observe the working of the other RISCV instruction i.e addi
  - To observe the working of addi, first move the pc to 100b8(which means program is executed till the instruction 100b8).Then observe the change in the value of sp(-16 in decimal is 10 in hexadecimal).We can observe the value of sp is decreased by 10.
    ![output](images/6.png)
### References
[RISCV-INSTRUCTIONS](https://riscv.org/wp-content/uploads/2017/05/riscv-spec-v2.2.pdf) 
</details>


<details>
 <summary><strong>Lab-3</strong></summary>.

   ## Description
   To identify various RISC-V instruction type (R, I, S, B, U, J) and determine exact 32-bit instruction code in the instruction type format for below RISC-V instructions. 
   ## Theory
   - The RISC-V architecture employs six main instruction formats—R-Type, I-Type, S-Type, B-Type, U-Type, and J-Type—each used for specific operations like arithmetic, logic, immediate value processing, branching, memory access, and jumps respectively.
  - Instruction format for different RISC-V instructions:
  
  ![ins](images/ins.png)

- Instruction set table for predefined values:
  
  ![ins2](images/ins2.png)

     
      
   ## Instructions
   ```
    ADD r6, r7, r8
    SUB r8, r6, r7
    AND r7, r6, r8
    OR r8, r7, r5
    XOR r8, r6, r4
    SLT r10, r2, r4
    ADDI r12, r3, 5
    SW r3, r1, 4
    SRL r16, r11, r2
    BNE r0, r1, 20
    BEQ r0, r0, 15
    LW r13, r11, 2
    SLL r15, r11, r2
   ```
   ## Detailed Instruction Breakdown

  -  ```ADD r6, r7, r8```
     - **Opcode for ADD**: `0110011`
     - **rd** = `r6` = `00110`
     - **rs1** = `r7` = `00111`
     - **rs2** = `r8` = `01000`
     - **func3** = `000`
     - **func7** = `0000000`
     - **Instruction Type**: R Type
     - **32-bit Instruction**: `0000000_01000_00111_000_00110_0110011`
     - **Hexadecimal representation**: ` 0x00838333`
    
  -  ```SUB r8, r6, r7```
     - **Opcode for SUB**: `0110011`
     - **rd** = `r8` = `01000`
     - **rs1** = `r6` = `00110`
     - **rs2** = `r7` = `00111`
     - **func3** = `000`
     - **func7** = `0100000`
     - **Instruction Type**: R Type
     - **32-bit Instruction**: `0100000_00111_00110_000_01000_0110011`
     - **Hexadecimal representation**: `0x40730433`
    
  - ```AND r7, r6, r8```
    
     - **Opcode for AND**: `0110011`
     - **rd** = `r7` = `00111`
     - **rs1** = `r6` = `00110`
     - **rs2** = `r8` = `01000`
     - **func3** = `111`
     - **func7** = `0000000`
     - **Instruction Type**: R Type
     - **32-bit Instruction**: `0000000_01000_00110_111_00111_0110011`
     - **Hexadecimal representation**: `0x008373B3`
   
  - ```OR r8, r7, r5```
     - **Opcode for OR**: `0110011`
     - **rd** = `r8` = `01000`
     - **rs1** = `r7` = `00111`
     - **rs2** = `r5` = `00101`
     - **func3** = `110`
     - **func7** = `0000000`
     - **Instruction Type**: R Type
     - **32-bit Instruction**: `0000000_00101_00111_110_01000_0110011`
     - **Hexadecimal representation**: `0x0053E433`
   
  - ```XOR r8, r6, r4```
    - **Opcode for XOR**: `0110011`
    - **rd** = `r8` = `01000`
    - **rs1** = `r6` = `00110`
    - **rs2** = `r4` = `00100`
    - **func3** = `100`
    - **func7** = `0000000`
    - **Instruction Type**: R Type
    - **32-bit Instruction**: `0000000_00100_00110_100_01000_0110011`
    - **Hexadecimal representation**: `0x00434433`
   
  -  ```SLT r10, r2, r4```
     - **Opcode for SLT**: `0110011`
     - **rd** = `r10` = `01010`
     - **rs1** = `r2` = `00010`
     - **rs2** = `r4` = `00100`
     - **func3** = `010`
     - **func7** = `0000000`
     - **Instruction Type**: R Type
     - **32-bit Instruction**: `0000000_00100_00010_010_01010_0110011`
     - **Hexadecimal representation**: `0x00412533`
     
  - ```ADDI r12, r3, 5```
   
    - **Opcode for ADDI**: `0010011`
    - **rd** = `r12` = `01100`
    - **rs1** = `r3` = `00011`
    - **imm[11:0]** = `000000000101`
    - **func3** = `000`
    - **Instruction Type**: I Type
    - **32-bit Instruction**: `000000000101_00011_000_01100_0010011`
    - **Hexadecimal representation**: `0x00518613`
   
   - ```SW r3, r1, 4```
   
     - **Opcode for SW**: `0100011`
     - **imm[11:5]** = `0000000`
     - **rs2** = `r3` = `00011`
     - **rs1** = `r1` = `00001`
     - **imm[4:0]** = `00100`
     - **func3** = `010`
     - **Instruction Type**: S Type
     - **32-bit Instruction**: `0000000_00011_00001_010_00100_0100011`
     - **Hexadecimal representation**: `0x0030A223`
   
  - ```SRL r16, r11, r2```
   
    - **Opcode for SRL**: `0110011`
    - **rd** = `r16` = `10000`
    - **rs1** = `r11` = `01011`
    - **rs2** = `r2` = `00010`
    - **func3** = `101`
    - **func7** = `0000000`
    - **Instruction Type**: R Type
    - **32-bit Instruction**: `0000000_00010_01011_101_10000_0110011`
    - **Hexadecimal representation**: `0x0025D833`
   
  - ```BNE r0, r1, 20```
   
    - **Opcode for BNE**: `1100011`
    - **imm[12|10:5]** = `0000010`
    - **rs2** = `r1` = `00001`
    - **rs1** = `r0` = `00000`
    - **imm[4:1|11]** = `10100`
    - **func3** = `001`
    - **Instruction Type**: B Type
    - **32-bit Instruction**: `0000010_00001_00000_001_10100_1100011`
    - **Hexadecimal representation**: `0x04101A63`
   
  - ```BEQ r0, r0, 15```
   
    - **Opcode for BEQ**: `1100011`
    - **imm[12|10:5]** = `0000000`
    - **rs2** = `r0` = `00000`
    - **rs1** = `r0` = `00000`
    - **imm[4:1|11]** = `01111`
    - **func3** = `000`
    - **Instruction Type**: B Type
    - **32-bit Instruction**: `0000000_00000_00000_000_01111_1100011`
    - **Hexadecimal representation**: `0x000007E3`
   
  - ```LW r13, r11, 2```
   
    - **Opcode for LW**: `0000011`
    - **rd** = `r13` = `01101`
    - **rs1** = `r11` = `01011`
    - **imm[11:0]** = `000000000010`
    - **func3** = `010`
    - **Instruction Type**: I Type
    - **32-bit Instruction**: `000000000010_01011_010_01101_0000011`
    - **Hexadecimal representation**: `0x0025A683`
   
  - ```SLL r15, r11, r2```
    
    - **Opcode for SLL**: `0110011`
    - **rd** = `r15` = `01111`
    - **rs1** = `r11` = `01011`
    - **rs2** = `r2` = `00010`
    - **func3** = `001`
    - **func7** = `0000000`
    - **Instruction Type**: R Type
    - **32-bit Instruction**: `0000000_00010_01011_001_01111_0110011`
    - **Hexadecimal representation**: `0x002597B3`
 
 # RISC-V Instruction Summary Table



| Instruction        | Type | 32-bit Binary Representation         | Hexadecimal Representation |
|--------------------|------|--------------------------------------|----------------------------|
| `ADD r6, r7, r8`   | R    | `0000000_01000_00111_000_00110_0110011` | `0x00838333` |
| `SUB r8, r6, r7`   | R    | `0100000_00111_00110_000_01000_0110011` | `0x40730433` |
| `AND r7, r6, r8`   | R    | `0000000_01000_00110_111_00111_0110011` | `0x008373B3` |
| `OR r8, r7, r5`    | R    | `0000000_00101_00111_110_01000_0110011` | `0x0053E433` |
| `XOR r8, r6, r4`   | R    | `0000000_00100_00110_100_01000_0110011` | `0x00434433` |
| `SLT r10, r2, r4`  | R    | `0000000_00100_00010_010_01010_0110011` | `0x00412533` |
| `ADDI r12, r3, 5`  | I    | `000000000101_00011_000_01100_0010011` | `0x00518613` |
| `SW r3, r1, 4`     | S    | `0000000_00011_00001_010_00100_0100011` | `0x0030A223` |
| `SRL r16, r11, r2` | R    | `0000000_00010_01011_101_10000_0110011` | `0x0025D833` |
| `BNE r0, r1, 20`   | B    | `0000010_00001_00000_001_10100_1100011` | `0x04101A63` |
| `BEQ r0, r0, 15`   | B    | `0000000_00000_00000_000_01111_1100011` | `0x000007E3` |
| `LW r13, r11, 2`   | I    | `000000000010_01011_010_01101_0000011` | `0x0025A683` |
| `SLL r15, r11, r2` | R    | `0000000_00010_01011_001_01111_0110011` | `0x002597B3` |

</details>

<details>
<summary><strong>Lab-4</strong></summary>
 
 ## Description
 Functional simulation experiment
 ## Standard RISCV ISA AND Hardcoded ISA(based on given verilog code)
 - In the Verilog code, each instruction type is assigned a unique opcode, and the func3 and func7 fields have distinct values compared to the standard RISC-V values. The func7 field is primarily used to differentiate immediate operations from other arithmetic functions; if it’s not serving this purpose, func7 is set to b'0 in the Verilog implementation.
 - Below is the table with hardcoded 32-bit code using func3,func7 and opcodes from the given verilog code.

| S.No | Instruction        | Hardcoded 32-bit Pattern             | Hardcoded Hexadecimal Pattern | 32-bit Pattern                        | Hexadecimal Pattern |
|------|--------------------|--------------------------------------|--------------------------------|---------------------------------------|---------------------|
| 1    | ADD r6,r7,r8        | 0000001_01000_00111_000_00110_0000000 | 0x02838300                     |0000000_01000_00111_000_00110_0110011 | 0x00838333          |
| 2    | SUB r8,r6,r7        | 0000001_00111_00110_001_01000_0000000 | 0x02731400                    |0100000_00111_00110_000_01000_0110011 | 0x40730433         |
| 3    | AND r7, r6, r8      | 0000001_01000_00110_010_00111_0000000 | 0x02832380                     |0000000_01000_00110_111_00111_0110011 | 0x008373B3          |
| 4    | OR r8, r7, r5       | 0000001_00101_00111_011_01000_0000000 | 0x0253B400                     | 0000000_00101_00111_110_01000_0110011 | 0x0053E433          |
| 5    | XOR r8, r6, r4      | 0000001_00100_00110_100_01000_0000000 | 0x02434400                     | 0000000_00100_00110_100_01000_0110011 | 0x00434433          |
| 6    | SLT r10, r2, r4     | 0000001_00100_00010_101_01010_0000000 | 0x02415500                     | 0000000_00100_00010_010_01010_0110011 | 0x00412533          |
| 7    | ADDI r12, r3, 5     | 000000000101_00011_000_01100_0000000 | 0x00518600                     | 0000000_000101_00011_000_01100_0010011 | 0x00518613         |
| 8    | SW r3, r1, 4        | 0000000_00001_00011_001_00100_0000001 | 0x00119201                     | 0000000_00001_00011_010_00100_0100011 | 0x0030A223          |
| 9    | SRL r16, r11, 2      | 0000000_00010_01011_001_10000_0000011 | 0x00259803                      | 0000000_00010_01011_101_10000_0110011 | 0x0025D833          |
| 10   | BEQ r0, r0, 15    | 0_000000_01111_00000_000_0000_0_0000010 | 	0X00f00002                     | 0000000_00000_00000_000_01111_1100011 | 0x000007E3          |
| 11   | LW r13, r11, 2     |  000000000010_01011_000_01101_0000001|   	0x00258681                       |0000000_00010_01011_010_01101_0000011 |0x0025A683 |



## Instructions provided in the verilog code
![41](images/41.png)
| S. No. | Operation          | Standard RISC-V ISA | Hardcoded ISA |
|--------|--------------------|---------------------|---------------|
| 1      | ADD R6, R1, R2      | 0x00110333          | 0x02208300    |
| 2      | SUB R7, R1, R2      | 0x402083b3          | 0x02209380    |
| 3      | AND R8, R1, R3      | 0x0030f433          | 0x0230a400    |
| 4      | OR R9, R2, R5       | 0x005164b3          | 0x02513480    |
| 5      | XOR R10, R1, R4     | 0x0040c533          | 0x0240c500    |
| 6      | SLT R11, R2, R4     | 0x0045a0b3          | 0x02415580    |
| 7      | ADDI R12, R4, 5     | 0x004120b3          | 0x00520600    |
| 8      | BEQ R0, R0, 15      | 0x00000f63          | 0x00f00002    |
| 9      | SW R3, R1, 2        | 0x0030a123          | 0x00209181    |
| 10     | LW R13, R1, 2       | 0x0020a683          | 0x00208681    |
| 11     | BEQ R0, R0, 15      | 0x00000f63          | 0x00f00002    |
| 12     | ADD R14, R2, R2     | 0x00210733          | 0x00210700    |

## Below are the output waveforms for each instruction from testbench.
  ```1.  ADD r6, r1, r2 ```
   
   ![42](images/42.png)


   ```2. SUB r7, r1, r2 ```

   ![43](images/43.png)



   ```3.  AND r8, r1, r3 ```

   ![44](images/44.png)


   ```4. OR r9, r2, r5 ```

   ![45](images/45.png)


   ```5. XOR r10, r1, r4 ```

   ![46](images/46.png)

  ```6. SLT r11, r2, r4 ```

   ![47](images/47.png)


   ```7. ADDI r12, r4, 5 ```

   ![48](images/48.png)


   ```8. SW r3, r1, 2 ```

   ![49](images/49.png)


   ```9. LW r0, r0, 15 ```

   ![410](images/410.png)


   ```10. BEQ r0, r0, 15 ```

   ![411](images/411.png)


   ```11. ADD r14, r2, r2```
   
   ![412](images/412.png)

   The final output plot is :

   ![413](images/413.png)



</details>
 
<details>
 <summary><strong>Lab-5</strong></summary>
 
 ### Description
   We compile a C program using GCC and RISCV compliers.
 ### Tasks
1.  This C program simulates a basic vending machine. It allows a user to display available items, purchase an item by selecting it from the menu, and handle transactions, including calculating the total cost and change. It uses a menu-driven interface to interact with the user.
2. Compile the program using GCC.
3.  Execute the same C program using RISCV compiler and compare the results.
### Procedure
 ### Code
 ```c
#include <stdio.h>

#define MAX_ITEMS 5

typedef struct {
    char name[30];
    double price;
    int quantity;
} Item;

void display_items(Item items[], int num_items);
void purchase_item(Item items[], int num_items);

int main() {
    Item items[MAX_ITEMS] = {
        {"Soda", 1.25, 10},
        {"Chips", 1.00, 15},
        {"Candy", 0.75, 20},
        {"Water", 1.50, 10},
        {"Juice", 1.75, 8}
    };

    int choice;

    while (1) {
        printf("\nVending Machine\n");
        printf("-----------------\n");
        printf("1. Display Items\n");
        printf("2. Purchase Item\n");
        printf("3. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                display_items(items, MAX_ITEMS);
                break;
            case 2:
                purchase_item(items, MAX_ITEMS);
                break;
            case 3:
                printf("Exiting...\n");
                return 0;
            default:
                printf("Invalid choice. Please try again.\n");
        }
    }

    return 0;
}

void display_items(Item items[], int num_items) {
    printf("\nAvailable Items:\n");
    printf("ID\tName\tPrice\tQuantity\n");
    printf("----------------------------------\n");

    for (int i = 0; i < num_items; i++) {
        printf("%d\t%s\t$%.2f\t%d\n", i + 1, items[i].name, items[i].price, items[i].quantity);
    }
}

void purchase_item(Item items[], int num_items) {
    int item_id;
    int quantity;
    double payment;

    printf("Enter the item ID you want to purchase: ");
    scanf("%d", &item_id);

    if (item_id < 1 || item_id > num_items) {
        printf("Invalid item ID.\n");
        return;
    }

    Item *selected_item = &items[item_id - 1];

    if (selected_item->quantity == 0) {
        printf("Sorry, %s is out of stock.\n", selected_item->name);
        return;
    }

    printf("Enter the quantity you want to purchase: ");
    scanf("%d", &quantity);

    if (quantity > selected_item->quantity) {
        printf("Sorry, we don't have enough %s in stock.\n", selected_item->name);
        return;
    }

    double total_cost = quantity * selected_item->price;
    printf("The total cost is $%.2f\n", total_cost);
    printf("Enter your payment amount: ");
    scanf("%lf", &payment);

    if (payment < total_cost) {
        printf("Insufficient payment. Transaction canceled.\n");
        return;
    }

    double change = payment - total_cost;
    selected_item->quantity -= quantity;
    printf("Purchase successful. Your change is $%.2f\n", change);
}
```
   ### To compile the code using GCC
   ```
   gcc vending.c
   ```

   ### To Run the code
   ```
   ./a.out
   ```
   ### Output
   ![Output of the code](images/v1.png)

   ### To compile the code using RISCV
   ```
   riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o vending.o vending.c
   ```
   ### To run the code
   ```
    spike pk vending.o
   ```
   ### Output
   
   ![Ouput of RISCV](images/v2.png)

   ### Conclusion
   We can see that outputs are same when compiled using both GCC and RISCV.

</details>

<details>
<summary><strong>Lab-6</strong></summary>
<details>
<summary><strong>Day3</strong></summary>

## Digital Logic Design and TL-Verilog with Makerchip:

### Logic Gates: 
Logic gates are the building blocks of digital circuits. They process binary data (0s and 1s) to perform logical operations. These operations are essential for creating complex electronic systems like computers and microprocessors. Logic gates receive input signals and produce output signals based on specific rules or functions.

Here are some common types of logic gates:
![logic gates](images/61.png)


### Multiplexer Using Ternary Operator: 

**2:1 Multiplexer:** Below Verilog code uses a ternary operator to implement a 2:1 multiplexer. The output f follows x1 when s is high (1) and x0 when s is low (0).

```
assign f = s ? x1 : x0;
```
Below code translates into hardware that selects between x1 and x0 based on the value of s.

![2:1mux](images/62.png)


**4:1 Multiplexer:**  A higher-bit multiplexer can be implemented using nested ternary operators, as demonstrated in the Verilog code below:
```
assign f = sel[0] ? a : (sel[1] ? b : (sel[2] ? c : d));
```
This code efficiently constructs a priority-encoded series of 2:1 multiplexers. In this setup, the sel vector is one-hot, meaning only one bit is high at a time, determining the selection among inputs a, b, c, and d.

![4:1mux](images/63.png)

### Makerchip IDE: 

Makerchip IDE is a robust tool for digital design, providing an integrated environment for coding, simulating, and testing HDL designs. It supports languages such as TL-Verilog, SystemVerilog, Verilog, and VHDL, offering a visual platform to construct and simulate digital systems in real-time. With its user-friendly interface and comprehensive features, Makerchip is well-suited for both beginners and seasoned designers. It allows you to prototype, debug, and fine-tune your digital designs efficiently, ensuring that your circuits perform correctly before transitioning to hardware implementation.

### Basic Combinational Circuits in Makerchip:
---

#### 1. Inverter
Code
```tl-verilog
$out = ! $in;
```
Block diaagram and waveforms

![not](images/64.png)


#### 2. Arithmetic Operation on Vectors
Code
```tl-verilog
$out[4:0] = $in1[3:0] + $in2[3:0];
```
Block diaagram and waveforms

![arith](images/65.png)


#### 3. 2-Input-And Gate
Code
```tl-verilog
$out = $in1 && $in2;
```
Block diaagram and waveforms

![and](images/66.png)

#### 3. 2-Input-OR Gate
Code
```tl-verilog
$out = $in1 || $in2;
```
Block diaagram and waveforms

![OR](images/67.png)

#### 4. 2-Input-XOR Gate
Code
```tl-verilog
$out = $in1 ^ $in2;
```
Block diaagram and waveforms

![XOR](images/68.png)

#### 6. 2:1 MUX
Code
```tl-verilog
$out[11:0] = $sel ? $in1[11:0] : $in0[11:0];
```
Block diaagram and waveforms

![21mux](images/69.png)

#### 7. Combinational Calculator Implementation in TL-Verilog

**Calculator Overview:**
In this section, we present a simple combinational calculator implemented using TL-Verilog on the Makerchip platform. The calculator performs four basic arithmetic operations: addition, subtraction, multiplication, and division.

```tl-verilog
$val1[31:0] = $rand1[3:0];
$val2[31:0] = $rand2[3:0];

$sum[31:0]  = $val1[31:0] + $val2[31:0];
$subt[31:0] = $val1[31:0] - $val2[31:0];
$mul[31:0] = $val1[31:0] * $val2[31:0];
$div[31:0] = $val1[31:0] / $val2[31:0];

$out[31:0] = ($sel[1:0] == 2'b00) ? $sum[31:0]:
             ($sel[1:0] == 2'b01) ? $subt[31:0]:
             ($sel[1:0] == 2'b10) ? $mul[31:0]:
                                    $div[31:0];
```
**Description:** 

In this code snippet, two random 4-bit values, `$rand1[3:0]` and `$rand2[3:0]`, are assigned to 32-bit variables `$val1[31:0]` and `$val2[31:0]`, respectively. The calculator then performs four arithmetic operations on these values. A multiplexer (MUX), controlled by the selection bits `$sel[1:0]`, chooses the result of one of these operations to be assigned to `$out[31:0]`.

Block diaagram and waveforms

![cal](images/610.png)


### Sequential Circuits in Makerchip:
---
A sequential circuit is a digital circuit that incorporates memory components to store data, enabling it to produce outputs based on both current inputs and the circuit's previous state. Unlike combinational circuits, which rely solely on present inputs, sequential circuits use feedback loops and storage elements like flip-flops or registers to maintain an internal state. This internal state, combined with current inputs, determines the circuit's behavior, allowing it to perform tasks that require tracking input history, such as counting, data storage, or event sequencing.

#### 1. Fibbonacci Series:

Code:

```tl-verilog

$num[31:0] = $reset ? 1 : (>>1$num + >>2$num);

```
Block diaagram and waveforms

![cal](images/611.png)
![cal](images/612.png)

#### 2. Counter Series:

Code:

```tl-verilog

$cnt[31:0] = $reset ? 0 : (>>1$cnt + 1);

```
The generated block diagram and waveforms are shown below:

![counter](images/614.png)

![counter](images/613.png)

#### 3.Sequential Calculator:

- Similar to the previous combinational calculator, this version simulates a real-world scenario where the result of the previous operation is used as one of the inputs for the next operation. Upon reset, the result is set to zero.

Code is given below:
```tl-verilog

   $val1[31:0] = >>2$out;
   $val2[31:0] = $rand2[3:0];

   $sum[31:0]  = $val1[31:0] + $val2[31:0];
   $subt[31:0] = $val1[31:0] - $val2[31:0];
   $mul[31:0] = $val1[31:0] * $val2[31:0];
   $div[31:0] = $val1[31:0] / $val2[31:0];

   $nxt[31:0] = ($sel[1:0] == 2'b00) ? $sum[31:0]:
                ($sel[1:0] == 2'b01) ? $subt[31:0]:
                ($sel[1:0] == 2'b10) ? $mul[31:0]:
                                       $div[31:0];
   
   $out[31:0] = $reset ? 32'h0 : $nxt;

```
Blockd diagram and waveforms

![d3_14](images/615.png)

![d3_13](images/616.png)

### Pipelined Logic:
---

In Transaction-Level Verilog (TL-Verilog), pipelined logic is effectively expressed using pipeline constructs that represent the flow of data through design stages, each aligned with a clock cycle. This method simplifies the modeling of sequential logic by automatically managing state propagation, allowing for clear and concise descriptions of complex, multi-stage operations, ultimately enhancing design clarity and maintainability.

#### 1. Recereating the design:
![d3_13](images/617.png)


Code:
```tl-verilog
|pipe
  @1
    $err1 = $bad_input || $illegal_op;
  @3
    $err2 = $over_flow || $err1;
  @6
    $err3 = $div_by_zero || $err2;
```
The generated block diagram and waveforms are as shown:
![d3_13](images/618.png)
So you can observe that the given design of pipeline and the recreated design are same.

#### 2. Pipelined Calculator:

- Similar to previous Sequential Calculator but with a pipelined design and using ```$valid``` inorder to clear alternate values.

Code is given below:
```tl-verilog
   |cal
      @1
         $reset = *reset;
         $clk_tej = *clk;

         $valid[31:0] = $reset ? 0 : (>>1$valid + 1);
         $nreset = $reset | ~$valid;
         
         $val1[31:0] = >>2$out;
         $val2[31:0] = $rand2[3:0];

         $sum[31:0]  = $val1[31:0] + $val2[31:0];
         $diff[31:0] = $val1[31:0] - $val2[31:0];
         $prod[31:0] = $val1[31:0] * $val2[31:0];
         $quot[31:0] = $val1[31:0] / $val2[31:0];
         
      @2
         $nxt[31:0] = ($sel[1:0] == 2'b00) ? $sum[31:0]:
                      ($sel[1:0] == 2'b01) ? $diff[31:0]:
                      ($sel[1:0] == 2'b10) ? $prod[31:0]:
                                             $quot[31:0];
        
         
         $out[31:0] = $nreset ? 32'h0 : $nxt;
         
   
```
The generated block diagram and waveforms are as shown:

![d3_13](images/619.png)
![d3_13](images/620.png)
#### 3. Cycle Calculator with validity:

- we add ```$valid_or_reset = $valid || $reset;``` as a when condition for calculation instead of zeroing ```$out```.

Code is given below:

```tl-verilog

   $reset = *reset;
   $clk_tej = *clk;
   
   |cal
      @1
         $reset = *reset;
         $clk_tej = *clk;
         
         $cnt[31:0] = $reset ? 0 : (>>1$cnt + 1);
         $valid = $cnt;
         $valid_or_reset = ($reset | $valid);
         
      
      ?$valid
         @1
            $val1[31:0] = >>2$out;
            $val2[31:0] = $rand2[3:0];
            
            $sum[31:0]  = $val1[31:0] + $val2[31:0];
            $diff[31:0] = $val1[31:0] - $val2[31:0];
            $prod[31:0] = $val1[31:0] * $val2[31:0];
            $quot[31:0] = $val1[31:0] / $val2[31:0];
            
         @2
            $nxt[31:0] = ($sel[1:0] == 2'b00) ? $sum[31:0]:
                         ($sel[1:0] == 2'b01) ? $diff[31:0]:
                         ($sel[1:0] == 2'b10) ? $prod[31:0]:
                                                $quot[31:0];
            
            $out[31:0] = $valid_or_reset ? 32'h0 : $nxt;

```

block diagram and waveforms
![d3_13](images/621.png)
</details>
<details>
<summary><strong>Day4</strong></summary>

## Basic RISC-V CPU Micro-architecture

-This section will discuss the implementation of a straightforward 3-stage RISC-V CPU core. The three main stages are: Fetch, Decode, and Execute. The following figure illustrates the basic block diagram of the CPU core:

<img width="536" alt="Screenshot 2024-08-21 at 2 29 38 AM" src="https://github.com/user-attachments/assets/34e0b4d5-31ab-4164-b572-6271da8ee3d0">

- **Implementation plan:**

<img width="644" alt="Screenshot 2024-08-21 at 2 30 44 AM" src="https://github.com/user-attachments/assets/6fbfd24b-e30c-488c-b25a-a3839d2b169e">

  
### 1. Program Counter

The Program Counter, also known as the Instruction Pointer, is a component that holds the address of the next instruction to be executed. Typically, the Program Counter increments by 4 to fetch the subsequent instruction from memory. If a reset is triggered, the Program Counter will be reinitialized to zero before continuing with the next instruction.

Below diagram explains the working of PC:

<img width="626" alt="Screenshot 2024-08-21 at 2 35 15 AM" src="https://github.com/user-attachments/assets/e2453cd8-497e-41bf-ba5c-072eac517218">


Below is the code for working of program counter

```tl-verilog
$pc[31:0] = >>1$reset ? 0 : ( >>1$pc + 31'h4 );
```
Output of the code:-
<img width="959" alt="image" src="https://github.com/user-attachments/assets/736be375-da94-41dd-bbee-29d0804c3906">


### 2. Adding the instruction memory

<img width="613" alt="Screenshot 2024-08-21 at 2 37 29 AM" src="https://github.com/user-attachments/assets/0352140c-a4e9-4e80-a391-7d7388b96abc">

- The Program Counter outputs an address used to fetch a 32-bit instruction from the instruction memory.
- The Program Counter increments by 4 with each valid iteration to point to the next instruction.
- During the Fetch stage, the processor retrieves the instruction from the memory based on the address provided by the Program Counter.

<img width="250" alt="Screenshot 2024-08-21 at 2 38 00 AM" src="https://github.com/user-attachments/assets/4c29565a-993a-4bc8-9dad-ca400c096dff">

Code for instruction memory

```tl-verilog
$imem_rd_en = >>1$reset ? 0 : 1;
$imem_rd_addr[31:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
$instr[31:0] = $imem_rd_data[31:0];
```

Output:-

<img width="959" alt="image" src="https://github.com/user-attachments/assets/67e4cc5b-a161-409d-836b-90796e7c2f6b">



### 3. Decoding the instruction

<img width="639" alt="Screenshot 2024-08-21 at 2 40 54 AM" src="https://github.com/user-attachments/assets/2bc79bfe-2774-4b34-9463-ee401dfb5e51">

- The 32-bit fetched instruction must be decoded to determine the operation to be performed and identify the source and destination addresses.
- There are six types of instructions:
    - R-type (Register)
    - I-type (Immediate)
    - S-type (Store)
    - B-type (Branch/Conditional Jump)
    - U-type (Upper Immediate)
    - J-type (Jump/Unconditional Jump)
- The instruction format includes an opcode, immediate value, source address, and destination address.
- During the Decode stage, the processor decodes the instruction according to its format and type.
- The RISC-V ISA typically provides 32 registers, each with a width of XLEN (e.g., 32 bits for RV32). The register file allows two simultaneous reads and one write.

<img width="672" alt="Screenshot 2024-08-21 at 2 42 01 AM" src="https://github.com/user-attachments/assets/16a7daf5-c03d-4b1d-8fd9-13fd76239b9e">

We have decoded the instructions based on the six types of RISC-V instruction sets. The decoding code is as follows:

```tl-verilog
$is_i_instr = $instr[6:2] ==? 5'b0000x ||
              $instr[6:2] ==? 5'b001x0 ||
              $instr[6:2] ==? 5'b11001;
$is_r_instr = $instr[6:2] ==? 5'b01011 ||
              $instr[6:2] ==? 5'b011x0 ||
              $instr[6:2] ==? 5'b10100;
$is_s_instr = $instr[6:2] ==? 5'b0100x;
$is_b_instr = $instr[6:2] ==? 5'b11000;
$is_j_instr = $instr[6:2] ==? 5'b11011;
$is_u_instr = $instr[6:2] ==? 5'b0x101;
```

Output:-

<img width="959" alt="image" src="https://github.com/user-attachments/assets/475e2826-4173-4f65-909f-c76dced5b5b3">



### 3a. Immediate Decode Logic

a.<img width="661" alt="Screenshot 2024-08-21 at 2 43 15 AM" src="https://github.com/user-attachments/assets/832acd74-85d0-45f8-b7d4-93bba21faba7">

b.<img width="606" alt="Screenshot 2024-08-21 at 2 43 29 AM" src="https://github.com/user-attachments/assets/53e45a9e-c913-477f-8fcc-83a24e01e7ba">

The above instruction sets have an immediate field. In order to decode this field we use the following code:-

```tl-verilog
$imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
             $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
             $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
             $is_u_instr ? {$instr[31:12], 12'b0} :
             $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} : 32'b0;
```


<img width="959" alt="image" src="https://github.com/user-attachments/assets/bc7cf65d-91d1-4575-95ae-219820c95ae8">

### 3b. Decode logic for other fields like rs1,rs2,func3,func7

Apart from the immediate we have other fields which also need to be decoded. The code for the same is as follows:-

```tl-verilog
         $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
            
         $rs1_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         ?$rs1_valid
            $rs1[4:0] = $instr[19:15];
         
         $funct3_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         ?$funct3_valid
            $funct3[2:0] = $instr[14:12];
            
         $funct7_valid = $is_r_instr ;
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
            
         $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         ?$rd_valid
            $rd[4:0] = $instr[11:7];

         $opcode[6:0] = $instr[6:0];
```

At any given time, only one instruction is decoded, which could belong to any of the six instruction types. Therefore, it's essential to validate the instruction to ensure it fits into its specific category, preventing conflicts between different instruction types.

<img width="956" alt="image" src="https://github.com/user-attachments/assets/ea4a7f80-2551-4d8b-bca7-0a49d521ffad">

### 3c. Decoding Individual Instructions

<img width="451" alt="Screenshot 2024-08-21 at 2 48 25 AM" src="https://github.com/user-attachments/assets/64ec1d71-d805-4f2d-bb6c-75f7e945d262">

In order to decode the above circled individual instructions use the following code

```tl-verilog
$dec_bits [10:0] = {$funct7[5], $funct3, $opcode};
$is_beq = $dec_bits ==? 11'bx_000_1100011;
$is_bne = $dec_bits ==? 11'bx_001_1100011;
$is_blt = $dec_bits ==? 11'bx_100_1100011;
$is_bge = $dec_bits ==? 11'bx_101_1100011;
$is_bltu = $dec_bits ==? 11'bx_110_1100011;
$is_bgeu = $dec_bits ==? 11'bx_111_1100011;
$is_addi = $dec_bits ==? 11'bx_000_0010011;
$is_add = $dec_bits ==? 11'b0_000_0110011;
```

We also need to update the program counter to account for branch instructions.

```tl-verilog
$pc[31:0] = >>1$reset ? 32'b0 :
            >>1$taken_branch ? >>1$br_target_pc :
            >>1$pc + 32'd4;
```

Ouput:-

<img width="958" alt="image" src="https://github.com/user-attachments/assets/4d161fa8-8e19-4397-971c-45bad04f5180">


### 4. Register File Read and Enable

<img width="604" alt="Screenshot 2024-08-21 at 2 50 51 AM" src="https://github.com/user-attachments/assets/6f36089a-3922-4385-b561-1fbbc7eaaec3">

In this process, instructions are fetched from the instruction memory and stored in registers. Two register slots are used to read the instructions, which are then sent to the ALU for processing.

Code:-

```tl-verilog

$rf_rd_en1 = $rs1_valid;
$rf_rd_en2 = $rs2_valid;
$rf_rd_index1[4:0] = $rs1;
$rf_rd_index2[4:0] = $rs2;
$src1_value[31:0] = $rf_rd_data1;
$src2_value[31:0] = $rf_rd_data2;
```

Output:-

<img width="959" alt="image" src="https://github.com/user-attachments/assets/b3c0e473-c129-4f3d-8a96-179873cdbb35">

### 5. Arithmetic and Logic Unit

<img width="603" alt="Screenshot 2024-08-21 at 2 51 56 AM" src="https://github.com/user-attachments/assets/dbec9e58-e4e5-4edf-ad27-a5052bb105f7">

This code is used to perform arithmetic operations on the values stored in the registers:

```tl-verilog

$result[31:0] = $is_addi ? $src1_value + $imm :
                $is_add ? $src1_value + $src2_value :
                32'bx ;

```

In this section, we have implemented the code to account for ```addi``` and ```add``` operations.

<img width="959" alt="image" src="https://github.com/user-attachments/assets/6082a21c-1091-47cf-adc8-3ed5cb01d9a8">


### 6. Register File Write

<img width="621" alt="Screenshot 2024-08-21 at 2 53 26 AM" src="https://github.com/user-attachments/assets/32a6160e-9c83-40a2-b015-69faf34d18be">


After the ALU processes the values stored in the registers, it may be necessary to write these results back into the registers. To do this, we use the register file write operation. It is important to ensure that no values are written to the destination register if it is x0, as this register is always intended to remain 0. The following code handles this:

```tl-verilog
$rf_wr_en = $rd_valid && $rd != 5'b0;
$rf_wr_index[4:0] = $rd;
$rf_wr_data[31:0] = $result;
```
Output:-

<img width="959" alt="image" src="https://github.com/user-attachments/assets/5e5f1a9d-b152-48f5-bd9c-7c587fec6859">



### 7. Branch instructions

<img width="559" alt="Screenshot 2024-08-21 at 2 55 26 AM" src="https://github.com/user-attachments/assets/f65a4952-c6fb-4ea9-9f53-d6bdccb26215">

```bash

$taken_branch = $is_beq ? ($src1_value == $src2_value):
                $is_bne ? ($src1_value != $src2_value):
                $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])):
                $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])):
                $is_bltu ? ($src1_value < $src2_value):
                $is_bgeu ? ($src1_value >= $src2_value):1'b0;

$br_target_pc[31:0] = $pc +$imm;

```
Output:-

<img width="959" alt="image" src="https://github.com/user-attachments/assets/4fc4076a-267c-4e14-84c3-ad30e5a0489a">


### Testbench
To verify the correctness of the code, we use a testbench to check its operation over the first five cycles.

```bash
*passed = |cpu/xreg[10]>>5$value == (1+2+3+4+5+6+7+8+9) ;
```
Upon checking the log file we get the following result

<img width="518" alt="image" src="https://github.com/user-attachments/assets/ce25f05a-085a-4913-8cd0-98fed8600f2b">

Waveforms showing output's gradual addition from 1 to 9 ;that is from 0(h00) to 45(h2d):

![image](https://github.com/user-attachments/assets/3ee48b93-fa5e-4cf7-b71d-e9f5bfb3aed1)


</details>
<details>
<summary><strong>Day5</strong></summary>

# Pipelining the CPU
Pipelining introduces hazards, particularly the "branch instruction hazard" or "branch penalty." This hazard arises because branch instructions can change the execution sequence, leading to uncertainties. The key hazards include:
  - Structural Hazard: Occurs when multiple instructions compete for the same resource, such as shared execution units, causing pipeline stalls until the conflict is          resolved.
  - Data Hazard: Happens when an instruction depends on the result of a previous instruction, risking incorrect outcomes if the required data isn't available in time.
  - Control Hazard (Branch Hazard): Arises from uncertainty in the outcome of branch instructions, which can delay the confirmation of the next instruction, potentially leading to incorrect instruction fetches and performance penalties due to pipeline flushing.

## Valid signal for Pipelined Logic:

Code:
```
$start = >>1$reset && !$reset;
$valid = $reset ? 1'b0 : ($start || >>3$valid);
$valid_or_reset = $valid || $reset;
$rs1_or_funct3_valid    = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
$rs2_valid              = $is_r_instr || $is_s_instr || $is_b_instr;
$rd_valid               = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
$funct7_valid           = $is_r_instr;
         
```
## Handling Data Hazards in Register File with Bypassing:

```
$src1_value[31:0] = $rs1_bypass ? >>1$result[31:0] : $rf_rd_data1[31:0];
$src2_value[31:0] = $rs2_bypass ? >>1$result[31:0] : $rf_rd_data2[31:0];
```

### Correcting branch target path:

```
   //Current instruction is valid if one of the previous 2 instructions were not (taken_branch or load or jump)
   $valid = ~(>>1$valid_taken_br || >>2$valid_taken_br || >>1$is_load || >>2$is_load || >>2$jump_valid 	|| >>1$jump_valid);
         
   //Current instruction is valid & is a taken branch
   $valid_taken_br = $valid && $taken_br;
         
   //Current instruction is valid & is a load
   $valid_load = $valid && $is_load;
         
   //Current instruction is valid & is jump
   $jump_valid = $valid && $is_jump;
   $jal_valid  = $valid && $is_jal;
   $jalr_valid = $valid && $is_jalr;
    
    *passed = |cpu/xreg[14]>>5$value == (1+2+3+4+5+6+7+8+9+10);

```
### Final 5 Stage Pipelined Logic:

```
\m4_TLV_version 1d: tl-x.org
\SV
   // Template code can be found in: https://github.com/stevehoover/RISC-V_MYTH_Workshop
   
   m4_include_lib(['https://raw.githubusercontent.com/BalaDhinesh/RISC-V_MYTH_Workshop/master/tlv_lib/risc-v_shell_lib.tlv'])

\SV
   m4_makerchip_module   // (Expanded in Nav-TLV pane.)
\TLV

   // /====================\
   // | Sum 0 to 9 Program |
   // \====================/
   //
   // Add 0,1,2,3,...,9 (in that order).
   //
   // Regs:
   //  r10 (a0): In: 0, Out: final sum
   //  r12 (a2): 10
   //  r13 (a3): 1..10
   //  r14 (a4): Sum
   // 
   // External to function:
   m4_asm(ADD, r10, r0, r0)             // Initialize r10 (a0) to 0.
   // Function:
   m4_asm(ADD, r14, r10, r0)            // Initialize sum register a4 with 0x0
   m4_asm(ADDI, r12, r10, 1010)         // Store count of 10 in register a2.
   m4_asm(ADD, r13, r10, r0)            // Initialize intermediate sum register a3 with 0
   // Loop:
   m4_asm(ADD, r14, r13, r14)           // Incremental addition
   m4_asm(ADDI, r13, r13, 1)            // Increment intermediate register by 1
   m4_asm(BLT, r13, r12, 1111111111000) // If a3 is less than a2, branch to label named <loop>
   m4_asm(ADD, r10, r14, r0)            // Store final result to register a0 so that it can be read by main program
   m4_asm(SW, r0, r10, 10000)           // Store r10 result in dmem
   m4_asm(LW, r17, r0, 10000)           // Load contents of dmem to r17
   m4_asm(JAL, r7, 00000000000000000000) // Done. Jump to itself (infinite loop). (Up to 20-bit signed immediate plus implicit 0 bit (unlike JALR) provides byte address; last immediate bit should also be 0)
   m4_define_hier(['M4_IMEM'], M4_NUM_INSTRS)

   |cpu
      @0
         $reset = *reset;
         $clk_CHA = *clk;
         
         //PC fetch - branch, jumps and loads introduce 2 cycle bubbles in this pipeline
         $pc[31:0] = >>1$reset ? '0 : (>>3$valid_taken_br ? >>3$br_tgt_pc :
                                       >>3$valid_load     ? >>3$inc_pc[31:0] :
                                       >>3$jal_valid      ? >>3$br_tgt_pc :
                                       >>3$jalr_valid     ? >>3$jalr_tgt_pc :
                                                     (>>1$inc_pc[31:0]));
         // Access instruction memory using PC
         $imem_rd_en = ~ $reset;
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         
         
      @1
         //Getting instruction from IMem
         $instr[31:0] = $imem_rd_data[31:0];
         
         //Increment PC
         $inc_pc[31:0] = $pc[31:0] + 32'h4;
         
         //Decoding I,R,S,U,B,J type of instructions based on opcode [6:0]
         //Only [6:2] is used here because this implementation is for RV64I which does not use [1:0]
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                       $instr[6:2] ==? 5'b001x0 ||
                       $instr[6:2] == 5'b11001;
         
         $is_r_instr = $instr[6:2] == 5'b01011 ||
                       $instr[6:2] ==? 5'b011x0 ||
                       $instr[6:2] == 5'b10100;
         
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         
         $is_b_instr = $instr[6:2] == 5'b11000;
         
         $is_j_instr = $instr[6:2] == 5'b11011;
         
         //Immediate value decode
         $imm[31:0] = $is_i_instr ? { {21{$instr[31]}} , $instr[30:20]} :
                      $is_s_instr ? { {21{$instr[31]}} , $instr[30:25] , $instr[11:8] , $instr[7]} :
                      $is_b_instr ? { {20{$instr[31]}} , $instr[7] , $instr[30:25] , $instr[11:8] , 1'b0} :
                      $is_u_instr ? { $instr[31] , $instr[30:12] , { 12{1'b0}} } :
                      $is_j_instr ? { {12{$instr[31]}} , $instr[19:12] , $instr[20] , $instr[30:21] , 1'b0} :
                      >>1$imm[31:0];
         
         //Generate valid signals for each instruction fields
         $rs1_or_funct3_valid    = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $rs2_valid              = $is_r_instr || $is_s_instr || $is_b_instr;
         $rd_valid               = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         $funct7_valid           = $is_r_instr;
         
         //Decode other fields of instruction - source and destination registers, funct, opcode
         ?$rs1_or_funct3_valid
            $rs1[4:0]    = $instr[19:15];
            $funct3[2:0] = $instr[14:12];
         
         ?$rs2_valid
            $rs2[4:0]    = $instr[24:20];
         
         ?$rd_valid
            $rd[4:0]     = $instr[11:7];
         
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
         
         $opcode[6:0] = $instr[6:0];
         
         //Decode instruction in subset of base instruction set based on RISC-V 32I
         $dec_bits[10:0] = {$funct7[5],$funct3,$opcode};
         
         //Branch instructions
         $is_beq   = $dec_bits ==? 11'bx_000_1100011;
         $is_bne   = $dec_bits ==? 11'bx_001_1100011;
         $is_blt   = $dec_bits ==? 11'bx_100_1100011;
         $is_bge   = $dec_bits ==? 11'bx_101_1100011;
         $is_bltu  = $dec_bits ==? 11'bx_110_1100011;
         $is_bgeu  = $dec_bits ==? 11'bx_111_1100011;
         
         //Jump instructions
         $is_auipc = $dec_bits ==? 11'bx_xxx_0010111;
         $is_jal   = $dec_bits ==? 11'bx_xxx_1101111;
         $is_jalr  = $dec_bits ==? 11'bx_000_1100111;
         
         //Arithmetic instructions
         $is_addi  = $dec_bits ==? 11'bx_000_0010011;
         $is_add   = $dec_bits ==  11'b0_000_0110011;
         $is_lui   = $dec_bits ==? 11'bx_xxx_0110111;
         $is_slti  = $dec_bits ==? 11'bx_010_0010011;
         $is_sltiu = $dec_bits ==? 11'bx_011_0010011;
         $is_xori  = $dec_bits ==? 11'bx_100_0010011;
         $is_ori   = $dec_bits ==? 11'bx_110_0010011;
         $is_andi  = $dec_bits ==? 11'bx_111_0010011;
         $is_slli  = $dec_bits ==? 11'b0_001_0010011;
         $is_srli  = $dec_bits ==? 11'b0_101_0010011;
         $is_srai  = $dec_bits ==? 11'b1_101_0010011;
         $is_sub   = $dec_bits ==? 11'b1_000_0110011;
         $is_sll   = $dec_bits ==? 11'b0_001_0110011;
         $is_slt   = $dec_bits ==? 11'b0_010_0110011;
         $is_sltu  = $dec_bits ==? 11'b0_011_0110011;
         $is_xor   = $dec_bits ==? 11'b0_100_0110011;
         $is_srl   = $dec_bits ==? 11'b0_101_0110011;
         $is_sra   = $dec_bits ==? 11'b1_101_0110011;
         $is_or    = $dec_bits ==? 11'b0_110_0110011;
         $is_and   = $dec_bits ==? 11'b0_111_0110011;
         
         //Store instructions
         $is_sb    = $dec_bits ==? 11'bx_000_0100011;
         $is_sh    = $dec_bits ==? 11'bx_001_0100011;
         $is_sw    = $dec_bits ==? 11'bx_010_0100011;
         
         //Load instructions - support only 4 byte load
         $is_load  = $dec_bits ==? 11'bx_xxx_0000011;
         
         $is_jump = $is_jal || $is_jalr;
         
      @2
         //Get Source register values from reg file
         $rf_rd_en1 = $rs1_or_funct3_valid;
         $rf_rd_en2 = $rs2_valid;
         
         $rf_rd_index1[4:0] = $rs1[4:0];
         $rf_rd_index2[4:0] = $rs2[4:0];
         
         //Register file bypass logic - data forwarding from ALU to resolve RAW dependence
         $src1_value[31:0] = $rs1_bypass ? >>1$result[31:0] : $rf_rd_data1[31:0];
         $src2_value[31:0] = $rs2_bypass ? >>1$result[31:0] : $rf_rd_data2[31:0];
         
         //Branch target PC computation for branches and JAL
         $br_tgt_pc[31:0] = $imm[31:0] + $pc[31:0];
         
         //RAW dependence check for ALU data forwarding
         //If previous instruction was writing to reg file, and current instruction is reading from same register
         $rs1_bypass = >>1$rf_wr_en && (>>1$rd == $rs1);
         $rs2_bypass = >>1$rf_wr_en && (>>1$rd == $rs2);
         
      @3
         //ALU
         $result[31:0] = $is_addi  ? $src1_value +  $imm :
                         $is_add   ? $src1_value +  $src2_value :
                         $is_andi  ? $src1_value &  $imm :
                         $is_ori   ? $src1_value |  $imm :
                         $is_xori  ? $src1_value ^  $imm :
                         $is_slli  ? $src1_value << $imm[5:0]:
                         $is_srli  ? $src1_value >> $imm[5:0]:
                         $is_and   ? $src1_value &  $src2_value:
                         $is_or    ? $src1_value |  $src2_value:
                         $is_xor   ? $src1_value ^  $src2_value:
                         $is_sub   ? $src1_value -  $src2_value:
                         $is_sll   ? $src1_value << $src2_value:
                         $is_srl   ? $src1_value >> $src2_value:
                         $is_sltu  ? $sltu_rslt[31:0]:
                         $is_sltiu ? $sltiu_rslt[31:0]:
                         $is_lui   ? {$imm[31:12], 12'b0}:
                         $is_auipc ? $pc + $imm:
                         $is_jal   ? $pc + 4:
                         $is_jalr  ? $pc + 4:
                         $is_srai  ? ({ {32{$src1_value[31]}} , $src1_value} >> $imm[4:0]) :
                         $is_slt   ? (($src1_value[31] == $src2_value[31]) ? $sltu_rslt : {31'b0, $src1_value[31]}):
                         $is_slti  ? (($src1_value[31] == $imm[31]) ? $sltiu_rslt : {31'b0, $src1_value[31]}) :
                         $is_sra   ? ({ {32{$src1_value[31]}}, $src1_value} >> $src2_value[4:0]) :
                         $is_load  ? $src1_value +  $imm :
                         $is_s_instr ? $src1_value + $imm :
                                    32'bx;
         
         $sltu_rslt[31:0]  = $src1_value <  $src2_value;
         $sltiu_rslt[31:0] = $src1_value <  $imm;
         
         //Jump instruction target PC computation
         $jalr_tgt_pc[31:0] = $imm[31:0] + $src1_value[31:0]; 
         
         //Branch resolution
         $taken_br = $is_beq ? ($src1_value == $src2_value) :
                     $is_bne ? ($src1_value != $src2_value) :
                     $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                     $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                     $is_bltu ? ($src1_value < $src2_value) :
                     $is_bgeu ? ($src1_value >= $src2_value) :
                     1'b0;
         
         //Current instruction is valid if one of the previous 2 instructions were not (taken_branch or load or jump)
         $valid = ~(>>1$valid_taken_br || >>2$valid_taken_br || >>1$is_load || >>2$is_load || >>2$jump_valid || >>1$jump_valid);
         
         //Current instruction is valid & is a taken branch
         $valid_taken_br = $valid && $taken_br;
         
         //Current instruction is valid & is a load
         $valid_load = $valid && $is_load;
         
         //Current instruction is valid & is jump
         $jump_valid = $valid && $is_jump;
         $jal_valid  = $valid && $is_jal;
         $jalr_valid = $valid && $is_jalr;
         
         //Destination register update - ALU result or load result depending on instruction
         $rf_wr_en = (($rd != '0) && $rd_valid && $valid) || >>2$valid_load;
         $rf_wr_index[4:0] = $valid ? $rd[4:0] : >>2$rd[4:0];
         $rf_wr_data[31:0] = $valid ? $result[31:0] : >>2$ld_data[31:0];
         
      @4
         //Data memory access for load, store
         $dmem_addr[3:0]     =  $result[5:2];
         $dmem_wr_en         =  $valid && $is_s_instr;
         $dmem_wr_data[31:0] =  $src2_value[31:0];
         $dmem_rd_en         =  $valid_load;
         
      
         //Write back data read from load instruction to register
         $ld_data[31:0]      =  $dmem_rd_data[31:0];
         
      
      

      // Note: Because of the magic we are using for visualisation, if visualisation is enabled below,
      //       be sure to avoid having unassigned signals (which you might be using for random inputs)
      //       other than those specifically expected in the labs. You'll get strange errors for these.

   
   // Assert these to end simulation (before Makerchip cycle limit).
   //Checks if sum of numbers from 1 to 9 is obtained in reg[17] and runs 10 cycles extra after this is met
   *passed = |cpu/xreg[17]>>10$value == (1+2+3+4+5+6+7+8+9);
   //Run for 200 cycles without any checks
   //*passed = *cyc_cnt > 200;
   *failed = 1'b0;
   
   // Macro instantiations for:
   //  o instruction memory
   //  o register file
   //  o data memory
   //  o CPU visualization
   |cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@2, @3)  // Args: (read stage, write stage) - if equal, no register bypass is required
      m4+dmem(@4)    // Args: (read/write stage)
   
   m4+cpu_viz(@4)    // For visualisation, argument should be at least equal to the last stage of CPU logic
                       // @4 would work for all labs
\SV
   endmodule
```
### Block diagram

<img width="587" alt="image" src="https://github.com/user-attachments/assets/3b895041-3c84-4a34-ac22-05f57304e280">


### WaveForms

**CLK:**

<img width="599" alt="image" src="https://github.com/user-attachments/assets/5a5853de-1c6f-4b76-abf7-9edcc15d3dba">

**RESET:**

<img width="599" alt="image" src="https://github.com/user-attachments/assets/ec044d5a-3ab1-4f6b-852a-5669f5db1c03">

**Gradual increment of output from 0(h00) to 25(h2d):**

<img width="489" alt="image" src="https://github.com/user-attachments/assets/9d617116-d6d5-4c4a-b14a-ab4838b828c1">


**VIZ TABLE::**

You'll notice that the value of register 10 will reach 45 after 59 cycles.

<img width="499" alt="image" src="https://github.com/user-attachments/assets/8e650776-c435-4784-9a6d-4968c7dfc831">


</details>
</details>

<details>
 
<summary><strong>Lab-7</strong> </summary>
 
## Comparision of GTK wave output and Makerchip output

## Comparision of RISC-V Pre-Synthesis Simulation outputs using Iverilog GTKwave and Makerchip

The RISC-V processor was initially designed using TL-Verilog in the Makerchip IDE. For FPGA implementation, it was converted to Verilog using the Sandpiper-SaaS compiler. Pre-synthesis simulations were subsequently carried out using the GTKWave simulator.

**Step-by-Step process of simulation:**

1.Execute the following set of commands to set up a development environment for working with simulation and synthesis tools, specifically for tasks involving Verilog and RISC-V.

``` bash
$ sudo apt install make python python3 python3-pip git iverilog gtkwave

$ cd ~

$ sudo apt-get install python3-venv

$ python3 -m venv .venv

$ source ~/.venv/bin/activate

$ pip3 install pyyaml click sandpiper-saas
```

<img width="501" alt="image" src="https://github.com/user-attachments/assets/26e66f8d-094e-49b9-8083-c48fca3810ee">
<img width="812" alt="image" src="https://github.com/user-attachments/assets/4658cf7b-7f57-4c4b-84b5-aa21a74acaff">

2. To install the required packages, run the following commands within a virtual environment:
   
```bash
$ sudo apt install make python python3 python3-pip git iverilog gtkwave docker.io

$ sudo chmod 666 /var/run/docker.sock

$ cd ~

$ pip3 install pyyaml click sandpiper-saas

```
<img width="362" alt="image" src="https://github.com/user-attachments/assets/73d41cad-240d-48f8-bc8e-1afbff684bb5">
<img width="824" alt="image" src="https://github.com/user-attachments/assets/9b06bf83-88e7-43d5-b5b8-c55570778a7f">

3. Next, clone the following repository into the home directory and create a ```pre_synth_sim``` directory to store the output.

```bash
$ cd ~

$ git clone https://github.com/manili/VSDBabySoC.git

$ cd /home/vsduser/VSDBabySoC

$ make pre_synth_sim

```

![image](https://github.com/user-attachments/assets/36e4b6d9-fdad-43cb-a7c0-696abeeecad7)



4.Replace the ```rvmyth.tlv``` file in the VSDBabySoC/src/module folder with your RISC-V design ```.tlv``` file from Makerchip that you want to convert to Verilog. Additionally, update the testbench to match your Makerchip code.

5.To obtain the Verilog code from your TLV code and translate the .tlv definition of the RISC-V into a .v definition, use the following code.

```bash
$ sandpiper-saas -i ./src/module/rvmyth.tlv -o rvmyth.v --bestsv --noline -p verilog --outdir ./src/module/
```
![image](https://github.com/user-attachments/assets/abfa2d5b-9db8-4ddb-954c-6e248e450576)

6. Now to compile and simulate RISC-V design run the following code.
   
```bash
$ iverilog -o output/pre_synth_sim.out -DPRE_SYNTH_SIM src/module/testbench.v -I src/include -I src/module
```

6. The result of the simulation (i.e. pre_synth_sim.vcd) will be stored in the output/pre_synth_sim directory.
```bash
$ cd output

$ ./pre_synth_sim.out
```
<img width="763" alt="image" src="https://github.com/user-attachments/assets/7f395831-6d5c-4f10-ba0d-89ae0e601a01">

 7. To open the .vcd simulation file through GTKWave simulation tool
    
```bash
$ gtkwave pre_synth_sim.vcd
```

**Pre-synthesis Simulation results:**
Signals to plot are the following:
- clk_tej: This is the clock input to the RISC-V core. 
- reset: This is the input reset signal to the RISC-V core. 
- OUT[9:0]: This is the 10-bit output [9:0] OUT port of the RISC-V core. This port comes from the RISC-V register #14, originally.

**GTKWave Simulation waveforms:**

- clk_tej plot:
  
<img width="918" alt="image" src="https://github.com/user-attachments/assets/c17eaf87-c396-429d-9080-2774f1641919">

- reset plot:
  
<img width="926" alt="image" src="https://github.com/user-attachments/assets/59bf656f-00ea-445f-8914-638670cacd88">

  
- OUT[9:0] plot:

<img width="926" alt="image" src="https://github.com/user-attachments/assets/9cd97d29-3929-44ed-b28a-56094bb1d7e0">


**Makerchip IDE simulation results for comparison**

- clk_tej plot:
  
 <img width="599" alt="image" src="https://github.com/user-attachments/assets/5a5853de-1c6f-4b76-abf7-9edcc15d3dba">

- reset plot:

  <img width="599" alt="image" src="https://github.com/user-attachments/assets/ec044d5a-3ab1-4f6b-852a-5669f5db1c03">
 

- OUT[9:0] plot:

   <img width="489" alt="image" src="https://github.com/user-attachments/assets/9d617116-d6d5-4c4a-b14a-ab4838b828c1">  
</details>

<details>
 <summary><strong>Lab-8</strong> </summary>
 
 ### AIM
* The task involves integrating and validating the functionality of a custom RISC-V processor (rvmyth) within the BabySoC platform, using specialized tools for digital design and simulation.
* The objective is to generate DAC and PPL waveforms for the RISC-V processor.

### PHASE-LOCKED LOOP (PLL)
* A Phase-Locked Loop (PLL) is an electronic control system that produces an output signal with a phase that is synchronized to the phase of an input signal.
* It is widely utilized in telecommunications, radio, and computing for signal synchronization, frequency stabilization, and clock generation for digital circuits.

### Download and Prepare Project Files
* You can download all the files for BabySoC using the following command.
```
git clone https://github.com/manili/VSDBabySoC.git
```
![Screenshot from 2024-09-02 23-06-41](https://github.com/user-attachments/assets/c7812c84-d88a-4f36-b0f8-a7090a39897f)

### Verilog Code

![Screenshot from 2024-09-02 23-09-12](https://github.com/user-attachments/assets/9d230bb7-f7bd-4442-8de6-b5676542d800)


### Editing the Top-level verilog code

### Simulation Procedure
* You can perform a functional simulation using the following command.
```
cd BabySoC_Simulation
iverilog -o ./pre_synth_sim.out -DPRE_SYNTH_SIM src/module/testbench.v -I src/include -I src/module/
./pre_synth_sim.out
gtkwave pre_synth_sim.vcd
```
### Results

* Waveforms
  
   - Clock waveform as clk_tej along with PLL clk.
     
      ![Screenshot from 2024-09-02 23-13-44](https://github.com/user-attachments/assets/88f17028-b88f-4efa-8d85-993f2b8aa79e)

   - Reset Waveform
     ![Screenshot from 2024-09-02 23-14-44](https://github.com/user-attachments/assets/90bf0248-5c0c-46e1-8e8d-f7513ce818ae)
   -  Final Result waveform (with PLL output signal, rvmyth 10-bit output signal, DAC output analog waveform)
     ![Screenshot from 2024-09-02 23-20-59](https://github.com/user-attachments/assets/940adf80-629f-4088-9e6a-ed0edc96bd46)

</details>



<details>
<summary><strong>Lab 9:</strong></summary>


<details>
<summary><strong>Day 1</strong></summary>

## Introduction to iverilog

In digital circuit design, Register-Transfer Level (RTL) is an abstraction that models synchronous digital circuits by detailing the flow of data between hardware registers and the logic operations applied to these signals. This RTL abstraction is expressed using HDL (Hardware Description Language), enabling the creation of high-level models that are subsequently transformed into lower-level representations and, ultimately, physical hardware designs.

**Simulator**:
A tool used to verify the circuit design. In this workshop, we use the **Icarus Verilog (iverilog)** tool. Simulation involves creating models that mimic the behavior of the target device (simulation models) and using test models to validate the device (test benches). RTL design is typically composed of one or more Verilog files that specify the design’s functionality and requirements.

**Test Bench**:
A setup that delivers stimuli (test vectors) to the design, verifying its behavior and ensuring it meets the required specifications.

### HOW SIMULATOR WORKS 
The simulator monitors changes in input signals. When an input change occurs, the output is evaluated accordingly. If there are no changes to the input signals, the simulator will not evaluate the output.


![Screenshot from 2024-10-21 10-51-23](https://github.com/user-attachments/assets/8612b99d-cf8d-4e88-b1bc-89d8a0bb9bbc)


**Design** may have 1 or more primary inputs and primary outputs but **testbench** doesn't have it.

### SIMULATION FLOW

![Screenshot from 2024-10-21 10-59-06](https://github.com/user-attachments/assets/1862f4ea-b19a-4fce-9c8d-de6a2db43151)


## Introduction to LABS

### ENVIRONMENT SETUP

Set up the tool flow using the below commands:

```
mkdir VLSI

cd VLSI

git clone https://github.com/kunalg123/vsdflow.git

git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git

cd sky130RTLDesignAndSynthesisWorkshop
```

![Screenshot from 2024-10-21 22-31-40](https://github.com/user-attachments/assets/704f3639-2fed-4c30-89af-687fba3d6263)


The **sky130RTLDesignAndSynthesisWorkshop** directory includes **My_Lib**, which holds all the required library files. The **lib** folder contains standard cell libraries for synthesis, while the **verilog_model** folder has the Verilog models for all the standard cells in the library. The **verilog_files** folder has all the lab session experiments, including both the Verilog code and testbench files.

![image](https://github.com/user-attachments/assets/7b4d1c71-b36f-4913-8586-8b4b4a3dddfb)


## Labs using iverilog & gtkwave

### Simulation using iverilog simulator - 2:1 multiplexer rtl design

#### VERILOG FILE OF A SIMPLE 2:1 MUX

To compile the verilog and testbench file use the following commands which will generate an executable file and will dump the waveform to view it using the gtkwave

```
iverilog good_mux.v tb_good_mux.v

./a.out

gtkwave tb_good_mux.vcd
```

We can view the waveform of a simple 2:1 mux which selects the input based on the select line

![Screenshot from 2024-10-21 22-49-24](https://github.com/user-attachments/assets/9fc3458f-2041-4145-a590-19c0306bb6cf)
![Screenshot from 2024-10-21 22-50-00](https://github.com/user-attachments/assets/16b74f57-242c-4d01-b17f-fb11835a3d77)
![Screenshot from 2024-10-21 22-51-18](https://github.com/user-attachments/assets/d5fe6613-348a-4ef1-a822-49c53a3721b5)


#### Access Module Files
To view the contents of the **good_mux** file run the following command
```
 vim tb_good_mux.v -o good_mux.v 
```

Design file and Testbench File

![Screenshot from 2024-10-21 22-53-46](https://github.com/user-attachments/assets/a0570c9b-7129-4e70-a4cf-64872ddd5298)
![Screenshot from 2024-10-21 22-53-37](https://github.com/user-attachments/assets/e0d3f40c-8c7f-4e4a-b764-eef83e58694c)


## Introduction to Yosys & Logic Synthesis

**Synthesizer** is a tool for converting the **RTL** to Netlist and here we are using the **Yosys** Synthesizer.

### Yosys SETUP

![Screenshot from 2024-10-21 11-10-21](https://github.com/user-attachments/assets/537bebbf-c706-46f3-84f2-f3d05a36656f)


### Verifying the Synthesis

![Screenshot from 2024-10-21 11-10-50](https://github.com/user-attachments/assets/a267721b-7a56-4396-ae22-671c7b0b62f3)

**Note**:- The set of Primary inputs / primary outputs will remain the same between the RTL design and Synthesis so we can use the same testbench.

## Logic Synthesis

RTL Design - behavioral representation in HDL form for the required specification.

 **Synthesis** - RTL to Gate level translation.
 The design is converted int gates and connections are made. This given outas a file called **netlist**.

**Liberty (.lib)** is a collection of logical modules, including basic logic gates like AND, OR, and NOT, with different variants such as 2-input, 3-input, and 4-input gates, as well as slow, medium, and fast versions. Fast cells are used for high-performance needs, while slower cells help address hold time issues. Using faster cells can increase area and power consumption and may cause hold time violations. On the other hand, overusing slower cells can degrade performance. Optimal cell selection during synthesis is based on constraints that balance area, power, and timing requirements.

![Screenshot from 2024-10-21 11-11-26](https://github.com/user-attachments/assets/0581e61c-704b-429f-bb03-2f115343d18c)

### Faster cells and Slower Cells

Cell delay in a digital logic circuit is influenced by the circuit's load, which is represented by capacitance. Faster charging or discharging of the capacitance results in a lower cell delay.

To speed up the charging/discharging process, wider transistors are used as they can provide more current, reducing cell delay. However, wider transistors also increase power consumption and area. On the other hand, narrower transistors reduce area and power consumption but lead to higher cell delay. Therefore, designing a circuit with low cell delay involves a trade-off between area and power.

![Screenshot from 2024-10-21 11-13-28](https://github.com/user-attachments/assets/b910f788-d5e3-4951-aaca-00eafaf3f7fe)

## Yosys flow

**1.**  start yosys.
          
```
yosys
```
![Screenshot from 2024-10-21 22-57-56](https://github.com/user-attachments/assets/6c4ab589-01c8-4d0b-bf59-e87a0de94e19)

**2.** load the sky130 standard library.
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
![Screenshot from 2024-10-21 22-58-51](https://github.com/user-attachments/assets/dc5b0a04-3f29-4292-8e50-fc0b1598e89b)

**3.** Read the design files
```
read_verilog good_mux.v
```

![Screenshot from 2024-10-21 22-59-06](https://github.com/user-attachments/assets/50b47aee-b232-4d4e-8d63-f4e58546039a)


**4.** Synthesize the top level module
```
synth -top good_mux
```
![Screenshot from 2024-10-21 22-59-35](https://github.com/user-attachments/assets/cfcdbcd8-6b29-488d-8a89-03dfef58bf8d)


        
**5.** Map to the standard library
```
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
![Screenshot from 2024-10-21 23-00-22](https://github.com/user-attachments/assets/d464cda5-94dc-4c18-867a-c770eaaf57b7)



**6.** Two view the result as a graphich use the show command.
```
show
```
![Screenshot from 2024-10-21 23-20-49](https://github.com/user-attachments/assets/14859371-ebef-4940-a0bd-eca0161e5f8e)

**7.** To write the result netlist to a file use the write_veriog command. This will output the netlist to a file in the current directory.
```
write_verilog -noattr good_mux_netlist.v
```

![Screenshot from 2024-10-21 23-21-52](https://github.com/user-attachments/assets/bd3170bc-2e03-4f09-8743-18d3eb0cb971)

</details>



<details>  
<summary><strong>Day 2:</strong></summary>

## Introduction to timing labs

Run the following commands to view the contents inside the .lib file:

```
cd VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/

vim sky130_fd_sc_hd__tt_025C_1v80.lib

```
![Screenshot from 2024-10-21 23-51-26](https://github.com/user-attachments/assets/398166c2-fa1f-49ee-94b0-edbd2b5bc252)

![Screenshot from 2024-10-21 23-50-58](https://github.com/user-attachments/assets/bec8779e-020e-4d86-89cb-63861730a771)


##  Cell library
 A standard cell library is a collection of characterized logic gates that can be used to implement digital circuits. The Liberty (.lib) files contain PVT parameters (Process, Voltage, Temperature) that can significantly impact circuit performance. Variations in manufacturing, changes in voltage, and fluctuations in temperature all play a role in affecting how the circuit functions.

![Screenshot from 2024-10-21 12-18-00](https://github.com/user-attachments/assets/e084adcb-14b2-4f9c-95e7-3a6c8a7ab6d8)

We can also find various flavours of AND gate

![22](https://github.com/user-attachments/assets/1c5784b6-d129-4b74-97cd-30a70c84b9fb)
![23](https://github.com/user-attachments/assets/619718fb-8e3b-4aeb-af6b-47d00f6f0f40)
![24](https://github.com/user-attachments/assets/1d6bc766-a9aa-47b4-9ce6-d2c201e5ce80)

We can observe that:
* and2_0 -- takes the least area, more delay and low power.
* and2_1 -- takes more area, less delay and high power.
* and2_2 -- takes the largest area, larger delay and highest power.

## Hierarchial synthesis vs Flat synthesis 

Hierarchical synthesis involves breaking down a complex design into various sub-modules, each of which is synthesized separately to produce gate-level netlists before being integrated. This approach enhances organization, allows for module reuse, and enables incremental design changes without impacting the entire system. In contrast, flat synthesis treats the entire design as a single unit during the synthesis process, resulting in a single netlist regardless of any hierarchical relationships. While flat synthesis can optimize certain designs, it becomes difficult to maintain, analyze, and modify the design due to its absence of structural modularity.

### Hierarchial synthesis  

Consider the verilog file ```multiple_modules.v``` which is given in the verilog_files directory
```
module sub_module2 (input a, input b, output y);
    assign y = a | b;
endmodule

module sub_module1 (input a, input b, output y);
    assign y = a&b;
endmodule


module multiple_modules (input a, input b, input c , output y);
    wire net1;
    sub_module1 u1(.a(a),.b(b),.y(net1));  //net1 = a&b
    sub_module2 u2(.a(net1),.b(c),.y(y));  //y = net1|c ,ie y = a&b + c;
endmodule
```
To perform hierarchical synthesis on the ```multiple_modules.v ``` file use the following commands:
```
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog multiple_modules.v

synth -top multiple_modules

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show multiple_modules

write_verilog -noattr multiple_modules_hier.v

!vim multiple_modules_hier.v

```
When you do synth -top 'topmodulename' in yosys, it does an hierarchical synthesis. ie the different hierarchies between modules are preserved.

![Screenshot from 2024-10-21 23-55-32](https://github.com/user-attachments/assets/9352963f-9146-4f64-9267-2da488c43cfa)


**Staistics of Multiple Modules**
![Screenshot from 2024-10-21 23-56-19](https://github.com/user-attachments/assets/45c2ef54-b771-4184-99b9-132c8cea25e8)


**Realization of the Logic**

![Screenshot from 2024-10-21 23-58-08](https://github.com/user-attachments/assets/e6ba4db2-ea45-48cf-9dd5-2460e65d9f87)


**Map to the standard library**

![Screenshot from 2024-10-21 23-59-46](https://github.com/user-attachments/assets/9dc01201-afc0-49a2-81cd-c00732251500)



**Netlist file**

![Screenshot from 2024-10-22 00-00-20](https://github.com/user-attachments/assets/8d444e83-5bed-492d-82e6-387898ce6bf0)




#### Flat synthesis  
Merges all hierarchical modules in the design into a single module to create a flat netlist. To perform flat synthesis on the ```multiple_modules.v``` file type the following commands:
```
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog multiple_modules.v

synth -top multiple_modules

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

flatten

show

write_verilog -noattr multiple_modules_flat.v

!vim multiple_modules_flat.v
```
![210](https://github.com/user-attachments/assets/42b2b8cc-8616-41fc-b79d-9674d6182b61)

**Realization of the Logic**

![Screenshot from 2024-10-22 00-08-52](https://github.com/user-attachments/assets/b8d9c896-9581-4b9a-9127-8b50b657ca1b)

**Netlist file**

![Screenshot from 2024-10-22 00-09-54](https://github.com/user-attachments/assets/7d6851d0-623f-4227-affb-0f7004ab622b)



### Sub Module Level Synthesis
This method is preferred when multiple instances of same module are used. The synthesis is carried out once and is replicate multiple times, and the multiple instances of the same module are stitched together in the top module. This method is helpful when making use of divide and conquer algorithm


 ```
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog multiple_modules.v

synth -top sub_module1

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show
```

![213](https://github.com/user-attachments/assets/0a46f5fe-d9f0-4bef-a7d0-72f5192a9739)

**Realization of the Logic**

![Screenshot from 2024-10-22 00-10-49](https://github.com/user-attachments/assets/cfe44c0d-e941-47aa-b116-4031d5f60994)



## Flop coding styles and optimization

Flip-Flops are an essential part of sequential logic in a circuit and here we explore the design and synthesis of various types of flip-flops. To prevent glitches in digital circuits, we use flip-flops to store intermediate values. This ensures that combinational circuit inputs remain stable until the clock edge, avoiding glitches and maintaining correct operation:

### Asynchronous Reset/set:

**Verilog Code for Asynchronous Reset:**

```
module dff_asyncres ( input clk ,  input async_reset , input d , output reg q );
always @ (posedge clk , posedge async_reset)
begin
	if(async_reset)
		q <= 1'b0;
	else	
		q <= d;
end
endmodule
```
**Verilog Code Asynchronous Set:**

```
module dff_async_set ( input clk ,  input async_set , input d , output reg q );
always @ (posedge clk , posedge async_set)
begin
	if(async_set)
		q <= 1'b1;
	else	
		q <= d;
end
endmodule
```

In this design, the `always` block is triggered by changes in the clock or the reset signal. The circuit is sensitive to the positive edge of the clock. When the reset/set signal goes low or high, the signal on the `q` line changes accordingly. Therefore, the behavior associated with the reset/set occurs immediately and does not wait for the positive edge of the clock.

### Synchronous Reset:

```
module dff_syncres ( input clk , input async_reset , input sync_reset , input d , output reg q );
always @ (posedge clk )
begin
	if (sync_reset)
		q <= 1'b0;
	else	
		q <= d;
end
endmodule
```

#### FLIP FLOP SIMULATION

```
iverilog dff_asyncres.v tb_dff_asyncres.v 

ls

./a.out

gtkwave tb_dff_asyncres.vcd
```

**GTK WAVE OF ASYNCHRONOUS RESET**
![Screenshot from 2024-10-22 00-15-08](https://github.com/user-attachments/assets/96cf2438-0776-4781-9343-4aad52064eef)


**GTK WAVE OF ASYNCHRONOUS SET**

![Screenshot from 2024-10-22 00-17-54](https://github.com/user-attachments/assets/4a6edef1-10e6-4840-ba17-d4e94d61fb0c)


**GTK WAVE OF SYNCHRONOUS RESET**

![Screenshot from 2024-10-22 00-19-24](https://github.com/user-attachments/assets/afa39f4c-f578-4a1d-a6e7-e4de18e5cf29)


#### FLIP FLOP SYNTHESIS

```

yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog dff_asyncres.v

synth -top dff_asyncres

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show 
```
**Statistics of D FLipflop with Asynchronous Reset**

![Screenshot from 2024-10-22 00-22-41](https://github.com/user-attachments/assets/8544690a-0f77-4454-9144-5e2713b3d738)

![Screenshot from 2024-10-22 00-23-19](https://github.com/user-attachments/assets/c01017fd-e415-4200-af06-ea5b4bb0a504)


**Realization of Logic**

![Screenshot from 2024-10-22 00-20-35](https://github.com/user-attachments/assets/d7223158-3d06-415d-976d-6a3a6579b79b)


**Note:**  We wrote a flop with active high reset but the flop is having acting low reset so the tool inserted the inverter so (!(!(reset))) is just reset so at the end we got a flop with active high reset

**Statistics of D FLipflop with Asynchronous set**\
Follow the same steps as given above just the file name changes to dff_async_set.v

![222](https://github.com/user-attachments/assets/4e40503f-28f9-4c14-ab11-08a2de0946f1)

![223](https://github.com/user-attachments/assets/d4910286-fb0e-4e89-991b-d96d0e8b8653)

**Realization of Logic**

![224](https://github.com/user-attachments/assets/90253526-d4c0-472d-a5af-fd6c52c258dc)

**Note:**  We wrote a flop with active high set but the flop is having acting low set so the tool inserted the inverter so (!(!(set))) is just set so at the end we got a flop with active high set

**Statistics of D FLipflop with Synchronous Reset**

![Screenshot from 2024-10-22 00-29-04](https://github.com/user-attachments/assets/7a6b936e-2cff-499b-b3f9-70d9e3856d92)


![Screenshot from 2024-10-22 00-28-27](https://github.com/user-attachments/assets/223d8c96-4603-4c43-93dc-64dc7e64d4b1)



**Realization of Logic**


![Screenshot from 2024-10-22 00-27-55](https://github.com/user-attachments/assets/6c118f0c-8a8f-4a10-b59f-bf7ee304ba26)




## Optimizations

### Example 1: mult_2.v 

**verilog code**

```
module mul2 (input [2:0] a, output [3:0] y);
assign y = a * 2;
endmodule
```

**truth table**

![Screenshot from 2024-10-21 15-35-31](https://github.com/user-attachments/assets/b53852d4-4db4-417e-8532-8f5e453fc84a)

We can see the multiplication of a number by 2 doesnt really need any extra hardware we just need to append the LSB's with zeroes and the remaining bits are the input bits of same, It can be realised by grouding the LSB's and wiring the input properly to the output.

Run the below code to view the netlist:

```
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog mult_2.v

synth -top mul2

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show 

write_verilog -noattr mult_2_net.v

!vim mult_2_net.v
```
**Statestics**

![Screenshot from 2024-10-22 00-42-26](https://github.com/user-attachments/assets/1fa0a801-da95-41de-bb6f-e7d5daa71e20)



**Realization of Logic**

![Screenshot from 2024-10-22 00-41-44](https://github.com/user-attachments/assets/15d6f78b-2f72-4779-be25-e0e208e5b34c)



**Netlist**

![Screenshot from 2024-10-22 00-42-50](https://github.com/user-attachments/assets/5f919272-0b06-4084-ac9b-9746c25d7d93)



### Example 2: mult_8.v

**verilog code**

```
module mult8 (input [2:0] a , output [5:0] y);
	assign y = a * 9;
endmodule
```

**logic behaviour**

![Screenshot from 2024-10-21 15-43-38](https://github.com/user-attachments/assets/a66a1c34-977d-446c-a462-fae3073a1425)


In this design the 3-bit input number "a" is multiplied by 9 i.e (a9) which can be re-written as (a8) + a . The term (a8) is nothing but a left shifting the number a by three bits. Consider that a = a2 a1 a0. (a8) results in a2 a1 a0 0 0 0. (a9)=(a8)+a = a2 a1 a0 a2 a1 a0 = aa(in 6 bit format). Hence in this case no hardware realization is required. The synthesized netlist of this design is shown below:

```
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog mult_8.v

synth -top mul8

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr mult_8_net.v

```

**Statestics**

![Screenshot from 2024-10-22 00-43-49](https://github.com/user-attachments/assets/c438ac55-31e0-407e-bb77-c877fa3bd5b9)



**Realization of Logic**

![Screenshot from 2024-10-22 00-43-29](https://github.com/user-attachments/assets/3e91d7d2-161c-417f-9b7d-20df0af69c75)



**Netlist**
 
![Screenshot from 2024-10-22 00-45-58](https://github.com/user-attachments/assets/21771605-8806-4430-a721-a96313e89ed4)




</details>

<details>
<summary><strong>Day 3</strong></summary>

##  Introduction to Combinational Logic Optimization and sequential Optimization
There are two types of optimisations: Combinational and Sequential optimisations. These optimisations are done inorder to achieve designs that are efficient in terms of area, power, and performance.

**Combinational Optimization**

The techiniques used are:

- Constant Propagation (Direct Optimisation)
- Boolean Logic Optimisation (using K-Map or Quine McCluskey method)

**Constant Propagation:**

Consider the below circuit:

![image](https://github.com/user-attachments/assets/24fcec7b-7b46-4d73-b93d-a257883ef6e5)

The top circuit uses 6 transistors (3 NMOS and 3 PMOS), while the bottom circuit uses 2 transistors (1 NMOS and 1 PMOS) when input A is set to zero, turning the logic into an inverter.

**Boolean Logic Optimisation:**

Consider the below verilog code:

```assign y = a?(b?c:(c?a:0)):(!c)```

The ternary operator (?:) will make the circuit behave like a mux upon synthesis as shown below. 

![Screenshot from 2024-10-21 16-15-29](https://github.com/user-attachments/assets/77afa517-d6e6-498e-be71-f5dac5c0d777)


The circuit can be optimised as follows:

![Screenshot from 2024-10-21 16-15-54](https://github.com/user-attachments/assets/e5cfb3b9-1e20-4a13-ad2e-3a4774e03632)


**Example 1:**

Verilog code:

```
module opt_check (input a , input b , output y);
	assign y = a?b:0;
endmodule
```

The above code infers a multiplexer and since one of the inputs of the multiplexer is always connected to the ground it will infer an AND gate on optimisation.

Run the following commands for netlist:

```
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog opt_check.v

synth -top opt_check

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr opt_check_net.v

!vim opt_ckeck_net.v
```

![Screenshot from 2024-10-22 01-19-07](https://github.com/user-attachments/assets/716a9463-996e-4371-83ee-360833907107)


![31](https://github.com/user-attachments/assets/c925acbe-5385-4c4f-a3b9-aa69c00d3f58)


**Example 2:**

Verilog code:

```
module opt_check2 (input a , input b , output y);
	assign y = a?1:b;
endmodule
```

Since one of the inputs of the multiplexer is always connected to the logic 1 it will infer an OR gate on optimisation.The OR gate will be NAND implementation since NOR gate has stacked pmos while NAND implementation has stacked nmos.

Run the below code for netlist:

```
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog opt_check2.v

synth -top opt_check2

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr opt_check2_net.v

!vim opt_ckeck2_net.v
```


![Screenshot from 2024-10-22 01-21-40](https://github.com/user-attachments/assets/e59899a6-728c-458d-8bcc-a12025c9aa36)


![32](https://github.com/user-attachments/assets/08fe3c54-faf2-4093-ba77-83bc2971c509)


**Example 3:**

Verilog code:

```
module opt_check3 (input a , input b, input c , output y);
	assign y = a?(c?b:0):0;
endmodule
```

On optimisation the above design becomes a 3 input AND gate.

Run the below code for netlist:

```
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog opt_check3.v

synth -top opt_check3

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr opt_check3_net.v

!vim opt_ckeck3_net.v
```


![Screenshot from 2024-10-22 01-22-16](https://github.com/user-attachments/assets/1b9e701b-5df6-4639-8e72-508dc99b35fa)


![33](https://github.com/user-attachments/assets/9764687e-9c97-46a6-bdc9-78e93d9bac15)

**Example 4:**

Verilog code:

```
module opt_check4 (input a , input b, input c , output y);
	assign y = a?(c?b:0):0;
endmodule
```

On optimisation the above design becomes a 2 input XNOR gate.

Run the below code for netlist:

```
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog opt_check4.v

synth -top opt_check4

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr opt_check4_net.v

!vim opt_ckeck4_net.v
```

![Screenshot from 2024-10-22 01-23-00](https://github.com/user-attachments/assets/1ffe373b-fb6d-4239-9b22-fdf3c1505ddd)


![34](https://github.com/user-attachments/assets/a9c21325-aba8-4e4c-89d6-680b7b38bcfc)

**Example 5:**

Verilog code:

```
module sub_module1(input a , input b , output y);
 assign y = a & b;
endmodule

module sub_module2(input a , input b , output y);
 assign y = a^b;
endmodule

module multiple_module_opt(input a , input b , input c , input d , output y);
wire n1,n2,n3;

sub_module1 U1 (.a(a) , .b(1'b1) , .y(n1));
sub_module2 U2 (.a(n1), .b(1'b0) , .y(n2));
sub_module2 U3 (.a(b), .b(d) , .y(n3));

assign y = c | (b & n1); 

endmodule
```

On optimisation the above design becomes a AND OR gate

Run the below code for netlist:

```
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog multiple_module_opt.v

synth -top multiple_module_opt

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

flatten

show

write_verilog -noattr multiple_module_opt_net.v

!vim multiple_module_opt_net.v
```


![Screenshot from 2024-10-22 01-23-35](https://github.com/user-attachments/assets/76a6a3b1-4ad8-41f7-9d82-01731cccb25e)


![35](https://github.com/user-attachments/assets/05b7a73b-546a-4fff-b1d9-052d4328a81d)



**Example 6:**

Verilog code:

```
module sub_module(input a , input b , output y);
	assign y = a & b;
endmodule

module multiple_module_opt2(input a , input b , input c , input d , output y);
		wire n1,n2,n3;
	sub_module U1 (.a(a) , .b(1'b0) , .y(n1));
	sub_module U2 (.a(b), .b(c) , .y(n2));
	sub_module U3 (.a(n2), .b(d) , .y(n3));
	sub_module U4 (.a(n3), .b(n1) , .y(y));
endmodule
```

On optimisation the above design becomes Y=0 

Run the below code for netlist:

```
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog multiple_module_opt2.v

synth -top multiple_module_opt2

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

flatten

show

write_verilog -noattr multiple_module_opt2_net.v

!vim multiple_module_opt_net2.v
```


![Screenshot from 2024-10-22 01-24-16](https://github.com/user-attachments/assets/89daff84-aa80-477b-825c-c8b86c96d387)



![36](https://github.com/user-attachments/assets/0416007d-9701-426c-b020-ca7144719062)


**Sequential Logic Optimizations**

**Example 1:**

Verilog code:

```
module dff_const1(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b0;
	else
		q <= 1'b1;
end
endmodule
```

Run the below code for netlist:

```
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog dff_const1.v

synth -top dff_const1

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr dff_const1_net.v

!vim dff_const1_net.v
```



![Screenshot from 2024-10-22 01-25-04](https://github.com/user-attachments/assets/9060350e-3fbb-45cf-887b-45da6683ffaf)


![Screenshot from 2024-10-21 17-50-00](https://github.com/user-attachments/assets/42f1d861-1789-40b5-9ad3-9bfab4d82e59)


GTKWave Output:

```
iverilog dff_const1.v tb_dff_const1.v

./a.out

gtkwave tb_dff_const1.vcd

```

![Screenshot from 2024-10-22 01-27-57](https://github.com/user-attachments/assets/fe76170e-d4b0-4a3e-94a1-65c681c5a329)



**Example 2:**

Verilog code:

```
module dff_const2(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b1;
	else
		q <= 1'b1;
end
endmodule
```

Run the below code for netlist:

```
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog dff_const2.v

synth -top dff_const2

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr dff_const2_net.v

!vim dff_const2_net.v
```


![Screenshot from 2024-10-22 01-29-00](https://github.com/user-attachments/assets/ba3a123c-e086-41d4-95a2-0e0d6823a245)


![Screenshot from 2024-10-21 17-57-07](https://github.com/user-attachments/assets/437dc4e6-5ed5-4aa7-8cd9-716219293ae0)


GTKWave Output:

```
iverilog dff_const2.v tb_dff_const2.v

./a.out

gtkwave tb_dff_const2.vcd
```

![Screenshot from 2024-10-22 01-30-33](https://github.com/user-attachments/assets/ffac9816-3fe9-429a-90e6-04b08fce8db0)


**Example 3:**

Verilog code:

```
module dff_const3(input clk, input reset, output reg q);
reg q1;

always @(posedge clk, posedge reset)
begin
	if(reset)
	begin
		q <= 1'b1;
		q1 <= 1'b0;
	end
	else
	begin
		q1 <= 1'b1;
		q <= q1;
	end
end
endmodule
```

Run the below code for netlist:

```
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog dff_const3.v

synth -top dff_const3

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr dff_const3_net.v
```



![Screenshot from 2024-10-22 01-31-11](https://github.com/user-attachments/assets/3997a4cc-284b-4679-b4a1-1fccba553aa6)


![Screenshot from 2024-10-21 18-02-49](https://github.com/user-attachments/assets/5d0dbaa6-03d6-4650-9488-5ad58ddf6aaa)


GTKWave Output:

```
iverilog dff_const3.v tb_dff_const3.v

./a.out

gtkwave tb_dff_const3.vcd
```


![Screenshot from 2024-10-22 01-32-21](https://github.com/user-attachments/assets/b38bd0dd-2adc-4fc2-9e8f-4a63270cec5a)



**Example 4:**

Verilog code:

```
module dff_const4(input clk, input reset, output reg q);
reg q1;

always @(posedge clk, posedge reset)
begin
	if(reset)
	begin
		q <= 1'b1;
		q1 <= 1'b1;
	end
else
	begin
		q1 <= 1'b1;
		q <= q1;
	end
end
endmodule
```

Run the below code for netlist:

```
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog dff_const4.v

synth -top dff_const4

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr dff_const4_net.v
```

![Screenshot from 2024-10-22 01-33-26](https://github.com/user-attachments/assets/af6276d7-58be-49e2-85d6-b881eb63e506)


![Screenshot from 2024-10-21 18-03-58](https://github.com/user-attachments/assets/0af3ff11-2c59-49ff-b9dd-49707a344c78)

 
GTKWave Output:

```
iverilog dff_const4.v tb_dff_const4.v

./a.out

gtkwave tb_dff_const4.vcd
```


![Screenshot from 2024-10-22 01-34-47](https://github.com/user-attachments/assets/c6c06d99-1d91-4036-90d7-26a3a1f43439)



**Example 5:**

Verilog code:

```
module dff_const5(input clk, input reset, output reg q);
reg q1;
always @(posedge clk, posedge reset)
	begin
		if(reset)
		begin
			q <= 1'b0;
			q1 <= 1'b0;
		end
	else
		begin
			q1 <= 1'b1;
			q <= q1;
		end
	end
endmodule
```

Run the below code for netlist:

```
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog dff_const5.v

synth -top dff_const5

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr dff_const5_net.v
```


![Screenshot from 2024-10-22 01-35-39](https://github.com/user-attachments/assets/c3db131b-4b32-4179-a60e-b800f79a829e)


![Screenshot from 2024-10-21 18-04-56](https://github.com/user-attachments/assets/1e6b6509-ede0-43e2-b877-7ab4f5047733)


GTKWave Output:

```
iverilog dff_const5.v tb_dff_const5.v

./a.out

gtkwave tb_dff_const5.vcd
```

![Screenshot from 2024-10-22 01-37-29](https://github.com/user-attachments/assets/94e37491-071b-4b60-b128-c0d0120b06c9)


**Sequential Logic Optimizations for unused outputs**

**Example 1:**

Verilog code:

```
module counter_opt (input clk , input reset , output q);
reg [2:0] count;
assign q = count[0];
always @(posedge clk ,posedge reset)
begin
	if(reset)
		count <= 3'b000;
	else
		count <= count + 1;
end
endmodule
```

Run the below code for netlist:

```
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog counter_opt.v

synth -top counter_opt

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr counter_opt_net.v
```


![Screenshot from 2024-10-22 01-38-06](https://github.com/user-attachments/assets/263a5c9e-fa5c-4951-8500-777500b4d13b)



![Screenshot from 2024-10-22 01-41-25](https://github.com/user-attachments/assets/7a39d57e-2846-4e0d-89ab-8e6d280be6df)



GTKWave Output:

```
iverilog counter_opt.v tb_counter_opt.v

./a.out

gtkwave tb_counter_opt.vcd
```


![Screenshot from 2024-10-22 01-39-31](https://github.com/user-attachments/assets/afe067b1-e21c-4083-9467-36a833243c2b)


Modified counter logic:

Verilog code:

```
module counter_opt (input clk , input reset , output q);
reg [2:0] count;
assign q = {count[2:0]==3'b100};
always @(posedge clk ,posedge reset)
begin
if(reset)
	count <= 3'b000;
else
	count <= count + 1;
end
endmodule
```
Run the below code for netlist:

```
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog counter_opt2.v

synth -top counter_opt

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr counter_opt_net.v

```
![Screenshot from 2024-10-22 11-32-10](https://github.com/user-attachments/assets/0bfa7bc6-818d-4f8f-bb58-a069ab2bd3ad)


![Screenshot from 2024-10-21 18-07-39](https://github.com/user-attachments/assets/758cc273-b58d-4286-a891-bf9f6756c58e)


GTKWave Output:

```
iverilog counter_opt2.v tb_counter_opt.v

./a.out

gtkwave tb_counter_opt.vcd
```

![Screenshot from 2024-10-21 18-10-22](https://github.com/user-attachments/assets/ae5620f9-2bbb-4a74-bddb-1da1d42d427c)




</details>


<details>
<summary><strong>Day 4:</strong></summary>

## Introduction to GLS and Synthesis-Simulation mismatch

Gate Level Simulation (GLS) is an important step in verifying digital circuits. It simulates the synthesized netlist, a lower-level representation of the design, using a testbench to check its logical accuracy and timing behavior. By comparing the simulation outputs with expected results, GLS ensures the synthesis process hasn't introduced any errors and that the design meets performance requirements.

Sensitivity lists are key for ensuring correct circuit behavior. An incomplete sensitivity list can result in unintended latches. Blocking and non-blocking assignments in `always` blocks behave differently, and improper use of blocking assignments can inadvertently create latches, leading to mismatches between synthesis and simulation. To prevent this, it's crucial to carefully review the circuit behavior and ensure the sensitivity list and assignments match the intended functionality.

![image](https://github.com/user-attachments/assets/fa59f230-4a0d-4c8e-9353-a01a9209a6d6)

**Example 1:**

Verilog code:

```
module ternary_operator_mux (input i0 , input i1 , input sel , output y);
assign y = sel?i1:i0;
endmodule
```

Simulation:

```
iverilog ternary_operator_mux.v tb_ternary_operator_mux.v

./a.out

gtkwave tb_ternary_operator_mux.vcd

```

![Screenshot from 2024-10-22 02-03-54](https://github.com/user-attachments/assets/94f7d3c2-ef3e-4389-9b4e-e9ded6ab81b3)


Netlist:

```
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog ternary_operator_mux.v

synth -top ternary_operator_mux

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr ternary_operator_mux_net.v

```


![Screenshot from 2024-10-22 02-05-02](https://github.com/user-attachments/assets/36be1935-6fcd-4b7e-a155-d7ab6bfdd33e)

![Screenshot from 2024-10-22 02-05-17](https://github.com/user-attachments/assets/e52d1e99-e4a8-4eb8-bf47-c5b7f302d068)


![Screenshot from 2024-10-22 02-06-23](https://github.com/user-attachments/assets/759657ba-c350-4a5a-8dd8-80951332cf8e)


GLS:

```
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v ternary_operator_mux_net.v tb_ternary_operator_mux.v

./a.out

gtkwave tb_ternary_operator_mux.vcd
```

![Screenshot from 2024-10-22 02-08-23](https://github.com/user-attachments/assets/8b1b7d63-df97-44f6-b160-48fa73056e03)



In this case there is no mismatch between the waveforms before and after synthesis

**Example 2:**

Verilog code:

```
module bad_mux (input i0 , input i1 , input sel , output reg y);
always @ (sel)
begin
	if(sel)
		y <= i1;
	else 
		y <= i0;
end
endmodule
```

Simulation:

```
iverilog bad_mux.v tb_bad_mux.v

./a.out

gtkwave tb_bad_mux.vcd
```


![Screenshot from 2024-10-22 02-09-40](https://github.com/user-attachments/assets/d6d5be5c-e9e9-4254-9f76-72cb383860a5)



Netlist:

```
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog bad_mux.v

synth -top bad_mux

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr bad_mux_net.v

```

![Screenshot from 2024-10-22 02-10-56](https://github.com/user-attachments/assets/6454e756-c8c3-463e-9613-d4571e8875af)

![Screenshot from 2024-10-22 02-10-32](https://github.com/user-attachments/assets/774c5655-aacb-48c3-a4b0-ac827bcab3ce)


![Screenshot from 2024-10-22 02-11-23](https://github.com/user-attachments/assets/ecc30df5-15d8-4606-acf2-cc4a6d4f12ab)


GLS:

```
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v bad_mux_net.v tb_bad_mux.v

./a.out

gtkwave tb_bad_mux.vcd
```

![Screenshot from 2024-10-22 02-12-49](https://github.com/user-attachments/assets/f571a283-fa61-48ec-a33a-e8a67a774a21)


In this case there is a synthesis and simulation mismatch. While performing synthesis yosys has corrected the sensitivity list error.

**Labs on Synthesis-Simulation mismatch for blocking statements**

Verilog code:

```
module blocking_caveat (input a , input b , input  c, output reg d); 
reg x;
always @ (*)
begin
d = x & c;
x = a | b;
end
endmodule
```

Simulation:

```
iverilog blocking_caveat.v tb_blocking_caveat.v

./a.out

gtkwave tb_blocking_caveat.vcd
```

![Screenshot from 2024-10-22 02-15-00](https://github.com/user-attachments/assets/066533fb-5555-4489-bc0d-0ace8fadd90d)



Netlist:

```
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog blocking_caveat.v

synth -top blocking_caveat

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr blocking_caveat_net.v
```

![Screenshot from 2024-10-22 02-15-46](https://github.com/user-attachments/assets/73a9d30c-56dc-4cb6-a31a-079c90cd4a70)


![Screenshot from 2024-10-22 02-15-56](https://github.com/user-attachments/assets/2f355f5c-d182-4594-9903-41c69f3e60e6)


![Screenshot from 2024-10-22 02-16-36](https://github.com/user-attachments/assets/28ea805a-592a-49b0-9e99-fd0855590c30)


GLS:

```
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v blocking_caveat_net.v tb_blocking_caveat.v

./a.out

gtkwave tb_blocking_caveat.vcd
```


![Screenshot from 2024-10-22 02-17-46](https://github.com/user-attachments/assets/87ce8522-cf87-4dcc-9029-723fc7b4b5ec)


In this case there is a synthesis and simulation mismatch. While performing synthesis yosys has corrected the latch error.

</details>

</details>

</details>



<details>
<summary><strong>Lab 10</strong></summary>
	
## Synthesizing RISC-V and comparing output with functional (RTL) simulation.


### Steps 

Copy the ```src``` folder from your ```BabySoC``` folder to your ```sky130RTLDesignAndSynthesisWorkshop``` folder in your ```VLSI``` folder from previous lab.

Go to the Directory: 

```
cd /home/tejas/VLSI/sky130RTLDesignAndSynthesisWorkshop/src/module
```

### Synthesis: 

```
yosys       

read_liberty -lib /home/tejas/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog clk_gate.v

read_verilog rvmyth.v

synth -top rvmyth

abc -liberty /home/tejas/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib

write_verilog -noattr rvmyth_net.v

!gvim rvmyth_net.v

exit
```
![Screenshot from 2024-10-24 01-06-20](https://github.com/user-attachments/assets/df4b5373-bcba-4cc6-bf30-b93e7e3051c9)
![Screenshot from 2024-10-24 01-11-00](https://github.com/user-attachments/assets/a4770927-109d-485f-837a-96851f80cef3)



Now to observe the output waveform of synthesised RISC-V
```
iverilog ../../my_lib/verilog_model/primitives.v ../../my_lib/verilog_model/sky130_fd_sc_hd.v rvmyth.v testbench.v vsdbabysoc.v avsddac.v avsdpll.v clk_gate.v

./a.out

gtkwave dump.vcd
```
![Screenshot from 2024-10-24 01-23-10](https://github.com/user-attachments/assets/0d5776e0-2b9a-4ac0-bcf6-b0920494ea79)

![final](https://github.com/user-attachments/assets/319139df-8d17-4740-856f-e89abfe2f060)

![Screenshot from 2024-10-24 01-21-20](https://github.com/user-attachments/assets/2563c28b-12f1-4cba-9e34-71b499891318)

### Realization: 

![PHOTO-2024-10-24-01-55-37](https://github.com/user-attachments/assets/f400bd2e-0285-4dcf-991a-46662df07a2b)

![PHOTO-2024-10-24-01-56-14](https://github.com/user-attachments/assets/9cb37544-d272-42b8-9006-3f3e86f2fc13)

## RTL Simulations

 ### Command Steps :
 ```
cd BabySoC

iverilog -o ./pre_synth_sim.out -DPRE_SYNTH_SIM src/module/testbench.v -I src/include -I src/module/

./pre_synth_sim.out

gtkwave pre_synth_sim.vcd

```

![Screenshot from 2024-10-24 01-28-10](https://github.com/user-attachments/assets/47f868c6-6a4e-479b-8b66-e0de71ba43b4)

![Screenshot from 2024-10-24 01-26-50](https://github.com/user-attachments/assets/f4a3cd3b-820b-454b-b076-50b49b00833f)

![Screenshot from 2024-10-24 01-27-33](https://github.com/user-attachments/assets/7a9c6e4a-77ee-4b66-8bae-606c4bc63e51)


 
</details>

<details>
<summary><strong>Lab11</strong></summary>
	

### STA for synthesized Risc-V core using time period of 10.9 ns.
To verify that our synthesized RISC-V Core module meets its timing constraints, we will generate setup and hold timing reports. These reports will confirm that data signals propagate correctly throughout the core. Run the following commands:
```
set PERIOD 10.9

set_units -time ns

create_clock [get_pins {pll/CLK}] -name clk -period $PERIOD
set_clock_uncertainty -setup  [expr $PERIOD * 0.05] [get_clocks clk]
set_clock_transition [expr $PERIOD * 0.05] [get_clocks clk]
set_clock_uncertainty -hold [expr $PERIOD * 0.08] [get_clocks clk]
set_input_transition [expr $PERIOD * 0.08] [get_ports ENb_CP]
set_input_transition [expr $PERIOD * 0.08] [get_ports ENb_VCO]
set_input_transition [expr $PERIOD * 0.08] [get_ports REF]
set_input_transition [expr $PERIOD * 0.08] [get_ports VCO_IN]
set_input_transition [expr $PERIOD * 0.08] [get_ports VREFH]
```
Now, run the below commands:
```
cd VSDBabySoc/src

sta

read_liberty -min ./lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_liberty -min ./lib/avsdpll.lib

read_liberty -min ./lib/avsddac.lib

read_liberty -max ./lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_liberty -max ./lib/avsdpll.lib

read_liberty -max ./lib/avsddac.lib

read_verilog ../output/synth/vsdbabysoc.synth.v

link_design vsdbabysoc

read_sdc ./sdc/vsdbabysoc_synthesis.sdc

report_checks -path_delay min_max -format full_clock_expanded -digits 4
```

![Screenshot from 2024-10-29 00-13-26](https://github.com/user-attachments/assets/245f6051-2b42-4a17-87b4-63f3ab01339d)


Setup Time:

![Screenshot from 2024-10-28 23-44-47](https://github.com/user-attachments/assets/e337e543-ea13-459f-8c85-f82869c5f58e)

Hold Time:

![Screenshot from 2024-10-28 23-44-30](https://github.com/user-attachments/assets/99456d0c-ad8f-4c26-ad38-ed52f4fc368f)

</details>


<details>
<summary><strong>Lab 12</strong></summary>

## STA comparison for various sky libraries

Store all the sky `lib` files in a folder named `timing_libs`. Now, go to `VSDBabySoC/src` and create a tickle file `sta_across_pvt.tcl` . The below block is the content of the tickle file:

```
set list_of_lib_files(1) "sky130_fd_sc_hd__ff_100C_1v65.lib"
set list_of_lib_files(2) "sky130_fd_sc_hd__ff_100C_1v95.lib"
set list_of_lib_files(3) "sky130_fd_sc_hd__ff_n40C_1v56.lib"
set list_of_lib_files(4) "sky130_fd_sc_hd__ff_n40C_1v65.lib"
set list_of_lib_files(5) "sky130_fd_sc_hd__ff_n40C_1v76.lib"
set list_of_lib_files(6) "sky130_fd_sc_hd__ff_n40C_1v95.lib"
set list_of_lib_files(7) "sky130_fd_sc_hd__ff_n40C_1v95_ccsnoise.lib.part1"
set list_of_lib_files(8) "sky130_fd_sc_hd__ff_n40C_1v95_ccsnoise.lib.part2"
set list_of_lib_files(9) "sky130_fd_sc_hd__ff_n40C_1v95_ccsnoise.lib.part3"
set list_of_lib_files(10) "sky130_fd_sc_hd__ss_100C_1v40.lib"
set list_of_lib_files(11) "sky130_fd_sc_hd__ss_100C_1v60.lib"
set list_of_lib_files(12) "sky130_fd_sc_hd__ss_n40C_1v28.lib"
set list_of_lib_files(13) "sky130_fd_sc_hd__ss_n40C_1v35.lib"
set list_of_lib_files(14) "sky130_fd_sc_hd__ss_n40C_1v40.lib"
set list_of_lib_files(15) "sky130_fd_sc_hd__ss_n40C_1v44.lib"
set list_of_lib_files(16) "sky130_fd_sc_hd__ss_n40C_1v60.lib"
set list_of_lib_files(17) "sky130_fd_sc_hd__ss_n40C_1v60_ccsnoise.lib.part1"
set list_of_lib_files(18) "sky130_fd_sc_hd__ss_n40C_1v60_ccsnoise.lib.part2"
set list_of_lib_files(19) "sky130_fd_sc_hd__ss_n40C_1v60_ccsnoise.lib.part3"
set list_of_lib_files(20) "sky130_fd_sc_hd__ss_n40C_1v76.lib"
set list_of_lib_files(21) "sky130_fd_sc_hd__tt_025C_1v80.lib"
set list_of_lib_files(22) "sky130_fd_sc_hd__tt_100C_1v80.lib"

for {set i 1} {$i <= [array size list_of_lib_files]} {incr i} {
read_liberty ./timing_libs/$list_of_lib_files($i)
read_verilog ../output/synth/vsdbabysoc.synth.v
link_design vsdbabysoc
read_sdc ./sdc/vsdbabysoc_synthesis.sdc
check_setup -verbose
report_checks -path_delay min_max -fields {nets cap slew input_pins fanout} -digits {4} > ./sta_output/min_max_$list_of_lib_files($i).txt

}

```

![Screenshot from 2024-11-04 18-31-29](https://github.com/user-attachments/assets/f044a446-b187-41df-9095-d9026046c006)


constraints file:

![Screenshot from 2024-11-04 23-10-27](https://github.com/user-attachments/assets/2c69dd1f-8ea6-4a12-97db-48ac9c106fd2)



Now, run the following commands:

```
cd VSDBabySoC/src

sta

source sta_across_pvt.tcl
```

![Screenshot from 2024-11-04 17-48-59](https://github.com/user-attachments/assets/f27eb01a-640f-4e4c-a0ad-6e141471f68a)

table:

![Screenshot from 2024-11-04 17-40-29](https://github.com/user-attachments/assets/a3956a3b-0445-49f9-bcef-a346a22f2d8c)



Graphs:

**Worst Setup Slack:**

![image](https://github.com/user-attachments/assets/33e153fc-1b12-4916-9a60-17fcbe1c62a1)



**Worst Hold Slack:**

![image](https://github.com/user-attachments/assets/a5105677-7f14-48ae-a035-0013a205fd03)



<details>
<summary><strong>Timing Reports:</strong></summary>
	
**sky130_fd_sc_hd__ff_100C_1v65.lib:**
![Screenshot from 2024-11-04 17-54-15](https://github.com/user-attachments/assets/d84ea20e-f566-4959-94d8-f1e4568ea545)

![Screenshot from 2024-11-04 17-54-30](https://github.com/user-attachments/assets/09e15494-c5fc-45e2-b5e3-6d284dce6e7c)



**sky130_fd_sc_hd__ff_100C_1v95.lib:**

![Screenshot from 2024-11-04 17-54-51](https://github.com/user-attachments/assets/85221735-09ed-48a5-b598-7d2c202af44d)
![Screenshot from 2024-11-04 17-55-05](https://github.com/user-attachments/assets/dde3a009-9305-444b-b2e5-9fc2f257206f)



**sky130_fd_sc_hd__ff_n40C_1v56.lib:**

![Screenshot from 2024-11-04 17-55-20](https://github.com/user-attachments/assets/bdf104ce-92c7-4841-b777-51bcc2a03bc8)

![Screenshot from 2024-11-04 17-55-41](https://github.com/user-attachments/assets/9a782463-1d61-4388-b552-f5967db18fc6)

**sky130_fd_sc_hd__ff_n40C_1v65.lib:**
![Screenshot from 2024-11-04 17-56-00](https://github.com/user-attachments/assets/13055a32-9f09-4e3f-b6e8-308682dd5cd8)

![Screenshot from 2024-11-04 17-56-12](https://github.com/user-attachments/assets/467b1367-9b9e-474d-bdb9-c0cd2eb0cdb8)


**sky130_fd_sc_hd__ff_n40C_1v76.lib:**
![Screenshot from 2024-11-04 17-56-30](https://github.com/user-attachments/assets/b720f071-35bf-4690-9ce5-fc3b74ea6b2e)

![Screenshot from 2024-11-04 17-56-44](https://github.com/user-attachments/assets/b5bef9e4-7b08-4a38-b3f0-80d0b51b8d4f)


**sky130_fd_sc_hd__ff_n40C_1v95.lib:**
![Screenshot from 2024-11-04 18-01-07](https://github.com/user-attachments/assets/25af82f6-25ee-4667-a8df-cdd2b069f02f)
![Screenshot from 2024-11-04 18-01-18](https://github.com/user-attachments/assets/b46693bf-4d28-480e-9fdf-9faa47ba41d5)



**sky130_fd_sc_hd__ff_n40C_1v95_ccsnoise.lib.part1:**

![Screenshot from 2024-11-04 18-01-35](https://github.com/user-attachments/assets/867289fe-8893-4d48-bf75-b037d4645ebe)
![Screenshot from 2024-11-04 18-01-50](https://github.com/user-attachments/assets/fc154de1-ca27-4c04-b565-f98195e2d12b)


**sky130_fd_sc_hd__ff_n40C_1v95_ccsnoise.lib.part2:**
![Screenshot from 2024-11-04 18-02-07](https://github.com/user-attachments/assets/9f667121-4e8c-4c75-93f0-da27cb32892c)

![Screenshot from 2024-11-04 18-02-20](https://github.com/user-attachments/assets/80f313b2-1121-4f39-846e-a5a784e64fae)


**sky130_fd_sc_hd__ff_n40C_1v95_ccsnoise.lib.part3:**
![Screenshot from 2024-11-04 18-02-38](https://github.com/user-attachments/assets/a2c66a2b-eda2-4eb4-b407-9e0d5b1dec45)

![Screenshot from 2024-11-04 18-02-49](https://github.com/user-attachments/assets/ba9a459e-8c7b-4852-bcba-e183a791178d)



**sky130_fd_sc_hd__ss_100C_1v40.lib:**
![Screenshot from 2024-11-04 18-03-09](https://github.com/user-attachments/assets/ace0a8c1-41ee-4917-9341-a293dce2059f)
![Screenshot from 2024-11-04 18-03-25](https://github.com/user-attachments/assets/6f28f496-5f74-45a9-a72e-fd3f999408f2)



**sky130_fd_sc_hd__ss_100C_1v60.lib:**

![Screenshot from 2024-11-04 18-07-58](https://github.com/user-attachments/assets/1656b1cd-4133-4ea2-be0d-3cde8114a3da)

![Screenshot from 2024-11-04 18-08-12](https://github.com/user-attachments/assets/ed69aba0-931f-42f6-a83a-84a68cf1bfa3)

**sky130_fd_sc_hd__ss_n40C_1v28.lib:**
![Screenshot from 2024-11-04 18-08-27](https://github.com/user-attachments/assets/493e2d5e-9c83-4afb-abac-a0b424278209)

![Screenshot from 2024-11-04 18-08-42](https://github.com/user-attachments/assets/2719058f-c98f-41a0-adea-3da18b27cfc5)


**sky130_fd_sc_hd__ss_n40C_1v35.lib:**
![Screenshot from 2024-11-04 18-09-09](https://github.com/user-attachments/assets/f795171a-fbd3-458f-987f-beb063297a7e)
![Screenshot from 2024-11-04 18-09-25](https://github.com/user-attachments/assets/135e33f8-ef83-4120-ade1-aa37094852fb)


**sky130_fd_sc_hd__ss_n40C_1v40.lib:**

![Screenshot from 2024-11-04 18-09-45](https://github.com/user-attachments/assets/935867ff-024c-4863-91d7-d0e1bd2efb79)

![Screenshot from 2024-11-04 18-09-56](https://github.com/user-attachments/assets/d6e73496-c19c-441d-bd32-d95b8ebc9ff2)

**sky130_fd_sc_hd__ss_n40C_1v44.lib:**

![Screenshot from 2024-11-04 18-10-10](https://github.com/user-attachments/assets/e5ec4d36-7111-4465-90b5-5d4f907661b6)

![Screenshot from 2024-11-04 18-10-21](https://github.com/user-attachments/assets/a9c6e636-4a18-449f-ab0d-1ad72f48b4bc)

**sky130_fd_sc_hd__ss_n40C_1v60.lib:**

![Screenshot from 2024-11-04 18-10-38](https://github.com/user-attachments/assets/187738da-e8aa-464b-92f3-d1893d76fe7c)
![Screenshot from 2024-11-04 18-10-47](https://github.com/user-attachments/assets/3fb5a71e-e0e3-4ec0-ae8e-ece594584dd3)


**sky130_fd_sc_hd__ss_n40C_1v60_ccsnoise.lib.part1:**

![Screenshot from 2024-11-04 18-11-03](https://github.com/user-attachments/assets/14df8a9f-306f-4eca-bafd-8625f3118493)

![Screenshot from 2024-11-04 18-11-15](https://github.com/user-attachments/assets/3f1d7443-2932-42d9-97df-217b8e196d4d)


**sky130_fd_sc_hd__ss_n40C_1v60_ccsnoise.lib.part2:**
![Screenshot from 2024-11-04 18-11-30](https://github.com/user-attachments/assets/94f8a8a3-700e-4946-8fd6-a13422830075)

![Screenshot from 2024-11-04 18-11-46](https://github.com/user-attachments/assets/796b9c6f-1998-4ebc-85fe-a6a1e014315f)


**sky130_fd_sc_hd__ss_n40C_1v60_ccsnoise.lib.part3:**
![Screenshot from 2024-11-04 18-12-01](https://github.com/user-attachments/assets/99b9a361-31bc-443b-8859-8add547eedd9)

![Screenshot from 2024-11-04 18-12-16](https://github.com/user-attachments/assets/c5aff0f5-c530-4a53-82da-5ea0e965586e)


**sky130_fd_sc_hd__ss_n40C_1v76.lib:**
![Screenshot from 2024-11-04 18-12-31](https://github.com/user-attachments/assets/6b252ac7-c5f1-481f-b744-d58e13c8b7dd)

![Screenshot from 2024-11-04 18-12-41](https://github.com/user-attachments/assets/1ab92b48-50de-4267-aa3a-7a6b080929c7)


**sky130_fd_sc_hd__tt_025C_1v80.lib:**
![Screenshot from 2024-11-04 18-12-57](https://github.com/user-attachments/assets/f91d40cc-a7a6-4dc3-8ef3-6ca0c6a4d47d)
![Screenshot from 2024-11-04 18-13-11](https://github.com/user-attachments/assets/3910b804-491f-4bbf-8de8-a30bfe759304)


**sky130_fd_sc_hd__tt_100C_1v80.lib:**
![Screenshot from 2024-11-04 18-13-32](https://github.com/user-attachments/assets/8f68853c-f818-43c9-8e0b-df56ba5c06a8)

![Screenshot from 2024-11-04 18-13-47](https://github.com/user-attachments/assets/9a76227b-25fa-4744-b1db-6c75827783a8)
</details>
</details>
<details>
<summary><strong>Lab 13</strong></summary>

<details>
<summary><strong>Day 1</strong> Inception of open-source EDA, OpenLane and Sky130 PDK</summary>

## Inception of open-source EDA, OpenLane and Sky130 PDK

**QFN-48 Package**: The QFN-48 is a compact leadless package with 48 connection pads around its perimeter, providing excellent thermal and electrical performance, ideal for high-density applications.

![Screenshot from 2024-11-11 14-32-57](https://github.com/user-attachments/assets/3f1237c7-c6a6-453f-9655-636f8778eb9c)

**Chip**: An integrated circuit (IC) on a silicon substrate with various functional blocks like memory, processors, and I/O, designed for specific electronics applications.

![Screenshot from 2024-11-11 14-33-11](https://github.com/user-attachments/assets/32a46c31-bde4-46b7-807a-5f09e7a42a82)

**Pads**: Small metallic contact points on a chip or package, used to connect internal circuitry with external connections, enabling signal transfer.

**Core**: The main processing unit of a chip, containing functional logic optimized for power and performance.

**Die**: The section of a silicon wafer containing an individual IC before packaging, hosting all the active circuitry of the chip.

![Screenshot from 2024-11-11 14-33-23](https://github.com/user-attachments/assets/ff797bc6-ea8e-4bd9-be22-e3cf8ce4df95)

**IPs (Intellectual Properties)**: Pre-designed functional modules, such as USB controllers or memory interfaces, licensed for reuse in various designs to save development time and costs.

![Screenshot from 2024-11-11 14-33-32](https://github.com/user-attachments/assets/4e0b10e5-eea2-4b68-a42c-3db071423e38)

---

### Software to Hardware Execution Flow

When running an application on hardware, it first enters the system software layer, which translates it to binary form. Key components here include the Operating System (OS), Compiler, and Assembler.

1. **OS**: Breaks down application functions written in high-level languages (e.g., C, Java) and passes them to a compiler.
2. **Compiler**: Translates functions into hardware-specific low-level instructions.
3. **Assembler**: Converts these instructions into binary code, which the hardware can execute.

![Screenshot from 2024-11-11 14-33-50](https://github.com/user-attachments/assets/0f07a5f9-9ce0-457e-a7af-1caffa64f809)

For instance, take a stopwatch app running on a RISC-V core. The OS generates a function in C, which is sent to the compiler. The compiler produces RISC-V-specific instructions adapted to the architecture. These instructions are then processed by the assembler, converting them to binary code. This binary code is then integrated into the chip layout, allowing the hardware to execute the intended functionality.

![Screenshot from 2024-11-11 14-34-04](https://github.com/user-attachments/assets/1d9b1e0e-1170-4709-98b9-3f2f71f28098)

For the above stopwatch the below figure shows the input and output of the compiler and assembler.

![Screenshot from 2024-11-11 14-34-16](https://github.com/user-attachments/assets/205f26fb-62b5-40d4-8bf2-ef96fc9ff711)

The compiler produces instructions specific to the architecture, and the assembler converts these into corresponding binary patterns. To execute them on hardware, an RTL (written in a Hardware Description Language) interprets and implements the instructions. This RTL design is then synthesized into a netlist, represented by interconnected logic gates. Finally, the netlist undergoes physical design implementation, preparing it for chip fabrication.

![Screenshot from 2024-11-11 14-34-28](https://github.com/user-attachments/assets/f4df4c33-62e9-4a78-9f14-98282aa9c2a2)

---

### Components of ASIC Design

- **RTL IPs**: Verified circuit blocks (adders, flip-flops) in HDL, used to accelerate complex designs.
- **EDA Tools**: Automates ASIC tasks like synthesis, optimization, and timing analysis, ensuring performance meets requirements.
- **PDK Data**: Semiconductor foundry files defining the manufacturing process, ensuring ASIC designs are fabrication-ready.

![Screenshot from 2024-11-11 14-34-40](https://github.com/user-attachments/assets/610fc21f-8262-4d4b-b868-01a9d84b40ca)

---

### Simplified RTL to GDSII Flow

![Screenshot from 2024-11-11 14-34-49](https://github.com/user-attachments/assets/9d8d2664-476d-4cd6-b5b3-7e0aed241a2f)

1. **RTL Design**: Describes circuit function using HDLs like Verilog or VHDL.
2. **RTL Synthesis**: Converts RTL to a gate-level netlist with optimized cells.
3. **Floor and Power Planning**: Lays out major components, power grid, and I/O.
4. **Placement**: Allocates cells to minimize wirelength and signal delay.
5. **Clock Tree Synthesis (CTS)**: Distributes clock signals uniformly to reduce skew.
6. **Routing**: Connects components while meeting design rules.
7. **Sign-off**: Final verification, confirming design readiness for fabrication.
8. **GDSII Generation**: Creates the layout for chip production.

---

### OpenLane ASIC Flow Overview

![Screenshot from 2024-11-11 14-34-58](https://github.com/user-attachments/assets/414fbce3-7665-4375-9123-42a11f41ccec)

1. **RTL Synthesis and Technology Mapping**: Uses Yosys and ABC.
2. **Static Timing Analysis**: Uses OpenSTA.
3. **Floor Planning**: Utilizes init_fp, ioPlacer, pdn, tapcell.
4. **Placement**: Managed by RePLace, Resizer, OpenPhySyn, and OpenDP.
5. **Clock Tree Synthesis**: Uses TritonCTS.
6. **Fill Insertion**: Uses OpenDP.
7. **Routing**: Uses FastRoute/CU-GR (global) and TritonRoute/DR-CU (detailed).
8. **SPEF Extraction**: OpenRCX for parasitic data.
9. **GDSII Output**: Uses Magic and KLayout.
10. **Design Rule Checks**: Magic and KLayout.
11. **Layout vs. Schematic Check**: Uses Netgen.
12. **Antenna Checks**: Handled by Magic.

---

### OpenLane Directory Structure

```plaintext
├── OpenLane            # Tool directory
│   ├── designs         # Holds all designs
│   │   └── picorv32a   # Example design
├── pdks                # PDK-related files
│   ├── skywater-pdk    # Skywater 130nm PDKs
│   ├── open-pdks       # Scripts for open-source tool compatibility
│   ├── sky130A         # Open-source compatible PDK variant
│   │   ├── libs.ref    # Node-specific files (e.g., timing, tech LEF)
│   │   ├── libs.tech   # Tool-specific files (e.g., for KLayout)
```

### Running Synthesis in OpenLane

1. **Navigate and start OpenLane**:
   ```bash
   cd Desktop/work/tools/openlane_working_dir/openlane
   docker
   ./flow.tcl -interactive
   package require openlane 0.9
   prep -design picorv32a
   run_synthesis
   ```

  ![Screenshot 2024-11-13 190133](https://github.com/user-attachments/assets/05ec0450-3aec-4c43-9f7a-1e45dc9081ff)



3. **View the Netlist**:
   ```bash
   cd designs/picorv32a/runs/13-11_13-29/results/synthesis/
   gedit picorv32a.synthesis.v
   ```

  ![Screenshot 2024-11-13 190913](https://github.com/user-attachments/assets/434e1082-37f7-42d4-8b82-563e0799acce)


5. **Yosys Report**:
   ```bash
   cd ../..
   cd reports/synthesis
   gedit 1-yosys_4.stat.rpt
   ```

  <img width="475" alt="image" src="https://github.com/user-attachments/assets/1a2fa30b-18cd-467c-830a-7dc1db8e9536">


  

**Report:**

```
28. Printing statistics.

=== picorv32a ===

   Number of wires:              14596
   Number of wire bits:          14978
   Number of public wires:        1565
   Number of public wire bits:    1947
   Number of memories:               0
   Number of memory bits:            0
   Number of processes:              0
   Number of cells:              14876
     sky130_fd_sc_hd__a2111o_2       1
     sky130_fd_sc_hd__a211o_2       35
     sky130_fd_sc_hd__a211oi_2      60
     sky130_fd_sc_hd__a21bo_2      149
     sky130_fd_sc_hd__a21boi_2       8
     sky130_fd_sc_hd__a21o_2        57
     sky130_fd_sc_hd__a21oi_2      244
     sky130_fd_sc_hd__a221o_2       86
     sky130_fd_sc_hd__a22o_2      1013
     sky130_fd_sc_hd__a2bb2o_2    1748
     sky130_fd_sc_hd__a2bb2oi_2     81
     sky130_fd_sc_hd__a311o_2        2
     sky130_fd_sc_hd__a31o_2        49
     sky130_fd_sc_hd__a31oi_2        7
     sky130_fd_sc_hd__a32o_2        46
     sky130_fd_sc_hd__a41o_2         1
     sky130_fd_sc_hd__and2_2       157
     sky130_fd_sc_hd__and3_2        58
     sky130_fd_sc_hd__and4_2       345
     sky130_fd_sc_hd__and4b_2        1
     sky130_fd_sc_hd__buf_1       1656
     sky130_fd_sc_hd__buf_2          8
     sky130_fd_sc_hd__conb_1        42
     sky130_fd_sc_hd__dfxtp_2     1613
     sky130_fd_sc_hd__inv_2       1615
     sky130_fd_sc_hd__mux2_1      1224
     sky130_fd_sc_hd__mux2_2         2
     sky130_fd_sc_hd__mux4_1       221
     sky130_fd_sc_hd__nand2_2       78
     sky130_fd_sc_hd__nor2_2       524
     sky130_fd_sc_hd__nor2b_2        1
     sky130_fd_sc_hd__nor3_2        42
     sky130_fd_sc_hd__nor4_2         1
     sky130_fd_sc_hd__o2111a_2       2
     sky130_fd_sc_hd__o211a_2       69
     sky130_fd_sc_hd__o211ai_2       6
     sky130_fd_sc_hd__o21a_2        54
     sky130_fd_sc_hd__o21ai_2      141
     sky130_fd_sc_hd__o21ba_2      209
     sky130_fd_sc_hd__o21bai_2       1
     sky130_fd_sc_hd__o221a_2      204
     sky130_fd_sc_hd__o221ai_2       7
     sky130_fd_sc_hd__o22a_2      1312
     sky130_fd_sc_hd__o22ai_2       59
     sky130_fd_sc_hd__o2bb2a_2     119
     sky130_fd_sc_hd__o2bb2ai_2     92
     sky130_fd_sc_hd__o311a_2        8
     sky130_fd_sc_hd__o31a_2        19
     sky130_fd_sc_hd__o31ai_2        1
     sky130_fd_sc_hd__o32a_2       109
     sky130_fd_sc_hd__o41a_2         2
     sky130_fd_sc_hd__or2_2       1088
     sky130_fd_sc_hd__or2b_2        25
     sky130_fd_sc_hd__or3_2         68
     sky130_fd_sc_hd__or3b_2         5
     sky130_fd_sc_hd__or4_2         93
     sky130_fd_sc_hd__or4b_2         6
     sky130_fd_sc_hd__or4bb_2        2

   Chip area for module '\picorv32a': 147712.918400

```

**Report Summary**:

```
Flop ratio = Number of D Flip flops = 1613  = 0.1084
             ______________________   _____
             Total Number of cells    14876
```

- **Wire Count**: 14,596
- **Cell Count**: 14,876, including specific cells like `sky130_fd_sc_hd__a2111o_2`, `sky130_fd_sc_hd__and2_2`.
- **D Flip-flops**: 1,613 with a flop ratio of 0.1084

</details>

<details>
<summary><strong>Day-2:</strong>  Good floorplan vs bad floorplan and introduction to library cells </summary>

## Good floorplan vs bad floorplan and introduction to library cells 
 
### **Utilization Factor and Aspect Ratio**
In IC floor planning, the utilization factor and aspect ratio are key parameters. The utilization factor is defined as the area occupied by the netlist divided by the total core area. Although a utilization factor of 1 (100%) is ideal, practical designs aim for a factor of 0.5 to 0.6 to provide room for buffer zones, routing channels, and adjustments. The aspect ratio, calculated as height divided by width, determines the shape of the chip. An aspect ratio of 1 indicates a square, while other values create a rectangular layout, with the specific ratio chosen based on functional, packaging, and manufacturing requirements.

```
Utilization Factor =  Area occupied by netlist
                     __________________________
                         Total area of core
                         

Aspect Ratio =  Height
               ________
                Width
```

### **Pre-placed Cells**
Pre-placed cells are key functional blocks—like memory, custom processors, and analog circuits—that are placed manually in fixed positions. These blocks are crucial for performance and are kept fixed during placement and routing to maintain functionality and layout integrity.

### **Decoupling Capacitors**
Decoupling capacitors stabilize power supply voltages near logic circuits during transient events. Acting as local energy reserves, they reduce voltage fluctuations, crosstalk, and EMI, ensuring reliable power delivery to sensitive circuits.

### **Power Planning**
Effective power planning involves creating a power and ground mesh to evenly distribute VDD and VSS across the chip. This arrangement stabilizes power delivery, reduces voltage drops, and enhances efficiency. Multiple power and ground points further support power needs and reduce the risks of instability.

### **Pin Placement**
Strategic pin placement, or I/O planning, is essential for functionality and reliability. Proper pin assignments minimize signal degradation, manage heat dissipation, and support thermal stability. Placing power and ground pins strategically enhances both signal strength and manufacturability.

### **Floorplanning with OpenLANE**
To initiate floorplanning in OpenLANE, execute these commands:

```
cd Desktop/work/tools/openlane_working_dir/openlane
docker
```

```
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
run_synthesis
run_floorplan
```

![Screenshot 2024-11-13 192356](https://github.com/user-attachments/assets/f676b6cd-1a50-45fd-b8ad-8bbf41ffdcf4)

![Screenshot 2024-11-13 192454](https://github.com/user-attachments/assets/4fcabc84-5242-4f85-b1ac-51707d75f126)

Then, in a new terminal, access the floorplan file as follows:

```
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_13-50/results/floorplan
gedit picorv32a.floorplan.def
```
![Screenshot 2024-11-13 193056](https://github.com/user-attachments/assets/fc55d2a8-4fe2-4415-9773-f04c9d3f65c4)


According to the floorplan definitions:

- 1000 Unit Distance = 1 Micron
- Die width in unit distance = 660685−0 = 660685  
- Die height in unit distance = 671405−0 = 671405  
- Width in microns = 660685 / 1000 = 660.685 Microns  
- Height in microns = 671405 / 1000 = 671.405 Microns  
- Die area in microns² = 660.685 × 671.405 = 443587.212425 Microns²  

To view the floorplan in Magic:

```
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_13-50/results/floorplan/
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```
![Screenshot from 2024-11-13 19-45-37](https://github.com/user-attachments/assets/c1c1fbd9-6e30-48c9-9b5f-abc1d2e464e3)



### **Decap and Tap Cells**

Decap cells and tap cells are placed to manage power delivery and maintain electrical connectivity across the chip.

![Screenshot from 2024-11-13 19-57-18](https://github.com/user-attachments/assets/40769f01-c69e-4821-9290-08e36ef56f8c)



### **Unplaced Standard Cells at Origin**
![Screenshot from 2024-11-13 19-58-42](https://github.com/user-attachments/assets/d5bb10fe-af43-48f5-83ec-980139d1a474)



```
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
run_synthesis
run_floorplan
run_placement
```

![Screenshot from 2024-11-13 20-02-15](https://github.com/user-attachments/assets/2722b2e9-9421-4848-8b03-d3f6a89436c9)


View the placement in Magic with:

```
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_14-30/results/placement/
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```
![Screenshot from 2024-11-13 20-03-43](https://github.com/user-attachments/assets/f73efee0-dfb2-403f-ab01-cdf582892110)

![Screenshot from 2024-11-13 20-05-07](https://github.com/user-attachments/assets/c71af639-9f87-45b6-9c4c-bb5c05ccf292)


### **Cell Design and Characterization Flow**
The library provides essential cell information, including various sizes, functionalities, and threshold voltages. A typical design flow includes:

- Inputs: Process Design Kits (PDKs) for DRC & LVS, SPICE models, and library/user-defined specs.
- Steps: Circuit design, layout design (using techniques like Euler’s path and stick diagrams), parasitic extraction, and characterization (timing, noise, power).
- Outputs: CDL, LEF, GDSII, extracted SPICE netlist (.cir), and timing/noise/power .lib files.

### **Standard Cell Characterization Flow**
Standard cell characterization includes:

1. Reading models and tech files
2. Loading the extracted SPICE netlist
3. Recognizing cell behavior
4. Reading subcircuits
5. Applying power sources
6. Stimulating characterization setup
7. Setting output capacitance loads
8. Running simulation commands

These steps are processed by the GUNA software to produce timing, noise, and power models.

### **Timing Parameters**

| Timing Definition     | Value       |
|-----------------------|-------------|
| slew_low_rise_thr     | 20%         |
| slew_high_rise_thr    | 80%         |
| slew_low_fall_thr     | 20%         |
| slew_high_fall_thr    | 80%         |
| in_rise_thr           | 50%         |
| in_fall_thr           | 50%         |
| out_rise_thr          | 50%         |
| out_fall_thr          | 50%         |

**Propagation Delay**: Time for an input change to reach 50% of its final value and affect the output similarly.

```
rise delay =  time(out_fall_thr) - time(in_rise_thr)
```

**Transition Time**: Time taken for signal transitions between 10% to 90% or 20% to 80% of signal levels.

```
Fall transition time: time(slew_high_fall_thr) - time(slew_low_fall_thr)
Rise transition time: time(slew_high_rise_thr) - time(slew_low_rise_thr)
```

</details>

<details>
<summary><strong>Day-3:</strong> Design library cell using Magic Layout and ngspice characterization </summary>

## CMOS Inverter NGSPICE Simulations

#### SPICE Deck for a CMOS Inverter Simulation

1. **Netlist Creation**: Define a netlist for the CMOS inverter circuit, labeling nodes clearly (e.g., `input`, `output`, `ground`, and `supply`) for easier identification during SPICE simulation.

2. **Device Sizing**: Define the Width-to-Length (W/L) ratios for the PMOS and NMOS transistors. Typically, the PMOS width is 2x to 3x the NMOS width for balanced drive strength.

3. **Voltage Levels**: Set gate and supply voltages in multiples of the transistor length.

4. **Node Naming**: Assign names (e.g., `VDD`, `GND`, `IN`, `OUT`) to nodes around each component in the SPICE netlist for easier reference.

   <img width="953" alt="Screenshot 2024-11-12 at 11 23 10 PM" src="https://github.com/user-attachments/assets/cbd5664c-8342-4f0c-9cc9-1320c3d1ca68">


5. **Simulation Commands**: For transient analysis, use:
   ``` 
   source [filename].cir 
   run 
   setplot 
   dc1 
   plot out vs in 
   ```

   <img width="938" alt="Screenshot 2024-11-12 at 11 23 32 PM" src="https://github.com/user-attachments/assets/4339530d-407c-4be8-a2de-ecdac84d3523">

#### Simulation and Analysis

- **Switching Threshold (Vm)**: Vm is the input voltage at which the inverter output switches between logic levels. For equal PMOS/NMOS sizes, Vm is around VDD/2. Adjusting PMOS or NMOS sizes shifts Vm higher or lower.

- **SPICE Command for Threshold Calculation**:
   ``` 
   Vin in 0 2.5 
   .op 
   .dc Vin 0 2.5 0.05 
   ```

    <img width="950" alt="Screenshot 2024-11-12 at 11 23 49 PM" src="https://github.com/user-attachments/assets/09eb989c-2a66-4225-b15a-2ded2d9299ee">


- **Transient Analysis for Propagation Delay**: SPICE command for pulse input:
   ``` 
   Vin in 0 0 pulse 0 2.5 0 10p 10p 1n 2n 
   .op 
   .tran 10p 4n 
   ```

    <img width="884" alt="Screenshot 2024-11-12 at 11 28 02 PM" src="https://github.com/user-attachments/assets/4f45f9d0-43ab-494e-8e18-c775f27509ae">

#### Cloning the Custom Inverter Layout

1. Clone the inverter:
   ``` 
   cd Desktop/work/tools/openlane_working_dir/openlane 
   git clone https://github.com/nickson-jose/vsdstdcelldesign 
   cd vsdstdcelldesign 
   cp /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech . 
   magic -T sky130A.tech sky130_inv.mag & 
   ```

   ![Screenshot from 2024-11-12 23-41-52](https://github.com/user-attachments/assets/7a9892ee-2b6f-4b28-98e1-b0f55c74fc26)

#### CMOS Fabrication Process (16 Masks)

1. **Substrate Preparation**: The process begins with silicon wafer preparation as the base for the circuit.

2. **N-Well Formation**: Phosphorus impurities are implanted or diffused to create N-well regions on the substrate.

3. **P-Well Formation**: P-well regions are formed similarly, using boron or other dopants through ion implantation or diffusion.

4. **Gate Oxide Deposition**: A thin layer of silicon dioxide is deposited to act as the gate insulator.

5. **Polysilicon Deposition**: A polysilicon layer is deposited on the gate oxide to form the gate electrode.

6. **Polysilicon Masking and Etching**: Areas where polysilicon should remain are defined with a photoresist mask, and unprotected sections are etched away.

7. **N-Well Masking and Implantation**: A mask preserves N-well regions while phosphorus or similar impurities are implanted in other areas.

8. **P-Well Masking and Implantation**: A mask protects P-well regions, allowing boron or equivalent dopants to be implanted elsewhere.

9. **Source/Drain Implantation**: Using masks, dopants (e.g., arsenic for NMOS, boron for PMOS) are implanted to form the source and drain regions.

10. **Gate Formation**: The gate electrode is defined by etching the polysilicon layer, guided by a mask.

11. **Source/Drain Masking and Etching**: Masks define the source and drain areas, followed by etching to clear the oxide from those regions.

12. **Contact/Via Formation**: Holes are etched through the oxide layer to expose areas like the source/drain or polysilicon gates for contacts.

13. **Metal Deposition**: A metal layer, usually aluminum or copper, is deposited to create electrical interconnections.

14. **Metal Masking and Etching**: Metal interconnect patterns are defined using a photoresist mask, with etching to remove unneeded metal.

15. **Passivation Layer Deposition**: A protective silicon dioxide or nitride layer is applied to shield metal interconnections.

16. **Final Testing and Packaging**: Rigorous testing ensures chip functionality, after which functional chips are separated, packaged, and prepared for deployment.

    <img width="773" alt="Screenshot 2024-11-12 at 11 46 46 PM" src="https://github.com/user-attachments/assets/1b5b48f3-ee2d-47a5-8ebd-41614f33b050">

#### Inverter Layout

![Screenshot from 2024-11-12 23-54-13](https://github.com/user-attachments/assets/2a56754f-c532-49fd-9b5b-fb856f12012a)

![Screenshot from 2024-11-12 23-54-26](https://github.com/user-attachments/assets/7674039b-6ed1-4ed1-9897-902f042e7096)

![Screenshot from 2024-11-12 23-54-39](https://github.com/user-attachments/assets/7d601e97-0fd2-458a-908f-90a123c57d53)

![Screenshot from 2024-11-12 23-55-26](https://github.com/user-attachments/assets/1445b939-6717-4fb0-8ff4-7a4edb397fbd)

![Screenshot from 2024-11-12 23-55-53](https://github.com/user-attachments/assets/da582617-e15b-47c5-b1f5-bd0777e9461e)

#### SPICE Extraction with Magic

Run commands in `tkcon` to extract and generate SPICE:
``` 
pwd 
extract all 
ext2spice cthresh 0 rthresh 0 
ext2spice 
```

![Screenshot from 2024-11-13 00-06-58](https://github.com/user-attachments/assets/da8b3fa8-5e39-480e-a472-58a994479b1a)

#### Modifying SPICE File for Transient Analysis

![Screenshot from 2024-11-13 00-07-35](https://github.com/user-attachments/assets/caa04964-e21d-49b0-9f7b-4c76b94fda3c)

![Screenshot from 2024-11-13 00-07-42](https://github.com/user-attachments/assets/0e09dbd7-2872-48c8-981a-b9ab25284ab6)

Edit `sky130_inv.spice`:
```
* SPICE3 file created from sky130_inv.ext - technology: sky130A

.option scale=0.01u
.include ./libs/pshort.lib
.include ./libs/nshort.lib

M1000 Y A VGND VGND nshort_model.0 w=35 l=23
+  ad=1.44n pd=0.152m as=1.37n ps=0.148m
M1001 Y A VPWR VPWR pshort_model.0 w=37 l=23
+  ad=1.44n pd=0.152m as=1.52n ps=0.156m

VDD VPWR 0 3.3V
VSS VGND 0 0V
Va A VGND PULSE(0V 3.3V 0 0.1ns 0.1ns 2ns 4ns)

.tran 1n 20n
.control
run
.endc
.end
```

To simulate:
``` 
ngspice sky130_inv.spice 
plot y vs time a 
```
![Screenshot from 2024-11-13 00-12-43](https://github.com/user-attachments/assets/86b19c94-3ce9-4e95-8aa3-6833c976202e)

![Screenshot from 2024-11-13 00-13-12](https://github.com/user-attachments/assets/defbf5aa-4975-4f56-bcf1-af4b032145fd)


#### Characterizing Slew Rate and Propagation Delay

Using transient response:
- **Rise Transition**: Time for output to rise from 20% to 80% of max value.
- **Fall Transition**: Time for output to fall from 80% to 20%.
- **Cell Rise/Fall Delay**: Difference in time for 50% output change compared to input transition.

Example calculations:
```
Rise Transition : 2.24638 - 2.18242 =  0.06396 ns = 63.96 ps
Fall Transition : 4.0955 - 4.05536 =  0.0419 ns = 41.9 ps
Cell Rise Delay : 2.21144 - 2.15008 = 0.06136 ns = 61.36 ps
Cell Fall Delay : 4.07807 - 4.05 =0.02 ns = 20 ps
```

#### Magic Tool DRC Rules Check

Set up and run:
``` 
cd 
wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz 
tar xfz drc_tests.tgz 
cd drc_tests 
gvim .magicrc 
magic -d XR & 
```

![Screenshot from 2024-11-13 00-18-20](https://github.com/user-attachments/assets/421d67f3-8c6e-46b8-9af5-df1339da4c91)

![Screenshot from 2024-11-13 00-21-58](https://github.com/user-attachments/assets/dc4f6182-faba-4842-a6cb-b1c8f35bb9ae)

Run DRC commands:
``` 
tech load sky130A.tech 
drc check 
drc why 
```

![Screenshot from 2024-11-13 00-33-24](https://github.com/user-attachments/assets/b99c626f-6e9f-4312-a934-31b964dc1a5c)

![Screenshot from 2024-11-13 00-35-16](https://github.com/user-attachments/assets/fe5c1e08-92e6-46b5-9791-34731fd28a46)

<img width="805" alt="Screenshot 2024-11-13 at 12 38 16 AM" src="https://github.com/user-attachments/assets/ddccab54-9579-42a6-80e2-a4b74dc21eea">

</details>

</details>












