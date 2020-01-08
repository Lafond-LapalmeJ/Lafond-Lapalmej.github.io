# Bioinformatics Platform wiki
- [Bioinformatics Platform wiki](#bioinformatics-platform-wiki)
  - [FAQ](#faq)
  - [Admin](#admin)
    - [Create User, project, quote, invoice on LabKey](#create-user-project-quote-invoice-on-labkey)
    - [Create a project directory on Beluga](#create-a-project-directory-on-beluga)
    - [Price List](#price-list)
  - [Beluga](#beluga)
    - [Important paths](#important-paths)
    - [Shared data folder procedure](#shared-data-folder-procedure)
  - [ngs_tools](#ngstools)
      - [Rules to follow](#rules-to-follow)
  - [vscode](#vscode)
    - [Extensions to have](#extensions-to-have)
  - [Compute Canada Cloud](#compute-canada-cloud)
    - [Access the Cloud space on Arbutus](#access-the-cloud-space-on-arbutus)
    - [Connect to the cloud via terminal with user: tomcat](#connect-to-the-cloud-via-terminal-with-user-tomcat)
    - [Renew SSL Certificate](#renew-ssl-certificate)
    - [Tomcat server](#tomcat-server)
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
      - [Connect by ssh to lxvmap34](#connect-by-ssh-to-lxvmap34)
    - [lxvmap35](#lxvmap35)
      - [Connect by ssh to lxvmap34](#connect-by-ssh-to-lxvmap34-1)
  - [Moodle](#moodle)

---
## FAQ

**I am interested in learning bioinformatics. Do you have any workshops, training or courses in bioinformatics?**  
Here is the link to Calcul Quebec and MICM website. They have some workshops on Biofinformatics during the year.  
http://www.calculquebec.ca/en/academic-research-services/training/
https://mcgill.ca/micm/events
http://micm.mcgill.ca/classes/

I also added you to our workshop mailing list. We will have some basic bioinformatics courses in 2020.

---
## Admin

### Create User, project, quote, invoice on LabKey

First open the LabKey Platform project. [Link](https://206-12-89-216.cloud.computecanada.ca/labkey/Platform/project-begin.view?)

1. If the user is not in the plaform_users list, Create a new user by clicking `insert new item` in the list menu.
2. If the project is not in the Project list, Create a new project. [link](https://206-12-89-216.cloud.computecanada.ca/labkey/Platform/wiki-page.view?name=new_project)
3. To create a quote, fill the information at this [link](https://206-12-89-216.cloud.computecanada.ca/labkey/Platform/wiki-page.view?name=new_quote). Press `refresh pdf` before saving to make sure the display of the pdf is OK.
4. Each invoice is link to a quote. To create an invoice you have to make sure the 3 steps above are completed, then fill the information at this [link](https://206-12-89-216.cloud.computecanada.ca/labkey/Platform/wiki-page.view?name=new_invoices).

### Create a project directory on Beluga

See the script `new_project` at this location `/project/6033480/projects`

### Price List

| Analysis | Price | Price Floor | Included |
|---|---:|---:|---|
| Whole Genome Sequencing | $50 per sample | $1000 | Reports and QCs for variants and CNV |
| Whole Exome Sequencing | $30 per sample | $500  | Reports and QC for variants and CNV |
| RNA-Seq Expression analysis Case/Control<sup>1</sup> | $800 | $800  | QCs, tables and figures for expression and pathway analysis |
| Bioinformatics support | $55 per hour | $55 |  |

<sup>1</sup> For RNA-Seq analysis other than Case/Control, please contact us for a quote.

Contact us for any other services to plan a meeting and get the right price for the services you need.

---
## Beluga

### Important paths

- ngs_tools production: `$BIN/ngs_tools`
- ngs_tools development: `$BIN/ngs_tools_dev`
- reference controls
  - cftr: `$RESOURCES/controls/cftr`
  - AmpliSeq: `$RESOURCES/controls/ampliseq`
  - exome (GIAB): `$RESOURCES/controls/giab`
- Shared directory: `/project/6033480/shared_data`

### Shared data folder procedure

The `shared_data` folder is to share data with compute canada user that are not from our group (def-jbriv). The procedure use the `setfacl` command to add supplemental permissions other that owner and group permission.

1- In `shared_data`, create a directory based on the project name for the group/user you need to share data with like `ABC-001`.

2- Copy the data to share into the directory.

3- Change recursively the permissions for the group/user that need access.
```
# for a group (like def-jbriv)
setfacl -R -m g:def-jbriv:rx ABC-001

# for a user
setfacl -R -m u:lapalmej:rx ABC-001
```
---

This command will give read access to group def-jbriv and user lapalmej. You can always verify the permissions of a folder or a file with the command `getfacl ABC-001` or `getfacl ABC-001/file`

---
## ngs_tools

#### Rules to follow

- Commit often
- Pull before starting to work
- Test your code before pushing
- Keep the README files up-to-date
- Master branch is for production
- *Develop* branch is a **stable** development branch
- For non-minor changes, create a new *feature* branch, test it and then merge your *feature* branch to *develop*. This is the best approach to keep the *develop* branch stable

---
## vscode

### Extensions to have

- Python
- Remote - SSH
- Git Graph
- GitLens
- HTML Preview
- Markdown All in one

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

### Tomcat server

Tomcat is the tool that run the LabKey and PhenoTips application. Here everything you need to know.

- Tomcat directory: `/usr/local/tomcat`
- Tomcat server config: `/usr/local/tomcat/conf/server.xml`
- Phenotips config: `/usr/local/tomcat/webapps/phenotips/WEB-INF`
- Labkey config: `/usr/local/tomcat/conf/Catalina/localhost/labkey.xml`

### LabKey

#### LabKey paths

- Path to tomcat folder: `/usr/local/tomcat`
- Path to LabKey folder: `/usr/local/labkey`
- Labkey config: `/usr/local/tomcat/conf/Catalina/localhost/labkey.xml`
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
--- 

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
---


#### LabKey recovery

Performing a backup recovery takes 2 steps

1- Application recovery
unpack the `tar.gz` file in /usr/local/labkey  
`tar -xzvf labkey.tar.gz
2- Replace the database

```
sudo su - postgres
dropdb labkey
createdb labkey
psql -U postgres -d labkey -f labkey.sql
```
---

### Phenotips

#### PhenoTips Paths

PhenoTips is working on the same tomcat instance than LabKey but have a different database (mysql)

- Path to tomcat: `/usr/local/tomcat`
- Path to PhenoTips config in tomcat: `/usr/local/tomcat/`
- Path to phenotips folder: `/var/lib/phenotips`
- Phenotips config: `/usr/local/tomcat/webapps/phenotips/WEB-INF`

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
- Path to ngs_tools (develop branch): `/data/bin/ngs_tools_dev`
- Path to backup for lxvmap34 and lxvmap35: `/labgenet/backup_labkey`

### lxvmap34

lxvmap34 is the virtual machine(VM) on the MUHC network. The VM is used for the LabKey of the Clinical Lab of the MUHC.

- Database
  - Postgresql
  - db name: labkey
  - User: postgres
  - password: *see password file*

#### Connect by ssh to lxvmap34

The password for the user **integrator** is in the password file.
```
# You need to be on the MUHC network to access the VM
ssh integrator@lxvmap34
```
---

The LabKey is install with the same structure as the Compute Canada cloud. you can refer to the above sections for the documentation.

### lxvmap35

#### Connect by ssh to lxvmap34

The password for the user **integrator** is in the password file.
```
# You need to be on the MUHC network to access the VM
ssh integrator@lxvmap34
```
---

## Moodle
