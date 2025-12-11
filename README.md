# Bash & Linux commands

## Some commands

* They have been used on Linux Debian or Ubuntu, various versions.

* Some require root access, simply use `sudo` per command or `sudo -i` once and then the commands

```

# use > to write to a file and use >> to append to a file
echo "some text" >  file.txt
echo "some text" >> file.txt

# Add text to beginning and/or end of text file_old and output as file_new
(echo 'some text at beginning' ; cat file_old ; echo 'some text at end') > file_new

# compare text files line by line => use commands comm or diff depending on usage, see documentation
man comm
man diff

# How to add same text to each line of a text file
# source: https://www.baeldung.com/linux/add-string-line-end

# compare two .pdf files: first.pdf and second.pdf (as texts), first install meld (a program)
# source: https://askubuntu.com/questions/40813/diff-of-two-pdf-files
sudo apt-get install meld
meld <(pdftotext -layout first.pdf /dev/stdout) <(pdftotext -layout second.pdf /dev/stdout)

# Convert pdf file to single images in one go 
# source: https://askubuntu.com/questions/50170/how-to-convert-pdf-to-image
pdftoppm input.pdf outputname -png

# Ubuntu apt-get command
# see also: https://help.ubuntu.com/community/AptGet/Howto 

# List file placements in couchdb (which is an open source apache database)
dpkg -L couchdb

# Check package version installed
dpkg -s openssl

# List (current) upgradable packages
apt list --upgradable

# Upgrade a specific package
apt-get install openssl

# Check Linux or Linux kernel version:
lsb_release -a
cat /proc/version
uname -a
cat /etc/os-release

```
```

# Upgrade to new Linux release
# if the internet connection is lost during upgrading, log on again, and reconnect to the upgrading information texts by doing: sudo screen -D -r
do-release-upgrade

# Need to reboot? Simply check if /var/run/reboot-required exists on vm instance => which means that reboot is required
cat /var/run/reboot-required
# or
if [ -f /var/run/reboot-required ]; then echo 'Restart required'; fi

# Check which services start at boot time
systemctl list-units --type service

# Logs, latest 1000 lines in system log
# See more at: https://www.digitalocean.com/community/tutorials/how-to-use-journalctl-to-view-and-manipulate-systemd-logs
journalctl -n 1000

#Specific service logs, for example, for couchdb and nginx
journalctl -u couchdb.service
journalctl -u nginx.service

# How to kill an application by name:
sudo killall -9 scp

# 10 oldest processes
ps -elf | sort -r -k12 | head -n 10

# Kill a process by <pid>
kill <pid>

# Reboot Linux:
sudo reboot

#Power off:
poweroff, halt or shutdown -h now

# Ports:
netstat -ntlp | grep LISTEN

# DNS
nslookup -query=ns google.com
nslookup google.com
nslookup -query=mx google.com

# More at: http://www.thegeekstuff.com/2012/07/nslookup-examples/
internet listen
netstat -l

# Internet connections
netstat -plant

```
```

# Internet connection speed test via linux terminal
# source: https://askubuntu.com/questions/104755/how-to-check-internet-speed-via-terminal
curl -s https://raw.githubusercontent.com/sivel/speedtest-cli/master/speedtest.py | python -

# http(s) (application) server speed test
# source: https://httpd.apache.org/docs/2.4/programs/ab.html => Ubuntu install => apt-get install apache2-utils
# see also: https://www.petefreitag.com/item/689.cfm => 100 yahoo.com requests with max 10 concurrent:
ab -n 100 -c 10 http://www.yahoo.com/

# My terminals IP address
wget -qO - http://ipecho.net/plain; echo

# Disk size
df -H

# Sum of .csv files sizes (in current directory)
du -hc *.csv | tail -1

# Count # of lines in file
wc -l <filename>

# Count # of lines in *.csv files in current directory
wc -l *.csv

# Count # of files in directory
ls -1 | wc -l

# Count number of characters in file, and words in file
wc -m <filename>
wc -w <filename>

# Symbolic link to a file, here using nginx configuration as an example: this links to the file mydomain
$ /etc/nginx/sites-enabled ln -s ../sites-available/mydomain .

# bash script: Loop over files (*.csv) in current directory (.) and
# do something with each file (file -i) e.g. list the encoding of each file
    #!/bin/bash
    for xx in ./*.csv; do
        file -i "$xx"
    done

# bash script function: difference between return and exit
# return -> returns a value from a function
# exit -> abandons the current shell

# Compress files and folders in directory to named file backup.tgz’
tar -cvzf backup.tgz *

# Uncompress the above tar file. Flag f must be the last flag
tar -xvzf backup.tgz

# check IP addresses
nslookup riskbutler.com

```
```

# Memory check
cat /proc/meminfo 

# A simple CPU speed test - calculation of Pi with (eg.) 1000 decimals
time echo "scale=1000; 4*a(1)" | bc -l &

# Services
service --status-all 

# Linux node/system limitations
# soft limits are simply the currently enforced limits
# hard limits mark the maximum value which cannot be exceeded by setting a soft limit
ulimit -a

# Tell who current user is: print userid
whoami

# List all users 
cut -d: -f1 /etc/passwd

# add a user
adduser user_name

# make local sudo'er 
# test that it works => [a source](https://www.digitalocean.com/community/tutorials/how-to-create-a-sudo-user-on-ubuntu-quickstart)
usermod -a -G sudo local

# start an empty crontab for a user
su user_name
crontab -e

```
```

# delete a user
userdel user_name

# add a user to a group, refer to: https://linuxize.com/post/how-to-add-user-to-group-in-linux/

# --- make a user with restricted access ---
ln -s /bin/bash /bin/rbash

useradd -d /home/user_name -s /bin/rbash -m user_name
passwd user_name

mkdir /home/user_name/bin

chown root. /home/user_name/.profile
chmod 755 /home/user_name/.profile

## set PATH
nano /home/user_name/.profile

--- .bash_profile ---
PATH=$HOME/bin
---

## allow above user: "ls", "mkdir", "ping" commands
ln -s /bin/ls /home/user_name/bin/ls
ln -s /bin/mkdir /home/user_name/bin/mkdir
ln -s /bin/ping /home/user_name/bin/ping

--- end of: make a user with restricted access ... ---

# Users of processes
ps aux

# local a command e.g. nodejs
which nodejs

# list all files in folder but not folders
ls -p | grep -v / 

# concatenate the contents of all the files from above and place in file name files-contents.txt
cat $(ls -p | grep -v /) > files-contents.txt

# Combine a number of .json objects in folder `json_files/` into a single .json file named merged.json
# [source](https://stackoverflow.com/questions/29636331/merging-json-files-using-a-bash-script)
jq -s 'reduce .[] as $item ({}; . * $item)' json_files/* > merged.json

# Check if folder exists, if not then create folder
[ ! -d /path_to_folder/folder ] && mkdir -p /path_to_folder/folder

# Find folder named “radar”
find / -name "radar"

# Find file named textfile.txt
find / -name textfile.txt

#Find files not named jquery-ui*.css
find . -type f -not -name 'jquery-ui*.css'

# Delete files not named logo.png
find . -type f ! -name 'logo.png' -delete
delete a lot of files
find . -name "*.pdf" -delete

# Rename all files with ending .txt to .csv
# [optionally] To find placement of perl command rename: $ which rename => /usr/bin/rename
rename 's/txt/csv/' *.txt

# Add text in more.txt to the end of a text file orig.txt
cat more.txt >> orig.txt

# convert test.json file to test.csv
# Related: Online conversions: https://tableconvert.com/json-to-excel
cat test.json | jq -r '(map(keys) | add | unique) as $cols | map(. as $row | $cols | map($row[.])) as $rows | $cols, $rows[] | @csv' > test.csv

```
```

# Find and replace
# http://unix.stackexchange.com/questions/112023/how-can-i-replace-a-string-in-a-files 
# http://www.brunolinux.com/02-The_Terminal/Find_and%20Replace_with_Sed.html 
cat filename | sed  's/oldstring/newstring/g'  >  new_filename
cat filename | sed  's/oldstring/newstring/g'  >> current_filename
echo ‘oldstring’ | sed ...

# Even simpler: In-place using '-i' find and replace
sed -i 's/oldstring/newstring/g' filename

# Look for particular string in a group of files
# ... this finds all cases of the string “echo” in the .php files in all folders from “.”
# note: w is to match whole word (not only part of text), and l is to only return the file name
grep -rn --color 'echo' *.php
grep -rnw '/path/to/somewhere/' -e "pattern"

# get the difference between two files e.g. two .csv files, remove the "+" signs and the first line, output to diff.csv
diff -u old.csv new.csv | grep -e "^\+" | sed -e 's/^\+//' | sed -e'' 1d > diff.csv

# find difference of two files each with one item per line
grep -Fxvf file1 file2

# delete first line in text file
sed -i'' 1d file

# insert line first in text file
sed -i -e 1i"some text for first line in file" file

# Copy directory but exclude a directory within
rsync -av --progress sourcefolder /destinationfolder --exclude thefoldertoexclude    

# make a directory ("olddir") read-only in a new directory ("newdir") => thus making a read-only copy of the new dir to olddir
mount --bind --read-only olddir newdir

# remove a directory mount (see mount example above)
umount -R newdir

# Check encoding of file
file -i YourFile.txt

# Change encoding of text file f: from t: to o: output
iconv -f ISO_8859-1 -t UTF-8 -o YourNewFile.txt YourOldFile.txt 

# ... list all iconv known encodings
iconv -l

# ... combine with curl
curl URL | iconv -f ISO_8859-1 -t UTF-8 > YourCurlDownloadedFile.txt

# curl post file
curl -X POST -H "Content-Type: application/json" -d @FILENAME DESTINATION

# Check date of ssl certificate validity
# returns eg:
# notBefore=Jan  8 09:57:51 2014 GMT
# notAfter=Jan  8 09:57:51 2015 GMT
# Note: I personally now use Let's Encrypt and its auto cron job that fully automates updates
# read more at: https://letsencrypt.org/
echo | openssl s_client -connect site:port 2>/dev/null | openssl x509 -noout -dates

```
```

# Start graphical environment
startx

# Discrete graphics card on desktop
# source: https://www.linuxbabe.com/desktop-linux/switch-intel-nvidia-graphics-card-ubuntu
# How To Install: https://documentation.ubuntu.com/server/how-to/graphics/install-nvidia-drivers/

# Install desktop on Ubuntu
sudo apt-get install ubuntu-desktop

# Time
# https://help.ubuntu.com/community/UbuntuTime 
echo "UTC" | sudo tee /etc/timezone
sudo dpkg-reconfigure --frontend noninteractive tzdata

# is UTC time in seconds since 1970
date +%s

# Full date YYYY-MM-DD
date +%F

# Download file(s)
# http://www.thegeekstuff.com/2012/07/wget-curl/
# Upload file index.html to ftp server
curl --user username:password --ftp-ssl --ftp-pasv -T "index.html" ftp://ftp_server_address/

# tar and compress, and uncompress: j (bzip2) and z (gzip)
tar -cvjf ../yahoo.tar.bz2 yahoo*
tar cvzf MyImages.tgz /home/img/ 
tar -xvf thumbnails.tar.gz

# tar and compress many files
find . -name 'my_prefix*' > filelist.txt
tar czvf archive.tar.gz --files-from filelist.txt

# Working with strings and conversions
# Note the arrow and following text is not part of the command but shows expected output
YY="20160201"

echo ${YY:2:2}  => “16”

echo ${YY:0:4}"-"${YY:4:2}"-"${YY:6:2}  => “2016-02-01”

# The below also works on complete .csv and other text files i.e. simply pipe the full .csv file in "one go"

echo "YYYYMMDD" | sed -n -e "s_\(....\)\(..\)\(..\)_\2/\3/\1_p" => "MM/DD/YYYY"

echo "YYYYMMDD" | sed -n -e "s_\(....\)\(..\)\(..\)_\1-\2-\3_p" => YYYY-MM-DD

echo "YYYYMMDDHHMMSS" | sed -n -e "s_\(....\)\(..\)\(..\)\(..\)\(..\)\(..\)_\1-\2-\3 \4:\5:\6_p" => YYYY-MM-DD HH:MM:SS

echo "YYYYMMDD,HHMMSS" | sed -n -e "s_\(....\)\(..\)\(..\)\(.\)\(..\)\(..\)\(..\)_\1-\2-\3 \5-\6-\7_p" => YYYY-MM-DD HH-MM-SS

echo "20160201,235432" | sed -n -e "s_\(....\)\(..\)\(..\)\(.\)\(..\)\(..\)\(..\)_\1-\2-\3 \5:\6:\7_p" => 2016-02-01 23:54:32

# sleep (pause): s, m, h, d for seconds, minutes, hours, days
sleep 1s

```

### Links

* [Bash quoting](https://wiki.bash-hackers.org/syntax/quoting)

## linux: cron

* A common error is not to set PATH correctly because cron runs at default with a very limited PATH and env.

```
# first view your env in terminal
$ env

# now create a env check in cron
$ crontab -e
* * * * * env >> env.txt

# By comparing the two above you will see a big difference in the two environments
# A solution is e.g. to place a PATH within each shell script e.g.

    #!/bin/bash
    PATH=/bin:/usr/bin:/usr/local/bin:/snap/bin:$PATH
    
# Use whereis to find your required command path 
# $ whereis [command]    

```

## code validation

* .js validation: [Google Closure compiler](https://closure-compiler.appspot.com/home). This is available as java file for script compilation & validation.

* .html5 validation: [W3 org validator](https://validator.w3.org/) and [other W3 tools](https://w3c.github.io/developers/tools/)


## nodejs: debug nodejs with chrome browser

* source: [nodejs debugger](https://nodejs.org/api/debugger.html#v8-inspector-integration-for-nodejs)

* To install nodejs: [nodesource](https://github.com/nodesource/distributions#installation-instructions) and information on [nodejs releases](https://nodejs.org/en/about/releases/)

* Open Chrome browser and devtools

```
# 1. The file to debug is index.js. In a terminal type

cd path_to_index.js/index.js

node --inspect-brk index.js

# 2. Open Chrome browser and type in URL: about:inspect

# 3. Click on link: "Open dedicated DevTools for Node" and go to Sources tab

```

* That is it, you are now in Chrome developer mode, and your index.js file is open for debugging etc.

* Note: use `--inspect` above if it is an app listing on `localhost:port`, then add breakpoints in devtools:Sources afterwards. This is because the app has to start listining on port so you can send requests to the app in order to debug its code.

* To test one can use: `node --inspect-brk -e "console.log(1 + 2)"`

## nodejs: handle mix of callback and async functions

Wrap the "traditional" callback functions in a function that returns a Promise e.g.

```
// Example

const fs = require('fs');

// write file

function writeFilePromise(file, str) {
    return new Promise((resolve, reject) => {
        fs.writeFile(file, str, 'utf8', (err) => {
            if (! err) { 
                resolve(true); 
                console.log('file written');
            } else { 
                reject(err);
                console.error(err);
            }
        });
    });
}

async function writeFile(file, str) {
    return await writeFilePromise(file, str);
}

writeFile('/path/filename.extension', 'Some text string').then(c => console.log(c)).catch((err) => console.error(err));

// read file

function readFilePromise(file) {
    return new Promise((resolve, reject) => {
        fs.readFile(file, 'utf8', (err, data) => {
            if (! err) { 
                resolve(data); 
                console.log('file read');
            } else { 
                reject(err);
                console.error(err);
            }
        });
    });
}

```

## nodejs: large files line-by-line

```
"use strict";

// https://nodejs.org/dist/latest-v14.x/docs/api/readline.html#readline_example_read_file_stream_line_by_line
// https://nodejs.org/api/fs.html#fs_fs_createwritestream_path_options

const fs = require('fs');
const readline = require('readline');  // nodejs 10+

const rl = readline.createInterface({
  input: fs.createReadStream('path/big-file-in.csv', {encoding: "utf8"}),
  output: fs.createWriteStream('path/big-file-out.csv'),                                  
  crlfDelay: Infinity
});

rl.on('line', (line) => {
    // process line prior to writing it out
    rl.output.write(line + "\n");
    // console.log(`Line from file: ${line}`);
});
```

## sqlite3

* Note difference between real numbers and integers.

select 1/10  = 0
select 1/10. = 0.1
select 1./10 = 0.1

### export to .CSV

`echo -e ".separator ','\n.headers on\n.mode csv\n.once newfile.csv\nSELECT * from [tablename];" | sqlite3 [databasename].db`

### links

* [CLI commands](https://sqlite.org/cli.html)
* [tutorial site](https://www.sqlitetutorial.net/)

## google cloud storage

```
# Add/remove versioning to bucket
gsutil versioning set (on|off) gs://[bucket]

# List all files (including versions) in versioned bucket 
gcloud storage ls --all-versions gs://[bucket]

# Generate a file urls.txt with all files, one on each row
# Optionally use urls.txt to fetch all the files to local folder (for example): see the bash script below
gcloud storage ls --all-versions gs://bucket/doc.json | cat > urls.txt

# Count all files (including versions) in versioned bucket (subtract approx. 1-3 lines providing extra information, possibly test showing files first) 
gcloud storage ls --all-versions gs://[bucket] | wc -l

# List file detailed information in versioned bucket
gsutil ls -a -L gs://[bucket]/[file]

# Copy files from (versioned) source to (versioned) destination. 
# Note that version timestamps are not transferred to destination (new version timestamps are made). 
# Note that repeated copying of the same bucket source data will be added (in addition) to the already available data in the destination i.e. it does not “understand” that the data is already present in the destination): 
gsutil cp -r -A gs://[source bucket] gs://[destination bucket]

# Sync source to destination. 
# Note that versions (of versioning) are not transferred
gsutil rsync -r -n gs://[source bucket] gs://[destination bucket]


## bash script for combining with urls.txt above - one file at a time (not parallel)
--- start ---

#!/bin/bash

# Replace 'urls.txt' with the actual name of your file

while read -r url; do

# Extract the filename from the URL

filename=$(basename "$url")

# Copy the file to Google Cloud Storage

gsutil cp "$url" ./"$filename"

done < urls.txt

--- end ---

## bash script for combining with urls.txt above - parallel
--- start ---

#!/bin/bash

# Replace 'urls.txt' with the actual name of your file
# Replace '4' with the desired number of parallel processes
# (adjust based on your system resources)

cat urls.txt | xargs -n 1 -P 4 sh -c 'gcloud storage cp "$1" ./files2/"$(basename "$1")"' --

--- end ---

# Continuing from the above bash scripts one can use the range of files downloaded from Google Storage and possibly transforming the files contents, file by file, for example:
cat * | jq '{"meta": .meta, "shareclass": .ShareClass }'

```

### Install Google Cloud CLI 

```
# source: https://cloud.google.com/sdk/docs/downloads-interactive
curl https://sdk.cloud.google.com | bash
# retart terminal
exec -l $SHELL

# init and authenticate (to google cloud) 
su [ubuntu_user]
gcloud init

# --- Now the cli is installed, the below can be used when needed ---

# optional (additional) key:file service account authentication - this is not necessary if the above was done 
# source: https://cloud.google.com/storage/docs/authentication#gsutilauth
# Thus download a .json key to e.g. /tmp/
gcloud auth activate-service-account --key-file /tmp/KEY_FILE

# list config
gcloud config list

# change project-id
gcloud auth application-default login
gcloud auth application-default set-quota-project [project-id]
gcloud config set project [project-id]

```

## some git commands

```
# roll back Y number of commits
git reset --soft HEAD~Y
# then one can e.g. do 
git push origin master

# git show diff of changes to be committed
git diff --cached

```

## Ubuntu OS issues

### wayland display server and nvidia driver 510+

Below in `/usr/lib/udev/rules.d/61-gdm.rules` there is a bug: make a new rule that enables Wayland if the nvidia driver is > version 510, then reboot and choose Wayland as the display system.

### sound

```
# System log: worth checking once in a while to ensure there are not errors
less /var/log/syslog

# Fix 1
systemctl --user restart pipewire wireplumber

# Fix 2 status or restart audio (sound) service - e.g. if not working or not available
# done with OS 20.10
systemctl --user status pulseaudio
systemctl --user restart pulseaudio


```

## Debian OS update (with apt)

[https://www.cyberciti.biz/faq/update-upgrade-debian-9-to-debian-10-buster/](https://www.cyberciti.biz/faq/update-upgrade-debian-11-to-debian-12-bookworm/)

```
If connection during installation is lost (for some reason) you can for example reboot and then run: 'dpkg --configure -a' to correct the installation problem after lost connection

```

## Ubuntu OS upgrade (with apt)

* [releases](https://wiki.ubuntu.com/Releases)
* [source](https://www.cloudbooklet.com/how-to-upgrade-to-ubuntu-20-04-lts/)

I said yes to use package maintainers files.

There might be some packages that are not fully updated: see sources.list and update accordingly.

```

sudo -i

# Some packages might be marked as held back which cannot be installed, upgraded, or removed automatically. This may cause issues during the upgrade process.
# If there are any on hold, packages, you need to unhold the packages with the following command.

apt-mark showhold

apt-mark unhold [package_name]

apt update
apt upgrade

reboot

sudo -i
apt full-upgrade
apt --purge autoremove

apt install update-manager-core
do-release-upgrade -d


```

For packages like nginx it might be better to retain config files as-is (after viewing the differences).

For packages like couchdb one needs to re-establish the correct sources.list for the upgraded OS and make an "apt upgrade" (not a new install).

* [couchdb](https://docs.couchdb.org/en/stable/install/unix.html#installation-using-the-apache-couchdb-convenience-binary-packages)

```
sudo apt update && sudo apt install -y curl apt-transport-https gnupg
curl https://couchdb.apache.org/repo/keys.asc | gpg --dearmor | sudo tee /usr/share/keyrings/couchdb-archive-keyring.gpg >/dev/null 2>&1
source /etc/os-release
echo "deb [signed-by=/usr/share/keyrings/couchdb-archive-keyring.gpg] https://apache.jfrog.io/artifactory/couchdb-deb/ ${VERSION_CODENAME} main" \
    | sudo tee /etc/apt/sources.list.d/couchdb.list >/dev/null
    
sudo apt update

# Either (if couchdb has not been installed earlier)
sudo apt install -y couchdb
# Or (if couchdb is already installed)
sudo apt full-upgrade

```

sqlite3 should be auto re-installed, check this.

nodejs and npm needs to be re-installed: 
> apt install nodejs
> apt install npm

pm2 process manager needs to re-installed.
> npm install pm2@latest -g

other relevant npm nodejs packages also need to be re-installed.

For SSL certificates for Ubuntu 20.04: https://certbot.eff.org/lets-encrypt/ubuntufocal-nginx



## Move repo to github.com

```

# git push all changes on local drive

# create a new private repository on Github.com. It’s important to keep the repository empty, e.g. don’t check option Initialize this repository with a README when creating the repository.

# go to root of existing repo
git remote add upstream https://github.com/[USER]/[PROJECT].git
git push upstream master
git push --tags upstream

git remote rm origin
git remote add origin https://github.com/[USER]/[PROJECT].git


git config master.remote origin
git config master.merge refs/heads/master

# now check contents of path_to_repo/.git/config
less config

git push origin master

# delete old repo (not on local desktop machine but at other service provider e.g. bitbucket if the old repo is there)

```

## github.com tips

* After enabling two factor authentication one cannot use a user & password to clone private repos: see: `https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token`

## Firewall problems for URLs - possibly check here
https://sitereview.bluecoat.com/#/

## apache2 webserver on (Ubuntu) desktop

It can be quite tricky to get a symbolic linked directory of a user to be accessed through the localhost apache webserver (on a desktop).
Assume the user name is "local" and the folder name is "files" i.e.

`/home/local/files/` is the folder to access through the web browser on `localhost/files`.

`DocumentRoot /var/www/html` - thus this is the default apache2 document root (else adjust below).

One way to fix this is to:

```
sudo -i
cd /var/www/html
ln -s /home/local/files/ .

cd /home/
chmod 755 local

cd /home/local
chmod -R 755 files


# To fix this message:
# AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 127.0.1.1.
# Set the 'ServerName' directive globally to suppress this message
# Do the following (for localhost-use-only):
cd /etc/apache2/
cp apache2.conf apache2.conf.orig
nano apache2.conf

# add this to the default apache2.conf
<Directory /var/www/html/files/>
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted
</Directory>

ServerName localhost

```

## usefull software

```
sudo -i

# git
apt install git

git config --global user.email my_github_email
git config --global user.name my_github_username

# apache2
apt install apache2

apachectl configtest

# java
sudo apt install default-jre

# google closure compiler
# source: https://mvnrepository.com/artifact/com.google.javascript/closure-compiler

# curl
sudo apt  install curl

# npm
sudo apt install npm

# nodejs
# source: https://github.com/nodesource/distributions and https://github.com/nodesource/distributions/tree/master/scripts/deb
curl -fsSL https://deb.nodesource.com/setup_24.x | sudo -E bash - &&\
sudo apt-get install -y nodejs

# jsdoc
sudo npm install -g jsdoc

# gsutil
sudo apt  install gsutil

#uuid
sudo apt install uuid

# sqlite3
sudo apt-get install sqlite3


```

