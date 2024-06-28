import tkinter as tk
from tkinter import scrolledtext, messagebox
from nltk.chat.util import Chat, reflections

# Define pairs of patterns and responses
patterns = [
    (r'hi|hello|hey there', ['Hello!', 'Hey!', 'Hi!']),
    (r'how are you?', ['I am good, thank you.', 'Feeling awesome!']),
    (r'what is your name?', ['I am a chatbot.', 'I am ChatGPT, your assistant.']),
    (r'(.*) your name(.*)', ['I am ChatGPT, your assistant.']),
    (r'(.*) help (.*)', ['I can help you with various tasks. Just ask!']),
    (r'(.*) thank you(.*)', ['You are welcome!', 'No problem.']),
    (r'(.*) (good|great|fine|well) (.*)', ['Nice to hear that!', 'Awesome!', 'Good for you.']),
    (r'(.*) (bye|goodbye) (.*)', ['Goodbye!', 'Have a nice day!']),
]

# Create a chatbot with defined patterns
apbot = Chat(patterns, reflections)

def send_message(event=None):
    user_input = entry.get()
    if user_input.strip() == '':
        messagebox.showwarning("Warning", "Please enter a message.")
    else:
        dialog.config(state=tk.NORMAL)
        dialog.insert(tk.END, "You: " + user_input + "\n")
        dialog.see(tk.END)
        dialog.config(state=tk.DISABLED)
        
        response = "ChatGPT: " + apbot.respond(user_input) + "\n"
        dialog.config(state=tk.NORMAL)
        dialog.insert(tk.END, response)
        dialog.see(tk.END)
        dialog.config(state=tk.DISABLED)
        
        entry.delete(0, tk.END)

# Create main window
root = tk.Tk()
root.title("apbot")

# Create dialog area
dialog = scrolledtext.ScrolledText(root, wrap=tk.WORD, width=40, height=10, state=tk.DISABLED)
dialog.grid(row=0, column=0, padx=10, pady=10, columnspan=2)

# Create entry field for user input
entry = tk.Entry(root, width=30)
entry.grid(row=1, column=0, padx=10, pady=10)

# Create send button
send_button = tk.Button(root, text="Send", width=10, command=send_message)
send_button.grid(row=1, column=1, padx=10, pady=10)

# Bind Enter key to send_message function
root.bind('<Return>', send_message)

# Start the GUI main loop
root.mainloop()

