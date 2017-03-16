## Recall

- e.g.

  ```assembly
  lis $3
  .word 24
  jr $31
  ```

- | P0   | asm  | =>   | merl1    |
  | ---- | ---- | ---- | -------- |
  | P1   | asm  | =>   | merl2    |
  | ...  | ...  | ...  | ...      |
  | Pn-1 | asm  | =>   | merl n-1 |

- merl(i):

  |   header(hi)    |
  | :-------------: |
  |   program(pi)   |
  | footer(fi)(B+k) |

  ​

- Bi (location of Pi in merl) = 12

- $$
  \hat\beta_i\ = \beta_i + \sum_j^i |P_j|
  $$

- |    hQ    |
  | :------: |
  |    P0    |
  |    P1    |
  |    P2    |
  | .word 42 |
  |   ...    |
  |  P(n-1)  |
  |    f0    |
  |    f1    |
  |   ...    |

  - the locatio of `.word 42`
    $$
    \hat\beta_i + k = \beta_i + \sum_j^i |P_j| + k
    $$


------

- P1

  ```assembly
  .import P2
  lis  $3
  .word P2
  jr $3
  ```

- P2

  ```assembly
  .export P2
  add $1, $1, $2
  jr $31
  ```

  ​

## ESR External Symbol Reference

```assembly
lis $3
.word P2		;(fill in P2 location here !)
jr $31
```

- Format in MERL
  - `0x11` (1 word)
  - location to be fixed
  - N (length of symbol chars)
  - N <u>words</u> (symbal in ascii)

## ESD External Symbol Definition 

- ESD: word containing `0x05` (ESD)
- value of symbol
- N (length of symbol)
- N word siring symbol in ascii

## Using linker

- `cat merl1 merl2 merl3 merl4 ... | linker > merlX`
- `cat merlX more.merl | linker ...`
- `run merlX(provided merlX has no ESRs)`
- Libraries
  - container with lots of MERLs
  - `zip lib.zip merl1 merl2 ..... merln`
  - `linker lib.zip < mymerl > mylinked`
- Linux `ar` instead of `zip`
  - `runlib builds maleX`

## Dynamic Link Libraries (DLL)

- Linux `.so`

- merls => final linker => 

- dynamic linker                mips program

  - look up symbol(parameter)
  - put address in $3
  - jar $3

  ```assembly
  lis $x
  .word link.stub
  jalr $x
  ```

  - `link.stub` point `$1` to "fred"

    ```assembly
    lis $3
    ; dynamic linker
    jalr $3
    ```

    ​