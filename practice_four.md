# 🎒 Inventory Console App (Java)

Учебный проект по Java: консольное приложение для управления инвентарём игрока.  
Можно добавлять предметы, просматривать рюкзак и удалять их через простое меню.

---

## 📌 Функционал
- Добавление предмета (название, тип, ценность)
- Просмотр списка предметов
- Удаление предмета по индексу
- Выход из программы

---

## 🛠️ Используемые технологии
- **Java 17+**
- **ArrayList** для хранения предметов
- Консольное меню (`Scanner` + `switch-case`)

---

##📌 code
---
```
import java.util.ArrayList;
import java.util.Scanner;

class Item {
    private String name;
    private String type;
    private int value;

    public Item(String name, String type, int value) {
        this.name = name;
        this.type = type;
        this.value = value;
    }

    @Override
    public String toString() {
        return "Название: " + name + ", Тип: " + type + ", Ценность: " + value;
    }
}

class Inventory {
    private ArrayList<Item> items = new ArrayList<>();

    public void addItem(Item item) {
        items.add(item);
        System.out.println("Добавлен: " + item);
    }

    public void showItems() {
        if (items.isEmpty()) {
            System.out.println("Инвентарь пуст.");
        } else {
            System.out.println("Предметы в инвентаре:");
            for (int i = 0; i < items.size(); i++) {
                System.out.println(i + " - " + items.get(i));
            }
        }
    }

    public void removeItem(int index) {
        if (index >= 0 && index < items.size()) {
            Item removed = items.remove(index);
            System.out.println("Удалён: " + removed);
        } else {
            System.out.println("Неверный номер предмета.");
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Scanner user = new Scanner(System.in);
        Inventory inventory = new Inventory();

        while (true) {
            System.out.println("\n=== Меню ===");
            System.out.println("1 - Добавить предмет");
            System.out.println("2 - Показать инвентарь");
            System.out.println("3 - Удалить предмет");
            System.out.println("0 - Выйти");
            System.out.print("Выберите действие: ");

            int choice = user.nextInt();
            user.nextLine(); // очищаем буфер

            switch (choice) {
                case 1 -> {
                    System.out.print("Введите название предмета: ");
                    String name = user.nextLine();

                    System.out.print("Введите тип предмета (оружие, зелье, броня, артефакт, еда): ");
                    String type = user.nextLine();

                    System.out.print("Введите ценность предмета: ");
                    int value = user.nextInt();

                    inventory.addItem(new Item(name, type, value));
                }
                case 2 -> inventory.showItems();
                case 3 -> {
                    inventory.showItems();
                    System.out.print("Введите номер предмета для удаления: ");
                    int index = user.nextInt();
                    inventory.removeItem(index);
                }
                case 0 -> {
                    System.out.println("Выход из программы.");
                    user.close();
                    return;
                }
                default -> System.out.println("Неверный выбор. Попробуйте снова.");
            }
        }
    }
}

```

---
## ▶️ Запуск
```bash
javac Main.java
java Main
```
