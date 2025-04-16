# How to Migrate HUGE WordPress Sites to a VPS Server over SSH with wget - Tutorial Video & Documentation

Migrating a large WordPress website can be challenging, especially when dealing with slow downloads or timeouts. 

In this video, I’ve explained how to efficiently migrate huge WordPress sites using SSH and WGET, a powerful combination that speeds up the process and ensures reliability.

## ❤️ Youtube Video - The Tutorial

[![Website Migration Tutorial Video - Detailed](https://img.youtube.com/vi/D5vCuWfnWDI/0.jpg)](https://www.youtube.com/watch?v=D5vCuWfnWDI)
[Jump to the section with commands](https://github.com/vashishthakapoor/website-migrate?tab=readme-ov-file#how-to-migrate-wordpress-sites-easity-even-huge-sites)

## 💼 Affiliate Links (Thank you for your support!):

- [Contabo VPS](https://vashishthakapoor.com/website-migrate-contabo) Free Setup: https://vashishthakapoor.com/website-migrate-contabo
- [UpCloud VPS](https://vashishthakapoor.com/recommend/upcloud) (High performance & uptime): https://vashishthakapoor.com/recommend/upcloud

## **Why You’ll Love This Server‑to‑Server Migration Guide**

### 🚫 No More Local Downloads  
Forget about downloading huge backup files to your laptop or workstation. With this method, **all file transfers happen directly** between your old VPS and your new one—no local detours required.

### 🔄 Direct Server‑to‑Server Transfers  
By using SSH and wget, you’ll stream your WordPress backup straight from source to destination. This means you:
- **Bypass your home or office network** entirely  
- **Eliminate double‑handling** of large files  

### ⏱️ Save Hours of Your Time  
Why waste time waiting for a download to finish, then re‑uploading it again? This approach slashes your migration time in half (or more!), so you can:
- Get your new server up and running faster  
- Focus on testing your site instead of babysitting file transfers  

### ⚡ Zero Risk of Network Hiccups  
Local connections can be flaky—timeouts, dropped packets, and retries can derail your backup. By moving data server‑to‑server, you:
- Rely on data‑center grade network links  
- Avoid frustrating “incomplete transfer” errors  

### 🔍 How It Works (At a Glance)
1. **SSH into your new VPS**  
2. **Run a single wget command** that pulls your backup archive from the old server  
3. **Extract and restore** your files and database  

That’s it—no middleman, no local machine, and no wasted time. Follow the full tutorial below and migrate your large WordPress site **reliably**, **quickly**, and **without network headaches**!

## How to Migrate WordPress Sites Easity (Even HUGE Sites)

### Prepare the old server
- clean up wordpress if needed
- backup database
   ```bash
   ## Create a database backup
   mysqldump -u DB_USER -p DB_NAME > sqldump_backup.sql
   ```
- backup files and extras (wp-content) folder
   ```bash
   ## Navigate to the website's directory
   cd /var/www/domain.com/htdocs/
   ```
   ```bash
   ## Create a gzip compressed backup file of the whole wp-content folder
   tar -czvf wp_content_bck.tar.gz wp-content/
   ```
- move the backup files to a downloadable destination
   ```bash
   ## Move wp-content backup to downloadable directory /var/www/html/
   mv wp_content_bck.tar.gz /var/www/html/
   ```
   ```bash
   ## Move database backup to downloadable directory /var/www/html/
   mv sqldump_backup.sql /var/www/html/
   ```

### Prepare the new server
- download backup in root folder
  ```bash
  ## Confirm directory
  pwd
  ```
  ```bash
  ## Navigate to /root
  cd /root
  ```
  ```bash
  ## Download both backup files (Replace server_ip with your actual old-server IP address)
  wget server_ip/wp_content_bck.tar.gz && wget server_ip/sqldump_backup.sql
  ```
- install server stack for wordpress on new server
  ```bash
  ## Install WordPress (My Favorite Panel)
  wget -qO wo wops.cc && sudo bash wo
  ```
  ```bash
  ## Enable bash completion
  bash -l
  ```
  ```bash
  ## install the required stack for running Wordpress
  wo stack install
  ```
- install blank WordPress with SSL and setup DNS of the domain
  ```bash
  ## Install WordPress with SSL, PHP 8.1 with the same domain on the new server
  wo site create domain.com --wp --php81 -le --force
  ```
- remove wp-content folder
  ```bash
  ## Navigate to domain's file directory
  cd /var/www/domain.com/htdocs/
  ```
  ```bash
  ## Remove the existing wp-content folder
  rm -r wp-content/
  ```
- move wp-content backup to /var/www/domain/htdocs folder
  ```bash
  ## Navigate to the /root where the backup files were downloaded from old-server
  cd /root
  ```
  ```bash
  ## Move the wp-content backup file from root to website's file directory
  mv wp_content_bck.tar.gz /var/www/domain.com/htdocs/
  ```
- extract wp-content backup folder with TAR
  ```bash
  ## Navigate to website's file directory
  cd /var/www/domain.com/htdocs/
  ```
  ```bash
  ## Extract the compressed backup file to create wp-content folder
  tar -xzvf wp_content_bck.tar.gz
  ```
- copy the database details in a file or create a temp file and view in terminal
  ```bash
  ## Edit the wp-config.php file (Make sure to copy the database credentials)
  nano ../wp-config.php
  ```
   ctrl + x | y | enter
  ```bash
  ## Navigate to Root folder
  cd /root
  ```
  ```bash
  ## Create a temporary file and paste the database credentials
  nano temp
  ```
   ctrl + v | ctrl + x | y | enter
  ```bash
  ## Display the content of temp file in the terminal screen
  cat temp
  ```
- import the mysql database
  ```bash
  ## Restore Database (Use the username, password, and database name displayed in the terminal screen)
  mysql -u DB_USER -p DB_NAME < sqldump_backup.sql
  ```
### Finalize the Transfer and Final fixes

- Confirm the file-permissions
- remove the temporary files
  ```bash
  ## Navigate to root
  cd /root
  ```
  ```bash
  ## Remove the temp file
  rm -r temp
  ```
- secure the backup in root folder for future
  ```bash
  ## Navigate again to root
  cd /root
  ```
  ```bash
  ## Create backupfiles folder
  mkdir backupfiles
  ```
  ```bash
  ## Movie wp-content compressed backup to backupfiles/ folder
  mv wp_content_bck.tar.gz backupfiles/
  ```
  ```bash
  ## Movie sqldump_backup.sql to backupfiles/ folder
  mv sqldump_backup.sql backupfiles/
  ```

###Happy migrations....
