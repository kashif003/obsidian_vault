# Part 2: Moving In — Running Your First Local Model in 5 Minutes

Welcome back. In Part 1, we set up the big picture: instead of sending our questions across the internet to a distant genius (remote model), we're inviting a smart, capable roommate right onto our own machine.

No cloud subscriptions, no creeping data privacy concerns, and zero API keys.

Since we are working in a Linux environment today, we'll be doing everything right from the terminal—fast, lightweight, and completely under our control.

Today, theory meets terminal. By the time you finish scrolling down this page, your computer will spin up its very first local AI model and answer a prompt — entirely offline.

Let's get our hands dirty.

## Step 1: Install the Landlord (The Ollama Runtime)

To manage and run AI models locally without needing a PhD in machine learning, we need a lightweight tool called **Ollama**. Think of Ollama as the apartment manager that helps our roommate move in, keeps it running smoothly in the background, and gives us a simple way to talk to it.

Ollama works the same way no matter what you're on — Mac, Windows, or Linux.

1. Head over to [ollama.com](https://ollama.com/?utm_source=gemini) and download the installer for your operating system.
    
2. Once installed, Ollama runs as a background service.
    

> **Note:** This guide's commands and screenshots are shown on Linux, since that's the environment we're using for this series. If you're on Mac or Windows, the install step will look slightly different (a downloaded app instead of a terminal script), but every command from here on — `ollama --version`, `ollama run llama3.2`, and the Python script — works identically once Ollama is installed.

On Linux specifically, installation is usually a quick one-line script in your terminal:

Bash

```
curl -fsSL https://ollama.com/install.sh | sh
```

To verify everything is working, open your terminal and type:

Bash

```
ollama --version
```

If it spits out a version number, you're golden. The infrastructure is ready.

## Step 2: Welcoming the Roommate (Pulling a Model)

Now we need to download our actual roommate — the model weights file we talked about. We'll use **Llama 3.2** (specifically the lightweight 1B or 3B version—where the "B" stands for **billion parameters**, meaning the size and complexity of the model's neural network brain). These smaller versions are lightning-fast and small enough to run smoothly on almost any modern Linux laptop.

In your terminal, run this single command:

Bash

```
ollama run llama3.2
```

**What’s happening right now?** Ollama is securely downloading the model file directly to your machine. Because it's a few gigabytes, give it a minute depending on your internet speed.

Once the download finishes, your terminal will transform into an interactive chat prompt. Go ahead and type anything you want:

> _You:_ `Tell me a short joke about a programmer.` _Roommate:_ `Why do programmers prefer dark mode? Because light attracts bugs.`

Boom. You just ran an AI model locally on your own hardware. No cloud server touched your prompt, no API key was checked, and it would work just as well if you unplugged your router right now.

Type `/bye` to exit the terminal chat.

## Step 3: Talking to Your Roommate via Python

Chatting in the terminal is fun, but we're builders—we want to write code that talks to our local AI. Let's write a tiny Python script to automate our roommate, complete with comments so we know exactly what every line is doing.

First, make sure you have Python installed.

> **Quick tip:** It's best practice to create a virtual environment first, then add `ollama` to a `requirements.txt` file, and install from that instead of installing packages globally.

Bash

```
python -m venv venv
source venv/bin/activate
echo "ollama" > requirements.txt
pip install -r requirements.txt
```

Next, create a file named `roommate.py` and drop this commented code inside:

Python

```
import ollama

# Call the ollama chat function to communicate with our local model
response = ollama.chat(
    model='llama3.2',  # Specify which local model to use
    messages=[
        {
            'role': 'user',  # Define the sender role as the user
            'content': (
                'Give me three brilliant names for a pet robot.'
            ),  # The actual prompt text
        }
    ],
)

# Extract and print just the content of the message returned by the model
print(response['message']['content'])
```

Run your script in your terminal:

Bash

```
python roommate.py
```

Within a second or two, your local model reads the code, generates the response locally, and prints it out in your console.

## What Just Happened?

Look at what we've accomplished:

- We installed a local runtime on Linux.
    
- We downloaded an open-source intelligence model.
    
- We queried it via both the command line and a Python script—all with **zero internet required** after the initial download, **zero billing info**, and **total privacy**.
    

Now that our roommate has officially moved in and unpacked their bags, we're ready to take the next big step.

In **Part 3**, we are going to dive deeper into the **theory** and explore how we can use our local model to read and answer questions based on **our own private documents** using a technique called RAG.