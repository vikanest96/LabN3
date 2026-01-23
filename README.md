## Отчет по лабораторной работе № 3

#### № группы: `ПМ-2502`

#### Выполнила: `Нестерчук Виктория Юрьевна`

#### Вариант: `12`

### Cодержание:

- [Постановка задачи](#1-постановка-задачи)
- [Описание класса](#2-описание-класса)
- [Алгоритм](#4-алгоритм)
- [Программа](#5-программа)
- [Анализ правильности решения](#6-анализ-правильности-решения)

### 1. Постановка задачи

> Разработать программу для работы с множествами, представляющими списки уникальных элементов. Реализовать базовые операции, включая добавление и удаление элементов, а также сложные операции, такие как объединение, пересечение и проверка
вложенности множеств.

### 5. Программа

```java
class Set
{
    double[] set;   // Поле - массив вещественных чисел;

    public static Scanner in = new Scanner(System.in);

    public double[] setOrig(double[] set) {    // Вспомогательный метод setOrig, который возвращает массив уникальных элементов; 
        int cnt = 0;
        int cnt1 = 0;
        for (int i = 0; i < set.length; i++) {
            boolean flag = false;
            for (int j = 0; j < i; j++) {
                if (set[i] == set[j]) {
                    flag = true;
                }
            }
            if (!flag)
                cnt++;
        }
        double[] a = new double[cnt];
        for (int i = 0; i < set.length; i++) {
            boolean flag = false;
            for (int j = 0; j < i; j++) {
                if (set[i] == set[j]) {
                    flag = true;
                }
            }
            if (!flag) {
                a[cnt1++] = set[i];
            }
        }
        return a;
    }

    public Set() {        // Конструктор который создает множество из пустого списка;
        this.set = setOrig(new double[0]);
    }
    public Set(double[] a) {     // Конструктор который создает множество из списка вещественных чисел;
        this.set = setOrig(a);
    }
    public Set(int n) {              // Конструктор который создает множество из n вещественных чисел введенных с клавиатуры;
        double[] a = new double[n];
        for (int i = 0; i < n; i++) a[i] = in.nextInt();
        this.set = setOrig(a);
    }
    public void add(double element) {        // Добавление указанного элемента в текущее множество;
        double[] a = new double[set.length + 1];
        for (int i = 0; i < set.length; i++) a[i] = set[i];
        a[set.length] = element;

        this.set = setOrig(a);
    }
    public void delete(double element) {      // Удаление указанного элемента из текущего множество;
        boolean flag = false;
        int Index = 0;
        for (int i = 0; i < set.length; i++) {
            if (set[i] == element) {
                flag = true;
                Index = i;
            }
        }
        if (flag) {
            double[] a = new double[set.length - 1];
            for (int i = 0; i < Index; i++) {
                a[i] = set[i];
            }
            for (int i = Index; i < a.length; i++) {
                a[i] = set[i+1];
            }
            this.set = a;
        }
    }
    public int countSet() {   // размер множества;
        return set.length;
    }
    public boolean elementInSet(double element) {   // Проверяем есть ли указанный элемент в текущем множестве;
        boolean flag = false;
        for (int i = 0; i < set.length; i++) {
            if (set[i] == element) flag = true;
        }
        return flag;
    }
    public static Set combiningSets(Set c1, Set c2) {           // Объединение множеств;
        double[] a = new double[c1.countSet() + c2.countSet()];

        for (int i = 0; i < c1.countSet(); i++) a[i] = c1.set[i];
        for (int i = 0; i < c2.countSet(); i++) a[i+c1.countSet()] = c2.set[i];

        return new Set(a);
    }
    public static Set intersectionSets(Set c1, Set c2) {      // Пересечение множеств;
        Set m1;
        Set m2;
        if (c1.countSet() <= c2.countSet()) {
            m1 = c1;
            m2 = c2;
        } else {
            m1 = c2;
            m2 = c1;
        }
        double[] a = new double[m1.countSet()];

        for (int i = 0; i < m1.countSet(); i++) {
            if (m2.elementInSet(m1.set[i])) a[i] = m1.set[i];
        }
        return new Set(a);
    }
    public static Set simmetricDifferenceSets(Set c1, Set c2) {      // Симметрическая разность множеств;
        double[] a = new double[c1.countSet() + c2.countSet()];

        Set Combining = Set.combiningSets(c1,c2);
        Set Intersection = Set.intersectionSets(c1,c2);

        for (int i = 0 ; i < Intersection.countSet(); i++) {
            Combining.delete(Intersection.set[i]);
        }
        return Combining;
    }
    public void differenceSets(Set A) {             // Разность множеств;
        for (int i = 0; i < A.countSet(); i++) {
            if (elementInSet(A.set[i]))
                delete(A.set[i]);
        }
    }
    public static boolean comparisonSets(Set c1, Set c2) {    // Сравнение множеств;
        boolean flag;
        if (simmetricDifferenceSets(c1,c2).countSet() == 0) {
            flag = true;
        } else {
            flag =false;
        }
        return flag;
    }
    public boolean nestingSets(Set A) {       // Вложенность множеств;
        boolean flag;
        Set Intersection = intersectionSets(this, A);
        if (comparisonSets(Intersection, A)) {
            flag = true;
        } else {
            flag = false;
        }
        return flag;
    }
    @Override
    public String toString() {
        String s = "[";
        if (set.length > 0) {
            for (int i = 0; i < set.length; i++) {
                s += String.format("%.2f", set[i]);
                if (i != set.length - 1) s += ", ";
            }
        }
        return s + "]";
    }
}
```
