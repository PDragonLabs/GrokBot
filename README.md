# GrokBot
discord bot for answering simple questions under 280 character replises only


To create a Discord Grok bot using a free API, follow these steps:

---

### 1. Set Up a Discord Bot
You'll need to create a bot account on Discord and invite it to your server.

- **Go to the Discord Developer Portal**: Visit [https://discord.com/developers/applications](https://discord.com/developers/applications) and log in with your Discord account.
- **Create a New Application**: Click "New Application," give it a name (e.g., "GrokBot"), and click "Create."
- **Add a Bot**: In the application settings, go to the "Bot" tab, then click "Add Bot" and confirm.
- **Copy the Bot Token**: Under the bot settings, click "Copy" next to the token. Keep this safe—you'll need it later to authenticate your bot.
- **Invite the Bot to Your Server**:
  - Go to the "OAuth2" tab, then select "URL Generator."
  - Under "Scopes," check "bot."
  - Under "Bot Permissions," select permissions like "Send Messages" and "Read Messages/View Channels."
  - Copy the generated URL, paste it into your browser, and follow the prompts to invite the bot to a server you manage.

---

### 2. Choose a Free API for Conversational AI
A "Grok" bot implies conversational capabilities, so you need a free API that can process user input and generate responses. One solid option is:

- **[Hugging Face's Transformers](https://huggingface.co/models?pipeline_tag=conversational)**: Hugging Face offers free access to pre-trained conversational models via their API. You can use a model like "BlenderBot" or "DialoGPT" to generate human-like responses.

---

### 3. Code the Bot
You'll write a script to connect your bot to Discord and the API. Here's how to do it in **Python** (a beginner-friendly choice):

#### Prerequisites
- Install Python (download from [python.org](https://www.python.org/)).
- Install required libraries:
  ```bash
  pip install discord.py requests
  ```

#### Sample Code
Create a file (e.g., `grokbot.py`) and add this code:

```python
import discord
import requests
import os

# Set up Discord client
intents = discord.Intents.default()
intents.message_content = True
client = discord.Client(intents=intents)

# Hugging Face API details
HF_API_URL = "https://api-inference.huggingface.co/models/facebook/blenderbot-400M-distill"
HF_API_TOKEN = "YOUR_HUGGING_FACE_API_TOKEN"  # Get this from huggingface.co (free tier available)

# Event: Bot is ready
@client.event
async def on_ready():
    print(f'Logged in as {client.user}')

# Event: Handle messages
@client.event
async def on_message(message):
    # Ignore messages from the bot itself
    if message.author == client.user:
        return

    # Process user message
    user_input = message.content
    response = get_grok_response(user_input)
    await message.channel.send(response)

# Function to get response from Hugging Face API
def get_grok_response(user_input):
    headers = {"Authorization": f"Bearer {HF_API_TOKEN}"}
    payload = {"inputs": user_input}
    try:
        response = requests.post(HF_API_URL, headers=headers, json=payload)
        response.raise_for_status()
        result = response.json()
        return result['generated_text'] if 'generated_text' in result else "Sorry, I couldn't generate a response."
    except Exception as e:
        return f"Error: {str(e)}"

# Run the bot with your token
client.run('YOUR_DISCORD_BOT_TOKEN')
```

#### How to Use the Code
1. **Get a Hugging Face API Token**:
   - Sign up at [huggingface.co](https://huggingface.co/).
   - Go to your profile settings, create an API token, and copy it.
   - Replace `YOUR_HUGGING_FACE_API_TOKEN` in the code with your token.
2. **Add Your Discord Token**:
   - Replace `YOUR_DISCORD_BOT_TOKEN` with the token you copied earlier.
3. **Run the Bot**:
   - Save the file and run it with:
     ```bash
     python grokbot.py
     ```
   - The bot should log in and respond to messages in your server.

#### Notes
- The model (`facebook/blenderbot-400M-distill`) is lightweight and free-tier-friendly. You can explore other models on Hugging Face’s model hub.
- Add error handling (e.g., for API rate limits) to make the bot more robust.

---

### 4. Deploy the Bot
To keep your bot running 24/7, use a free hosting service:

- **[Replit](https://replit.com/)**:
  - Create a new Python repl, upload your code, and add your tokens as environment variables (via Replit’s "Secrets" tab).
  - Run the repl to keep the bot online.
- **[Heroku](https://www.heroku.com/)**:
  - Free tier available (with limits). Deploy your bot using a `Procfile` and Git.
- **Personal Computer**: Run it locally, but it’ll stop when your machine is off.

---

### 5. Security Reminder
- **Keep Your Tokens Private**: Never share your Discord bot token or API token publicly (e.g., on GitHub). Use environment variables or a `.env` file if possible.

---

### Final Thoughts
With this setup, your Discord Grok bot will listen to messages, send them to Hugging Face’s free API, and reply with conversational responses. Check the [Hugging Face API docs](https://huggingface.co/docs/api-inference) for rate limits or model options, and tweak the code as needed. Happy coding!
