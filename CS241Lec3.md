

- control Algorithm

  ```assembly
  PC<- 0
  while(1){
    fetch word from RAM at address in PC(RAM[PC]) in to IR
    PC<- PC+4
    execute instruction in IR
  }
  ```



- Jump and link register

  ```assembly
  jalr $s

  #
  tmp = $s
  31 = PC
  PC = tmp
  ```

  ​

## Mips Reference

- Add `add $d, $s, $t` …… `$d = $s + $t` (mod 2^32)

- Subtract `sub $d, $s, $t` …… `$d = $s - $t` (mod 2^32)

- Load Immadiate & Skip `lis $d` …… `$d = RAM[PC]`

  ​								 `PC = PC + 4`

- `$0` always contains 0

- Load Word `lw $t, i($s)`…… `$t = RAM[$s+i]` ($s+i == 0(mod 4))

- Store Word `sw $t, i($s)`……`Ram[$s+i] = $t`

- Set Lessthan `slt $d, $s, $t`……`$d = ($s < $t)`

- Set Less than Unsign `sltu $d, $s, $t`……`%d = ($s < $t)`

- Branch Equal `beq $s, $t, i`……`if $s == $t PC = PC + 4i`

- Branch Not Equal `bne $s, $t, i`……`if $s != $t PC = PC + 4i`

  - But ooo I wanted `beq $1, $2, 1000000` too big!

  - `.word 1000000`

  - ```assembly
    bneq $1, $2, 1
    jr $3
    ```

- Move from HI `mfhi $d`……`$d = HI`

- Move from LO `mflo $d`……`$d = LO`

- Divide `dive $s, $t`……``

- Multiply Unsigned `multu $s, $t`

- Divide Unsigned `divu $s, $t`

## Doc 2

- 2. Mips Assembly Language (us c241 edition)

- Mips assembly language program is text file 

- Each line is either … NULL or cantains instruction each line:

  - (0 or more labels)      (0 or 1 instructions); comment

  - e.g.

    ```assembly
    jr $3
    jr $3; hello world
    ```

    ```assembly
    foo: bar: diff: beq $1, $1, 1
    boo:
    	; comment
    bah:; comment
    ```

- each non-null line contains instruction:

  - mnemonic
    - `.word`
    - `add`
    - `sub`
    - `mult`
    - etc...
  - [operands]
    - 0xabc    123 — 123
    - upto 8 digits 2^32 -1
    - \- 2^32

