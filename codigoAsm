.data
    # Arrays e buffers
    numeros:        .space 400        
    entrada_buffer: .space 1024       
    saida_buffer:   .space 12         
    
    
    arquivo_entrada: .asciiz "D:/Asus Prime A520M-E/Documents/trabalhoASM/lista.txt"
    arquivo_saida:   .asciiz "D:/Asus Prime A520M-E/Documents/trabalhoASM/lista_ordenada.txt"
    virgula_espaco: .asciiz ", "      
.text
.globl main

main:
    
    li $t9, 0              
    
ler_arquivo:
    
    li $v0, 13
    la $a0, arquivo_entrada
    li $a1, 0              
    syscall
    move $s0, $v0          
    
    

    
    
    li $v0, 14
    move $a0, $s0
    la $a1, entrada_buffer
    li $a2, 1024
    syscall
    
   
    li $v0, 16
    move $a0, $s0
    syscall

processar_numeros:
    la $s1, entrada_buffer  
    la $s2, numeros        
    
loop_processamento:
    lb $t0, ($s1)          
    
    
    beqz $t0, iniciar_ordenacao
    beq $t9, 100, iniciar_ordenacao
    
    
    beq $t0, 10, proximo_char   
    beq $t0, 13, proximo_char    
    beq $t0, 32, proximo_char    
    beq $t0, ',', proximo_char   
    
    
    li $t1, 0              
    li $t2, 1              
    
   
    bne $t0, '-', digito
    li $t2, -1
    addi $s1, $s1, 1
    lb $t0, ($s1)
    
digito:
    
    blt $t0, '0', salvar_numero
    bgt $t0, '9', salvar_numero
    
   
    subi $t0, $t0, 48      
    mul $t1, $t1, 10
    add $t1, $t1, $t0
    
   
    addi $s1, $s1, 1
    lb $t0, ($s1)
    j digito
    
salvar_numero:
    mul $t1, $t1, $t2      
    sw $t1, ($s2)          
    addi $s2, $s2, 4       
    addi $t9, $t9, 1       
    
proximo_char:
    addi $s1, $s1, 1
    j loop_processamento

iniciar_ordenacao:
    
    la $s0, numeros        
    move $s1, $t9          
    
loop_externo:
    beqz $s1, escrever_arquivo   
    li $s3, 0                     
    
loop_interno:
    beq $s3, $s1, proximo_externo 
    sll $t0, $s3, 2              
    add $t0, $s0, $t0            
    lw $t1, ($t0)                
    lw $t2, 4($t0)               
    
    
    ble $t1, $t2, sem_troca
    sw $t2, ($t0)                
    sw $t1, 4($t0)
    
sem_troca:
    addi $s3, $s3, 1           
    j loop_interno
    
proximo_externo:
    addi $s1, $s1, -1           
    j loop_externo

escrever_arquivo:
    
    li $v0, 13
    la $a0, arquivo_saida
    li $a1, 1              
    syscall
    move $s0, $v0
    

    
    
    la $s1, numeros
    li $s2, 0              
    
loop_escrita:
    beq $s2, $t9, finalizar
    
   
    lw $t0, ($s1)
    la $t1, saida_buffer
    li $t2, 0              
    
    
    bgez $t0, numero_positivo
    li $t3, '-'
    sb $t3, ($t1)
    addi $t1, $t1, 1
    addi $t2, $t2, 1
    neg $t0, $t0
    
numero_positivo:
    
    beqz $t0, escrever_zero
    
    move $t3, $t1          
    
converter_digitos:
    beqz $t0, reverter
    divu $t0, $t0, 10
    mfhi $t4              
    mflo $t0               
    addi $t4, $t4, '0'
    sb $t4, ($t1)
    addi $t1, $t1, 1
    addi $t2, $t2, 1
    j converter_digitos
    
escrever_zero:
    li $t4, '0'
    sb $t4, ($t1)
    addi $t2, $t2, 1
    j escrever_numero
    
reverter:
    move $t4, $t3          
    subi $t1, $t1, 1       
    
loop_reversao:
    bge $t4, $t1, escrever_numero
    lb $t5, ($t4)
    lb $t6, ($t1)
    sb $t6, ($t4)
    sb $t5, ($t1)
    addi $t4, $t4, 1
    subi $t1, $t1, 1
    j loop_reversao
    
escrever_numero:
    
    li $v0, 15
    move $a0, $s0
    la $a1, saida_buffer
    move $a2, $t2
    syscall
    
    
    addi $t0, $s2, 1
    beq $t0, $t9, proximo_numero_saida
    
    
    li $v0, 15
    move $a0, $s0
    la $a1, virgula_espaco
    li $a2, 2
    syscall
    
proximo_numero_saida:
    addi $s1, $s1, 4
    addi $s2, $s2, 1
    j loop_escrita
    
finalizar:
   
    li $v0, 16
    move $a0, $s0
    syscall
    
    
    
