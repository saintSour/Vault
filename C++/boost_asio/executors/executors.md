---
category: C++ Asio
type: Концепция
tags:
  - cpp/asio
  - concurrency
  - executors
status: ⏳ В процессе
created: 2026-09-29 23:23
---

# 🌐 Исполнители

> [!abstract] Суть темы
> **Executor (исполнитель)** — это контракт и стратегия выполнения задач, определяющая *где, когда и в каком контексте* запускается переданный блок кода (обработчик, лямбда или корутина). Экзекуторы изолируют бизнес-логику сетевых компонентов от конкретной многопоточной инфраструктуры.

---

## 🧠 Ключевые концепции и Жизненный цикл
- **Специализированные экзекуторы:** 
  [[any_io_executor]],
  [[strand]],
  [[io_context_executor]],
  [[thread_pool_executor]],
  [[system_executor]]
- **Контексты исполнения:** [[io_context]], [[thread_pool]]
- **Роль в архитектуре:** Начиная с Boost 1.70+, любой I/O-объект параметризуется типом экзекутора (`basic_stream_socket<tcp, Executor>`). По умолчанию используется полиморфный `any_io_executor`.

### 🔄 Потоковая модель и Контекст выполнения
- **Потокобезопасность:** [Да/Нет/Только константные методы]. Требуется ли `strand`?
- **Executor:** Каким экзекутором управляется данный компонент?

```mermaid
graph LR
    A[Инициация Асинхронной операции] --> B[Постановка в очередь / Ядро ОС]
    B --> C[Завершение операции]
    C --> D[Вызов Callback / Handler в io_context]
```

---

## 💻 Сигнатуры и Код (MRE)

### Минимальный рабочий пример (Minimal Reproducible Example)
```cpp
#include <boost/asio.hpp>
#include <iostream>
#include <memory>

namespace asio = boost::asio;
using asio::ip::tcp;

int main() {
    try {
        asio::io_context io;
        // Код демонстрации концепции
        
        io.run();
    } catch (const boost::system::system_error& e) {
        std::cerr << "Error: " << e.what() << std::endl;
    }
    return 0;
}
```

### 📋 Важные перегрузки и параметры
* `handler`: Требования к сигнатуре колбэка (например, `void(boost::system::error_code, std::size_t)`).
* `buffers`: Какие типы буферов принимает (`const_buffer`, `mutable_buffer`).

---

## ⚠️ Подводные камни и Управление памятью (Gotchas)

> [!danger] Время жизни объектов (Lifetime Issues)
> Асинхронные операции не копируют данные буферов. 
> **Правило:** Гарантировать, что `socket`, `buffer` или сессия (`shared_from_this()`) живут до тех пор, пока не отработает Handler.

- **Обработка ошибок:** Как ведет себя функция при исключениях vs `boost::system::error_code`?
- **Аллокация памяти:** Выделяет ли операция память в куче на каждый вызов? (Использование `asio_handler_allocate`).

---

## ⚙️ Практические сценарии (Use Cases)

### 🚀 Способы интеграции / Асинхронные парадигмы
*Выбрать подходящее:*
- [ ] **Callbacks** (Классический подход с `std::bind` или лямбдами)
- [ ] **Stackless Coroutines** (`asio::coroutine`, `yield_context`)
- [ ] **C++20 Coroutines** (`asio::awaitable`, `co_await`)
- [ ] **Futures** (`asio::use_future`)

---

## 🔗 Источники и Референсы
- 📖 [Официальная документация Boost.Asio](https://boost.org)
- 🔗 Исходники на GitHub: `boostorg/asio`
- Смотри также: `[[Asio Map of Content]]`