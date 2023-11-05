# Multiple SSH Keys for Different GitHub Accounts

Managing multiple GitHub accounts on the same computer can be tricky, especially when you want to use different SSH keys for each account. This step-by-step guide will walk you through the process.

### Step 1: Generate a New SSH Key

Open your terminal and execute the following command to generate a new SSH key. Replace your-email@example.com with your email address:
```bash
ssh-keygen -t ed25519 -C "your-email@example.com"
```

When prompted, choose a unique filename for your key to avoid overwriting your existing key.

Enter a file in which to save the key `(/Users/your-username/.ssh/id_ed25519): /Users/your-username/.ssh/id_ed25519_github_second`

### Step 2: Add the New SSH Key to SSH-Agent

Ensure that ssh-agent is running by running the following command:

```bash
eval "$(ssh-agent -s)"
```

Now, add the new SSH key to the SSH agent:

```bash
ssh-add ~/.ssh/id_ed25519_github_second
```

### Step 3: Add the New SSH Key to GitHub

Copy the new SSH key to your clipboard using the appropriate command based on your operating system:

For macOS:

```bash
cat ~/.ssh/id_ed25519_github_second.pub | pbcopy
```

For Linux:

```bash
xclip -sel clip < ~/.ssh/id_ed25519_github_second.pub
```

Next, navigate to your GitHub account settings, and under "SSH and GPG keys," add a new SSH key. Paste the contents from your clipboard into the key field.

### Step 4: Create an SSH Config File

Edit the SSH config file located in your ~/.ssh directory or create it if it doesn't exist:

```bash
nano ~/.ssh/config
```

Add an entry for each GitHub account, making sure the Host is unique for each entry:

```bash
# Default GitHub account
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519

# Second GitHub account
Host github.com-second
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_github_second
```

### Step 5: Update or Clone Repositories

When cloning a new repository, specify which account to use by modifying the URL to use the custom host:

For the default account:

```bash
git clone git@github.com:username/repository.git
```

For the second account:

```bash
git clone git@github.com-second:second-username/second-repo.git
```

For existing repositories, update the remote URL:

```bash
git remote set-url origin git@github.com-second:second-username/second-repo.git
```

**Congratulations, you are now set up to use multiple GitHub accounts on the same machine with different SSH keys!**
