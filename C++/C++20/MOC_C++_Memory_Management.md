---
category: Index / MOC
tags:
  - moc
  - cpp/memory
status: 🔄 В процессе
---

# 🗺️ MOC: Управление памятью и ресурсами в C++

## 🏛️ Фундаментальные принципы и идиомы
- Базовый принцип: [[RAII]] (Resource Acquisition Is Initialization)
- Механика передачи владения: 
  [[move_semantics]] (`move`, `forward`)
- Сокрытие деталей реализации: 
  [[pimpl_idiom]]

## 📦 Умные указатели (Smart Pointers)
- Эксклюзивное владение (Zero-cost): 
  [[unique_ptr]]
- Разделяемое владение (Reference Counting): 
  [[shared_ptr]]
- Разрыв циклических ссылок: 
  [[weak_ptr]]
- Стратегии очистки: 
  [[custom_deleters]]
- Фабричные функции: 
  `make_unique`, `make_shared`

## ⚙️ Низкоуровневая аллокация и память
- Куча vs Стек: организация сегментов памяти процесса
- Работа операторов `new` / `delete`
- Пользовательские аллокаторы: `std::allocator`, `pmr` (Polymorphic Memory Resources)

## 💣 Подводные камни и инструменты отладки
- Типовые ошибки: 
  [[Double_Free]], 
  [[Memory_Leaks]], 
  [[Use_After_Free]]
- Санитайзеры: [[AddressSanitizer]] (ASan), [[LeakSanitizer]] (LSan)
- Диагностика: [[Valgrind]]