# **Понимание Java Virtual Machine (JVM)**
## **Цель**
Проанализировать код и описать каждую строчку с точки зрения происходящего в JVM.
## **Исходный код**
```java
public class JvmComprehension {

    public static void main(String[] args) {
        int i = 1;                      // 1
        Object o = new Object();        // 2
        Integer ii = 2;                 // 3
        printAll(o, i, ii);             // 4
        System.out.println("finished"); // 7
    }

    private static void printAll(Object o, int i, Integer ii) {
        Integer uselessVar = 700;                   // 5
        System.out.println(o.toString() + i + ii);  // 6
    }
}
```

## **Анализ кода**
1. Первым делом, JVM дает команду осуществить загрузку класса JvmComprehension. Загрузка классов реализуется с помощью загрузчика классов JAVA - *ClassLoader*.
   ```java
   public class JvmComprehension {
   ```
   Класс JvmComprehension, а также все системные классы, используемые программой, попадают в области памяти *Metaspace*, которая будет хранить в себе всю информацию о классах (имя, методы, поля и тд.).

    ![Metaspace](/pic1.png) 

2. Когда программа доходит до второй строчки кода, в Stack Memory создается frame для метода `main`. 

   ```java
   public static void main(String[] args) {
   ```

   ![full](/pic2.png) 

3. Во frame `main` попадает локальная переменная переменная i со значением 1.
   ```java
   int i = 1;
   ```
    ![main](/pic3.png) 

4. В куче (heap) происходит создание объекта `Object`. После этого в Stack Memeory во frame `main` создается переменная `o` типа `Object`, которая ссылается на объект `Object`.

    ```java
    Object o = new Object();
    ```

    ![Object](/pic4.png) 

5. В куче (heap) происходит создание нового объекта, после этого в Stack Memory во frame `main` создается переменная `ii`, которая ссылается на новый объект типа `Integer` со значением 2.

    ```java
    Integer ii = 2;
    ```

    ![Integer](/pic5_new.png) 

6. Далее в Stack Memory попадает метод `printAll`, вызывая метод с 3 параметрами: `o` и `ii` берутся из кучи, а примитив `i` - из frame `main`.

    ```java
    printAll(o, i, ii);
    ```

    ![printAll](/pic6.png)   

7. При вызове метода `printAll` в куче происходит создание нового объекта, после этого в Stack Memory во frame `printAll` создается переменная `uselessVar`, которая ссылается на новый объект типа `Integer` со значением 700.

    ```java
    Integer uselessVar = 700;
    ```

    ![uselessVar](/pic7_new.png)

8. По окнчанию метода `printAll`, в консоле выводятся переданные параметры, и после этого из Stack Memory frame `printAll` удаляется. Также сборщик мусора удалить из кучи объект `Integer` со значением 700 из-за отсутсвия ссылок на этот объект.
   
    ```java
    private static void printAll(Object o, int i, Integer ii) {
        Integer uselessVar = 700;                   
        System.out.println(o.toString() + i + ii); 
    }
    ```

    ![printAll](/pic8.png)

9. Последним шагом будет выполнена последняя строчка кода из метода `main`, произойдет вывод в консоли "finished" и программа завершит свою работу. Из Stack Memory будет удален frame `main`, а сборщик мусора удалить из кучи все оставшиеся объекты: `Integer` со значением 2 и `Object`, так как ссылок на эти объекты не существует. 
   
    ```java
    System.out.println("finished");
    ```

    ![printAll](/pic9.png)