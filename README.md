# Звіт з виконання Work-case №2
**Дисципліна:** Фокін Степан Володимирович  
**Студент:** [Твоє Прізвище] Тимур
**Група:** РПЗ-33
**Роль:** Technical Support / Reporter

---

## 1. Встановлення гіпервізора (Завдання 1)
**Мета:** Встановити на робочій станції гіпервізор ІІ типу.

Для виконання лабораторної роботи я обрав **Oracle VirtualBox**, оскільки це безкоштовне програмне забезпечення з відкритим вихідним кодом, яке ідеально підходить для навчальних цілей та підтримує широкий спектр гостьових ОС.

<p align="center">
  <img src="assets/Picture1.png" alt="Головне вікно VirtualBox">
</p>

* **Версія ПЗ:** VirtualBox 7.2.6 
* **Хост-система:** Windows 11
* **Статус:** Встановлено успішно.

---

## 2. Базові налаштування гіпервізора (Завдання 2)

### 2.1. Створення нової віртуальної машини
Мною було створено нову віртуальну машину (ВМ) з назвою "Ubuntu24.04
* **Тип:** Linux
* **Версія:** Ubuntu (64-bit)

<p align="center">
  <img src="assets/Picture2.png" alt="Створення віртуальної машини">
</p>

### 2.2. Налаштування обладнання
Для стабільної роботи гостьової ОС я виділив наступні ресурси:
* **Оперативна пам'ять (RAM):** 8192
* **Процесор:** 4 ядра

### 2.3. Налаштування мережі та Wi-Fi
Щоб віртуальна машина мала прямий доступ до мережі Інтернет як окремий пристрій:
* У налаштуваннях мережі я змінив тип підключення з **NAT** на **Проміжний адаптер** (Bridged Adapter).
* У полі "Назва" обрав свій фізичний Wi-Fi адаптер, щоб віртуальна машина використовувала бездротове з'єднання хоста.
* Це дозволяє ВМ отримувати власну IP-адресу від роутера та бути повноцінним учасником локальної мережі.

<p align="center">
  <img src="assets/Picture_Network.png" alt="Налаштування проміжного адаптера">
</p>

### 2.4. Робота із зовнішніми носіями (USB)
Для повноцінної роботи з USB-накопичувачами (флешками) у віртуальному середовищі необхідно активувати підтримку контролерів USB 2.0/3.0.

**Алгоритм налаштування (Host OS: Windows):**

1.  **Встановлення VirtualBox Extension Pack:**
    * Завантажив необхідний пакет розширень (`.vbox-extpack`) з офіційного сайту Oracle: [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads).
    * Встановив його подвійним кліком по файлу, підтвердивши дію в інтерфейсі VirtualBox та погодившись із ліцензійною угодою (License Agreement).

<p align="center">
  <img src="assets/Picture7.png" alt="Встановлення Extension Pack">
</p>

2.  **Активація контролера:**
    * У налаштуваннях віртуальної машини перейшов у розділ "USB".
    * Активував опцію **USB 3.0 (xHCI) Controller**.

3.  **Додавання пристрою:**
    * Створив фільтр для свого фізичного накопичувача (натиснувши на іконку "USB із зеленим плюсом").
    * Це дозволило "прокинути" флешку всередину віртуальної машини, від'єднавши її від основної системи Windows.
---

## 3. Встановлення ОС з графічною оболонкою (Завдання 3)
**Мета:** Встановити GNU/Linux у базовій конфігурації (GUI).

На першій віртуальній машині я встановив **Ubuntu 24.04 LTS**. Інсталяція проходила у штатному режимі, після завершення я отримав готовий робочий стіл GNOME.

<p align="center">
  <img src="assets/Picture3.png" alt="Робочий стіл Ubuntu Desktop">
</p>

---

## 4. Робота з другою ВМ: CLI та графічні оболонки (Завдання 4)

### 4.1. Встановлення у мінімальній конфігурації
Створив другу ВМ та встановив **Ubuntu Server 24.04**.
* **Особливість:** Система не має графічного інтерфейсу, керування відбувається через командний рядок (CLI).

<p align="center">
  <img src="assets/Picture4.png" alt="Консольний інтерфейс CLI">
</p>

### 4.2. Встановлення оболонки GNOME
Для додавання графічного інтерфейсу я використав команду:
`sudo apt install ubuntu-desktop`
Після перезавантаження система запустилась у графічному режимі GNOME.

<p align="center">
  <img src="assets/Picture5.png" alt="Встановлений GNOME">
</p>

### 4.3. Встановлення другої оболонки (XFCE) та порівняння
Для виконання порівняння я встановив легковагове середовище XFCE командою:
`sudo apt install xubuntu-desktop`

**Порівняльна таблиця:**

| Характеристика | GNOME (За замовчуванням) | XFCE (Додаткова) |
| :--- | :--- | :--- |
| **Інтерфейс** | Сучасний, адаптований під сенсори. | Класичний (Пуск, панель завдань). |
| **Ресурси** | Споживає більше RAM та CPU. | Дуже легка та швидка. |
| **Призначення** | Сучасні потужні ПК. | Слабкі ПК, сервери, ВМ. |

<p align="center">
  <img src="assets/Picture6.png" alt="Встановлений XFCE">
</p>

---

## 5. Glossary (English Terminology)

* **Hypervisor (Type 2):** Software that creates and runs virtual machines on a physical host machine (e.g., Oracle VirtualBox).
* **CLI (Command Line Interface):** A text-based user interface used to view and manage computer files.
* **GUI (Graphical User Interface):** A user interface that allows users to interact with electronic devices through graphical icons and audio indicator such as primary notation.
* **Open Source:** Source code that is made freely available for possible modification and redistribution.
* **Repository:** A central file storage location used by version control systems or package management systems.

---

## 6. Conclusions
In this work-case, I successfully deployed virtual environments using Oracle VirtualBox.
I mastered two types of Linux installation:
1.  **Standard installation** with a pre-configured graphical interface.
2.  **Minimal installation (CLI)** with manual setup of desktop environments.

I compared **GNOME** and **XFCE** desktop environments. The practice showed that while GNOME offers a modern user experience, XFCE is significantly more efficient for virtual machines due to lower resource consumption.
