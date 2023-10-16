
# **_Jarkom Modul 2 - IT21_**
| Nama                           | NRP      |
|--------------------------------|-------------|
| Maria Teresia Elvara Bumbungan | 5027211042  |
| Nathania Elirica Aliyah        | 5027211057  |



## **_Nomor 1_**
Yudhistira akan digunakan sebagai DNS Master, Werkudara sebagai DNS Slave, Arjuna merupakan Load Balancer yang terdiri dari beberapa Web Server yaitu Prabakusuma, Abimanyu, dan Wisanggeni.

### Topologi 3
![1](https://i.ibb.co/wgNrXrp/image.png)

### Network configuration
- Pandudewanata
```bash
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 10.74.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 10.74.2.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 10.74.3.1
	netmask 255.255.255.0

auto eth4
iface eth4 inet static
	address 10.74.4.1
	netmask 255.255.255.0
```
- Yudhistira
```bash
auto eth0
iface eth0 inet static
	address 10.74.1.2
	netmask 255.255.255.0
	gateway 10.74.1.1
```
- Werkudara
```
auto eth0
iface eth0 inet static
	address 10.74.2.2
	netmask 255.255.255.0
	gateway 10.74.2.
```
- Sadewa
```
auto eth0
iface eth0 inet static
	address 10.74.3.2
	netmask 255.255.255.0
	gateway 10.74.3.1
```
- Nakula
```
auto eth0
iface eth0 inet static
	address 10.74.3.3
	netmask 255.255.255.0
	gateway 10.74.3.1
```
- Prabukusuma
```
auto eth0
iface eth0 inet static
	address 10.74.4.2
	netmask 255.255.255.0
	gateway 10.74.4.1
```
- Abimanyu
```
auto eth0
iface eth0 inet static
	address 10.74.4.3
	netmask 255.255.255.0
	gateway 10.74.4.1
```
- Wissanggeni
```
auto eth0
iface eth0 inet static
	address 10.74.4.4
	netmask 255.255.255.0
	gateway 10.74.4.1
```
- Arjuna
```
auto eth0
iface eth0 inet static
	address 10.74.4.5
	netmask 255.255.255.0
	gateway 10.74.4.1
```
## **_Nomor 2 dan 3_**
Buatlah website utama pada node arjuna dengan akses ke arjuna.yyy.com dengan alias www.arjuna.yyy.com dengan yyy merupakan kode kelompok.

Dengan cara yang sama seperti soal nomor 2, buatlah website utama dengan akses ke abimanyu.yyy.com dan alias www.abimanyu.yyy.com.

### Yudhistira
- apt-get update
- apt-get install bind9 dnsutils -y
- nano /etc/bind/named.conf.local
Buat konfigurasi
```bash
zone "arjuna.IT21.com" {
	type master;
	file "/etc/bind/arjuna/arjuna.it21.com";
};
zone "abimanyu.IT21.com" {
	type master;
	file "/etc/bind/abimanyu/abimanyu.it21.com";
};
```
- mkdir /etc/bind/arjuna
- nano /etc/bind/arjuna/arjuna.IT21.com
Buat konfigurasi
```bash
$TTL 604800
@       IN      SOA arjuna.IT21.com. root.arjuna.IT21.com. (
        1       ; Serial
        604800  ; Refresh
        86400   ; Retry
        2419200 ; Expire
        604800 )        ; Negative Cache TTL
;
@       IN      NS      arjuna.IT21.com.
@       IN      A       10.74.4.5
www     IN      CNAME   arjuna.IT21.com.
```
- mkdir /etc/bind/abimanyu
- nano /etc/bind/abimanyu/abimanyu.IT21.com
Buat konfigurasi
```bash
$TTL 604800
@       IN      SOA abimanyu.IT21.com. root.abimanyu.IT21.com. (
        1       ; Serial
        604800  ; Refresh
        86400   ; Retry
        2419200 ; Expire
        604800 )        ; Negative Cache TTL
;
@       IN      NS      abimanyu.IT21.com.
@       IN      A       10.74.4.3
www     IN      CNAME   abimanyu.IT21.com.
```
- service bind9 restart

### Nakula dan Sadewa
- apt-get update
- apt-get install dnsutils
- nano /etc/resolv.conf
Sambungkan dengan server dituju

```bash
nameserver 10.74.1.2
nameserver 10.74.4.5
nameserver 10.74.4.3
```
### Testing
#### Nakula dan Sadewa
- ping abimanyu.IT21.com
- ping arjuna.IT21.com

![2](https://i.ibb.co/rbXcQTj/image.png)


## **_Nomor 4_**
Kemudian, karena terdapat beberapa web yang harus di-deploy, buatlah subdomain parikesit.abimanyu.yyy.com yang diatur DNS-nya di Yudhistira dan mengarah ke Abimanyu.

### Yudhistira
- nano /etc/bind/abimanyu/abimanyu.IT21.com
Tambahkan konfigurasi
```bash
$TTL 604800
@       IN      SOA     abimanyu.IT21.com. root.abimanyu.IT21.com. (
        1       ; Serial
        604800  ; Refresh
        86400   ; Retry
        2419200 ; Expire
        604800 )        ; Negative Cache TTL
;

@       IN      NS      abimanyu.IT21.com.
@       IN      A       10.74.4.3
www     IN      CNAME   abimanyu.IT21.com.
parikesit       IN      A       10.74.4.3
```
- service bind9 restart

### Testing
#### Nakula dan Sadewa
- ping parikesit.abimanyu.IT21.com
![Teks Alternatif](https://i.ibb.co/bmZ4vF7/image.png)

## **_Nomor 5_**
Buat juga reverse domain untuk domain utama. (Abimanyu saja yang direverse)

### Yudhistira
- nano /etc/bind/named.conf.local
Tambahkan code berikut 
```bash
zone "3.4.74.10.in-addr.arpa" {
	type master;
        file "/etc/bind/abimanyu/3.4.74.10.in-addr.arpa";
};'
```

- nano /etc/bind/abimanyu/3.4.74.10.in-addr.arpa
```bash
 $TTL 604800
@       IN      SOA     abimanyu.IT21.com root.abimanyu.IT21.com. (
        1       ; Serial
        604800  ; Refresh
        86400   ; Retry
        2419200 ; Expire
        604800 )        ; Negative Cache TTL
;

@       IN      NS abimanyu.IT21.com
3       IN      PTR     abimanyu.IT21.com.
```
- service bind9 restart

### Testing
#### Nakula dan Sadewa
- host -t PTR 10.74.4.3
![s](https://i.ibb.co/gPghZnq/image.png)

## **_Nomor 6_**
Agar dapat tetap dihubungi ketika DNS Server Yudhistira bermasalah, buat juga Werkudara sebagai DNS Slave untuk domain utama.

### Yudhistira
- nano /etc/bind/named.conf.local
Modifikasi konfigurasi 
```
zone “arjuna.IT21.com” {
	type master;
	notify yes;
	also-notify { 10.74.2.2; }; #IP Werkudara
	allow-transfer { 10.74.2.2; };
	file “/etc/bind/arjuna/arjuna.IT21.com”
};
zone “abimanyu.IT21.com” {
	type master;
	notify yes;
	also-notify { 10.74.2.2; }; #IP Werkudara
	allow-transfer { 10.74.2.2; };
	file “/etc/bind/abimanyu/abimanyu.IT21.com”
};
```

- service bind9 restart

### Werkudara
- apt-get update
- apt-get install bind9
- nano /etc/bind/named.conf.local
Tambahkan
```
zone “arjuna.IT21.com” {
	type slave;
	masters { 10.74.1.2; }; #IP  Yudhistira
	file “/var/lib/bind/arjuna.IT21.com”;
};
zone “abimanyu.IT21.com” {
	type slave;
	masters { 10.74.1.2; }; #IP  Yudhistira
	file “/var/lib/bind/abimanyu.IT21.com”;
};
```
- service bind9 restart

### Yudhistira
Untuk mengecek apabila yudhistira bermasalah
- service bind9 stop

### Testing
#### Sadewa dan Nakula
- nano /etc/resolv.conf
```bash
nameserver 10.74.1.2
nameserver 10.74.2.2
nameserver 10.74.4.5
nameserver 10.74.4.3
```

- ping abimanyu.IT21.com -c 5
- ping www.abimanyu.IT21.com -c 5
![image25](https://i.ibb.co/YZRw3TP/image.png)

## **_Nomor 7_**
Seperti yang kita tahu karena banyak sekali informasi yang harus diterima, buatlah subdomain khusus untuk perang yaitu baratayuda.abimanyu.yyy.com dengan alias www.baratayuda.abimanyu.yyy.com yang didelegasikan dari Yudhistira ke Werkudara dengan IP menuju ke Abimanyu dalam folder Baratayuda.

### Yudhistira

- nano /etc/bind/abimanyu/abimanyu.IT21.com
Sebagai alias
```bash
$TTL 604800
@       IN      SOA     abimanyu.IT21.com. root.abimanyu.IT21.com. (
        1       ; Serial
        604800  ; Refresh
        86400   ; Retry
        2419200 ; Expire
        604800 )        ; Negative Cache TTL
;

@       IN      NS      abimanyu.IT21.com.
@       IN      A       10.74.4.3
www     IN      CNAME   abimanyu.IT21.com.
parikesit       IN      A       10.74.4.3
ns1     IN      A       10.74.2.2
baratayuda      IN      NS      ns1
```
- nano /etc/bind/named.conf.options
Modifikasi konfigurasi
```bash
options {
	directory \"/var/cache/bind\";

	// Uncomment the following block if you want to specify forwarders.
	// Replace 0.0.0.0 with the actual IP addresses of your forwarders.
	// forwarders {
	//    0.0.0.0;
	// };

	//========================================================================
	// If BIND logs error messages about the root key being expired,
	// you will need to update your keys. See https://www.isc.org/bind-keys
	//========================================================================
	// dnssec-validation auto;

	allow-query { any; };
	auth-nxdomain no; # conform to RFC1035
	listen-on-v6 { any; };
};
```


- service bind9 restart

### Werkudara
- apt-get update
- apt-get install bind9
- mkdir /etc/bind/baratayuda
- nano /etc/bind/named.conf.options
Modifikasi konfigurasi
```bash
 options {
            directory \"/var/cache/bind";

            // If there is a firewall between you and nameservers you want
            // to talk to, you may need to fix the firewall to allow multiple
            // ports to talk.  See http://www.kb.cert.org/vuls/id/800113

            // If your ISP provided one or more IP addresses for stable
            // nameservers, you probably want to use them as forwarders.
            // Uncomment the following block, and insert the addresses replacing
            // the all-0's placeholder.

            // forwarders {
            //      0.0.0.0;
            // };

            //========================================================================
            // If BIND logs error messages about the root key being expired,
            // you will need to update your keys.  See https://www.isc.org/bind-keys
            //========================================================================
            //dnssec-validation auto;

            allow-query{any;};

            auth-nxdomain no;    # conform to RFC1035
            listen-on-v6 { any; };
    };
```

- nano /etc/bind/named.conf.local
Tambahkan konfigurasi
```bash
 zone "baratayuda.abimanyu.IT21.com" {

        type master;

        file "/etc/bind/baratayuda/baratayuda.abimanyu.IT21.com";

    };
```
- nano etc/bind/baratayuda/baratayuda.abimanyu.IT21.com
```bash
Tambahkan konfigurasi
$TTL 604800
@       IN      SOA     baratayuda.abimanyu.IT21.com. root.baratayuda.abimanyu.IT21.com. (
        1       ; Serial
        604800  ; Refresh
        86400   ; Retry
        2419200 ; Expire
        604800 )        ; Negative Cache TTL
;

@       IN      NS      baratayuda.abimanyu.IT21.com.
@       IN      A       10.74.4.3
www     IN      CNAME   baratayuda.abimanyu.IT21.com.
```

- service bind9 restart

### Testing
#### Nakula dan Sadewa
- ping baratayuda.abimanyu.IT21.com
- ping www.baratayuda.abimanyu.IT21.com 
![image28](https://i.ibb.co/g70NcGH/image.png)

## **_Nomor 8_**
Untuk informasi yang lebih spesifik mengenai Ranjapan Baratayuda, buatlah subdomain melalui Werkudara dengan akses rjp.baratayuda.abimanyu.yyy.com dengan alias www.rjp.baratayuda.abimanyu.yyy.com yang mengarah ke Abimanyu.

### Yudhistira
- nano /etc/bind/abimanyu/abimanyu.IT21.com
Tambahkan konfigurasi
```bash
$TTL 604800
@       IN      SOA     abimanyu.IT21.com. root.abimanyu.IT21.com. (
        1       ; Serial
        604800  ; Refresh
        86400   ; Retry
        2419200 ; Expire
        604800 )        ; Negative Cache TTL
;

@       IN      NS      abimanyu.IT21.com.
@       IN      A       10.74.4.3
www     IN      CNAME   abimanyu.IT21.com.
parikesit       IN      A       10.74.4.3
ns1     IN      A       10.74.2.2
baratayuda      IN      NS      ns1
```
- service bind9 restart

### Werkudara
- nano /etc/bind/baratayuda/baratayuda.abimanyu.IT21.com
Tambahkan konfigurasi pada direktori alias
```bash
$TTL 604800
@       IN      SOA     baratayuda.abimanyu.IT21.com. root.baratayuda.abimanyu.IT21.com. (
        1       ; Serial
        604800  ; Refresh
        86400   ; Retry
        2419200 ; Expire
        604800 )        ; Negative Cache TTL
;

@       IN      NS      baratayuda.abimanyu.IT21.com.
@       IN      A       10.74.4.3
www     IN      CNAME   baratayuda.abimanyu.IT21.com.
rjp     IN      A       10.74.4.3
www.rjp IN      CNAME   baratayuda.abimanyu.IT21.com.
```
- service bind9 restart

### Testing
#### Nakula dan Sadewa
- ping rjp.baratayuda.abimanyu.IT21.com 
- ping www.rjp.baratayuda.abimanyu.IT21.com
![image2](https://i.ibb.co/xMFS2hv/image.png)

## **_Nomor 9_**
Arjuna merupakan suatu Load Balancer Nginx dengan tiga worker (yang juga menggunakan nginx sebagai webserver) yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Lakukan deployment pada masing-masing worker.

### Worker (Prabukusuma, Abimanyu, Wisanggeni)
Lakukan hal yang sama di ketiga worker 

- apt-get update && apt install nginx php php-fpm -y
- php -v
- apt-get install lynx -y
- apt-get install nginx -y
- mkdir /var/www/jarkom

- nano /var/www/jarkom/index.php
Tambahkan isi sesuai dengan instruksi drive 
```bash
<?php
\$hostname = gethostname();
\$date = date('Y-m-d H:i:s');
\$php_version = phpversion();
\$username = get_current_user();

echo \"Hello World!<br>\";
echo \"Saya adalah: $username<br>\";
echo \"Saat ini berada di: $hostname<br>\";
echo \"Versi PHP yang saya gunakan: $php_version<br>\";
echo \"Tanggal saat ini: $date<br>\";
?>
```
- service php7.0-fpm start

- nano /etc/nginx/sites-available/jarkom
Buat konfigurasi
``` server {

        listen 80;

        root /var/www/jarkom;

        index index.php index.html index.htm;
        server_name _;

        location / {
                try_files \$uri \$uri/ /index.php?\$query_string;
        }

        # pass PHP scripts to FastCGI server
        location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
        }

    location ~ /\.ht {
                deny all;
        }

        error_log /var/log/nginx/jarkom_error.log;
        access_log /var/log/nginx/jarkom_access.log;
    }
```
- rm -rf /etc/nginx/sites-available/default

- ln -s /etc/nginx/sites-available/jarkom /etc/nginx/sites-enabled

- service nginx restart

- nginx -t


### Testing
#### Nakula dan Sadewa
- apt-get update
- apt-get install lynx
- lynx http://10.74.4.2
![image10]()
- lynx http://10.74.4.3
![image31]()
- lynx http://10.74.4.4
![image32]()

## **_Nomor 10_**
Kemudian gunakan algoritma Round Robin untuk Load Balancer pada Arjuna. Gunakan server_name pada soal nomor 1. Untuk melakukan pengecekan akses alamat web tersebut kemudian pastikan worker yang digunakan untuk menangani permintaan akan berganti ganti secara acak. Untuk webserver di masing-masing worker wajib berjalan di port 8001-8003. Contoh
    - Prabakusuma:8001
    - Abimanyu:8002
    - Wisanggeni:8003


### Arjuna
- nano /etc/nginx/sites-available/arjuna.IT21.com
```
echo 'upstream back {
  server 192.243.2.5:8001; # IP PrabuKusuma
  server 192.243.2.4:8002; # IP Abimanyu
  server 192.243.2.6:8003; # IP Wisanggeni
}

server {
  listen 80;
  server_name arjuna.IT21.com www.arjuna.IT21.com;

  location / {
    proxy_pass http://back;
  }
}
```

- ln -s /etc/nginx/sites-available/jarkom /etc/nginx/sites-enabled/jarkom

- rm /etc/nginx/sites-enabled/default

- service nginx restart

### Worker (Prabukusuma, Abimanyu, Wisanggeni)
Lakukan hal yang sama di ketiga worker dengan menyesuaikan port pada soal

- nano /etc/nginx/sites-available/jarkom
Buat konfigurasi dimana port menyesuaikan 800_ diisi sesuai port worker masing masing
``` server {

        listen 800_;

        root /var/www/jarkom;

        index index.php index.html index.htm;
        server_name _;

        location / {
                try_files \$uri \$uri/ /index.php?\$query_string;
        }

        # pass PHP scripts to FastCGI server
        location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
        }

    location ~ /\.ht {
                deny all;
        }

        error_log /var/log/nginx/jarkom_error.log;
        access_log /var/log/nginx/jarkom_access.log;
    }
```
### Testing
#### Nakula dan Sadewa
- apt-get update
- apt-get install lynx
- lynx http://10.74.4.2:8001
![image10]()
- lynx http://10.74.4.3:8002
![image31]()
- lynx http://10.74.4.4:8003
![image32]()
- lynx arjuna.IT21.com
![image32]()

## **_Nomor 11_**
Selain menggunakan Nginx, lakukan konfigurasi Apache Web Server pada worker Abimanyu dengan web server www.abimanyu.yyy.com. Pertama dibutuhkan web server dengan DocumentRoot pada /var/www/abimanyu.yyy

### Abimanyu
Melakukan download kebutuhan website dari google drive serta meletakkannya pada direktori var/www/
-  cd /var/www
- wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=1a4V23hwK9S7hQEDEcv9FL14UkkrHc-Zc' -O abimanyu
- unzip abimanyu -d abimanyu.IT21
- mv abimanyu.IT21/abimanyu.yyy.com/* abimanyu.IT21
- rmdir abimanyu.it03/abimanyu.yyy.com
- nano /etc/apache2/sites-available/abimanyu.IT21.conf
```bash
Membuat konfigurasi 
<VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/parikesit.abimanyu.IT21
        ServerName parikesit.abimanyu.IT21.com
        ServerAlias www.parikesit.abimanyu.IT21.com

        RewriteEngine On 

        ErrorLog \${APACHE_LOG_DIR}/error.log
        CustomLog \${APACHE_LOG_DIR}/access.log combined

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with \"a2disconf\".
        #Include conf-available/serve-cgi-bin.conf
</VirtualHost>
```

- a2ensite abimanyu.IT21.conf
- service apache2 restart

### Testing
#### Nakula dan Sadewa
- lynx abimanyu.IT21.com
![image27](https://i.ibb.co/26QMkGM/image.png)

## **_Nomor 12_**
Setelah itu ubahlah agar url www.abimanyu.yyy.com/index.php/home menjadi www.abimanyu.yyy.com/home.

### Abimanyu
- nano /etc/apache2/sites-available/abimanyu.IT21.conf
Menambahka konfigurasi agar dapat www.abimanyu.yyy.com/home
```bash
<VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/parikesit.abimanyu.IT21
        ServerName parikesit.abimanyu.IT21.com
        ServerAlias www.parikesit.abimanyu.IT21.com

        <Directory /var/www/abimanyu-IT21>
        Options +Indexes
    </Directory>

    Alias /home /var/www/abimanyu-IT21/index.php/home

       RewriteEngine On 
     
        ErrorLog \${APACHE_LOG_DIR}/error.log
        CustomLog \${APACHE_LOG_DIR}/access.log combined

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with \"a2disconf\".
        #Include conf-available/serve-cgi-bin.conf
</VirtualHost>
```

- service apache2 restart

### Testing
#### Nakula dan Sadewa

- lynx abimanyu.IT21.com/home
![image2](https://i.ibb.co/s2sbWqq/image.png)

## **_Nomor 13_**
Selain itu, pada subdomain www.parikesit.abimanyu.yyy.com, DocumentRoot disimpan pada /var/www/parikesit.abimanyu.yyy

### Abimanyu
- nano /etc/apache2/sites-available/parikesit.abimanyu.IT21.com.conf
Membuat konfigurasi
```bash
<VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/parikesit.abimanyu.IT21
        ServerName parikesit.abimanyu.IT21.com
        ServerAlias www.parikesit.abimanyu.IT21.com

        ErrorLog \${APACHE_LOG_DIR}/error.log
        CustomLog \${APACHE_LOG_DIR}/access.log combined

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with \"a2disconf\".
        #Include conf-available/serve-cgi-bin.conf
</VirtualHost>
```

- a2ensite parikesit.abimanyu.IT21.com.conf

- service apache2 restart

### Testing
#### Nakula dan Sadewa
- lynx parikesit.abimanyu.IT21.com
![image2](https://i.ibb.co/x8kDcZB/image.png)


## **_Nomor 14_**
Pada subdomain tersebut folder /public hanya dapat melakukan directory listing sedangkan pada folder /secret tidak dapat diakses (403 Forbidden).

### Abimanyu
- nano /etc/apache2/sites-available/parikesit.abimanyu.it21.com
tambahkan code berikut dimana +Indexes berguna untuk public dan -Indexes untuk secret 
  ```
<VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/parikesit.abimanyu.IT21
        ServerName parikesit.abimanyu.IT21.com
        ServerAlias www.parikesit.abimanyu.IT21.com

        <Directory /var/www/parikesit.abimanyu.IT21/public>
                Options +Indexes
        </Directory>

        <Directory /var/www/parikesit.abimanyu.IT21/secret>
                Options -Indexes
        </Directory>

        Alias \"/public\" \"/var/www/parikesit.abimanyu.IT21/public\"
        Alias \"/secret\" \"/var/www/parikesit.abimanyu.IT21/secret\"
       
        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn

        ErrorLog \${APACHE_LOG_DIR}/error.log
        CustomLog \${APACHE_LOG_DIR}/access.log combined

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with \"a2disconf\".
        #Include conf-available/serve-cgi-bin.conf
</VirtualHost>
```
- service apache2 restart

### Testing
#### Nakula dan Sadewa

- lynx parikesit.abimanyu.IT21.com/public
![image20](https://i.ibb.co/rpK66Kr/image.png)
- lynx parikesit.abimanyu.IT21.com/secret
![image20](https://i.ibb.co/hMpRqqY/image.png)

## **_Nomor 15_**
Buatlah kustomisasi halaman error pada folder /error untuk mengganti error kode pada Apache. Error kode yang perlu diganti adalah 404 Not Found dan 403 Forbidden.

### Abimanyu
- nano /etc/apache2/sites-available/parikesit.abimanyu.it03.conf 
Menambahkan konfigurasi ErrorDocument
```
<VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/parikesit.abimanyu.IT21
        ServerName parikesit.abimanyu.IT21.com
        ServerAlias www.parikesit.abimanyu.IT21.com

        <Directory /var/www/parikesit.abimanyu.IT21/public>
                Options +Indexes
        </Directory>

        <Directory /var/www/parikesit.abimanyu.IT21/secret>
                Options -Indexes
        </Directory>

        <Directory /var/www/parikesit.abimanyu.IT21>
            Options +FollowSymLinks -Multiviews
            AllowOverride All
        </Directory>

        Alias \"/public\" \"/var/www/parikesit.abimanyu.IT21/public\"
        Alias \"/secret\" \"/var/www/parikesit.abimanyu.IT21/secret\"
        Alias \"/js\" \"/var/www/parikesit.abimanyu.IT21/public/js\"

        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn

        ErrorDocument 404 /error/404.html
        ErrorDocument 403 /error/403.html

        ErrorLog \${APACHE_LOG_DIR}/error.log
        CustomLog \${APACHE_LOG_DIR}/access.log combined

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with \"a2disconf\".
        #Include conf-available/serve-cgi-bin.conf
</VirtualHost>
```

- service apache2 restart

### Testing
#### Nakula dan Sadewa
-  lynx parikesit.abimanyu.IT21.com/apakekbingung
Untuk mencoba string random
![image12](https://i.ibb.co/9h9McM5/image.png)

2. lynx parikesit.abimanyu.IT21.com/secret
Untuk 403 forbidden
![image3](https://i.ibb.co/XWL3WQN/image.png)

## **_Nomor 16_**
Buatlah suatu konfigurasi virtual host agar file asset www.parikesit.abimanyu.yyy.com/public/js menjadi 
www.parikesit.abimanyu.yyy.com/js 

### Abimanyu
- nano /etc/sites-available/parikesit.abimanyu.IT21.conf
Menambahkan konfigurasu `Alias /js /var/www/parikesit-abimanyu-it11/public/js`
```
<VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/parikesit.abimanyu.IT21
        ServerName parikesit.abimanyu.IT21.com
        ServerAlias www.parikesit.abimanyu.IT21.com

        <Directory /var/www/parikesit.abimanyu.IT21/public>
                Options +Indexes
        </Directory>

        <Directory /var/www/parikesit.abimanyu.IT21/secret>
                Options -Indexes
        </Directory>


        Alias \"/public\" \"/var/www/parikesit.abimanyu.IT21/public\"
        Alias \"/secret\" \"/var/www/parikesit.abimanyu.IT21/secret\"
        Alias \"/js\" \"/var/www/parikesit.abimanyu.IT21/public/js\"

        RewriteEngine On
        RewriteCond %{REQUEST_URI} ^/public/images/(.*)(abimanyu)(.*\.(png|jpg))
        RewriteCond %{REQUEST_URI} !/public/images/abimanyu.png
        RewriteRule abimanyu http://parikesit.abimanyu.IT21.com/public/images/abimanyu.png\$1 [L,R=301]

        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn

        ErrorDocument 404 /error/404.html
        ErrorDocument 403 /error/403.html

        ErrorLog \${APACHE_LOG_DIR}/error.log
        CustomLog \${APACHE_LOG_DIR}/access.log combined

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with \"a2disconf\".
        #Include conf-available/serve-cgi-bin.conf
</VirtualHost>
```
- service apache2 restart

### Testing
#### Nakula dan Sadewa
- lynx parikesit.abimanyu.IT21.com/js
![image33](https://i.ibb.co/8jFFF7J/image.png)

# **_Nomor 17_**
Agar aman, buatlah konfigurasi agar www.rjp.baratayuda.abimanyu.yyy.com hanya dapat diakses melalui port 14000 dan 14400.
## Abimanyu
- nano /etc/apache2/sites-available/rjp.baratayuda.abimanyu.IT21.com.conf
Membuat konfigurasi dengan `<VirtualHost *:14000 *:14400>`
```bash
<VirtualHost *:14000 *:14400>
  ServerAdmin webmaster@localhost
  DocumentRoot /var/www/rjp.baratayuda.abimanyu.IT21
  ServerName rjp.baratayuda.abimanyu.IT21.com
  ServerAlias www.rjp.baratayuda.abimanyu.IT21.com

  ErrorLog \${APACHE_LOG_DIR}/error.log
  CustomLog \${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
- nano /etc/apache2/ports.conf
Menambahkan konfigurasi
```bash
# If you just change the port or add more ports here, you will likely also
# have to change the VirtualHost statement in
# /etc/apache2/sites-enabled/000-default.conf

Listen 80
Listen 14000
Listen 14400

<IfModule ssl_module>
        Listen 443
</IfModule>

<IfModule mod_gnutls.c>
        Listen 443
</IfModule>
```
### Testing
#### Nakula dan Sadewa

- lynx rjp.baratayuda.abimanyu.IT21.com:1234
![image23]()

- lynx rjp.baratayuda.abimanyu.IT21.com:14000
![image23]()

- lynx rjp.baratayuda.abimanyu.IT21.com:14400
![image23]()

## **_Nomor 18_**
### Abimanyu
- nano /etc/apache2/sites-available/rjp.baratayuda.abimanyu.it03.com
```
<VirtualHost *:14000 *:14400>
  ServerAdmin webmaster@localhost
  DocumentRoot /var/www/rjp.baratayuda.abimanyu.IT21
  ServerName rjp.baratayuda.abimanyu.IT21.com
  ServerAlias www.rjp.baratayuda.abimanyu.IT21.com

  <Directory /var/www/rjp.baratayuda.abimanyu.IT21>
          AuthType Basic
          AuthName \"Restricted Content\"
          AuthUserFile /etc/apache2/.htpasswd
          Require valid-user
  </Directory>

  ErrorLog \${APACHE_LOG_DIR}/error.log
  CustomLog \${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

- cd /etc/apache2/sites-available/
- htpasswd -c /etc/apache2/.htpasswd Wayang (kemudian, masukkan password baratayudaIT21)
### Testing 
#### Nakula dan Sadewa
- lynx rjp.baratayuda.abimanyu.IT21.com (tanpa auth)`
![image56]()
- lynx rjp.baratayuda.abimanyu.IT21.com:14000 -u Wayang:baratayudaIT21
![image28]()

# **_Nomor 19_**
### Abimanyu
- nano /etc/apache2/sites-available/000-default.conf
```bash
<VirtualHost *:80>
    ServerAdmin webmaster@abimanyu.IT21.com
    DocumentRoot /var/www/html

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined

    Redirect / http://www.abimanyu.IT21.com/
</VirtualHost>
```
- apache2ctl configtest
- service apache2 restart

### Testing
#### Nakula dan Sudawa
- lynx 10.74.4.3
![image28]()

## **_Nomor 20_**
### Abimanyu
- nano /etc/apache2/sites-available/parikesit.abimanyu.IT21.com.conf
```bash
<VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/parikesit.abimanyu.IT21
        ServerName parikesit.abimanyu.IT21.com
        ServerAlias www.parikesit.abimanyu.IT21.com

        <Directory /var/www/parikesit.abimanyu.IT21/public>
                Options +Indexes
        </Directory>

        <Directory /var/www/parikesit.abimanyu.IT21/secret>
                Options -Indexes
        </Directory>

        <Directory /var/www/parikesit.abimanyu.IT21>
            Options +FollowSymLinks -Multiviews
            AllowOverride All
        </Directory>

        Alias \"/public\" \"/var/www/parikesit.abimanyu.IT21/public\"
        Alias \"/secret\" \"/var/www/parikesit.abimanyu.IT21/secret\"
        Alias \"/js\" \"/var/www/parikesit.abimanyu.IT21/public/js\"

        RewriteEngine On
        RewriteCond %{REQUEST_URI} ^/public/images/(.*)(abimanyu)(.*\.(png|jpg))
        RewriteCond %{REQUEST_URI} !/public/images/abimanyu.png
        RewriteRule abimanyu http://parikesit.abimanyu.IT21.com/public/images/abimanyu.png\$1 [L,R=301]

        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn

        ErrorDocument 404 /error/404.html
        ErrorDocument 403 /error/403.html

        ErrorLog \${APACHE_LOG_DIR}/error.log
        CustomLog \${APACHE_LOG_DIR}/access.log combined

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with \"a2disconf\".
        #Include conf-available/serve-cgi-bin.conf
</VirtualHost>
```

### Testing
#### Nakula dan Sadewa
- lynx parikesit.abimanyu.IT21.com/alyaelvaraabimanyu`
![image37]()
