# Set up SSH keys
- For everything refer to official [GitHub SSH Docs](https://docs.github.com/en/search?query=ssh).
- Following steps are for Windows.
### Generate SSH keys
1. Open Git Bash
2. Paste there
```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```
- ed25519 is the algorithm used to generate key.
- On this path: /c/Users/YOU/.ssh/id_ALGORITHM , replace id_ALGORITHM with any name you want or just press enter
- This generates two things: 
    1. Private ssh key 
    2. Public ssh key (with .pub extension)
- Don't bother with passphrase setup.

### Add private ssh key to ssh agent
1. Start powershell with admin privlages and paste this to start agent manually in the background.
```powershell
Get-Service -Name ssh-agent | Set-Service -StartupType Manual
Start-Service ssh-agent
```
2. Start another terminal with NO admin privlages and paste this to add key to the agent.
```powershell
ssh-add c:/Users/YOU/.ssh/id_ed25519
```

### Add public ssh key to GitHub account
1. Open GitHub account and go to 'Settings -> SSH and GPG keys'
2. Click 'New SSH key'
3. Copy the content of .pub key file in C:/Users/YOU/.ssh
```bash
clip < ~/.ssh/id_ed25519.pub
```
OR
```powershell
cat ~/.ssh/id_ed25519.pub | clip
```
4. Give a 'Title' to your key.
5. Paste the content of .pub file here on browser.
6. Set as 'Authentication' only.
7. Click done/add.
8. Confirm change with your password (if required).

# Set up rest of it
- Git config changes from terminal. Usually need to just add the local ones only to the working local repo (from clone, creating and initiallizing, etc.)
```bash
git config --local user.name "username"
git config --local user.email "you@email.com"

git config --global user.name "username"
git config --global user.email "you@email.com"
```
- Run this and check output
```bash
git remote -v
```
- If none then set up remote repo (adding new), can get the ssh url from github repository.
```bash
git remote add origin git@github.com:User/UserRepo.git
```
- Push to remote repo
```bash
git push origin main
```

# Set up multiple ssh keys in ssh agent for different accounts from same machine
- Two GitHub Accounts 
    1. username1 & email1
    2. username2 & email2
- Generate and add private and public keys for them both.
- But now when you want to push to say email2 the agent only selects the first added ssh key (say of email1), and hence it fails to connect.
- For this create a 'config' file in C:/Users/YOU/.ssh/ directory.
```powershell
echo. > config
```
- Open this file in Notepad and paste this
```
# Configuration for first GitHub account (Vikas-Indora)
Host github.com-Vikas-Indora
    HostName github.com
    User git
    IdentityFile C:/Users/indor/.ssh/id_ed25519

# Configuration for second GitHub account (VikasIndora)
Host github.com-VikasIndora
    HostName github.com
    User git
    IdentityFile C:/Users/indor/.ssh/id2_ed25519
```
- So essentially now each ssh key now is associated with different host name alias instead of just github.com as host name.
- Now need to always change remote address or use custom host name from start when cloning a repo.
```bash
#from
git remote set-url origin git@github.com:VikasIndora/exampleRepo.git

#to
git remote set-url origin git@github.com-VikasIndora:VikasIndora/exampleRepo.git
```
- Also change once per repo, the config user.name and user.email to match with account info you want to push to.