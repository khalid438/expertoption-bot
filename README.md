# expertoption-bot
Bot for sending auto trading signals to Telegram
import requests
import time
import random
from telegram import Bot, InlineKeyboardButton, InlineKeyboardMarkup
from telegram.ext import Updater, CommandHandler, CallbackQueryHandler

TOKEN = "YOUR_BOT_TOKEN"
CHAT_ID = "YOUR_CHAT_ID"

bot = Bot(token=TOKEN)

# توليد توصية وهمية كمثال - يتم لاحقاً ربطه بتحليل فني حقيقي
def generate_signal():
    pair = random.choice(["EUR/USD", "GBP/USD", "USD/JPY"])
    timeframe = random.choice(["1m", "3m", "5m"])
    action = random.choice(["BUY", "SELL"])
    return f"🔔 توصية ({timeframe})
زوج: {pair}
الإجراء: {action}"

# إرسال التوصية تلقائياً
def send_auto_signal(context):
    signal = generate_signal()
    context.bot.send_message(chat_id=CHAT_ID, text=signal)

# زر يدوي لطلب توصية
def handle_signal_request(update, context):
    signal = generate_signal()
    context.bot.send_message(chat_id=update.effective_chat.id, text=signal)

def main():
    updater = Updater(token=TOKEN, use_context=True)
    dp = updater.dispatcher

    # أمر /signal لطلب توصية يدوية
    dp.add_handler(CommandHandler("signal", handle_signal_request))

    # جدولة إرسال تلقائي كل دقيقتين
    updater.job_queue.run_repeating(send_auto_signal, interval=120, first=10)

    updater.start_polling()
    updater.idle()

if __name__ == "__main__":
    main()
    python-telegram-bot==13.15
requests
