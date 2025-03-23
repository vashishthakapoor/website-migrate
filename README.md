# website-migrate
Migrating a large WordPress website can be challenging, especially when dealing with slow downloads or timeouts. 
In this video, I’ve explained how to efficiently migrate huge WordPress sites using SSH and WGET, a powerful combination that speeds up the process and ensures reliability.

## How to Migrate WordPress Sites Easity (Even HUGE Sites)

### Prepare the old server
- clean up wordpress if needed
- backup database
   ```bash
   ## Backup Database
   mysqldump -u DB_USER -p DB_NAME > sqldump_backup.sql
   ```
- backup files and extras (wp-content) folder
   ```bash
   cd /var/www/domain.com/htdocs/
   tar -czvf wp_content_bck.tar.gz wp-content/
   ```
- move the backup files to a downloadable destination
   ```bash
   mv wp_content_bck.tar.gz /var/www/html/
   mv sqldump_backup.sql /var/www/html/
   ```

### Prepare the new server
- download backup in root folder
  ```bash
  ## Confirm directory
  pwd
  ## Navigate to /root
  cd /root
  ## Download both backup files (Replace server_ip with your actual old-server IP address)
  wget server_ip/wp_content_bck.tar.gz && wget server_ip/sqldump_backup.sql
  ```
- install server stack for wordpress on new server
- install SSL and setup DNS of the domain
- install blank WordPress on the new server
- remove wp-content folder
  ```bash
  cd /var/www/domain.com/htdocs/
  rm -r wp-content/
  ```
- move wp-content backup to /var/www/domain/htdocs folder
  ```bash
  cd /root
  mv wp_content_bck.tar.gz /var/www/domain.com/htdocs/
  ```
- extract wp-content backup folder with TAR
  ```bash
  tar -xzvf wp_content_bck.tar.gz
  ```
- copy the database details in a file or create a temp file and view in terminal
  ```bash
  cd /var/www/domain/
  nano wp-config.php
  ctrl + x | y | enter
  cd /root
  nano temp
  ctrl + v | ctrl + x | y | enter
  cat temp
  ```
- import the mysql database
  ```bash
  ## Restore Database
  mysql -u DB_USER -p DB_NAME < sqldump_backup.sql
  ```
### Finalize the Transfer and Final fixes

- Confirm the file-permissions
- remove the temporary files
  ```bash
  cd /root
  rm -r temp
  ```
- secure the backup in root folder for future
  ```bash
  cd /root
  mkdir backupfiles
  mv wp_content_bck.tar.gz backupfiles/
  mv sqldump_backup.sql backupfiles/
  ```

####Happy migrations....
