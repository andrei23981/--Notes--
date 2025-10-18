# 📚 Notițe de Studiu

## 📋 Rezumatul Conținutului
Acest document servește ca un ghid practic pentru Laboratorul 3, axat pe scripting Bash. Acoperă concepte fundamentale precum declararea variabilelor, manipularea și operațiile aritmetice. În plus, se adâncește în redirecționarea intrărilor/ieșirilor, pipe-uri și utilitare esențiale de linie de comandă precum `tr`, `wc`, `grep`, `tee` și `diff`, culminând cu logica condițională și exemple avansate de scripturi.

## 🎯 Puncte Cheie
*   **Variabile în Bash:** Înțelegerea modului de declarare, atribuire a valorilor și accesare a variabilelor, inclusiv conceptul de variabile `readonly`.
*   **Aritmetica în Bash:** Stăpânirea expresiilor aritmetice folosind `expr` și sintaxa `$((...))`, împreună cu diverși operatori aritmetici.
*   **Redirecționarea I/O și Pipe-uri:** Gestionarea eficientă a fluxurilor de intrare și ieșire folosind `>`, `>>`, `<`, `2>` și conectarea comenzilor folosind pipe-uri (`|`).
*   **Utilitare Esențiale Bash:** Învățarea funcționalității și a opțiunilor comune ale `tr`, `wc`, `grep`, `tee` și `diff` pentru procesarea textului și manipularea fișierelor.
*   **Logica Condițională și comanda `test`:** Implementarea luării deciziilor în scripturi folosind expresii condiționale și comanda `test` cu diverșii săi operatori.

## 💡 Explicații Detaliate

### Variabile în Bash
*   **Declarare și Atribuire:**
    *   Variabilele sunt declarate prin simpla atribuire a unei valori unui nume.
    *   Sintaxă: `nume_variabila="valoare"`
    *   Nu sunt permise spații în jurul semnului egal (`=`).
    *   Exemplu: `variabila="Salut"`
*   **Variabile `readonly`:**
    *   Variabilele declarate cu `readonly` nu pot fi modificate sau anulate după atribuirea inițială.
    *   Sintaxă: `readonly nume_variabila=valoare`
    *   Exemplu: `readonly x=2`
*   **Comanda `declare`:**
    *   Folosită pentru a defini variabile cu atribute speciale.
    *   **Opțiuni Comune:**
        *   `-r`: Face o variabilă `readonly`.
        *   `-i`: Tratează o variabilă ca un întreg, permițând evaluarea aritmetică.
        *   `-a`: Definește un tablou indexat.
        *   `-A`: Definește un tablou asociativ (similar cu dicționarele).
        *   `-x`: Exportă variabila în mediul shell-ului.
        *   `-p`: Afișează variabila și atributele sale.
    *   Exemple:
        ```bash
        declare mesaj="hello"
        declare -r pi_string="3.1415"
        declare -i numar=10
        declare -a fructe=("mere" "pere" "prune")
        declare -A capitale
        capitale[Italia]="Roma"
        declare -x PATH="/usr/bin:/bin"
        ```
*   **Accesarea Variabilelor:**
    *   Folosiți simbolul `$` înaintea numelui variabilei.
    *   Exemplu: `echo $variabila`

### Aritmetica în Bash
*   **Evaluarea Expresiilor:**
    *   **Comanda `expr`:** Utilă pentru operații aritmetice simple.
        *   Sintaxă: `expr operand1 operator operand2`
        *   Exemplu: `suma=$(expr $x + $y)`
    *   **Expansiunea Aritmetică `$((...))`:** Mai puternică și flexibilă pentru expresii complexe, inclusiv paranteze.
        *   Sintaxă: `$(( expresie ))`
        *   Exemplu: `suma2=$((a + b))`
*   **Operatori Aritmetici:**
    *   `+`: Adunare
    *   `-`: Scădere
    *   `*`: Înmulțire
    *   `/`: Împărțire
    *   `%`: Modulo (restul împărțirii)
    *   `**`: Exponențiere
    *   `++`: Incrementare (ex: `((a++))`)
    *   `--`: Decrementare (ex: `((a--))`)
*   **Expresii Complexe:** `$((...))` poate gestiona paranteze imbricate și operatori multipli.

### Redirecționarea Intrărilor/Ieșirilor și Pipe-uri

*   **Fluxuri Standard:**
    *   **STDIN (0):** Intrare standard (tastatură).
    *   **STDOUT (1):** Ieșire standard (ecran).
    *   **STDERR (2):** Eroare standard (mesaje de eroare).
*   **Operatori de Redirecționare:**
    *   `>`: Redirecționează STDOUT către un fișier (suprascrie).
    *   `>>`: Redirecționează STDOUT către un fișier (adaugă).
    *   `2>`: Redirecționează STDERR către un fișier.
    *   `<`: Redirecționează STDIN de la un fișier.
    *   `|&`: Redirecționează atât STDOUT, cât și STDERR.
*   **Pipe-uri (`|`):**
    *   Conectează STDOUT-ul unei comenzi la STDIN-ul alteia, creând un flux continuu de date.
    *   Sintaxă: `comanda1 | comanda2`
    *   Exemplu: `echo "Salut" | wc -c` (trimite "Salut" la `wc -c` pentru a număra caracterele).

### Utilitare Esențiale Bash

*   **`tr` (Translate Characters):**
    *   Traduce sau șterge caractere din fluxurile de intrare.
    *   Sintaxă: `tr [opțiuni] <set1> <set2>`
    *   Utilizări comune: conversia majusculelor/minusculelor (`tr 'a-z' 'A-Z'`), eliminarea caracterelor repetate (`tr -s`), ștergerea caracterelor (`tr -d`).
*   **`wc` (Word Count):**
    *   Numără linii, cuvinte, caractere sau octeți în fișiere sau intrarea standard.
    *   **Opțiuni Comune:**
        *   `-l`: Linii
        *   `-w`: Cuvinte
        *   `-c`: Caractere
        *   `-m`: Octeți
*   **`grep` (Global Regular Expression Print):**
    *   Caută modele (șiruri de caractere sau expresii regulate) în fișiere.
    *   **Opțiuni Comune:**
        *   `-r`: Căutare recursivă în directoare.
        *   `-v`: Inversarea potrivirii (afișează liniile care *nu* se potrivesc).
        *   `-c`: Numără liniile care se potrivesc.
        *   `-l`: Listează numele fișierelor cu potriviri.
        *   `-i`: Ignoră majusculele/minusculele.
        *   `-n`: Afișează numerele de linie.
        *   `-o`: Afișează doar partea potrivită a liniei.
        *   `-q`: Mod silențios (suprimă ieșirea, returnează starea de ieșire).
        *   `--color`: Evidențiază potrivirile.
*   **`tee`:**
    *   Citește de la intrarea standard și scrie atât la ieșirea standard, cât și în unul sau mai multe fișiere. Acționează ca o bifurcație în fluxul de date.
    *   Sintaxă: `comanda | tee [opțiuni] <fișier1> [fișier2] ...`
    *   Opțiunea `-a`: Adaugă la fișiere.
*   **`diff`:**
    *   Compară două fișiere linie cu linie și arată diferențele.

### Logica Condițională și comanda `test`

*   **Comanda `test` (și `[ ]`):**
    *   Evaluează expresii condiționale. Returnează 0 (adevărat) sau non-zero (fals).
    *   Sintaxă: `test <condiție>` sau `[ <condiție> ]`
*   **Operatori Logici:**
    *   `&&` (AND): Execută a doua comandă doar dacă prima reușește.
    *   `||` (OR): Execută a doua comandă doar dacă prima eșuează.
*   **Operatori de Comparație Comuni:**
    *   **Numerici:** `-eq` (egal), `-ne` (diferit), `-lt` (mai mic decât), `-le` (mai mic sau egal), `-gt` (mai mare decât), `-ge` (mai mare sau egal).
    *   **Fișier/Director:** `-e` (există), `-f` (este un fișier obișnuit), `-d` (este un director), `-r` (lizibil), `-w` (inscriptibil), `-x` (executabil).
    *   **Logici:** `!` (negație), `-a` (AND, pentru combinarea condițiilor în `test`), `-o` (OR, pentru combinarea condițiilor în `test`).
*   **Exemple Practice:** Verificarea existenței fișierelor, compararea numerelor, verificarea permisiunilor.

## 🔑 Concepte Cheie
*   **Variabilă:** O locație de stocare numită pentru date.
*   **Scripting Bash:** Scrierea de secvențe de comenzi pentru shell-ul Bash pentru a automatiza sarcini.
*   **Shell:** Un interpretor de linie de comandă (ex: Bash).
*   **Intrare Standard (STDIN):** Sursa implicită de date pentru o comandă.
*   **Ieșire Standard (STDOUT):** Destinația implicită pentru ieșirea cu succes a unei comenzi.
*   **Eroare Standard (STDERR):** Destinația implicită pentru mesajele de eroare ale unei comenzi.
*   **Redirecționare:** Schimbarea destinației implicite de intrare sau ieșire a unei comenzi.
*   **Pipe:** Conectarea ieșirii unei comenzi la intrarea alteia.
*   **Descriptor de Fișier:** Un număr care reprezintă un fișier deschis sau un flux I/O.
*   **Expresie Regulată:** O secvență de caractere care definește un model de căutare.
*   **Instrucțiune Condiționată:** Cod care se execută în funcție de dacă o condiție este adevărată sau falsă.
*   **Stare de Ieșire:** O valoare numerică returnată de o comandă.
