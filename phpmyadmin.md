# Commands
```bash
$ wget https://files.phpmyadmin.net/phpMyAdmin/5.2.3/phpMyAdmin-5.2.3-all-languages.tar.gz
$ tar -xzf phpMyAdmin-5.2.3-all-languages.tar.gz
$ mkdir -p /srv/phpmyadmin
$ cp -a phpMyAdmin-5.2.3-all-languages/. /srv/phpmyadmin/
$ mkdir -p /var/lib/phpmyadmin/tmp
$ chown -R www-data:www-data /var/lib/phpmyadmin
$ chmod 700 /var/lib/phpmyadmin/tmp
$ cp /srv/phpmyadmin/config.sample.inc.php /srv/phpmyadmin/config.inc.php
$ openssl rand -hex 16 # generate blowfish_secret   
$ vim /srv/phpmyadmin/config.inc.php
```

# Files

## /srv/phpmyadmin/config.inc.php
>Variables:
>- key
>- phpmyadmin-user
>- phpmyadmin-password
Change or add next lines
```php
$cfg['blowfish_secret'] = '<key>';
$cfg['TempDir'] = '/var/lib/phpmyadmin/tmp';
$cfg['CookiePath'] = '/phpmyadmin/';

$i = 1;
$cfg['Servers'][$i]['auth_type'] = 'cookie';
$cfg['Servers'][$i]['host'] = 'localhost';
$cfg['Servers'][$i]['compress'] = false;
$cfg['Servers'][$i]['AllowNoPassword'] = false;

$cfg['Servers'][$i]['controluser'] = '<phpmyadmin-user>';
$cfg['Servers'][$i]['controlpass'] = '<phpmyadmin-password>';

$cfg['Servers'][$i]['pmadb'] = 'phpmyadmin';
$cfg['Servers'][$i]['bookmarktable'] = 'pma__bookmark';
$cfg['Servers'][$i]['relation'] = 'pma__relation';
$cfg['Servers'][$i]['table_info'] = 'pma__table_info';
$cfg['Servers'][$i]['table_coords'] = 'pma__table_coords';
$cfg['Servers'][$i]['pdf_pages'] = 'pma__pdf_pages';
$cfg['Servers'][$i]['column_info'] = 'pma__column_info';
$cfg['Servers'][$i]['history'] = 'pma__history';
$cfg['Servers'][$i]['table_uiprefs'] = 'pma__table_uiprefs';
$cfg['Servers'][$i]['tracking'] = 'pma__tracking';
$cfg['Servers'][$i]['userconfig'] = 'pma__userconfig';
$cfg['Servers'][$i]['recent'] = 'pma__recent';
$cfg['Servers'][$i]['favorite'] = 'pma__favorite';
$cfg['Servers'][$i]['users'] = 'pma__users';
$cfg['Servers'][$i]['usergroups'] = 'pma__usergroups';
$cfg['Servers'][$i]['navigationhiding'] = 'pma__navigationhiding';
$cfg['Servers'][$i]['savedsearches'] = 'pma__savedsearches';
$cfg['Servers'][$i]['central_columns'] = 'pma__central_columns';
$cfg['Servers'][$i]['designer_settings'] = 'pma__designer_settings';
$cfg['Servers'][$i]['export_templates'] = 'pma__export_templates';
```
