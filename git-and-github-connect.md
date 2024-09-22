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