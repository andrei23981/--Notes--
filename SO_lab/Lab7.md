# 📘 Ghid Conceptual: Argumente, Fișiere și Procese

**Laborator 8 - Sisteme de Operare**
Acest ghid descompune logica din spatele comenzilor. Scopul este să înțelegi fluxul datelor și gestionarea memoriei în shell, astfel încât să poți construi soluții proprii, nu doar să adaptezi șabloane.

---

## 1. Logica Parsării Argumentelor (getopt)

Când scrii un script simplu, folosești `$1`, `$2`. Dar ce faci când utilizatorul scrie `./script -b valoare -a` în loc de `./script -a -b valoare`? Pozițiile `$1` și `$2` se schimbă, iar logica se rupe.

**Soluția este normalizarea inputului înainte de procesare.**

### ⚙️ Mecanismul din spate

`getopt` nu modifică direct variabilele scriptului. El este doar un procesor de text care ia un șir dezordonat și returnează un șir ordonat, standardizat.

**Fluxul de date:**

* Input utilizator: `-b valoare -a --long`
* Procesare getopt: identifică ce e flag (`-a`), ce e valoare (`valoare`) și ce e greșit
* Output normalizat: `-b 'valoare' -a --long --` (adaugă `--` la final pentru a marca sfârșitul opțiunilor)

---

### 🧠 Comanda Critică: `set --`

Cea mai importantă linie din codul de parsare nu este `getopt`, ci aceasta:

```bash
eval set -- "$OUTPUT_NORMALIZAT"
```

Ce face `set --`?

* Rescrie variabilele poziționale ale shell-ului curent (`$1`, `$2`, `$3`...)
* Înlocuiește argumentele „haotice” date de utilizator cu argumentele „curate” generate de `getopt`

De ce `eval`?

* Pentru ca shell-ul să interpreteze corect ghilimelele și spațiile (ex: dacă un argument este "nume fisier")

---

### 🔄 Ciclul `shift`

După ce am normalizat lista, o parcurgem cu un `while`. Aici intervine conceptul de **fereastră glisantă** folosind `shift`.

* `$1` este mereu elementul curent
* `shift` elimină `$1` și mută totul la stânga (`$2` devine `$1`)

**Logica de consum:**

* Dacă găsesc un flag simplu (`-a`) → execut codul și fac `shift 1`
* Dacă găsesc un flag cu valoare (`-b valoare`) → citesc valoarea din `$2` și fac `shift 2`

---

## 2. Filosofia "Pipeline" (find & xargs)

În Linux, puterea vine din conectarea uneltelor mici. Aici învățăm cum să procesăm eficient volume mari de date.

### 🔍 `find`: Generatorul de liste

`find` este un motor care **generează o listă de căi (paths)**. El nu ar trebui să facă treaba grea, ci doar să identifice resursele.

### 🚀 `xargs`: Procesorul de liste

`xargs` rezolvă o problemă de inginerie: **costul lansării unui proces**.

**Abordarea naivă:**

```bash
find . -name "*.log" -exec rm {} \;
```

* Pentru 1000 de fișiere → 1000 de procese `rm`
* Foarte lent

**Abordarea eficientă (xargs):**

```bash
find . -name "*.log" | xargs rm
```

* 1000 de fișiere → 1 proces `rm`
* Mult mai rapid

---

### 🛡️ Problema Separatorului (NULL byte)

Fișierele pot conține spații sau `
`. Implicit, `xargs` separă după spațiu – ceea ce rupe numele de fișier.

**Soluția corectă (bulletproof):**

```bash
find . -name "*.txt" -print0 | xargs -0 rm
```

Explicație:

* `-print0` → `find` separă prin caracterul **NULL** (`�`)
* `-0` → `xargs` citește folosind caracterul **NULL**

Este singurul separator 100% sigur deoarece este **ilegal în numele fișierelor Linux**.

---

## 3. Arhitectura Proceselor

Când scrii un script care lansează comenzi în background (`&`), devii un **orchestrator de procese**. Trebuie să decizi structura arborelui de procese.

### A. Modelul "Pieptene" (Comb / Fan)

Modelul clasic de paralelism.

**Vizualizare:**

```
       [Părinte]
      /    |    \
  [Fiu1] [Fiu2] [Fiu3] ...
```

**Logică:**

* Părintele lansează mai mulți copii simultan
* Copiii sunt independenți
* Părintele există doar ca să-i aștepte (`wait`)

**Utilizare:**

* Când ai mai multe sarcini independente (ex: descarci 10 fișiere)

---

### B. Modelul "Lanț" (Chain)

Model de dependență secvențială.

**Vizualizare:**

```
[Părinte] → [Fiu] → [Nepot] → [Strănepot]
```

**Logică:**

* Fiecare proces creează următorul
* Se folosește când pasul 2 depinde de pasul 1
* Păstrează separarea memoriei între pași

---

## 4. Concurență: Deadlock și Sincronizare

Când rulezi lucruri în paralel (modelul pieptene), apare riscul conflictelor pe resurse.

### ☠️ Anatomia unui Deadlock

Deadlock = situație în care două procese se blochează reciproc.

**Scenariu clasic:**

* Procesul X deține resursa A și așteaptă B
* Procesul Y deține resursa B și așteaptă A
* Niciunul nu poate continua

Deadlock-ul este o **eroare de logică, nu de sintaxă**.

---

### ✅ Soluția: Ordinea Universală

Impunerea unei reguli globale:

> Toate procesele cer resursele în aceeași ordine (ex: întâi A, apoi B)

Astfel, **așteptarea circulară devine imposibilă**.

---

## 🔒 Lacătul Consultativ (`flock`)

Linux folosește **Advisory Locking** (lacăte pe bază de convenție, nu forțate).

### Ce este un File Descriptor (FD)?

Un număr prin care procesul ține evidența unui fișier deschis:

* `0` → stdin
* `1` → stdout
* `2` → stderr
* `3+` → fișierele tale

### Logica `flock`

```bash
FILE="date.txt"
LOCK_FD=200

# Asociem fișierul de lock cu FD 200
exec 200>$FILE.lock

# Punem lacătul
flock -x 200

# Secțiune critică
 echo "Scriu în fișier în siguranță..." >> $FILE
 sleep 2

# Lacătul se eliberează automat la final
```

Cât timp lacătul este activ:

* Orice alt proces care folosește `flock` pe același fișier va fi pus în așteptare

⚠️ **Important:** `flock` nu protejează împotriva editării manuale (ex: `vim`). Este un sistem bazat pe cooperare ("consultativ").
