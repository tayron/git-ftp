# GIT FTP

[http://git-ftp.github.io](https://git-ftp.github.io)


Instructions for syncing a local folder with a remote FTP or SFTP server
The only requirement is git-ftp:

```SSH
apt-get update && apt-get install git-ftp
```

Initialize a git repo in the directory you want to sync. 
Track all files, and commit them to your repo:

```SSH
git init
git add -A && git commit -m "Committed all files
```

Set config options for your FTP server. These config options are added to the git repo: 
```SSH
git config git-ftp.user username
git config git-ftp.url sftp://domain.net
git config git-ftp.password passwordhere
git config git-ftp.syncroot /Users/userlocal/desktop/folder/
git config git-ftp.insecure 0
```

Upload files the first time:
```SSH
git-ftp init --remote-root home/public/ -vv --syncroot /Users/userlocal/desktop/folder
```

After the files have been pushed once, updating requires this: 
```SSH
git add -A && git commit -m "Committed all files"
git-ftp push --remote-root /var/www/html/site/ -vv --syncroot /Users/userlocal/desktop/folder
```

 
Other options:
```SSH
git config git-ftp.deployedsha1file mySHA1File
git config git-ftp.insecure 1
git config git-ftp.key ~/.ssh/id_rsa
git config git-ftp.keychain user@example.com
