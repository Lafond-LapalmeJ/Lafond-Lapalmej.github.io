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
      - [Backup](#backup)
      - [Automatic Backup](#automatic-backup)
      - [Backup recovery](#backup-recovery)
    - [Phenotips](#phenotips)
      - [Backup](#backup-1)
      - [Automatic Backup](#automatic-backup-1)
      - [Backup recovery](#backup-recovery-1)
  - [MUHC server](#muhc-server)
    - [lxap56](#lxap56)
    - [lxvmap34](#lxvmap34)
    - [lxvmap35](#lxvmap35)
    - [LabKey](#labkey-1)
    - [Phenotips](#phenotips-1)
  - [Moodle](#moodle)
## FAQ

**I am interested in learning bioinformatics. Do you have any workshops, training or courses in bioinformatics?**  
We will have a second series

## Beluga

## ngs_tools

## vscode

## Compute Canada Cloud

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
`sudo keytool -import -trustcacerts -alias tomcat -file 0001_chain.pem -keystore .keystore`
- Start the tomcat server
`/usr/local/tomcat/bin/startup.sh`

### LabKey

#### Backup

#### Automatic Backup

#### Backup recovery

### Phenotips

#### Backup

#### Automatic Backup

#### Backup recovery

## MUHC server

### lxap56

### lxvmap34

### lxvmap35

### LabKey

### Phenotips

## Moodle
