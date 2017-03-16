## Recall

```assembly
; sum 1 to N(N = $1) result = $3 -> no registers other than $3 changes

sumto:
; prologue
; save registers
sw $7, -4($30)
sw $8, -8($30)
sw $9, -12($30)

add $3, $0, $0
lis $7
.word 1
loop:
	slt $8, $1, $7
	bne $8, $0, endloop
	add $3, $3, $7
	lis $9
	.word 1
	add $7, $7, %9
	beq $0, $0, loop
endloop:
jr $31

;Epilogue
lw $7, -4($30)
lw $8, -8($30)
lw $12, -12($30)
```

```assembly
;  sum 1 to n, result in $3, 1 to m, result in $4 N = $1 m = $2

;Prologue
sw $5, -4($30)
sw $31, -8($30)
sw $6, -12($30)
;Subtract from sp(stack pointer) push
lis $6
.word 12
sub $30, $30, $6

; call sumto($1, $2) -> $3
lis $31
.word sumto
jalr $31
add $5, $3, $0

; call sumto($) -> $3
add $1, $2, $0
lis $31
.word sumto
jalr $31

add $4, $3, $0
add $3, $5, $0

;Epilogue
; add back to sp(stack pointer) pop
add $30, $30, $6

lw $5, -4($30)
lw $31, -8($30)
lw $6, -12($30)

jr $31
```

```assembly
; recursive sumto(N) sumto(0)= 0 sumto(N) = N + sumto(N-1)

Sumto:
	sw $1, -4($30)
	sw $11, -8($30)
	sw $4, -12($30)
	sw $31, -16($30)
	lis $4
	.word 16
	sub $30, $30, $4
	bne $1, $0, doit
	add $3, $0, $0
	beq $0, $0, done
doit:
	lis $11
	.word 1
	sub $1, $1, $11
	lis $31
	.word Sumto
	jalr $31
	add $1, $1, $11
	add $3, $1, $3
done:
	add $30, $30, $4
	lw $1, -4($30)
	lw $11, -8($30)
	lw $4, -12($30)
	lw $31, -16($30)
	
jr $31

```

```assembly
; writ a single line of text  "Hello"(ended by New line)

lis $1
.word 0xffff000c
lis $2
.word 72
sw $2, 0($1) ; ASCII H
lis $2
.word 101
sw $2 0($1) ; ASCII e
lis $2
.word 108
sw $2 0($1) ; ASCII l
sw $2 0($1)
lis $2
.word 111
sw $2, 0($1) ;ASCII o
lis $2
.word 10
sw $2, 0($1)

jr $31
```

```assembly
; "cat"
lis $1
.word 0xffff0004
lis $3
.word -1
loop:
	lw $2, 0($1) ; read 1 byte
	beq $2, $3, quit
	sw $2, 8($1)
	beq $0, $0, loop
quit:
	jr $31
```

- `java mips.array foo.mips`

  - allocates N adjacent words in memory
  - `$1` address of 1st word
  - `$2` N

  ```assembly
  ; multiply $r by 4
  add $r, $r, $r
  add $r, $r, $r
  add $r, $1, $r
  sw $s, 0($r)
  lw $s, 0($r)
  ```

  ​

## How to write an Assembler

- Take a assembly language file(text file/source file, source code) to a binary mips file(object file/object code)
  - analysis
    - scanner
    - …...
  - synthesis