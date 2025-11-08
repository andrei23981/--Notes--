# 🧠 README – Curs 4: Generice și Colecții în Java (explicații clare și interactive)

## 🎯 Scopul cursului
În acest curs vei învăța două concepte esențiale din Java:

1. **Genericele** – cum să lucrezi cu tipuri parametrizate (de exemplu, liste care conțin doar un anumit tip de obiecte), pentru siguranță și claritate în cod.
2. **Colecțiile** – structuri de date gata făcute (liste, mulțimi, dicționare) care te ajută să stochezi și să manipulezi informații ușor.

Vom explica totul **clar**, cu **exemple de cod** și **comentarii explicative**.

---

## 🧩 1. Generice – conceptul de bază

### Ce sunt genericele?
Genericele îți permit să scrii cod care funcționează pentru **orice tip de date**, dar **cu siguranță la compilare**. Adică nu mai trebuie să convertești ("cast") tipuri de fiecare dată.

📦 **Fără generice:**
```java
List lista = new ArrayList(); // listă fără tip
lista.add("Salut");
lista.add(10); // merge, dar poate da probleme mai târziu
String text = (String) lista.get(0); // trebuie conversie manuală
```
❗ Aici poți adăuga orice tip de date și obții erori abia la rulare.

📦 **Cu generice:**
```java
List<String> lista = new ArrayList<>(); // listă de String-uri
lista.add("Salut");
// lista.add(10); // ❌ eroare la compilare
String text = lista.get(0); // fără conversie
```
➡️ Avantaj: **siguranță și claritate** – știi exact ce tip de date conține colecția.

---

### Cum definim o clasă generică?
```java
public class Cutie<T> { // T este un tip generic (poate fi orice)
    private T continut;

    public void pune(T ceva) {
        continut = ceva;
    }

    public T scoate() {
        return continut;
    }
}

public class Main {
    public static void main(String[] args) {
        Cutie<String> c1 = new Cutie<>();
        c1.pune("Cuvânt");
        System.out.println(c1.scoate()); // → Cuvânt

        Cutie<Integer> c2 = new Cutie<>();
        c2.pune(123);
        System.out.println(c2.scoate()); // → 123
    }
}
```
🧠 `T` e un tip generic – poate deveni `String`, `Integer`, `Student`, orice.

---

### Tipuri generice restricționate (Bounded Types)
Dacă vrei să permiți doar anumite tipuri:
```java
class Numarator<T extends Number> { // doar clase care extind Number
    private T valoare;
}
```
Acum `T` poate fi doar `Integer`, `Double`, `Float` etc.

---

## 🧱 2. Colecțiile în Java

### Ce sunt colecțiile?
Colecțiile sunt **structuri de date gata făcute**, care pot stoca mai multe elemente de același tip (liste, mulțimi, dicționare).

✅ Avantaje:
- Stochezi și gestionezi datele eficient.
- Nu trebuie să scrii manual structuri precum tablouri dinamice.
- Ai metode gata făcute pentru adăugare, ștergere, căutare, sortare etc.

### Ierarhia principală
```
Collection
├── List (ordine, duplicate permise)
├── Set (fără duplicate)
└── Map (perechi cheie-valoare)
```

> Lucrează mereu cu interfețele (`List`, `Set`, `Map`), nu direct cu clasele.

```java
List<String> lista = new ArrayList<>();
Set<Integer> multime = new HashSet<>();
Map<String, Integer> dictionar = new HashMap<>();
```

---

## 📜 3. `List` – liste ordonate (cu poziții și duplicate)

`List` este o colecție **ordonată**, care permite **elemente duplicate** și **acces prin index**.

### Exemple de implementări
#### 🔹 ArrayList – bazată pe tablou (rapidă la citire, mai lentă la inserare în mijloc)
```java
import java.util.*;

List<String> fructe = new ArrayList<>();
fructe.add("Măr");
fructe.add("Pere");
fructe.add("Măr"); // duplicate permis

System.out.println(fructe.get(1)); // accesează al doilea element
System.out.println(fructe); // [Măr, Pere, Măr]
```

#### 🔹 LinkedList – bazată pe noduri legate (rapidă la adăugare/ștergere, mai lentă la acces)
```java
List<String> animale = new LinkedList<>();
animale.add("Pisică");
animale.add("Câine");
animale.remove(0); // elimină primul element
System.out.println(animale); // [Câine]
```

#### 🔹 Vector – vechea variantă sincronizată (rulaje multi-thread)
```java
Vector<Integer> v = new Vector<>();
v.add(10);
v.add(20);
System.out.println(v); // [10, 20]
```
> Folosit rar azi, doar dacă ai nevoie de sincronizare implicită.

---

## 🔢 4. `Set` – colecții fără duplicate

Un `Set` **nu permite valori duplicate** și **nu are index**.

### 🔹 HashSet – rapid, dar nu păstrează ordinea
```java
Set<String> culori = new HashSet<>();
culori.add("Roșu");
culori.add("Verde");
culori.add("Roșu"); // ignorat, deja există
System.out.println(culori); // [Roșu, Verde] (ordinea e aleatorie)
```

### 🔹 TreeSet – păstrează elementele ordonate automat
```java
Set<Integer> numere = new TreeSet<>();
numere.add(5);
numere.add(1);
numere.add(3);
System.out.println(numere); // [1, 3, 5]
```

> Pentru `TreeSet`, elementele trebuie să fie comparabile (adică să implementeze `Comparable`) sau să oferi un `Comparator`.

---

## 🗺️ 5. `Map` – perechi cheie → valoare

Un `Map` leagă o **cheie unică** de o **valoare**. Gândește-l ca un dicționar.

### 🔹 HashMap – rapid, dar fără ordine
```java
Map<String, Integer> note = new HashMap<>();
note.put("Andrei", 10);
note.put("Maria", 9);
note.put("Andrei", 8); // suprascrie valoarea pentru cheia Andrei

System.out.println(note.get("Andrei")); // 8
System.out.println(note); // {Maria=9, Andrei=8}
```

### 🔹 TreeMap – păstrează cheile ordonate alfabetic / natural
```java
Map<String, Integer> persoane = new TreeMap<>();
persoane.put("Zoe", 20);
persoane.put("Ana", 18);
System.out.println(persoane); // {Ana=18, Zoe=20}
```

### 🔹 Cum parcurgem un `Map`
```java
for (Map.Entry<String, Integer> entry : note.entrySet()) {
    System.out.println(entry.getKey() + " are nota " + entry.getValue());
}
```

---

## ⚖️ 6. Diferențe rapide între principalele colecții

| Tip | Permite duplicate | Ordine | Acces prin index | Implementări comune |
|------|--------------------|---------|------------------|----------------------|
| List | ✅ Da | ✅ Da | ✅ Da | ArrayList, LinkedList |
| Set | ❌ Nu | ⚠️ Doar TreeSet | ❌ Nu | HashSet, TreeSet |
| Map | Cheile: ❌ / Valorile: ✅ | ⚠️ Doar TreeMap | ❌ Nu | HashMap, TreeMap |

---

## 🧠 7. Ce trebuie să ții minte
- **Genericele** previn erorile de tip și fac codul clar.
- **List** – ordonată, permite duplicate, are index.
- **Set** – fără duplicate, uneori ordonat (`TreeSet`).
- **Map** – cheie → valoare, cheia e unică.
- `Hash*` – rapid, fără ordine.  
  `Tree*` – ordonat, dar puțin mai lent.  
- Folosește mereu **interfețele (`List`, `Set`, `Map`)**, nu clasele direct.

---

## 🚀 8. Ce urmează
În **Cursul 5**, vei învăța despre **Comparator**, **Lambda expressions**, **sortarea colecțiilor** și **colecțiile imutabile** (`List.of`, `Set.of`, `Map.of`).

