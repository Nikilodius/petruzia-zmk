<div align="center">

# ⌨️ PetruziaMini ZMK

**Конфигурация ZMK-прошивки для самодельной 40% беспроводной клавиатуры**

[![Firmware](https://img.shields.io/badge/Firmware-ZMK-3C3C3C?style=flat-square&logo=zephir&logoColor=white)](https://zmk.dev/)
[![MCU](https://img.shields.io/badge/MCU-nRF52840-00A9CE?style=flat-square)](https://www.nordicsemi.com/)
[![Build](https://img.shields.io/badge/Build-GitHub%20Actions-2088FF?style=flat-square&logo=githubactions&logoColor=white)](../../actions)
[![Keyboard](https://img.shields.io/badge/Layout-40%25%20%7C%2036%20keys-FF5722?style=flat-square)](#-раскладка)

Собирается автоматически в GitHub Actions — не нужен ни локальный тулчейн, ни Docker.

[Обзор](#-обзор) • [Раскладка](#-раскладка) • [Сборка](#-сборка-прошивки) • [Железо](#-железо) • [3D-модель](#-3d-модель-корпуса) • [Структура](#-структура-проекта)

</div>

---

## 📖 Обзор

**PetruziaMini** — компактная ортолинейная клавиатура формата **40%** (36 клавиш) на беспроводном контроллере **nRF52840** в форм-факторе Pro Micro. Этот репозиторий содержит конфигурацию прошивки [ZMK](https://zmk.dev/) и собирает готовые `.uf2`-файлы прямо в GitHub Actions.

| | |
|---|---|
| 🔢 **Размер** | 40%, 36 клавиш, 10 колонок × 4 ряда |
| 📡 **Подключение** | Bluetooth LE (батарея, без проводов) |
| 🧠 **Контроллер** | nRF52840 в форм-факторе Pro Micro |
| 🔀 **Матрица** | 10×4, диоды COL2ROW |
| 🎛 **Настройка раскладки** | ZMK Studio — прямо в браузере, без перепрошивки |

## 🖨 3D-модель корпуса

Корпус — **handwired** (без печатной платы, матрица разводится проводами вручную). 3D-модель для печати выложена на MakerWorld:

> 👉 **[PetruziaMini 40% Handwired Mechanical Keyboard](https://makerworld.com/ru/models/476290-petruziamini-40-handwired-mechanical-keyboard)**

Там же можно скачать STL-файлы для печати, посмотреть фото собранной клавиатуры и профиль автора.

## ⌨️ Раскладка

### Base — QWERTY

```
┌───┬───┬───┬───┬───┬───┬───┬───┬───┬───┐
│ Q │ W │ E │ R │ T │ Y │ U │ I │ O │ P │
├───┼───┼───┼───┼───┼───┼───┼───┼───┼───┤
│ A │ S │ D │ F │ G │ H │ J │ K │ L │ ↵ │
├───┼───┼───┼───┼───┼───┼───┼───┼───┼───┤
│ Z │ X │ C │ V │ B │ N │ M │ , │ . │Fn │
├───┴─┬─┴─┬─┴─┬─┴───┴─┬─┴───┴─┬─┴───┼───┤
│Ctrl │GUI│Alt│ Space │ Bksp  │ Tab │
└─────┴───┴───┴───────┴───────┴─────┘
```

### Fn-слой

- Клавиша **`.`** — hold-слой Fn (зажми — активен Fn-слой)
- В Fn-слое на её месте — обычный **`/`** (SLASH)
- Вместо `Ctrl` — **`studio_unlock`** 🔓 для разблокировки ZMK Studio

### Слои

| Слой | Статус | Назначение |
|---|---|---|
| `Base` | активный | Обычная QWERTY |
| `Fn` | активный | Служебные клавиши, `/`, разблокировка Studio |
| `extra1` / `extra2` | 🟡 зарезервированы | ZMK Studio сможет включать и редактировать |

## 🛠 Сборка прошивки

Никакой локальной сборки не требуется — всё делает GitHub Actions:

1. Сделай **Fork** этого репозитория (или используй свой)
2. Открой вкладку **Actions** → workflow **Build ZMK firmware**
3. Дождись зелёной галочки ✅
4. Скачай **Artifacts** — внутри будет `petruzia uf2` файл

<details>
<summary><b>⚡ Прошивка контроллера</b></summary>

1. Подключи nRF52840 по USB
2. Дважды быстро нажми кнопку **RESET** — появится диск `NICENANO` (или аналогичный)
3. Скопируй `.uf2` файл на этот диск
4. Контроллер сам перезагрузится с новой прошивкой 🎉
5. Клавиатура подключится к компьютеру по Bluetooth автоматически — добавь её в списке устройств

</details>

## 🔌 Железо

Распиновка для контроллера на **nRF52840** (Pro Micro-совместимый):

<details>
<summary><b>📍 Пины матрицы</b></summary>

**Колонки (10):**

```
P1.00, P0.24, P0.22, P0.20, P0.17, P0.31, P0.29, P0.02, P1.15, P1.13
```

**Ряды (4):**

```
P1.11, P0.11, P1.04, P1.06
```

**Направление диодов:** COL2ROW

> ⚠️ Перед прошивкой проверь свою распайку — порядок пинов должен совпадать с фактической разводкой платы.

</details>

## 🗂 Структура проекта

```
petruzia-zmk/
├── 🤖 .github/workflows/
│   └── build.yml              # GitHub Actions: сборка прошивки
├── ⚙️ config/
│   ├── petruzia.keymap        # Раскладка: слои Base / Fn / extra
│   ├── petruzia.overlay       # Матрица, пины, физическая раскладка
│   ├── west.yml               # Манифест ZMK (зависимости)
│   └── boards/shields/
│       └── petruzia/          # Описание клавиатуры (shield)
│           ├── Kconfig.shield
│           └── petruzia.overlay
├── 🏗 build.yml / build.yaml  # Конфигурация сборки
└── 📄 README.md
```

## 📚 Полезные ссылки

- 🖨 [PetruziaMini на MakerWorld](https://makerworld.com/ru/models/476290-petruziamini-40-handwired-mechanical-keyboard) — 3D-модель корпуса
- 🌐 [Документация ZMK](https://zmk.dev/docs) — поведение клавиш, слои, биндинги
- 🎛 [ZMK Studio](https://zmk.dev/docs/features/studio) — редактирование раскладки без перепрошивки
- 💬 [Discord ZMK](https://zmk.dev/community/discord) — помощь сообщества
- 📦 [Список поддерживаемых биндингов](https://zmk.dev/docs/keymaps)

---

<div align="center">

**⭐ Если собираешь PetruziaMini — звезда очень поможет проекту! ⭐**

</div>
