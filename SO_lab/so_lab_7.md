# 📘 Ghid de Studiu: Sisteme de Operare - Laborator 8

**Argumente, Fișiere și Controlul Proceselor**  
Acest document sintetizează conceptele critice pentru rezolvarea temelor de laborator. Este structurat pentru a fi parcurs rapid, dar conține toate detaliile tehnice necesare implementării.

---

## 1. Procesarea Argumentelor (getopt)

În scripturile profesionale, nu folosim `$1`, `$2` direct dacă ordinea lor variază. Folosim `getopt` pentru a parsa flag-uri (ex: `-a`, `--file output.txt`).

### 🛠️ Cum funcționează?
`getopt` reordonează argumentele date scriptului astfel încât opțiunile să vină primele, urmate de argumentele poziționale.

### 🔑 Sintaxa Opțiunilor

Trebuie să definești ce litere/cuvinte sunt acceptate și dacă cer argumente:

| Tip | Simbol în string | Semnificație | Exemplu utilizare |
|-----|-------------------|--------------|-------------------|
| Fără argument | `a` | Doar un switch (on/off) | `-a` |
| Argument obligatoriu | `b:` | Cere o valoare după el | `-b valoare` |
| Argument opțional | `c::` | Poate avea valoare sau nu | `-c` sau `-cValoare` |

---

### 📝 Template-ul Standard (De copiat în temă)

Acesta este scheletul pe care trebuie să îl folosești pentru a procesa argumentele robust:

```bash
#!/bin/bash

# 1. Definirea regulilor
# -o: opțiuni scurte (ex: a, b:)
# --long: opțiuni lungi (ex: alpha, beta:)
# "$@": transmite toate argumentele scriptului către getopt
OPTS=$(getopt -o ab:c:: --long alpha,beta:,gamma:: -n 'nume_script' -- "$@");

# Verificăm dacă getopt a returnat eroare (ex: utilizatorul a dat o opțiune invalidă)
if [ $? != 0 ]; then
    echo "Eroare la parsarea argumentelor." >&2
    exit 1
fi

# 2. Reorganizarea argumentelor
# Comanda 'eval' aplică schimbările, punând opțiunile la începutul listei de parametri
eval set -- "$OPTS"

# 3. Bucla de procesare
while true; do
  case "$1" in
    -a | --alpha)
      echo "Opțiunea A a fost selectată"
      shift # Trecem la următorul argument
      ;;
    -b | --beta)
      echo "Opțiunea B cu valoarea: $2"
      shift 2 # Sărim peste opțiune ($1) ȘI peste valoarea ei ($2)
      ;;
    -c | --gamma)
      case "$2" in
        "") echo "Opțiunea C fără argument"; shift 2 ;;
        *)  echo "Opțiunea C cu valoarea: $2"; shift 2 ;;
      esac
      ;;
    --)
      shift; break ;; # 'break' iese din while
    *)
      echo "Eroare internă!"; exit 1 ;;
  esac
done
```

---

## 2. Manipularea Avansată a Fișierelor (find)

Comanda `find` este unică pentru că parcurge recursiv directoarele.

### 🔍 Predicate Principale (Criterii de căutare)

**După nume:**
- `-name "*.log"` (Case sensitive)
- `-iname "*.log"` (Case insensitive – găsește și `FILE.LOG`)

**După tip:**
- `-type f` (fișier)
- `-type d` (director)
- `-type l` (link simbolic)

**După timp:**
- `-mtime -7` (modificat în ultimele 7 zile)
- `-mtime +30` (modificat acum mai mult de 30 de zile)

**După mărime:**
- `-size +10M` (mai mare de 10MB)
- `-size -1k` (mai mic de 1KB)

### ⚡ Acțiuni pe rezultate
- `-delete`: Șterge fișierele găsite.
- `-exec cmd {} \;`: Execută `cmd` pe fiecare fișier (`{}` este înlocuit cu numele fișierului). Este lent pentru multe fișiere.
- `-print0`: Afișează numele fișierelor separate prin caracterul `NULL` (nu prin Enter). Critic pentru fișierele care conțin spații în nume.

---

## 3. Procesare Paralelă și Eficientă (xargs)

`xargs` este partenerul lui `find`. El construiește comenzi folosind output-ul de la `find`.

### De ce `xargs` și nu `find -exec`?
- **Performanță**: `xargs` grupează argumentele (rulează `rm file1 file2 file3` o singură dată, nu `rm file1`, `rm file2`...).
- **Paralelism**: Poate rula comenzi pe mai multe nuclee.

### 🛡️ Combinația "Bulletproof" (Pentru nota 10)

Aceasta este metoda sigură de a procesa fișiere, chiar dacă au nume ciudate (spații, newline-uri):

```bash
# Caută fișiere .txt și le arhivează folosind 4 procese în paralel
find . -name "*.txt" -print0 | xargs -0 -P 4 -I {} tar -czf {}.tar.gz {}
```

### Flag-uri `xargs`

| Flag | Explicație |
|------|-----------|
| `-0` | Citește input-ul separat prin `NULL` (trebuie combinat cu `find -print0`). |
| `-I {}` | Definește `{}` ca placeholder pentru numele fișierului curent. |
| `-P N` | Rulează N procese în paralel. Dacă pui `-P 0`, folosește toate nucleele posibile. |
| `-n N` | Ia maxim N argumente per comandă. |

---

## 4. Controlul Proceselor și Job Control

În Linux, poți lansa procese în fundal (background) pentru a nu bloca terminalul sau scriptul.

### ⚙️ Comenzi de bază
- `&` (la finalul comenzii): Trimite procesul în background.
- `$!` : Variabilă specială care reține PID-ul ultimului proces lansat în background.
- `wait` : Așteaptă terminarea proceselor din background.
- `wait $PID` : Așteaptă doar un anumit proces.

### 🌳 Structuri de Procese (Important la examen/temă)

#### A. Structura "Pieptene" (Comb / Fan)

Un părinte creează N fii. Toți fiii sunt egali, frați între ei. Părintele îi gestionează pe toți.

```bash
echo "Părintele ($$) începe..."
for i in {1..3}; do
    # Lansăm 3 procese care rulează simultan
    sleep 5 &
    echo "Am lansat copilul cu PID $!"
done

wait # Părintele nu iese până nu termină toți cei 3 copii
echo "Toți copiii au terminat."
```

#### B. Structura "Lanț" (Chain)

Părintele creează un fiu, fiul creează un nepot, etc. (A → B → C).  
Aceasta se face recursiv sau iterativ, unde fiecare proces așteaptă doar copilul direct.

---

## 5. Deadlock și Sincronizare

### ☠️ Ce este Deadlock-ul?

Situația în care două sau mai multe procese se blochează reciproc, fiecare așteptând o resursă deținută de celălalt.

**Exemplu:**
- Procesul A are Fișierul 1 și vrea Fișierul 2.
- Procesul B are Fișierul 2 și vrea Fișierul 1.

**Rezultat:** Blocaj etern.

### 🛑 Condițiile Coffman (Toate 4 necesare pentru Deadlock)

1. **Excludere Mutuală** – O resursă nu poate fi folosită simultan de 2 procese.
2. **Hold and Wait** – Ții o resursă în timp ce aștepți alta.
3. **No Preemption** – Resursa nu poate fi luată cu forța de la un proces.
4. **Circular Wait** – Lanțul de așteptare este circular (A → B → A).

### ✅ Soluția: Ordonarea Resurselor

Cea mai simplă metodă de prevenire este ruperea condiției de "Circular Wait".

**Regulă:** Toate procesele trebuie să ceară resursele în aceeași ordine (ex: întotdeauna cere Lock 1, apoi Lock 2).

---

## 🔒 Sincronizare cu `flock` (File Lock)

În bash, folosim fișiere pe post de lacăte (mutex-uri) pentru a proteja zonele critice (ex: scrierea într-un fișier partajat).

```bash
FILE="date.txt"
LOCK_FD=200

# Deschidem fișierul de lock pe descriptorul 200
exec 200>$FILE.lock

# Încercăm să luăm lacătul (exclusiv)
# -x: eXclusive lock (scriere)
# flock va bloca scriptul aici până când lock-ul este eliberat de altcineva
flock -x 200

# --- ÎNCEPUT SECȚIUNE CRITICĂ ---
echo "Scriu în fișier în siguranță..." >> $FILE
sleep 2 # Simulăm lucru
# --- FINAL SECȚIUNE CRITICĂ ---

# Lock-ul se eliberează automat când procesul se termină
# sau explicit cu: flock -u 200
```

