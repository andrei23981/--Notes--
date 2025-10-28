# 📚 Note de Studiu

## 📋 Rezumatul Conținutului
Acest document este un manual de laborator pentru "Sisteme de Operare I", concentrându-se în special pe "Laboratorul 5". Acoperă instrucțiuni repetitive în Bash, inclusiv buclele `for` și `while`, și introduce comanda `select` pentru crearea de meniuri interactive. Manualul se adâncește în mecanismele de comunicare inter-proces (IPC), gestionarea utilizatorilor și grupurilor în Linux, identificatorii de fișiere, și distincția între legături hard și soft. Se încheie cu exerciții practice și seturi de probleme concepute pentru a consolida aceste concepte.

## 🎯 Puncte Cheie

*   **Instrucțiuni Repetitive în Bash (Bucle):** Bash oferă bucle `for` și `while` pentru executarea repetată a comenzilor.
    *   Bucla `for`: Iterează peste o listă de elemente, ieșirea unei comenzi sau fișiere folosind wildcards.
    *   Bucla `while`: Execută comenzi atâta timp cât o condiție specificată este adevărată.
*   **Meniuri Interactive cu `select`:** Comanda `select` permite crearea de meniuri interactive, permițând utilizatorilor să aleagă opțiuni dintr-o listă.
*   **Comunicare Inter-Proces (IPC):** Mecanisme precum fișiere temporare, FIFO (pipe-uri numite), cozi de mesaje SysV, memorie partajată și semafoare sunt folosite pentru ca procesele să comunice.
*   **Gestionarea Utilizatorilor și Grupurilor în Linux:** Înțelegerea categoriilor de utilizatori (root, normal, sistem), permisiunilor și rolurilor UID-urilor, EUID-urilor, GID-urilor și EGID-urilor este crucială pentru securitatea și administrarea sistemului.
*   **Legături de Fișiere (Hard vs. Soft):** Diferențierea între legături hard (împărtășesc același inode) și legături soft (legături simbolice care acționează ca pointeri) este esențială pentru gestionarea fișierelor.

## 💡 Explicație Detaliată

### 🚀 Instrucțiuni Repetitive în Bash (Bucle)

Bash oferă construcții puternice de buclare pentru automatizarea sarcinilor:

#### 🗄️ Bucla `for`

*   **Scop:** Pentru a itera peste o secvență de elemente. Această secvență poate fi o listă de valori, ieșirea unei comenzi sau fișiere identificate prin wildcards.
*   **Sintaxă:**
    ```bash
    for variabilă in listă; do
        # comenzi de executat pentru fiecare element
    done
    ```
*   **Exemple:**
    *   **Iterarea peste o listă de valori:**
        ```bash
        for număr in 1 2 3 4 5; do
            echo "Număr curent: $număr"
        done
        ```
        Acest script afișează numerele de la 1 la 5, fiecare pe o linie nouă.
    *   **Iterarea peste ieșirea unei comenzi:**
        ```bash
        for fișier in $(ls *.txt); do
            echo "Procesare fișier: $fișier"
        done
        ```
        Această buclă procesează toate fișierele care se termină cu `.txt` în directorul curent.
    *   **Folosind wildcards:**
        ```bash
        for script in *.sh; do
            echo "Script găsit: $script"
        done
        ```
        Aceasta iterează prin toate fișierele cu extensia `.sh`.

#### 🔄 Bucla `while`

*   **Scop:** Pentru a executa un bloc de comenzi în mod repetat atâta timp cât o condiție specificată rămâne adevărată.
*   **Sintaxă:**
    ```bash
    while [ condiție ]; do
        # comenzi de executat
    done
    ```
*   **Exemple:**
    *   **Numărare inversă:**
        ```bash
        număr=5
        while [ $număr -gt 0 ]; do
            echo "Număr curent: $număr"
            număr=$((număr-1))
        done
        ```
        Acest script numără invers de la 5 la 1.
    *   **Citirea unui fișier linie cu linie:**
        ```bash
        while read linie; do
            echo "Linie citită: $linie"
        done < nume_fișier.txt
        ```
        Aceasta citește și procesează fiecare linie din `nume_fișier.txt`.
    *   **Buclă infinită:**
        ```bash
        while true; do
            echo "Aceasta este o buclă infinită. Apasă Ctrl+C pentru a opri."
        done
        ```
        Această buclă rulează continuu până când este întreruptă manual.

#### ⏸️ Bucla `until`

*   **Scop:** Similară cu `while`, dar execută comenzi până când o condiție devine adevărată.
*   **Sintaxă:**
    ```bash
    until [ condiție ]; do
        # comenzi de executat
    done
    ```
*   **Exemplu (Numărare crescătoare):**
    ```bash
    număr=1
    until [ $număr -ge 10 ]; do
        echo "Număr curent: $număr"
        număr=$((număr+1))
    done
    ```
    Acest script incrementează `număr` de la 1 la 10.

### 🍽️ Combinarea Buclelor și Wildcards-urilor

Buclele pot fi combinate cu comenzi care folosesc wildcards pentru a efectua acțiuni asupra mai multor fișiere eficient.
*   **Exemplu (Ștergerea fișierelor temporare):**
    ```bash
    for fișier in /tmp/*; do
        echo "Ștergere fișier: $fișier"
        rm "$fișier"
    done
    ```
    Această buclă șterge toate fișierele din directorul `/tmp`.

### 🖱️ Meniuri Interactive cu `select`

*   **Scop:** Pentru a crea meniuri interactive în linia de comandă, permițând utilizatorilor să selecteze opțiuni.
*   **Sintaxă:**
    ```bash
    select variabilă in opțiune1 opțiune2 ...; do
        # comenzi bazate pe opțiunea selectată
    done
    ```
*   **Cum funcționează:** `select` afișează opțiunile cu numere și solicită utilizatorului să introducă un număr. `variabila` va conține valoarea opțiunii selectate.
*   **Exemplu (Meniu Simplu):**
    ```bash
    #!/bin/bash
    PS3="Alege o opțiune: "
    select opțiune in "Listează fișiere" "Spațiu pe disc" "Ieșire"; do
        case $opțiune in
            "Listează fișiere")
                echo "Fișiere în directorul curent:"
                ls
                ;;
            "Spațiu pe disc")
                echo "Utilizarea spațiului pe disc:"
                df -h
                ;;
            "Ieșire")
                echo "Ieșire din script."
                break
                ;;
            *)
                echo "Opțiune invalidă."
                ;;
        esac
    done
    ```
    Acest script prezintă un meniu utilizatorului și execută comenzi corespunzătoare bazate pe alegerea lor.

### 🗄️ Mecanisme de Comunicare Inter-Proces (IPC)

IPC permite diferitelor procese să comunice și să împărtășească date.

*   **Fișiere Temporare:** Folosite pentru stocarea intermediară a datelor. Create cu `mktemp`.
*   **FIFO (Pipe-uri Numite):** Permit comunicarea între procese printr-un canal unidirecțional.
*   **SysV IPC:**
    *   **Cozi de Mesaje:** Pentru trimiterea de mesaje structurate între procese. Accesate via `ipcs` și `msgsnd`.
    *   **Memorie Partajată:** Permite mai multor procese să acceseze simultan o regiune de memorie. Gestionată cu `ipcs` și `ipcrm`.
    *   **Semafoare:** Folosite pentru sincronizarea accesului la resurse partajate. Gestionate cu `ipcs` și `ipcrm`.

### 🧑‍🤝‍🧑 Gestionarea Utilizatorilor și Grupurilor în Linux

*   **Categorii de Utilizatori:**
    *   **Root (Superutilizator):** Are privilegii nelimitate.
    *   **Utilizatori Normali:** Au acces limitat, cu un director home dedicat.
    *   **Utilizatori de Sistem:** Folosiți pentru rularea serviciilor, de obicei fără director home sau login direct.
*   **Permisiuni:** Fișierele și directoarele sunt asociate cu un proprietar (utilizator), un grup și alții. Permisiunile (citire `r`, scriere `w`, executare `x`) sunt definite pentru fiecare categorie.
*   **Identificatori:**
    *   **UID (ID Utilizator):** Identificator unic pentru un utilizator.
    *   **EUID (ID Utilizator Efectiv):** Controlează drepturile de acces efective.
    * **GID (ID Grup):** Identifică grupul unui utilizator.
    *   **EGID (ID Grup Efectiv):** Controlează accesul efectiv al grupului.
    *   **RUID/RGID (ID Utilizator/Grup Real):** Urmăresc utilizatorul și grupul original care a inițiat un proces.
    *   **SUID/SGID (ID Utilizator/Grup Salvat):** Permit proceselor să revină la permisiunile originale.

### 🔗 Legături de Fișiere

*   **Legătură Hard:**
    *   O referință directă la datele de pe disc, împărtășind același inode ca fișierul original.
    *   Ștergerea unei legături hard nu șterge datele fișierului până când toate legăturile hard sunt eliminate.
    *   Nu poate lega către directoare (cu excepția root-ului) sau peste sisteme de fișiere diferite.
*   **Legătură Soft (Legătură Simbolică):**
    *   Un fișier special care conține calea către un alt fișier sau director.
    *   Are propriul inode, separat de țintă.
    *   Dacă fișierul țintă este șters, legătura soft devine ruptă.
    *   Poate lega către directoare și peste sisteme de fișiere diferite.

### 🔍 Comanda `stat`

*   **Scop:** Afișează informații detaliate despre fișiere și directoare, inclusiv permisiuni, timestamp-uri, dimensiune, tip, UID, GID etc.
*   **Opțiuni Cheie:**
    *   `-c FORMAT`: Format de ieșire personalizat.
    *   `-f`: Informații despre sistemul de fișiere.
    *   `-L`: Urmărește legăturile simbolice.
    *   `-t`: Ieșire concisă.
    *   `-x`: Ieșire detaliată extinsă.
    *   `-printf`: Ieșire formatată fără linie nouă la sfârșit.