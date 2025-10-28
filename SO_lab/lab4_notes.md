# 📚 Note de Studiu

## 📋 Rezumatul Conținutului
Acest document este un manual de laborator pentru cursul "Sisteme de Operare", concentrându-se în special pe Laboratorul 4. Acesta acoperă concepte esențiale de scripting Bash, inclusiv instrucțiuni condiționale (`if`, `elif`, `else`), structuri de decizie (`case`), permisiuni pentru fișiere și directoare (reprezentări simbolice, binare și octale), și comanda `umask`. Materialul este prezentat cu explicații clare, exemple practice și exerciții pentru a consolida învățarea.

## 🎯 Puncte Cheie

*   **Instrucțiuni Condiționale (`if`, `elif`, `else`)**:
    *   Execută blocuri de cod pe baza veridicității condițiilor.
    *   Suportă condiții complexe folosind operatori logici (`&&` pentru ȘI, `||` pentru SAU) și negație (`!`).
    *   Pot fi folosite pentru a verifica existența fișierelor, permisiunile, comparații de șiruri și evaluări numerice.

*   **Structuri de Decizie (`case`)**:
    *   Oferă o modalitate mai curată de a gestiona mai multe condiții sau potriviri de modele comparativ cu instrucțiunile `if-else` imbricate.
    *   Ideală pentru scripturi conduse de meniu sau când se verifică o variabilă față de mai multe valori posibile.

*   **Permisiuni pentru Fișiere**:
    *   Controlează accesul la fișiere și directoare pentru utilizatori, grupuri și alții.
    *   Reprezentate în formate simbolice (`rwx`), binare (`101`) și octale (`751`).
    *   Comanda `chmod` este folosită pentru a modifica aceste permisiuni.

*   **Comanda `umask`**:
    *   Determină permisiunile implicite pentru fișierele și directoarele nou create.
    *   Acționează ca o "mască" pentru a elimina permisiuni din maximul posibil (666 pentru fișiere, 777 pentru directoare).
    *   Înțelegerea `umask` este crucială pentru gestionarea securității sistemului și controlul accesului.

## 💡 Explicație Detaliată

### 1. Instrucțiuni Condiționale (`if`, `elif`, `else`)

Instrucțiunea `if` este fundamentală pentru controlul fluxului de execuție în scripturile Bash. Permite executarea anumitor comenzi doar dacă o condiție dată evaluează la adevărat.

#### Sintaxă:
```bash
if [ condition ]; then
  # Comenzi de executat dacă condiția este adevărată
fi
```

#### Extinderea cu `elif` și `else`:
Pentru scenarii cu mai multe rezultate posibile, `elif` (else if) și `else` pot fi folosite:

```bash
if [ condition1 ]; then
  # Comenzi pentru condition1
elif [ condition2 ]; then
  # Comenzi pentru condition2
else
  # Comenzi dacă nici o condiție nu este îndeplinită
fi
```

#### Caracteristici Cheie:
*   **Comparații Numerice**: Operatori precum `-gt` (mai mare decât), `-lt` (mai mic decât), `-eq` (egal cu), `-ne` (diferit de) sunt folosiți pentru numere.
*   **Comparații de Șiruri**: Operatori precum `=` (egal cu), `!=` (diferit de) sunt folosiți pentru șiruri. Constructul `[[ ]]` oferă potrivire avansată de șiruri și evită problemele cu caractere speciale.
*   **Teste de Fișiere**: Opțiuni precum `-f` (fișierul există), `-d` (directorul există), `-s` (fișierul nu este gol), `-r` (citibil), `-w` (inscriptibil), `-x` (executabil) sunt indispensabile pentru interacțiunea cu sistemul de fișiere.
*   **Condiții Complexe**:
    *   `&&` (ȘI): Ambele condiții trebuie să fie adevărate.
    *   `||` (SAU): Cel puțin o condiție trebuie să fie adevărată.
    *   `!` (NU): Neagă o condiție.

#### Constructul Avansat `[[ ]]`:
Sintaxa `[[ ]]` oferă capabilități îmbunătățite:
*   Potrivire mai puternică de modele pentru șiruri (de ex., `==` poate fi folosit cu wildcards).
*   Utilizare directă a `&&` și `||` în expresie fără a necesita grupare suplimentară.
*   Evită necesitatea de escaping excesiv al caracterelor speciale.

### 2. Structuri de Decizie (`case`)

Instrucțiunea `case` este un instrument puternic pentru potrivirea de modele față de valoarea unei variabile. Este adesea mai lizibilă decât o serie lungă de instrucțiuni `if-elif-else` când se ocupă de mai multe posibilități discrete.

#### Sintaxă:
```bash
case $variable in
  pattern1)
    # Comenzi pentru pattern1
    ;;
  pattern2|pattern3)
    # Comenzi pentru pattern2 sau pattern3
    ;;
  *)
    # Comenzi implicite (catch-all)
    ;;
esac
```

#### Caracteristici Cheie:
*   **Modele**: Pot fi valori specifice, wildcards (`*`), sau valori multiple separate prin `|`.
*   **`;;`**: Marchează sfârșitul unui bloc de comenzi pentru un model specific.
*   **`*)`**: Cazul implicit, executat dacă nici un alt model nu se potrivește.
*   **Cazuri de Utilizare**: Ideal pentru crearea de meniuri, parsarea argumentelor din linia de comandă, sau gestionarea diferitelor tipuri de intrare.

### 3. Permisiuni pentru Fișiere

Permisiunile pentru fișiere sunt o piatră de temelie a sistemelor de operare de tip Unix, definind cine poate citi, scrie sau executa un fișier sau director.

#### Categorii de Utilizatori:
*   **Utilizator (`u`)**: Proprietarul fișierului.
*   **Grup (`g`)**: Utilizatorii aparținând grupului fișierului.
*   **Alții (`o`)**: Toți ceilalți utilizatori.

#### Tipuri de Permisiuni:
*   **Citire (`r`)**: Vizualiza conținutul fișierului sau lista conținutul directorului.
*   **Scriere (`w`)**: Modifica conținutul fișierului sau creează/șterge fișiere într-un director.
*   **Executare (`x`)**: Rulează un fișier ca program sau intră într-un director.

#### Reprezentări:
*   **Simbolice**: Folosește litere `r`, `w`, `x` pentru fiecare categorie (de ex., `rwxr-xr--`).
*   **Binare**: Folosește `1` pentru permisiune acordată și `0` pentru permisiune refuzată (de ex., `111101100`).
*   **Octale**: Convertește fiecare grup de trei cifre binare într-o singură cifră octală (0-7).
    *   `rwx` = `111` = 7
    *   `rw-` = `110` = 6
    *   `r-x` = `101` = 5
    *   `r--` = `100` = 4
    *   `-wx` = `011` = 3
    *   `-w-` = `010` = 2
    *   `--x` = `001` = 1
    *   `---` = `000` = 0

#### Comanda `chmod`:
Folosit pentru a schimba permisiunile.
*   **Mod Simbolic**: `chmod u+w,g-r,o=x file.txt` (adaugă scriere pentru utilizator, elimină citire pentru grup, setează alții la executare).
*   **Mod Octal**: `chmod 754 file.txt` (setează permisiunile la `rwxr-xr--`).

### 4. Comanda `umask`

Comanda `umask` controlează permisiunile implicite pentru fișierele și directoarele create de un utilizator. Specifică care permisiuni ar trebui *eliminate* din maximul posibil când un element nou este creat.

#### Permisiuni Maxime:
*   Fișiere: `666` (`rw-rw-rw-`)
*   Directoare: `777` (`rwxrwxrwx`)

#### Cum Funcționează:
`umask` scade valoarea sa din permisiunile maxime.
*   Exemplu: Dacă `umask` este `022`, atunci:
    *   Pentru fișiere: `666 - 022 = 644` (`rw-r--r--`).
    *   Pentru directoare: `777 - 022 = 755` (`rwxr-xr-x`).

#### Valori Implicite:
Valori comune implicite pentru `umask` sunt `002` sau `022`, concepute pentru a oferi acces restrictiv pentru alții în timp ce permit acces complet proprietarului.

## 🔑 Concepte Cheie

*   **Execuție Condițională**: Rularea blocurilor de cod doar când criterii specifice sunt îndeplinite.
*   **Luarea Deciziilor**: Capacitatea unui script de a alege între diferite căi de execuție.
*   **Permisiuni (rwx)**: Mecanismul fundamental de control al accesului pentru fișiere și directoare în sistemele de tip Unix.
*   **Proprietar, Grup, Alții**: Cele trei categorii principale de utilizatori pentru care sunt definite permisiunile.
*   **Permisiuni Simbolice**: Reprezentare lizibilă pentru oameni a permisiunilor folosind litere (`r`, `w`, `x`).
*   **Permisiuni Octale**: O reprezentare numerică a permisiunilor, adesea folosită cu `chmod`.
*   **Permisiuni Implicite**: Permisiuni atribuite automat fișierelor și directoarelor nou create.
*   **Mască de Permisiuni (`umask`)**: O valoare care modifică permisiunile implicite prin eliminarea drepturilor specifice.
*   **Scripting Shell**: Scrierea scripturilor în shell-ul Bash pentru a automatiza sarcini.
*   **Flux de Control**: Ordinea în care instrucțiunile sunt executate într-un script.
*   **Potrivire de Modele**: Compararea unei variabile sau intrări față de modele predefinite (folosit în instrucțiunile `case`).

## 📝 Rezumatul Învățării
Acest document oferă o introducere cuprinzătoare în structurile de flux de control (`if`, `case`) și concepte esențiale de gestionare a sistemului de fișiere (permisiuni, `umask`) în scripting Bash. Înțelegerea acestor subiecte este crucială pentru scrierea scripturilor robuste și sigure. Exemplele practice și exercițiile sunt concepute pentru a consolida înțelegerea modului de implementare a acestor concepte în scenarii din lumea reală.