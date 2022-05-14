# Задача "Понимание JVM"


### Описание кода с точки зрения происходящего в JVM
```java 

public class JvmComprehension {              // в Metaspace помещаются все мета-данные класса JvmComprehension

    public static void main(String[] args) { // создание в Stack фрейма main
    
        int i = 1;                           // локальная переменная 'i' помещается в Stack во фрейм main
        
        Object o = new Object();             // ClassLoader находит класс Object 
                                             // и загружает его мета-информацию в Metaspace
                                             // в Heap выделяется память под объект Object, 
                                             // а ссылка 'o' на данный объект в Heap помещается в Stack во фрейм main 
                                             
        Integer ii = 2;                      // ClassLoader находит класс-обёртку Integer 
                                             // и загружает его мета-информацию в Metaspace
                                             // в Heap выделяется память под объект Integer
                                             // объект инициализируется значением 2 (immutable объект)                                     
                                             // ссылка 'ii' на объект в Heap помещается в Stack во фрейм main
                                             
        printAll(o, i, ii);                  // в Stack создаётся новый фрейм printAll,
                                             // в данный фрейм помещаются:
                                             // ссылка 'o' на ранее созданный Object в Heap,
                                             // локальная переменная i = 1,
                                             // ссылка 'ii' на ранее созданный Integer в Heap
                                             // выполнение метода printAll
                                             // после выполнения метода фрейм удаляется из Stack                                   
                                             
        System.out.println("finished");      // в Stack создаётся новый фрейм println,
                                             // в Heap выделяется память под новый объект String
                                             // объект инициализируется значением "finished"
                                             // во фрейм println помещается ссылка на объект String в куче
                                             // выполнение метода println                                         
                                             // после выполнения метода фрейм println удаляется из Stack 
                                                                                    
    }                                        // фрейм main удаляется из Stack

    private static void printAll(Object o, int i, Integer ii) {
        Integer uselessVar = 700;                   // в Heap выделяется память под объект Integer
                                                    // объект инициализируется значением 700                                     
                                                    // ссылка 'uselessVar' на объект в Heap помещается 
                                                    // в Stack во фрейм printAll 
                                            
        System.out.println(o.toString() + i + ii);  // в Stack создаётся новый фрейм println,
                                                    // в Heap выделяется память под новый объект String
                                                    // ссылка на объект помещается во фрейм println
                                                    // выполнение метода println                                         
                                                    // после выполнения метода фрейм println удаляется из Stack 
                                                    // новый объект типа String может быть удалён из кучи при следующем 
                                                    // запуске сборщика мусора, т.к. отсутствует ссылка на данный объект
    }
    
}                                       // после завершения программы сборщик муссора очистит Heap
```