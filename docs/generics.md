# Что такое дженерики (обобщения)?

В документации **TypeScript** сказано
> В таких языках, как C# и Java, одним из основных инструментов в наборе инструментов для создания повторно используемых компонентов являются обобщения, то есть возможность создавать компонент, который может работать с множеством типов, а не с одним. Это позволяет пользователям использовать эти компоненты и использовать свои собственные типы.

Если говорить более понятным языком, то
> Обобщённый тип (обобщение, дженерик) позволяет ***резервировать*** место для типа, который будет заменён на ***конкретный***, переданный пользователем, при вызове функции или метода, а также при работе с классами.

---

\
Небольшой пример функции возвращающей переданое ей значение:
``` 
/* Javascript */

function foo(value) {
  return value
}

console.log( foo(1) )  // 1
```
```
/* TypeScript (without generics) */

function foo(value: any): any {   // WE USE ANY =(
  return value
}

console.log( foo(1) )  // 1
```
Для того чтобы эта функция работала без `any` мы можем написать несколько функций которые будут работать с конкретными типами: `number`, `string`, `boolean`...

А можем использовать **дженерики**:
```
/* TypeScript (with generics) */

function foo<T>(value: T): T {
  return value
}

console.log( foo<number>(1) )  // 1
```
Можно заметить что теперь мы принимаем тип `T` и возвращаем тип `T`. \
Еще появилась странная синтаксическая конструкция `< >` в обьявлении функции и в ее вызове. 

Проще всего представить себе что `< >` тоже самое что и `( )`, но в первом случаем мы передаем **типы** а не значения, и тип `T` станет типом который мы передали при вызове функции 
```
foo<number>(1) // T becomes number
```
---

# Для чего это может пригодится?

Допустим у нас есть функция которая выводит в консоль все значения массива. На JavaScript это выглядит так:
```
function printArr(arr) {
  const length = arr.length  
  for (let i = 0; i < length; i++) {
    console.log(arr[i])
  }
}

const primes = [2, 3, 5, 7, 11]
printArr(primes)
```
Ничего сложного, мы принимаем любой аргумент и со всей силы надеемся что это массив.

В TypeScript мы не будем использовать `any` - потому что мы вернемся к проблемам JavaScript. Используем дженерики:
```
function printArr<T>(arr: T[]) {
  const lenght = arr.length
  for (let i = 0; i < length; i++) {
    console.log(arr[i])
  }
}

const primes: number[] = [2, 3, 5, 7, 11]
printArr<number>(primes)
```
Мы можем использовать любой нужный нам тип:
```
const strings: string[] = ['Generics', 'are', 'awesome', '!']
printArr<string>(strings)
```
---

# Ограничение типов
Бывает необходимо ограничить допустимые типы. Допустим у нас есть 3 типа:
- Number
- BigNumber `extends` Number
- SmallNumber `extends` Number

И нужна функция которая будет выводить любой из этих типов.
В этом поможет ключевой слово `extends` которое также можно использовать в обьявлении функции. 
```
class BigNumber extends Number { }
class SmallNumber extends Number { }

function printNumber<T extends Number>(value: T) {
  console.log(value)
}

const n0 = new Number(1)
const n1 = new BigNumber(100000)
const n2 = new SmallNumber(0.00001)

printNumber<Number>(n0)
printNumber<BigNumber>(n1)
printNumber<SmallNumber>(n2)
```
Кстати, так тоже работает:
```
printNumber<Number>(n0)
printNumber<Number>(n1)
printNumber<Number>(n2)
```

Еще пример:
```
class A {
  constructor(
    public name: string
  ) { }
  print(): void {
    console.log(`ClassA: ${this.name}`)
  }
}

class B extends A {
  print(): void {
    console.log(`ClassB: ${this.name}`)
  }
}


function printName<T extends A>(obj: T) {
  obj.print()
}

const a = new A('Aname')
const b = new B('Bname')
printName<A>(a)
printName<B>(b)
```

# Обобщенные классы и интерфейсы
Допустим нам нужен класс, обьект которого хранит значение заданного типа.
```
class Container<T> {
  value: T
  constructor(value: T) {
    this.value = value
  }
}
```
И далее создаем экземпляры с нужными типами
```
const stringContainer = new Container<string>('some string')
const numberContainer = new Container<number>(42)
```

Все тоже самое работает и с интерфейсами:
```
interface User<T> {
  ID: T
}

class Person implements User<number> {
  ID: number
}
```
# Передача нескольких типов
Естественно мы можем передать в класс\функцию\интерфейс сколько угодно типов:
```
class SomeClass<A, B> {
  fieldA: A
  fieldB: B
}

interface SomeInterface<_, $> {
  value1: _
  value2: $
}

function doSomething<Type1, Type2>(
  arg1: Type1, arg2: Type2
) {
  //do stuff
}
```
---


# Именование
Мы конечно же можем придумывать любые имена для типов, но чаще всего можно встретить короткие имена для типов:
- `E` - Элемент
- `K` - Ключ
- `N` - Число
- `T` - Тип
- `V` - Значение

Также существует практика называть типы по аналогии с классами:
```
class Box<Content> {

  private content: Content
  
  public getContent(): Content {
    return this.content
  }
  
  public setContent(content: Content): void {
    this.content = content
  }

}
```