# 🎮 Java Practic Six

Учебный проект на **Java** с демонстрацией принципов ООП: 🏭 фабрики, 🎭 стратегии и 👀 наблюдатели.

## 📂 Структура проекта

- `Game.java` – 🎯 точка входа в игру
- `characters/` – 🧙‍♂️ персонажи игры (Archer, Mage, Warrior)
- `fabric/` – 🏭 фабрики для создания персонажей
- `interfaces/` – 📑 интерфейсы (стратегии и наблюдатели)
- `observer/` – 📜 наблюдатели (лог боя)
- `strategy/` – ⚔️ стратегии атаки (Bow, Magic, Melee)

## 📝 Пояснения

- `Character` – абстрактный класс, описывающий общие свойства персонажей (имя, здоровье, стратегия атаки).
- `Archer`, `Mage`, `Warrior` – конкретные классы героев, наследуются от `Character` и задают базовые параметры.
- `AttackStrategy` – интерфейс для разных типов атаки (стрельба из лука, магия, ближний бой).
- `GameObserver` – интерфейс для подписчиков, которые реагируют на события в игре.
- `BattleLog` – реализация наблюдателя, выводит ход сражения в консоль.
- `CharacterFactory` и её наследники (`ArcherFactory`, `MageFactory`, `WarriorFactory`) – реализуют паттерн **Factory**, создавая персонажей.
- `Game` – управляет логикой боя: создаёт персонажей, чередует атаки и уведомляет наблюдателей.

## 🚀 Как запустить

```bash
javac Game.java
java Game
```

---
## 📜 Код проекта

### `Game.java`
```java

import characters.*;
import fabric.*;
import java.util.Random;
import java.util.Scanner;
import observer.BattleLog;

public class Game{
     public static void main(String[] args){
        CharacterFactory factory;
        Scanner user = new Scanner(System.in);

        System.out.println("Выберите класс персонажа: 1 - Воин, 2 - Маг, 3 - Лучник");
        int choice = user.nextInt();

        System.out.print("Введите имя героя: ");
        String name = user.nextLine();

        
        switch(choice){
            case 1: 
                factory = new WarriorFactory();
            case 2: 
                factory = new MageFactory();
            case 3:
                factory = new ArcherFactory();
            default:{
                System.out.println("Неверный выбор. По умолчанию выбран Воин.");
                factory = new WarriorFactory();
            }            
        }
        characters.Character player = factory.createCharacter(name);
        characters.Character enemy = new Warrior("Орк");

        BattleLog log = new BattleLog();
        player.addObserver(log);
        enemy.addObserver(log);

        System.out.println("Бой начинается между " + player.getName() + " и " + enemy.getName());

        Random rand = new Random();
        while (player.isAlive() && enemy.isAlive()) {
            if (rand.nextBoolean()) {
                player.performAttack(enemy);
            } else {
                enemy.performAttack(player);
            }
            System.out.println(player.getName() + " HP: " + player.getHealth());
            System.out.println(enemy.getName() + " HP: " + enemy.getHealth());
            System.out.println("----------------------------");
        }

        if (player.isAlive()) {
            System.out.println(player.getName() + " победил!");
        } else {
            System.out.println(enemy.getName() + " победил!");
        }
    }

}

```

### `characters/Archer.java`
```java
package characters;

import strategy.BowAttack;

public class Archer extends Character{
    public Archer(String name){
        super(name, 90, new BowAttack());
    }
}

```

### `characters/Character.java`
```java
package characters;

import interfaces.AttackStrategy;
import interfaces.GameObserver;
import java.util.ArrayList;
import java.util.List;

public abstract class Character {
    protected String name;
    protected int health;
    protected AttackStrategy attackStrategy;
    private List<GameObserver> observers = new ArrayList<>();


    public Character(String name, int health, AttackStrategy attackStrategy){
        this.name = name;
        this.health = health;
        this.attackStrategy = attackStrategy;
    }

    public String getName(){
        return name;
    }

    public int getHealth(){
        return health;
    }




   public void performAttack(Character enemy) {
        if (attackStrategy != null && this.isAlive()) {
            attackStrategy.attack(this, enemy);
        } else {
            notifyObservers(name + " не может атаковать!");
        }
    }

    public void takeDamage(int damage) {
        health -= damage;
        notifyObservers(name + " получил " + damage + " урона. Здоровье: " + health);
        if (health <= 0) {
            notifyObservers(name + " пал в бою!");
        }
    }

    public void addObserver(GameObserver observer) {
        observers.add(observer);
    }

    public void notifyObservers(String event) {
        for (GameObserver obs : observers) {
            obs.onEvent(event);
        }
    }

    public boolean isAlive() {
        return health > 0;
    }
}

```

### `characters/CharacterFactory.java`
```java
package characters;

public abstract  class CharacterFactory {
    public abstract Character createCharacter(String name);
}

```

### `characters/Mage.java`
```java
package characters;

import strategy.MagicAttack;

public class Mage extends Character {
    public Mage(String name){
        super(name, 90, new MagicAttack());
    }
}

```

### `characters/Warrior.java`
```java
package characters;
import strategy.MeleeAttack;

public class Warrior extends Character {
    public Warrior(String name) {
        super(name, 100, new MeleeAttack());
    }
}

```

### `fabric/ArcherFactory.java`
```java
package fabric;

import characters.Archer;
import characters.Character;
import characters.CharacterFactory;

public class ArcherFactory extends CharacterFactory {
    @Override
    public Character createCharacter(String name){
        return new Archer(name);

    }
}

```

### `fabric/MageFactory.java`
```java
package fabric;

import characters.Character;
import characters.CharacterFactory;
import characters.Mage;

public class MageFactory extends CharacterFactory {
    @Override
    public Character createCharacter(String name){
        return new Mage(name);
    }
}

```

### `fabric/WarriorFactory.java`
```java
package fabric;

import characters.CharacterFactory;
import characters.Character;
import characters.Warrior;

public class WarriorFactory extends CharacterFactory {
    @Override
    public Character createCharacter(String name) {
        return new Warrior(name);
    }
}

```

### `interfaces/AttackStrategy.java`
```java
package interfaces;
import characters.Character;

public interface AttackStrategy {
    void attack(Character attacker, Character enemy);
}

```

### `interfaces/GameObserver.java`
```java
package interfaces;

public interface  GameObserver {
    void onEvent(String event);
}

```

### `observer/BattleLog.java`
```java
package observer;

import interfaces.GameObserver;

public class BattleLog implements GameObserver{
    @Override
    public void onEvent(String event){
        System.out.println("[LOG] " + event);
    }
}

```

### `strategy/BowAttack.java`
```java
package strategy;

import characters.Character;
import interfaces.AttackStrategy;

public class BowAttack implements AttackStrategy{
    @Override
    public void attack(Character attacker, Character target){
        int baseDamage = 8;
        int poisonDamage = 5;


        target.takeDamage(baseDamage + poisonDamage);
        attacker.notifyObservers(attacker.getName() + " отравил врага! Урон: " + (baseDamage + poisonDamage));
    }
}

```

### `strategy/MagicAttack.java`
```java
package strategy;
import characters.Character;
import interfaces.AttackStrategy;

public class MagicAttack implements AttackStrategy {
    @Override
    public void attack(Character attacker, Character target) {
       int damage = 20;
       int selfDamage = 5;

       target.takeDamage(damage);
       attacker.takeDamage(selfDamage);
       attacker.notifyObservers(attacker.getName() + " использовал магию: враг -"  + damage + " HP, сам -" + selfDamage + " HP");
    }
}

```

### `strategy/MeleeAttack.java`
```java
package strategy;
import characters.Character;
import interfaces.AttackStrategy;

public class MeleeAttack implements AttackStrategy {

	@Override
	public void attack(Character attacker, Character target) {
		int damage = 15;
        target.takeDamage(damage);
       attacker.notifyObservers(attacker.getName() + " ударил врага мечом на " + damage + " урона!");
	}
}

```

