# FreshLine

FreshLine is a project aimed at providing a web application for a cleaning company.

## Technology

- Wordpress
- MySQL
- Elementor
- PHP 7.4.0 or higher.

## Installation

To install FreshLine, clone the repository and navigate to the project directory:

```bash
git clone git@github.com:vladik9/freshLine.git
cd freshLine
```

### Dump image sql

Import backup.sql

Create a:

```bash
'DB_NAME', 'wordpress'
```

```bash
'DB_USER' :  'wpuser'
```

```bash
'DB_PASSWORD' : 'password'
```

```bash
'DB_HOST' : 'localhost'
```

Visit [link](https://ubuntu.com/tutorials/install-and-configure-wordpress "config") for more information!
update db to refelct new links

```bash
UPDATE wp_options SET option_value = 'http://yourIp:yourPort/' WHERE option_name = 'siteurl';
UPDATE wp_options SET option_value = 'http://yourIp:yourPort/' WHERE option_name = 'home';
```

### Restoring

```bash
mysql -u root -p
CREATE DATABASE wordpress;
SHOW GRANTS FOR 'root'@'localhost';
GRANT ALL PRIVILEGES ON wordpress.* TO 'root'@'localhost';
FLUSH PRIVILEGES;
sudo systemctl restart mysql
mysql -h localhost -u root -p --verbose wordpress < wordpress.sql
```

###

Wordpress user: 
l: admin
p:autoAppLog123

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Contact

For any questions or feedback, please open an issue or contact us at [v.cornici@gmail.com](mailto:email@example.com).

## Images

### Web

![img](./images/web.gif "Web")

### Mobile


![img](./images/mobile.gif "Mobile")
