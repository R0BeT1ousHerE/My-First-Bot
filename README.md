import telebot

# Insert your bot token obtained from BotFather in Telegram
TOKEN = 'YOUR_BOT_TOKEN'
bot = telebot.TeleBot(TOKEN)

# Function to handle the /start command
@bot.message_handler(commands=['start'])
def send_welcome(message):
    welcome_text = (
        "👋 Hello! I'm a calculator bot.\n"
        "I can perform basic arithmetic operations.\n\n"
        "📋 Commands:\n"
        "/help - Show help information\n"
        "/start - Start interacting with the bot\n"
        "Just type any mathematical expression, e.g., 3 + 4 * 2.\n"
    )
    bot.reply_to(message, welcome_text)

# Function to handle the /help command
@bot.message_handler(commands=['help'])
def send_help(message):
    help_text = (
        "🛠️ **Help Information** 🛠️\n"
        "I can help you with basic calculations.\n\n"
        "🧮 Supported Operations:\n"
        "- Addition: `+`\n"
        "- Subtraction: `-`\n"
        "- Multiplication: `*`\n"
        "- Division: `/`\n"
        "- Power: `**` (e.g., 2 ** 3)\n\n"
        "📌 Note:\n"
        "Avoid using complex expressions or unsupported characters.\n"
        "For example, input expressions like: `5 + 5 * 2`."
    )
    bot.reply_to(message, help_text)

# Function to handle calculations
@bot.message_handler(func=lambda message: True)
def calculate_expression(message):
    expression = message.text
    try:
        # Basic security check to prevent unwanted function executions
        if any(char.isalpha() for char in expression):
            bot.reply_to(message, "⚠️ Invalid input! Only numbers and operators are allowed.")
            return
        
        # Evaluate the expression safely
        result = eval(expression)
        bot.reply_to(message, f"🧮 The result is: {result}")
    except ZeroDivisionError:
        bot.reply_to(message, "❌ Error: Division by zero is not allowed.")
    except (SyntaxError, NameError, TypeError):
        bot.reply_to(message, "❌ Error: Invalid expression. Please check your input.")
    except Exception as e:
        bot.reply_to(message, f"⚠️ An error occurred: {str(e)}")

# Start the bot
bot.polling()
