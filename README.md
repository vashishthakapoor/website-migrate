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
   cd /var/www/domain.com/htdoc/
   tar -czvf wp_content_bck.tar.gz wp-content/
   ```
- move the backup files to a downloadable destination
   ```bash
   mv wp_content_bck.tar.gz /var/www/html/
   mv sqldump_backup.sql /var/www/html/
   ```

### Prepare the new server
- download backup in root folder
- install server stack for wordpress on new server
- install SSL and setup DNS of the domain
- install blank WordPress on the new server
- remove wp-content folder
- move wp-content backup to /var/www/domain/htdocs folder
- extract wp-content backup folder with TAR
- copy the database details in a file or create a temp file and view in terminal
- import the mysql database

### Finalize the Transfer and Final fixes

- Confirm the file-permissions
- remove backup and temporary files
- secure the backup in root folder for future

Happy migrations....
