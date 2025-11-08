# 🧠 README – Curs 6: Stream API și Excepții în Java

## 🎯 Scopul cursului
Cursul 6 te învață două concepte fundamentale și des folosite în Java:
1. **Stream API** – modul modern și elegant de a lucra cu colecții de date (filtrare, sortare, transformare etc.).
2. **Tratarea Excepțiilor** – cum gestionezi erorile în mod controlat, pentru a evita blocarea aplicațiilor.

Toate exemplele sunt explicate pas cu pas, pentru a înțelege **ce face codul și de ce**.

---

## 🌊 1. Stream API – ce este și de ce e util
Un **Stream** este un flux de date (elemente) peste care poți aplica **operații funcționale** – fără să modifici colecția originală.

🧠 Gândește-l ca pe o bandă rulantă: iei elemente, le filtrezi, le transformi, le aduni... fără să atingi lista originală.

### ✅ Avantaje:
- Cod scurt și expresiv (prin **lambda**)
- Ușor de citit și întreținut
- Poți procesa date în **paralel** (`parallelStream()`)

### 🔹 Exemplu simplu:
```java
import java.util.*;
import java.util.stream.*;

public class ExempluStream {
    public static void main(String[] args) {
        List<Integer> numere = Arrays.asList(1, 2, 3, 4, 5, 6);

        // Filtrăm doar numerele pare și le dublăm
        List<Integer> rezultat = numere.stream()
                .filter(n -> n % 2 == 0) // păstrează doar numerele pare
                .map(n -> n * 2)         // înmulțește fiecare cu 2
                .collect(Collectors.toList()); // colectează rezultatul într-o listă

        System.out.println(rezultat); // [4, 8, 12]
    }
}
```
➡️ Observă că `stream()` creează fluxul, iar operațiile se aplică în lanț.

---

## 🧩 2. Tipuri de Stream-uri

| Tip | Ce face |
|-----|----------|
| `stream()` | flux secvențial (execută operațiile în ordine) |
| `parallelStream()` | flux paralel (procesează elementele pe mai multe thread-uri) |

```java
numere.parallelStream()
       .forEach(n -> System.out.println(n)); // se execută în paralel
```
> Atenție: `parallelStream()` accelerează procesarea doar pentru liste mari!

---

## ⚙️ 3. Operații intermediare (modifică fluxul)
Acestea **transformă** datele, dar nu produc un rezultat final. Se pot **înlănțui**.

| Operație | Descriere | Exemplu |
|-----------|------------|----------|
| `filter()` | păstrează doar elementele care îndeplinesc o condiție | `.filter(x -> x > 10)` |
| `map()` | transformă fiecare element | `.map(x -> x * x)` |
| `sorted()` | sortează elementele | `.sorted()` |
| `distinct()` | elimină duplicatele | `.distinct()` |
| `limit(n)` | păstrează primele `n` elemente | `.limit(5)` |
| `skip(n)` | sare peste primele `n` elemente | `.skip(2)` |

📘 **Exemplu:**
```java
List<String> nume = Arrays.asList("Ana", "Ion", "Ana", "Mihai", "Ioana");

nume.stream()
    .distinct()           // elimină duplicatele
    .sorted()             // sortează alfabetic
    .forEach(System.out::println);
```
Rezultat:
```
Ana
Ion
Ioana
Mihai
```

---

## 🏁 4. Operații terminale (produc un rezultat final)
| Operație | Descriere | Exemplu |
|-----------|------------|----------|
| `forEach()` | execută o acțiune pe fiecare element | `.forEach(System.out::println)` |
| `collect()` | adună elementele într-o colecție nouă | `.collect(Collectors.toList())` |
| `count()` | numără elementele | `.count()` |
| `reduce()` | combină toate elementele într-o singură valoare | `.reduce(0, (a,b)->a+b)` |
| `findFirst()` | returnează primul element | `.findFirst().get()` |

📘 **Exemplu:**
```java
List<Integer> lista = Arrays.asList(1, 2, 3, 4, 5);
int suma = lista.stream()
                .reduce(0, (a, b) -> a + b); // adună toate elementele
System.out.println(suma); // 15
```

---

## 💡 5. Exemplu complet – combinarea operațiilor
```java
List<String> cuvinte = Arrays.asList("ion", "andrei", "ana", "maria", "paul");

List<String> rezultat = cuvinte.stream()
        .filter(s -> s.length() > 3)     // păstrează doar cuvintele mai lungi de 3 litere
        .map(String::toUpperCase)        // le transformă în majuscule
        .sorted()                        // le sortează alfabetic
        .collect(Collectors.toList());   // le colectează într-o nouă listă

System.out.println(rezultat); // [ANDREI, MARIA, PAUL]
```
➡️ Observă lanțul logic: **filtrare → transformare → sortare → colectare.**

---

## ⚡ 6. Tratarea Excepțiilor în Java

### Ce este o excepție?
O **excepție** este o eroare apărută în timpul execuției (de exemplu, împărțire la zero, acces invalid la un fișier etc.).

Scopul sistemului de excepții este să **trateze erorile fără să oprească programul brusc.**

### 6.1. Tipuri de excepții
| Tip | Descriere | Exemple |
|------|------------|----------|
| **Checked** | verificate la compilare | `IOException`, `SQLException` |
| **Unchecked** | apar la rulare | `NullPointerException`, `ArithmeticException` |
| **Error** | erori grave (nu se tratează) | `OutOfMemoryError` |

---

### 6.2. Blocul `try-catch-finally`
```java
public class ExempleExceptii {
    public static void main(String[] args) {
        try {
            int x = 5 / 0; // provoacă excepție
        } catch (ArithmeticException e) {
            System.out.println("Eroare: împărțire la zero!");
        } finally {
            System.out.println("Blocul finally se execută mereu.");
        }
    }
}
```
📘 **Explicație:**
- `try` = cod care poate genera o excepție
- `catch` = cod care tratează excepția
- `finally` = se execută **indiferent** dacă apare sau nu o excepție (util pentru închidere fișiere, conexiuni etc.)

---

### 6.3. Aruncarea excepțiilor manual (`throw` și `throws`)
```java
public class Validare {
    static void verificaVarsta(int varsta) throws Exception {
        if (varsta < 18)
            throw new Exception("Prea mic pentru a continua!");
        System.out.println("Acces permis!");
    }

    public static void main(String[] args) {
        try {
            verificaVarsta(16);
        } catch (Exception e) {
            System.out.println(e.getMessage());
        }
    }
}
```
➡️ `throw` = aruncă o excepție nouă.  
➡️ `throws` = declară că metoda poate arunca o excepție (și altcineva trebuie s-o prindă).

---

## 🧠 7. Recapitulare rapidă
| Concept | Ce face | Exemplu |
|----------|----------|----------|
| `stream()` | Creează un flux de date | `lista.stream()` |
| `filter()` | Filtrează elementele | `.filter(x -> x>0)` |
| `map()` | Transformă fiecare element | `.map(x -> x*2)` |
| `collect()` | Adună rezultatele într-o colecție | `.collect(Collectors.toList())` |
| `try-catch-finally` | Tratează erorile în execuție | vezi exemplul de mai sus |
| `throw` / `throws` | Aruncă sau declară o excepție | `throw new Exception()` |

---

## 🚀 8. Sfaturi practice
- Folosește **Stream API** pentru filtrări, mapări și procesări funcționale simple.
- Evită buclele tradiționale acolo unde un stream e mai clar.
- Tratează întotdeauna excepțiile care pot apărea (citire fișiere, calcule, conexiuni).  
- Folosește `try-with-resources` pentru lucrul cu fișiere sau resurse externe – se închid automat.

```java
try (FileReader fr = new FileReader("fisier.txt")) {
    // cod de citire
} catch (IOException e) {
    e.printStackTrace();
}
```

---

📘 **Ce urmează:**
După acest curs, vei putea scrie aplicații sigure și clare, care procesează date eficient (prin **Stream API**) și tratează erorile corect (prin **excepții**).

