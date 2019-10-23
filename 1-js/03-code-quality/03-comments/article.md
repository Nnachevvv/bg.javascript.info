# Коментари

Както знаем от темата <info:structure>, коментарите могат да бъдат едноредови започващи с `//` или многоредови : `/* ... */`.

Ние ги ползваме да обясним как и защо работи кодът.

На пръв поглед , коментарите могат да бъдат очевидни , но начинаещите в програмирането често ги ползват грешно.

## Лоши коментари

Начинаещите са склонни да използват коментарите за да обяснят "какво става в кода". Като този:

```js
// Този код ще прави тези неща (...) и тези неща (...)
// ...и кой ли знае какво още...
много;
сложен;
код;
```

Но  в качественият код , количеството от тези "обясняващи" коментари трябва да бъде минимален. Кодът трябва да бъде лесен за разбиране.

Има страхотно правило за това: "Ако кодът е неясен и изисква коментар, тогава може би трябва да се пренапише".

### Рецепта: разделяйте кода на функции

Понякога е  полезно да заменим дадено парче код със функция, ето така:

```js
function showPrimes(n) {
  nextPrime:
  for (let i = 2; i < n; i++) {

*!*
    // проверяваме  дали i е просто число
    for (let j = 2; j < i; j++) {
      if (i % j == 0) continue nextPrime;
    }
*/!*

    alert(i);
  }
}
```

По - добрият вариант е като използваме отделна функция за `isPrime`:


```js
function showPrimes(n) {

  for (let i = 2; i < n; i++) {
    *!*if (!isPrime(i)) continue;*/!*

    alert(i);  
  }
}

function isPrime(n) {
  for (let i = 2; i < n; i++) {
    if (n % i == 0) return false;
  }

  return true;
}
```

Сега ние можем да разберем кодът лесно. Самата  функция се превръща в коментар. Такъв код се нарича *самодокументиращ се*

### Рецепта: създавайте функции

Ако имаме толкова дълъг код:

```js
// тук добавяме уиски
for(let i = 0; i < 10; i++) {
  let drop = getWhiskey();
  smell(drop);
  add(drop, glass);
}

// тук добавяме сок
for(let t = 0; t < 3; t++) {
  let tomato = getTomato();
  examine(tomato);
  let juice = press(tomato);
  add(juice, glass);
}

// ...
```

Тогава ще бъде по-добре да рефакторирате кода във функции по този начин:

```js
addWhiskey(glass);
addJuice(glass);

function addWhiskey(container) {
  for(let i = 0; i < 10; i++) {
    let drop = getWhiskey();
    //...
  }
}

function addJuice(container) {
  for(let t = 0; t < 3; t++) {
    let tomato = getTomato();
    //...
  }
}
```

Тук също не са необходими  коментари, фукнциите сами по себе си казват какво правят. Няма какво да се коментира.  Също така структурата на кода е по - добра разделена .  Ясно е какво прави всяка една функция ,  какъв вход приема и какво връща.

В действителност не можем напълно да избегнем "описателните" коментари. Съществуват сложни алгоритми. И има трикове за Но като цяло трябва да се опитаме да запазим кода прост и самодокументиращ се.

## Добри коментари

Така щом, описателните коментари обикновено са лоши. Кои коментари са добри?

Опишете архитектурата
: Provide a high-level overview of components, how they interact, what's the control flow in various situations... In short -- the bird's eye view of the code. There's a special language [UML](http://wikipedia.org/wiki/Unified_Modeling_Language) to build high-level architecture diagrams explaining the code. Definitely worth studying.

Document function parameters and usage
: There's a special syntax [JSDoc](http://en.wikipedia.org/wiki/JSDoc) to document a function: usage, parameters, returned value.

    For instance:
    ```js
    /**
     * Returns x raised to the n-th power.
     *
     * @param {number} x The number to raise.
     * @param {number} n The power, must be a natural number.
     * @return {number} x raised to the n-th power.
     */
    function pow(x, n) {
      ...
    }
    ```

    Such comments allow us to understand the purpose of the function and use it the right way without looking in its code.

    By the way, many editors like [WebStorm](https://www.jetbrains.com/webstorm/) can understand them as well and use them to provide autocomplete and some automatic code-checking.

    Also, there are tools like [JSDoc 3](https://github.com/jsdoc3/jsdoc) that can generate HTML-documentation from the comments. You can read more information about JSDoc at <http://usejsdoc.org/>.

Защо задачата е решена по този начин?
: Това което е написано, е важно. Но това, което *не* е написано може да бъде още по-важно за да разберете какво се случва. Защо задачата е решена по този начин? Кодът дава този отговор.

    Ако има много начини за решение на тази задача, защо точно този? Особено като този не е от най-очевидтните.

    Without such comments the following situation is possible:
    1. You (or your colleague) open the code written some time ago, and see that it's "suboptimal".
    2. You think: "How stupid I was then, and how much smarter I'm now", and rewrite using the "more obvious and correct" variant.
    3. ...The urge to rewrite was good. But in the process you see that the "more obvious" solution is actually lacking. You even dimly remember why, because you already tried it long ago. You revert to the correct variant, but the time was wasted.

    Comments that explain the solution are very important. They help to continue development the right way.

Any subtle features of the code? Where they are used?
: If the code has anything subtle and counter-intuitive, it's definitely worth commenting.

## Обобщение

Важен знак, че един разработчик е добър са коментарите: техната прецизност и дори отсъствието им.

Добрите коментари ни позволяват да поддържаме добре кода, и като се върнем към него след време да го използваме по ефективно.

**Коментирайте това:**

- Overall architecture, high-level view.
- Function usage.
- Important solutions, especially when not immediately obvious.

**Избягвайте коменатри:**

- Такива които ви казват " как работи кода " и "какво прави" .
- Ползвайте ги само тогава ако е невъзможно да направите кода лесен за четене и самоопистален, че да не ги изисква ..

Коментарите се ползват като инструменти за автоматично документиране като JSDoc3: те ги чеът и генерират HTML-docs ( или docs във друг формат).
