# barcode-scanner[README.md](https://github.com/user-attachments/files/21749443/README.md)
# Telegram WebApp сканер → Excel

Цей комплект дає сканування штрихкодів через камеру телефону у Telegram та запис у локальний Excel.

## Склад
- `index.html` — веб-сканер (ZXing + Telegram WebApp SDK)
- `bot_webapp_excel.py` — бот на aiogram 3.7+, що приймає коди та пише у Excel
- `.env.example` — приклад змінних середовища

## Вимоги
- Python 3.10+
- Пакети: `aiogram>=3.7`, `openpyxl`
- HTTPS хостинг для `index.html` (наприклад, ngrok або GitHub Pages)

## Варіант A — швидкий запуск із ngrok (рекомендовано для тесту)
1. Встановіть залежності:
   ```bash
   python -m pip install --upgrade pip
   python -m pip install aiogram==3.7.0 openpyxl
   ```
2. Запустіть локальний сервер у папці з `index.html`:
   ```bash
   python -m http.server 8080
   ```
3. У новому терміналі запустіть ngrok:
   ```bash
   ngrok http 8080
   ```
   Скопіюйте `https://....ngrok-free.app` із вікна ngrok.
4. Запустіть бота (macOS/Linux):
   ```bash
   export BOT_TOKEN="ВАШ_ТОКЕН"
   export EXCEL_PATH="/Users/mero/Desktop/boxes.xlsx"
   export WEBAPP_URL="https://....ngrok-free.app/index.html"
   python bot_webapp_excel.py
   ```
5. В Telegram відкрийте бота, натисніть **«Сканувати штрихкод»**, дозвольте камеру та скануйте.

## Варіант B — GitHub Pages (постійний HTTPS)
1. Створіть публічний репозиторій і покладіть туди `index.html` у корінь.
2. Ввімкніть **Settings → Pages → Deploy from branch → main / root**.
3. Отримаєте адресу `https://<user>.github.io/<repo>/index.html`.
4. Запустіть бота з цією адресою в `WEBAPP_URL`:
   ```bash
   export WEBAPP_URL="https://<user>.github.io/<repo>/index.html"
   python bot_webapp_excel.py
   ```

## Примітки
- Тримайте Excel-файл **закритим** під час роботи бота.
- Антидубль: якщо код уже є у файлі — бот повідомить і не дублюватиме.
- Кнопка **Ліхтар** працює на пристроях, які підтримують `torch` у WebRTC.
- За потреби можна фільтрувати формат коду (довжина/маска) у коді бота перед записом.
