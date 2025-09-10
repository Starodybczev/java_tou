# java_tou

Практическая работа №1


#code

```
import java.util.Scanner;

public class Main {

    public static void main(String[] args) {
        // Ввод данных
        Scanner user = new Scanner(System.in);

        System.out.print("Введите имя героя: ");
        String name = user.nextLine();

        System.out.print("Введите возраст героя: ");
        int age = user.nextInt();

        System.out.print("Лицензия на меч (true/false): ");
        boolean hasSword = user.nextBoolean();

        System.out.print("Броня (true/false): ");
        boolean hasArmor = user.nextBoolean();

        System.out.print("Уровень силы (1–100): ");
        int power = user.nextInt();
        

        // Проверка допуска
        if (!((age > 18 && hasSword) || hasArmor)) {
            System.out.println(name + ", вы слишком слабы и не можете участвовать в бою!");
            user.close();
            return;
        }

        System.out.println(name + ", вы допущены к бою с драконом!");

        //! Исход боя
        if (power < 30) {
            System.out.println("Вы проиграли дракону!");
        } else if (power <= 60) {
            System.out.println("Вы сражались достойно, но дракон улетел!");
        } else {
            System.out.println("Поздравляем! Вы победили дракона!");
        }

        //! Подсчёт очков
        int points = age / 2 + power + (hasArmor ? 20 : 0) + (hasSword ? 10 : 0);

        //! Присвоение титула
        String title = points < 50 ? "Новичок" :
                       points <= 100 ? "Рыцарь" :
                       "Легендарный герой";

        //! Вывод результатов
        System.out.println("Очки героя: " + points);
        System.out.println(name + ", ваш титул: " + title);

        user.close();
    }
}

```
