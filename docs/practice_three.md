# 🧱 Лабиринт (Java)

Учебная консольная игра: герой ищет выход из лабиринта, встречает монстров и сражается ⚔️.

## 🚀 Функции
- Двумерный лабиринт 10x10:
  - `#` — стены  
  - `_` — пустота  
  - `P` — игрок  
  - `M` — монстр  
  - `K` — ключ  
  - `E` — выход  

- Герой:
  - HP = 100
  - XP = 0
  - LVL = 1
  - ATK = 20

- Управление: `w` ↑, `s` ↓, `a` ←, `d` →  

- События:
  - Удар об стену → -20 HP  
  - Монстр → бой (атака, лечение, побег)  
  - Ключ `K` → нужен для выхода  
  - Выход `E` открывается только с ключом  

- Условия конца игры:
  - HP ≤ 0 → проигрыш  
  - Истёк лимит времени (40 секунд) или шагов (30)  
  - Добрался до выхода с ключом → победа 🎉  

## ▶️ Запуск
```bash
javac Main.java 
java Main
```


## 📌 code

```
import java.util.Scanner;
import java.util.random.RandomGenerator;

public class Main {
    // Индексы в массиве героя
    static final int HP = 0, XP = 1, LVL = 2, ATK = 3;

    static char[][] maze;
    static int heroRow, heroCol;
    static int[] hero = {100, 0, 1, 20}; // HP, XP, LVL, ATK
    static boolean gameOver = false;
    static boolean isKey = false;
    static String lastMessage = "";
    static int numSteps = 30; // Лимит ходов

    static Scanner user = new Scanner(System.in);
    static RandomGenerator rng = RandomGenerator.getDefault();

    public static void main(String[] args) {
        long startTime = System.currentTimeMillis();
        long timeLimit = 40_000; // 40 секунд

        initMaze();

        while (!gameOver) {
            long elapsed = System.currentTimeMillis() - startTime;
            if (elapsed >= timeLimit) {
                System.out.println("⏰ Время вышло! Игра окончена.");
                break;
            } else if (numSteps <= 0) {
                System.out.println("🚶‍♂️ Лимит ходов исчерпан! Игра окончена.");
                break;
            } else {
                clearConsole();
                printMaze();
                System.out.print("Введите команду (w/a/s/d): ");
                char move = user.next().charAt(0);
                moveHero(move);
            }
        }

        user.close();
    }

    // === Лабиринт ===
    private static void initMaze() {
        maze = new char[][]{
                {'#','#','#','#','#','#','#','#','#','#'},
                {'#','P','_','_','_','M','_','_','_','#'},
                {'#','_','#','#','_','#','#','#','_','#'},
                {'#','_','#','_','_','_','M','#','_','#'},
                {'#','_','#','_','#','#','#','#','_','#'},
                {'#','K','_','_','#','M','_','_','_','#'},
                {'#','#','#','_','#','#','#','#','_','#'},
                {'#','_','M','_','_','_','#','_','_','#'},
                {'#','_','#','#','#','_','#','E','_','#'},
                {'#','#','#','#','#','#','#','#','#','#'}
        };
        heroRow = 1;
        heroCol = 1;
    }

    private static void printMaze() {
        for (char[] row : maze) {
            for (char c : row) {
                System.out.print(c + " ");
            }
            System.out.println();
        }
        System.out.println("\nHP: " + hero[HP] + "  XP: " + hero[XP] +
                "  LVL: " + hero[LVL] + "  ATK: " + hero[ATK]);
        if (!lastMessage.isEmpty()) {
            System.out.println("Сообщение: " + lastMessage);
            lastMessage = "";
        }
    }

    private static void moveHero(char direction) {
        int newRow = heroRow;
        int newCol = heroCol;

        switch (direction) {
            case 'w' -> newRow--;
            case 's' -> newRow++;
            case 'a' -> newCol--;
            case 'd' -> newCol++;
            default -> {
                System.out.println("Некорректная команда!");
                return;
            }
        }

        char cell = maze[newRow][newCol];

        // Стена
        if (cell == '#') {
            hero[HP] -= 20;
            System.out.println("Ай! Стена. -20 HP");
            if (hero[HP] <= 0) {
                System.out.println("💀 Герой погиб. Игра окончена.");
                gameOver = true;
            }
            return;
        }

        // Ключ
        if (cell == 'K') {
            isKey = true;
            lastMessage = "🔑 Вы подобрали ключ!";
        }

        // Выход
        if (cell == 'E') {
            if (isKey) {
                lastMessage = "🎉 Победа! Вы выбрались из лабиринта.";
                gameOver = true;
            } else {
                lastMessage = "🚪 Дверь закрыта! Нужен ключ.";
            }
        }

        // Монстр
        if (cell == 'M') {
            battle(hero);
            if (hero[HP] <= 0) {
                gameOver = true;
                return;
            } else {
                maze[newRow][newCol] = '_'; // монстр исчезает
            }
        }

        numSteps--;
        maze[heroRow][heroCol] = '_';
        heroRow = newRow;
        heroCol = newCol;
        maze[heroRow][heroCol] = 'P';
    }

    // === Бой ===
    private static void battle(int[] hero) {
        System.out.println("⚔ Встреча с монстром!");

        int monsterHP = 30 + rng.nextInt(20); // 30–50 HP
        int monsterATK = 8 + rng.nextInt(6);  // 8–13 ATK
        int[] monster = {monsterHP, monsterATK};

        boolean ranAway = false;

        while (monster[0] > 0 && hero[HP] > 0 && !ranAway) {
            System.out.println("\nВаше состояние: HP=" + hero[HP] +
                    " | XP=" + hero[XP] + " | LVL=" + hero[LVL]);
            System.out.println("Выберите действие: 1 → атаковать, 2 → лечиться, 3 → убежать");

            int action = user.nextInt();

            switch (action) {
                case 1 -> attack(hero, monster);
                case 2 -> heal(hero);
                case 3 -> {
                    if (rng.nextBoolean()) {
                        System.out.println("🏃 Герой убежал от монстра!");
                        ranAway = true;
                    } else {
                        System.out.println("Не удалось убежать!");
                    }
                }
                default -> System.out.println("⚠️ Некорректный выбор!");
            }

            if (monster[0] > 0 && !ranAway) {
                monsterAttack(hero, monster[1]);
            }

            if (hero[HP] <= 0) {
                System.out.println("💀 Герой погиб в бою.");
                return;
            }

            if (monster[0] <= 0) {
                int xpGain = 10;
                System.out.println("🎉 Монстр повержен! +" + xpGain + " XP");
                hero[XP] += xpGain;
                levelUp(hero);
            }
        }
    }

    private static void attack(int[] hero, int[] monster) {
        int damage = hero[ATK] + rng.nextInt(6); // +0..5
        monster[0] -= damage;
        System.out.println("🗡️ Герой атакует и наносит " + damage + " урона!");
    }

    private static void heal(int[] hero) {
        int healAmount = rng.nextInt(10, 31); // 10..30
        hero[HP] += healAmount;
        System.out.println("💊 Герой лечится и восстанавливает " + healAmount + " HP.");
    }

    private static void monsterAttack(int[] hero, int monsterATK) {
        int damage = monsterATK + rng.nextInt(4); // +0..3
        hero[HP] -= damage;
        System.out.println("👹 Монстр атакует и наносит " + damage + " урона!");
    }

    private static void levelUp(int[] hero) {
        while (hero[XP] >= 20) {
            hero[XP] -= 20;
            hero[LVL]++;
            hero[ATK] += 5;
            System.out.println("✨ Level UP! LVL=" + hero[LVL] + " | ATK=" + hero[ATK]);
        }
    }

    // === "Очистка" консоли (ANSI escape) ===
    private static void clearConsole() {
        System.out.print("\033[H\033[2J");
        System.out.flush();
    }
}

```
