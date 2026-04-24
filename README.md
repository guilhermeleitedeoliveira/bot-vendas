# bot-vendas
import telebot
import random
import time

import os
TOKEN = os.getenv("8694943382:AAGv_C5vEloZIoC9Bob0RsnfWw3F9rRA3Kg")

produtos = [
    {
        "nome": "Fone Bluetooth",
        "preco": "R$49,90",
        "link": "SEU_LINK_AFILIADO"
    },
]

def gerar_post(produto):
    frases = [
        "🚨 PROMOÇÃO IMPERDÍVEL",
        "🔥 OFERTA RELÂMPAGO",
        "💥 DESCONTO HOJE"
    ]
    
    return f"""
{random.choice(frases)}

{produto['nome']} por apenas {produto['preco']} 😱

✔ Alta qualidade
✔ Estoque limitado

👉 Compre agora:
{produto['link']}
"""

@bot.message_handler(commands=['start'])
def start(msg):
    bot.reply_to(msg, "🤖 Bot ativo! Use /post para enviar oferta.")

@bot.message_handler(commands=['post'])
def postar(msg):
    produto = random.choice(produtos)
    texto = gerar_post(produto)
    bot.send_message(msg.chat.id, texto)

def auto_post():
    while True:
        produto = random.choice(produtos)
        texto = gerar_post(produto)
        bot.send_message("SEU_CANAL_ID", texto)
        time.sleep(3600)

import threading
threading.Thread(target=auto_post).start()

bot.infinity_polling()
