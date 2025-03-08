import logging
import requests
from telegram import Update
from telegram.ext import ApplicationBuilder, CommandHandler, ContextTypes

# Настройка логгирования
logging.basicConfig(
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
    level=logging.INFO
)

# Функция для получения данных о событиях
async def get_events(region):
    url = f"https://ff-event-nine.vercel.app/events?region={region}"
    response = requests.get(url)
    if response.status_code == 200:
        return response.json()
    else:
        return None

# Обработчик команды /events
async def events(update: Update, context: ContextTypes.DEFAULT_TYPE):
    region = context.args[0] if context.args else "CIS"  # По умолчанию регион CIS
    events_data = await get_events(region)
    
    if events_data:
        message = f"Регион: {events_data['region']}\n\n"
        for event in events_data['events']:
            message += (
                f"Событие: {event['poster-title']}\n"
                f"Начало: {event['start']}\n"
                f"Конец: {event['end']}\n"
                f"Описание: {event['desc']}\n"
                f"Постер: {event['src']}\n"
                f"Статус: {event['status']}\n\n"
            )
        await update.message.reply_text(message)
    else:
        await update.message.reply_text(f"Не удалось получить данные для региона {region}.")

# Обработчик команды /region
async def region(update: Update, context: ContextTypes.DEFAULT_TYPE):
    allowed_regions = "ID, IND, NA, PK, BR, ME, SG, BD, TW, TH, VN, CIS, EU, SAC, IND-HN"
    await update.message.reply_text(f"Allowed regions: {allowed_regions}")

# Основная функция
if __name__ == '__main__':
    application = ApplicationBuilder().token("7296645272:AAHuU1LfmZ2mce6R-6TOYQxRD_WTLCy2zWU").build()
    
    # Регистрация обработчиков команд
    application.add_handler(CommandHandler("events", events))
    application.add_handler(CommandHandler("region", region))
    
    # Запуск бота
    application.run_polling()
