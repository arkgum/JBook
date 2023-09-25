# Абстрактный класс

- [Абстрактный класс](#абстрактный-класс)
  - [Введение](#введение)
  - [Объявление](#объявление)
  - [Использование](#использование)
    - [Когда применять](#когда-применять)
    - [Анонимные классы](#анонимные-классы)
  - [Заключение](#заключение)

## Введение

Как мы уже обсуждали во [введении в ООП](intro.md), класс - это совокупность `поведения` и `состояния`.

Состояние - это то, какую информацию, какие данные хранит этот класс.

Например, класс `Person`:

```java
class Person {
    private int age;
    private String name;

    // some code
}
```

Наш класс содержит переменные, которые хранят возраст и имя персонажа.
У каждого объекта будет свой возраст и имя, в данном случае - это и есть состояние объекта.

Поведение же - это то, что мы можем ожидать при работе с классом, как с ним взаимодействовать и т.д..

За поведение в `Java` отвечает понятие [интерфейса](interface.md).

До этого мы имели дело с полностью определенными, законченными(завершенными), классами.

Что значит полностью определенный класс или что класс законченный(завершенный)?

Назовем класс полностью определенным, если его поведение(интерфейс) реализовано, т.е мы точно знаем **что** умеет делать объект класса и **как** он это делает, такой класс **полностью** реализует свой интерфейс.
И, что самое главное, такой класс **конкретен**.

Однако, бывают ситуации, когда мы можем описать класс, но определение его поведения **не завершено**. Мы знаем **что** умеет делать класс, но еще не определили **как именно** он это делает.

Возьмем любимый пример всех учебников по программированию для начинающих: класс `Figure`, фигура.

Фигура - это нечто асбтрактное, когда говорят фигура мы не можем представить ничего конкретного.
Фигурой может быть и квадрат, и круг, и прямоугольник.

Каждая фигура состоит из линий, углов, имеет цвет.
Также у фигуры может быть метод `draw`, рисующий фигуру. Но в отличии от свойств, этот метод реализуется у каждого класса-наследника по разному. При этом нет некоего 'общего' поведения или поведения по-умолчанию. Т.е мы хотим объявить такой метод в классе, но реализацию оставить наследникам.
Это значит, что наш класс `Figure` будет незаконченным.

Для подобных случаев в `Java` можно объявить `абстрактный класс`.

## Объявление

Для того, чтобы объявить класс абстрактным существует ключевое слово `abstract`.

```java
public abstract class Figure {
    // some code
}
```

Как было сказано выше: абстрактность относится еще и к тому, что `поведение` класса определено не до конца.
Из этого можно сделать вывод, что методы класса, поведение которых мы в данном классе определить/реализовать не можем тоже помечаются как абстрактные:

```java
public abstract class Figure {
    protected int height;
    protected String width;

    abstract void draw();

    // some code
}
```

Если класс содержит **хотя бы один** абстрактный метод - он обязан быть абстрактным классом. Это логично, ведь абстрактный метод - это не завершенность в определении класса.

---

**Вопрос**:

Может ли класс быть абстрактным, при этом **не имея** абстрактных методов? Другими словами: валиден ли следующий код?

```java
public abstract class Figure {
    protected int height;
    protected String width;

    public void draw() {
        System.out.println("Привет!");
    }
}
```

**Ответ**:

Абсолютно валиден.

Да, класс может быть абстрактным, при этом не имея абстрактных методов.

Такие классы могут понадобиться вам, чтобы выстраивать более гибкую цепочку иерархий классов. К тому же, никто не запрещает вам при необходимости переопределить какие-то методы такого класса.

---

Как объявить абстрактный класс разобрались, теперь посмотрим как c этим работать.

## Использование

Основным отличием абстрактных классов от обычных в использовании является то, что запрещается создавать экземпляр(instance) абстрактного класса.
Другими словами, вы не можете написать пример ниже с абстрактным классом `Figure`:

```java
public abstract class Figure {
    protected int height;
    protected String width;

    // some code
}

public class Main {
    public static void main(String[] args) {
        Figure p = new Figure(); // Compile Error
    }
}
```

Запрет этот наложен именно из-за незавершенности класса.
Экземпляр класса должен олицетворять нечто законченное, то, с чем можно работать и от чего понятно что ждать.

Если задуматься, то в реальном мире происходит нечто похожее: мы оперируем только завершенными объектами, но можем ссылаться на какие-то абстрактные вещи.
Например, вы можете сказать: "Я купил лампу", но без деталей вашей новой покупки каждый человек представит себе "свою" лампу, ведь вы описали нечто абстрактное.

Еще одной аналогией можно привести пример шаблона чертежа дома, на котором обозначены только несущие стены.
Это ещё только эскиз, строить дом по нему нельзя, но на его основе уже можно делать разные варианты законченных чертежей, которые уже можно использовать в строительстве разных домов.

При этом абстрактный класс **может** иметь конструктор.

```java
public abstract class Figure {
    protected int height;
    protected String width;

    public Figure(int height, String width) {
        this.height = height;
        this.width = width;
    }
}
```

---

**Вопрос**:

А какой смысл в конструкторе, если создать экземпляр абстрактного класса все равно нельзя?

**Ответ**:

Надо помнить, что конструктор супер-класса(родительского) явно участвует в создании объекта класса-наследника, конструктор абстрактного класса - не исключение.
Например, его можно использовать для задания начальных значений общих переменных, объявленных в абстрактном классе.

---

**Вопрос**:

Может ли абстрактный класс иметь абстрактный конструктор?

**Ответ**:

Нет, не может.
Ответ на этот вопрос логичен, если задуматься какую роль выполняет конструктор.

---

**Вопрос**:

Может ли абстрактный класс иметь абстрактный статический метод?

**Ответ**:

Снова нет, что, на мой взгляд, довольно логично.
При этом абстрактный класс спокойно может иметь определенный статический метод, хоть это и не совсем желательно.

> Потому что статика плохо ложится на `ООП`.

Более того, абстрактный класс может даже содержать метод `main` - так как это просто еще один статический метод, и абстрактный класс можно выполнять при помощи метода `main`, если не создавать его экземпляров.

---

**Вопрос**:

Может ли абстрактный класс быть объявлен с модификатором `final`? Т.е быть финальным?

**Ответ**:

Разумеется нет, иначе теряется весь смысл абстрактного класса, о чем вам сообщит компилятор, выдав ошибку компиляции.

---

### Когда применять

Когда стоит применять абстрактные классы?

Как вы наверняка поняли, абстрактные классы в основном предназначены для использования в [наследовании](inheritance.md). Т.е стоит создавать абстрактный класс тогда, когда вам нужен еще один слой абстракции, при этом, на этом слое вы **уже** знаете как определить какие-то параметры или методы.

Возвращаясь к примеру с нашим `Figure`-ом:

```java
public abstract class Figure {
    protected int height;
    protected String width;

     abstract void draw();
}

public class Rectangle extends Figure {
    @Override
    void draw() {
        System.out.println("Draw Rectangle");
    }
}

public class Circle extends Figure {
    @Override
    void draw() {
        System.out.println("Draw Circle");
    }
}
```

И у `Rectangle`, и у `Circle` **уже** известны общие свойства: ширина и высота, но каждая фигура рисуется по-своему. То, **как** каждая фигура отрисовывается - это уже детали реализации, детали конкретного класса.

В качестве примера использования абстрактных классов в стандартной библиотеке `Java` можно посмотреть любую реализацию `java.util.List`.

Для примера мы возьмем `java.util.ArrayList` и `java.util.LinkedList`. Эти классы являются наследниками `java.util.AbstractList`.

В промежуточном слое, в `java.util.AbstractList`, мы **уже** знаем как определить некоторые методы, например, `indexOf`, но при этом вы еще не знаете как делать `get(int index)`, так как реализация `get` явно зависит от конкретной реализации.

### Анонимные классы

 К слову говоря, необязательно каждый раз для использования абстрактного класса создавать отдельный файл и наследоваться от абстрактного класса, определяя все его абстрактные методы.

 Можно создать `анонимный класс` и определить абстрактные методы.

 ```java
 public abstract class Figure {
    protected int height;
    protected String width;

     abstract void draw();
}

public class Main {
    public static void main(String[] args) {
        Figure p = new Figure() {
            public void draw() {
                System.out.println("Draw something anonymous");
            }
        }
    }
}
 ```

Здесь объявлен абстрактный класс `Figure` и в методе `main` мы создали анонимный класс, реализующий абстрактные методы `Figure`.

Таким образом мы **завершили** абстрактный класс, но не дали имени этому завершенному классу, отсюда и название: `анонимный` класс.

## Заключение

Абстрактные классы помогают описывать промежуточные состояния, выстраивать иерархии классов и добавлять новые слои абстракции.

> Стоит отметить, что `java.lang.Object` не является абстрактным классом, хоть иногда и кажется, что сделать это было бы логично.
>
> Почему так сделано, я рассуждаю [здесь](../object/intro.md).

Как и полностью завершенные, абстрактные классы могут реализовывать интерфейсы.
При этом, так как абстрактный класс может содержать недоопределенные методы, допускается реализовывать **не все** методы у реализуемых интерфейсов.

Класс может не содержать абстрактных методов и при этом быть абстрактным.
Однако класс, содержащий хотя бы один абстрактный метод **обязан** быть абстрактным классом.

Абстрактные классы тесно связаны с понятием [интерфейса](interface.md), поэтому [здесь](abstract_vs_interface.md) мы разберем отличия абстрактного класса от интерфейса, а также когда что предпочтительнее использовать.

Также стоит познакомиться с [SOLID](SOLID.md)