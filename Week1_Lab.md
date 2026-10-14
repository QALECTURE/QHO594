# QHO594 - Week 1 Lab

## Introduction to Network Automation, Linux, Python, Git and Netmiko

**Duration:** 2 Hours  
**Mode:** Practical Lab  
**Goal:** By the end of this lab, you should be able to prepare a basic network automation environment, use essential terminal commands, inspect your network, create a simple Python device inventory, and understand how Netmiko connects to network devices.

---

# Learning Outcomes

By completing this lab, you should be able to:

- use basic Linux/macOS terminal commands;
- understand file and folder permissions;
- check your operating system, Python, Git and SSH installation;
- create and activate a Python virtual environment;
- install Python packages using `pip`;
- use network diagnostic commands such as `ping`, `nslookup` and `traceroute`;
- create a basic Python network-device inventory;
- validate IP addresses using Python;
- understand how Netmiko connects to Cisco devices using SSH;
- create a simple pre-flight checker for a network automation environment;
- store device data in JSON instead of hard-coding everything inside Python.

---

# Lab Schedule

| Time | Activity |
|---|---|
| 0-10 min | OS, kernel and terminal basics |
| 10-30 min | Files, folders and permissions |
| 30-50 min | Python, virtual environment and Git |
| 50-70 min | Network diagnostic commands |
| 70-90 min | Python device inventory |
| 90-105 min | Netmiko and SSH concepts |
| 105-115 min | Pre-flight automation checker |
| 115-120 min | Quiz and discussion |

---

# Part 1 - Create Your Lab Workspace

Open your terminal.

## macOS / Linux

```bash
mkdir -p ~/QHO594/week1
cd ~/QHO594/week1
pwd
```

You should now be inside a folder similar to:

```text
/Users/yourname/QHO594/week1
```

or on Linux:

```text
/home/yourname/QHO594/week1
```

Create the folders and files we will use during the lab:

```bash
touch notes.txt
touch devices.txt
mkdir scripts
mkdir evidence
ls -la
```

Your workspace should now look similar to:

```text
week1/
├── devices.txt
├── evidence/
├── notes.txt
└── scripts/
```

## Quick Check

Answer these questions:

1. What command shows your current directory?
2. What command shows files in a folder?
3. What does `mkdir` do?
4. What does `touch` do?

---

# Part 2 - Essential Terminal Commands

Run the following commands one at a time.

## Show your current directory

```bash
pwd
```

## List files

```bash
ls
```

## List files with more information

```bash
ls -la
```

## Move into another directory

```bash
cd scripts
pwd
cd ..
```

## Create a file

```bash
touch test.txt
```

## Add content to the file

```bash
echo "QHO594 Network Automation Lab" > test.txt
```

## Read the file

```bash
cat test.txt
```

## Add another line

```bash
echo "Week 1 Seminar" >> test.txt
```

Check the result:

```bash
cat test.txt
```

## Copy a file

```bash
cp test.txt evidence/test-copy.txt
```

## Rename a file

```bash
mv test.txt lab-notes.txt
```

## View the files you created

```bash
find . -maxdepth 2 -type f
```

---

# Part 3 - File Permissions

Check the permissions of your file:

```bash
ls -l lab-notes.txt
```

You may see something similar to:

```text
-rw-r--r--
```

This can be read as:

```text
-   rw-   r--   r--
    owner group others
```

Where:

```text
r = read
w = write
x = execute
```

## Change permissions

Run:

```bash
chmod 600 lab-notes.txt
ls -l lab-notes.txt
```

You should see something similar to:

```text
-rw-------
```

This means that only the owner has read and write access.

Now run:

```bash
chmod 644 lab-notes.txt
ls -l lab-notes.txt
```

You should now see:

```text
-rw-r--r--
```

## Create an executable script

Create the file:

```bash
touch scripts/demo.sh
```

Add the following content:

```bash
cat > scripts/demo.sh <<'SCRIPT'
#!/bin/bash

echo "Hello from QHO594"
echo "Current machine: $(hostname)"
echo "Current user: $(whoami)"
SCRIPT
```

Make the script executable:

```bash
chmod +x scripts/demo.sh
```

Run it:

```bash
./scripts/demo.sh
```

## Discussion

Why might an automation script require execute permission?

---

# Part 4 - Operating System and Kernel

Run:

```bash
uname
```

On macOS you will normally see:

```text
Darwin
```

Now run:

```bash
uname -a
```

Check the processor architecture:

```bash
uname -m
```

You may see:

```text
arm64
```

or:

```text
x86_64
```

On macOS you can also run:

```bash
sw_vers
```

## Concept

Think of the system like this:

```text
Applications
     ↓
Operating System
     ↓
Kernel
     ↓
CPU / RAM / Storage / Network
```

The **kernel** is the core part of the operating system that communicates with hardware and manages system resources.

---

# Part 5 - Check Python

Run:

```bash
python3 --version
```

Then:

```bash
which python3
```

Test Python:

```bash
python3 -c "print('QHO594 Python is working')"
```

If you receive output similar to this, Python is working:

```text
QHO594 Python is working
```

---

# Part 6 - Create a Python Virtual Environment

Make sure you are inside the Week 1 folder:

```bash
cd ~/QHO594/week1
```

Create a virtual environment:

```bash
python3 -m venv .venv
```

Activate it:

```bash
source .venv/bin/activate
```

Your terminal should now start with something similar to:

```text
(.venv)
```

Check which Python is being used:

```bash
which python
```

It should point to a Python installation inside `.venv`.

Upgrade pip:

```bash
python -m pip install --upgrade pip
```

## Why do we use a virtual environment?

A virtual environment keeps the Python packages for one project separate from other projects on the same computer.

---

# Part 7 - Install Netmiko

Make sure your virtual environment is active.

Run:

```bash
pip install netmiko
```

Check that Netmiko is installed:

```bash
pip show netmiko
```

You can also check the version using:

```bash
python -c "import netmiko; print(netmiko.__version__)"
```

Save the installed Python packages:

```bash
pip freeze > requirements.txt
```

Check the file:

```bash
cat requirements.txt
```

---

# Part 8 - Check Git

Run:

```bash
git --version
```

Initialise a Git repository:

```bash
git init
```

Check the repository:

```bash
git status
```

Create a `.gitignore` file:

```bash
cat > .gitignore <<'GITIGNORE'
.venv/
__pycache__/
.env
.DS_Store
GITIGNORE
```

Add your files:

```bash
git add .
```

Check:

```bash
git status
```

Create a commit:

```bash
git commit -m "Complete QHO594 Week 1 environment setup"
```

If Git asks for your name and email, configure them first:

```bash
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
```

Then run the commit again.

---

# Part 9 - Network Diagnostic Commands

## Find your hostname

```bash
hostname
```

## Find your IP address

### macOS

```bash
ipconfig getifaddr en0
```

You can also inspect the interface using:

```bash
ifconfig en0
```

### Linux

```bash
ip addr
```

## Find your default gateway

### macOS

```bash
route -n get default
```

or:

```bash
netstat -rn | grep default
```

### Linux

```bash
ip route
```

Look for the default route.

The basic idea is:

```text
Your Laptop
     ↓
Local Network
     ↓
Default Gateway / Router
     ↓
Internet
```

---

# Part 10 - Test Connectivity

## Ping an internet IP address

```bash
ping -c 4 8.8.8.8
```

## Ping a domain name

```bash
ping -c 4 google.com
```

If `8.8.8.8` works but `google.com` does not work, what could be the issue?

Think about **DNS**.

---

# Part 11 - DNS Testing

Run:

```bash
nslookup google.com
```

If available, also try:

```bash
dig google.com
```

DNS converts names such as:

```text
google.com
```

into IP addresses.

---

# Part 12 - Trace a Network Path

### macOS / Linux

```bash
traceroute google.com
```

You may see several hops.

Example concept:

```text
Your Laptop
    ↓
Router
    ↓
ISP
    ↓
Intermediate Routers
    ↓
Destination
```

Each line represents another network hop between your computer and the destination.

---

# Part 13 - Test a TCP Port

On macOS and many Linux systems, you can use `nc`.

Run:

```bash
nc -vz google.com 443
```

A successful result means that TCP port `443` is reachable.

## Important Difference

```text
ping
  ↓
Can I reach the host?

nc -vz host 443
  ↓
Can I reach a particular TCP service?
```

---

# Part 14 - Create a Python Device Inventory

Create the file:

```bash
nano scripts/inventory.py
```

If you use VS Code, you can instead open the folder using:

```bash
code .
```

Add the following Python code:

```python
devices = [
    {
        "name": "R1",
        "ip": "192.168.1.1",
        "device_type": "router"
    },
    {
        "name": "SW1",
        "ip": "192.168.1.2",
        "device_type": "switch"
    },
    {
        "name": "FW1",
        "ip": "192.168.1.3",
        "device_type": "firewall"
    }
]

print("Network Device Inventory")
print("-" * 40)

for device in devices:
    print(f"Name: {device['name']}")
    print(f"IP: {device['ip']}")
    print(f"Type: {device['device_type']}")
    print("-" * 40)
```

Run the program:

```bash
python scripts/inventory.py
```

## Questions

1. What is a Python list?
2. What is a Python dictionary?
3. Why do we use a loop?
4. How could this be useful in network automation?

---

# Part 15 - Validate IP Addresses with Python

Create:

```bash
nano scripts/check_ip.py
```

Add:

```python
import ipaddress

addresses = [
    "192.168.1.1",
    "10.10.10.1",
    "172.16.1.20",
    "999.1.1.1"
]

for address in addresses:
    try:
        ipaddress.ip_address(address)
        print(f"{address}: VALID")
    except ValueError:
        print(f"{address}: INVALID")
```

Run:

```bash
python scripts/check_ip.py
```

Expected output:

```text
192.168.1.1: VALID
10.10.10.1: VALID
172.16.1.20: VALID
999.1.1.1: INVALID
```

## Challenge

Why is `999.1.1.1` invalid?

---

# Part 16 - Understand Netmiko

Netmiko allows Python to communicate with network devices using SSH.

The workflow is:

```text
Your Laptop
    ↓
Python
    ↓
Netmiko
    ↓
SSH
    ↓
Router / Switch
    ↓
CLI Output
```

For this part of the lab, use only an **approved classroom device or authorised Cisco sandbox**.

Do not connect to devices you do not own or do not have permission to access.

---

# Part 17 - Store Credentials Safely

Do not write passwords directly inside your Python code.

Instead, set environment variables.

```bash
export DEVICE_USERNAME="your_username"
export DEVICE_PASSWORD="your_password"
```

Check the username:

```bash
echo $DEVICE_USERNAME
```

Do not display your password on a shared or projected screen.

---

# Part 18 - Create a Netmiko Script

Create:

```bash
nano scripts/cisco_check.py
```

Add:

```python
import os
from netmiko import ConnectHandler

device = {
    "device_type": "cisco_ios",
    "host": "DEVICE_IP_OR_HOSTNAME",
    "username": os.environ["DEVICE_USERNAME"],
    "password": os.environ["DEVICE_PASSWORD"],
}

print("Connecting...")

connection = ConnectHandler(**device)

print("Connected successfully.")

output = connection.send_command("show ip interface brief")

print(output)

connection.disconnect()

print("Connection closed.")
```

Do not run this until your lecturer provides the approved device or sandbox details.

Replace:

```text
DEVICE_IP_OR_HOSTNAME
```

with the value provided during the lab.

---

# Part 19 - Test SSH Before Testing Netmiko

Before troubleshooting Python, first test whether SSH is reachable.

Use the approved username and host:

```bash
ssh username@hostname
```

You can also test whether TCP port 22 is reachable:

```bash
nc -vz hostname 22
```

Use this troubleshooting order:

```text
Can DNS resolve the host?
        ↓
Can I reach the host?
        ↓
Can I reach TCP port 22?
        ↓
Can SSH authenticate?
        ↓
Can Netmiko connect?
        ↓
Can the automation run?
```

---

# Part 20 - Read-Only Commands Only

During Week 1, use safe, read-only commands such as:

```text
show version
show ip interface brief
```

Do not enter configuration mode unless your lecturer specifically instructs you to do so.

The objective is to learn how to retrieve information safely before learning how to change device configuration.

---

# Part 21 - Save Device Output to a File

After connecting with Netmiko, you can save the returned output.

Example:

```python
output = connection.send_command("show ip interface brief")

with open("evidence/interface-status.txt", "w") as file:
    file.write(output)
```

You can then read it with:

```bash
cat evidence/interface-status.txt
```

---

# Part 22 - Create a JSON Device Inventory

Create:

```bash
nano devices.json
```

Add:

```json
[
  {
    "hostname": "R1",
    "ip": "192.168.1.1",
    "platform": "cisco_ios"
  },
  {
    "hostname": "SW1",
    "ip": "192.168.1.2",
    "platform": "cisco_ios"
  }
]
```

Now create:

```bash
nano scripts/json_inventory.py
```

Add:

```python
import json

with open("devices.json") as file:
    devices = json.load(file)

for device in devices:
    print(
        device["hostname"],
        device["ip"],
        device["platform"]
    )
```

Run:

```bash
python scripts/json_inventory.py
```

## Why is JSON useful?

Instead of storing everything directly inside your Python program, you can store device data separately.

```text
Device Inventory
      ↓
JSON / YAML / Database / API
      ↓
Python Automation
      ↓
Network Devices
```

This makes automation easier to maintain and reuse.

---

# Part 23 - Build a Pre-Flight Checker

Create:

```bash
nano scripts/preflight.py
```

Add:

```python
import shutil
import socket
import sys

print("=" * 50)
print("QHO594 NETWORK AUTOMATION PRE-FLIGHT CHECK")
print("=" * 50)

print("\n1. Python")
print(sys.version)

print("\n2. Git")
git = shutil.which("git")

if git:
    print(f"PASS: Git found at {git}")
else:
    print("FAIL: Git not installed")

print("\n3. Internet / DNS")

try:
    ip = socket.gethostbyname("developer.cisco.com")
    print(f"PASS: developer.cisco.com -> {ip}")
except socket.gaierror:
    print("FAIL: DNS resolution failed")

print("\n4. Netmiko")

try:
    import netmiko
    print(f"PASS: Netmiko {netmiko.__version__}")
except ImportError:
    print("FAIL: Netmiko not installed")
```

Run:

```bash
python scripts/preflight.py
```

A successful result should show that Python, Git, DNS and Netmiko are available.

---

# Part 24 - Final Project Structure

Your folder should now contain files similar to:

```text
week1/
├── .gitignore
├── devices.json
├── devices.txt
├── evidence/
│   └── test-copy.txt
├── lab-notes.txt
├── requirements.txt
└── scripts/
    ├── check_ip.py
    ├── cisco_check.py
    ├── demo.sh
    ├── inventory.py
    ├── json_inventory.py
    └── preflight.py
```

Check your Git status:

```bash
git status
```

Add your changes:

```bash
git add .
```

Commit your work:

```bash
git commit -m "Complete QHO594 Week 1 lab"
```

View the commit history:

```bash
git log --oneline
```

---

# Evidence Checklist

Before finishing the lab, make sure you can show the following:

- [ ] Week 1 workspace created
- [ ] Basic terminal commands completed
- [ ] File permissions changed using `chmod`
- [ ] Python installed and tested
- [ ] Virtual environment created and activated
- [ ] Netmiko installed
- [ ] Git repository initialised
- [ ] IP address identified
- [ ] Default gateway identified
- [ ] Ping completed
- [ ] DNS lookup completed
- [ ] Traceroute completed
- [ ] TCP port test completed
- [ ] Python device inventory created
- [ ] IP-validation script completed
- [ ] JSON inventory created
- [ ] Pre-flight checker completed
- [ ] Git commit created

---

# End-of-Lab Quiz

Try to answer the questions before looking at the answers.

## Question 1

Which command shows your current working directory?

A. `ls`  
B. `pwd`  
C. `cd`  
D. `mkdir`

## Question 2

What does `chmod` control?

A. Network speed  
B. Python packages  
C. File permissions  
D. IP addresses

## Question 3

What does the `x` permission mean?

A. Exit  
B. Execute  
C. Export  
D. Examine

## Question 4

Why do we use a Python virtual environment?

A. To increase internet speed  
B. To isolate project dependencies  
C. To replace Git  
D. To configure a router

## Question 5

What does `pip` do?

A. Installs Python packages  
B. Configures IP addresses  
C. Creates Git commits  
D. Changes permissions

## Question 6

What is Netmiko mainly used for?

A. Designing websites  
B. Connecting Python to network-device CLIs  
C. Editing images  
D. Creating databases

## Question 7

What is the purpose of DNS?

A. Encrypt passwords  
B. Convert domain names into IP addresses  
C. Create Git repositories  
D. Install Python

## Question 8

What is the difference between `ping` and `nc -vz host 443`?

A. They are exactly the same  
B. Ping checks reachability while `nc` can test a specific TCP port  
C. Ping installs software  
D. `nc` only checks DNS

## Question 9

Why should passwords not be hard-coded into Python scripts?

A. It makes Python slower  
B. It creates a security risk  
C. Python cannot read passwords  
D. Git requires passwords in JSON

## Question 10

What does JSON allow us to do in this lab?

A. Store structured device inventory data separately from the Python code  
B. Replace the operating system  
C. Create a physical router  
D. Install Netmiko

## Question 11

If `ping 8.8.8.8` works but `ping google.com` does not, which service should you investigate first?

A. DNS  
B. Git  
C. Python  
D. SSH

## Question 12

Which is the best troubleshooting order before blaming Netmiko?

A. Netmiko -> Python -> DNS -> SSH  
B. DNS/reachability -> TCP 22 -> SSH -> Netmiko  
C. Git -> JSON -> ping -> chmod  
D. Python -> Git -> chmod -> DNS

---

# Quiz Answers and Discussion

## Answer 1 - B: `pwd`

`pwd` means **print working directory**. It tells you where you currently are in the file system.

## Answer 2 - C: File permissions

`chmod` changes file or directory permissions.

## Answer 3 - B: Execute

The `x` permission allows a file, such as a shell script, to be executed.

## Answer 4 - B: Isolate project dependencies

A virtual environment allows each Python project to have its own packages and versions.

## Answer 5 - A: Installs Python packages

`pip` is the standard Python package installer.

## Answer 6 - B: Connecting Python to network-device CLIs

Netmiko simplifies SSH communication with devices such as routers and switches.

## Answer 7 - B: Convert domain names into IP addresses

DNS allows humans to use names such as `google.com` instead of remembering IP addresses.

## Answer 8 - B: Ping checks reachability while Netcat can test a TCP port

A device might be reachable but a specific service may still be unavailable.

## Answer 9 - B: It creates a security risk

Hard-coded credentials could accidentally be exposed, especially if the code is committed to Git.

## Answer 10 - A: Store structured device inventory data separately

This makes the automation easier to maintain and allows the same Python code to work with different device inventories.

## Answer 11 - A: DNS

If an IP address works but a domain name fails, name resolution is a likely problem.

## Answer 12 - B: DNS/reachability -> TCP 22 -> SSH -> Netmiko

Troubleshooting from the lower layers first helps you identify whether the issue is networking, SSH or the Python automation itself.

---

# Final Discussion

At the end of this lab, you have created the beginning of a real network automation workflow:

```text
Prepare Laptop
      ↓
Create Project Workspace
      ↓
Check Python / Git / SSH
      ↓
Create Virtual Environment
      ↓
Install Netmiko
      ↓
Check Network Connectivity
      ↓
Create Device Inventory
      ↓
Validate Data
      ↓
Connect to Approved Devices
      ↓
Collect Network Information
      ↓
Store Evidence
      ↓
Track Changes with Git
```

This is the foundation we will build on in later QHO594 labs.

---

# Optional Independent Study

Create a larger `devices.json` file containing at least five devices.

Each device should include:

```text
hostname
ip
platform
role
location
```

Write a Python program that:

1. loads the JSON file;
2. prints every device;
3. checks whether each IP address is valid;
4. counts how many routers, switches and firewalls exist;
5. reports any invalid inventory entries.

Do not connect to any real device unless you have explicit permission to do so.
