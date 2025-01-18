# WordPress Setup on Apache with Port 8087

This guide covers the process of setting up a WordPress site on Apache, configured to run on port 8087. It includes troubleshooting steps for common errors encountered during the setup process.

---

## **1. Configure Apache to Listen on Port 8087**

1. Open the Apache ports configuration file:

   ```bash
   sudo nano /etc/apache2/ports.conf
   ```
2. Add the following line to make Apache listen on port `8087`:

   ```apache
   Listen 8087
   ```
3. Save and close the file.

---

## **2. Set Up the Virtual Host**

1. Create a virtual host configuration file:

   ```bash
   sudo nano /etc/apache2/sites-available/freshline-8087.conf
   ```
2. Add the following configuration:

   ```apache
   <VirtualHost *:8087>
       ServerName freshline.example.com
       DocumentRoot /srv/www/freshline

       <Directory /srv/www/freshline>
           Options FollowSymLinks
           AllowOverride All
           DirectoryIndex index.php
           Require all granted
       </Directory>

       <Directory /srv/www/freshline/wp-content>
           Options FollowSymLinks
           Require all granted
       </Directory>
   </VirtualHost>
   ```
3. Enable the site:

   ```bash
   sudo a2ensite freshline-8087.conf
   ```
4. Reload Apache:

   ```bash
   sudo systemctl reload apache2
   ```

---

## **3. Check File and Directory Permissions**

Ensure Apache has the correct permissions:

```bash
sudo chown -R www-data:www-data /srv/www/freshline
sudo chmod -R 755 /srv/www/freshline
```

---

## **4. Install PHP Extensions**

Install the required PHP extensions:

```bash
sudo apt install php libapache2-mod-php php-mysql php-xml
```

Restart Apache:

```bash
sudo systemctl restart apache2
```

Verify the `php-xml` extension is enabled:

```bash
php -m | grep xml
```

---

## **5. Configure WordPress Permalinks**

1. Log in to WordPress Admin.
2. Go to **Settings > Permalinks**.
3. Choose a structure (e.g., "Post name") and click **Save Changes**.

### Ensure `.htaccess` Exists

1. Create or edit the `.htaccess` file in `/srv/www/freshline`:

   ```bash
   sudo nano /srv/www/freshline/.htaccess
   ```
2. Add the following rules:

   ```apache
   # BEGIN WordPress
   <IfModule mod_rewrite.c>
   RewriteEngine On
   RewriteBase /
   RewriteRule ^index\.php$ - [L]
   RewriteCond %{REQUEST_FILENAME} !-f
   RewriteCond %{REQUEST_FILENAME} !-d
   RewriteRule . /index.php [L]
   </IfModule>
   # END WordPress
   ```
3. Set permissions:

   ```bash
   sudo chown www-data:www-data /srv/www/freshline/.htaccess
   sudo chmod 644 /srv/www/freshline/.htaccess
   ```

---

## **6. Enable Apache Rewrite Module**

Enable the `mod_rewrite` module:

```bash
sudo a2enmod rewrite
sudo systemctl reload apache2
```

---

## **7. Debugging Common Issues**

### **Issue 1: "Class \"DOMDocument\" Not Found"**

- Ensure the `php-xml` extension is installed:
  ```bash
  sudo apt install php-xml
  ```
- Restart Apache:
  ```bash
  sudo systemctl restart apache2
  ```

### **Issue 2: "Not Found" on Pretty Permalinks**

1. Check that `.htaccess` exists and contains the correct rules (see above).
2. Ensure the Apache virtual host configuration allows overrides:
   ```apache
   <Directory /srv/www/freshline>
       AllowOverride All
   </Directory>
   ```
3. Reload Apache:
   ```bash
   sudo systemctl reload apache2
   ```

### **Issue 3: "The Requested URL Was Not Found"**

- Verify the page or post exists in WordPress Admin.
- Check the `DirectoryIndex` in the virtual host file:
  ```apache
  DirectoryIndex index.php
  ```
- Ensure the `/contact/` page slug exists in WordPress.

---

## **8. Test the Setup**

Access the site:

```
http://127.0.0.1:8087
```

For specific pages:

```
http://127.0.0.1:8087/contact/
```

---

## **9. Check Logs for Errors**

If you encounter issues, check the Apache error log:

```bash
sudo tail -f /var/log/apache2/error.log
```
