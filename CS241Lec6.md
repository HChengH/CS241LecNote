## Low-level Details

- Assambler just a program 

  - c++
  - Racket
  - Scala

- Reads lines from source

  - c++ `getline`
  - Racket (read-line)

- Breaks them up in sequence of token

  - e.g.

    ```assembly
    beq $1, $6, foo
    ; (opcode beq) (register 1) comma (register 6) comma (label foo:)
    ```

## Synthesis

- how to output bits!

  - for every non-null line of source, output 32 bits

  - C/C++ allow you write words but **<u>DONT DO THIS!</u>**

  - C++ — use C stdin library

    ```c
    putchar(67);	// output 01000011 (Binary)
    				// 43 (hex)
    				// C (ascii)
    putchar(0x43);
    putchar('C');
    putchar(0x12345643);	// C
    						// 1 byte(loweest order byte)
    ```

  - Racket

    ```Racket
    (write-byte 67)
    (write-byte #x43)
    (write-byte #x12345643)	 ERROR!
    (write-byte (bitwise-and #x12345643 $xff))
    $xff mask/bitmask
    ```

    ```c++
    0x12345643 & 0xff; // -> 0x43
    ```

- Out put a word

  - C++

    ```c++
    int w = 0x12345643;
    // output 1st byte
    putchar(w>>24);
    // output 2nd byte
    putchar(w>>16);
    // output 3rd byte
    putchar(w>>8);
    // output 4th byte
    putchar(w);
    ```

  - Racket

    ```racket
    (write-byte (bitwise-and (arithmetic-shift w -24) #xff))
    (write-byte (bitwise-and (arithmetic-shift w -16) #xff))
    (write-byte (bitwise-and (arithmetic-shift w -8) #xff))
    (write-byte (bitwise-and w #xff))
    ```

- Translate mips instruction to machine language

  ```c++
  int x;
  // encode mips instruction in x
  // output x, as 4 bytes
  // e.g. encode add $1, $2, $3
  int template = 0x00000020;
  int s = 2 << 21; // (s followed by 21 0's)
  int t = 3 << 16; // (t followed by 16 0's)
  int d = 1 << 11; // (d followed by 11 0's)
  x = template + s + t + d
  ```

  ```assembly
  add $d, $s, $t
  0000 00ss ssst tttt dddd dd00 0010 0000
  ; template:
  add $d, $s,$t
  0000 0000 0000 0000 0000 0000 0010 0000
  ```



## Two-Pass Assembler

- Pass 1
  - Input
    - Source
  - Output
    - Symbel Table(dictionary)
      - label...
    - intermediate representation
      - (non null instruction….)
  - Error message(stderr)
    - ​
- Pass 2
  - Input
    -  ​
  - Output
    - Object

------

### Pass 1

- for every line of input
  - location_counter = 0
  - read the line
  - scan the line into a list of tokens
  - for each label at the start of the list
    - if label in smbel table(already)
    - ERROR to stderr
  - add < label, location_counter > to symbel table
  - if next token is valid opcode
    - if remaing tokens are valid sequence of operands for opcode
      - output representation of instruction to instermediate code location_counter t = 4
    - else ERROR to  stderr & quit
  - else if there is a token other than opcode
    - ERROR + quit

### Pass 2

- location_counter = 0
- for every instruction in intermediate rep
  - construct MIPS represantation for instruction
    - template
    - register operands
    - number operands
    - labels — look up label in symbol table 
      - not found: ERROR + quit
      - found: check value is in range 
        - if not: ERROR + quit
      - else encode value
  - out put MIPS instruction
  - location_count = location_count + 4