; multi-segment executable file template.

data segment
    ; add your data here!
    intro db ">> Hello please enter a number of 5 digits max: $"
    Enter db 13h
    binaryPrint db " *Binary Representation : $"
    hexPrint db " *Hex Representation : $"
    octalPrint db " *Octal Representation : $"
    positiveNumber db " -It is a positive number! $"
    negativeNumber db " -It is a negative number! $"
    even db " - It is an Even number! $"
    odd db " - It is an Odd number! $"
   
ends

stack segment
    dw   128  dup(0)
ends

code segment
start:

; set segment registers:

    mov ax, data
    mov ds, ax
    mov es, ax

    ; add your code here
            
lea dx, intro 
mov ah, 9
int 21h     
;mov cx , 0



;;;;;;;;;;;;;;;
mov cx , 5
mov ax , 0
mov bx , 0
mov si , 0 ;  (dl == 0)? + , -



user_input:

mov ah , 1 
int 21H
cmp al , '-'
JZ signBit
cmp al , 0Dh
JZ exit
cmp al , "A"
jge letterInput
cmp al , "+"
je letterInput
mov ah , 0
mov di , ax


mov dx , 0AH
mov ax , bx
mul dx
mov bx , ax

mov ax , di

sub al ,30h

cbw
add bx , ax


jmp DONESign

signBit:
mov si , 1
add cx , 1
jmp DONESign

letterInput:
add cx ,1

DONESign:
loop user_input

exit:

call NewLine

cmp si , 0
JZ positive ; +

neg bx ; 2's complement
lea dx , negativeNumber
mov ah , 9
int 21h 
jmp skipPositive

positive:
lea dx , positiveNumber
mov ah , 9
int 21h 

skipPositive:

test bx , 1 ;odd
JZ evenPrint
lea dx , odd
mov ah , 9
int 21h 
jmp Binary

evenPrint:
lea dx , even
mov ah , 9
int 21h 


Binary:

call NewLine

lea dx , binaryPrint
mov ah , 9
int 21h 
    
mov di , bx
mov cx , 16

PrintBinary:

shl di , 1
JC printOne

mov dl , 30h
mov ah , 2
int 21h
jmp printedZero

printOne:
mov dl , 31h
mov ah , 2
int 21h
printedZero:
loop PrintBinary
ends

call NewLine

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;Hex:

lea dx , hexPrint
mov ah , 9
int 21h

mov si , 08000h
mov cx , 4

FourHexDigits:

mov dx , 0
push cx
mov cx , 4

innerLoop:
mov ax ,bx
and ax , si
JZ skipAdding
add dl , 1
skipAdding:

shr si , 1
cmp cx , 1 
je done
shl dl , 1
done:
loop innerLoop
pop cx

cmp dl , 0AH
jl number

;letter
add dl , 37h
mov ah , 2
int 21h
jmp skimNum

number:
add dl , 30h
mov ah , 2
int 21h

skimNum:
loop FourHexDigits


;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

;octal

call NewLine

lea dx , octalPrint
mov ah , 9
int 21h

shl bx ,1
JC OctalOne

mov dl , 30h
mov ah , 2
int 21h
jmp OctalZero

OctalOne:
mov dl , 31h
mov ah , 2
int 21h
OctalZero:

mov si , 08000h
mov cx , 5


FiveOctalDigits:

mov dx , 0
push cx
mov cx , 3

inLoop:
mov ax ,bx
and ax , si
JZ skipAdd
add dl , 1
skipAdd:

shr si , 1
cmp cx , 1 
je finished
shl dl , 1
finished:
loop inLoop
pop cx

add dl , 30h
mov ah , 2
int 21h

loop FiveOctalDigits


;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

;mouse activition 
 mov cx , 0
; loop read mouse
MouseLP:
mov ax , 0
int 33h
  
;Show mouse
mov ax,1h
int 33h
mov cx , 0
mov dx , 0
mov bx , 0
mov ax,3h
int 33h
test bx, 01h ; check left mouse click
jz skipLeft
call MouseLeft
skipLeft:
test bx, 02h ; check right mouse click
jz skipRight
call MouseRight ;4Eh
skipRight:
test bx, 03h ; check left mouse click
jz skipBoth
call Both ;
skipBoth:
loop MouseLP


;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

NewLine:
push dx
push ax

mov dl , 0Ah
mov ah , 2
int 21h
mov dl , 0Dh
mov ah , 2
int 21h
pop ax
pop dx
RET

MouseLeft:
;div cx and dx by 8
mov bx , 8
mov ax , cx
div bl
mov bx ,2
mul bl
mov ah ,0
mov SI , ax

mov bx , 8
mov ax , dx
div bl
mov bx , 160
mul bl 
;mov ah ,0
mov BX , ax


mov ax , 0B800h
mov es , ax
mov es:[BX+SI+1] , 05h ;green 
Ret

 
 
MouseRight:
;div cx and dx by 8
mov bx , 8
mov ax , cx
div bl
mov bl ,2
mul bl
mov ah ,0
mov SI , ax

mov bx , 8
mov ax , dx
div bl
mov bx , 160
mul bl 
;mov ah ,0
mov BX , ax 


mov ax , 0B800h
mov es , ax
mov es:[BX+SI+1] , 0ch ;red
Ret

 

Both:
;div cx and dx by 8
mov bx , 8
mov ax , cx
div bl
mov bl ,2
mul bl
mov ah ,0
mov SI , ax

mov bx , 8
mov ax , dx
div bl
mov bx , 160
mul bl 
;mov ah ,0
mov BX , ax 


mov ax , 0B800h
mov es , ax
mov es:[BX+SI+1] ,bh ;3Eh ;lightblue
Ret
 
 
end start ; set entry point and stop the assembler.