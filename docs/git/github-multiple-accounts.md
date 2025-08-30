# GitHub Multiple Accounts with SSH on macOS

This guide explains how to configure **two GitHub accounts** (e.g., personal and work) on a Mac using SSH keys. It also
provides a simple script to switch between accounts easily.

---

## 1. Generate SSH Keys

Generate a separate SSH key for each GitHub account.
Enter the filename accordingly when prompted **Enter file in which to save the key (/Users/you/.ssh/id_ed25519):**.

```bash
# Personal account
ssh-keygen -t ed25519 -C "your_personal_email@example.com"
# Save as: /Users/you/.ssh/id_ed25519_personal

# Work account
ssh-keygen -t ed25519 -C "your_work_email@example.com"
# Save as: /Users/you/.ssh/id_ed25519_work
```

When prompted, give each key a unique filename. You **can leave the passphrase empty** or set one for extra security.

---

## 2. Add SSH Keys to ssh-agent

Start the agent and add both keys:

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519_personal
ssh-add ~/.ssh/id_ed25519_work
```

---

## 3. Add Public Keys to GitHub

Copy the **public** key (the `.pub` file):

```bash
pbcopy < ~/.ssh/id_ed25519_personal.pub
```

- Go to GitHub → **Settings** → **SSH and GPG keys** → **New SSH key**
- Paste the key.
- Repeat for your work account.

⚠️ Never add the private key (`id_ed25519`) — only the `.pub` file goes to GitHub.

---

## 4. (Optional) SSH Config File

If you prefer to avoid switching manually, you can configure host aliases in `~/.ssh/config`.

```bash
# Work GitHub account (default)
Host github.com
  IgnoreUnknown UseKeychain
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519_work

# Personal GitHub account
Host github-personal
  HostName github.com
  User git
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519_personal
```

Usage:

- Clone work repos:  
  `git clone git@github.com:workuser/repo.git`
- Clone personal repos:  
  `git clone git@github-personal:personaluser/repo.git`
- Set remote URL for work repos:  
  `git remote set-url origin git@github.com:workuser/repo.git`
- Set remote URL for personal repos:  
  `git remote set-url origin git@github-personal:personaluser/repo.git`

> Note: If you don't want to use aliases, skip this step and use the script in the next section.

Or you can separate ssh config files and use `-F` option in the script below.

Create `~/.ssh/config_personal`:

```plaintext
Host *
  IgnoreUnknown UseKeychain
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519_personal
```

---

## 5. Switching Accounts with a Script

If you want to keep using `git@github.com:username/repo.git` (no aliases), you can switch accounts using a small script.

Create `~/.ssh/switch-github.sh`:

```bash
#!/bin/bash

if [ "$1" == "personal" ]; then
    git config user.name "Your Personal Name"
    git config user.email "your_personal_email@example.com"
    git config core.sshCommand "ssh -i ~/.ssh/id_ed25519_personal -F ~/.ssh/config_personal"
    echo "Switched to PERSONAL GitHub account."
elif [ "$1" == "work" ]; then
    git config user.name "Your Work Name"
    git config user.email "your_work_email@example.com"
    git config core.sshCommand "ssh -i ~/.ssh/id_ed25519_work -F ~/.ssh/config"
    echo "Switched to WORK GitHub account."
else
    echo "Usage: switch-github [personal|work]"
fi
```

Make it executable:

```bash
chmod +x ~/.ssh/switch-github.sh
```

Move it to `/usr/local/bin` for convenience:

```bash
sudo mv ~/.ssh/switch-github.sh /usr/local/bin/switch-github
```

Now you can switch accounts per repo:

```bash
switch-github personal
switch-github work
```

> Note: You need to run the switch command for one time in each repository where you want to change the account.
> Because we did not use --global value.
---

## 6. Testing the Setup

Check which account SSH is using:

```bash
ssh -T git@github.com
ssh -T git@github-personal # if using alias
```

- If successful, GitHub will greet you by your username:  
  `Hi workuser! You've successfully authenticated...`
  `Hi personaluser! You've successfully authenticated...`

Check your current Git identity:

```bash
git config user.name
git config user.email
git config core.sshCommand
```

List loaded keys:

```bash
ssh-add -l
```

Sample output:

```plaintext
$ ssh-add -l
256 SHA256:aedewfweqdwedw your_personal_email@example.com (ED25519)
256 SHA256:dsdfdffefeerer your_work_email@example.com (ED25519)
```

If you see only one key, re-add the missing one with `ssh-add`:

```bash
ssh-add -D   # remove all keys
ssh-add ~/.ssh/id_ed25519_personal
ssh-add ~/.ssh/id_ed25519_work
```

---

## 7. Common Issues

- **403 error on push** → Your remote is HTTPS. Switch it to SSH:  
  `git remote set-url origin git@github.com:username/repo.git`

- **Wrong account used** → Ensure the correct key is added with `ssh-add -l`, or run the switch script again.

---

✅ With this setup, you can use two GitHub accounts on the same Mac and easily switch between them with SSH keys.  
