## Assembly Language Programming

```assembly
; Return, doing nothing

foo:		;command
	jr $31 ;return to pt of gold
	
```

- assemble and give some unknow binary code… and run in mips computer

- Sum integers from 1 to N

  - how is stuff represented?
    - N : $1
    - Sum : $3
    - i : $7

- In C++

  ```c++
  int sum = 0;
  for(int i = 0; i < N; i++)
  {
    sum +=i;
  }
  return sum;
  ```

- In Assembly

  ```assembly
  ;; sum from 1 to N
  ; N : $1
  ; sum : $3
  ; i : $7

  ; sum = 0
  add $3,$0,$0
  ; i = 1
  lis $7
  .word 1

  slt $8,$1,$7 ; N < i?
  ben $8,$0,5   ; yes, break!
  add $3,$3,$7
  lis $10
  .word 1
  add $7,$7,$10
  beq $0,$0,-7

  jr $31
  ```

- Every line has location L(line)

  - L(line) = 4 * (number of non-null lines that precede "line")
  - L(label) = L(line where label is defined)

- Store 42 in RAM at address 1000

  ```assembly
  ; store 42 at RAM address 1000

  lis $4
  .word 42
  sw $4, 1000($0)
  jr $31
  ```

  ​