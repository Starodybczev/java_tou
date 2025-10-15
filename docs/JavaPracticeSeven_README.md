# 🎮 Java Practice Seven
Учебный проект на Java, демонстрирующий применение ключевых **паттернов проектирования**:  
🏭 **Factory Method**, 🎭 **Strategy**, 🧩 **Template Method**, 💎 **Decorator**, 👀 **Observer** и ⚠️ **Custom Exceptions**.

---

## 📂 Структура проекта

```
src/
 ├─ AbstractClass/        – 🧱 базовые абстрактные классы (Hero, Monster)
 ├─ Exceptions/           – ⚠️ собственные исключения (LowHealthException, DragonFireException)
 ├─ Factories/            – 🏭 фабрики для создания героев и монстров
 ├─ Heros/                – 🧙‍♂️ конкретные герои (Warrior, Mage, Archer)
 ├─ Interfaces/           – 📑 интерфейсы (AttackStrategy)
 ├─ Items/                – 🎒 предметы и их декораторы (Potion, GreaterPotion)
 ├─ Monsters/             – 👹 противники (Goblin, Skeleton, Dragon)
 ├─ Strategy/             – ⚔️ стратегии атаки (Melee, Magic, Ranged)
 ├─ Utils/                – 🧠 шаблон боя (BattleTemplate)
 └─ Main.java             – 🎯 точка входа в игру
```

---

## 🧠 Используемые паттерны

### 🏭 Factory Method / Abstract Factory
**Классы:**  
`HeroFactory` — создаёт героев разных типов (Warrior, Mage, Archer)  
`MonsterFactory` — создаёт монстров, включая случайного врага  

**Пример:**
```java
Hero hero = HeroFactory.createHero("mage");
Monster monster = MonsterFactory.createRandomMonster();
```

---

### 🎭 Strategy + 🧩 Template Method
- `AttackStrategy` — интерфейс для разных типов атак  
- `MeleeAttack`, `MagicAttack`, `RangeAttack` — конкретные реализации  
- `BattleTemplate` — задаёт шаблон хода боя (герой → монстр → проверка HP)

**Пример:**
```java
public class Mage extends Hero {
    public Mage() { this.attackStrategy = new MagicAttack(); }
}
```

---

### 💎 Decorator
**Предметы:**  
- `Potion` — базовое зелье, восстанавливает HP  
- `GreaterPotion` — декоратор, усиливающий эффект обычного зелья  

**Пример:**
```java
Item potion = new Potion();
Item strongPotion = new GreaterPotion(potion);
strongPotion.apply(hero);
```

---

### 👀 Observer
**Классы:**  
- `HeroObserver` — интерфейс наблюдателя  
- `LogObserver` — логирует изменение характеристик героя  

**Пример:**
```java
hero.addObserver(new LogObserver());
hero.takeDamage(20);
```

**Результат:**
```
📜 [LOG]: HP героя изменился → 80
```

---

### ⚠️ Исключения
**Реализованы:**
- `LowHealthException` — если здоровье героя ≤ 0  
- `InventoryFullException` — если рюкзак переполнен  
- `DragonFireException` — особая атака босса-дракона  

**Пример:**
```java
try {
    battle.startBattle(hero, monster);
} catch (LowHealthException e) {
    System.out.println("💀 " + e.getMessage());
}
```

---

### 🧮 Коллекции и Итераторы
- Рюкзак героя — `List<Item>`  
- Монстры подземелья — `List<Monster>` с `Iterator`  
- Рейтинг игроков — `Map<String, Integer>`  

**Пример:**
```java
Map<String, Integer> scores = new HashMap<>();
scores.put("Viktor", 5);
scores.put("Anna", 3);
```

---

## 🚀 Как запустить

```bash
javac src/Main.java
java Main
```

---

## 💬 Пример игрового вывода

```
🏰 Добро пожаловать в подземелье!

Выберите героя:
1.Воин ⚔️  2.Маг 🔮  3.Лучник 🏹
> 2
Вы выбрали: Mage!
Ваш противник: Dragon!

> 1.Атаковать  2.Предмет  3.Бежать
1
Attacking with magic spells!
🐉 Dragon unleashes fiery breath!
🔥 Ошибка при атаке дракона: Дракон промахнулся! Огонь ушёл в небо!

❤️ HP героя: 85
💀 HP монстра: 55
```

---

## 🧾 Автор
**Виктор Стародубцев (Wictor)**  
💻 Учебный проект по курсу *ООП и паттерны проектирования (Java)*  
📅 Практическая работа №7 — *ноябрь–декабрь 2025*
