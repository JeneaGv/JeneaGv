# Containerizare-si-Virtualizare Lab01
# Server Virtual
Realizare lucrarii de laborator nr.1 unde ne familiarizam cu virtualizarea sistemelor de operare si configurarea unui server HTTP virtual.

Realizat de: Gavenco Evghenii, I2301 Data: 12.02.2025

# Scopul Lucrairii 
Descărcarea distributivul Debian pentru servere pentru arhitectura x64 (fără interfață grafică) și sistemul de virtualizare (hypervizor) QEMU.

Instalarea QEMU. Redenumirea imaginii distributivului Debian descărcat în debian.iso.

# Realizarea lucrarii
1.Descarcam Debian de pe site-ul oficial. Dupa descarcare, il adaugam in directorul dvd cu numele debian.iso
2.Descarcam si instalam QEMU.
3.Deschidem consola si introducem comanda: qemu-img create -f qcow2 debian.qcow2 8G pentru a crea imaginea discului pentru masina virtuala de dimensiunea 8 GB si formagt qcow2.
4.Instalam debian so pe masina virtuala folosind comanda: qemu-system-x86_64 -hda debian.qcow2 -cdrom dvd/debian.iso -boot d -m 2G
5.Alegem optiunea de Install si citim cu atentie urmatorii pasi pentru finisarea instalarii. Folosim urmatorii parametrii: nume calculator: debian; nume domeniu: debian.localhost; nume utilizator: user; parola utilizator: password.
![image](https://github.com/user-attachments/assets/5d27d588-3385-4a2c-8495-8ba47e6f8c62)
6.Repornim masina virtuala folosind comanda: qemu-system-x86_64 -hda debian.qcow2 -m 2G -smp 2 -device e1000,netdev=net0 -netdev user,id=net0,hostfwd=tcp::1080-:80,hostfwd=tcp::1022-:22
7.Instalam LAMP in masina virtuala. pentru a face asta folosim comanda su ca sa avem drepturi de superutilizator si dupa comenzile: apt update -y apt install -y apache2 php libapache2-mod-php php-mysql mariadb-server mariadb-client unzip
![image](https://github.com/user-attachments/assets/597db11e-3a34-456f-8ffa-fc36615c91ed)
![image](https://github.com/user-attachments/assets/9643c2cf-c21d-4686-96c5-a239600331e9)
8.Descarcarea SGBD phpmyadmin si CMS drupal cu ajutorul comenzii wget.
![image](https://github.com/user-attachments/assets/990c5fda-5bcb-4a56-b2c8-82f8f3b051aa)
![image](https://github.com/user-attachments/assets/053695ae-15db-4c20-82d0-3b4fdb7da25b)
9.Dezarhivați fișierele descărcate în directoarele:
 1.SGBD PhpMyAdmin ==> /var/www/phpmyadmin;
![image](https://github.com/user-attachments/assets/b01728f0-b4ab-4178-9cdb-1920086d9484)
 2.CMS Drupal ==> /var/www/drupal.
![image](https://github.com/user-attachments/assets/a99066ab-76d2-49a6-93da-9ddf8e188ee7)
![image](https://github.com/user-attachments/assets/a7c0f7b3-5da6-437e-ac25-911845cc6eee)
10Creați din linia de comandă baza de date drupal_db și utilizatorul bazei de date cu numele dvs.
 1.introducem comanda mysql -u root pentru a porni clientul MySQL
 ![image](https://github.com/user-attachments/assets/f7d57e2e-3abf-4b03-a23b-f9346ca343a7)
 2.Cream baza de date CREATE DATABASE drupal_db;
 3.Cream un user cu numele nostru CREATE USER 'Eugen'@'localhost' IDENTIFIED BY 'password';
 ![image](https://github.com/user-attachments/assets/319cec69-6084-4ae3-ae78-5fa4d21def69)
 4.acordam priviliegii user-ului nostru GRANT ALL PRIVILEGES ON drupal_db.* TO 'Eugen'@'localhost';
 FLUSH PRIVILEGES;
 ![image](https://github.com/user-attachments/assets/2ba153a0-e1fc-4db9-8fc4-ca8b2113e343)
11.În directorul /etc/apache2/sites-available creaam fișierul 01-phpmyadmin.conf cu continut
![image](https://github.com/user-attachments/assets/392f813e-9c6b-4b91-a1b4-dfe1171cd02c)
![image](https://github.com/user-attachments/assets/4056e6b1-2308-40f9-bdc5-61bacf6a6206)
12.Executam aceleasi pasi si pentru fisierul 02-drupal.conf
![image](https://github.com/user-attachments/assets/20fc63dc-72c0-4523-9541-06cd76f064f3)
Pentru a activa configurațiile, executam comenzile:
![image](https://github.com/user-attachments/assets/8a1ba074-1740-4b18-9926-52f24d73814a)
13.Adăugam în fișierul /etc/hosts următoarele linii:
127.0.0.1 phpmyadmin.localhost
127.0.0.1 drupal.localhost
![image](https://github.com/user-attachments/assets/415b947a-ef23-40ef-b1b2-c91615dbc3c1)
14.În consolă executam comanda:
În consolă executați comanda:
uname -a
![image](https://github.com/user-attachments/assets/1960eb4b-be00-445b-890d-a5777b37bea6)
Verificam disponibilitatea site-urilor http://drupal.localhost:1080 și http://phpmyadmin.localhost:1080. Finalizați instalarea site-urilor.
![image](https://github.com/user-attachments/assets/6a0dae2d-3e3d-4982-9c1c-a61c93775792)
![image](https://github.com/user-attachments/assets/374a01a7-6728-4dab-aba7-b0d8338e635e)
![image](https://github.com/user-attachments/assets/757b0fd8-2ba3-4a36-9229-0e19d3da8244)
![image](https://github.com/user-attachments/assets/6cdd6716-0f9e-4db2-b998-d749de732584)

 # Raspunsuri la intrebari 
 1.pentru a descarca un fisier cu wget se poate adauga in consola comanda: wget + link URL
 2.De ce este necesar să creați pentru fiecare site baza de date și utilizatorul său?
 2.1 Securitate îmbunătățită
Dacă toate site-urile ar folosi același utilizator MySQL, o breșă de securitate într-unul dintre ele ar putea compromite toate celelalte.
Fiecare utilizator are acces doar la baza de date specifică, reducând riscul de acces neautorizat la alte site-uri.
2.2. Gestionare mai ușoară
Dacă un site trebuie mutat pe un alt server sau șters, este mai simplu să gestionezi baza de date separată decât să cauți datele într-o bază de date comună.
Poți seta permisiuni diferite pentru fiecare utilizator în funcție de necesitățile fiecărui site.
2.3. Performanță optimizată
Unele CMS-uri (precum WordPress, Drupal) generează multe tabele. Dacă toate site-urile ar folosi aceeași bază de date, ar deveni greu de administrat și ar putea afecta performanța.
Separarea bazelor de date permite backup-uri și restaurări mai rapide pentru fiecare site în parte.
2.4. Izolare și evitare de conflicte
Unele CMS-uri sau aplicații web folosesc tabele cu nume similare (users, sessions, etc.). Dacă toate site-urile ar folosi aceeași bază de date, ar putea apărea conflicte.
O bază de date separată previne suprascrierea accidentală a datelor între site-uri.
2.5. Mai ușor de implementat strategii de backup și restaurare
Poți face backup doar pentru baza de date a unui anumit site fără să afectezi altele.
În caz de probleme, restaurarea unei baze de date individuale este mai rapidă decât a unei baze de date comune.
3.pentru a schimba portul trebuie sa editam fisierul de configurare si sa modificam portul in 1234. dupa facem restart pentru a vedea modificarile.
4.avantajele virtualizarii sunt: reducerea folosirii hardware-ului ,mediile de testare pot fi create și distruse fără a afecta infrastructura principală.
5.Setarea corectă a fusului orar pe un server asigură securitate, consistența datelor și sincronizarea corectă între servicii. Este o practică esențială pentru orice infrastructură IT fiabilă.
6.so are dimensiunea de 2 GB.
7.Pentru servere, se recomandă ca anumite directoare să fie plasate pe partiții separate pentru a îmbunătăți securitatea, performanța și gestionarea resurselor. Astfel, partiționarea discului ar trebui să includă:

Partiție separată pentru /var – Deoarece conține fișiere de log, cache și baze de date temporare, izolarea acestuia previne umplerea accidentală a discului principal.
Partiție separată pentru /tmp – Stochează fișiere temporare care pot consuma mult spațiu, iar separarea previne afectarea altor părți ale sistemului.
Partiție separată pentru /home – Utilizatorii își stochează fișierele aici, iar o partiție dedicată asigură că datele utilizatorilor nu afectează spațiul sistemului de operare.
Partiție separată pentru /usr/local (opțional) – Dacă se instalează multe programe în afara distribuției Debian, o partiție separată previne interferențele cu sistemul principal.
Partiție separată pentru /var/mail (dacă serverul este utilizat pentru e-mail) – Împiedică mesajele stocate să ocupe spațiul destinat altor servicii.
Acest tip de partiționare ajută la gestionarea mai eficientă a spațiului, îmbunătățește securitatea prin izolarea anumitor zone și previne situațiile în care un director suprasolicitat afectează întregul sistem.
# Bibliografie
https://www.debian.org/releases/bullseye/amd64/apcs03.en.html
https://ro.ubunlog.com/wget-exemple-fac-instrument/
