TOKEN = 'YOUR_TOKEN'
bot = telebot.TeleBot(TOKEN)

# Dictionary to store group information
groups = {6}
(commands=['start', 'help'])
def send_welcome(message):
    bot.reply_to(message, "Welcome to Group Management Bot! Use /join to join a group and /leave to leave a group.")

@bot.message_handler(commands=['join'])
def join_group(message):
    group_name = message.text.split()[1]
    user_id = message.from_user.id
    if group_name not in groups:
        groups[group_name] = [user_id]
    else:
        groups[group_name].append(user_id)
    bot.reply_to(message, f"You have joined the group {group_name}.")

@bot.message_handler(commands=['leave'])
def leave_group(message):
    group_name = message.text.split()[1]
    user_id = message.from_user.id
    if group_name in groups and user_id in groups[group_name]:
        groups[group_name].remove(user_id)
        bot.reply_to(message, f"You have left the group {group_name}.")
    else:
        bot.reply_to(message, "You are not a member of this group.")

@bot.message_handler(commands=['info'])
def group_info(message):
    group_name = message.text.split()[1]
    if group_name in groups:
        member_count = len(groups[group_name])
        bot.reply_to(message, f"Group: {group_name}\nMembers: {member_count}")
    else:
        bot.reply_to(message, "Group not found.")

bot.polling(10
)
