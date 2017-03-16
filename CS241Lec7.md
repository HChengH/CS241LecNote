## Loaders(Relocation & Linkers)

- Externial Loader

  - `mips.twoints`

- Assemblem -> MIPS/Binary ->Externial Loader -> mips program

  ​						 ->Loading -> mips program

  ​						 ->Read -> Loader

  ​				      		 Loader -> Loader

- Initial Program Load

- Bootstrap Loader

  ```assembly
  				; location(B)
  lis $3			; 0
  .word ft		; 4
  lw $3, 0($3)	; 8
  jr $31			; 12
  ft: .word 42	; 16
  ```

- E.g.

  ```assembly
  ; preasmble(header)								     
  .word 0x10000002	;cookie							 ;0
  .word 40			;overall length(binary file) in  ;4
  					;   bytes						 
  .word 32			;location of first sticky note	 ;8

  ; program
  lis $3												 ;12
  .word 28 + a										 ;16
  lw $3 0($3)											 ;20
  jr $31												 ;24
  .word 42											 ;28

  ; sticky note
  .word 1			    ;please relocate something		 ;32 
  					;   for me!
  .word 16			;please add a to the word		 ;36
  					;   at location  B
  ```

- MERL file

  - mips
  - Executable
  - Relocatable(Loadable/Linkable)
  - format

## Loader

- figure out length N of program to beloaded

  - read 4 bytes
  - assert 4 bytes  == 0x10000002

- find N bytes of available RAM(at address a)

  - store `0x10000002` at a
  - store N at a + 4
  - read N - 8 bytes i nto RAM starting at a + 8

- sticky

  ```c++
  m = RAM[a+8];
  for(p=a+m; p<a+N;){
    if (RAM[p] = 1){
      RAM[p+4] = B; //a location in program/a+B is the 
      			  //   corresponding address
      RAM[a+B] += a;
    }
    p+=8;
  }
  ```

- jalr $r(a)

  - location 			address

    B				a+B

    B1-B2			(a+B1)-(a+B2)

    B+k				a+B+k

## Creating MERL files

- hard way — CS241.binasm

- easier way — CS241.relasm

  ```assembly
  .word beq $0, $0, 8 ;(0x10000002)
  .word  end
  .word sticky
  lis $3
  fixme:.word ft
  lw $3, 0($3)
  jr $31

  ft: .word 42
  sticky:
  	.word 1
  	.word fixme
  end:
  ```

## Linkers

- multiple assembly program files