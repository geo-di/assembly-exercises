# Y86 Assembly Programming Exercises

## Summary

This repo contains several Y86 assembly programming exercises, including fundamental operations such as multiplication and division, as well as more advanced tasks like computing string lengths, averages, and square roots. Additionally, it covers bitwise operations and efficient multiplication techniques. These exercises help in understanding low-level programming and optimizing assembly code for better performance.

**Note:** The provided solutions do not include initialization. Therefore, the following initialization:

```
.pos 0
irmovq $2,%rax
irmovq $3,%rbx   
main:
   addq %rax,%rbx
   halt
```

Would instead be written as:

```
main:
   addq %rax,%rbx
   halt
```

## Multiplication

Implement multiplication using Y86 assembly language. Store the multiplicand in register `%rdi` and the multiplier in `%rsi`, with the result placed in `%rax`.

**Implementation:**

```
%rax = %rdi * %rsi
```

---

## Division

Since Y86 does not have a division instruction, implement division manually.

- Store the dividend in `%rdi` and the divisor in `%rsi`.
- The quotient should be stored in `%rax`.
- Ignore decimals and rounding; only return the lower integer part.

**Example:** `5 / 2 = 2`

**Implementation:**

```
%rax = %rdi / %rsi
```

---

## String Length Calculation

Write Y86 assembly code to compute the length of a string stored in memory, starting at address `0x400`. The result should be returned in `%rax`.

**Note:** Strings are ASCII-encoded and terminate with a null character (`0x00`). Each character occupies `64` bits in memory.

**Example:**

```
Memory Layout:
0x0400: 0x4100000000000000
0x0408: 0x4200000000000000
0x0410: 0x4300000000000000
0x0418: 0x4400000000000000
0x0420: 0x0000000000000000
```

`strlen` would return `4`.

---

## Average Calculation

Write a Y86 assembly program that calculates the average of numbers stored in the stack. Return the average in `%rax`.

**Hint:** Use stack registers `%rsp` and `%rbp` or the `pushq` and `popq` commands.

**Number of steps in a stack with 5 numbers:** `95`

---

## Finding Maximum and Minimum Values

Write a Y86 assembly program to find the smallest and largest numbers in the stack.

- Store the smallest value in `%rsi`.
- Store the largest value in `%rdi`.

**Hint:** Use stack registers. The stack will be automatically initialized.

**Number of steps in a stack with 12 numbers:** `148`

---

## Extracting Ambient Light Sensor Data

A SensorTag ambient light sensor stores illumination data in a 16-bit register, with two values:

- `E[3:0]`: Extracted into `%r14`
- `R[11:0]`: Extracted into `%r13`

**Input Register:** `%r13`

**Output Registers:**

```
%r13 = R[11:0]
%r14 = E[3:0]
```

**Number of steps for a 5-digit number:** `42`

---

## Square Root Calculation

Implement a square root calculation using the following algorithm. The number to be processed is stored in `%r12`, and the result should be stored in `%rcx`.

```c
int32_t res = 0;
int32_t bit = 1 << 16;
while (bit > num) {
   bit >>= 2;
}
while (bit != 0) {
   if (num >= res + bit) {
      num -= res + bit;
      res = (res >> 1) + bit;
   } else {
      res >>= 1;
   }
   bit >>= 2;
}
```

**Example:**

```
sqrt(2345) = 48.425200051 → 48
```

The final result must be an integer, always rounding down.

**Register Mapping:**

```
%rcx = sqrt(%r12)
```

**Number of steps for a medium-sized 5-digit number:** `<3500`

---

## Efficient Multiplication Using Bitwise Operations

Implement an optimized multiplication algorithm by examining the bits of the multiplier:

1. If a bit is `0`, the corresponding partial product is `0`.
2. If a bit is `1`, compute the multiplication using bit shifts.
3. Sum the partial products to obtain the final result.

**Registers Used:**

```
Input: %r11 (Multiplicand), %r12 (Multiplier)
Output: %rax (Result)
```

This method improves efficiency compared to naive addition-based multiplication.

**Number of steps for a 9-digit number:** `<300`

---

## Fibonacci Number Detection

Implement a Y86 assembly program to determine if a given number is a Fibonacci number. The program should use the following logic:

### C Reference Implementation:

```c
bool isPerfectSquare(int x) {
    int s = sqrt(x);
    return (s*s == x);
}
  
bool isFibonacci(int x) {
    return isPerfectSquare(5*x*x + 4) || isPerfectSquare(5*x*x - 4);
}
```

The program stops execution when it encounters the first number that is not a Fibonacci number. This number should be returned in the `%rax` register. If all numbers in the sequence are Fibonacci numbers, the program should return `0` in `%rax`.

**Number of steps for array with 4-6 numbers <1000:** `<21000`

---

