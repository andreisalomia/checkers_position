%include "../include/io.mac"

section .data
    array dd 1,2,3,4,5,6,7,8

section .text
    global bonus
    extern printf

bonus:
    ;; DO NOT MODIFY
    push ebp
    mov ebp, esp
    pusha

    mov eax, [ebp + 8]	; x
    mov ebx, [ebp + 12]	; y
    mov ecx, [ebp + 16] ; board

    ;; DO NOT MODIFY
    ;; FREESTYLE STARTS HERE


      ;; Creating an array of the possible position of the given dame
    mov esi, eax
    mov edx, ebx
    sub esi, 1
    sub edx, 1

    mov eax, 7
    mov ebx,esi
    sub eax, ebx
    mov dword [array], eax
    mov dword [array+4], edx

    add edx, 2
    mov eax, 7
    mov ebx,esi
    sub eax, ebx
    mov dword [array+8], eax
    mov dword [array+12], edx

    add esi, 2
    sub edx, 2
    mov eax, 7
    mov ebx,esi
    sub eax, ebx
    mov dword [array+16], eax
    mov dword [array+20], edx

    add edx, 2
    mov eax, 7
    mov ebx,esi
    sub eax, ebx
    mov dword [array+24], eax
    mov dword [array+28], edx

    mov ebx, -8 ; storing array iterator
    xor esi, esi ; first number
    xor edi, edi ; second number
check_compatibility_1:
    add ebx, 8
    cmp ebx, 32
    je done
    mov edx, dword [array+ebx]
    cmp edx, 0
    jge check_compatibility_2
    jmp check_compatibility_1

check_compatibility_2:
    mov edx, dword [array+ebx]
    cmp edx, 7
    jle check_compatibility_3
    jmp check_compatibility_1

check_compatibility_3:
    mov edx, dword [array+ebx+4]
    cmp edx, 0
    jge check_compatibility_4
    jmp check_compatibility_1

check_compatibility_4:
    mov edx, dword [array+ebx+4]
    cmp edx, 7
    jle perform_shift
    jmp check_compatibility_1

perform_shift:
    mov eax, dword [array+ebx]
    cmp eax, 3
    jle first_number
    jmp second_number

first_number:
    mov eax, dword [array+ebx]
    mov edx, 3
    sub edx, eax
    shl edx, 3
    add edx, dword [array+ebx+4]
    xor eax, eax
    mov eax, 1
    jmp shift_byte_esi

second_number:
    mov eax, dword [array+ebx]
    mov edx, 7
    sub edx, eax
    shl edx, 3
    add edx, dword [array+ebx+4]
    xor eax, eax
    mov eax, 1
    jmp shift_byte_edi

shift_byte_esi:
    cmp edx, 0
    je or_registers_esi
    shl eax, 1
    dec edx
    jmp shift_byte_esi

shift_byte_edi:
    cmp edx, 0
    je or_registers_edi
    shl eax, 1
    dec edx
    jmp shift_byte_edi

or_registers_esi:
    or esi, eax
    jmp check_compatibility_1

or_registers_edi:
    or edi, eax
    jmp check_compatibility_1

done:
    mov [ecx], esi
    mov [ecx+4], edi

    ;; FREESTYLE ENDS HERE
    ;; DO NOT MODIFY
    popa
    leave
    ret
    ;; DO NOT MODIFY