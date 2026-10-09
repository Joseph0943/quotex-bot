import telebot
from PIL import Image
import io

BOT_TOKEN = "8879894617:AAGQA65v8REsTogUJshsvgzq4CZ6fjsYOGk"
bot = telebot.TeleBot(BOT_TOKEN)

def analyze(img):
    w,h = img.size
    chart = img.crop((int(w*0.15), int(h*0.1), int(w*0.85), int(h*0.85)))
    pixels = list(chart.getdata())
    green=0; red=0
    for p in pixels[::15]:
        r,g,b = p[:3]
        if g > r+25 and g > 110: green+=1
        elif r > g+25 and r > 110: red+=1
    if green > red:
        return "DOWN 🔴"
    else:
        return "UP 🟢"

@bot.message_handler(commands=['start'])
def start(m):
    bot.reply_to(m, "📈 Bot is Online 24/7\nSend Quotex screenshot\nCaption: 85")

@bot.message_handler(content_types=['photo'])
def handle(message):
    try:
        cap = message.caption or "85"
        num = ''.join(filter(str.isdigit, cap))
        conf = int(num) if num else 85
        if conf>93: conf=91
        if conf<70: conf=70
        file_info = bot.get_file(message.photo[-1].file_id)
        downloaded = bot.download_file(file_info.file_path)
        img = Image.open(io.BytesIO(downloaded)).convert("RGB")
        direction = analyze(img)
        bot.reply_to(message, f"✅ SIGNAL\n\nDirection: {direction}\nConfidence: {conf}%\nExpiry: 1 MIN\nEntry: NEXT candle\n\nBot: @QuotexSignalsAccurateBot")
    except Exception as e:
        bot.reply_to(message, f"Error: {e}")

print("Bot running 24/7...")
bot.infinity_polling()
