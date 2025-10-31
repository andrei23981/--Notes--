### Ghid complet de comenzi Bash și rolurile lor (Laboratoarele 2–5 SO I, versiune completă)

---

## 1. Comenzi de afișare, intrare și formatare

| Comandă | Rol | Exemple |
|----------|------|----------|
| **echo** | Afișează text sau valori de variabile | `echo "Salut lume"`, `echo -e "Linie1\nLinie2"` |
| **printf** | Afișare formatată, similară limbajului C | `printf "%s are %d ani.\n" "Ana" 20` |
| **read** | Primește input de la utilizator | `read -p "Nume: " nume` |
| **man** | Deschide manualul unei comenzi | `man ls` |
| **tput** | Controlează culorile și poziția cursorului în terminal | `tput setaf 2; echo "Text verde"` |

---

## 2. Redirecționarea fluxurilor de date (I/O)

````markdown
| Operator / Comandă | Descriere | Exemplu |
|--------------------|------------|----------|
| `>` | Redirecționează STDOUT către fișier (rescrie) | `echo "text" > fisier.txt` |
| `>>` | Redirecționează STDOUT către fișier (append) | `echo "linie" >> fisier.txt` |
| `2>` | Redirecționează STDERR către fișier (rescrie) | `ls /x 2> erori.log` |
| `2>>` | Redirecționează STDERR către fișier (append) | `ls /x 2>> erori.log` |
| `&>` | Redirecționează STDOUT + STDERR către același fișier | `comanda &> all.log` |
| `>&2` | Redirecționează STDOUT către STDERR | `echo "Eroare!" >&2` |
| `|` | Conectează ieșirea unei comenzi la intrarea alteia (pipeline) | `cat fisier.txt | grep "root"` |
| `|&` | Trimite STDOUT și STDERR la următoarea comandă | `comanda |& tee log.txt` |
| `<` | Citește din fișier în loc de tastatură | `read linie < fisier.txt` |
| `<<` | Heredoc (input multi-linie) | `cat <<EOF ... EOF` |
| `<<<` | Here string (input o singură linie) | `grep root <<< "root:x:0:0:"` |
````

## 3. Manipularea fișierelor și directoarelor

| Comandă | Rol | Exemplu |
|----------|------|----------|
| **ls** | Listează fișiere și directoare | `ls -la` |
| **cp** | Copiază fișiere sau directoare | `cp fisier.txt backup/` |
| **mv** | Mută sau redenumește fișiere | `mv fisier.txt arhiva.txt` |
| **mkdir** | Creează directoare | `mkdir -p dir1/dir2` |
| **rmdir** | Șterge directoare goale | `rmdir dir1` |
| **rm -rf** | Șterge recursiv fișiere și directoare | `rm -rf dir1` |
| **touch** | Creează fișiere goale sau actualizează timestamp | `touch fisier.txt` |
| **cat** | Afișează conținutul fișierelor | `cat fisier.txt` |
| **head** | Afișează primele linii din fișier | `head -n 10 /var/log/syslog` |
| **tail** | Afișează ultimele linii din fișier / monitorizează loguri | `tail -f /var/log/syslog` |
| **wc** | Numără linii, cuvinte, caractere | `wc -lwm fisier.txt` |
| **cut** | Extrage câmpuri delimitate | `cut -d',' -f2 date.csv` |
| **grep** | Caută text în fișiere | `grep -E "root|nologin" /etc/passwd` |
| **diff** | Compară fișiere linie cu linie | `diff f1.txt f2.txt` |
| **chmod** | Schimbă permisiuni | `chmod 755 script.sh` |
| **chown** | Schimbă proprietarul unui fișier | `chown user:group fisier.txt` |
| **umask** | Setează permisiunile implicite pentru fișiere noi | `umask 022` |
| **ln** | Creează linkuri hard sau simbolice | `ln original.txt hard.txt`, `ln -s original.txt soft.txt` |
| **readlink / realpath** | Afișează ținta unui link simbolic | `readlink -f soft.txt` |
| **stat** | Afișează metadate fișier (inode, link count, owner, mtime) | `stat fisier.txt` |
| **basename / dirname** | Extrage numele fișierului sau directorul dintr-o cale | `basename /home/user/file.txt` |

---

## 4. Procese, utilizatori și sistem

| Comandă | Rol | Exemplu |
|----------|------|----------|
| **ps** | Afișează procesele curente | `ps aux | grep bash` |
| **pgrep** | Caută procese după nume | `pgrep sshd` |
| **df / du** | Arată spațiul liber pe disc / utilizarea per director | `df -h`, `du -sh /home` |
| **id** | Afișează UID, GID și grupurile utilizatorului | `id` |
| **useradd / userdel** | Adaugă / șterge utilizatori | `sudo useradd testuser`, `sudo userdel testuser` |
| **chmod / chown / chgrp** | Modifică permisiuni și proprietăți fișiere | `chmod 600 fisier`, `chgrp staff fisier` |
| **uname -sr** | Informații despre sistemul de operare | `uname -sr` |
| **ping / traceroute** | Testează conectivitatea și traseul pachetelor | `ping -c 1 8.8.8.8`, `traceroute google.com` |
| **mktemp** | Creează fișiere temporare | `temp=$(mktemp)` |
| **mkfifo** | Creează FIFO (named pipe) pentru comunicare între procese | `mkfifo canal && echo "mesaj" > canal & cat canal` |
| **ipcs / ipcrm** | Gestionează cozi de mesaje, memorie partajată și semafoare SysV | `ipcs -q`, `ipcrm -q 123` |

---

## 5. Structuri de control și bucle

| Tip | Descriere | Exemplu |
|------|------------|----------|
| **if / elif / else** | Execuție condiționată | `if [ -f fisier ]; then echo exista; fi` |
| **case** | Meniu interactiv sau ramificare pe valori | `case $opt in 1) echo unu;; 2) echo doi;; *) echo invalid;; esac` |
| **for** | Iterare peste liste sau rezultate | `for f in *.sh; do echo $f; done` |
| **while / until** | Repetă până la îndeplinirea unei condiții | `while [ $x -lt 10 ]; do echo $x; ((x++)); done` |
| **select** | Creează meniuri interactive automate | `select opt in start stop exit; do echo $opt; done` |

---

## 6. Funcții în Bash

| Comandă / Concept | Rol | Exemplu |
|--------------------|------|----------|
| `function f() { }` | Definește o funcție | `f() { echo "Salut"; }` |
| `return` | Returnează cod de ieșire | `return 0` |
| `export -f` | Exportă o funcție în subshell | `export -f f` |
| **Logging colorat cu tput** | Mesaje INFO/ERROR colorate | `tput setaf 2; echo "[INFO] ok"; tput setaf 1; echo "[ERROR] fail" >&2` |

---

## 7. Exerciții practice (Laboratoare 2–5)

1. **Backup automat:** verifică dacă `/etc/fstab` există; creează `/backup` și salvează fișierul acolo.
2. **Monitorizare disc:** cu `df -h` și `if`, avertizează dacă spațiul liber < 10%.
3. **Monitorizare procese:** verifică cu `pgrep sshd` și salvează statusul în log.
4. **Verificare permisiuni:** afișează și modifică cu `stat`, `chmod`, `umask`.
5. **Meniu interactiv (select):** adăugare / ștergere / listare utilizatori (useradd, userdel, cat /etc/passwd).
6. **Analiza logurilor:** `head`, `tail -f`, `grep`, `wc` — numără erori și avertismente.
7. **Linkuri:** creează și verifică `ln`, `readlink`, `stat` pentru hard și soft links.
8. **IPC (Inter-Process Communication):** folosește `mktemp`, `mkfifo`, `ipcs`, `ipcrm` pentru fișiere temporare și canale FIFO.
9. **Variabile de mediu:** gestionează `export`, `unset`, `env`, `printenv`.
10. **Automatizare:** bucle `for`, `while`, `until` pentru procesare fișiere și monitorizare periodică.

---

## 8. Rezumat general (Cheat Sheet)

- **Fișiere și directoare:** `ls`, `cp`, `mv`, `rm`, `mkdir`, `touch`, `ln`, `stat`.
- **Procese și sistem:** `ps`, `pgrep`, `df`, `du`, `id`, `uname`, `ping`.
- **Analiză text:** `cat`, `head`, `tail`, `grep`, `cut`, `wc`, `diff`.
- **Control:** `if`, `case`, `for`, `while`, `select`, `tput`.
- **IPC și variabile:** `mktemp`, `mkfifo`, `ipcs`, `export`, `unset`.

---

### Notă finală
Acest ghid include **toate comenzile acoperite în laboratoarele 2–5** (SO I), cu descrieri, exemple și exerciții aplicative pentru fiecare categorie: manipulare fișiere, procese, bucle, IPC, variabile de mediu și administrare sistem.

