

- `xxd ` show as hex
  - `-b` show as binary
- `fred.hex`
  - `.word 0x46726564`

------

- `java cs241.wordasm < fred.hex > foofoo.in`


- `xdd foofoo.bin`
- `0000000: 4672 6564        Fred`

## Stored Program Computer

- mips - silicon Graphics

- Two parts

  - central Processing Unit(CPU)
    - Control unit
    - Arithmatic Logic Unit (ALI)
    - PC
    - IR
    - R
  - Random Access Memory (RAM / Core)

- Control Unit Algorithm

  ```MIPS
  PC <— [constant known value e.g. 0]
  while(1){
    fetch a word from RAM whose address is in the PC
    put it in IR
    PC <- PC+4
    decode and execute instruction from IR
    }
  ```

  ```assembly
  jr $31
  #"jump registor"
  jr sssss
  #PC <- sssss
  ```