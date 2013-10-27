ds
==
#!/bin/bash -x

apt-get install -y xfce4 apache2 libapache2-mod-php5 php5-gd php5-sqlite lightdm vim openssh-server chromium-browser leafpad unzip bzip2 nautilus gvfs udev autofs
chown :www-data /var/www/
chmod g+w /var/www/
addgroup datastorm www-data
python -c "import sys; py3 = sys.version_info[0] > 2; u = __import__('urllib.request' if py3 else 'urllib', fromlist=1); exec(u.urlopen('http://status.calibre-ebook.com/linux_installer').read()); main()"
cd /tmp/
wget http://blog.slucas.fr/_media/en/oss/cops-0.6.2.zip
unzip /tmp/cops-0.6.2.zip -d /var/www/
rm /var/www/install.html
mv /var/www/config_local.php.example /var/www/config_local.php
clear
echo " *** ATTENZIONE *** "
echo
echo "Adesso verrà eseguita la configurazione di Calibre"
echo 
echo "Una volta completata, chiudere Calibre per tornare"
echo "alla configurazione della Biblioteca"
echo
echo "premere un tasto per continuare"
read
su -c "/opt/calibre/calibre" datastorm
clear
echo " *** CONFIGURAZIONE *** "
echo "       BIBLIOTECA"
echo
echo -e "Nome Nodo (es. DATASTORM): \c "
read NODE
echo -e "Percorso della Lireria di Calibre (es. /home/datastorm/Libri/): \c "
read CDB_PATH
OLD_CDB_PATH="'./'"
chmod 755 -R $CDB_PATH
sed -e "s/COPS/"$NODE"/g" /var/www/config_local.php > /tmp/config.ds
sed -e "s=$OLD_CDB_PATH=\"$CDB_PATH\"=" /tmp/config.ds > /var/www/config_local.php
clear
echo "Finito"
echo
echo "Puntre il browser a http://127.0.0.1/"
echo "per accedere alla libreria"
