# pyEnv - Python Version Manager

pyenv is a command-line tool that helps you manage multiple Python versions easily.  
It allows you to switch between different Python versions without modifying system settings.

---

## Install pyenv

### macOS

```bash
brew update
brew install pyenv
```

### Linux (Ubuntu/Debian)

```bash
sudo apt update; sudo apt install -y make build-essential libssl-dev zlib1g-dev \
libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev \
xz-utils tk-dev libffi-dev liblzma-dev python3-openssl git

curl https://pyenv.run | bash
```

---

## Configure your shell

Add pyenv to your shell environment:

### Bash

```bash
echo 'export PATH="$HOME/.pyenv/bin:$PATH"' >> ~/.bash_profile
echo 'eval "$(pyenv init --path)"' >> ~/.bash_profile
echo 'eval "$(pyenv init -)"' >> ~/.bash_profile
source ~/.bash_profile  # Reload the shell
```

### Zsh

```bash
echo 'export PATH="$HOME/.pyenv/bin:$PATH"' >> ~/.zshrc
echo 'eval "$(pyenv init --path)"' >> ~/.zshrc
echo 'eval "$(pyenv init -)"' >> ~/.zshrc
source ~/.zshrc  # Reload the shell
```

---

## Install Python Versions

List available versions:

```bash
pyenv install --list
```

Install a specific version:

```bash
pyenv install 3.13.7
```

---

## Set Python Versions

### Global Version

```bash
pyenv global 3.13.7
```

### Local Version (per project)

```bash
pyenv local 3.13.7
```

### Shell Version (current terminal session)

```bash
pyenv shell 3.13.7
```

---

## Verify Installation

Check the current pyenv version:

```bash
pyenv version
```

Check the Python version:

```bash
python --version
```

Example output:

```
Python 3.13.7
```

---

## Using pyenv with Virtual Environments

Install `pyenv-virtualenv` plugin for isolated environments:

```bash
brew install pyenv-virtualenv
```

Enable the plugin:

```bash
eval "$(pyenv virtualenv-init -)"
```

Create a virtual environment:

```bash
pyenv virtualenv 3.13.7 myenv
```

Activate the virtual environment:

```bash
pyenv activate myenv
```

Deactivate:

```bash
pyenv deactivate
```

---

## Troubleshooting

**Problem:** Python not switching correctly

**Solution:**

```bash
pyenv rehash
```

Make sure `pyenv` paths are correctly set in your shell initialization file.

---

## References

- Official pyenv repository: [https://github.com/pyenv/pyenv](https://github.com/pyenv/pyenv)
- pyenv-virtualenv: [https://github.com/pyenv/pyenv-virtualenv](https://github.com/pyenv/pyenv-virtualenv)
- Python documentation: [https://docs.python.org/3/](https://docs.python.org/3/)

