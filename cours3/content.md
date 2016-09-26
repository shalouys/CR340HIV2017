# CR340

###Systèmes d'exploitation

### Cours 3 - Le démarrage du système

---
# Concepts - Composantes matérielles
--
# BIOS

Le BIOS (Basic Input/Output System) est une composante matérielle qui sert
 d'interface entre le processeur et le matériel de l'ordinateur.  Le BIOS inclus
 un environnement d'exécution qui est activé au démarrage de l'ordinateur.  Le
BIOS implémente aussi certaine fonctionalités d'accès au matériel, qui peuvent
être remplacé par des fonctionalité du *bootloader* ou noyaux

--
# BIOS

![bios](images/bios.jpg)
--
## Registre

Les registres sont des espaces mémoire intégré à même le processeur.  La
quantité de mémoire dans un registre est égal à la longeur d'un mot (8, 16, 32
ou 64-bit) dans l'architecture système.  Les registres sont les seuls espaces
mémoire pouvant être accédés directement par le processeur (à moins d'utiliser
des fonctionalités avancées).
--
## Registres x86 - Registres généraux

* `AX, AH, AL`: Registres accumulateurs. Utilisé pour des opérations arithmétiques et d'accès entrée-sortie
* `BX, BH, BL`: Registres de base. Utilisé pour des opérations mémoires
* `CX, DH, CL`: Registres de compteurs.  Utilisé pour des opérations de boucles.
* `DX, DH, DL`: Registres de données.  Utilisé pour des opérations d'entrée sorties
--
## Registres x86 - Registres de segments

* `CS`: Contient un pointeur vers le code du programme en mémoire
* `DS`: Contient un pointeur vers la zone de donnée du programme
* `SS`: Contient un pointeur vers l'espace de pile du programme.

--
## Registres x86 - Pointeurs

* `IP`: Pointeur vers la prochaine instruction
* `SP`: Pointeur vers l'item du haut de la pile
* `SI`: Contient un pointeur vers la source mémoire actuelle
---
# Concepts - Langage assembleur
--
## Instructions

Le langage assembleur fait une correspondance entre les opérations machines du
processeur et un langage textuelle.  Ainsi, le langage assembleur à une
quantitée limitée d'instructions.
--
## Instructions

* `mov`: Copie des données de mémoire à un registre, ou vice-versa
*`add`: Additionne une constante à un registre
*`jmp`: Déplace le pointeur d'exécution vers une addresse donnée
*`je`: Déplace le pointeur d'exécution vers une addresse donnée, si l'instructionprécédente était vraie
* `cmp`: Compare la valeur d'un registre avec une constante
--
## Interruptions

Une interruption est une action qui interrompt temporairement l'execution du programme courant et donne la priorité à un autre segment de code (appelé *interrupt handler*).  Les interruptions sont utilisé pour effectuer des tâches urgentes.  Les interruptions sont implémenté au niveau matériel.
--
## Pile (Stack)

Une pile des une structure de donnée ou les donnée sont écrite dans un ordre et lues dans l'ordre inverse (FILO).  Une pile est utilisé pour stocker des données temporaires (par exemple, des addresses de retour de fonction).

---
# Le démarrage
--
## Étapes du démarrage

* L'ordinateur initialise le BIOS
* Le BIOS démarre le processus de vérification matériel (POST)
* En utilisant l'ordre configuré dans le BIOS, l'ordinateur lit les 512 premier octets de chacun des disques
* Si les deux dernier octets d'un disque sont `Ox55 0xAA`, le disque est considéré démarrable.
* Si le disque est démarrable, le premier secteur du disque est copié en mémoire à l'addresse `0x7C00`
* Le BIOS transfert le contrôle au code contenu dans le secteur (`jmp 7c00h`)

---
# Le bootloader
--
## Initialization de la pile
```x86asm
mov ax, 07C0h   ; Set 'ax' equal to the location of this bootloader divided by 16
add ax, 20h     ; Skip over the size of the bootloader divided by 16
mov ss, ax      ; Set 'ss' to this location (the beginning of our stack region)
mov sp, 4096    ; Set 'ss:sp' to the top of our 4K stack
mov ax, 07C0h   ; Set 'ax' equal to the location of this bootloader divided by 16
mov ds, ax      ; Set 'ds' to the this location
```
--
## Fonction d'impression
```x86asm
print:
                mov ah, 0Eh     ; Specify 'int 10h' 'teletype output' function
                               ; [AL = Character, BH = Page Number, BL = Colour (in graphics mode)]
.printchar:
                lodsb           ; Load byte at address SI into AL, and increment SI
                cmp al, 0
                je .done        ; If the character is zero (NUL), stop writing the string
                int 10h         ; Otherwise, print the character via 'int 10h'
                jmp .printchar  ; Repeat for the next character
.done:
                ret
```
--
## Appel de fonction
```x86asm
mov si, message ; Put address of the null-terminated string to output into 'si'
call print      ; Call our string-printing routine
cli             ; Clear the Interrupt Flag (disable external interrupts)
hlt
```
--
## Signature de démarrage
```x86asm
times 510-($-$$) db 0
dw 0xAA55
```
