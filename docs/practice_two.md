# 👾 Мини-RPG (Java)

Учебная консольная игра на **Java**, в которой герой сражается с тремя монстрами подряд.

## 🚀 Функционал
1. **Характеристики героя** хранятся в массиве:
   - `hero[0]` → здоровье (HP)  
   - `hero[1]` → опыт (XP)  
   - `hero[2]` → уровень (LVL)  
   - `hero[3]` → сила атаки  

2. **Монстры**:
   - HP: `15 + номерРаунда * 10`  
   - Атака: `5 + номерРаунда * 3`

3. **Методы**:
   - `attack(hero, monster, rng)` → герой атакует  
   - `heal(hero, rng)` → герой лечится  
   - `monsterAttack(hero, monsterAttack, rng)` → монстр атакует  
   - `levelUp(hero)` → повышение уровня  

4. **Игровой процесс**:
   - Три боя (3 раунда) с монстрами  
   - Внутри раунда цикл `while`, пока оба живы  
   - Действия героя:  
     - `1` → атаковать  
     - `2` → лечиться  
     - `3` → попытаться убежать (50% шанс)  

5. **Рандомизация**:
   - Урон героя: `baseAttack ± 0–5`  
   - Лечение: `10–30 HP`  
   - Урон монстра: `baseMonsterAttack ± 0–3`  
   - Побег: шанс 50%  

6. **Конец игры**:
   - Если HP героя ≤ 0 → проигрыш  
   - Если все 3 монстра побеждены → финальная статистика  

---

```
import java.util.Scanner;
import java.util.random.RandomGenerator;

public class main {
    public static void main(String[] args) {
        Scanner user = new Scanner(System.in);
        RandomGenerator rng = RandomGenerator.getDefault();

        System.out.println("Введите здоровье героя:");
        int health = user.nextInt();

        System.out.println("Введите уровень героя:");
        int level = user.nextInt();

        int attack = 20; 

        int[] hero = {health, 0, level, attack};

        System.out.println("Вы готовы к бою!");

        for (int round = 1; round <= 3; round++) {
            int[] monster = {15 + round * 10, 5 + round * 3};

            System.out.println("\n--- Раунд " + round + " ---");
            System.out.println("Появился монстр с HP: " + monster[0] + " и атакой: " + monster[1]);

            boolean ranAway = false;

            while (monster[0] > 0 && hero[0] > 0 && !ranAway) {
                System.out.println("\nВаше состояние: HP=" + hero[0] + " | XP=" + hero[1] + " | LVL=" + hero[2]);
                System.out.println("Выберите действие: 1 → атаковать, 2 → лечиться, 3 → убежать");

                int action = user.nextInt();

                switch (action) {
                    case 1 -> attack(hero, monster, rng);
                    case 2 -> heal(hero, rng);
                    case 3 -> {
                        if (rng.nextBoolean()) {
                            System.out.println("🏃 Герой убежал от монстра!");
                            ranAway = true;
                        } else {
                            System.out.println("Не удалось убежать!");
                        }
                    }
                    default -> System.out.println("⚠️ Некорректный выбор, попробуйте снова.");
                }

                if (monster[0] > 0 && !ranAway) {
                    monsterAttack(hero, monster[1], rng);
                }

                if (hero[0] <= 0) {
                    System.out.println("💀 Вы погибли. Игра окончена.");
                    user.close();
                    return;
                }

                if (monster[0] <= 0) {
                    int xpGain = round * 10;
                    System.out.println("🎉 Монстр повержен! Вы получаете +" + xpGain + " XP.");
                    hero[1] += xpGain;
                    levelUp(hero);
                }
            }
            System.out.println("✨ Раунд " + round + " завершен!");
        }

        System.out.println("\n🏆 Конец приключения! Герой достиг уровня " + hero[2] +
                " с " + hero[1] + " XP и " + hero[0] + " HP.");
        user.close();
    }

    public static void attack(int[] hero, int[] monster, RandomGenerator rng) {
        int damage = hero[3] + rng.nextInt(0, 6); // baseAttack ±0-5
        monster[0] -= damage;
        System.out.println("🗡️ Герой атакует монстра и наносит " + damage + " урона!");
    }

    public static void heal(int[] hero, RandomGenerator rng) {
        int healAmount = rng.nextInt(10, 31); // 10–30 HP
        hero[0] += healAmount;
        System.out.println("💊 Герой лечится и восстанавливает " + healAmount + " HP.");
    }

    public static void monsterAttack(int[] hero, int monsterAttack, RandomGenerator rng) {
        int damage = monsterAttack + rng.nextInt(0, 4); // baseMonsterAttack ±0-3
        hero[0] -= damage;
        System.out.println("👹 Монстр атакует и наносит " + damage + " урона!");
    }

    public static void levelUp(int[] hero) {
        while (hero[1] >= 20) { // например, каждые 20 XP уровень
            hero[1] -= 20;
            hero[2] += 1;
            hero[3] += 5; // увеличиваем силу атаки
            System.out.println("✨ Герой повышает уровень! Теперь LVL=" + hero[2] + ", сила атаки=" + hero[3]);
        }
    }
}

```

---
