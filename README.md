# Aot – автоматическая обработка текста

Java библиотека для быстрого (!) получения леммы и морфологической информации по заданному слову
русского языка. Реинкарнация [aot-lematizier](https://github.com/bazhenov/aot-lematizer) с удобным
обновленным API, избавленная от closed-source зависимостей, упрощенная для сборки, и как следствие
легко подключаемая в другие проекты с помощью репозитория Jitpack. Поддерживает все версии Java
начиная с 8-ой версии, а также протестирована со всеми версиями Kotlin начиная с 1.5.

1. Определяет исходную форму слова согласно правилам русского языка, так называемую лемму (первое
   лицо, единственное число).
2. Определяет всевозможные характеристики слова, такие как часть речи, род, падеж, число и т. д.
3. Определяет всевозможные преобразования слова с их характеристиками (flexion).
4. Различает слова с полным соответствием письменной формы, но разной морфологией, например
   замок (что? строение) и замок (что сделал? замок под дождем).
5. Различает омонимии, то есть слова с полным соответствием и письменной формы и морфологии, но
   различным смыслом, например замок (что? строение) и замок (что? запор).

## Как подключить?

Вам понадобится Gradle или Maven, или другая система сборки.

[![](https://jitpack.io/v/demidko/aot.svg)](https://jitpack.io/#demidko/aot)

## Как использовать?

```java
import java.io.IOException;

import static java.lang.System.out;
import static com.github.demidko.aot.WordformMeaning.lookupForMeanings;

class Example {

  public static void main(String[] args) throws IOException {

    var meanings = lookupForMeanings("люди");
    out.println(meanings.size());
    /* 1 */

    out.println(meanings.get(0).getMorphology());
    /* [С, мр, им, мн] */

    out.println(meanings.get(0).getLemma());
    /* человек */

    for (var t : meanings.get(0).getTransformations()) {
      out.println(t.toString() + " " + t.getMorphology());
      /*
       * человек [С, мр, им, ед]
       * человека [рд, С, мр, ед]
       * человеку [С, мр, ед, дт]
       * человека [С, мр, ед, вн]
       * человеком [тв, С, мр, ед]
       * человеке [С, мр, ед, пр]
       * люди [С, мр, им, мн]
       * людей [рд, С, мр, мн]
       * человек [рд, С, мр, мн]
       * людям [С, мр, мн, дт]
       * человекам [С, мр, мн, дт]
       * людей [С, мр, мн, вн]
       * людьми [тв, С, мр, мн]
       * человеками [тв, С, мр, мн]
       * людях [С, мр, мн, пр]
       * человеках [С, мр, мн, пр]
       */
    }
  }
}
```

## Откуда данные?

Используется предварительно скомпилированный бинарный словарь морфологии из
проекта [aot-binary](https://github.com/demidko/aot-binary).
