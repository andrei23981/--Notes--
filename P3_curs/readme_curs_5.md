# 🧠 README – Curs 5: Colecții Avansate, Comparatori, Lambda și Colecții Imutabile

## 🎯 Scopul cursului
În acest curs vei învăța cum să:
- sortezi colecțiile și să controlezi ordinea elementelor;
- folosești comparatori (`Comparable` și `Comparator`);
- scrii cod mai curat cu **expresii Lambda**;
- creezi **colecții imutabile** (`List.of`, `Set.of`, `Map.of`);
- folosești clasa **`Collections`** și **`Arrays`** pentru operații comune.

Toate exemplele sunt explicate pas cu pas, pentru ca tu să înțelegi clar cum funcționează codul.

---

## 🧩 1. Clasa `Collections` – metode utile
Clasa `Collections` oferă metode statice pentru a lucra cu colecțiile (liste, seturi etc.) fără a scrie tu cod suplimentar.

### Exemple de bază:
```java
import java.util.*;

public class ExempleCollections {
    public static void main(String[] args) {
        List<Integer> numere = new ArrayList<>(Arrays.asList(5, 2, 9, 1, 3));

        Collections.sort(numere); // sortează lista crescător
        System.out.println("Sortare crescătoare: " + numere); // [1, 2, 3, 5, 9]

        Collections.reverse(numere); // inversează ordinea
        System.out.println("Sortare descrescătoare: " + numere); // [9, 5, 3, 2, 1]

        System.out.println("Valoarea maximă: " + Collections.max(numere)); // 9
        System.out.println("Valoarea minimă: " + Collections.min(numere)); // 1
    }
}
```

📘 Alte metode utile:
| Metodă | Ce face |
|--------|----------|
| `Collections.shuffle(lista)` | amestecă elementele aleatoriu |
| `Collections.frequency(lista, elem)` | numără aparițiile unui element |
| `Collections.copy(dest, src)` | copiază elementele între liste |
| `Collections.fill(lista, val)` | înlocuiește toate elementele cu aceeași valoare |

---

## 🧱 2. Interfața `Comparable` – ordinea naturală
Când vrei ca obiectele tale să aibă o ordine „naturală” (de exemplu, crescător după nume sau vârstă), implementezi interfața `Comparable`.

### Exemplu:
```java
class Student implements Comparable<Student> {
    String nume;
    int varsta;

    public Student(String nume, int varsta) {
        this.nume = nume;
        this.varsta = varsta;
    }

    @Override
    public int compareTo(Student alt) {
        return this.varsta - alt.varsta; // ordonează crescător după vârstă
    }

    @Override
    public String toString() {
        return nume + " (" + varsta + ")";
    }
}

public class TestComparable {
    public static void main(String[] args) {
        List<Student> studenti = new ArrayList<>();
        studenti.add(new Student("Andrei", 22));
        studenti.add(new Student("Maria", 20));
        studenti.add(new Student("Ioana", 23));

        Collections.sort(studenti); // folosește compareTo()
        System.out.println(studenti); // [Maria (20), Andrei (22), Ioana (23)]
    }
}
```
🧠 `Comparable` definește ordinea implicită a unei clase.

---

## ⚙️ 3. Interfața `Comparator` – ordini personalizate
Dacă vrei mai multe moduri de sortare (de exemplu, după nume, apoi după vârstă), folosește `Comparator`.

### Exemplu simplu:
```java
import java.util.*;

public class TestComparator {
    public static void main(String[] args) {
        List<String> cuvinte = Arrays.asList("ana", "ion", "alexandru");

        // Comparator care ordonează după lungime
        Comparator<String> dupaLungime = (a, b) -> a.length() - b.length();

        Collections.sort(cuvinte, dupaLungime);
        System.out.println(cuvinte); // [ion, ana, alexandru]
    }
}
```

### Comparatori compuși:
```java
Comparator<Student> dupaVarsta = Comparator.comparingInt(s -> s.varsta);
Comparator<Student> dupaNume = Comparator.comparing(s -> s.nume);
Comparator<Student> compus = dupaVarsta.thenComparing(dupaNume);
```
➡️ `thenComparing()` = dacă doi studenți au aceeași vârstă, se sortează după nume.

---

## ⚡ 4. Expresii Lambda – cod scurt și elegant
Expresiile **Lambda** sunt funcții scurte, fără nume. Ele simplifică mult codul atunci când ai interfețe funcționale (cu o singură metodă abstractă).

### Exemplu clasic:
```java
List<Integer> numere = Arrays.asList(5, 2, 8, 1);

// Sortare descrescătoare cu expresie lambda
numere.sort((a, b) -> b - a);
System.out.println(numere); // [8, 5, 2, 1]
```

### Alt exemplu: filtrare rapidă cu lambda
```java
numere.stream()
      .filter(n -> n % 2 == 0) // păstrează doar numerele pare
      .forEach(System.out::println); // afișează fiecare element
```
🧠 `System.out::println` = referință la metoda `println`.

---

## 🧮 5. Colecții Imutabile (`List.of`, `Set.of`, `Map.of`)
Colecțiile imutabile **nu pot fi modificate** după crearea lor.

```java
List<String> zile = List.of("Luni", "Marți", "Miercuri");
System.out.println(zile);
// zile.add("Joi"); // ❌ Eroare: colecția este imutabilă

Set<Integer> numere = Set.of(1, 2, 3);
Map<String, Integer> varste = Map.of("Andrei", 21, "Maria", 22);
```
Avantaje:
- Siguranță (nu se pot modifica accidental)
- Performanță (Java optimizează astfel de colecții)

---

## 🧠 6. Ce trebuie să reții
| Concept | Ce face | Exemplu | Observație |
|----------|----------|----------|-------------|
| `Collections` | oferă metode statice pentru colecții | `Collections.sort()` | lucrează cu colecții existente |
| `Comparable` | definește ordinea naturală | `compareTo()` | ordinea implicită a clasei |
| `Comparator` | definește ordini personalizate | `(a,b)->a-b` | flexibil, combinabil |
| Lambda | cod scurt și expresiv | `(a,b)->a+b` | funcții anonime |
| Colecții imutabile | nu pot fi modificate | `List.of(1,2,3)` | utile pentru date fixe |

---

## 🚀 7. Ce urmează
În **Cursul 6**, vei explora **Stream API**: modul modern de a prelucra colecțiile folosind lanțuri de operații (filtrare, mapare, reducere), combinat cu expresii Lambda.

---

📘 **Sfat final:** Folosește `Comparator.comparing()` și `List.of()` cât mai des — sunt moderne, clare și fac codul tău mai elegant.

