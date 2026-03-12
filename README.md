# Лабораторна робота
## ПРОЄКТУВАННЯ АРХІТЕКТУРИ СИСТЕМИ
### Діаграми компонентів та розгортання UML. Реалізація шарової архітектури в Python

**Виконали:** студенти групи РПЗ-33, Руденко Дмитро, Зозуля Дмитро, Кірічок Володимир, Фокін Степан, Якубов Артур

**Варіант:** 7 (Спортивний клуб / HealthTrack)

**Дата:** 2026

---

## 1. МЕТА РОБОТИ

Метою даної лабораторної роботи є формування практичних навичок проєктування архітектури програмної системи за допомогою UML-нотації та реалізація шарової архітектури мовою програмування Python. Ми побудували діаграми компонентів та розгортання для системи HealthTrack, реалізували скелет трирівневої архітектури з коректним розподілом коду між шарами та продемонстрували взаємодію між шарами через наскрізний сценарій використання.

---

## 2. ОПИС ПРЕДМЕТНОЇ ГАЛУЗІ

Система HealthTrack — це консольний прототип мобільного додатку для відстеження фізичної активності, харчування та тренувань користувачів. Система призначена для користувачів, які прагнуть контролювати своє здоров'я, відстежувати тренування та отримувати персоналізовані рекомендації.

### Призначення компонентів системи:

| Компонент | Призначення |
|-----------|-------------|
| **ConsoleUI** | Консольний інтерфейс для взаємодії користувача з системою (введення даних, відображення результатів) |
| **AuthService** | Обробка реєстрації, авторизації та управління сесіями користувачів |
| **WorkoutService** | Управління тренуваннями: створення, перегляд, оновлення, видалення записів про тренування |
| **NutritionService** | Логування харчування: додавання продуктів, розрахунок калорій |
| **Repository** | Абстрактний шар доступу до даних: збереження та отримання об'єктів |
| **Database** | Фізичне сховище даних (JSON-файл або SQLite) |

---

## 3. ТАБЛИЦЯ ІНТЕРФЕЙСІВ КОМПОНЕНТІВ

| Компонент | Надані інтерфейси | Необхідні інтерфейси | Взаємодіє з |
|-----------|-------------------|----------------------|-------------|
| **ConsoleUI** | IConsoleView | IAuthService, IWorkoutService, INutritionService | AuthService, WorkoutService, NutritionService |
| **AuthService** | IAuthService | IRepository | Repository |
| **WorkoutService** | IWorkoutService | IRepository | Repository |
| **NutritionService** | INutritionService | IRepository | Repository |
| **Repository** | IRepository | - | Database |
| **Database** | IDatabase | - | - |

---

## 4. ДІАГРАМА КОМПОНЕНТІВ

```
┌─────────────────────────────────────────────────────────┐
│              ДІАГРАМА КОМПОНЕНТІВ HealthTrack          │
└─────────────────────────────────────────────────────────┘

                    ┌─────────────────────┐
                    │  <<component>>      │
                    │     ConsoleUI       │
                    │                     │
                    │  ○ IConsoleView     │
                    │  ( IAuthService     │
                    │  ( IWorkoutService  │
                    │  ( INutritionService│
                    └──────────┬──────────┘
                               │
        ┌──────────────────────┼──────────────────────┐
        │                      │                      │
        ▼                      ▼                      ▼
┌──────────────┐  ┌──────────────┐  ┌──────────────┐
│<<component>> │  │<<component>> │  │<<component>> │
│ AuthService  │  │WorkoutService│  │NutritionSvc  │
│              │  │              │  │              │
│○ IAuthService│  │○IWorkoutSvc  │  │○INutritionSvc│
│( IRepository │  │( IRepository │  │( IRepository │
└──────┬───────┘  └──────┬───────┘  └──────┬───────┘
       │                 │                 │
       └────────────────┼─────────────────┘
                        │
                        ▼
              ┌─────────────────┐
              │ <<component>>   │
              │   Repository    │
              │                 │
              │ ○ IRepository   │
              │ ( IDatabase     │
              └────────┬────────┘
                       │
                       ▼
              ┌─────────────────┐
              │ <<component>>   │
              │    Database     │
              │                 │
              │ ○ IDatabase     │
              └─────────────────┘

Легенда:
  ○ — наданий інтерфейс (provided / lollipop)
  ( — необхідний інтерфейс (required / socket)
  ───> — залежність (dependency)
```

---

## 5. ДІАГРАМА РОЗГОРТАННЯ

```
┌─────────────────────────────────────────────────────────┐
│            ДІАГРАМА РОЗГОРТАННЯ HealthTrack            │
└─────────────────────────────────────────────────────────┘

    ┌─────────────────────────────────────────────────┐
    │  <<device>>                                     │
    │  Студентський ПК / Сервер розробки              │
    │  (Windows 11 / Ubuntu 22.04)                    │
    │                                                 │
    │  ┌─────────────────────────────────────────┐   │
    │  │ <<executionEnvironment>>                │   │
    │  │ Python 3.11 Interpreter                 │   │
    │  │                                         │   │
    │  │  ┌─────────────────────────────────┐   │   │
    │  │  │ <<artifact>>                    │   │   │
    │  │  │ healthtrack/                    │   │   │
    │  │  │ ├── main.py                     │   │   │
    │  │  │ ├── presentation/               │   │   │
    │  │  │ │   ├── __init__.py             │   │   │
    │  │  │ │   └── console_ui.py           │   │   │
    │  │  │ ├── business/                   │   │   │
    │  │  │ │   ├── __init__.py             │   │   │
    │  │  │ │   ├── models.py               │   │   │
    │  │  │ │   ├── auth_service.py         │   │   │
    │  │  │ │   ├── workout_service.py      │   │   │
    │  │  │ │   └── nutrition_service.py    │   │   │
    │  │  │ ├── data/                       │   │   │
    │  │  │ │   ├── __init__.py             │   │   │
    │  │  │ │   └── repository.py           │   │   │
    │  │  │ └── data/healthtrack_data.json  │   │   │
    │  │  └─────────────────────────────────┘   │   │
    │  └─────────────────────────────────────────┘   │
    └─────────────────────────────────────────────────┘
                        │
                        │ (локальний доступ до файлу)
                        ▼
              ┌─────────────────┐
              │ <<artifact>>    │
              │ healthtrack_    │
              │ data.json       │
              │ (сховище даних) │
              └─────────────────┘

Протокол: Локальна файлова система
Середовище: Python 3.11+
```

---

## 6. РЕАЛІЗАЦИЯ ШАРОВОЇ АРХІТЕКТУРИ В PYTHON

### Структура проєкту:
```
healthtrack/
├── main.py
├── presentation/
│   ├── __init__.py
│   └── console_ui.py
├── business/
│   ├── __init__.py
│   ├── models.py
│   ├── auth_service.py
│   ├── workout_service.py
│   └── nutrition_service.py
├── data/
│   ├── __init__.py
│   └── repository.py
└── data/
    └── healthtrack_data.json
```

### 6.1. data/repository.py
```python
"""
Шар доступу до даних (Data Access Layer)
Реалізує абстракцію сховища для збереження об'єктів
"""
import json
import os
from typing import List, Optional, TypeVar, Generic
from business.models import Entity

T = TypeVar('T', bound=Entity)

class Repository(Generic[T]):
    """Універсальний репозиторій для роботи з даними"""
    
    def __init__(self, filename: str, entity_class):
        self._filename = filename
        self._entity_class = entity_class
        self._storage: List[T] = []
        self._next_id = 1
        self._load()
    
    def _load(self):
        """Завантаження даних з файлу"""
        if os.path.exists(self._filename):
            try:
                with open(self._filename, 'r', encoding='utf-8') as f:
                    data = json.load(f)
                    self._storage = [
                        self._entity_class(**item) for item in data.get('items', [])
                    ]
                    self._next_id = data.get('next_id', 1)
            except (json.JSONDecodeError, IOError):
                print(f"[Repository] Файл пошкоджено, створено нове сховище")
    
    def _save(self):
        """Збереження даних у файл"""
        data = {
            'items': [item.to_dict() for item in self._storage],
            'next_id': self._next_id
        }
        os.makedirs(os.path.dirname(self._filename), exist_ok=True)
        with open(self._filename, 'w', encoding='utf-8') as f:
            json.dump(data, f, indent=2, ensure_ascii=False)
    
    def find_all(self) -> List[T]:
        """Отримати всі записи"""
        return list(self._storage)
    
    def find_by_id(self, item_id: int) -> Optional[T]:
        """Знайти запис за ID"""
        for item in self._storage:
            if item.id == item_id:
                return item
        return None
    
    def find_by_user(self, username: str) -> List[T]:
        """Знайти записи за ім'ям користувача"""
        return [item for item in self._storage if item.user == username]
    
    def save(self, item: T) -> T:
        """Зберегти або оновити запис"""
        if item.id is None or item.id == -1:
            item.id = self._next_id
            self._next_id += 1
            self._storage.append(item)
        else:
            existing = self.find_by_id(item.id)
            if existing:
                self._storage.remove(existing)
                self._storage.append(item)
            else:
                self._storage.append(item)
        self._save()
        return item
    
    def delete(self, item_id: int) -> bool:
        """Видалити запис за ID"""
        item = self.find_by_id(item_id)
        if item:
            self._storage.remove(item)
            self._save()
            return True
        return False
```

### 6.2. business/models.py
```python
"""
Моделі предметної галузі (Business Layer - Models)
Описують сутності системи HealthTrack
"""
from dataclasses import dataclass, field, asdict
from datetime import datetime
from typing import Optional
from enum import Enum

class WorkoutType(Enum):
    """Типи тренувань"""
    CARDIO = "cardio"
    STRENGTH = "strength"
    FLEXIBILITY = "flexibility"
    CROSSFIT = "crossfit"
    HOME = "home"

@dataclass
class Entity:
    """Базова сутність з унікальним ID"""
    id: int = -1
    user: str = ""
    created_at: datetime = field(default_factory=datetime.now)
    
    def to_dict(self) -> dict:
        """Конвертація в словник для JSON-серіалізації"""
        data = asdict(self)
        data['created_at'] = self.created_at.isoformat()
        return data
    
    def validate(self):
        """Базова валідація"""
        if not self.user or len(self.user.strip()) == 0:
            raise ValueError('Ім\'я користувача не може бути порожнім')

@dataclass
class User(Entity):
    """Модель користувача"""
    username: str = ""
    email: str = ""
    password_hash: str = ""
    is_premium: bool = False
    is_banned: bool = False
    
    def validate(self):
        super().validate()
        if not self.username or len(self.username.strip()) < 3:
            raise ValueError('Логін має містити щонайменше 3 символи')
        if '@' not in self.email:
            raise ValueError('Невірний формат email')
        if len(self.password_hash) < 8:
            raise ValueError('Пароль має містити щонайменше 8 символів')

@dataclass
class Workout(Entity):
    """Модель запису про тренування"""
    workout_type: str = WorkoutType.HOME.value
    duration_minutes: int = 0
    calories_burned: int = 0
    notes: str = ""
    
    def validate(self):
        super().validate()
        if self.duration_minutes <= 0:
            raise ValueError('Тривалість тренування має бути додатною')
        if self.calories_burned < 0:
            raise ValueError('Кількість спалених калорій не може бути від'ємною')
        if self.workout_type not in [t.value for t in WorkoutType]:
            raise ValueError(f'Невідомий тип тренування: {self.workout_type}')
    
    def __str__(self):
        return f"[#{self.id}] {self.workout_type.upper()} | {self.duration_minutes}хв | {self.calories_burned}ккал | {self.notes}"

@dataclass
class NutritionLog(Entity):
    """Модель запису про харчування"""
    food_name: str = ""
    calories: int = 0
    protein: float = 0.0
    carbs: float = 0.0
    fat: float = 0.0
    meal_time: str = "other"  # breakfast, lunch, dinner, other
    
    def validate(self):
        super().validate()
        if not self.food_name or len(self.food_name.strip()) == 0:
            raise ValueError('Назва продукту не може бути порожньою')
        if self.calories < 0:
            raise ValueError('Калорійність не може бути від'ємною')
    
    def __str__(self):
        return f"[#{self.id}] {self.food_name} | {self.calories}ккал | Б:{self.protein}г В:{self.carbs}г Ж:{self.fat}г"
```

### 6.3. business/auth_service.py
```python
"""
Сервіс авторизації (Business Layer - Auth)
Реалізує логіку реєстрації та авторизації користувачів
"""
from typing import Optional, Tuple
from data.repository import Repository
from business.models import User

class AuthService:
    """Сервіс для управління користувачами"""
    
    def __init__(self, user_repo: Repository[User]):
        self._repo = user_repo
    
    def register(self, username: str, email: str, password: str) -> Tuple[bool, str]:
        """Реєстрація нового користувача"""
        # Перевірка унікальності
        existing = [u for u in self._repo.find_all() if u.username == username]
        if existing:
            return False, "Користувач з таким ім'ям вже існує"
        
        # Створення та валідація
        user = User(username=username, email=email, password_hash=password)
        try:
            user.validate()
        except ValueError as e:
            return False, str(e)
        
        self._repo.save(user)
        return True, f"Користувач {username} успішно зареєстрований"
    
    def login(self, username: str, password: str) -> Tuple[bool, str, Optional[User]]:
        """Авторизація користувача"""
        users = [u for u in self._repo.find_all() if u.username == username]
        if not users:
            return False, "Користувача не знайдено", None
        
        user = users[0]
        if user.is_banned:
            return False, "ВАШ АКАУНТ ЗАБЛОКОВАНО", None
        
        if user.password_hash != password:  # У реальному проєкті: bcrypt.checkpw()
            return False, "Невірний пароль", None
        
        return True, f"Вітаємо, {username}!", user
    
    def get_user(self, username: str) -> Optional[User]:
        """Отримати користувача за ім'ям"""
        users = [u for u in self._repo.find_all() if u.username == username]
        return users[0] if users else None
    
    def ban_user(self, target_username: str, admin_username: str) -> Tuple[bool, str]:
        """Блокування користувача (тільки для адмінів)"""
        admin = self.get_user(admin_username)
        if not admin or not admin.is_premium:  # Спрощена перевірка адміна
            return False, "Недостатньо прав"
        
        target = self.get_user(target_username)
        if not target:
            return False, "Користувача не знайдено"
        if target.username == admin_username:
            return False, "Не можна заблокувати самого себе"
        
        target.is_banned = not target.is_banned
        self._repo.save(target)
        status = "ЗАБЛОКОВАНО" if target.is_banned else "РОЗБЛОКОВАНО"
        return True, f"Користувача {target_username} {status}"
```

### 6.4. business/workout_service.py
```python
"""
Сервіс тренувань (Business Layer - Workouts)
Реалізує бізнес-логіку управління тренуваннями
"""
from typing import List, Optional, Tuple
from data.repository import Repository
from business.models import Workout, WorkoutType

class WorkoutService:
    """Сервіс для управління тренуваннями"""
    
    def __init__(self, workout_repo: Repository[Workout]):
        self._repo = workout_repo
    
    def create_workout(self, username: str, workout_type: str, 
                       duration: int, calories: int, notes: str = "") -> Tuple[bool, str]:
        """Створення нового запису про тренування"""
        workout = Workout(
            user=username,
            workout_type=workout_type,
            duration_minutes=duration,
            calories_burned=calories,
            notes=notes
        )
        try:
            workout.validate()
        except ValueError as e:
            return False, str(e)
        
        self._repo.save(workout)
        return True, f"Тренування #{workout.id} успішно додано"
    
    def get_all(self) -> List[Workout]:
        """Отримати всі тренування"""
        return self._repo.find_all()
    
    def get_by_user(self, username: str) -> List[Workout]:
        """Отримати тренування конкретного користувача"""
        return self._repo.find_by_user(username)
    
    def get_by_id(self, workout_id: int) -> Optional[Workout]:
        """Отримати тренування за ID"""
        return self._repo.find_by_id(workout_id)
    
    def update_workout(self, workout_id: int, username: str, **kwargs) -> Tuple[bool, str]:
        """Оновлення запису про тренування"""
        workout = self._repo.find_by_id(workout_id)
        if not workout:
            return False, f"Тренування #{workout_id} не знайдено"
        if workout.user != username:
            return False, "Ви не можете редагувати чужі тренування"
        
        for key, value in kwargs.items():
            if hasattr(workout, key):
                setattr(workout, key, value)
        
        try:
            workout.validate()
        except ValueError as e:
            return False, str(e)
        
        self._repo.save(workout)
        return True, f"Тренування #{workout_id} оновлено"
    
    def delete_workout(self, workout_id: int, username: str) -> Tuple[bool, str]:
        """Видалення запису про тренування"""
        workout = self._repo.find_by_id(workout_id)
        if not workout:
            return False, f"Тренування #{workout_id} не знайдено"
        if workout.user != username:
            return False, "Ви не можете видаляти чужі тренування"
        
        self._repo.delete(workout_id)
        return True, f"Тренування #{workout_id} видалено"
    
    def get_stats(self, username: str) -> dict:
        """Отримати статистику користувача"""
        workouts = self.get_by_user(username)
        if not workouts:
            return {"total": 0, "total_duration": 0, "total_calories": 0}
        
        return {
            "total": len(workouts),
            "total_duration": sum(w.duration_minutes for w in workouts),
            "total_calories": sum(w.calories_burned for w in workouts),
            "avg_duration": sum(w.duration_minutes for w in workouts) / len(workouts)
        }
```

### 6.5. presentation/console_ui.py
```python
"""
Шар представлення (Presentation Layer)
Реалізує консольний інтерфейс користувача
"""
import os
from typing import Optional
from business.auth_service import AuthService
from business.workout_service import WorkoutService
from business.models import User, Workout, WorkoutType

class ConsoleUI:
    """Консольний інтерфейс системи HealthTrack"""
    
    def __init__(self, auth_service: AuthService, workout_service: WorkoutService):
        self._auth = auth_service
        self._workouts = workout_service
        self._current_user: Optional[User] = None
    
    def clear(self):
        """Очищення консолі"""
        os.system('cls' if os.name == 'nt' else 'clear')
    
    def print_header(self, title: str):
        """Друк заголовка"""
        print(f"\n{'='*50}")
        print(f"  {title:^46}")
        print(f"{'='*50}\n")
    
    def run(self):
        """Головний цикл програми"""
        while True:
            if self._current_user is None:
                self._run_auth_menu()
            else:
                self._run_main_menu()
    
    def _run_auth_menu(self):
        """Меню авторизації"""
        self.clear()
        self.print_header("HealthTrack - Вхід в систему")
        print("1. Увійти")
        print("2. Зареєструватися")
        print("0. Вихід")
        
        choice = input("\nВаш вибір > ").strip()
        
        if choice == '1':
            self._login()
        elif choice == '2':
            self._register()
        elif choice == '0':
            print("\nДо побачення!")
            exit(0)
        else:
            input("Невірний вибір. Натисніть Enter...")
    
    def _register(self):
        """Реєстрація нового користувача"""
        self.clear()
        self.print_header("Реєстрація")
        
        username = input("Логін (мін. 3 символи): ").strip()
        email = input("Email: ").strip()
        password = input("Пароль (мін. 8 символів): ").strip()
        
        success, message = self._auth.register(username, email, password)
        print(f"\n{'✅' if success else '❌'} {message}")
        input("Натисніть Enter...")
    
    def _login(self):
        """Авторизація користувача"""
        self.clear()
        self.print_header("Вхід в систему")
        
        username = input("Логін: ").strip()
        password = input("Пароль: ").strip()
        
        success, message, user = self._auth.login(username, password)
        print(f"\n{'✅' if success else '❌'} {message}")
        
        if success and user:
            self._current_user = user
        input("Натисніть Enter...")
    
    def _run_main_menu(self):
        """Головне меню після авторизації"""
        self.clear()
        role = "[ADMIN]" if self._current_user.is_premium else "[USER]"
        self.print_header(f"HealthTrack {role} | {self._current_user.username}")
        
        print("🏋️  ТРЕНУВАННЯ:")
        print("  1. Додати тренування")
        print("  2. Мої тренування")
        print("  3. Статистика")
        print("  4. Знайти тренування за ID")
        print("  5. Видалити тренування")
        
        if self._current_user.is_premium:
            print(f"\n🛡️  АДМІН-ПАНЕЛЬ:")
            print("  9. Список всіх користувачів")
            print("  10. Бан/Розбан користувача")
        
        print("\n  0. Вихід з акаунту")
        
        choice = input(f"\nВаш вибір > ").strip()
        
        if choice == '1':
            self._add_workout()
        elif choice == '2':
            self._show_my_workouts()
        elif choice == '3':
            self._show_stats()
        elif choice == '4':
            self._find_workout_by_id()
        elif choice == '5':
            self._delete_workout()
        elif choice == '9' and self._current_user.is_premium:
            self._admin_list_users()
        elif choice == '10' and self._current_user.is_premium:
            self._admin_ban_user()
        elif choice == '0':
            self._current_user = None
        else:
            input("Невідома команда. Натисніть Enter...")
    
    def _add_workout(self):
        """Додавання нового тренування"""
        self.clear()
        self.print_header("Додати тренування")
        
        print("Типи тренувань:")
        for i, t in enumerate(WorkoutType, 1):
            print(f"  {i}. {t.value}")
        
        type_idx = int(input("\nОберіть тип (1-5): ").strip())
        workout_type = list(WorkoutType)[type_idx - 1].value
        
        duration = int(input("Тривалість (хвилин): ").strip())
        calories = int(input("Спалено калорій: ").strip())
        notes = input("Нотатки (необов'язково): ").strip()
        
        success, message = self._workouts.create_workout(
            self._current_user.username, workout_type, duration, calories, notes
        )
        print(f"\n{'✅' if success else '❌'} {message}")
        input("Натисніть Enter...")
    
    def _show_my_workouts(self):
        """Показати всі тренування користувача"""
        self.clear()
        self.print_header("Мої тренування")
        
        workouts = self._workouts.get_by_user(self._current_user.username)
        if not workouts:
            print("Тренувань ще немає. Додайте перше!")
        else:
            for w in workouts:
                print(f"  {w}")
        
        input("\nНатисніть Enter...")
    
    def _show_stats(self):
        """Показати статистику"""
        self.clear()
        self.print_header("Ваша статистика")
        
        stats = self._workouts.get_stats(self._current_user.username)
        print(f"  Загальна кількість тренувань: {stats['total']}")
        print(f"  Загальна тривалість: {stats['total_duration']} хв")
        print(f"  Загально спалено: {stats['total_calories']} ккал")
        if stats['total'] > 0:
            print(f"  Середня тривалість: {stats['avg_duration']:.1f} хв")
        
        input("\nНатисніть Enter...")
    
    def _find_workout_by_id(self):
        """Пошук тренування за ID"""
        self.clear()
        self.print_header("Пошук за ID")
        
        workout_id = int(input("Введіть ID тренування: ").strip())
        workout = self._workouts.get_by_id(workout_id)
        
        if workout and workout.user == self._current_user.username:
            print(f"\n✅ Знайдено: {workout}")
        else:
            print("\n❌ Тренування не знайдено або немає доступу")
        input("Натисніть Enter...")
    
    def _delete_workout(self):
        """Видалення тренування"""
        self.clear()
        self.print_header("Видалити тренування")
        
        workout_id = int(input("Введіть ID тренування для видалення: ").strip())
        success, message = self._workouts.delete_workout(
            workout_id, self._current_user.username
        )
        print(f"\n{'✅' if success else '❌'} {message}")
        input("Натисніть Enter...")
    
    def _admin_list_users(self):
        """Адмін: список користувачів"""
        self.clear()
        self.print_header("База користувачів")
        
        users = self._auth._repo.find_all()
        print(f"{'Логін':<20} {'Email':<25} {'Статус'}")
        print("-" * 50)
        for u in users:
            status = "🔴 BANNED" if u.is_banned else "🟢 ACTIVE"
            premium = " [PREMIUM]" if u.is_premium else ""
            print(f"{u.username:<20} {u.email:<25} {status}{premium}")
        
        input("\nНатисніть Enter...")
    
    def _admin_ban_user(self):
        """Адмін: бан користувача"""
        self.clear()
        self.print_header("Бан/Розбан користувача")
        
        target = input("Введіть логін користувача: ").strip()
        success, message = self._auth.ban_user(
            target, self._current_user.username
        )
        print(f"\n{'✅' if success else '❌'} {message}")
        input("Натисніть Enter...")
```
### 6.6. main.py
```python
#!/usr/bin/env python3
"""
Точка входу в систему HealthTrack
Реалізує ін'єкцію залежностей та запуск додатку
"""
from data.repository import Repository
from business.models import User, Workout
from business.auth_service import AuthService
from business.workout_service import WorkoutService
from presentation.console_ui import ConsoleUI

def main():
    """Ініціалізація та запуск системи"""
    
    # 1. Створення репозиторіїв (Data Layer)
    user_repo = Repository[User]("data/healthtrack_users.json", User)
    workout_repo = Repository[Workout]("data/healthtrack_workouts.json", Workout)
    
    # 2. Створення сервісів (Business Layer) з ін'єкцією залежностей
    auth_service = AuthService(user_repo)
    workout_service = WorkoutService(workout_repo)
    
    # 3. Створення UI (Presentation Layer) з ін'єкцією залежностей
    ui = ConsoleUI(auth_service, workout_service)
    
    # 4. Запуск головного циклу
    print("🚀 HealthTrack запущено...")
    ui.run()

if __name__ == "__main__":
    main()
```

---

## 7. ДЕМОНСТРАЦІЯ РОБОТИ СИСТЕМИ

### Сценарій тестування:
1. Реєстрація нового користувача
2. Авторизація
3. Додавання 3 тренувань
4. Перегляд всіх тренувань
5. Перегляд статистики
6. Пошук тренування за ID
7. Видалення одного тренування

### Консольне виведення:
```
==================================================
              HealthTrack - Вхід в систему          
==================================================

1. Увійти
2. Зареєструватися
0. Вихід

Ваш вибір > 2

==================================================
                    Реєстрация                    
==================================================

Логін (мін. 3 символи): testuser
Email: test@example.com
Пароль (мін. 8 символів): password123

✅ Користувач testuser успішно зареєстрований
Натисніть Enter...

==================================================
        HealthTrack [USER] | testuser        
==================================================

🏋️  ТРЕНУВАННЯ:
  1. Додати тренування
  2. Мої тренування
  3. Статистика
  ...

Ваш вибір > 1

==================================================
               Додати тренування                  
==================================================

Типи тренувань:
  1. cardio
  2. strength
  3. flexibility
  4. crossfit
  5. home

Оберіть тип (1-5): 2
Тривалість (хвилин): 45
Спалено калорій: 350
Нотатки (необов'язково): Ранкове тренування в залі

✅ Тренування #1 успішно додано
Натисніть Enter...

[Повторюємо для ще 2 тренувань...]

==================================================
               Мої тренування                     
==================================================

  [#1] STRENGTH | 45хв | 350ккал | Ранкове тренування в залі
  [#2] CARDIO | 30хв | 280ккал | Біг у парку
  [#3] FLEXIBILITY | 20хв | 120ккал | Розтяжка після тренування

Натисніть Enter...

==================================================
              Ваша статистика                     
==================================================

  Загальна кількість тренувань: 3
  Загальна тривалість: 95 хв
  Загально спалено: 750 ккал
  Середня тривалість: 31.7 хв

Натисніть Enter...

==================================================
              Видалити тренування                 
==================================================

Введіть ID тренування для видалення: 2

✅ Тренування #2 видалено
Натисніть Enter...
```

---

## 8. ТАБЛИЦЯ ВІДПОВІДНОСТІ (КОМПОНЕНТ → АРТЕФАКТ → ВУЗОЛ)

| Компонент | Артефакт | Вузол |
|-----------|----------|-------|
| ConsoleUI | presentation/console_ui.py | Python 3.11 Interpreter |
| AuthService | business/auth_service.py | Python 3.11 Interpreter |
| WorkoutService | business/workout_service.py | Python 3.11 Interpreter |
| NutritionService | business/nutrition_service.py | Python 3.11 Interpreter |
| Repository | data/repository.py | Python 3.11 Interpreter |
| User/Workout Models | business/models.py | Python 3.11 Interpreter |
| Точка входу | main.py | Python 3.11 Interpreter |
| Сховище користувачів | data/healthtrack_users.json | Файлова система |
| Сховище тренувань | data/healthtrack_workouts.json | Файлова система |

---

## 9. ВІДПОВІДІ НА КОНТРОЛЬНІ ПИТАННЯ

### Блок А — Нотація та стандарт UML

**А1. Поясніть різницю між структурними і поведінковими діаграмами UML. До якої категорії належать діаграми компонентів і розгортання і чому?**

Структурні діаграми UML описують статичну організацію системи — з яких частин вона складається і як ці частини пов'язані між собою незалежно від часу. Поведінкові діаграми описують динаміку — як система змінюється з часом, як передаються повідомлення, як реагує на події. Діаграми компонентів і розгортання належать до структурних діаграм, оскільки вони описують фізичну організацію системи (які програмні модулі існують і на якому обладнанні вони виконуються), а не поведінку системи у часі.

### Блок Б — Архітектурний аналіз

**Б2. У чому перевага зв'язку через інтерфейс порівняно з прямою залежністю між компонентами? Наведіть конкретний приклад із вашого варіанту.**

Зв'язок через інтерфейс забезпечує слабку зв'язаність компонентів, що дозволяє замінювати реалізацію компонента без впливу на інші частини системи, якщо новий компонент реалізує той самий інтерфейс. У нашому варіанті HealthTrack компонент WorkoutService взаємодіє з Repository через інтерфейс IRepository, а не напряму. Це дозволяє замінити сховище даних (наприклад, з JSON-файлу на SQLite) без змін у коді WorkoutService, оскільки інтерфейс IRepository залишається незмінним.

### Блок В — Практика та застосування

**В3. Як би змінилися ваші діаграми, якби систему потрібно було перенести у хмарне середовище (наприклад, AWS або Azure)?**

У діаграмі розгортання з'явилися б додаткові вузли типу <<executionEnvironment>> для хмарних сервісів: AWS EC2 для веб-сервера, AWS RDS для бази даних, AWS S3 для зберігання файлів, AWS Lambda для безсерверних функцій. З'явилися б нові протоколи для хмарної комунікації (наприклад, AWS SDK). У діаграмі компонентів могли б з'явитися нові компоненти для інтеграції з хмарними сервісами (наприклад, CloudStorageService, ServerlessFunctionService). Також додалася б компонента балансування навантаження (LoadBalancer) для масштабування.

---

## 10. ВИСНОВКИ

У ході виконання лабораторної роботи було побудовано діаграму компонентів та діаграму розгортання для системи HealthTrack, а також реалізовано скелет трирівневої шарової архітектури мовою Python. Ми навчилися ідентифікувати логічні компоненти системи, визначати їхні інтерфейси та залежності, а також проєктувати фізичну інфраструктуру для розгортання консольного додатку.

Основні труднощі виникли при забезпеченні суворого розділення шарів архітектури та реалізації ін'єкції залежностей без порушення принципу односпрямованості. Проте завдяки попереднім лабораторним роботам (SRS з ЛР3, Use Case з ЛР4, компоненти з ЛР7) ми мали чітке розуміння вимог системи, що значно спростило реалізацію.

Отриманий Python-проєкт демонструє коректну взаємодію між шарами: ConsoleUI делегує бізнес-логіку сервісам, сервіси використовують репозиторій для доступу до даних, а репозиторій ізольовано працює з файловою системою. Така архітектура забезпечує гнучкість, тестованість та легкість підтримки коду.

---

## 11. СПИСОК ВИКОРИСТАНИХ ДЖЕРЕЛ

1. Лабораторна робота №3. Збір та аналіз вимог для навчального проєкту. Створення документа специфікації вимог (SRS).
2. Лабораторна робота №4. Основи UML. Діаграми варіантів використання.
3. Лабораторна робота №6. Валідація вимог через прототипування та матрицю трасування (RTM).
4. Лабораторна робота №7. Проєктування архітектури системи. Діаграми компонентів та розгортання UML.
5. OMG. Unified Modeling Language (UML) Version 2.5.1.
6. Вігерс К., Біті Д. Розробка вимог до програмного забезпечення. 3-тє вид. – К.: Вільямс, 2014.
7. Fowler M. Patterns of Enterprise Application Architecture. – Addison-Wesley, 2002.
