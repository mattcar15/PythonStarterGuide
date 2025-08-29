# Python Projects Setup (macOS) — Read Me First

This repo teaches you (hi, sis! 👋) how to set your Mac up for **many Python projects** without breaking anything.

## Goal & Folder Layout

We’ll create two folders at your home directory (a.k.a. **tilde** `~`):

```
~
├── commands   # reusable helper scripts you can run from Terminal
└── code       # each project lives in its own folder here
```

* **Projects** go in `~/code`.
* **Helper commands** (small scripts) go in `~/commands`.

> You’ll copy this repo’s `commands/` into your `~/commands`, make the scripts executable, and then use them for new Python projects.

---

## Step 0 — Prereqs (Terminal & Homebrew)

* Open **Terminal** (Cmd+Space → “Terminal”).
* Install **Homebrew** (if you don’t have it): [https://brew.sh/](https://brew.sh/)

---

## Step 1 — Make the base folders

```bash
mkdir -p ~/commands ~/code
```

---

## Step 2 — Copy the helper commands

From this repo’s root (the folder containing this README in your cloned version of this repo):

```bash
cp -R ./commands/* ~/commands/
```

You should now have at least:

```
~/commands
├── psetup
└── python_gitignore
```

---

## Step 3 — What does `chmod` mean?

`chmod` changes **file permissions**. On Unix/macOS, each file has permissions for:

* **Owner**, **Group**, **Others**
* Each can **r**ead (4), **w**rite (2), e**x**ecute (1)

Numbers add up: `7 = 4+2+1`, `5 = 4+1`, etc.

* `755` → Owner: `rwx` (7), Group: `r-x` (5), Others: `r-x` (5). Common for scripts you want to run.
* `700` → Owner only can run/read/write. More private.

We want our helper scripts to be **executable**:

```bash
chmod 755 ~/commands/*
```

Now you can run them directly from Terminal (we’ll add them to PATH in a later step if needed, but for this tutorial we’ll call them explicitly).

> If macOS blocks running a script, you may need to allow it in System Settings → Privacy & Security.

---

## Step 4 — Install a Python **version manager** (pyenv)

We’ll use **pyenv** to pick and switch Python versions per machine and per project.

```bash
brew install pyenv
```

Add pyenv init to your shell and add our commands to PATH (choose the file your shell uses; for most people on new macOS that's **zsh**):

```bash
# Zsh (default on macOS)
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
echo 'eval "$(pyenv init -)"' >> ~/.zshrc
echo 'export PATH="$HOME/commands:$PATH"' >> ~/.zshrc

# Bash users would do the same lines in ~/.bash_profile or ~/.bashrc
```

Reload your shell config:

```bash
exec $SHELL -l
```

Install and set a current Python version (example: 3.12.x):

```bash
pyenv install 3.12.5     # this can take a few minutes the first time
pyenv global 3.12.5      # make it the default system-wide Python
python --version         # should show Python 3.12.5
```

> If you ever need a different version for a project: run `pyenv local 3.11.9` (for example) **inside that project folder**.

### What is `.zshrc`?

`.zshrc` is your shell's configuration file — it's where your system stores settings for Terminal behavior, shortcuts, and environment setup. Generally, you **don't need to touch it directly**; the commands above automatically add the necessary lines. If you're curious, you can view your current config at:

```bash
cat ~/.zshrc
```

## What is your `path`?
Your path is essentially a globally visible group of paths. The idea is that you can access these anywhere. This makes it perfect for storing the commands you run. For example, we want our script `psetup` to run without needing to write `~/commands/psetup my-project` every time. So by putting `~/commands` on the path, now we can just run `psetup my-project` because your computer looks in `~/commands` when you are running a command now in your terminal.


---

## Step 5 — Try it in this example repo

From this repo’s root:

1. Create a virtual environment (a private Python for this project):

```bash
python -m venv venv
```
*Note: If this fails saying it can't find python try python3 (only needed here once in venv it'll always be python)*

2. Activate it:

```bash
source venv/bin/activate
```

You’ll see your prompt start with `(venv)` — that means the venv is ON.

3. Run the starter file:

```bash
python main.py
```

4. Deactivate when done:

```bash
deactivate
```

### Handy alias for activation

Add this to your `~/.zshrc` (or bash profile) to make activating quicker:

```bash
echo "alias venv='source venv/bin/activate'" >> ~/.zshrc
exec $SHELL -l
```

Now you can just type `venv` inside any project folder to activate its venv after you've created the venv for that project folder.

---

## Step 6 — Pick your editor (Cursor or VS Code)

The `psetup` script ends by opening your new project in an editor. By default it uses **Cursor** via:

```bash
cursor .
```

If you prefer **VS Code**, open `~/commands/psetup` and change the last line to:

```bash
code .
```

(Pay attention to it with the ~ if you only change it locally that'll have no impact as that is NOT the script that runs, the script that runs is the copy of it that you made)

To enable the `code` command on macOS, open VS Code and run:

* **Cmd+Shift+P** → type **“Shell Command: Install 'code' command in PATH”** → hit Enter.
* Docs: [https://code.visualstudio.com/docs/setup/mac#\_launching-from-the-command-line](https://code.visualstudio.com/docs/setup/mac#_launching-from-the-command-line)

---

## Step 7 — Use `psetup` to make a fresh project

`psetup` creates a new project folder under `~/code`, sets up a venv, a `main.py`, initializes Git, adds a Python `.gitignore`, and opens it in your editor.

Run it like this:

```bash
psetup my-first-project
```

Next steps inside the new project:

```bash
cd ~/code/my-first-project
source venv/bin/activate   # or just: venv (if you added the alias)
python main.py
```

---

## What’s inside this repo

```
.
├── commands
│   ├── psetup
│   └── python_gitignore
├── main.py
└── README.md
```

### `commands/psetup` (what it does)

* Creates `~/code/<project-name>`
* Creates and activates `venv`
* Initializes Git repo and `.gitignore`
* Creates `main.py`
* Opens the project in your editor (`cursor .` or `code .`)

> You can open `~/commands/psetup` to see or customize the exact steps.

---

## Quick mental model recap

* **pyenv** controls which **Python** you have.
* **venv** gives each **project** its own isolated packages.
* **psetup** bootstraps a new project in seconds.

You're ready. 🎉 Open Terminal and try:

```bash
psetup hello-python
```

Then:

```bash
cd ~/code/hello-python
venv
python main.py
```
