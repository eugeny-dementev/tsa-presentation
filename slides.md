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

# Что такое Tiger Style

Подход к написанию кода. Привносит новый взгляд на идеи проверенные временем

- 🤹 **KISS** - keep it simply silly
- 🧑‍💻 **DevEx** - developer experience
- 👶 **E2E** - end-to-end testing
- 📝 **TDD** - test driven development
- 🚀 **NASA** - 10 принципов надёжного кода
- 🛠 **Fail Fast** - если что-то пошло не так, ошибкуй

---

# Задачи разработки

- 🧑‍💻 Добавление изменений в код
- 🧩 Проверка что весь функционал работает как задумано
- 🐛 Дебаг багов на проде в условиях ограниченного контекcта
- 🌀 Расшифровка неочевидных stack traces
- 📋 Поддержка огромного кол-ва юнит тестов
- 🤦 Необходимость двойной работы, сначала код, потом тесты
- ⏱️  Отсутствие мгновенной обратной связи на внесение изменений в код

<!--
Есть множество проблем/задач с которыми приходится сталкиваться в процессе поддержки продукта. Существующие решения безусловно облегчают работу но так же привносят свои ограничения и проблемы с которыми часто приходится боросться отдельно что в итоге может привести к двойной работе
-->

---

# Unit tests

- `lib.ts`
```ts
function foo(value: number): string {
  if (value === 5) return 'bar';

  throw new Error('Invalid value');
}
```

- `lib.unit.ts`
```ts
describe('foo', ()=>{
  it('should return foo on 5',()=>{
    assert(foo(5) === 'bar', 'should return bar');
  });
  it('should return foo on 6',()=>{
    assert.throws(foo(6), 'should throw on 6');
  });
});
```

<!--
Синтетический пример того как обычно выглядит проект. Код с какой-то функциональностью которую нам надо проверить и юнит тесты которые проверяют эту функциональность. При этом качество юнит тестов условно зависит от фантазии автора на момент написание и не гарантирует покрытие всех возможных крайних случаев
-->
---

# Ключ момент

Самая полезная часть

```ts
assert(foo(5) === 'bar', 'should return bar');
assert.throws(foo(6), 'should throw on 6');
```

- 🟢 Плюсы
  - ✅ Даёт понимание если что-то не так
- 🔴 Минусы
  - ⛔ Полезна только на этапе разработки

<!--
Юнит тесты могут быть очень детальны и проверять специфичные кейсы. Но даже в условиях когда их тысячи нет гарантии что покрыты все тесты. Баги на проде не исчезают сколько тестов не придумывай. Плюс юниты могут быть дублировны тестами интеграции отдельно написанными на основе опыта полученного от дебага ошибок на проде. Польза юнитов очевидна но при этом мы всегда ограничеваем эту пользу контролируемой средой где всё в большей степени предсказуемо и работает так как мы задумали. На проде же наш код остаётся без условной защиты и если что-то пойдет не так, а оно всегда идёт не так, нам приходится ко крупицам собирать и догадываться что же произошло и на основе этого делать фикс и придумывать тест чтобы такого больше не повторилось
-->


---

# Tiger Style Assertions

- `lib.ts`

```ts
import { assert } from 'tiger-assert';

function foo(value: number): string {
  assert(value === 5, "value must be 5", { value });

  let result = 'bar';

  assert(result === 'bar', 'result must be "bar"', { value, result })

  return result;
}
```

<!--
Почему бы не взять эту пользу и не поместить её туда где в ней самая большая необходимость. Самый быстрый способо ощутить пользу от такого подхода можно при разработки новой фичи. Пишим новый код, сразу фиксируем его ограничения, валидируем поток данных и ождиния который у нас есть от кода который пишем
-->

---

# Более живой пример

```ts
function emitIceCandidateToCaller(candidate: RTCIceCandidate, targetSocketId: Socket['id']) {
  assert.isString(targetSocketId, 'targetSocketId should be a non-empty Socket["id"] string');
  assert.is(targetSocketId, this.store.callerSocketId, 'should only emit ice candidates to caller');

  const payload: IceCandidateFront = {
    ...frontToBack,
    candidate,
    targetSocketId,
  };

  console.log(`Emitting ${SIGNALING_EVENT.ICE_CANDIDATE_FROM_CALLEE}`, payload);

  this.socket.emit(SIGNALING_EVENT.ICE_CANDIDATE_FROM_CALLEE, payload);
}
```

---

# Проверка параметров метода либы

```ts
function addIngredient(ingredient) {
  assert.oneOf(ingredient, Object.values(Ingredient), 'Ингредиент должен быть одим из значений объекта Ingredients');

  this.#ingredients.push(ingredient);

  return this;
}
```

---

# Почему это работает

- 📚 Документация сразу в коде
- 🧹 Уменшение необходимости чрезмерного юнит тестирования (e2e/интеграция)
- ⚡ Моментадльная обратная связь о некорректном использовании кода при внесении изменений
- 🐅 Легкость внесения изменений в код в условиях постоянно меняющихся требований от продукта
- 🛠️ Контекст для дебага
- 💪 Уверенность в коде

<!--
Assertions сразу говорят как код предполагается использовать, не как он работает. Так же можно расставлять assert на инварианты, когда мы говорим как код точно не предполагается использовать. Документация которая сама выскочет и расскажет о себе моментально при попытке неправильного использования кода. Такую не требуется допольнительных усилий чтобы читать. Контекст для дебага сразу приложенный к ошибке с данными вызвавшими проблему. Когда код пыполняется и ни один assert не срабатывает это дает уверенность в том что всё работает так как надо. Плюс даже если случится крайний случай, то будет предоставлен полезный контекст и возможность воспроизвести и найти баг с меньшими усилиями
-->

---

# Где использовать

- 🔍 Валидация ключевых точек логики и мест обработки данных
- 🚦 Убедится что код выполняется только в корректных условиях
- ✅ Валидация переданной конфигурации
- 🕵️ Валидация коннектов с сервисами
- 🐞 Дебаггинг на ходу с авто включаеммыми брейкпоинтами

```ts
funciont assertDebug(truth: boolean, msg: string, context: object) {
  if (!truth) {
    console.log(message);
    console.log(context);
    debugger
  }
}
```


<!--
"Assertions привносит строгость в поток выполнения/использования кода через валидацию данных и условий при этом не фиксируя реализацию кучей отдельно лежажих юнит тестов. Если нужно что-то поменять, ничего не нужно искать, вся ключавая логики и ограничения доступны прямо на месте и позволяют легче принимать решения о внесении изменений"
-->

---

# Для ознакомления

- https://github.com/tigerbeetle/tigerbeetle/blob/main/docs/TIGER_STYLE.md
- https://tigerstyle.dev
- https://eugeny-dementev.github.io/tigerstyle/

