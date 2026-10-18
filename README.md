# QalbgaEslatma_bot


import random

from telegram import Update, ReplyKeyboardMarkup
from telegram.ext import (
    Application,
    CommandHandler,
    MessageHandler,
    ContextTypes,
    filters,
)

ESLATMALAR = {
    "🫀 Siqilgan paytda": [
        (
            "Albatta, Allohning zikri bilan qalblar orom olur.",
            "Qur'on, Ra'd surasi, 28-oyat"
        ),
        (
            "Albatta, qiyinchilik bilan birga yengillik bordir.",
            "Qur'on, Sharh surasi, 5–6-oyatlar"
        ),
    ],
    "📖 Oyat": [
        (
            "Albatta, Alloh sabr qiluvchilar bilan birgadir.",
            "Qur'on, Baqara surasi, 153-oyat"
        ),
        (
            "Robbingizning qaysi ne'matlarini inkor etursiz?",
            "Qur'on, Rahmon surasi"
        ),
    ],
    "🕌 Hadis": [
        (
            "Amallar niyatlarga bog'liqdir.",
            "Sahih al-Buxoriy, 1-hadis"
        ),
        (
            "Yaxshi so'z sadaqadir.",
            "Sahih al-Buxoriy va Sahih Muslim"
        ),
    ],
}

TUGMALAR = [
    ["🫀 Siqilgan paytda"],
    ["📖 Oyat", "🕌 Hadis"],
    ["🔄 Tasodifiy eslatma"],
]


async def start(
    update: Update,
    context: ContextTypes.DEFAULT_TYPE
):
    keyboard = ReplyKeyboardMarkup(
        TUGMALAR,
        resize_keyboard=True
    )

    await update.message.reply_text(
        "🌙 Qalbga Eslatma botiga xush kelibsiz!\n\n"
        "Qalbingizga taskin va foydali eslatma tanlang.",
        reply_markup=keyboard
    )


async def eslatma(
    update: Update,
    context: ContextTypes.DEFAULT_TYPE
):
    tanlov = update.message.text

    if tanlov == "🔄 Tasodifiy eslatma":
        tanlov = random.choice(list(ESLATMALAR.keys()))

    if tanlov in ESLATMALAR:
        matn, manba = random.choice(ESLATMALAR[tanlov])

        await update.message.reply_text(
            f"🌙 {matn}\n\n📚 Manba: {manba}"
        )


async def main():
    import os

    token = os.environ["BOT_TOKEN"]

    app = Application.builder().token(token).build()

    app.add_handler(CommandHandler("start", start))
    app.add_handler(
        MessageHandler(filters.TEXT & ~filters.COMMAND, eslatma)
    )

    await app.initialize()
    await app.start()
    await app.updater.start_polling()

    import asyncio
    await asyncio.Event().wait()


if __name__ == "__main__":
    import asyncio
    asyncio.run(main())
