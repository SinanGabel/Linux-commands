# Linux tips

## cron

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
    PATH=/bin:/usr/bin:/usr/local/bin/

```
