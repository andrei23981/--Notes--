# 📚 Notițe de Studiu

## 📋 Rezumatul Conținutului
Acest document, "Programare III - Curs 2," acoperă concepte fundamentale de Programare Orientată pe Obiecte (OOP) în Java. Se aprofundează definiția și sintaxa claselor și obiectelor, se explorează diverși modificatori pentru clase, metode și atribute și se discută afișarea obiectelor prin metoda `toString()` și clasele asociate. Documentul introduce, de asemenea, subiectul crucial al testării unitare, incluzând principiile, tipurile și implementarea practică în Java folosind framework-ul JUnit 5.

## 🎯 Puncte Cheie
*   **Clase și Obiecte**: Clasele sunt șabloane pentru crearea obiectelor, care sunt instanțe cu stare (atribute) și comportament (metode).
*   **Modificatori**: Modificatorii de acces și alți modificatori (precum `public`, `private`, `abstract`, `final`, `static`) controlează vizibilitatea, comportamentul și ciclul de viață al claselor, metodelor și atributelor.
*   **Reprezentarea Obiectelor**: Metoda `toString()`, moștenită de la clasa `Object`, oferă o reprezentare sub formă de șir de caractere a unui obiect, iar `StringBuilder`/`StringBuffer` oferă modalități eficiente de a manipula șiruri de caractere.
*   **Testare Unitară**: Testarea unitară este un aspect crucial al dezvoltării software care implică testarea componentelor individuale (unități) de cod pentru a se asigura că funcționează corect, JUnit 5 fiind un framework popular pentru acest scop în Java.

## 💡 Explicații Detaliate

### 1. Clase și Obiecte 🏛️

*   **Definiția unei Clase**:
    *   O clasă este un șablon sau un model pentru crearea obiectelor.
    *   Grupează obiecte cu caracteristici similare.
    *   Numele claselor în Java încep de obicei cu literă mare și nu conțin spații.
*   **Sintaxa unei Clase**:
    ```java
    [modificator_clasa] class NumeClasa [extends NumeSuperClasa] [implements NumeInterfata1, NumeInterfata2, ...] {
        // Atribute (câmpuri/variabile)
        // Metode
    }
    ```
*   **Atribute (Câmpuri/Variabile)**:
    *   Acestea descriu proprietățile unei clase.
    *   Sintaxă: `[modificator_atribut] tip_variabila nume_variabila [, nume_variabilaN];`
    *   Atributele pot fi, de asemenea, inițializate în timpul declarării.
*   **Metode**:
    *   Acestea definesc comportamentul unui obiect aparținând unei clase.
    *   Sintaxă: `[modificator_metoda] tip_retur nume_metoda ([lista_parametri]) [throws Exceptie1, ..., ExceptieN] { ... }`
*   **Obiecte**:
    *   Obiectele sunt instanțe ale claselor.
    *   Ele posedă atât stare (definită de atributele lor), cât și comportament (definit de metodele lor).
    *   **Crearea unui Obiect**:
        1.  **Declarare**: Declararea unei variabile de tipul clasei. Astfel de variabile sunt inițializate cu `null` în mod implicit.
            ```java
            NumeClasa numeObiect;
            ```
        2.  **Inițializare**: Folosirea cuvântului cheie `new` urmat de un apel la constructor pentru a crea un obiect real în memorie.
            ```java
            numeObiect = new NumeClasa();
            ```
            Sau, combinat:
            ```java
            NumeClasa numeObiect = new NumeClasa();
            ```

### 2. Modificatori 🔑

Modificatorii definesc caracteristicile claselor, metodelor și variabilelor.

*   **Modificatori de Clasă**:
    *   `public`: Vizibilă în toate pachetele. Numele clasei trebuie să corespundă cu numele fișierului.
    *   `abstract`: Folosit pentru clasele care conțin metode abstracte sau moștenesc metode abstracte de la superclase/interfețe. Clasele abstracte nu pot fi instanțiate.
    *   `final`: Definiția clasei este completă și nu poate fi extinsă de alte clase. O clasă nu poate fi simultan `abstract` și `final`.
*   **Modificatori de Atribut (Câmp)**:
    *   **Modificatori de Acces**:
        *   `public`: Vizibil în toate clasele și pachetele.
        *   `protected`: Vizibil în clasele derivate.
        *   `private`: Vizibil doar în interiorul clasei curente.
        *   *Implicit (pachet)*: Dacă nu este specificat niciun modificator de acces, este vizibil în cadrul aceluiași pachet.
    *   **Alți Modificatori**:
        *   `final`: Declară o constantă a cărei valoare nu poate fi schimbată după inițializare. Constantele sunt de obicei scrise cu majuscule.
        *   `static`: Alocă o singură locație de memorie partajată de toate obiectele clasei. Accesibil prin numele clasei.
        *   `transient`: Indică faptul că o variabilă nu ar trebui să fie persistentă (de exemplu, în timpul serializării).
        *   `volatile`: Asigură că valorile variabilelor nu sunt stocate în cache de către firul de execuție curent și sunt întotdeauna citite și scrise în memoria principală.
*   **Modificatori de Metodă**:
    *   **Modificatori de Acces**: La fel ca pentru atribute (`public`, `protected`, `private`, implicit).
    *   **Alți Modificatori**:
        *   `abstract`: Declară o metodă fără implementare (doar un prototip). Trebuie utilizată în clase abstracte. Nu poate fi `private`, `static`, `final`, `native` sau `synchronized`.
        *   `final`: Metoda nu poate fi suprascrisă de subclase.
        *   `static`: Metoda aparține clasei însăși, nu unui obiect specific. Nu poate accesa variabile de instanță sau metode non-statice (de exemplu, nu are acces la `this`).
        *   `synchronized`: Asigură că un singur fir de execuție poate accesa metoda la un moment dat.
        *   `native`: Indică faptul că metoda este implementată într-un alt limbaj de programare (de exemplu, C, C++).

### 3. Afișarea Obiectelor și Manipularea Șirurilor de Caractere 📝

*   **Metoda `toString()`**:
    *   Fiecare clasă Java moștenește metoda `toString()` de la clasa `Object`.
    *   Această metodă este apelată automat atunci când un obiect este convertit într-un șir de caractere (de exemplu, când este utilizat în `System.out.println()` sau în concatenarea de șiruri).
    *   Este o bună practică să suprascrieți `toString()` în clasele voastre pentru a oferi o reprezentare semnificativă sub formă de șir de caractere a stării obiectului.
*   **Clase de Șiruri de Caractere**:
    *   `String`: Obiecte de șiruri de caractere imuabile. Operațiile care modifică un `String` creează de fapt noi obiecte `String`, ceea ce poate duce la probleme de performanță din cauza creării multor obiecte temporare.
    *   `StringBuilder`: Constructor de șiruri de caractere mutabil. Este eficient pentru manipularea șirurilor, deoarece modifică șirul în loc. **Nu** este sigur pentru fire de execuție (thread-safe). Recomandat pentru medii cu un singur fir de execuție.
    *   `StringBuffer`: Constructor de șiruri de caractere mutabil, similar cu `StringBuilder`, dar este **sigur pentru fire de execuție** datorită sincronizării interne. Recomandat pentru medii cu mai multe fire de execuție.

### 4. Testare Unitară 🧪

*   **Scop**: Să verifice dacă unitățile individuale de cod (de exemplu, metode, funcții) funcționează corect în izolare.
*   **Pregătirea pentru Testare**:
    *   Codul ar trebui scris cu testarea în minte, nu ca o fază separată.
    *   Împărțiți codul în module testabile.
    *   Documentați codul, inclusiv intrările și ieșirile așteptate.
    *   Documentați precondițiile.
*   **Când să Testați**:
    *   După ce codul se compilează fără erori.
    *   Când cunoașteți rezultatele așteptate pentru un set dat de intrări.
    *   Când vă puteți imagina scenarii care ar putea face codul să eșueze.
*   **Tipuri de Teste**:
    *   **Teste Unitare**: Testează funcții sau componente individuale.
    *   **Teste de Regresie**: Asigură că bug-urile remediate anterior nu reapar și că noile modificări nu au stricat funcționalitatea existentă.
    *   **Teste de Integrare**: Verifică dacă diferite module sau componente funcționează corect împreună.
*   **Abordări de Testare**:
    *   **Bazată pe Intuiție**: Verificare parțială.
    *   **Testare Sistematică**: Bazată pe cerințe, acoperind cazuri de excepție și cazuri comune.
    *   **Testare Black Box**: Testare bazată pe specificații și perspectiva utilizatorului, fără cunoașterea structurii interne a codului.
    *   **Testare White/Glass Box**: Testare bazată pe explorarea căilor de cod și a logicii interne, efectuată de obicei de dezvoltatori.
*   **Testarea Unitară în Java cu JUnit 5**:
    *   **Framework**: JUnit 5 este un framework de testare utilizat pe scară largă.
    *   **Clase de Test**: De obicei, denumite prin prefixarea "Test" la clasa testată (de exemplu, `NumeClasaTest`).
    *   **Metode de Test**:
        *   Adnotate cu `@Test`.
        *   Tipul de retur este `void`.
        *   Nu acceptă parametri.
    *   **Asertări**: Clasa `Assertions` oferă metode (de exemplu, `assertEquals`, `assertTrue`, `assertNotNull`) pentru a verifica rezultatele așteptate față de rezultatele reale.
    *   **Adnotări**: Diverse adnotări precum `@DisplayName`, `@BeforeEach`, `@AfterEach`, `@AfterAll` sunt folosite pentru a configura execuția testelor.
    *   **Dependențe Maven**: JUnit 5 necesită includerea unor dependențe specifice (de exemplu, `junit-jupiter-api`, `junit-jupiter-engine`) în configurația de build a proiectului.

## 🔑 Concepte Cheie

*   **Clasă**: Un șablon pentru crearea obiectelor, definind atributele și comportamentele acestora.
*   **Obiect**: O instanță a unei clase, reprezentând o entitate din lumea reală cu propria sa stare și comportament.
*   **Atribut (Câmp/Variabilă)**: O variabilă declarată în cadrul unei clase care reprezintă starea unui obiect.
*   **Metodă (Funcție)**: Un bloc de cod în cadrul unei clase care definește comportamentul unui obiect.
*   **Modificator**: Cuvinte cheie care specifică nivelul de acces, comportamentul sau alte caracteristici ale claselor.
