INCLUDE Irvine32.inc

.data

students STRUCT

    id DWORD ?
    
    marks DWORD ?
    
students ENDS

dataArray students 10 DUP({})

welcomeMsg BYTE "                       Welcome to Student Management System", 0

menuPrompt1 BYTE "Press 1 to insert data", 0

menuPrompt2 BYTE "Press 2 to display", 0

promptMenu BYTE "Enter your choice: ", 0

promptCount BYTE "Enter the number of students data you want to enter: ", 0

promptId BYTE "Enter student id: ", 0

promptMarks BYTE "Enter student marks: ", 0

choiceMsg BYTE "Your choice: ", 0

userChoice BYTE 0

.code

main PROC

    mov edx, OFFSET welcomeMsg
    
    call WriteString
    call Crlf 
     call Crlf
    mov edx, OFFSET menuPrompt1
    call WriteString
    call Crlf 

    mov edx, OFFSET menuPrompt2
    call WriteString
    call Crlf 


mainLoop:

    mov edx, OFFSET promptMenu
    call WriteString
    call ReadChar

    mov userChoice, al 
    mov dl, userChoice 
    mov edx, OFFSET choiceMsg
    call WriteString
    mov edx, OFFSET userChoice 
    call WriteString
    call Crlf

    cmp al, '1'
    je insertData
    cmp al, '2'
    je displayData

    jmp exitProgram

insertData:

    call InputData
    jmp mainLoop

displayData:

    call Crlf 
    call DisplayData
    jmp mainLoop

exitProgram:

    exit

main ENDP


InputData PROC

    mov edx, OFFSET promptCount
    call WriteString
    call Crlf 
    call ReadInt
    mov ecx, eax 

    mov esi, OFFSET dataArray 

inputLoop:
    
    mov edx, OFFSET promptId
    call WriteString
    call ReadInt
    mov [esi].students.id, eax 

    
    mov edx, OFFSET promptMarks
    call WriteString
    call ReadInt
    mov [esi].students.marks, eax 

    add esi, SIZEOF students 

    loop inputLoop

    ret
InputData ENDP

DisplayData PROC

    mov ecx, 10 ; Loop counter
    mov esi, OFFSET dataArray 

displayLoop:
    
    call Crlf
    
    
    mov eax, [esi].students.id
    call WriteDec

    mov edx, OFFSET tabSpace
    call WriteString

    
    mov eax, [esi].students.marks
    call WriteDec

    add esi, SIZEOF students 
    loop displayLoop

    ret
DisplayData ENDP

tabSpace BYTE "    ", 0 

END main

