# 🧠 README – Cursurile 7–8: Clase interne anonime, Optional, I/O, Serializare și Tratarea Fișierelor

## 🎯 Scopul cursului
Cursurile 7–8 explică **partea practică a lucrului cu fișiere și fluxuri de date în Java**, dar și **mecanisme moderne de siguranță**: clase interne anonime, `Optional`, tratarea excepțiilor în I/O și serializarea obiectelor.

Totul este explicat **clar, pe înțelesul tău**, cu exemple concrete de cod și comentarii pas cu pas.

---

## 🧩 1. Clase interne anonime – recapitulare
O **clasă internă anonimă** este o clasă **fără nume**, creată direct în momentul folosirii.

Sunt utile când vrei **o implementare rapidă** a unei interfețe sau a unei clase abstracte, fără să mai declari o clasă separată.

### Exemplu:
```java
interface Salut {
    void spuneSalut();
}

public class Main {
    public static void main(String[] args) {
        // Clasă anonimă care implementează interfața Salut
        Salut s = new Salut() {
            public void spuneSalut() {
                System.out.println("Salut anonim!");
            }
        };
        s.spuneSalut(); // → Salut anonim!
    }
}
```
➡️ Aceasta este o **formă veche** de a scrie cod concis. Azi, se preferă **expresiile Lambda**, care fac același lucru mai elegant.

---

## ⚡ 2. Clasa `Optional` – prevenirea erorilor `NullPointerException`
`Optional` este un container care **poate sau nu** să conțină o valoare.

Scopul său este să te ajute să eviți verificările de tipul `if (x != null)`.

### Exemplu:
```java
import java.util.Optional;

public class ExempluOptional {
    public static void main(String[] args) {
        Optional<String> nume = Optional.of("Andrei");
        System.out.println(nume.get()); // → Andrei

        Optional<String> gol = Optional.empty();
        System.out.println(gol.orElse("Nume implicit")); // → Nume implicit
    }
}
```
📘 Metode utile:
| Metodă | Ce face |
|--------|----------|
| `of(x)` | creează un Optional cu o valoare |
| `empty()` | creează un Optional gol |
| `get()` | returnează valoarea (sau aruncă excepție dacă e gol) |
| `orElse(val)` | returnează valoarea sau un default dacă e gol |
| `isPresent()` | verifică dacă există o valoare |

---

## 📁 3. Introducere în Input/Output (I/O)

I/O = **Input/Output**, adică lucrul cu **fișiere**, **tastatura**, **consola**, **rețele**, etc.

Java folosește **fluxuri (streams)** pentru a citi și scrie date.

### Tipuri de fluxuri:
| Tip | Descriere | Exemple |
|------|------------|----------|
| **Fluxuri de caractere** | lucrează cu text (`char`) | `FileReader`, `FileWriter` |
| **Fluxuri de octeți** | lucrează cu date binare | `FileInputStream`, `FileOutputStream` |

---

## 🧾 4. Citirea și scrierea fișierelor text
### Citire cu `FileReader` și `BufferedReader`
```java
import java.io.*;

public class CitireFisier {
    public static void main(String[] args) {
        try (BufferedReader br = new BufferedReader(new FileReader("text.txt"))) {
            String linie;
            while ((linie = br.readLine()) != null) {
                System.out.println(linie);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
➡️ `BufferedReader` este mai eficient decât `FileReader`, pentru că citește „în blocuri” (nu caracter cu caracter).

### Scriere cu `FileWriter` și `BufferedWriter`
```java
import java.io.*;

public class ScriereFisier {
    public static void main(String[] args) {
        try (BufferedWriter bw = new BufferedWriter(new FileWriter("iesire.txt"))) {
            bw.write("Salut lume!");
            bw.newLine();
            bw.write("Aceasta este o altă linie.");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
🧠 **`try-with-resources`** închide automat fișierul după folosire.

---

## 💾 5. Fișiere binare (stream-uri de octeți)
Pentru fișiere care conțin **imagini, sunete sau date binare**, se folosesc clasele `FileInputStream` și `FileOutputStream`.

```java
import java.io.*;

public class CopiereFisierBin {
    public static void main(String[] args) {
        try (FileInputStream fis = new FileInputStream("input.png");
             FileOutputStream fos = new FileOutputStream("copie.png")) {

            int byteData;
            while ((byteData = fis.read()) != -1) {
                fos.write(byteData);
            }
            System.out.println("Copiere completă!");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
📘 Aici se citește **fiecare octet** și se scrie în alt fișier.

---

## 🧱 6. Serializare și Deserializare
**Serializarea** = procesul prin care un obiect este transformat într-un flux de octeți pentru a fi salvat pe disc sau transmis prin rețea.  
**Deserializarea** = procesul invers.

### Exemplu:
```java
import java.io.*;

class Student implements Serializable {
    String nume;
    int varsta;

    public Student(String nume, int varsta) {
        this.nume = nume;
        this.varsta = varsta;
    }
}

public class SerializareExemplu {
    public static void main(String[] args) {
        // SERIALIZARE
        try (ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream("student.ser"))) {
            Student s = new Student("Andrei", 21);
            out.writeObject(s);
            System.out.println("Obiect salvat!");
        } catch (IOException e) {
            e.printStackTrace();
        }

        // DESERIALIZARE
        try (ObjectInputStream in = new ObjectInputStream(new FileInputStream("student.ser"))) {
            Student s = (Student) in.readObject();
            System.out.println("Obiect citit: " + s.nume + ", " + s.varsta);
        } catch (IOException | ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}
```
➡️ Pentru a putea fi serializat, o clasă trebuie să implementeze `Serializable`.

> 🔸 Evită serializarea câmpurilor sensibile (folosește `transient`).

---

## ⚙️ 7. Tratarea excepțiilor în I/O
În operațiile cu fișiere, pot apărea multe excepții (`FileNotFoundException`, `IOException`).

### Folosește `try-with-resources`:
```java
try (FileReader fr = new FileReader("fisier.txt")) {
    int c;
    while ((c = fr.read()) != -1)
        System.out.print((char) c);
} catch (IOException e) {
    System.out.println("Eroare la citirea fișierului!");
}
```
🧠 Avantaj: fișierul se închide automat chiar dacă apare o eroare.

---

## 🧠 8. Recapitulare rapidă
| Concept | Ce face | Clase/Metode importante |
|----------|----------|--------------------------|
| Clasă anonimă | Implementare rapidă fără nume | `new Interfață() { ... }` |
| `Optional` | Evită erorile `null` | `Optional.of()`, `orElse()` |
| Fluxuri text | Lucrează cu caractere | `FileReader`, `BufferedReader` |
| Fluxuri binare | Lucrează cu octeți | `FileInputStream`, `FileOutputStream` |
| Serializare | Salvează obiecte pe disc | `ObjectOutputStream`, `Serializable` |
| Deserializare | Reface obiectele din fișiere | `ObjectInputStream` |

---

## 🚀 9. Sfaturi practice
- Folosește mereu **`try-with-resources`** pentru a evita scurgerile de resurse.
- Folosește `BufferedReader` și `BufferedWriter` pentru fișiere text mari – sunt mai rapide.
- Evită folosirea `get()` la `Optional` fără verificare – folosește `orElse()` sau `ifPresent()`.
- Nu serializa informații sensibile (ex: parole).

---

📘 **Ce urmează:**
După aceste cursuri, vei fi pregătit să lucrezi cu **date persistente**, **fișiere** și **obiecte complexe**, construind aplicații reale care salvează și recuperează informații eficient și sigur.