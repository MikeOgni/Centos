# Centos

## Installazione
Configurare le caratteristiche della macchina e nella sezione rete scegliere, nella voce connessa a, "Scheda con bridge".
Inserire la ISO Centos e avviare la macchina. 
Arrivare in questa schermata e scegliere italiano.
<p align="center"/>
<img src="https://user-images.githubusercontent.com/77325793/118485031-2f900d80-b718-11eb-9f3b-e16ce6769417.png" width="500"/>
</p>

Andare avanti e accedere alla sezione ethernet. Spuntare la punta "SI". 
<p align="center"/>
<img src="https://user-images.githubusercontent.com/77325793/118485776-1cca0880-b719-11eb-9940-334ad2ba10fb.png" width="500"/>
</p>


Proseguire con l'installazione arrivando alla creazione della password root e dell'utente. 
Nella schermata crea utente spuntare la casella "imposta come amministratore"
<table align="center">
 <tr>
  <th>Password di root</th>
  <th>Creazione utente</th>
 </tr>
  <tr>
 <td><img width="600" src="https://user-images.githubusercontent.com/77325793/118485975-5b5fc300-b719-11eb-96ec-286ec0571df9.png" /></td>
 <td><img width="600" src="https://user-images.githubusercontent.com/77325793/118485973-58fd6900-b719-11eb-99fe-e6f04c5733c5.png" /></td>
 </tr>
</table>

Aspettare fino alla fine dell'installazione.
>Ci possono volere svariati minuti.



Se tutto è andato a buon fine dovremmo trovarci in una schermata del genere:

<p align="center"/>
<img src="https://user-images.githubusercontent.com/77325793/118486538-07a1a980-b71a-11eb-9ef7-e8541e4383a2.png" width="500"/>
</p>

## SSH
Il servizio SSH ci permette di gestire la nostra macchina virtuale in modo remoto.
>Bisogna essere nella stessa rete.

Installare nano oppure vim che ci permetteranno di modificare/creare file (Va bene anche uno dei 2):
```
sudo yum install nano  
sudo yum install vim
```

Ora possiamo iniziare con il servizio SSH:
scaricare tramite il comando:
```
sudo yum install sshd
```
Attivare il servizio

```
systemctl start sshd
systemctl enable sshd
```

Accedere al file di configurazione che si trova in ***`/etc/ssh/sshd_config`***

<br/>
Aprirlo ed eseguire i seguenti passagi:


```
  PermitRootLogin no
  AllowUsers nomeUtente
```

>Tramite il file di configurazione è possibile cambiare la porta, in questo README non viene spiegato come modificarla 

Ricaricare il servizio tramite il comando
`systemctl restart sshd`

Aprire o se non lo si è già stato fatto in precendeza scaricare il programma ***PUTTY*** 
(clicca [qui](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)  per scaricarlo).

Avviare il programma, inserire l'indirizzo IP della macchina e la porta (quella di default è 22)
>Se non si conosce l'indirizzo IP digitare il comando `ip addr`

<p align="center"/>
<img src="https://user-images.githubusercontent.com/77325793/118489477-29506000-b71d-11eb-9ea1-a527b30b3360.png" width="500"/>
</p>
## HTTP
Il server Apche HTTP è il web server più utilizzato.
Per installarlo seguire i seguenti passaggi:
```
  yum install httpd
```

Abilitare il servizio tramite i comandi:
```
  systemctl start httpd
  systemctl enable httpd
```
C'è bisogno anche di configurare i firewall per accedere alla porta di servizio (default 80), per farlo seguire i comandi riportati qua sotto

```
  sudo firewall-cmd --permanent --add-service=http
  sudo firewall-cmd --permanent --add-service=https
  firewall-cmd --reload
```

Riavviare il servizio tramite il comando `systemctl restart httpd`

Sucessivamente andare su un qualsiasi browser e digitare il proprio IP sull'url

<p align="center"/>
<img src="https://user-images.githubusercontent.com/77325793/118491138-f7d89400-b71e-11eb-9530-cac82e793267.png" width="800"/>
</p>


## PHP
Installare php tramite il comando  `yum install php`
>Questo comando installa php versione 5.4 (se si vuole sapere la propria versione scrivere `php -v`)

Riavviare il servizio HTTP con il comando `systemctl restart httpd`

Entrare nella directory della Document Root tramite il comando `/var/www/html`
Successivamente creare un file denominato ***info.php*** e scrivere al suo interno il seguente codice
```
  <html>
    <head>
    </head>
    <body>
      <?php phpinfo(); ?>
    </body>
  </html>
```

Andare su un browser qualsiasi e digitare il proprio ip seguito da `/info.php`.
Si dovrebbe visualizzare il seguente sito:


<p align="center"/>
<img src="https://user-images.githubusercontent.com/77325793/118492487-57836f00-b720-11eb-829e-63cb0474a764.png" width="800"/>
</p>

## SQL
Prima di scaricare SQL eseguire il comando `sudo yum install wget`

Ora procedere con l'installazione:
```
wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm
rpm -ivh mysql-community-release-el7-5.noarch.rpm
yum install mysql-server
```
Ativare il servizio tramite il comando `systemctl start mysqld`

Scrivere il comando `mysql_secure_installation`, impostare una nuova password per accedere a PhpMyAdmin e tramite la foto inserire le seguenti richieste


<p align="center"/>
<img src="https://user-images.githubusercontent.com/77325793/118496413-666c2080-b724-11eb-9cdb-80d5853b4bae.png" width="700"/>
</p>



## PhpMyAdmin

Scaricare il DBMS tramite i comandi:
```
yum epel-release
yum install phpmyadmin
```

Andare nel file di configurazione `/etc/httpd/conf.d/phpMyAdmin.conf`
aggiungere un allias a scelta e commentare gli altri 2
`Alias /MioAlias /usr/share/phpMyAdmin`
>sostituire MioAlias con un nome a scelta
Scrivere `AllowOverride All` sotto `AddDefaultCharset UTF-8`


Successivamente scrivere su un browser `ip/MioAlias` e accedere 


## FTP
Installazione FTP:
```
  yum install vsftpd
  yum install ftp
  systemctl start vsftpd
  systemctl enable vsftpd
```

Impostare il firewall:
```
  firewall-cmd --permanent --add-service=ftp
  firewall-cmd --reload
```
Nel file ***/etc/vsftp/vsftp.conf***

modificare o aggiungere le seguenti righe:

```
  anonymous_enable=NO
  chroot_local_user=YES
  chroot_list_enable=YES
  allow_writeable_chroot=YES
  use_localtime=YES
```
Testare il funzionamento inserendo nell'url ***ftp://ip*** oppure con filezilla oppure scrivendo ***ftp localhost*** da macchina
## Wordpress

Eseguire i seguenti comandi
```
sudo mysql -u root -p
CREATE DATABASE sito;
CREATE USER utente@localhost IDENTIFIED BY password;
```
>utente e password ci serviranno per accedere a wordpress

```
GRANT ALL PRIVILEGES ON sito.* TO utente@localhost IDENTIFIED BY password;
FLUSH PRIVILEGES;
exit;
```
Dopo essere usciti da SQL installare WordPress e seguire i passaggi
```
sudo yum install php-gd
sudo systemctl restart httpd
sudo cd
sudo yum install wget
sudo wget http://wordpress.org/latest.tar.gz
sudo tar xzvf latest.tar.gz
sudo rsync -avP wordpress/ /var/www/html/
sudo cd /var/www/html/
sudo ls -ltrh /var/www/html/
sudo chown -R apache:apache /var/www/html/*
sudo cp wp-config-sample.php wp-config.php
sudo yum install vim
sudo vim wp-config.php
```

L'ultimo comando mostra il seguente file:
>sostituire nella sezione DB_NAME, DB_USER, DB_PASSWORD con le informazioni inserite in SQL all'inizio 
<p align="center"/>
<img src="https://user-images.githubusercontent.com/77325793/118503764-33795b00-b72b-11eb-8f4b-3e2a68951c2d.png" width="700"/>
</p>

Salvare il documento e scrivere il proprio indirizzo IP

<p align="center"/>
<img src="https://user-images.githubusercontent.com/77325793/118504012-6f142500-b72b-11eb-902c-3a7a42c97bd6.png" width="700"/>
</p>
 
Cliccare ***Install WordPress*** e proseguire

<p align="center"/>
<img src="https://user-images.githubusercontent.com/77325793/118504171-94089800-b72b-11eb-86c1-9c36fe674244.png" width="700"/>
</p>
