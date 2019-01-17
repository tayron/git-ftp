# GIT FTP

[http://git-ftp.github.io](https://git-ftp.github.io)

Shared Host exemple using pipeline of Bitbucket:

```SSH
# you can use a Docker image from Docker Hub or your own container
# registry for your build environment

pipelines:
  default:
    - step:
        name: Deploy to production
        deployment: production   # can be test, staging or production.
        # trigger: manual  # Uncomment to make this a manual deployment.
        image: samueldebruyn/debian-git
        services:
          - docker
        script:
          - apt-get update
          - apt-get -qq install git-ftp
          # use git ftp init for the first deploy to server
          - git ftp push --user $FTP_USERNAME --passwd $FTP_PASSWORD ftp://site.com.br/public_html
```

Shared Host exemple using Shell script:

```SSH
#!/bin/bash

clear

echo "1) Put production or testing:"
read ENVIRONMENT

echo
echo "1) Put your action:"
read FTP_ACTION

FTP_USERNAME="site_user"
FTP_PASSWORD="site_password"

$ git config git-ftp.password $FTP_USERNAME
$ git config git-ftp.password $FTP_PASSWORD


$ git config git-ftp.production.url ftp://site.com.br/public_html
$ git config git-ftp.testing.url ftp://site.com.br/public_html/subdominio_homologacao

# git ftp push -s testing
# git ftp push -s production

echo
echo "Starting deploy in ${ENVIRONMENT}"
echo "---------------------------------------------------------------"
echo 

git ftp $FTP_ACTION -s $ENVIRONMENT -vv

echo
read -s -n 1 -p "Press any key to continue . . ."
echo
```

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
