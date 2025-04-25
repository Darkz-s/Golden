import logging
import requests
import random
import webbrowser
from telegram import Update
from telegram.ext import Application, MessageHandler, filters, CallbackContext, CommandHandler
import json
webbrowser.open(' ')
print(" انضم لقناتي ولك 🙂")
BOT_TOKEN = '7370738476:AAEY7BAXxcQC3oN2shmEuHrJEqNS4xFA6L4'

GEMINI_API_KEY = 'AIzaSyC9F7-JJ2jHd4SA4Qo90AwzKhrgHBpPn0A'

#لا تبعبص عوفه 

logging.basicConfig(format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
                    level=logging.INFO)
logger = logging.getLogger(__name__)

WELCOME_MESSAGES = [
    "Hi, I'm Fastest Ai bot by @top005. Send anything and I will answer you."
]

UNKNOWN_RESPONSES = [
    "Sorry, I didn't understand your question. Could you please clarify?",
    "I couldn't understand that. Could you rephrase your question?"
]

async def chat_with_gemini(question: str) -> str:
    try:
        url = f"https://generativelanguage.googleapis.com/v1beta/models/gemini-1.5-flash:generateContent?key={GEMINI_API_KEY}"
        headers = {'Content-Type': 'application/json'}
        
        payload = {
            "contents": [{
                "parts": [{"text": question}]
            }]
        }

        response = requests.post(url, headers=headers, data=json.dumps(payload))

        if response.status_code == 200:
            response_data = response.json()
            logger.info(f"API Response: {response_data}")
            
            if 'candidates' in response_data and len(response_data['candidates']) > 0:
                candidate = response_data['candidates'][0]
                if 'content' in candidate and 'parts' in candidate['content']:
                    return candidate['content']['parts'][0].get('text', random.choice(UNKNOWN_RESPONSES))
                else:
                    logger.error("No text parts found in content.")
                    return random.choice(UNKNOWN_RESPONSES)
            else:
                logger.error("No candidates found in response.")
                return random.choice(UNKNOWN_RESPONSES)
        else:
            logger.error(f"Failed to connect to Gemini API. Response status: {response.status_code}")
            return "There was an error while communicating with the server, please try again later."

    except requests.exceptions.RequestException as e:
        logger.error(f"Error while sending request: {e}")
        return "There was an error while communicating with the server, please try again later."

async def handle_message(update: Update, context: CallbackContext) -> None:
    user_message = update.message.text
    logger.info(f"Received message from user: {user_message}")

    #تكدر تغير الامر
    if "من هو مطورك" in user_message.lower():
        response = "أنا مطور بواسطة GAETH المحترم، قائدنا وداعمنا الكبير. شكراً جزيلاً له على تطوير هذا البوت الرائع!"
        #تكدر تغير رد هم الي يعجبك
    else:
        response = await chat_with_gemini(user_message)
    await update.message.reply_text(response)

async def welcome_message(update: Update, context: CallbackContext) -> None:
    welcome_text = "مرحبًا! أنا بوت الذكاء الاصطناعي. أرسل أي شيء وسأجيبك فورًا!"
    video_url = "https://t.me/ili1llii/9"  # رابط الفيديو المطلوب


    await update.message.reply_video(video_url, caption=welcome_text)

async def get_file_id(update: Update, context: CallbackContext):
    if update.message.video:
        await update.message.reply_text(f"File ID: {update.message.video.file_id}")
    else:
        await update.message.reply_text("لم يتم إرسال فيديو.")

def main():
    application = Application.builder().token(BOT_TOKEN).build()

    
    application.add_handler(CommandHandler("start", welcome_message))
    application.add_handler(MessageHandler(filters.TEXT & ~filters.COMMAND, handle_message))
    application.add_handler(MessageHandler(filters.VIDEO, get_file_id))

    application.run_polling()

if __name__ == '__main__':
    main()
