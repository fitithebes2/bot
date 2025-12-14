# bot
from telegram import Update, ReplyKeyboardMarkup
from telegram.ext import (
    ApplicationBuilder,
    CommandHandler,
    MessageHandler,
    ContextTypes,
    filters
)

TOKEN = "8581170892:AAF5bk5WrpTozwgHyuHSidyXv1aE6KGZB4s"

async def start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    keyboard = [
        ["(мем)  Кіт"],
        ["(мем)  Антон чігур нікого не вбивав"],
        ["(мем)  Вчимо німецьку мову"],
        ["(фільм) Початок"],
        ["(фільм) TENET"],
        ["(фільм) Interstelar"],
        ["(стікер) Баскетбол"],
        ["/start"]
    ]

    reply_keyboard = ReplyKeyboardMarkup(
        keyboard,
        resize_keyboard=True,
        one_time_keyboard=False
    )

    await update.message.reply_text(
        "Обери відео 👇 для оновлення натисни /start",
        reply_markup=reply_keyboard
    )

async def handle_text(update: Update, context: ContextTypes.DEFAULT_TYPE):
    if update.message.text == "(мем)  Кіт":
        await update.message.reply_text("https://www.youtube.com/watch?v=k-oMyXtrgME")

    elif update.message.text == "(мем)  Антон чігур нікого не вбивав":
        await update.message.reply_text("https://www.youtube.com/watch?v=VY--3seIhtA")

    elif update.message.text == "(мем)  Вчимо німецьку мову":
        await update.message.reply_text("https://www.youtube.com/watch?v=g6RFx7LlmHM&list=RDg6RFx7LlmHM&start_radio=1")

    elif update.message.text == "(фільм) Початок":
        await update.message.reply_text("https://uakino.best/filmy/genre-action/2-pochatok/download.html")

    elif update.message.text == "(фільм) TENET":
        await update.message.reply_text("https://uaserials.com/1634-tenet-2020.html")

    elif update.message.text == "(фільм) Interstelar":
        await update.message.reply_text("https://uaserials.my/2038-interstellar-2014.html")

    elif update.message.text == "(стікер) Баскетбол":
        await update.message.reply_text("🏀")


if __name__ == "__main__":
    app = ApplicationBuilder().token(TOKEN).build()

    app.add_handler(CommandHandler("start", start))
    app.add_handler(MessageHandler(filters.TEXT & ~filters.COMMAND, handle_text))

    print("Бот запущено")
    app.run_polling()


    
#weather bot

import logging
import requests
from telegram import Update
from telegram.ext import Application, CommandHandler, ContextTypes


TELEGRAM_TOKEN = "8581170892:AAF5bk5WrpTozwgHyuHSidyXv1aE6KGZB4s"

WEATHER_API_KEY = "YOUR_OPENWEATHERMAP_API_KEY"


logging.basicConfig(
    format="%(asctime)s - %(name)s - %(levelname)s - %(message)s", level=logging.INFO
)

def get_weather(city: str) -> str:
    url = f"http://api.openweathermap.org/data/2.5/weather?q={city}&appid={WEATHER_API_KEY}&units=metric&lang=ru"
    response = requests.get(url)
    if response.status_code == 200:
        data = response.json()
        temp = data["main"]["temp"]
        desc = data["weather"][0]["description"]
        return f"🌤 Погода в {city}:\nТемпература: {temp}°C\nОписание: {desc.capitalize()}"
    else:
        return "Не удалось получить данные. Проверь название города."


async def start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text("Привет! Я Weatherman_bot 🌦\nНапиши /weather <город>, чтобы узнать погоду.")


async def weather(update: Update, context: ContextTypes.DEFAULT_TYPE):
    if context.args:
        city = " ".join(context.args)
        forecast = get_weather(city)
        await update.message.reply_text(forecast)
    else:
        await update.message.reply_text("Используй команду так: /weather <город>")

def main():
    app = Application.builder().token(TELEGRAM_TOKEN).build()

    app.add_handler(CommandHandler("start", start))
    app.add_handler(CommandHandler("weather", weather))

    print("Бот запущен...")
    app.run_polling()

if __name__ == "__main__":
    main()
