# 🧠 README – Curs 3: Moștenire, Clase Abstracte, Interfețe, Clase Îmbricate și Enumerări (Explicații clare și complete)

## 🎯 Obiectivul cursului
Acest curs explică **relațiile dintre clase** și cum putem construi ierarhii logice prin **moștenire**, **clase abstracte** și **interfețe**. De asemenea, discută **clasele imbricate** (o clasă definită în interiorul alteia) și **enumerările (enum)** – tipuri speciale care definesc valori fixe.

Scopul este să înțelegi cum aceste mecanisme sprijină **organizarea, extinderea și reutilizarea codului** în Java.

---

## 🧩 1. Relațiile între clase
În Java, clasele pot fi legate între ele în mai multe moduri:

- **Dependență** – o clasă folosește temporar alta (ex: ca parametru într-o metodă).
- **Asociere** – o clasă are o legătură logică cu alta (ex: un `Profesor` are un `Student`).
- **Agregare** – o clasă conține o altă clasă, dar ambele pot exista separat.
- **Compoziție** – o clasă conține o altă clasă care **nu poate exista separat** (viață comună).
- **Moștenire** – o clasă „extinde” alta pentru a-i **prelua și completa comportamentul**.

---

## 🏗️ 2. Moștenirea
Moștenirea este un mecanism care permite **crearea unei clase noi (derivate)** pe baza unei **clase existente (de bază)**.

```java
public class Figura {
    Color culoare = Color.RED;
}

public class Cerc extends Figura {
    int raza;
}
```

➡️ `Cerc` **moștenește** `Figura`, deci are automat acces la variabila `culoare`.

### Termeni importanți
- **Clasa de bază (superclasă)** – clasa originală din care moștenim.
- **Clasa derivată (subclasă)** – clasa nouă care extinde funcționalitatea.

### Tipuri de moștenire
- **Simplă** – o singură clasă de bază (Java suportă doar aceasta).
- **Multiplă** – mai multe clase de bază (Java NU o suportă pentru clase, dar o permite pentru interfețe).

### Constructori și `super`
Când o clasă derivată este creată, **constructorul clasei de bază se apelează primul** cu ajutorul cuvântului cheie `super`:

```java
public class Figura {
    Color culoare;
    public Figura(Color c) { this.culoare = c; }
}

public class Cerc extends Figura {
    int raza;
    public Cerc(int raza, Color c) {
        super(c); // apel către constructorul clasei de bază
        this.raza = raza;
    }
}
```

---

## 🧠 3. Clase Abstracte
O **clasă abstractă** este o clasă care nu poate fi instanțiată direct (nu poți face `new`), dar poate conține **metode abstracte** – adică metode declarate, dar fără implementare.

```java
public abstract class Figura {
    abstract double aria(); // doar definită, fără corp

    public void descriere() {
        System.out.println("Figură geometrică generică");
    }
}

public class Dreptunghi extends Figura {
    double l, L;
    public Dreptunghi(double l, double L) {
        this.l = l; this.L = L;
    }
    @Override
    double aria() { return l * L; }
}
```

🧩 **Scopul**: clasele abstracte oferă un șablon comun pentru alte clase.

---

## ⚙️ 4. Interfețe
O **interfață** definește un **contract** de metode pe care alte clase trebuie să le implementeze. Interfețele pot fi considerate „promisiuni de comportament”.

```java
public interface Sunet {
    void reda(); // metodă abstractă implicit
}

public class Caine implements Sunet {
    public void reda() {
        System.out.println("Ham ham!");
    }
}
```

### Ce conține o interfață:
- **Constante** (implicite `public static final`)
- **Metode abstracte** (implicite `public abstract`)
- **Metode `default`** – cu implementare implicită (Java 8+)
- **Metode `static`** – metode de utilitate (Java 8+)
- **Metode `private`** – folosite doar intern (Java 9+)

### Exemple
```java
interface Salut {
    default void spuneSalut() {
        System.out.println("Salut din interfață!");
    }
}
```

O clasă poate **implementa mai multe interfețe**:
```java
class Telefon implements GPS, Camera, Bluetooth {}
```

🧠 **Important:** dacă două interfețe au metode `default` cu același nume, clasa trebuie să le **suprascrie**.

### Interfețe funcționale
O interfață cu **o singură metodă abstractă** (ex: `Runnable`) este numită *funcțională*. Poate fi folosită cu **lambda expressions**:
```java
@FunctionalInterface
interface Operatie { int aplica(int a, int b); }
Operatie aduna = (a, b) -> a + b;
System.out.println(aduna.aplica(3, 4)); // 7
```

---

## 🌀 5. Polimorfismul și operatorul `instanceof`
**Polimorfismul** înseamnă că același cod poate acționa diferit în funcție de tipul real al obiectului.

```java
Figura f = new Cerc(10, Color.BLUE);
System.out.println(f.toString()); // apel către versiunea din Cerc
```

### Operatorul `instanceof`
Verifică dacă un obiect aparține unui anumit tip:
```java
if (f instanceof Cerc) {
    System.out.println("f este un Cerc");
}
```
Returnează `true` dacă obiectul poate fi convertit în acel tip fără eroare.

---

## 🧱 6. Clase Imbricate (Nested Classes)
O **clasă imbricată** este o clasă definită în interiorul alteia. Ele ajută la **organizarea codului** și **încapsularea logică**.

### Tipuri principale
1. **Clase membre statice** – acces doar la membri statici ai clasei exterioare.
2. **Clase interne (non-statice)** – au acces complet la membrii clasei exterioare.
3. **Clase locale** – definite într-o metodă, vizibile doar acolo.
4. **Clase anonime** – fără nume, folosite pentru obiecte rapide, unice.

### Exemplu
```java
class Extern {
    private int valoare = 10;
    class Intern {
        void afiseaza() {
            System.out.println("Valoare: " + valoare);
        }
    }
}
```
Crearea unei instanțe:
```java
Extern e = new Extern();
Extern.Intern i = e.new Intern();
i.afiseaza(); // Valoare: 10
```

🧠 **Avantaje:** cod mai clar, izolare logică, securitate sporită.

### Clase anonime
Folosite când ai nevoie de o implementare unică:
```java
interface Salut {
    void spune();
}
public class Main {
    public static void main(String[] args) {
        Salut s = new Salut() {
            public void spune() { System.out.println("Salut anonim!"); }
        };
        s.spune();
    }
}
```

---

## 🧾 7. Enumerări (Enums)
Enumerările definesc **seturi fixe de valori**, cum ar fi zilele săptămânii sau culorile unui joc de cărți.

```java
public enum Suit {
    CLUBS, DIAMONDS, HEARTS, SPADES
}
```

Avantaj față de constantele clasice (`final int`) – oferă **siguranță de tip (type safety)** și **claritate**.

### Enum cu atribute și metode
```java
public enum Suit {
    CLUBS(Color.BLACK), DIAMONDS(Color.RED), HEARTS(Color.RED), SPADES(Color.BLACK);

    private Color color;
    Suit(Color c) { this.color = c; }
    public Color getColor() { return color; }
}
```

➡️ Fiecare element este, de fapt, un **obiect** al clasei `Suit`.

### Alte operații utile
- `values()` – returnează un tablou cu toate valorile:
```java
for (Suit s : Suit.values()) {
    System.out.println(s);
}
```
- Poți compara valorile cu `==` sau folosi `switch`.

```java
Suit s = Suit.HEARTS;
switch (s) {
    case HEARTS -> System.out.println("Roșu");
    case CLUBS -> System.out.println("Negru");
}
```

---

## ✅ Recapitulare rapidă
| Concept | Ce face | Exemple | Observații |
|----------|----------|----------|-------------|
| **Moștenire** | Extinde o clasă de bază | `class Cerc extends Figura` | Relație "este un" (`is-a`) |
| **Clasă abstractă** | Nu se poate instanția; are metode abstracte | `abstract double aria();` | Șablon comun pentru subclase |
| **Interfață** | Definește un contract de metode | `interface Sunet` | Suportă moștenire multiplă |
| **Polimorfism** | Obiecte diferite, același comportament | `Figura f = new Cerc();` | Apelurile se rezolvă dinamic |
| **Clase imbricate** | Clase definite în alte clase | `Outer.Inner` | Organizează și protejează codul |
| **Enum** | Set de constante cu comportament | `enum Suit { CLUBS, HEARTS }` | Tip sigur, lizibil, extensibil |

---

## 📚 Ce urmează
În **Cursul 4** vei învăța despre **Generice** și **Colecții (List, Set, Map)** – instrumente care permit stocarea și manipularea eficientă a datelor.

---

📘 **Bibliografie recomandată:**  
Ken Arnold, James Gosling, David Holmes – *The Java™ Programming Language*, Ed. IV, Cap. 21.

