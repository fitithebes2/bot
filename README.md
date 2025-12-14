# bot
import Update
from telegram.ext import ApplicationBuilder, CommandHandler, ContextTypes, MessageHandler, filters

TOKEN = "8581170892:AAF5bk5WrpTozwgHyuHSidyXv1aE6KGZB4s"


async def hello(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    await update.message.reply_text(f'Hello {update.effective_user.first_name}')


async def text(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    text = update.message.text
    await update.message.reply_text(f'Hello {update.effective_user.first_name}! You said: "{text}"')


app = ApplicationBuilder().token(TOKEN).build()

app.add_handler(CommandHandler("hello", hello))
app.add_handler(MessageHandler(filters.TEXT & ~filters.COMMAND, text))
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
