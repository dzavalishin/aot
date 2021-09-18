# Aot – автоматическая обработка текста

Java библиотека для быстрого (!) получения леммы и морфологической информации по заданному слову
русского языка. Реинкарнация [aot-lematizier](https://github.com/bazhenov/aot-lematizer) с
обновлениями, избавленная от closed-source зависимостей, упрощенная для сборки, как следствие легко
подключаемая в другие проекты. Поддерживает все версии Java начиная с 8ой версии, а также
протестирована со всеми версиями Kotlin 1.5.*.

1. Определяет исходную форму слова согласно правилам языка, т. н. лемму, первое лицо, единственное
   число.
2. Определяет всевозможные характеристики слова, такие как род, падеж, число и т. п.
3. Определяет всевозможные преобразования леммы (flexion).

## Как подключить?

Вам понадобится Gradle или Maven, или другая система сборки.

[![](https://jitpack.io/v/demidko/aot.svg)](https://jitpack.io/#demidko/aot)

## Как использовать?

```java
import static java.lang.System.out;
import static com.github.demidko.aot.WordformMeaning.lookupForMeanings;

class Example {

  public static void main(String[] args) {
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

## Пример для старого API (не рекомендуется)

Продемонстрируем работу словаря на примере смысловой коллизии:

```java
class Example {

  public static void main(String[] args) {

    // на создание словаря уходит несколько секунд,
    // поэтому, лучше иметь один экземпляр на все время работы
    var d = new com.github.demidko.aot.HashDictionary();

    // получаем все наборы смыслов по интересующему нас слову
    for (var word : d.lookup("замок")) {

      // проходим по всем возможным словоформам каждого смысла
      for (var flex : word.getFlexions()) {

        // и выводим их характеристики
        System.out.print(flex.toString() + flex.getTags());
        System.out.print(' ');
      }
      System.out.println("\n");
    }
  }
}

```

Получим что-то вроде

```shell
1. <замок, [С, мр, ед, им], [С, мр, ед, вн]> ..., ...
2. <замокнуть, [Г, дст, прш, мр, ед]> ..., ...
```

Первая лемма, это слово зАмок (строение). Это либо замок (кто?) в именительном падеже, либо замок (
кого?) в винительном падеже. Она же одновременно характеризует слово замОк (устройство для запирания
дверей). Для нее, слово замок также может быть либо именительным, либо винительным падежом. И
наконец, третий смысл, это вторая лемма, замокнуть (он что сделал? Он замок под дождем). Для нее
слово замок характеризуется лишь одним набором грамматической информации.

## Откуда данные?

Используется словарь бинарной морфологии из
проекта [aot-binary](https://github.com/demidko/aot-binary).








