# Assembly-project-


.386
.model flat, stdcall
.stack 4096
INCLUDE Irvine32.inc
ExitProcess PROTO, dwExitCode:DWORD

.data
    engineCCList DWORD 500, 700, 1000
    numOfBikes DWORD 3
    bikeNameAdmin BYTE "Honda", 0
    bikeColorAdmin BYTE "Red", 0
    bikePriceAdmin DWORD ?
    bikeModelAdmin DWORD ?
    userEngineCC DWORD ?

    menuPrompt BYTE "Press 1 for User and 2 for Admin: ", 0
    menuMsg BYTE "Welcome to Bike Shop!", 0
    allBikesUserMsg BYTE "Available bikes:", 0
    allBikesAdminMsg BYTE "Bike Information:", 0
    invalidChoiceMsg BYTE "Invalid choice. Please enter 1 or 2.", 0
    enterEngineCCMsg BYTE "Enter the engine CC to buy: ", 0
    bikeFoundMsg BYTE "Congratulations! The bike is available.", 0
    bikeNotFoundMsg BYTE "Sorry, the requested bike is not available.", 0
    adminMenuMsg BYTE "1. Add Bike Your choice: ", 0
    enterPriceMsg BYTE "Enter bike price: ", 0
    enterModelMsg BYTE "Enter bike model: ", 0
    bikeAddedMsg BYTE "New bike added to inventory.", 0
    runAgainMsg BYTE "Do you want to run the program again? (1 for yes, 0 for no): ", 0

.code
main PROC
    mov edx, OFFSET menuPrompt
    call WriteString
    call Crlf

    mov edx, OFFSET menuMsg
    call WriteString
    call Crlf

    call ReadInt
    cmp eax, 1
    je userFunc
    cmp eax, 2
    je adminFunc
    jmp invalidChoice

userFunc:
    mov ecx, numOfBikes
    mov esi, 0
displayBikesLoopUser:
    mov edx, OFFSET enterEngineCCMsg
    call WriteString
    mov eax, [engineCCList + esi*4]
    call WriteDec
    call Crlf
    inc esi
    loop displayBikesLoopUser

    call ReadInt
    mov userEngineCC, eax

    mov ecx, numOfBikes
    mov esi, 0
searchLoopUser:
    mov eax, userEngineCC
    cmp eax, [engineCCList + esi*4]
    je bikeFoundUser
    inc esi
    loop searchLoopUser

    mov edx, OFFSET bikeNotFoundMsg
    call WriteString
    jmp askRunAgain

bikeFoundUser:
    mov edx, OFFSET bikeFoundMsg
    call WriteString
    jmp askRunAgain

adminFunc:
    mov edx, OFFSET allBikesAdminMsg
    call WriteString
    call Crlf

    mov edx, OFFSET adminMenuMsg
    call WriteString
    call Crlf

    mov edx, OFFSET bikeNameAdmin
    call WriteString
    call Crlf

    mov edx, OFFSET bikeColorAdmin
    call WriteString
    call Crlf

    mov edx, OFFSET enterPriceMsg
    call WriteString
    call ReadInt
    mov bikePriceAdmin, eax

    mov edx, OFFSET enterPriceMsg
    call WriteString
    mov eax, bikePriceAdmin
    call WriteDec
    call Crlf

    mov edx, OFFSET enterModelMsg
    call WriteString
    call ReadInt
    mov bikeModelAdmin, eax

    mov edx, OFFSET enterModelMsg
    call WriteString
    mov eax, bikeModelAdmin
    call WriteDec
    call Crlf

    inc numOfBikes

    mov edx, OFFSET bikeAddedMsg
    call WriteString
    call Crlf
    jmp askRunAgain

invalidChoice:
    mov edx, OFFSET invalidChoiceMsg
    call WriteString

askRunAgain:
    mov edx, OFFSET runAgainMsg
    call WriteString
    call Crlf

    call ReadInt
    cmp eax, 1
    je main

    invoke ExitProcess, 0

main ENDP

END main
