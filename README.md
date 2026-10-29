# MAX Multi-Account Mod

> Независимый реверс-инжиниринг десктопного мессенджера **MAX** (Windows): несколько своих аккаунтов одновременно, изолированно, со входом, переживающим перезагрузку. **Витрина без кода.**
>
> Independent reverse engineering of the **MAX** desktop messenger (Windows): several of your own accounts at once, isolated, with logins that survive a reboot. **Code-free showcase.**

![type](https://img.shields.io/badge/type-research-8a6d3b)
![platform](https://img.shields.io/badge/platform-Windows%20x64-444)
![target](https://img.shields.io/badge/target-Qt6%20client-555)
![license](https://img.shields.io/badge/license-All%20Rights%20Reserved-b23)
![source](https://img.shields.io/badge/source-not%20published-999)

**Русский** (ниже) · **[English](#english)**

---

## О проекте

Официальный клиент рассчитан на один аккаунт на машину, а штатный мультиаккаунт отключён. Задача звучит просто — «открыть второе окно», — но упирается в закрытый бинарник: единственность инстанса, привязку сессии к устройству и хранение секретов нужно понять и аккуратно обойти, ничего не ломая.

Результат — не «патч на удачу», а воспроизводимое понимание того, как устроен клиент, и минимальный слой поверх него, который делает ровно одно: честно разводит профили по своим границам.

## Что получилось

- **Параллельные инстансы.** Обойдена защита от второго запуска; настоящий механизм блокировки найден инструментацией и нейтрализован для каждого профиля.
- **Изоляция аккаунтов.** Каждое окно видит только свою сессию — секреты разведены по профилям на уровне ОС.
- **Персистентные сессии.** Вход переживает перезапуск и перезагрузку ПК (подтверждено для основного профиля; автологин по каждому профилю ещё проверяется); ограничение переноса между машинами — сознательное следствие модели шифрования ОС.
- **Идентичность профиля.** Детерминированный, стабильный и отличный от других «отпечаток устройства» на каждый профиль.
- **Компактная нативная доставка.** Небольшой нативный x64-модуль без стандартной библиотеки; тупиковый путь вовремя распознан и отброшен.
- **Разбор протокола.** Сетевой протокол и телеметрический канал разобраны по бинарю и задокументированы — как результат анализа.

## Навыки и технологии

- **Windows-внутренности:** перехват user-mode API, DLL injection, IPC, credential store, реестр, shell-пути, sandboxing.
- **Реверс-инжиниринг:** PE/бинарный анализ, анализ таблиц импортов, реверс протокола, работа с декомпилятом.
- **Низкоуровневое/нативное:** C/C++ без CRT, x86/x64, inline-патчинг.
- **Инструменты:** Node.js-пайплайн сборки, PowerShell, WinForms-лончер.
- **Практика:** гипотезо-ориентированная отладка, статический + динамический анализ, документированная база знаний, гигиена секретов.

## Методология

Ничего на удачу — каждый вывод проходил через три независимых источника истины:

1. **Статика** — декомпилят, разобранный несколькими независимыми проходами со сведением и критиком.
2. **Динамика** — живой слепок запущенного процесса; гипотезы проверялись на реальном поведении.
3. **Сверка по бинарю** — таблицы импортов и разрядность как арбитр спорных предположений.

У каждого факта в базе знаний помечен честный статус: подтверждено, под вопросом или не сделано.

## Почему здесь нет кода

Это витрина инженерной работы, а не инструкция. Исходный код, собранные бинарники, реальные данные сессий и конкретные механизмы **сознательно не выкладываются**:

- интеллектуальная собственность автора;
- ответственное раскрытие — детали реверса закрытого клиента не место в открытом доступе;
- ценность здесь в решённых задачах и подходе, а не в копипасте.

Показаны классы проблем и навыки — не рецепт воспроизведения.

## Дисклеймер

Независимый, неофициальный исследовательский проект. **Не аффилирован с разработчиками мессенджера MAX, не одобрен и не спонсирован ими.** Все названия, продукты и товарные знаки принадлежат их владельцам и упоминаются только для идентификации. Проект создавался для личного использования, исследования и обучения. В репозитории нет учётных данных, токенов и собранных бинарников.

## Лицензия

**Все права защищены (All Rights Reserved).** Материалы можно просматривать для ознакомления; любое копирование, изменение, распространение или использование — только с письменного разрешения автора. См. [LICENSE](LICENSE).

---

<a id="english"></a>

## About

The official client is built for one account per machine, and its built-in multi-account mode is switched off. "Just open a second window" sounds trivial, but it runs into a closed binary: instance uniqueness, device-bound sessions and secret storage all have to be understood and carefully worked around without breaking anything.

The result isn't a patch-and-pray hack but a reproducible understanding of how the client works, and a minimal layer on top of it that does exactly one thing: cleanly keeps each profile within its own boundaries.

## What was achieved

- **Concurrent instances.** Got past the single-instance guard; the real lock was found through instrumentation and neutralized per profile.
- **Account isolation.** Each window sees only its own session — secrets scoped per profile at the OS level.
- **Persistent sessions.** The login survives restarts and reboots (verified for the primary profile; per-profile auto-login still being confirmed); limited cross-machine portability is an intentional consequence of the OS encryption model.
- **Per-profile identity.** A deterministic, stable, distinct "device fingerprint" for every profile.
- **Lean native delivery.** A small, CRT-free native x64 module; a dead-end path was recognized early and dropped.
- **Protocol analysis.** The network protocol and telemetry channel were mapped from the binary and documented — as an analysis result.

## Skills & technologies

- **Windows internals:** user-mode API interception, DLL injection, IPC, credential store, registry, shell paths, sandboxing.
- **Reverse engineering:** PE/binary analysis, import-table analysis, protocol RE, decompilation workflow.
- **Low-level / native:** C/C++ (CRT-free), x86/x64, inline patching.
- **Tooling:** Node.js build pipeline, PowerShell, WinForms launcher.
- **Practice:** hypothesis-driven debugging, static + dynamic analysis, a documented knowledge base, secrets hygiene.

## Methodology

Nothing left to luck — every conclusion went through three independent sources of truth:

1. **Static** — the decompilation, read in several independent passes, synthesized and challenged by a critic.
2. **Dynamic** — live snapshots of the running process; hypotheses tested against real behavior.
3. **Binary verification** — import tables and architecture as the arbiter of contested assumptions.

Every fact in the knowledge base carries an honest status: verified, uncertain, or not done.

## Why there's no code here

This is a showcase of engineering work, not a how-to. Source code, compiled binaries, real session data and the specific mechanisms are **deliberately withheld**:

- the author's intellectual property;
- responsible disclosure — reverse-engineering details of a closed client don't belong in the open;
- the value is in the problems solved and the approach, not in copy-paste.

What's shown is the class of problems and the skills — not a recipe to reproduce.

## Disclaimer

An independent, unofficial research project. **Not affiliated with, endorsed by or sponsored by the developers of the MAX messenger.** All names, products and trademarks belong to their respective owners and are referenced for identification only. The project was built for personal use, research and learning. The repository contains no credentials, tokens or compiled binaries.

## License

**All Rights Reserved.** The materials may be viewed for reference; any copying, modification, distribution or use requires the author's prior written permission. See [LICENSE](LICENSE).
