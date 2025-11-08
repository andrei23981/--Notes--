# 🧠 README - Curs 2: Clase, Obiecte, **Modificatori** și Testare în Java (versiune completă)

## 🎯 Obiectivul cursului
Materialul de față sintetizează clar și complet conceptele din Cursul 2: **clase**, **obiecte**, **modificatori (clasă, câmp, metodă)**, **constructori**, **tipuri de variabile**, **acces cu `this`/`super`**, **varargs**, **copiere/clonare**, **afișarea obiectelor (`toString`)**, **StringBuilder/StringBuffer** și **testarea unitară**. Este gândit ca un rezumat de învățare, fără sarcini de făcut.

---

## 1) Clasele în Java
O **clasă** descrie datele (câmpuri/atribute) și comportamentele (metode) unui tip de obiect.

```java
public class Student {
    String nume;     // câmp (variabilă de instanță)
    int varsta;      // câmp (variabilă de instanță)

    void afiseaza() { // metodă de instanță
        System.out.println(nume + " are " + varsta + " ani.");
    }
}
```

**Nume de tipuri**: numele claselor încep cu literă mare (PascalCase). O singură clasă `public` per fișier, iar numele fișierului = numele clasei.

---

## 2) **Modificatori de clasă**
Modificatorii controlează vizibilitatea și rolul unei clase:

- `public` – clasa este vizibilă din orice pachet.
- `abstract` – clasa poate conține **metode abstracte** (fără corp); **nu** poate fi instanțiată.
- `final` – clasa **nu** poate fi extinsă (nu poate fi părinte pentru alte clase).

> O clasă **nu** poate fi în același timp `abstract` și `final`.

---

## 3) **Câmpuri (atribute) ale clasei**
### 3.1 Modificatori de **acces** pentru câmpuri
- `public` – acces din orice clasă.
- `protected` – acces din subclase (și din același pachet).
- *(implicit)* (package-private) – acces doar din același pachet.
- `private` – acces doar în clasa curentă.

### 3.2 Alți modificatori pentru câmpuri
- `final` – **constantă**; valoarea nu se schimbă după inițializare. De regulă cu `static` și litere mari (`MAX_SIZE`).
- `static` – un singur exemplar partajat de **toate** obiectele clasei; se accesează ca `Clasa.camp`.
- `transient` – câmpul **nu se serializează** (nu se persistă).
- `volatile` – citirile/scrierile se fac direct în **memoria principală**; util pentru concurență (evită cache-ul pe thread).

```java
public class ExVariables {
    public static final int MAX_CAPACITY = 100; // constantă de clasă
    public String name;        // public
    protected double[] marks;  // protected
    private int i, j, k = 9;   // private
    transient double mean;     // nu se serializează
}
```

### 3.3 Tipuri de **variabile** în Java
| Tip | Unde trăiește | Când există | Observații |
|---|---|---|---|
| **Locală** | în metode/constructori/blocuri | pe durata execuției metodei | **nu** are valori implicite |
| **De instanță** | câmp în clasă (fără `static`) | pentru fiecare obiect | au **valori implicite** |
| **De clasă** (`static`) | câmp în clasă cu `static` | o singură dată per clasă | comună tuturor obiectelor |

---

## 4) **Metode**
### 4.1 Modificatori de **acces** pentru metode
- `public`, `protected`, *(implicit)*, `private` – identic ca la câmpuri.

### 4.2 Alți modificatori pentru metode
- `abstract` – doar semnătură, **fără corp**; obligatoriu în clase `abstract`.
- `static` – metodă de **clasă**; nu are acces la `this`.
- `final` – **nu** poate fi suprascrisă în subclase.
- `synchronized` – acces exclusiv (un singur thread odată) la metodă.
- `native` – implementată în alt limbaj (C/C++), declarată în Java.

```java
public class Calculator {
    public int aduna(int a, int b) { return a + b; }     // metodă de instanță
    public static double pi() { return Math.PI; }         // metodă statică
    public final int identitate(int x) { return x; }      // nu poate fi suprascrisă
}
```

### 4.3 **Varargs** (listă variabilă de parametri)
- Se declară la **finalul** listei de parametri: `tip... nume`.
- În interior, este tratat ca un **tablou**.

```java
void log(String level, String... mesaje) {
    for (String m : mesaje) System.out.println("[" + level + "] " + m);
}
// Apeluri valide:
log("INFO");
log("WARN", "a", "b", "c");
```

---

## 5) **Acces la membri** cu `this` și `super`
- `this` – referință la **obiectul curent** (dispare în `static`). Se folosește frecvent pentru a deosebi câmpurile de parametri (`this.nume = nume`).
- `super` – referință la **clasa de bază**; se folosește pentru a apela constructorul sau metodele părinteleui (`super(...)`, `super.metoda()`).

---

## 6) **Constructori**
- Au același nume ca și clasa; **nu** au tip de retur.
- Dacă **nu** definești niciunul, compilatorul adaugă un **constructor implicit** fără parametri.
- Pot fi **supraincărcați** (mai mulți constructori cu parametri diferiți).

```java
public class Punct {
    int x, y;
    public Punct() { this(0, 0); }           // delegare între constructori
    public Punct(int x, int y) { this.x = x; this.y = y; }
}
```

---

## 7) **Copierea obiectelor**: constructor de copiere vs. `clone()`
- **Constructor de copiere**: `Punct(Punct altul)` – scris manual; control total (ușor de făcut **deep copy**).
- **Clonare**: implementezi `Cloneable` și suprascrii `clone()`; `super.clone()` produce o **copie superficială** (shallow copy). Pentru **deep copy**, clonezi manual sub-structurile (ex. tablouri).

```java
class X implements Cloneable {
    @Override
    public X clone() throws CloneNotSupportedException {
        return (X) super.clone(); // shallow copy
    }
}
X a = new X();
X b = a;          // aceeași referință
X c = a.clone();  // referință diferită
```

---

## 8) **Obiecte și tablouri**
- Declarare tablou: `String[] s;`
- Alocare: `s = new String[3];`
- Inițializare directă: `String[] s = {"Java", "Course", "2"};`
- Parcurgere: `for-each` sau cu index.

```java
for (String el : s) System.out.println(el);
for (int i = 0; i < s.length; i++) System.out.println(s[i]);
```

---

## 9) **Afișarea obiectelor** – `toString()` și clasele de șiruri
### 9.1 Suprascrierea `toString()`
- `toString()` este moștenită din `Object` și se apelează automat când afișezi un obiect.

```java
class Student {
    String nume; int varsta;
    @Override public String toString() {
        return "Student{" + nume + ", " + varsta + "}";
    }
}
System.out.println(new Student()); // apelează toString()
```

### 9.2 `String`, `StringBuilder`, `StringBuffer`
- `String` – **imutabil**; fiecare concatenare creează obiecte noi (costisitor în bucle mari).
- `StringBuilder` – **mutabil**, **nesincronizat** (mai rapid în single-thread).
- `StringBuffer` – **mutabil**, **sincronizat** (thread-safe, dar mai lent).

```java
StringBuilder sb = new StringBuilder();
sb.append("Hello").append(" ").append("World");
String rezultat = sb.toString();
```

---

## 10) **Clasa `Object`** – metode utile
Toate clasele moștenesc `java.lang.Object`. Metode importante:
- `equals(Object o)` – compară **conținutul** (suprascrie pentru semnificație semantică).
- `hashCode()` – compatibil cu `equals()` (obiecte egale → același hash). **Obligatoriu** de păstrat în acord cu `equals`.
- `toString()` – reprezentarea ca text.

> Pentru colecții (`HashSet`, `HashMap`), **contractul** `equals`/`hashCode` corect este esențial.

---

## 11) **Testare unitară** (JUnit 5)
Scopul este verificarea **automată** a metodelor.

```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class Calculator {
    int aduna(int a, int b) { return a + b; }
}

class CalculatorTest {
    @Test void aduna_corect() {
        assertEquals(7, new Calculator().aduna(3, 4));
    }
}
```

- `@Test` marchează metoda ca test.
- Asserțiile (`assertEquals`, `assertTrue`, etc.) validează rezultatul așteptat.

---

## 12) **Rezumat de reținut**
- **Modificatori de clasă:** `public`, `abstract`, `final`.
- **Modificatori de câmp:** acces (`public`/`protected`/`package`/`private`) + `final`, `static`, `transient`, `volatile`.
- **Modificatori de metodă:** acces + `abstract`, `static`, `final`, `synchronized`, `native`.
- **Tipuri de variabile:** locală, de instanță, de clasă (`static`).
- **Constructori** (implicit, cu parametri), **this/super**, **varargs**.
- **Copiere**: constructor de copiere vs `clone()` (shallow vs deep copy).
- **`toString`/`equals`/`hashCode`** și alegerea corectă între `StringBuilder`/`StringBuffer`.
- **Testare** cu JUnit pentru fiabilitate.

---

## 13) Legături utile
- Documentație oficială: *The Java™ Tutorials* (cap. Classes and Objects)
- Javadoc: `java.lang.Object`, `java.lang.StringBuilder`, `java.lang.StringBuffer`
- JUnit 5 User Guide

