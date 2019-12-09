# Bioinformatics Platform wiki
- [Bioinformatics Platform wiki](#bioinformatics-platform-wiki)
  - [FAQ](#faq)
  - [Beluga](#beluga)
  - [ngs_tools](#ngstools)
  - [vscode](#vscode)
  - [Compute Canada Cloud](#compute-canada-cloud)
    - [Access the Cloud space on Arbutus](#access-the-cloud-space-on-arbutus)
    - [Connect to the cloud via terminal with user: tomcat](#connect-to-the-cloud-via-terminal-with-user-tomcat)
    - [Renew SSL Certificate](#renew-ssl-certificate)
    - [LabKey](#labkey)
      - [LabKey paths](#labkey-paths)
      - [LabKey Startup and Shutdown](#labkey-startup-and-shutdown)
      - [LabKey backup](#labkey-backup)
      - [LabKey recovery](#labkey-recovery)
    - [Phenotips](#phenotips)
      - [PhenoTips Paths](#phenotips-paths)
      - [PhenoTips Backup](#phenotips-backup)
      - [PhenoTips Automatic Backup](#phenotips-automatic-backup)
      - [PhenoTips recovery](#phenotips-recovery)
  - [MUHC server](#muhc-server)
    - [lxap56](#lxap56)
      - [lxap56 paths](#lxap56-paths)
    - [lxvmap34](#lxvmap34)
    - [lxvmap35](#lxvmap35)
    - [LabKey](#labkey-1)
    - [Phenotips](#phenotips-1)
  - [Moodle](#moodle)

## FAQ

**I am interested in learning bioinformatics. Do you have any workshops, training or courses in bioinformatics?**  
Here is the link to Calcul Quebec and MICM website. They have some workshops on RNA-Seq during the year.  
http://www.calculquebec.ca/en/academic-research-services/training/
https://mcgill.ca/micm/events
http://micm.mcgill.ca/classes/

I also added you to our workshop mailing list. We will have some basic bioinformatics courses in 2020.



## Beluga

## ngs_tools

## vscode

## Compute Canada Cloud

Documentation on how to use the cloud with Compute Canada:  
`https://docs.computecanada.ca/wiki/Cloud`

### Access the Cloud space on Arbutus

- In a web browser, go to `https://arbutus.cloud.computecanada.ca/`
- Login with your compute canada credendials
- On this page you can:
  -  create, modify the cloud specs
  -  Manage ssh key of the cloud
  -  Manage open/closed port of the cloud

### Connect to the cloud via terminal with user: tomcat

- Open a terminal
- Login with ssh (Public and private key are in password folder)
`ssh -i <path_to_private_key> centos@206.12.89.216`
- Change user to tomcat (password for the user `tomcat` is in password folder)
`sudo su - tomcat`

### Renew SSL Certificate

- Connect to the cloud with user tomcat (see above)
- Go to tomcat directory
`cd /usr/local/tomcat`
- Stop the tomcat server
`/usr/local/tomcat/bin/shutdown.sh`
- Renew the certificate (password for user `tomcat` is in password folder)
`sudo /usr/local/bin/certbot-auto certonly --csr request.csr --standalone`
- Push new certificate to the tomcat keystore (password for tomcat keystore is in password folder)
`sudo ../java/bin/keytool -import -trustcacerts -alias tomcat -file <most_recent_chain.pem> -keystore .keystore`
- Start the tomcat server
`/usr/local/tomcat/bin/startup.sh`

### LabKey

#### LabKey paths

- Path to tomcat folder: `/usr/local/tomcat`
- Path to LabKey folder: `/usr/local/labkey`
- Database
  - Postgresql
  - db name: labkey
  - User: postgres
  - password: *see password file*

#### LabKey Startup and Shutdown

If LabKey is not working. First thing to do is connect to the cloud and restart the tomcat server
```
/usr/local/tomcat/bin/shutdown.sh
# wait 10 seconds
/usr/local/tomcat/bin/startup.sh
```

#### LabKey backup

- Path to backup script (on cloud): `/home/centos/backup_labkey`
- Path to backup repository (on cedar): `/project/6004488/resources/backup_labkey`

The backup process

- Script to backup Labkey and Phenotips and then push the backup to Cedar: `/home/centos/backup_labkey/backup_labkey.sh`

- Script to remove old backup on cedar (keep only the folder from the past 7 days): `/project/6004488/resources/.remove_old_backup`

- Automatic backup  
  The backup process is triger everyday at midnight by a cron job that lunch the backup_labkey.sh script. to view the cron file: `sudo crontab -e`
- Manual backup  
  Here is the procedure to do a manual backup
  ```
  # Get the date
  date=$(date '+%Y%m%d%H%M')

  # Create a new directory for the backup
  BACKUPDIR=/home/centos/backup_labkey/$date
  mkdir -p $BACKUPDIR

  # Go where the labkey and tomcat folder are located
  cd /usr/local

  # Create a tar archive of the labkey directory (excluding some large folders)
  tar --exclude labkey/files/data_sharing -zcvf $BACKUPDIR/labkey_${date}.tar.gz labkey

  # Create the database backup
  sudo -u postgres pg_dump -U postgres -c labkey > $BACKUPDIR/labkey_${date}.sql
  
  ## Next section is to push you backup directory to Cedar
  # put your CC username here
  username=lapalmej

  # Create the directory on Cedar
  ssh -i /home/centos/.ssh/labkeykey $username@cedar.computecanada.ca "mkdir -p /project/6004488/resources/backup_labkey/${date}"
  
  # Copy the directory on Cedar
  scp -i /home/centos/.ssh/labkeykey -r $BACKUPDIR $username@cedar.computecanada.ca:/project/6004488/resources/backup_labkey
  ```

#### LabKey recovery

Performing a backup recovery takes 2 steps

1- Application recovery
unpack the `tar.gz` file in /usr/local/labkey  
`tar -xzvf labkey.tar.gz

### Phenotips

#### PhenoTips Paths

PhenoTips is working on the same tomcat instance than LabKey but have a different database (mysql)

- Path to tomcat: `/usr/local/tomcat`
- Path to phenotips folder: ``

#### PhenoTips Backup

See https://phenotips.org/Drafts/AdminGuide_Backups for backup documentation

#### PhenoTips Automatic Backup

The backup script is `/home/centos/backup_labkey/backup-phenotips-database.sh`

This script is automatically run in the `backup_labkey.sh` script.

#### PhenoTips recovery

1. Shutdown tomcat
2. Reset the database  
   `mysql -p -u phenotips phenotips < ~/data180227_phenotips.sql`
3. Startup tomcat

## MUHC server

### lxap56

lxap56 is the server doing all NGS analysis on the MUHC network. Documentation on how to set up your user to run ngs_tools is on the README of ngs_tools: https://github.com/jbriviere/ngs_tools#how-to-use

The server has 128G of RAM, 40 cores and 2 disks:

- `/labgenet` 10TB slow performance disk
- `/data` 240G high performance disk

#### lxap56 paths

- Path to MiSeq raw data: `/labgenet/data/miseq`
- Path to MiSeq analysis: `/data/analysis/miseq`
- Path to resources: `/labgenet/resources`
- Path to bin: `/data/bin`
- Path to ngs_tools (master branch): `/data/bin/ngs_tools`
- Path to ngs_tools (develop branch): `/data/bin/dev_ngs_tools`
- Path to backup for lxvmap34 and lxvmap35: `/labgenet/backup_labkey`

### lxvmap34

### lxvmap35

### LabKey

### Phenotips

## Moodle
