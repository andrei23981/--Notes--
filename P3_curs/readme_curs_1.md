# 🧠 README - Curs 1: Introducere în limbajul Java

## 🎯 Obiectivul cursului
Acest prim curs te ajută să înțelegi **bazele limbajului Java**, cum este organizat un program, cum se compilează și rulează codul, și care sunt conceptele fundamentale din programarea orientată pe obiecte (OOP).

---

## 🏗️ 1. Ce este Java?
- Java este un **limbaj de programare orientat pe obiecte**, portabil și multiplatformă (rulează pe orice sistem care are JVM).
- Este folosit pentru aplicații desktop, web, mobile și enterprise.

**Avantaje:**
- Portabilitate (write once, run anywhere)
- Securitate și stabilitate
- Ecosistem bogat de biblioteci și framework-uri

**Versiuni Java:**
- **J2SE** – aplicații standard (desktop)
- **J2EE** – aplicații enterprise (web, baze de date, servicii)
- **J2ME** – aplicații mobile (mai vechi)

---

## 🧩 2. Structura unui program Java
Un program Java este format **doar din clase**. Totul trebuie să fie în interiorul unei clase.

```java
public class Exemplu {
    public static void main(String[] args) {
        System.out.println("Hello World!");
    }
}
```

**Explicație:**
- `public class Exemplu` – definește o clasă publică numită `Exemplu`
- `public static void main(String[] args)` – punctul de pornire al aplicației
- `System.out.println(...)` – afișează text în consolă

**Compilare și rulare:**
```bash
javac Exemplu.java   # compilează codul (creează Exemplu.class)
java Exemplu         # rulează programul
```

🧠 **De reținut:**
- Numele fișierului trebuie să fie identic cu numele clasei publice.
- Metoda `main()` nu se poate modifica.

---

## 📦 3. Pachete (Packages)
Pachetele grupează clasele în mod logic și evită conflictele de nume.

```java
package ro.uvt.p3;
import java.util.Random;
```

- **`package`** – definește pachetul în care se află clasa
- **`import`** – permite utilizarea altor clase fără a scrie calea completă

📘 **Exemplu:**
```java
import java.util.*; // importă toate clasele din pachetul java.util
```

🔹 **Bine de știut:** pachetul implicit (fără `package`) este fără nume, dar nu este recomandat pentru proiecte mari.

---

## 📁 4. Fișiere JAR (Java ARchive)
- Reprezintă arhive comprimate care conțin mai multe clase (`.class`), imagini, sau alte resurse.
- Pot fi **executabile**, dacă includ un fișier `MANIFEST.MF` cu linia:
  ```text
  Main-Class: NumeClasaPrincipala
  ```
- Se rulează cu:
  ```bash
  java -jar nume_fisier.jar
  ```

---

## 🧱 5. Standarde de scriere a codului Java
Respectarea convențiilor face codul clar și ușor de citit.

| Tip element | Convenție | Exemplu |
|--------------|------------|----------|
| Pachete | litere mici | `ro.uvt.p3` |
| Clase | PascalCase | `StudentInfo` |
| Metode | camelCase | `getName()` |
| Variabile | camelCase | `totalSum` |
| Constante | MAJUSCULE | `MAX_SIZE` |

---

## 🔑 6. Cuvinte cheie importante
Câteva exemple din cele mai folosite:

- **Tipuri de date:** `int`, `float`, `double`, `boolean`, `char`
- **Control:** `if`, `for`, `while`, `switch`, `break`, `continue`
- **Definire clase/metode:** `class`, `interface`, `extends`, `implements`
- **Acces:** `public`, `private`, `protected`
- **Altele:** `this`, `super`, `static`, `final`, `new`, `return`

---

## 💻 7. Aplicație practică
Creează o clasă Java care calculează aria unui dreptunghi:

```java
public class Dreptunghi {
    public static void main(String[] args) {
        int latime = 5;
        int inaltime = 10;
        int aria = latime * inaltime;
        System.out.println("Aria este: " + aria);
    }
}
```

📤 **Extinde:** încearcă să ceri valorile de la tastatură folosind `Scanner`.

---

## 📚 8. Idei principale de reținut
✅ Java este **orientat pe obiecte** și **independent de platformă**.  
✅ Toate metodele și variabilele trebuie să fie într-o **clasă**.  
✅ Metoda `main()` este punctul de pornire al oricărei aplicații.  
✅ Respectă convențiile de denumire pentru un cod clar.  
✅ Pachetele și JAR-urile sunt modul de organizare și distribuție a aplicațiilor.

---

## 🧩 9. Exercițiu interactiv de gândire
> Ce s-ar întâmpla dacă încerci să rulezi o clasă Java fără metoda `main()`?  
💬 Răspuns: JVM nu va ști de unde să înceapă execuția și va arunca o eroare `Main method not found in class ...`.

---

## 🚀 10. Ce urmează
Următorul curs (Curs 2) introduce **clasele, obiectele și testarea unitară**, unde vei învăța cum se definesc structuri de date proprii și cum se scrie cod reutilizabil.

---

📘 **Recomandare:**
- Documentația Oracle: [https://docs.oracle.com/javase/tutorial/](https://docs.oracle.com/javase/tutorial/)
- IDE recomandate: IntelliJ IDEA, Eclipse, sau VSCode cu extensia Java.

