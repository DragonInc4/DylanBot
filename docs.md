# Привет вы попали в документацию Dylan bot и вы явно хотите написать код
приступим!
модули для моего бота пишутся на Пайтон, в нем используется библиотека telebot asyncio (не гнобите)
итак! начнем!
# Пример модуля "Кликер"

## 1. Импортируете апи или что то другое
 `import dylansigmakotik `

## 2. Глобальные переменные
```
class global:
    klik = 100
```
klik - Количество коинов за один клик

## 3. Добавление информации о модуле
{}-необязательный параметр

[]-обязательный параметр
```
"клик": { # Сама команда клик баланс клик повтори клик котик
    "id": "klik",  # Идентификатор команды
    "title": "Кликер",  # описание команды
    "description": "клик (баланс/повтори/котик) (текст)",  # как ее использовать
    "message_text": "Кликаю..."  # Текст, который будет отправлен при выборе команды
},
```
## 4. Код модуля
```
elif chosen.result_id == "klik":
    user_id = chosen.from_user.id
    if not args:
        balance = load(user_id, "klik_balance", 0)  # Получаем баланс пользователя
        await edit(chosen, f"Вы кликнули и получили {Settings.klik} коинов!")
        balance += Settings.klik  # Увеличиваем баланс
        save(user_id, "klik_balance", balance)  # Сохраняем новый баланс
    elif args[0].lower() == "баланс":
        balance = load(user_id, "klik_balance", 0)  # Получаем баланс
        await edit(chosen, f"Ваш баланс: {balance}")
    elif args[0].lower() == "повтори":
        text = " ".join(args[1:])  # Превращаем команду: повтори крутой текст в крутой текст
        await edit(chosen, f"*{text}*")
    elif args[0].lower() == "котик":
        markup = InlineKeyboardMarkup()
        markup.add(InlineKeyboardButton("ЕЩЕ БОЛЬШЕ КОТИКОВ", url="https://yandex.ru/images/search?from=tabbar&text=картинка%20котика"))#
        # кнопка ссылка на больше котиков
        await edit(chosen, f"Вот ваш котик:", photo="https://i.pinimg.com/originals/c0/2d/11/c02d11b807f28927def41b6346cb6da0.jpg", markup=markup)
        #markup это добавить кнопки(пока что только url или без каллбека), photo это ссылка на фото чтобы оно отобразилось
    else:
        await edit(chosen, f"Я немного не понял команду {fulltext}\nЕсли что, я умею показывать баланс: клик баланс и просто клик а также клик повтори [текст].")
        #fulltext это полная команда без клик тоесть типа повтори кошкам кошкас кошкаы
        #args это ['повтори', 'кошкам', 'кошкас', 'кошкаы']
        #args[2]=кошкас
```
# ОЧЕНЬ ВАЖНО!
```
balance = await load(user_id, "klik_balance", 0)
    """Загружает данные из базы данных.
    
    Параметры:
    - user_id: Уникальный идентификатор пользователя.
    - key: Название переменной в базе данных.
    - default: Значение по умолчанию, если данные не найдены.
    
    Возвращает:
    - Загруженное значение или значение по умолчанию.
    """

await save(user_id, "klik_balance", balance)
"""Сохраняет данные в базе данных.
    
    Параметры:
    - user_id: Уникальный идентификатор пользователя.
    - key: Название переменной в базе данных.
    - value: Значение, которое нужно сохранить.
    """
```
# Клавиатура в боте — просто, если знать формат моего бота
### ВАЖНО: callback_data = названиекоманды_данные_данные2_userid
```
text = "рикрол"
user_id = chosen.from_user.id  # кто вызвал — тот и владелец кнопки

markup = InlineKeyboardMarkup()
markup.add(
    InlineKeyboardButton(
        "кнопка", 
        callback_data=f"click_{text}_{user_id}"  # формат: действие_параметр_юзерайди
    )
)
await edit(chosen, "нажми на кнопкульку", markup=markup)
```
# === ОБРАБОТЧИК КНОПКИ ===
```
if data.startswith('click_'):
    try:
        param = data.split('_', 2)[1]#параметр тоесть между действем click_ и _юзерайди

        # Обработка: например, показ текста
        await edit(call, param)  # показываем параметр это надпись рикрол

        # Подтверждение нажатия (чтобы исчезло "вращение")
        await bot.answer_callback_query(call.id)
        # ИЛИ: await bot.answer_callback_query(call.id, "Готово!", show_alert=True) чтобы человек увидел сообщение ну вы знаете

    except (IndexError, ValueError):
        await bot.answer_callback_query(call.id, "❌ Данные повреждены", show_alert=True)
    return
```
