# **📘 Manualul Expertului în Scripting Bash & Linux**

Nivel: B (Avansat) | Bazat pe: Lab 8, 9, 10  
Scop: Înțelegerea profundă a automatizării, procesării textului și controlului proceselor.

## **🏗️ Capitolul 1: Arta Scripturilor Flexibile (Argumente & Căutare)**

Un script bun nu este "hardcodat". El se adaptează. Aici înveți cum să scrii scripturi care se comportă ca niște comenzi Linux adevărate.

### **1.1. Cum "vorbesc" scripturile tale? (getopts)**

Imaginează-ți că scrii un script de backup. Dacă scrii cp /home /backup în interiorul scriptului, e rigid. Utilizatorul ar vrea să zică ./backup.sh \-s /home \-d /backup \-v.  
Aici intervine **getopts**. Este "recepționerul" scriptului tău.  
**De ce getopts și nu getopt?**

* **getopts:** E built-in (rapid), simplu, standard POSIX. Nu suportă opțiuni lungi (--help), dar e perfect pentru 90% din cazuri.  
* **getopt:** E un program extern, suportă opțiuni lungi, dar sintaxa e mai complicată.

**Cum funcționează getopts (Anatomia unei bucle):**  
\# Optstring "f:v":  
\# \- 'f' urmat de ':' înseamnă că \-f CERE un argument (ex: \-f fisier.txt)  
\# \- 'v' fără ':' înseamnă că e un flag (ON/OFF), nu cere argument.

while getopts "f:v" optiune; do  
    case $optiune in  
        f)  
            \# $OPTARG este variabila magică ce ține valoarea argumentului lui \-f  
            FISIER="$OPTARG"  
            ;;  
        v)  
            VERBOSE=1  
            ;;  
        \*)  
            echo "Opțiune invalidă\!"  
            exit 1  
            ;;  
    esac  
done

### **1.2. Vânătoarea de Fișiere (find & xargs)**

Comanda ls e pentru oameni. Comanda find e pentru scripturi. Este mult mai puternică și precisă.  
**Sintaxa Logică:** find \[UNDE\] \[CE\] \[ACȚIUNE\]  
**Scenarii Reale:**

1. **"Vreau să șterg log-urile vechi de o săptămână."**  
   \# \-mtime \+7: modificat acum mai mult de (+) 7 zile  
   find /var/log \-name "\*.log" \-mtime \+7 \-delete

2. **"Vreau să găsesc fișierele mari care îmi ocupă spațiul."**  
   \# \-size \+100M: mai mari de 100 Megabytes  
   find /home \-type f \-size \+100M

Puterea lui xargs (Banda Rulantă)  
De ce să folosești xargs? Imaginează-ți că find găsește 1 milion de fișiere. Dacă faci rm $(find ...), linia de comandă va crăpa ("Argument list too long").  
xargs ia aceste 1 milion de rezultate și le dă comenzii rm în pachete mici, gestionabile.  
⚠️ Problema Spațiilor:  
Dacă ai un fișier "poza vacanta.jpg", xargs simplu va crede că sunt două fișiere: "poza" și "vacanta.jpg".  
Soluția Profesională:  
\# \-print0 separă fișierele cu NULL (invizibil), nu cu spațiu.  
\# \-0 îi spune lui xargs să se aștepte la NULL.  
find . \-name "\*.jpg" \-print0 | xargs \-0 mv \-t /backup/

## **🔬 Capitolul 2: Chirurgul de Text (Regex, Sed, Awk)**

Linux este bazat pe text. Configurările sunt text, log-urile sunt text. Dacă știi să manipulezi textul, stăpânești sistemul.

### **2.1. Expresii Regulate (Regex) \- Limbajul Modelelor**

Regex nu este o comandă, ci un "șablon" de căutare. Gândește-te la el ca la un filtru avansat.  
**Dicționar Vizual:**

| Simbol | Nume | Explicație Simplă | Exemplu | Se potrivește cu |
| :---- | :---- | :---- | :---- | :---- |
| ^ | Ancora de început | "Linia trebuie să înceapă cu asta" | ^Error | "Error: disc full" |
| $ | Ancora de sfârșit | "Linia trebuie să se termine cu asta" | done$ | "Task done" |
| . | Jokerul | "Orice caracter (o singură dată)" | c.t | "cat", "cut", "c@t" |
| \* | Repetitorul | "Caracterul dinainte de 0 sau mai multe ori" | 10\* | "1", "10", "1000" |
| \[ \] | Setul | "Oricare dintre caracterele astea" | \[aeiou\] | orice vocală |
| \[^ \] | Negația | "ORICE, dar NU astea" | \[^0-9\] | orice literă/simbol |

**Regex Extins (ERE) \- Necesită grep \-E:**

* \+: 1 sau mai multe ori (mai util decât \* de multe ori).  
* ?: 0 sau 1 dată (opțional). colou?r găsește și "color" și "colour".  
* |: SAU logic. error|warning.

### **2.2. grep \- Căutătorul**

Folosește grep când vrei să **găsești linii** într-un fișier.

* grep \-E "root|admin" /etc/passwd: Găsește userii importanți.  
* grep \-r "TODO" .: Caută recursiv în toate fișierele din folderul curent.  
* grep \-v "succes" log.txt: Arată tot ce **NU** e succes (deci erorile).

### **2.3. sed \- Editorul de Flux**

Folosește sed când vrei să **modifici textul** linie cu linie. Este faimos pentru substituții.  
**Formula Magică:** s/vechi/nou/g (Substitute / ce cauți / cu ce înlocuiești / Global pe linie).  
**Scenariu:** Vrei să schimbi toate aparițiile lui "http" în "https" într-un fișier config.  
\# Fără \-i doar afișează pe ecran. Cu \-i modifică fișierul.  
sed \-i 's/http:/https:/g' config.xml

**Ștergerea:**  
\# Șterge liniile care încep cu \# (comentarii)  
sed '/^\#/d' config.conf

### **2.4. awk \- Analistul de Date**

awk este regele textului structurat (coloane). Dacă ai un CSV, un tabel din ps sau ls, folosești awk. El vede textul ca pe o matrice.  
**Cum gândește AWK:**

1. Citește o linie.  
2. O sparge în coloane ($1, $2, $3...) pe baza unui separator (spațiu implicit, sau \-F pentru altceva).  
3. Execută codul tău.

**Structura unui program AWK:** BEGIN { ... } \-\> Se execută O DATĂ la început (inițializări). pattern { ... } \-\> Se execută PENTRU FIECARE linie care respectă pattern-ul. END { ... } \-\> Se execută O DATĂ la final (totaluri).  
**Exemplu:** Calculează dimensiunea totală a fișierelelor din ls \-l (Coloana 5 e mărimea).  
ls \-l | awk '  
BEGIN { print "Încep calculul..." }  
NR \> 1 { \# Sărim peste prima linie (totalul dat de ls)  
    sum \+= $5   
}   
END { print "Total Bytes:", sum }  
'

## **🚦 Capitolul 3: Dirijorul de Orchestră (Procese & Semnale)**

Un sistem Linux este o colecție de procese care rulează simultan. Tu ești dirijorul.

### **3.1. Semnale \- Limbajul Proceselor**

Cum îi spui unui proces să se oprească? Îi trimiți un semnal. kill nu doar omoară, ci trimite aceste semnale.

* **SIGINT (2):** "Întrerupe\!" (Echivalentul Ctrl+C). Politicos, dar ferm.  
* **SIGTERM (15):** "Termină, te rog." (Default la kill). Procesul are timp să salveze date și să închidă conexiuni.  
* **SIGKILL (9):** "MORI ACUM\!" (Brutal). Procesul dispare instant din memorie. Nu poate fi ignorat. Folosit doar în caz de urgență.  
* **SIGSTOP (19):** "Îngheață\!" (Pauză). Procesul rămâne în memorie dar nu mai primește timp CPU.  
* **SIGCONT (18):** "Continuă\!" (Un-pause).

### **3.2. trap \- Plasa de Siguranță**

Dacă scriptul tău creează fișiere temporare și utilizatorul dă Ctrl+C, fișierele rămân gunoi pe disc. Cu trap, poți "prinde" semnalul și face curățenie înainte să mori.  
cleanup() {  
    echo "Curățenia de final..."  
    rm \-f /tmp/temp\_\*  
}

\# Dacă primesc INT (Ctrl+C) sau TERM, rulez cleanup și apoi ies.  
trap "cleanup; exit" SIGINT SIGTERM

echo "Lucrez..."  
sleep 100

### **3.3. IPC (Inter-Process Communication)**

Cum vorbesc două procese între ele?

1. **Pipe Anonim (|):**  
   * ls | grep txt. Ieșirea lui ls devine direct intrarea lui grep. E temporar, există doar cât rulează comanda.  
2. **FIFO (Named Pipe):**  
   * Este un fișier fizic pe disc, marcat cu p.  
   * mkfifo mypipe  
   * **Comportament:** Dacă Procesul A scrie în FIFO, el **se blochează** (așteaptă) până când Procesul B vine să citească. Este o sincronizare perfectă.  
3. **File Locking (flock) \- Semaforul:**  
   * Problemă: Două scripturi scriu în același log simultan \=\> Log corupt (linii amestecate).  
   * Soluție: flock. "Eu țin cheia, voi așteptați."

(  
  flock \-x 200  \# Obține blocare exclusivă pe descriptorul 200  
  echo "Scriu ceva critic..." \>\> fisier.log  
  sleep 5  
) 200\>fisier.lock

### **3.4. Deadlock (Interblocajul)**

Coșmarul oricărui programator.

* **Scenariu:**  
  * Procesul A are Resursa 1 și vrea Resursa 2\.  
  * Procesul B are Resursa 2 și vrea Resursa 1\.  
  * **Rezultat:** Ambele așteaptă la infinit. Nimeni nu cedează.  
* **Soluție în Scripting:** Timeout-uri sau folosirea flock \-n (non-blocking) \- "Dacă nu e liber, renunț și încerc mai târziu, nu stau la coadă."

## **🌟 Rezumat Vizual al Comenzilor**

| Comandă | Categorie | Ce face ("Pe limba noastră") |
| :---- | :---- | :---- |
| getopts | Scripting | Citește opțiunile \-a \-b date scriptului. |
| find | Căutare | Caută fișiere după reguli complexe (timp, mărime). |
| xargs | Procesare | Transformă o listă lungă de rezultate în argumente. |
| grep | Text | Găsește rândul care conține cuvântul X. |
| sed | Text | Înlocuiește cuvântul X cu Y. |
| awk | Text | Extrage coloana 3 și face calcule cu ea. |
| ps | Procese | Îți arată ce rulează acum (Task Manager). |
| kill | Procese | Trimite semnale (Stop, Terminate, Kill). |
| mkfifo | IPC | Creează o țeavă cu nume pentru comunicare. |
| flock | IPC | Pune lacăt pe un fișier ca să nu scrie doi deodată. |

