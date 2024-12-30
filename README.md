# Mongoose CRUD Service

## Опис

**Mongoose CRUD Service** — це бібліотека, яка спрощує створення CRUD-операцій для моделей MongoDB за допомогою Mongoose. Вона дозволяє швидко налаштувати базові операції для роботи з базою даних, зменшуючи обсяг повторюваного коду.

## Можливості

- Створення CRUD-методів для будь-якої моделі Mongoose.
- Гнучка система налаштувань для специфічних запитів.
- Підтримка фільтрації, сортування, пагінації та вибірки полів.
- Простий інтерфейс для інтеграції з вашими REST API.

## Вимоги

- Node.js >= 14.x
- Mongoose >= 6.x
- MongoDB >= 4.x

## Встановлення

```bash
npm install mongoose
npm install express
```

## Використання

### Базовий приклад

```javascript
const mongoose = require('mongoose');
const { CrudService } = require('mongoose-crud-service');

// Створення схеми Mongoose
const userSchema = new mongoose.Schema({
  name: { type: String, required: true },
  email: { type: String, required: true, unique: true },
  age: { type: Number, required: true },
});

const User = mongoose.model('User', userSchema);

// Ініціалізація CRUD-сервісу
const userService = new CrudService(User);

// Використання CRUD-методів
(async () => {
  // Створення запису
  const newUser = await userService.create({ name: 'John Doe', email: 'john@example.com', age: 30 });

  // Отримання списку користувачів
  const users = await userService.find({ age: { $gte: 18 } }, { sort: '-age', limit: 10 });

  // Оновлення користувача
  const updatedUser = await userService.update(newUser._id, { age: 31 });

  // Видалення користувача
  await userService.delete(newUser._id);
})();
```

### Методи

- `create(data)` - Створює новий запис.
- `find(query, options)` - Отримує список записів з урахуванням фільтрації, сортування та пагінації.
- `findById(id)` - Знаходить запис за ідентифікатором.
- `update(id, data)` - Оновлює запис за ідентифікатором.
- `delete(id)` - Видаляє запис за ідентифікатором.

## Налаштування

CRUD-сервіс підтримує опції для гнучкої роботи:

- **Фільтрація**: Передавайте умови MongoDB для пошуку записів.
- **Сортування**: Використовуйте параметр `sort` для вказання порядку сортування (наприклад, `-field` для спадання).
- **Пагінація**: Використовуйте `limit` та `skip` для обмеження кількості записів.
- **Вибірка полів**: Використовуйте `select` для визначення полів, які будуть повернуті.

### Приклад із опціями

```javascript
const users = await userService.find(
  { age: { $gte: 18 } },
  { sort: '-age', limit: 5, select: 'name email' }
);
```

## Тестування


```bash
npm start
```

## Ліцензія

Цей проект ліцензовано під ліцензією MIT. Деталі дивіться у файлі [LICENSE](./LICENSE).

---
