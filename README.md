# 13-Digit-ISBN-Checker
# This project is built in Assembly Language (MIPS). It is used to verify whether the ISBN number is valid or not.
# ======================================================================================================================
# CODE:
.data
arr: .space 56
msg1: .asciiz "Enter 13 numbers of ISBN: "
msg2: .asciiz "\n"
msg3: .asciiz "\nValid ISBN"
msg4: .asciiz "\nInvalid ISBN"
msg5: .asciiz "Sum1: "
msg6: .asciiz "\nSum2: "
msg7: .asciiz "\nTotal: "
.text
.globl multiply 
.globl takeMod
.globl checkMod
.globl checkISBN
main:
			            	# VALID ISBN: 9781861972712 OR 9783161484100. YOU CAN ENTER ANY OTHER ISBN TOO. 
# print msg
li $v0, 4
la $a0, msg1 
syscall

la $t6, arr #initializing array

li $t0, 0 # i = 0
li $t1, 12  # n = 13
li $t2, 0   
input_loop:
bgt $t0, $t1, end_input_loop  # 0 >12
li $v0, 5
syscall
move $t2, $v0
sw $t2, 0($t6)
addi $t0, $t0, 1
addi $t6, $t6, 4
j input_loop

end_input_loop:

la $t6,arr

jal multiply # multiply (odd position * 1, even position * 3)

jal takeMod # add odd and even variables in TOTAL variable and take a mod of it

jal checkMod # check if mod = 0 or not. if 0 then its valid unless value will be subtracted by 10

jal checkISBN


li $v0, 10
syscall

multiply: # function

li $t0, 0 # i = 0
li $t1, 12  # n = 12

li $t3, 0  # sum1
li $t4, 0  # sum2


For:
beq $t0,$t1, end

lw $t2, 0($t6)
li $t7, 2 #mod

div $t0, $t7 # divide answer in $t0 -> i % 2

mfhi $s0 # move from hi register

beq $s0, $zero, label  # if( i==0 )

li $t5, 0  #odd
li $s4, 3 # *3
mul $t5, $t2, $s4   # odd = arr[i] * 1
add $t4, $t4, $t5  # sum1 += odd

addi $t0, $t0, 1 
addi $t6, $t6, 4 #addi $s0, $s0, 4
j For
label:

li $t5, 0  #even
li $s2, 1 # *1
mul $t5, $t2, $s2  # even = arr[i] * 1
add $t3, $t3, $t5   # sum2 += even

#li $t5, 2 #mod
addi $t0, $t0, 1  
addi $t6, $t6, 4 #addi $s0, $s0, 4

j For

end:
li $v0, 4
la $a0, msg2 
syscall

li $v0, 4
la $a0, msg5
syscall

li $v0, 1
move $a0, $t4	# print sum1
syscall


li $v0, 4
la $a0, msg6 
syscall

li $v0, 1
move $a0, $t3	# print sum2
syscall



jr $ra

takeMod: #function
li $t7, 0  # total


add $t7, $t3, $t4


li $v0, 4
la $a0, msg7 
syscall

li $v0, 1
move $a0, $t7	# print total
syscall

li $t4, 0
li $t6, 10  # %10
div $t7, $t6 # divide answer in $t0 -> total % 10

mfhi $t4 # move from hi register

bne $t4, $zero, innnvalid

li $v0, 4
la $a0, msg3  # print valid msg 
syscall

li $v0, 10
syscall

innnvalid:


jr $ra

checkMod: #function
li $t6, 10  # -10
li $t3, 0
##beq $t4, $zero, validdd

sub $t3, $t6, $t4

jr $ra

checkISBN: #function

la $t6, arr
lw  $t2, 48($t6)
move $t1, $t2


beq $t3, $t1, Finalvalid

li $v0, 4
la $a0, msg4
syscall

j finish

Finalvalid:
li $v0, 4
la $a0, msg3
syscall

finish:
li $v0, 10
syscall
jr $ra


