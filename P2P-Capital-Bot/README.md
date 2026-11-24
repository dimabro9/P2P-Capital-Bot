# P2P Capital Bot — без Mini App

Премиальный Telegram-бот на aiogram 3. Интерфейс — inline-кнопки прямо под сообщениями: переходы обновляют одно сообщение вместо создания новых экранов. Введённые пользователем сообщения бот пытается удалять после обработки, чтобы уменьшить количество сообщений в чате.
Поддерживаются только USDT и GRAM. TON и Mini App удалены.

## Функции
- Украинский, русский и английский языки.
- Заявки на покупку/продажу USDT и GRAM.
- Проверка наличия при покупке: если остаток равен нулю или актив отключён, бот сообщает «нет в наличии» и не создаёт заявку.
- Проверка количества и лимита покупки 10 000 UAH.
- Администратор получает каждую заявку с ID клиента, username и ссылкой на Telegram-профиль.
- Администратор может написать клиенту командой `/reply USER_ID текст`.
- Статусы: `/status ORDER_ID ACCEPTED` (также NEW, PROCESSING, COMPLETED, CANCELLED, REJECTED).
- Управление курсами и наличием в админ-панели.
- Отдельный раздел «Поддержка» внутри личного чата с ботом: пользователь пишет туда, администратор получает обращение с профилем и кнопкой «Ответить клиенту», ответ приходит в тот же диалог. Пользователь может продолжить переписку через кнопку ответа.
- Кнопки навигации закреплены под сообщением; основные экраны редактируются, а не дублируются. Кнопки «Назад» возвращают в меню.
- Бот пытается удалять текстовые сообщения пользователя после обработки; если Telegram не разрешит удаление, сценарий продолжит работать.

## Установка (Windows)
```powershell
py -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
```
Скопируй `.env.example` в `.env`, укажи токен и Telegram ID администратора, затем:
```powershell
python bot.py
```

## Настройка администратора
В `.env`:
- `BOT_TOKEN` — токен BotFather.
- `ADMIN_IDS` — Telegram ID администраторов через запятую.
- `REVIEWS_URL` — существующий канал отзывов.
- `SUPPORT_USERNAME` — username менеджера без `@`.

## Управление
В админ-панели:
- Курсы: `USDT 45 43` или `GRAM 66 61`.
- Наличие: `USDT 1500` или `GRAM 25000`. Ноль отключает покупку.
- Ответ клиенту: `/reply USER_ID Текст ответа`.
- Статус: `/status ORDER_ID ACCEPTED`.

## Важно
Это система сбора заявок и связи с оператором, а не платёжная система.
Она не выполняет обмен или переводы и не запрашивает приватные ключи, пароли,
CVV или коды подтверждения. Демо-остатки по умолчанию равны нулю.
Перед публичным использованием необходимы аудит, правовая проверка и проверка
всех сценариев в тестовом Telegram-боте.

## Дизайн меню и Telegram Premium Emoji

- Главное меню открывается с баннером `assets/menu_banner.png`.
- Переходы по inline-кнопкам редактируют caption баннера/текущего сообщения, а не создают отдельные сообщения.
- Админ-панель показывает статистику пользователей, заявок и обращений, а также разделы заявок, поддержки, курсов и остатков.
- Для Telegram Premium custom emoji можно указать ID в `.env`: `PREMIUM_EMOJI_DIAMOND_ID`, `PREMIUM_EMOJI_EXCHANGE_ID`, `PREMIUM_EMOJI_CHART_ID`, `PREMIUM_EMOJI_SUPPORT_ID`, `PREMIUM_EMOJI_LOCK_ID`. Если ID не заданы, используются обычные emoji. Telegram custom emoji применяются в тексте и подписях сообщений; Telegram inline-кнопки не поддерживают custom emoji entities.

Бот остаётся системой приёма заявок: он не принимает оплату и не выполняет переводы.

### Telegram Premium Custom Emoji

Copy `.env.example` to `.env`, then fill the `PREMIUM_EMOJI_*_ID` variables with numeric Custom Emoji IDs. The project includes a separate variable for every emoji symbol currently used in bot messages. Leave a value empty to keep its normal Unicode emoji. These are Custom Emoji IDs (not sticker file IDs). Telegram Bot API inline keyboard button labels do not support custom emoji entities, so configured Premium Emoji are rendered in message text and photo captions. Restart the bot after editing `.env`.


## Обновлённое Premium-меню

- Обновлён главный экран в RU/UK/EN: более выразительная шапка P2P CAPITAL PREMIUM.
- Кнопки меню получили единый стиль с тематическими emoji и ⭐.
- Добавлена настройка `PREMIUM_EMOJI_STAR_ID` для Premium Custom Emoji звезды в тексте и подписи меню.
- Важно: Telegram Bot API не поддерживает Premium Custom Emoji entities внутри inline-кнопок. Поэтому на самих кнопках используются обычные Unicode emoji/звезда, а Premium-звезда отображается в тексте меню.
- База `data/p2p_capital.db` и таблица `inventory` не изменялись. Все сохранённые остатки остаются в исходной базе.
