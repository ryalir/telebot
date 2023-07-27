import os

import telebot
import requests
from telebot import TeleBot

from dotenv import load_dotenv

project_folder = os.path.expanduser('/home/lungiprasad')

load_dotenv(os.path.join(project_folder, '.env'))

#BOT_TOKEN = os.environ.get('BOT_TOKEN')
BOT_TOKEN = os.getenv('BOT_TOKEN')

bot: TeleBot = telebot.TeleBot(BOT_TOKEN)


@bot.message_handler(commands=['start', 'hello'])
def send_welcome(message):
    bot.reply_to(message, "Hi, Lungi boy how are you doing?")

'''
@bot.message_handler(func=lambda msg: True)
def echo_all(message):
    bot.reply_to(message, message.text)
'''

@bot.message_handler(commands=['enquiry'])
def sign_handler(message):
    text = "What's your rollnumber?"
    sent_msg = bot.send_message(message.chat.id, text, parse_mode="Markdown")
    bot.register_next_step_handler(sent_msg, fetch_horoscope)

def day_handler(message):
    sign = message.text
    text = "What day do you want to know?\nChoose one: *TODAY*, *TOMORROW*, *YESTERDAY*, or a date in format YYYY-MM-DD."
    sent_msg = bot.send_message(
        message.chat.id, text, parse_mode="Markdown")
    bot.register_next_step_handler(
        sent_msg, fetch_horoscope, sign.capitalize())

def fetch_horoscope(message):
    rollnumber = message.text
    horoscope = get_daily_horoscope(rollnumber)
    data = horoscope["data"]
    horoscope_message = f'*Backlogs:* {data["result"]}'
    bot.send_message(message.chat.id, "Here's your result!")
    bot.send_message(message.chat.id, horoscope_message, parse_mode="Markdown")


def get_daily_horoscope(rollnumber: str) -> dict:
    """Get daily horoscope for a zodiac sign.
    Keyword arguments:
    rollnumber:int - Student rollnumber
    day:str - Date in format (YYYY-MM-DD) OR TODAY OR TOMORROW OR YESTERDAY
    Return:dict - JSON data
    """
    url = "https://idealtech.edu.in/r16uploads/backlogapi.php"
    params = {"rollnumber": rollnumber}
    response = requests.get(url, params)

    return response.json()


bot.infinity_polling()
