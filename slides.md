---
# You can also start simply with 'default'
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
# background: https://cover.sli.dev
# some information about your slides (markdown enabled)
title: Tiger Style
info: |
  ## Slidev Starter Template
  Assertions
# apply unocss classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: fade-out
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
---

# Tiger Style

Assertions

---
transition: fade-out
---

# Проблема

Задачи разработки

- 🧩 Проверка что весь функционал работает как задумано
- 🐛 Дебаг багов на проде в условиях ограниченного контекcта
- 🌀 Расшифровка неожевидных stack traces
- 📋 Поддержка огромного кол-ва юнит тестов
- 🤦 Необходимость двойной работы, сначала код, потом тесты
- ⏱️  Отсутствие мгновенной обратной связи на внесение изменений в код

<!--
Есть множество проблем с которыми приходится сталкиваться в процессе поддержки продукта. Существующие решения безусловно облегчают работу но так же привносят свои ограничения и проблемы с которыми часто приходится боросться отдельно что в итоге может привести к двойной работе
-->

---

# Unit tests

- `lib.ts`
```ts
function foo(value: number): string {
  if (value === 3) return 'foo';
  if (value === 5) return 'bar';

  throw new Error('Invalid value');
}
```

- `lib.unit.ts`
```ts
describe('foo', ()=>{
  it('should return foo on 3',()=>{
    assert(foo(3) === 'foo', 'should return foo');
  });

  it('should return foo on 5',()=>{
    assert(foo(5) === 'bar', 'should return bar');
  });

  it('should return foo on 6',()=>{
    assert.throws(foo(6), 'should throw on 6');
  });

  it('should return foo on 7',()=>{
    assert.throws(foo(7), 'should throw on 7');
  });
});
```


---

# Что такое Tiger Style

Подход к написанию кода, превносящий новые идеи в проверенное временем

- 🤹 **KISS** - keep it simply silly
- 🧑‍💻 **DevEx** - developer experience
- 👶 **E2E** - end-to-end testing
- 📝 **TDD** - test driven development
- 🚀 **NASA** - 10 принципов надёжного кода
- 🛠 **Fail Fast** - если что-то пошло не так, ошибкуй
---

# Решение

Assertions

- 🛠️  Использование `assert(...)` в местах где нужно:
  - 🔍 Валидация ключевых предположений о данных и логике
  - 🚦 Убедится что код выполняется только в корректных условиях
  - 🗒️ Предоставление ползеного контекста для дебагинга ошибок
- 🌟 Полезности:
  - 🧹 Уменшение необходимости чрезмерного юнит тестирования (e2e/интеграция)
  - ⚡ Моментадльная обратная связь о некорректном использовании кода
  - 📚 Документация сразу в коде

<!--
"Assertions привносит строгость в поток выполнения/использования кода через валидацию данных и условий при этом не фиксируя реализацию кучей отдельно лежажих юнит тестов. Если нужно что-то поменять, ничего не нужно искать, вся ключавая логики и ограничения доступны прямо на месте и позволяют легче принимать решения о внесении изменений"
-->
---

# Assertions

- `lib.ts`

```ts
import { assert } from 'tiger-assert';

function foo(value: number): string {
  assert([3, 5].includes(value), "value must be either 3 or 5", { value });

  let result = 'bar';
  if (value === 3) result = 'foo';

  assert(['bar', 'foo'].includes(result), 'result must be either "bar" or "foo"', {
    value, result,
  })

  return result;
}
```

---

# Draggable Elements

Double-click on the draggable elements to edit their positions.

<br>

###### Directive Usage

```md
<img v-drag="'square'" src="https://sli.dev/logo.png">
```

<br>

###### Component Usage

```md
<v-drag text-3xl>
  <div class="i-carbon:arrow-up" />
  Use the `v-drag` component to have a draggable container!
</v-drag>
```

<v-drag pos="663,206,261,_,-15">
  <div text-center text-3xl border border-main rounded>
    Double-click me!
  </div>
</v-drag>

<img v-drag="'square'" src="https://sli.dev/logo.png">

###### Draggable Arrow

```md
<v-drag-arrow two-way />
```

<v-drag-arrow pos="67,452,253,46" two-way op70 />

---
src: ./pages/imported-slides.md
hide: false
---

---

# Monaco Editor

Slidev provides built-in Monaco Editor support.

Add `{monaco}` to the code block to turn it into an editor:

```ts {monaco}
import { ref } from 'vue'
import { emptyArray } from './external'

const arr = ref(emptyArray(10))
```

Use `{monaco-run}` to create an editor that can execute the code directly in the slide:

```ts {monaco-run}
import { version } from 'vue'
import { emptyArray, sayHello } from './external'

sayHello()
console.log(`vue ${version}`)
console.log(emptyArray<number>(10).reduce(fib => [...fib, fib.at(-1)! + fib.at(-2)!], [1, 1]))
```

---
layout: center
class: text-center
---

# Learn More

[Documentation](https://sli.dev) · [GitHub](https://github.com/slidevjs/slidev) · [Showcases](https://sli.dev/resources/showcases)

<PoweredBySlidev mt-10 />
