# Аудит узгодженості початкової специфікації курсу

Курс: Kotlin for Android Fundamentals
Дата аудиту: 2026-10-29
Тип артефакту: рев'ю (не canonical artifact; нічого з канонічних файлів не змінено)

## 1. Межі перевірки

### 1.1 Версія та стан репозиторію

| Параметр | Значення |
|---|---|
| Commit SHA | `8056e3af15b22cde4b48cc808512226afa6bec58` ("Add initial course structure docs") |
| Гілка | `main` |
| Working tree на початку аудиту | чистий (`git status --short` порожній) |
| Repository instructions | не знайдено (`CLAUDE.md`, `AGENTS.md`, інструкції відсутні); `README.md` порожній |

### 1.2 Перевірені файли

| Файл | Розмір / рядків | Стан |
|---|---|---|
| `docs/01-course-contract.md` | 966 рядків, v0.1.0, Draft | непорожній |
| `docs/02-course-map.yaml` | 1665 рядків, v0.1.0, draft | непорожній, парситься |
| `docs/03-concept-dependency-map.yaml` | 2657 рядків, v0.1.0, draft | непорожній, парситься |

Інші файли каркаса, що стосуються перевірки, **порожні (0 байт)**: `curriculum/course-map.yaml`, `curriculum/dependency-map.yaml`, `curriculum/assessment-map.yaml`, `curriculum/lessons/lesson-01..14.md`, `docs/learning-outcomes.md`, `docs/scope.md`, `docs/pedagogy.md`, `docs/terminology.md`, `docs/audience.md`, `docs/course-vision.md`, усі `docs/decisions/*` (README та ADR-001..003), `continuity/*`, `specifications/*`.

### 1.3 Метод і обмеження

- YAML розібрано `PyYAML 6.0.3` (`safe_load`); перевірки (унікальність ID, посилання, цикли, порядок introduced_in, підсумки часу) виконано скриптами поза репозиторієм; решту — читанням усього тексту трьох файлів.
- Перевірялася узгодженість **внутрішня** та **між** трьома артефактами. Технічні твердження про Kotlin/Compose/AndroidX API **не** звірялися з офіційною документацією (поза межами цього етапу); вони позначені як припущення, де від них залежить висновок.
- Деталізованих специфікацій уроків немає (`curriculum/lessons/*` порожні), тому не перевірялися хвилинні розкладки, конкретні вправи та код. Це обмежує розділ D (див. F-13).
- Контракт не має стабільних ID для outcomes (див. F-21), тому в coverage matrix використано **позначення рецензента** (P1…F8 за порядком пунктів контракту §5.1–§5.6).
- Класифікація кожного finding: **[помилка]** — підтверджена суперечність/порушення; **[ризик]** — імовірна проблема, що залежить від ще не написаного змісту; **[рішення]** — відкрите рішення автора.

## 2. Verdict

**NOT READY** — для переходу до генерації уроків і похідних артефактів. Для самого задуму курсу вердикт ближчий до *READY WITH CONDITIONS*.

Обґрунтування:

- Загальна траєкторія курсу узгоджена з контрактом: 14 занять × 120 хв = 1680 хв; усі урочні суми по 120 хв; підсумки 820 + 860 = 1680 збігаються з оголошеними; граф `requires` не має циклів; жодна залежність не вказує на пізніший урок; ID унікальні; виключені теми (Hilt, Room, coroutines, DI, репозиторії) не є явними prerequisites.
- Але артефакти **порушують власні валідаційні правила** (GV-03, GV-04, CDM-04 і `generation_failure_conditions`): 40 concept ID з карти курсу не існують у dependency map (F-01), а одна залежність L10 вказує на концепт, введений у самому L10 (F-02). За правилами самих артефактів, будь-який згенерований урок, що торкається цих ID, невалідний.
- Є кілька прихованих залежностей (лямбди, `remember`/`by`, nullable, механізм оновлення дошки, API стану ViewModel), для яких «ранній preview» не оформлений так, як вимагає контракт §15.2 (F-04…F-08).

Підсумок за severity: BLOCKER — 1; MAJOR — 15; MINOR — 6; SUGGESTION — 3.

## 3. Findings

Формат: ID · severity · клас · місце · доказ · наслідок · мінімальна рекомендація.

### 3.1 BLOCKER

**F-01 · BLOCKER · [помилка] · 40 concept ID карти курсу не резолвляться в dependency map**
- Місце: `docs/02-course-map.yaml:1644-1645` (GV-03), `docs/03-concept-dependency-map.yaml:2635-2636` (CDM-04), `:2650-2652` (failure condition «unresolved concept ID»).
- Доказ: з 202 записів `new_concepts` у карті курсу 40 відсутні у `concepts` і `supporting_concepts` (повний список — Додаток A). Серед них такі, що несуть педагогічну вагу: `trailing-lambda` (`02-course-map.yaml:884`), `data-object` (`:992`), `null-check` (`:775`), `state-read`/`state-write` (`:782-783`), `collect-or-read-viewmodel-state` (`:1203`), `viewmodel-acquisition` (`:1201`), `separation-of-concerns-intro` (`:989`), `git-checkpoint-workflow` (`:1401`), `named-argument`/`default-argument` (`:466-467`). Додатково: `smart-cast-intro.requires` посилається на неіснуючий `null-check` (`03-concept-dependency-map.yaml:1377`); у `package-by-feature-intro.reinforces` — неіснуючий `separation-of-concerns-intro` (`:2150`); L14 `prerequisites.concepts` містить `student-final-project-alpha` — це ID артефакту, а не концепту (`02-course-map.yaml:1475`) та `separation-of-concerns-intro` (`:1477`).
- Наслідок: за `generation_failure_conditions` жоден урок, який покладається на ці ID, не може пройти перевірку; граф залежностей для 40 концептів (зокрема `trailing-lambda`, `data-object`, `null-check`) не можна перевірити.
- Рекомендація: для кожного з 40 ID автор вирішує — додати запис у dependency map (з `requires`/`introduced_in`/`target_level`) чи вилучити/перейменувати в карті курсу; ID артефактів (`student-final-project-alpha`) винести з простору концептів.

### 3.2 MAJOR

**F-02 · MAJOR · [помилка] · `immutable-component-input`: L08 у карті курсу, L10 у dependency map, але L10 вимагає його як prerequisite**
- Місце: `02-course-map.yaml:895` (new у L08), `:1077` (prerequisite L10); `03-concept-dependency-map.yaml:1980` (`introduced_in: L10`).
- Доказ: скрипт порівняння `introduced_in` дав єдиний розбіг саме тут; GV-04 («кожен prerequisite введений в **ранішому** уроці») порушено для L10.
- Наслідок: невизначено, де вводиться концепт; L10 формально залежить від самого себе.
- Рекомендація: обрати один урок (у dependency map його `requires` — `recomposition-basic-model` L07, `parameterized-composable` L04, `data-class` L05 — тож технічно можливі L08 та L10) і вирівняти обидва файли.

**F-03 · MAJOR · [помилка/ризик] · Лямбди та trailing-lambda використовуються з L01–L06, формально вводяться в L08**
- Місце: `02-course-map.yaml:156` (`onclick-callback-intro`, L01), `:258-259` (`Column`/`Row`, L02), `:358` (`repeat-function`, L03), `:451,470` (callback-параметр, L04), `:671-676` (`list-builder-intro`, `lazy-list-intro`, `stable-item-key-intro`, L06); `:880-884` (`lambda-expression`, `function-type`, `trailing-lambda` — L08). `03-concept-dependency-map.yaml`: `onclick-callback-intro` має рівень `recognize`; `callback-parameter-intro` (L04) не `requires` `function-type`; `callback-parameter-intro.avoid_before: higher-order-function-detail`.
- Доказ: `Button(onClick = { … }) { Text(…) }`, `Column { … }`, `repeat(n) { i -> … }`, `items(list) { cell -> … }`, `List(n) { … }`, параметр типу `() -> Unit` — це все синтаксис лямбд. Карта явно відкладає лише «Function-type syntax in depth» (`:481`) і «Higher-order function terminology» (`:480`), але не описує, **що** студент має розуміти про `{ … }` до L08 і **коли** це буде пояснено повністю — чого вимагають контракт §15.2 та правило dm `:33-34`.
- Наслідок: генератор або тихо використає лямбди (порушення §15.2), або обходитиме їх нестабільно (різний синтаксис у різних уроках); L04 домашня/вправа «Pass a click callback» залежить від лямбда-синтаксису.
- Рекомендація: додати формальні preview-записи (див. розділ 6.1): що студент розуміє («блок коду, який виконається пізніше/для кожного елемента»), що отримує як scaffold (підпис `() -> Unit`, форма `items { }`), в якому уроці пояснюється повністю (L08). Порядок уроків не змінювати.

**F-04 · MAJOR · [помилка/рішення] · `remember`/`mutableStateOf`/`by`: стан потрібен з L01, вводиться в L07; синтаксис `by` не визначено**
- Місце: `02-course-map.yaml:183-185` (L01: «State-related code may initially be treated as provided infrastructure»), `:284-289` (L02 лаб: реакція на клік), `:495-501` (L04 лаб: «enabled or selected state»), `:779-783` (state вводиться в L07); dm: `remember` `:1458`, `mutable-state-of` `:1473`, `state-intro` `:1441`; `delegation` у deferred `:2590`; контракт §9.1 `:442` («delegated properties as a language topic» виключені).
- Доказ: рядок про «provided infrastructure» є лише в L01; для L02–L06 аналогічного запису немає, хоча лаби L02 («Respond to a button click») та інтерактивні елементи L04–L06 потребують змінюваного стану. Концептів `by`, `.value`, `getValue/setValue` у dm немає взагалі. Типовий запис `var count by remember { mutableStateOf(0) }` використовує делегат, який контракт відносить до виключених.
- Наслідок: 6 уроків (L01–L06) спираються на невизначений scaffold; у L07 студент зустріне `by` без введення делегатів.
- Рекомендація: задокументувати scaffold стану для L01–L06 (що студент бачить, чого не повинен розуміти, коли пояснюється); рішення автора D-02 — `by` чи `.value` (у другому випадку делегати не потрібні).

**F-05 · MAJOR · [ризик] · Nullable/enum-подібні значення в моделі до їх введення в L07**
- Місце: `02-course-map.yaml:587-601` (L05: «Model players, positions, and cells», «Render at least two visual cell states»), `:623-625` (домашня: «value representing its current content»), `:698` (L06 sandbox: «Find a cell by position using a loop»), `:806` (L07: «Define a Player enum»); dm: `nullable-type` `:1319` (L07), `enum-class` (L07).
- Доказ: у L05/L06 клітинка має «стан» і «вміст», але тип вмісту не визначено; пошук клітинки циклом за позицією природно повертає «не знайдено» (тобто null), а `Player` в L05 — клас, у L07 — enum.
- Наслідок: приховані залежності від nullable та enum; або штучні заміни (рядок `""`, `' '`, Boolean), які потім треба явно рефакторити.
- Рекомендація: у специфікації L05–L06 задати тимчасове представлення вмісту (або обмежити завдання, щоб «не знайдено» не виникало) і явно позначити рефакторинг у L07 (контракт §15.4).

**F-06 · MAJOR · [ризик] · Механізм оновлення дошки в L07 не визначено**
- Місце: `02-course-map.yaml:809-816` (L07 лаб: клік змінює клітинку; дошка — `List<Cell>` з L06 `:653,711-713`), `:825-830` (misconceptions L07 не містять «зміна MutableList у state не оновлює UI»); dm: `immutable-list-update` `:1621` (L08), `mutable-list` (L06, `apply-guided`); `mutableStateListOf` відсутній у обох файлах.
- Доказ: L06 фіксує `List<Cell>` як єдине представлення дошки; L08 вводить `immutable-list-update` і `copy` для оновлення. Для L07 жоден артефакт не вказує, як саме змінюється дошка при кліку.
- Наслідок: lab L07 або тихо покладається на L08 (`map`/`copy`), або на `MutableList` у `remember` (відомий клас помилок рекомпозиції), або на не введений `mutableStateListOf`.
- Рекомендація: рішення автора D-05 щодо механізму; якщо використовується допоміжна функція — позначити її як named scaffold, яку L08 замінює навчальним кодом.

**F-07 · MAJOR · [рішення] · API стану ViewModel і прихована залежність від Flow/lifecycle**
- Місце: `02-course-map.yaml:1190-1204` (L11 new_concepts: `private-mutable-public-readonly-state`, `viewmodel-lifecycle-intro`, `viewmodel-acquisition`, `collect-or-read-viewmodel-state`), `:1218` («StateFlow unless selected by implementation baseline»); dm `:2602-2606` (`stateflow` deferred, «may be used by an advanced implementation baseline»), `:2645` (CDM-07); контракт `:451` (coroutines і Flow не є required outcomes), `:470` (lifecycle internals виключені); `02-course-map.yaml:166` («Activity lifecycle» відкладено в L01).
- Доказ: «collect» у назві концепту та застереження про StateFlow залишають відкритим вибір між Compose-станом у ViewModel (без Flow) та `StateFlow`+`collectAsState()` (Flow як чорна скринька). `viewmodel-acquisition` (`viewModel()`) може вимагати Gradle-залежності, а `viewmodel-lifecycle-intro` — розуміння перестворення Activity, якого курс не вводить.
- Наслідок: матеріали L11 можуть тихо вимагати Flow; мотивація ViewModel «виживає при повороті екрана» суперечить відкладеному lifecycle.
- Рекомендація: ADR (D-01) — зафіксувати один API стану для core path; якщо Flow — оформити як preview з явним scaffold-описом; Gradle-залежність для `viewModel()` — дати як готову.

**F-08 · MAJOR · [рішення] · Перемикання екранів і навігація**
- Місце: контракт `:417` (навігація «лише в обсязі, потрібному фінальному проєкту»), `:936` (відкрите: бібліотека чи мінімальне явне перемикання); `02-course-map.yaml:1204` (`route-vs-screen-intro`), `:1303` (`navigation-boundary-intro`), `:1313` (відкладено «Complex Navigation Compose graphs»), `:1598` (M-D: «Supports setup, play, and result states»), `:1331-1336`; dm `screen-flow` `:2212`.
- Доказ: термін «route» належить Navigation Compose; відкладення лише «complex graphs» припускає, що простий варіант бібліотеки допустимий. Водночас dm `screen-flow` вимагає `application-ui-state` (sealed) — що відповідає явному перемиканню станом, але ніде прямо не зафіксовано. Крім того, M-D вимагає setup-стан для всіх, хоча контракт §10.2 не вимагає його для мінімального capstone.
- Наслідок: неузгоджене використання navigation framework або його відсутність у різних уроках; розширення обов'язкового обсягу відносно контракту (§8.3).
- Рекомендація: рішення D-03 (явне перемикання станом vs бібліотека) і D-10 (чи setup/handover обов'язкові для всіх проєктів); прибрати термін «route» з core, якщо бібліотека не обрана.

**F-09 · MAJOR · [помилка] · Cumulative checkpoints: 4 milestones замість 5, оцінювання не визначено**
- Місце: контракт `:726-736` (п'ять точок: foundations; data modeling & collections; state-driven Compose; ViewModel & architecture; final project readiness); `02-course-map.yaml:1560-1597` (M-A L04, M-B L07, M-C L10, M-D L12); `curriculum/assessment-map.yaml` — 0 байт.
- Доказ: п'ятого checkpoint «final project readiness» немає; milestones мають лише `required_evidence` (перелік можливостей) без способу перевірки та без вимоги оцінювати **збережені знання й інтеграцію** (контракт `:736`). Assessment map порожній.
- Наслідок: невизначені cumulative checks, перед L13 немає оцінки готовності до самостійного проєкту.
- Рекомендація: додати п'ятий checkpoint (місце — рішення автора D-11) і описати спосіб перевірки в assessment map.

**F-10 · MAJOR · [помилка] · Немає оцінювання «малої зміни/налагодження без копіювання рішення»**
- Місце: контракт `:192` (outcome 5.5: «make a small architecture-preserving change»), `:748-749` (capstone dimension), `:757-758` (критерій успіху); `02-course-map.yaml:1548-1554` (assessment L14), `:1451-1456` (L13).
- Доказ: жоден `assessment.evidence` у L11–L14 не містить виконання нової невеликої зміни/дебагу в готовому проєкті; evidence L14 — демонстрація, пояснення, трасування, опис одного бага, обґрунтування одного рефакторингу.
- Наслідок: обов'язковий критерій успішного завершення (§13.5) та outcome 5.5 не мають перевірки.
- Рекомендація: додати в evidence L14 (або L13) завдання на невелику зміну/діагностику у проєкті з обмеженнями (без заміни проєкту).

**F-11 · MAJOR · [помилка] · Outcomes контракту без розміщення в карті курсу (Git, Gradle, IDE-інструменти, stack traces)**
- Місце: контракт `:131` (stack traces), `:160` (навігація IDE, completion, formatting, рефакторинг), `:162` (Gradle), `:163` + `:108` (Git); `02-course-map.yaml:165` (Gradle лише у `explicitly_deferred`), `:1401` (`git-checkpoint-workflow` лише в L13, не резолвиться — F-01), `:1495-1500` (`refactoring` лише в L14 як програмна техніка).
- Доказ: у dm немає концептів про IDE-навігацію/форматування/refactor-інструменти, залежності Gradle, stack trace. Git вводиться вперше в L13 (виконавчий workshop) без практики й оцінювання раніше; «dependencies at a user level» (§8.4) не представлені.
- Наслідок: частина контрактних outcomes не має навчання/практики/оцінювання; прихована потреба в Gradle-залежності для ViewModel (F-07).
- Рекомендація: рішення D-06 — де навчаються ці навички (інтегровано в існуючі уроки — зі списком у `new_concepts` tooling), або явно зменшити формулювання outcomes у контракті (мажорна версія за §18.3).

**F-12 · MAJOR · [ризик] · Домашня робота несе матеріал, від якого залежить наступний урок**
- Місце: `02-course-map.yaml:623-625` (L05: поле «current content» у `Cell` — вхід для L06/L07), `:839-841` (L07: «Add turn switching…» дублює вимоги лабораторної `:814-816` «display current player», «prevent replacing»), `:948-950` (L08: `empty cells`/`board fullness` — потрібні для draw в L09 `:1029`), `:1358-1360` (L12: «final project proposal» — вхід для L13), `:1458-1460` (L13: «Complete missing required features»); контракт `:938` (обсяг домашніх завдань — відкрите рішення).
- Доказ: у кожному випадку наступний урок або lab фактично припускає, що домашнє виконане; рішення про це не зафіксовано.
- Наслідок: студенти, що пропустили домашнє, не мають шляху core-progression; неочевидне, чи це «Practice», чи обов'язкова частина (§12.2).
- Рекомендація: для кожного з п'яти випадків або перенести обов'язкову частину в урок, або зафіксувати як свідому залежність і fallback (стартовий код наступного уроку).

**F-13 · MAJOR · [ризик] · Перевантаження уроків (педагогічний ризик)**
- Місце: `02-course-map.yaml:127-157` (L01 — 26 нових концептів; сума по картах: L01 26, L08 18, L03 17, L07 17, L02/L06 16, L09 15, L04 15, L05 14); таймінг `:82-95`.
- Доказ (факти): суми часу коректні; кількість нових концептів на урок — у блоці `new_concepts`. Оцінки (інтерпретація): L01 — 26 концептів при 30 хв Android/Compose, куди входять запуск емулятора, структура проєкту, Text/Button і лаб лічильника зі станом; L08 — лямбди, function types, trailing lambda, шість операцій над колекціями та immutable update на рівні `apply-independent` за 60+60 хв; L09 — interface, sealed, data object, extension, state machine, rule validation, terminal state і повна гра хрестики-нулики за 50/70 хв; L12 — 11 нових концептів, включно з `adaptive-layout-awareness`, `dialog-or-overlay`, `accessibility-basics` при відкладенні «Advanced adaptive layouts» (`:1317`).
- Наслідок: це **ризик**, не доведений факт; без хвилинних розкладок у специфікаціях уроків (їх ще немає) його не підтвердити і не спростувати.
- Рекомендація: у специфікаціях уроків для L01, L08, L09, L12 явно розділити Core і Practice/Challenge/Deep Dive (контракт §12.2), позначити, що з `new_concepts` є `recognize` без практики.

**F-14 · MAJOR · [ризик] · Capstone: реалістичність L13–L14 та відмінності типів ігор**
- Місце: `02-course-map.yaml:1384-1391,1426-1438` (L13: визначення обсягу + реалізація в одному уроці), `:1458-1460`, `:1599-1618` (варіанти); контракт `:531-552`; dm `deferred_concepts` `coroutines`.
- Доказ/аналіз (інтерпретація): типи проєктів мають різні архітектурні профілі, яких карта курсу не розрізняє:

| Профіль | Приклади | Гравці/ходи | Прихована інформація | Особливості, що виходять за core |
|---|---|---|---|---|
| Головоломка | Fifteen puzzle | немає гравців, лічильник ходів | немає | `Player.kt` з контракту `:608-609` недоречний; перемішування (розв'язність), термінал «solved» |
| Відкрита покрокова | Tic-Tac-Toe, Connect Four, Reversi | 2 гравці, черговість | немає | перевірка легальності ходу, перемога у кількох напрямках (L09 challenge) |
| Hot-seat з прихованою інформацією | Battleship, (Memory у режимі 2 гравців) | 2 гравці | так | handover (L12 `:1336` «applicable»); фаза розстановки; дві дошки |
| Memory | Memory matching | 1–2 | приховані картки | повернення карток після невдалої пари зазвичай потребує затримки → coroutines/`delay`, виключені контрактом |

Базова гра (хрестики-нулики) не має прихованої інформації, тому handover (L12) лише моделюється в sandbox і не відпрацьовується в core-лабораторії.
- Наслідок: L13 вимагає й визначення обсягу, і реалізації, і відлагодження в 120 хв; обов'язкові features (`:1428-1437`) не адаптовані до профілів; для Memory і Battleship є ризик вимоги недозволених технік.
- Рекомендація: рішення D-04: профілі/рівні варіантів, мінімальний набір features на профіль, затвердження обсягу до L13 (не в самому L13), політика «затримки» для Memory (напр., підтвердження натисканням).

**F-15 · MAJOR · [рішення] · Спільний навчальний приклад і власний capstone не розділені; перелік варіантів розходиться з контрактом**
- Місце: `02-course-map.yaml:1605` (`baseline_game: Tic-Tac-Toe`), `:1609` (той самий Tic-Tac-Toe у `suggested_variants.basic`), `:1613-1616` («Simplified Reversi» як intermediate і «Reversi» як advanced); контракт `:533-539` (стандартні варіанти включають Tic-Tac-Toe), `:543-548` (advanced: Reversi, checkers, Battleship; рівня intermediate немає), `:937` (чи обирається гра індивідуально — відкрите), `:758` (без заміни проєкту скопійованим рішенням).
- Доказ: студент, що обрав Tic-Tac-Toe, здає ту саму гру, яку весь курс будував разом; відмінність між «спільним прикладом» і «власним проєктом» тоді не перевіряється. Connect Four у контракті — стандартний, у карті — intermediate.
- Наслідок: неоднозначність оцінювання (§13.4) і суперечність контракту та карти (контракт має пріоритет за §18.4).
- Рекомендація: D-04 — чи дозволений Tic-Tac-Toe як capstone (за яких умов відрізняється від baseline); вирівняти рівні варіантів з контрактом (або оновити контракт).

**F-16 · MAJOR · [ризик/рішення] · Розподіл відповідальностей не доведено до однозначності**
- Місце: контракт `:601-621` (`GameRules.kt`, `GameUiState.kt`, `Player.kt`), `:596` (ViewModel застосовує дію через game logic); `02-course-map.yaml:998-1000` (`game-engine-intro`, `pure-rule-function`, `ui-logic-separation` у L09), `:1086` (`GameUiState` — data class у L10), `:1288-1298` (`application-ui-state`, `screen-mode` у L12), `:1260` (L11 challenge: «Extract a standalone GameEngine»), `:1056` (L09 домашня: тести); `:941` контракту (покриття тестами — відкрите); dm: `unit-test-intro` згадується в `enables` (`pure-rule-function`), але не існує.
- Доказ: (а) «game rules» (контракт), «game engine» (карта), «GameEngine» як окремий challenge в L11 — різні назви/статус для одного шару; якщо стандартом після L09 вже є pure rule functions, challenge L11 виглядає суперечливим. (б) `GameUiState` (дані для екрана) і `application-ui-state` (режими екрана) — не зв'язані: чи вкладається одне в інше, не сказано. (в) Де живуть правила в L09–L10 (Screen?) і що саме переходить у ViewModel в L11 — не зафіксовано. (г) Тести (L09 homework, L11 challenge) використовуються без інфраструктури й без рішення про автоматичні тести.
- Наслідок: різні уроки можуть описувати різні межі model/rules/UiState/ViewModel/Screen/Components.
- Рекомендація: таблиця відповідальностей (хто що володіє) в ADR-003; єдина назва шару правил; рішення про тести (D-09).

### 3.3 MINOR

**F-17 · MINOR · [рішення] · Порядок рефакторингу в контракті §11.5 не збігається з картою**
- Місце: контракт `:646-654` (локальна поведінка → модель → піднятий стан → екран зі станом → **винесена логіка** → ViewModel → компоненти) vs `02-course-map.yaml:952-1059` (логіка винесена в L09 **до** state hoisting в L10).
- Рекомендація: з'ясувати, чи перелік §11.5 нормативний (тоді змінити контракт/карту) чи ілюстративний (позначити).

**F-18 · MINOR · [помилка] · Розбіжності ідентичності та мови**
- `docs/01-course-contract.md:6` (`kotlin-android-fundamentals`) vs `docs/02-course-map.yaml:14` (`kaf`); `02-course-map.yaml:16` фіксує `language_of_instruction: "uk"`, тоді як контракт `:340-346,:940` залишає мову і білінгвальність відкритими.

**F-19 · MINOR · [помилка] · Дублікати та порожні канонічні місця**
- `curriculum/course-map.yaml`, `dependency-map.yaml`, `assessment-map.yaml` (0 байт) дублюють призначення `docs/02-…` та `docs/03-…`; `docs/learning-outcomes.md` порожній; `docs/decisions/*` порожні при відкритих рішеннях (контракт §18.2 `:895-903`). Це не суперечливий контент, але канонічне місце неоднозначне.

**F-20 · MINOR · [помилка] · Схема dependency map**
- `supporting_concepts` (`:2485-2565`) не мають `domain`/`target_level`/`enables`, на відміну від `concepts`.
- 11 концептів мають `domain`, що відрізняється від секції `new_concepts` у карті курсу (`build-run-cycle`, `domain-model`, `component-model-boundary`, `board-as-data`, `minimum-viable-scope`, `edge-case-analysis`, `manual-test-scenario`, `refactoring`, `code-smell-intro`, `regression-check`, `technical-explanation`).
- `usage_rules` (`:37`) згадує маркер `recognition_only`, якого немає в даних (використано рівень `recognize`); маркера `optional` немає взагалі, хоча правило `:35` посилається на optional-концепти.
- 4 концепти dm відсутні в `new_concepts` карти: `collection-transformations` (L08; його вимагає L09 `:970`), `final-project-architecture` (L13), `encapsulation-intro` (L05), `collection-iteration` (L06).
- `enables` не валідується: 31 пара `enables` вказує на неіснуючий ID (24 унікальні цілі: артефакти/можливості, напр. `static-board-prototype`, `unit-test-intro`), 33 пари `avoid_before` — на довільні назви; 84 з 303 `enables`-ребер не мають зворотного `requires`.
- CDM-02 вимагає «earlier», але 153 з 355 ребер `requires` — у межах одного уроку; порядок усередині уроку не визначений.

**F-21 · MINOR · [помилка] · Outcomes контракту без стабільних ID**
- Контракт §5 (`:118-205`) містить маркований список без ідентифікаторів, хоча §18.1 (`:889-891`) вимагає стабільні ID для outcomes. Карта курсу не має посилань на outcomes контракту, тому прямий outcome → урок трасується лише вручну (див. розділ 4).

**F-22 · MINOR · [помилка] · Списки prerequisites уроків не є замиканням**
- `prerequisites.concepts` лише частково включають прямі `requires` нових концептів: напр., L11 не перелічує `stateless-composable`, `source-of-truth`, `state-transition`, `ui-logic-separation`; L13 — `rule-validation`, `terminal-state`, `state-machine-intro`; L14 — 16 концептів. Якщо генератор спиратиметься лише на список уроку, він недоврахує prerequisites. Рекомендація: письмово зафіксувати, що джерелом істини є dm (`usage_rules:36`), або автогенерувати замикання.

### 3.4 SUGGESTION

**F-23 · SUGGESTION · Відсутні в карті елементи контракту §5.4**: `Box`, Material cards і «common input controls» (контракт `:171-172`); явний концепт «declarative UI» (`:166`); «derived values» як відмінність від стану (`:130`).

**F-24 · SUGGESTION · Базова техніка розкладки дошки не зафіксована**: вкладені `Row`/`Column` з L03 vs `LazyVerticalGrid` з L06 (`:673-675`); для фіксованої сітки 3×3 вибір техніки впливає на `GameBoard` у L10 і на всі варіанти проєктів. Також плоский `List<Cell>` (L06) фіксований як «єдине представлення» (`:711-713`), тоді як challenge L06 порівнює з вкладеними списками (`:731`).

**F-25 · SUGGESTION · Template-код Android Studio**: стандартний шаблон проєкту містить `Scaffold`, `Modifier.padding(innerPadding)`, `@Preview`, `setContent` — це є в L01, а `scaffold` вводиться в L12, `preview-intro` — у L03. Варто додати до preview-реєстру перелік «готового шаблонного коду» (це припущення про вміст шаблону; перевірити для обраної версії Android Studio). Також `map-indexed-intro` (L08) — core-концепт, практика якого лише в challenge (`:950`).

## 4. Coverage matrix: outcome → teaching → practice → assessment

Позначення рецензента (P — §5.1, K — §5.2, W — §5.3, C — §5.4, A — §5.5, F — §5.6; порядок як у контракті). ✓ — є; ◐ — часткове; ✗ — немає. Посилання «sb» — sandbox, «lab» — Android-лабораторна, «ev» — `assessment.evidence`.

| ID | Outcome (скорочено) | Навчання | Практика | Оцінювання | Оцінка |
|---|---|---|---|---|---|
| P1 | пояснити базові терміни | L01–L06 | sb L01, L05, L06 | ev L01, L05, L06 | ✓ |
| P2 | трасувати програми | L03 `manual-tracing` | sb L03 | ev L03 | ✓ |
| P3 | декомпозиція на функції/дані | L04, L05 | sb/lab L04, L05 | ev L04, L05 | ✓ |
| P4 | умови та цикли в алгоритмах | L02, L03 | sb/lab L02, L03 | ev L02, L03 | ✓ |
| P5 | змінюваний стан vs похідні значення | L07 `ui-state-intro`, L10 `source-of-truth` | lab L07, sb L10 | ev L10 (джерело істини) | ◐ (немає поняття «derived») |
| P6 | компіляторні повідомлення та stack traces | L01 (type mismatch), L06 (index error) | sb L01 | ev L06 («index error») | ◐ (stack trace не названо) |
| P7 | налагодження: output, діагностика IDE, debugger | L01 `println`, L03 debugger, L13 | L03, L13 | ev L14 («один баг і діагноз») | ◐ |
| K1 | `val`/`var` | L01 | sb L01 | ev L01 | ✓ |
| K2 | базові типи | L01 | sb L01 | ev L01 | ✓ |
| K3 | вирази, рядки | L01, L02 | sb L01, L02 | ev L01, L02 | ✓ |
| K4 | `if`/`when`/ranges/`for`/`while` | L02, L03 | sb L02, L03 | ev L02, L03 | ✓ |
| K5 | функції | L04 | sb L04 | ev L04 | ✓ |
| K6 | `Unit` | L04 `unit-return` | sb L04 («printing vs returning») | немає прямого | ◐ |
| K7 | класи, властивості, методи, data class | L05 | sb/lab L05 | ev L05 | ✓ |
| K8 | списки; set/map за потреби | L06 | sb L06 (set/map лише intro) | ev L06 (список) | ✓ (set/map ◐) |
| K9 | nullable, `?.`, `?:`, smart cast | L07 (`null-check` не резолвиться) | sb L07 | ev L07 | ◐ (F-01) |
| K10 | enum та sealed | L07, L09 | sb L07, L09 | ev L07, L09 | ✓ |
| K11 | лямбди та операції колекцій | L08 | sb/lab L08 | ev L08 | ✓ (preview до L08 — F-03) |
| K12 | extension functions | L09 | sb L09 (одне завдання) | немає прямого | ◐ |
| K13 | immutable data, `copy` | L05, L08, L14 | lab L08 | ev L08 | ✓ |
| K14 | читати/писати ідіоматичний Kotlin | L14 | sb L14 | ev L14 | ✓ |
| W1 | відкрити/зібрати/запустити/змінити проєкт | L01 | lab L01 | ev L01 | ✓ |
| W2 | емулятор/пристрій | L01 | lab L01 | ev L01 («Runs the app») | ✓ |
| W3 | структура проєкту | L01 `basic-project-structure` | — | немає | ◐ |
| W4 | навігація, completion, formatting, рефакторинг IDE | лише `refactoring` (L14) | sb L14 (ручне) | немає | ✗ (F-11) |
| W5 | build/compile/runtime-збої | L01 (type mismatch), L13 | — | немає | ◐ |
| W6 | Gradle: конфігурація та залежності | лише «internals deferred» | — | немає | ✗ (F-11) |
| W7 | базовий Git | L13 `git-checkpoint-workflow` (не резолвиться) | — | немає | ✗ (F-11) |
| C1 | декларативний UI | L01 `recomposition-observation`, L07 | lab L01, L07 | ev L07 | ◐ (явного концепту немає) |
| C2 | створювати/викликати composables | L01, L04 | lab L01, L04 | ev L01, L04 | ✓ |
| C3 | Material: text, buttons, cards, inputs | `material-component-intro` L02 | — | немає | ◐ (cards, inputs відсутні) |
| C4 | `Row`/`Column`/`Box`, modifiers | L02 | lab L02 | ev L02 | ◐ (`Box` відсутній) |
| C5 | повторюваний контент, lazy-розкладки | L03, L06 | lab L03, L06 | ev L03, L06 | ✓ |
| C6 | параметри vs локальний стан | L07, L10 | lab L07, sb L10 | ev L10 | ✓ |
| C7 | стан керує UI | L07 | lab L07 | ev L07 | ✓ |
| C8 | state hoisting | L10 | sb/lab L10 | ev L10 | ✓ |
| C9 | reusable stateless components | L04, L10 | lab L04, L10 | ev L04, L10 | ✓ |
| C10 | Material theme | L12 | lab L12 | ev L12 | ✓ |
| C11 | розуміти рекомпозицію | L07 | lab L07 | ev L07 | ✓ |
| A1 | правила vs рендеринг | L09 | lab L09 | ev L09 | ✓ |
| A2 | відповідальності model/state/VM/screen/components | L11 | sb L11 | ev L11 | ✓ (межі — F-16) |
| A3 | unidirectional data flow | L10, L11 | sb/lab L10, L11 | ev L10, L11 | ✓ |
| A4 | трасувати дію callback → VM → UI | L11 | sb L11 | ev L11, L14 | ✓ |
| A5 | чому Screen не містить усіх правил | L09, L11 | lab L11 | ev L11 | ✓ |
| A6 | чому компоненти отримують дані та callbacks | L10 | lab L10 | ev L10 | ✓ |
| A7 | пакети та файли за відповідальністю | L11 `package-by-feature-intro` | — | немає | ◐ |
| A8 | мала зміна без порушення архітектури | — | — | немає | ✗ (F-10) |
| F1 | реалізувати гру | L13 | lab L13 | ev L13 | ✓ (реалістичність — F-14) |
| F2 | дошка, гравці, ходи, статус у даних | L05–L09 | lab L05–L09 | ev L05–L09 | ✓ |
| F3 | валідація ходів | L09, L13 | lab L09, L13 | ev L09 | ✓ |
| F4 | рендер зі стану | L06, L10 | lab L06, L10 | ev L06, L10 | ✓ |
| F5 | події через явні callbacks/VM | L10, L11 | lab L10, L11 | ev L10, L11 | ✓ |
| F6 | restart | L09, L11 | lab L09, L11 | ev — немає прямого | ◐ |
| F7 | пояснити control/data flow | L14 | lab L14 | ev L14 | ✓ |
| F8 | демонстрація та design decision | L14 | lab L14 | ev L14 | ✓ |

Висновок: 4 outcomes без повного ланцюга (W4, W6, W7, A8 — ✗), 13 — часткові (◐); решта — ✓.

## 5. Приховані залежності та прогалини (preview → introduction → practice → application)

| Конструкція | Preview | Формальне введення | Практика | Застосування | Статус |
|---|---|---|---|---|---|
| Лямбди / trailing lambda | L01 (onClick), L02 (`Column {}`), L03 (`repeat`), L04 (callback), L06 (`items {}`) | L08 | L08 | L09–L14 | Пробіл між preview і введенням не оформлений (F-03) |
| `remember`/`mutableStateOf` | L01 (scaffold, лише в L01 описаний) | L07 | L07 | L10–L12 | Scaffold L02–L06 не описаний (F-04) |
| Синтаксис `by` / `.value` | — | — (делегати виключені) | — | L07+ | Не визначено (F-04, D-02) |
| Nullable у моделях | неявно L05–L06 | L07 | L07 | L08+ | Приховано (F-05) |
| Колекційні API з лямбдами | L06 (`List(n){}`, `items{}`) | L08 | L08 | L09+ | Приховано (F-03) |
| Sealed / interface / object / data object | — | L09 (`data-object` не резолвиться) | L09 | L12 | Концепт `object` відсутній (F-01) |
| Оновлення дошки (immutable) | — | L08 | L08 | L09–L11 | L07 без механізму (F-06) |
| ViewModel state API | — | L11 | L11 | L12–L14 | Flow/collect не вирішено (F-07) |
| Перемикання екранів | — | L12 | L12 | L13 | Навігація не вирішена (F-08) |
| Gradle-залежність для `viewModel()` | — | — | — | L11 | Не покрито (F-07, F-11) |

Підтверджено, що **не** виявлено: прямих циклів у `requires`; `requires` на пізніший урок; Hilt/Room/DI/StateFlow як явного `requires` core-концепту (перевірка по `deferred_concepts` — жоден deferred ID не є `requires`); `optional` матеріалу як явного prerequisite.

## 6. Рішення, які потребують погодження автора

| ID | Рішення | Пов'язано |
|---|---|---|
| D-01 | API стану в ViewModel: Compose-стан без Flow чи `StateFlow`; як подається `viewModel()` | F-07 |
| D-02 | Синтаксис стану в L01–L07: `by` чи `.value`; обсяг scaffold у L01–L06 | F-03, F-04 |
| D-03 | Перемикання екранів: явний стан чи navigation-бібліотека | F-08 |
| D-04 | Capstone: індивідуальний чи стандартизований; чи допустимий Tic-Tac-Toe; профілі й рівні варіантів; Memory і затримки | F-14, F-15 |
| D-05 | Представлення дошки (плоский список чи вкладений), техніка розкладки та механізм оновлення в L07 | F-06, F-24 |
| D-06 | Де навчаються Git, Gradle/залежності, IDE-інструменти, stack traces | F-11 |
| D-07 | Політика домашніх завдань (обсяг, чи можуть містити обов'язкове) | F-12 |
| D-08 | Нормативність порядку рефакторингу з контракту §11.5 | F-17 |
| D-09 | Автоматичні тести: в обсязі чи ні; інфраструктура | F-16 |
| D-10 | Чи setup/handover обов'язкові для всіх проєктів | F-08, F-14 |
| D-11 | Місце й форма п'ятого checkpoint «final project readiness» | F-09 |
| D-12 | Канонічне місце артефактів (`docs/` чи `curriculum/`) і схема ID outcomes | F-19, F-21 |

### 6.1 Пропонована (не внесена) форма preview-реєстру

Щоб ранній preview був допустимий (контракт §15.2, правило dm `:33-34`), кожен запис мав би містити: конструкцію; урок preview; що студент має зрозуміти; що отримує як scaffold; що явно не очікується; урок повного введення. Це лише форма для обговорення; змісту не вигадано, його визначає автор.

## 7. Рекомендований порядок виправлень

1. **F-01, F-02** — механічне вирівнювання ID між картою курсу та dependency map (після цього скрипти валідації можна запустити повторно).
2. **D-01…D-03, D-05** — рішення, що впливають на кілька уроків (стан, ViewModel API, навігація, дошка); зафіксувати в ADR (за §18.2).
3. **F-03, F-04, F-05, F-06** — preview-реєстр і scaffold-політика L01–L07.
4. **F-09, F-10, F-11** — checkpoints, оцінювання малої зміни, розміщення Git/Gradle/IDE (D-06, D-11).
5. **F-14, F-15, F-16** — профілі capstone та таблиця відповідальностей (D-04, D-09, D-10).
6. **F-12, F-13** — політика домашніх і розмітка Core/Practice/Challenge для перевантажених уроків; хвилинні розкладки в специфікаціях уроків (див. розділ 1.3).
7. **F-17…F-25** — дрібні розбіжності, схема, SUGGESTION.

Після кроків 1–3 варто повторити цей аудит.

## Додаток A. Concept ID з `02-course-map.yaml`, що не резолвяться в `03-concept-dependency-map.yaml` (40)

- L02: `mutually-exclusive-conditions`, `random-value-intro`
- L03: `loop-invariant-intro`, `index-variable`, `preview-intro`, `breakpoint-intro`, `step-over-intro`
- L04: `named-argument`, `default-argument`, `component-api-intro`
- L05: `entity`, `state-vs-behavior-intro`, `member-function`, `equality-intro`
- L06: `get-by-index`, `set-intro`, `map-collection-intro`, `list-builder-intro`, `lazy-list-intro`, `stable-item-key-intro`
- L07: `invalid-state-modeling`, `null-check`, `state-read`, `state-write`
- L08: `declarative-collection-processing`, `trailing-lambda`
- L09: `separation-of-concerns-intro`, `data-object`
- L10: `previewable-stateless-component`
- L11: `presentation-layer-intro`, `viewmodel-lifecycle-intro`, `viewmodel-acquisition`, `collect-or-read-viewmodel-state`
- L12: `top-app-bar`, `dialog-or-overlay`, `adaptive-layout-awareness`, `accessibility-basics`, `screen-mode`, `navigation-boundary-intro`
- L13: `git-checkpoint-workflow`

Розв'язуються як supporting concepts (не помилка): `material-component-intro`, `abstraction-intro`, `map-operation`, `filter-operation`, `find-operation`, `any-operation`, `all-operation`, `count-operation`, `map-indexed-intro`.

## Додаток B. Результати механічних перевірок

| Перевірка | Результат |
|---|---|
| YAML парситься (обидва файли) | так |
| Унікальність ID у `concepts` (155) | так, дублікатів немає; `supporting_concepts` (11) не перетинаються з `concepts` |
| `requires` → ID існує (з урахуванням supporting) | 1 помилка: `smart-cast-intro` → `null-check` |
| `requires` вказує на пізніший урок | 0 випадків |
| Цикли в графі `requires` | 0 |
| `requires` на deferred-концепт | 0 |
| Кількість уроків / ID | 14, `L01…L14`, послідовні |
| Сума хвилин на урок | усі 120 |
| Підсумки Kotlin/Android | 820 / 860, збігаються з оголошеними; разом 1680 = 14 × 120 |
| Phase ↔ lesson ↔ milestone | узгоджені |
| `cognitive_support` покриває всі уроки | так |
| `introduced_in` у dm = урок з `new_concepts` | 1 розбіг (`immutable-component-input`) |
| Prerequisites уроків: ID існують | 2 помилки в L14 (F-01) |
| Prerequisites уроків: введені раніше | 1 порушення (L10, F-02) |
