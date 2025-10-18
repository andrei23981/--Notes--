# 📚 Notițe de Studiu

## 📋 Rezumatul Conținutului
Acest document, "Programare III - Curs 3," acoperă concepte avansate de programare în Java, concentrându-se pe relațiile dintre clase, moștenire, clase abstracte, interfețe și clase imbricate. Se aprofundează nuanțele modului în care aceste construcții permit reutilizarea codului, extensibilitatea și proiectarea robustă a software-ului. Materialul atinge, de asemenea, enumerările și avantajele acestora față de declarațiile tradiționale de constante.

## 🎯 Puncte Cheie

*   **Moștenirea:** Un mecanism fundamental pentru crearea de noi abstracțiuni din cele existente, permițând reutilizarea codului și stabilirea relațiilor "este-un" (is-a).
    *   Suportă moștenirea unică pentru clase.
    *   Implică clase de bază (părinte) și derivate (copil).
*   **Clase Abstracte și Interfețe:** Instrumente pentru definirea contractelor și abstractizarea comportamentului.
    *   Clasele abstracte pot avea atât metode abstracte, cât și concrete și nu pot fi instanțiate.
    *   Interfețele definesc un set de metode abstracte și pot include, de asemenea, metode implicite (default), statice și private (începând cu Java 8/9).
*   **Clase Imbricate (Nested Classes):** Clase definite în interiorul altei clase, oferind grupare logică, încapsulare sporită și lizibilitate îmbunătățită a codului.
    *   Tipurile includ clase imbricate statice, clase interne, clase locale și clase anonime.
*   **Enumerări (Enums):** Un tip de date special pentru reprezentarea unui set fix de constante numite, oferind siguranță la nivel de tip (type safety) și o lizibilitate mai bună decât constantele tradiționale.

## 💡 Explicații Detaliate

### Moștenirea

*   **Concept:** Moștenirea permite unei clase să moștenească proprietăți și comportamente de la o altă clasă. Aceasta promovează relația "este-un" (de exemplu, un `Caine` este un `Animal`).
*   **Terminologie:**
    *   **Clasă de Bază (Superclasă/Clasă Părinte):** Clasa din care sunt moștenite proprietățile.
    *   **Clasă Derivată (Subclasă/Clasă Copil):** Clasa care moștenește proprietățile.
    *   **Relație de Tip (Kind-of):** La nivel de clasă, indicând o ierarhie de tipuri (de exemplu, `Cerc` este un tip de `Figura`).
    *   **Relație "Este-un" (Is-a):** La nivel de obiect, indicând că o instanță aparține unui tip (de exemplu, `cerculMeu` este o `Figura`).
*   **Tipuri de Moștenire:**
    *   **Moștenire Unică:** O clasă moștenește de la o singură clasă de bază. Acest lucru este suportat în Java pentru clase.
    *   **Moștenire Multiplă:** O clasă moștenește de la mai multe clase de bază. Acest lucru **nu** este suportat direct pentru clase în Java pentru a evita "problema diamantului". Cu toate acestea, Java realizează o formă de moștenire multiplă de **tip** prin intermediul interfețelor.
*   **Sintaxă:** `class ClasaDerivata extends ClasaDeBaza { ... }`
*   **Constructori și Cuvântul Cheie `super`:**
    *   Constructorii clasei derivate trebuie să apeleze un constructor al clasei de bază, de obicei folosind cuvântul cheie `super()`.
    *   `super` poate fi folosit pentru a face referire la metodele sau câmpurile clasei de bază.
*   **Tipuri de Retur Covariante:** La suprascrierea unei metode, metoda clasei derivate poate returna un subtip al tipului de retur al metodei din clasa de bază.
*   **Adnotarea `@Override`:** Folosită pentru a indica faptul că o metodă este menită să suprascrie o metodă dintr-o superclasă. Compilatorul verifică corectitudinea.

### Clase Abstracte

*   **Definiție:** Declarate folosind cuvântul cheie `abstract`. Pot conține atât metode abstracte, cât și concrete.
*   **Metode Abstracte:** Metode declarate fără implementare. Acestea trebuie implementate de subclasele concrete.
    *   Sintaxă: `public abstract void numeMetoda();`
*   **Proprietăți:**
    *   Nu pot fi instanțiate direct.
    *   Pot conține metode abstracte și non-abstracte (concrete).
    *   Pot conține câmpuri non-statice și non-constante.

### Interfețe

*   **Definiție:** Similare cu clasele abstracte, dar istoric puteau conține doar metode abstracte și constante. Java modern (8+) a introdus metode implicite (default), statice și private.
*   **Conținut:**
    *   Constante (implicit `public static final`).
    *   Prototipe de metode (implicit `public abstract`).
    *   Metode implicite (cu implementare, pot fi suprascrise).
    *   Metode statice (cu implementare, aparțin interfeței însăși).
    *   Metode private (introduse în Java 9 pentru metode ajutătoare în cadrul metodelor implicite/statice).
    *   Tipuri imbricate.
*   **Sintaxă:** `[modificator] interface NumeInterfata { ... }`
*   **API (Interfețe de Programare a Aplicațiilor):** Interfețele servesc adesea drept contracte care definesc modul în care diferite componente software interacționează.
*   **Moștenirea cu Interfețe:**
    *   O clasă poate implementa mai multe interfețe.
    *   O interfață poate extinde mai multe interfețe.
*   **Adnotarea `@Override`:** Esențială la implementarea metodelor de interfață pentru a asigura corectitudinea.
*   **Metode Implicite (Default Methods):** Permit adăugarea de noi metode la interfețe fără a strica implementările existente.
    *   **Conflicte:** Dacă o clasă implementează mai multe interfețe cu metode implicite având aceeași semnătură, apare un conflict. Clasa trebuie să implementeze sau să suprascrie explicit metoda pentru a rezolva ambiguitatea.
*   **Metode Statice:** Pot fi apelate direct pe numele interfeței (de exemplu, `NumeInterfata.metodaStatica()`). Nu pot fi suprascrise de clasele care implementează interfața.
*   **Metode Private:** Folosite pentru a partaja cod între metodele implicite și statice din cadrul unei interfețe.
*   **Interfețe Funcționale:** Interfețe cu o singură metodă abstractă. Sunt compatibile cu expresiile lambda și referințele la metode. Adnotarea `@FunctionalInterface` poate fi folosită pentru a asigura acest lucru.

### Clase Imbricate (Nested Classes)

*   **Definiție:** O clasă definită în interiorul altei clase.
*   **Scop:**
    *   **Grupare Logică:** Grupează clasele care sunt folosite doar într-un singur loc.
    *   **Încapsulare:** Crește încapsularea făcând clasa imbricată inaccesibilă din afara clasei care o conține.
    *   **Lizibilitate și Mentenabilitate:** Poate face codul mai ușor de înțeles și de gestionat.
*   **Tipuri:**
    *   **Clase Imbricate Statice:** Declarate cu cuvântul cheie `static`. Sunt membre ale clasei exterioare, dar nu au acces la membrii de instanță ai clasei exterioare. Sunt ca niște clase obișnuite în interiorul altei clase.
    *   **Clase Interne (Clase Imbricate Non-statice):** Nu sunt declarate `static`. Sunt asociate cu o instanță a clasei exterioare și au acces la toți membrii (inclusiv privați) ai clasei exterioare.
    *   **Clase Locale:** Declarate în interiorul unei metode sau al unui bloc de cod. Sunt vizibile doar în acel bloc.
    *   **Clase Anonime:** Clase locale fără nume. Sunt definite în linie la crearea unui obiect, adesea pentru a implementa interfețe sau a extinde clase din mers.

### Conversia Obiectelor și Polimorfismul

*   **Conversia Obiectelor:** Un obiect de tipul unei clase derivate poate fi tratat ca un obiect de tipul clasei sale de bază (upcasting).
*   **Polimorfism:** Abilitatea unei referințe la un obiect de a se referi la obiecte de diferite tipuri (derivate dintr-o clasă de bază comună sau care implementează o interfață comună).
    *   **Legare Dinamică (Dynamic Binding):** Decizia cu privire la ce metodă să se execute se ia la momentul rulării, pe baza tipului real al obiectului, nu a tipului referinței. Acest lucru este crucial pentru polimorfism.
    *   **Operatorul `instanceof`:** Folosit pentru a verifica tipul la momentul rulării al unui obiect. Returnează `true` dacă obiectul este o instanță a unui anumit tip sau a unui subtip, altfel `false`. Ajută la conversia sigură a obiectelor (casting).

### Enumerări (Enums)

*   **Scop:** Pentru a reprezenta un set fix de constante numite.
*   **Probleme cu Abordările Vechi (folosind `static final int`):**
    *   **Lipsa de Claritate:** Valorile sunt doar numere întregi, ceea ce face ca semnificația lor să fie neclară fără comentarii.
    *   **Fără Siguranță la Nivel de Tip:** Compilatorul permite amestecarea numerelor întregi cu constantele enum, ceea ce poate duce la erori.
    *   **Fără Spațiu de Nume (Namespace):** Constantele pot intra în conflict dacă sunt definite în aceeași clasă.
    *   **Afișare Slabă:** Afișarea valorii întregi nu transmite semnificația dorită.
*   **Cuvântul Cheie `enum` din Java (începând cu Java 5):**
    *   Oferă o modalitate sigură la nivel de tip pentru a defini constante.
    *   Sintaxă: `enum NumeEnum { CONSTANTA1, CONSTANTA2, ... }`
*   **Caracteristici:**
    *   **Siguranță la Nivel de Tip:** Compilatorul impune utilizarea doar a valorilor enum valide.
    *   **Lizibilitate:** Constantele enum au nume sugestive.
    *   **Date Suplimentare:** Constantele enum pot avea constructori, câmpuri și metode, permițându-le să stocheze informații suplimentare (de exemplu, asocierea unei culori cu o suită de cărți de joc).
    *   **Metoda `values()`:** Returnează un tablou cu toate constantele enum.
    *   **Comparație:** Constantele enum pot fi comparate folosind `==` sau utilizate în instrucțiuni `switch`.

## 🔑 Concepte Cheie

*   **Moștenirea:** Un mecanism prin care o clasă dobândește proprietățile alteia.
*   **Polimorfism:** Abilitatea unui obiect de a lua mai multe forme. În OOP, se realizează adesea prin moștenire și interfețe, permițând ca obiecte de clase diferite să fie tratate ca obiecte ale unei superclase comune.
*   **Abstracție:** Ascunderea detaliilor complexe de implementare și afișarea doar a caracteristicilor esențiale. Clasele abstracte și interfețele sunt instrumente cheie pentru abstracție.
*   **Încapsulare:** Gruparea datelor (câmpuri) și a metodelor care operează asupra datelor într-o singură unitate (clasă). Clasele imbricate pot spori încapsularea.
*   **Clasă Abstractă:** O clasă care nu poate fi instanțiată și poate conține metode abstracte.
*   **Interfață:** Un contract care definește un set de metode pe care o clasă trebuie să le implementeze.
*   **Clasă Imbricată:** O clasă definită în interiorul alteia.
