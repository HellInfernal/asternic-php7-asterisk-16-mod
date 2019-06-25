# asternic-php7-asterisk-16-mod

This is modified version of asternic-stats-1.5 from asternic.net
You can run it with php7.* and asterisk 16.3 (or lower). 
It does not support older versions of php.

Installation guide:
------------

1) Download the whole archive and unzip it.

2) Change to the extracton directory of asternic-php7-asterisk-16-mod

#> cd asternic*

3) Edit ./queue-stats/config.php to set your db user and pass,
   the AMI credentials (as set in /etc/asterisk/manager.conf).
   You can also select the language.

4) Move /queue-stats to a suitable place on your web root 

#> mv queue-stats /var/www/html/queue-stats

5) Create a new database named 'qstatslite' and related tables:

#> mysql -u root -p < ./sql/qstats.sql

6) Edit ./parselog/config.php to set the location and 
   name of the queue_log file and your db user and pass
   
   Beware: parselog folder must be on the same machine that asterisk, or,
   for example, you can also mount queue_log file 
   from asterisk machine to your machine.

7) Move the parselog directory to its final location:

#> mv parselog /usr/local/parselog


PARSING LOGS
------------


Inside /usr/local/parselog directory you will find a script to do
the job named parselog.php. To test it, change to that directory and
run it by hand:

#> cd /usr/local/parselog
#> ./parselog.php convertlocal

If you dont have binftm_misc installed you might need
to run 'php -q ./parselog.php'

#> cd /usr/local/parselog
#> php -q ./parselog.php convertlocal

To parse the log at periodic intervals via a cronjob, you can write
a script in /etc/cron.hourly named qstatslog with these commands :

#> echo "#!/bin/bash" > /etc/cron.hourly
#> echo "cd /usr/local/parselog" >> /etc/cron.hourly
#> echo "./parselog.php convertlocal" >> /etc/cron.hourly

Be sure to set execute permissions to it:

#> chmod a+x /etc/cron.hourly/qstatslog
