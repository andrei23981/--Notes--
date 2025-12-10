# 📘 Manualul Expertului în Scripting Bash & Linux

**Nivel:** B (Avansat)  
**Bază:** Laboratoarele 8, 9, 10  
**Scop:** Înțelegerea profundă a automatizării, procesării textului și controlului proceselor.

---

## 🏗️ Capitolul 1: Arta Scripturilor Flexibile
Scripturile puternice nu sunt rigide — ele se adaptează! În această secțiune înveți cum creezi scripturi care procesează argumente ca niște comenzi Linux reale și cum cauți fișiere eficient.

### 🔹 1.1. Cum „vorbesc” scripturile tale? — **getopts**
`getopts` este recepționerul care traduce opțiunile date de utilizator.

**De ce `getopts` și nu `getopt`?**
- **getopts**: rapid, built‑in, POSIX, perfect pentru majoritatea scripturilor.
- **getopt**: opțiuni lungi, dar mai complicat și extern.

#### 🧠 Anatomia unei bucle getopts
```bash
while getopts "f:v" optiune; do
    case $optiune in
        f) FISIER="$OPTARG" ;;
        v) VERBOSE=1 ;;
        *) echo "Opțiune invalidă!"; exit 1 ;;
    esac
done
```

---

### 🔹 1.2. Vânătoarea de Fișiere — **find & xargs**
`find` este unealta supremă pentru scripturi: precisă, puternică și scalabilă.

#### 🔍 Exemple utile
- Stergerea log‑urilor vechi:
```bash
find /var/log -name "*.log" -mtime +7 -delete
```

- Căutarea fișierelor mari:
```bash
find /home -type f -size +100M
```

#### 📦 Puterea lui `xargs`
`xargs` livrează eficient liste mari de elemente către alte comenzi, evitând erorile de tip *argument list too long*.

**Profesional:** manipularea fișierelor cu spații:
```bash
find . -name "*.jpg" -print0 | xargs -0 mv -t /backup/
```

---

## 🔬 Capitolul 2: Chirurgul de Text (Regex, sed, awk)
În Linux, textul este limbajul sistemului. Stăpânești textul? Stăpânești sistemul.

### 🔹 2.1. Expresii regulate (Regex) — limbajul modelelor
| Simbol | Nume | Explicație | Exemplu |
|-------|------|------------|----------|
| ^ | Anchor început | linia începe cu | `^Error` |
| $ | Anchor final | linia se termină cu | `done$` |
| . | Joker | orice caracter | `c.t → cat/cut` |
| * | Repetitor | 0+ repetări | `10*` |
| [ ] | Set | caractere permise | `[aeiou]` |
| [^ ] | Set negat | orice în afară | `[^0-9]` |

Regex extins (ERE): `+`, `?`, `|`.

---

### 🔹 2.2. **grep** — Detectivul de text
- Căutare utilizatori root/admin:
```bash
grep -E "root|admin" /etc/passwd
```
- Căutare recursivă:
```bash
grep -r "TODO" .
```
- Excluderi:
```bash
grep -v "succes" log.txt
```

---

### 🔹 2.3. **sed** — Editorul de flux
- Substituție globală:
```bash
sed -i 's/http:/https:/g' config.xml
```
- Ștergerea liniilor comentate:
```bash
sed '/^#/d' config.conf
```

---

### 🔹 2.4. **awk** — Analistul de date
Procesare pe coloane:
```bash
ls -l | awk '
BEGIN { print "Încep calculul..." }
NR > 1 { sum += $5 }
END { print "Total Bytes:", sum }
'
```

---

## 🚦 Capitolul 3: Dirijorul de orchestră (Procese & Semnale)
Procesele sunt „instrumentele” tale. Învață să le controlezi.

### 🔹 3.1. Semnale importante
| Semnal | Cod | Descriere |
|--------|-----|------------|
| SIGINT | 2 | întrerupere (Ctrl+C) |
| SIGTERM | 15 | terminare elegantă |
| SIGKILL | 9 | omoară instant |
| SIGSTOP | 19 | pune pe pauză |
| SIGCONT | 18 | reia execuția |

---

### 🔹 3.2. `trap` — plasa de siguranță
```bash
cleanup() {
    echo "Curățenie..."
    rm -f /tmp/temp_*
}
trap "cleanup; exit" SIGINT SIGTERM
```

---

### 🔹 3.3. Comunicarea între procese (IPC)
- **Pipe anonim:** `|`
- **Pipe numit (FIFO):**
```bash
mkfifo mypipe
```
- **Blocare fișier (`flock`):**
```bash
(
  flock -x 200
  echo "Scriu..." >> fisier.log
  sleep 5
) 200>fisier.lock
```

---

### 🔹 3.4. Deadlock — inamicul invizibil
Două procese blocate reciproc → stagnare totală.

Soluții: time‑out, `flock -n`, retry logic.

---

## 🌟 Rezumat Vizual al Comenzilor
| Comandă | Categorie | Explicație pe scurt |
|---------|-----------|----------------------|
| `getopts` | Scripting | procesează opțiuni |
| `find` | Căutare | găsește fișiere |
| `xargs` | Procesare | pasează liste mari |
| `grep` | Text | caută linii |
| `sed` | Text | modifică textul |
| `awk` | Text | procesează coloane |
| `ps` | Procese | arată procese |
| `kill` | Procese | trimite semnale |
| `mkfifo` | IPC | pipe cu nume |
| `flock` | IPC | blocare pentru scriere |

---

✍️ *Document generat automat și stilizat pentru lizibilitate maximă.*

