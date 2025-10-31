### Ghid complet de comenzi Bash și rolurile lor (Laboratoarele 2–4 SO I, revizuit complet)

---

## 1. Comenzi de afișare, intrare și formatare

| Comandă | Rol | Exemple |
|----------|------|----------|
| **echo** | Afișează text sau valori de variabile | `echo "Salut lume"`, `echo -e "Linie1\nLinie2"` |
| **printf** | Afișare formatată, similară limbajului C | `printf "%s are %d ani.\n" "Ana" 20` |
| **read** | Primește input de la utilizator | `read -p "Nume: " nume` |
| **man** | Deschide manualul de utilizare al unei comenzi | `man ls` |
| **tput** | Controlează culorile și cursorul în terminal | `tput setaf 2; echo "Text verde"` |

---

## 2. Redirecționarea fluxurilor de date (I/O)

| Operator / Comandă | Rol | Exemplu |
|--------------------|------|----------|
| `>` | Redirectează ieșirea standard (rescrie fișierul) | `echo "text" > fisier.txt` |
| `>>` | Adaugă la finalul fișierului | `echo "linie" >> fisier.txt` |
| `2>` | Trimite erorile (STDERR) într-un fișier | `ls /inexistent 2> erori.log` |
| `<` | Folosește un fișier ca sursă de input | `read -r linie < input.txt` |
| `|` | Conectează ieșirea unei comenzi la intrarea alteia | `cat fisier.txt | grep "root"` |
| `|&` | Trimite STDOUT și STDERR la următoarea comandă | `comanda |& tee log.txt` |
| **tee** | Afișează și salvează simultan ieșirea | `ls -l | tee lista.txt` |

---

## 3. Manipularea fișierelor și directoarelor

| Comandă | Rol | Exemplu |
|----------|------|----------|
| **ls** | Listează fișiere și directoare | `ls -la` |
| **mkdir** | Creează directoare (cu `-p` pentru recursiv) | `mkdir -p dir1/dir2` |
| **rmdir** | Șterge directoare goale | `rmdir dir1` |
| **rm -rf** | Șterge recursiv fișiere și directoare | `rm -rf dir1` |
| **touch** | Creează fișiere goale sau actualizează timestamp | `touch fisier.txt` |
| **cat** | Afișează conținutul fișierelor | `cat fisier.txt` |
| **head** | Primele linii dintr-un fișier | `head -n 5 fisier.txt` |
| **tail** | Ultimele linii dintr-un fișier | `tail -n 10 fisier.txt` |
| **wc** | Numără linii, cuvinte, caractere | `wc -lwm fisier.txt` |
| **cut** | Extrage câmpuri delimitate | `cut -d',' -f2 date.csv` |
| **grep** | Caută text (poate fi recursiv sau inversat) | `grep -r "nologin" /etc/passwd` |
| **diff** | Compară fișiere | `diff f1.txt f2.txt` |
| **chmod** | Schimbă permisiuni | `chmod 755 script.sh` |
| **umask** | Setează permisiuni implicite noi | `umask 022` |
| **stat** | Afișează metadatele fișierului (permisii, mtime, owner) | `stat fisier.txt` |
| **pgrep** | Verifică dacă un proces rulează | `pgrep sshd` |
| **ps** | Afișează procesele curente | `ps aux | grep ssh` |
| **df** | Afișează spațiul liber pe disc | `df -h` |
| **ping** | Testează conectivitatea rețelei | `ping -c 1 8.8.8.8` |

---

## 4. Variabile și expresii aritmetice

| Comandă | Rol | Exemple |
|----------|------|----------|
| **declare** | Definește variabile cu atribute specifice | `declare -i num=10`, `declare -A dictionar` |
| **readonly / -r** | Face o variabilă nemodificabilă | `readonly PI=3.14` |
| **export / -x** | Exportă o variabilă în mediul shell | `export PATH="/usr/bin"` |
| **expr / $(( )) / let** | Evaluează expresii aritmetice | `suma=$((a+b))`, `expr $x \* $y` |

---

## 5. Operatorii condiționali și logici

| Operator / Comandă | Rol | Exemplu |
|--------------------|------|----------|
| **test** sau `[ ]` | Verifică condiții numerice, textuale sau fișier | `[ -f fisier.txt ]` |
| **[[ ]]** | Expresii condiționale complexe | `[[ $nume == "Ana" && $varsta -gt 18 ]]` |
| `-eq, -ne, -lt, -le, -gt, -ge` | Compară numere | `[ $x -gt 5 ]` |
| `-z` / `-n` | Verifică șiruri goale / non-goale | `[ -z "$str" ]` |
| `-f, -d, -r, -w, -x` | Verifică fișiere și permisiuni | `[ -r script.sh ]` |
| `!` | Negare | `[ ! -d folder ]` |
| `&&` / `||` | Conectează comenzi logic | `mkdir test && echo ok` |

---

## 6. Structuri de control

### **if / elif / else**
```bash
if [ conditie ]; then
  comenzi
elif [ alta_conditie ]; then
  alte_comenzi
else
  comenzi_default
fi
```

### **case**
```bash
case $variabila in
  1) echo "Optiunea 1" ;;
  2) echo "Optiunea 2" ;;
  *) echo "Alta valoare" ;;
esac
```

---

## 7. Funcții în Bash

| Comandă | Rol | Exemplu |
|----------|------|----------|
| `function nume() { comenzi; }` | Definește o funcție | `salut() { echo "Salut!"; }` |
| `return` | Returnează cod de ieșire | `return 0` |
| `export -f` | Exportă funcția în subshell | `export -f salut` |

---

## 8. Comenzi suplimentare utile

| Comandă | Rol | Exemplu |
|----------|------|----------|
| **tr** | Traduce / elimină caractere | `echo "abc" | tr 'a-z' 'A-Z'` |
| **banner** | Text decorativ în terminal | `banner SO1` |
| **uname -sr** | Informații despre sistem | `uname -sr` |
| **traceroute** | Urmărește traseul pachetelor de rețea | `traceroute www.google.com` |
| **cut / head / tail / wc / grep** | Analizează fișiere text | `head -n 10 log.txt`, `grep error log.txt` |
| **ps / pgrep / stat / df / du** | Informații despre procese, fișiere și spațiu | `ps aux`, `df -h`, `du -sh /home` |

---

## 9. Noțiuni utile

- **STDIN / STDOUT / STDERR** – fluxuri de intrare, ieșire și erori.
- **Wildcard-uri** `{1..5}`, `*`, `?` – pentru extinderea modelelor de fișiere.
- **Permisiuni UNIX:** `r` (citire), `w` (scriere), `x` (executare).
- **Pipe-uri (|):** conectează comenzi.
- **Link count:** un director are cel puțin 2 legături (`.` și `..`).

---

## 10. Exerciții practice (din laboratoare)

1. **Backup automat:** verifică dacă `/etc/fstab` există; dacă da, creează `/backup` și copiază fișierul acolo.
2. **Verificarea spațiului liber:** `df -h` + condiție `if` → afișează alertă dacă <10% liber.
3. **Monitorizare procese:** folosește `ps`, `pgrep`, `grep` pentru a detecta procese și a salva rezultatul în log.
4. **Verificare permisiuni:** cu `stat`, `chmod`, `umask` pentru diverse fișiere.
5. **Meniu interactiv:** folosește `case` pentru operații: creare, listare, ștergere directoare.
6. **Analiza log-urilor:** `head`, `tail`, `grep`, `wc` – numără erorile și avertismentele.
7. **Script aritmetic:** calculează sume și diferențe cu `expr` și `$(( ))`.
8. **Gestionare fișiere goale:** verifică dacă sunt vide (`-s`) și arhivează-le.
9. **Test conexiune rețea:** rulează script doar dacă `ping -c 1 8.8.8.8` e accesibil.
10. **Raport sistem complet:** combină `ps`, `df`, `uname`, `date`, `tee` într-un raport text.

---

### Notă finală
Acest ghid include **toate comenzile din laboratoarele 2–4**, cu exemple și exerciții aplicate, inclusiv `ps`, `df`, `stat`, `head`, `tail` și altele pentru analiză, rețea și administrare sistem.

