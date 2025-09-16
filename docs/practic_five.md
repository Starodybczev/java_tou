# 🧪 Практическая работа №5  
**Тема:** «Алхимия и зелья»  
📅 *27 октября 2025 → 7 ноября 2025*  

---

## 📌 Требования к реализации

1. **Класс `Hero`**  
   - Приватные поля.  
   - Конструктор, принимающий начальные значения.  
   - Геттеры и сеттеры.  
   - Проверки в сеттерах:  
     - значение не меньше `0`;  
     - значение не больше `100`.  

2. **Класс `Potion`**  
   - Конструктор для инициализации.  
   - Метод  
     ```java
     public void apply(Hero hero)
     ```  
     который:  
     - изменяет характеристики героя через геттеры и сеттеры;  
     - выводит сообщение о применении.  

3. **Класс `Main`**  
   - Создать героя с начальными характеристиками.  
   - Создать коллекцию зелий (`ArrayList<Potion>`).  
   - Добавить:  
     - «Зелье силы» (+5 strength)  
     - «Зелье разума» (+5 intelligence)  
     - «Эликсир мудрости» (+3 strength, +3 intelligence)  
     - «Зелье яда» (-10 strength, -5 intelligence)  
   - Пройтись циклом и применить все зелья к герою.  

---

## 🧑‍💻 Код программы

### 🔹 Hero.java
```java
public class Hero {

    private String name; // имя
    private int strength; // сила
    private int intelligence; // интеллект

    // конструктор
    public Hero(String name, int strength, int intelligence){
        this.name = name;
        this.strength = strength;
        this.intelligence = intelligence;
    }

    public String getName(){
        return name;
    }

    public int getStrength() {
        return strength;
    }

    public int getIntelligence() {
        return intelligence;
    }

    public void setStrength(int strength) {
        if (strength < 0) {
            this.strength = 0;
        } else if (strength > 100) {
            this.strength = 100;
        } else {
            this.strength = strength;
        }
    }

    public void setIntelligence(int intelligence) {
        if (intelligence < 0) {
            this.intelligence = 0;
        } else if (intelligence > 100) {
            this.intelligence = 100;
        } else {
            this.intelligence = intelligence;
        }
    }

    @Override
    public String toString(){
        return name + " [Сила: " + strength + ", Интеллект: " + intelligence + "]";
    }
}
```
---
### 🔹 Potion.java
```java
public class Potion{
    private String name;
    private int strengthEffect;
    private int intelligenceEffect;

    public Potion(String name, int strengthEffect, int intelligenceEffect){
        this.name = name;
        this.strengthEffect = strengthEffect;
        this.intelligenceEffect = intelligenceEffect;
    }

    public void apply(Hero hero){
        int newStrength = hero.getStrength() + strengthEffect;
        hero.setStrength(newStrength);

        int newIntelligence = hero.getIntelligence() + intelligenceEffect;
        hero.setIntelligence(newIntelligence);

        System.out.println(hero + " выпил " + this);
    }

    @Override
    public String toString() {
        return name + " (сила: " + strengthEffect + ", интеллект: " + intelligenceEffect + ")";
    }

}    
```
---
### 🔹 Main.java
```java
import java.util.ArrayList;
import java.util.Scanner;

public class Main {
     public static void main(String[] args){
        Scanner user = new Scanner(System.in);

        System.out.println("Введи имя героя");
        String name = user.nextLine();

        System.out.print("Введите силу героя (0–100): ");
        int strength = user.nextInt();

        System.out.print("Введите интеллект героя (0–100): ");
        int intelligence = user.nextInt();

        Hero hero = new Hero(name, strength, intelligence);
        System.out.println("\n🎉 Герой создан: " + hero);

        ArrayList<Potion> potions = new ArrayList<>();
        potions.add(new Potion("Зелье силы", 5, 0));
        potions.add(new Potion("Зелье разума", 0, 5));
        potions.add(new Potion("Эликсир мудрости", 3, 3));
        potions.add(new Potion("Зелье яда", -10, -5));

        for(Potion potion : potions){
            potion.apply(hero);
        }

        System.out.println("\n🎯 Итоговые характеристики: " + hero);

        user.close();
    }    
}

```
