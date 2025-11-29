---
layout: custom
title: "SPRINT 2: INSTAL·LACIÓ, CONFIGURACIÓ DE PROGRAMARI DE BASE I GESTIÓ DE FITXERS"
---

# Sistemes de fitxers i particions

## Mida sector

Pel que fa a l'emmagatzematge físic, la unitat elemental es coneix com a sector. Aquest espai, destinat a allotjar la informació, té una capacitat predeterminada de 512 bytes, la qual és inalterable.

## Mida block

A nivell del Sistema Operatiu, l'espai mínim lògic per emmagatzemar informació s'anomena bloc o clúster. Encara que la configuració estàndard sol ser de 4096 bytes, aquest valor és modificable durant el procés de formatació de la unitat.

És important notar que en un mateix disc físic, cada partició és independent; per tant, cadascuna pot tenir el seu propi sistema de fitxers i una mida de clúster diferent.

Anàlisi de l'exemple: En el cas pràctic, comparem dues mètriques. La primera comanda ens retorna el pes literal del contingut ("Bon dia", que ocupa 8 bytes). En canvi, la segona comanda ens mostra l'ocupació real al disc, revelant l'espai mínim que el sistema ha reservat (un clúster sencer) per allotjar aquest fitxer tan petit.

![1](Imatges/1.png)

## Fragmentació interna

Aquesta situació es dóna quan hi ha una discrepància entre la mida del fitxer i la del bloc. Si el bloc és excessivament gran per a la informació que conté, la part sobrant d'aquest bloc queda buida i no es pot aprofitar per a cap altra dada, resultant en una pèrdua d'emmagatzematge.

## Fragmentació externa

Amb el pas del temps i l'activitat diària, l'emmagatzematge lliure del disc perd la seva continuïtat. En lloc de tenir un únic bloc gran d'espai buit, aquest queda escampat en petits forats separats entre si, fent més difícil guardar fitxers grans de manera seguida.

## Tipus de formateig

- Format de Baix Nivell (Físic): Aquest procés és el més dràstic; elimina absolutament tota la informació magnètica i retorna el disc al seu estat original de fàbrica. No es tracta d'una funció estàndard del Sistema Operatiu, per la qual cosa requereix eines de software específiques (sovint del fabricant).

- Format de Nivell Mitjà: A més de netejar el sistema de fitxers, realitza un escaneig superficial. Si troba sectors danyats, els etiqueta com a inutilitzables per evitar errors futurs, tot i que no té capacitat per reparar el dany físic.

- Format d'Alt Nivell (Lògic): És l'operació més ràpida i superficial. La seva funció es limita exclusivament a restablir l'estructura del sistema de fitxers, deixant l'espai llest per a ser sobreescrit sense comprovar l'estat físic del disc.

**D'una manera mes clara ho compararem amb una taula.**

| Nivell de Formatat | Acció Principal | Detalls i Requisits |
| :--- | :--- | :--- |
| **Baix Nivell** | Esborrat físic complet (restauració de fàbrica). | No es pot realitzar des del Sistema Operatiu estàndard; requereix programari específic de tercers o del fabricant. |
| **Nivell Mitjà** | Eliminació del sistema de fitxers. | Realitza un escaneig per detectar sectors defectuosos i els marca com a "no utilitzables", tot i que no corregeix el dany físic. |
| **Alt Nivell** | Neteja ràpida (lògica) de l'índex d'arxius. | És el formatat més superficial; només restableix l'estructura de fitxers sense comprovar l'estat del disc. |

## Gestió de particions

Aquest sistema implementa una virtualització de l'emmagatzematge. La seva funció principal és crear un nivell intermedi entre el maquinari físic i el sistema operatiu, permetent fusionar diversos dispositius (ja siguin discos sencers o particions individuals) en una única entitat gestionable de manera conjunta.

### Comandes

Per consultar la capacitat total i l'estructura de les unitats connectades, executarem la instrucció fdisk amb el paràmetre de llista (-l).

![2](image.png)

Per aïllar la informació relativa a la mida del bloc, canalitzem la sortida cap a grep, indicant-li que mostri només les línies que coincideixin amb el terme "Block".

![3](image-1.png)

Per diagnosticar la fragmentació externa, fem ús de l'eina e4defrag. Aquesta ens generarà un informe indicant si la partició té un nivell de dispersió que requereixi ser desfragmentat.

![4](Imatges/4.png)

Si volem aplicar la desfragmentació (en lloc de només simular-la o comprovar-la), haurem d'executar la instrucció ometent el paràmetre -c.

![5](Imatges/5.png)

### GPARTED

GParted és l'eina gràfica per excel·lència de l'entorn GNOME destinada a la gestió integral de discos, permetent operacions com generar, moure o suprimir particions. Tot i que ofereix una gran flexibilitat per formatar en diversos sistemes de fitxers (com ara NTFS, EXT4 o FAT32), té una limitació important: no permet personalitzar la mida del bloc (cluster size), ja que aquest paràmetre s'assigna automàticament.

#### Via interficie gràfica (GPARTED)

El primer pas requereix iniciar el programa. Un cop dins, cal localitzar el selector d'unitats situat a la part superior dreta de la finestra i assegurar-nos que tenim marcat el disc correcte abans de fer cap operació.

![alt text](Imatges/20.png)

A continuació, ens dirigim a la barra de menús superior. Cliquem sobre la pestanya "Dispositivo" i, dins del desplegable, seleccionem l'opció "Crear tabla de particiones".

![alt text](Imatges/21.png)

El sistema mostrarà una finestra d'advertència (ja que s'esborraran les dades). Dins d'aquest quadre de diàleg, hem de desplegar la llista de tipus de taula i seleccionar específicament l'opció gpt abans de continuar.

![alt text](Imatges/22.png)

Amb la taula ja definida, visualitzarem l'espai com a "no assignat". Fem clic amb el botó dret sobre aquesta àrea i seleccionem l'opció per generar una nova partició.

![alt text](Imatges/23.png)

En aquesta finestra de configuració, hem d'establir el sistema de fitxers com a NTFS. A més, tenim l'oportunitat de definir manualment la capacitat (mida) que volem assignar a aquest nou volum.

![alt text](Imatges/24.png)

Com a pas últim, cal fer efectives totes les operacions pendents. Per fer-ho, premem la icona de validació (el vist o "tick" verd) de la barra d'eines i confirmem l'execució a la finestra emergent que apareix.

![alt text](Imatges/25.png)

#### Via CLI (Command Line Interface)

Per dur a terme aquesta tasca utilitzarem la utilitat **fdisk**. Assumint que en el pas anterior ja hem localitzat el dispositiu objectiu, executem la comanda i reproduïm la seqüència d'accions que s'il·lustra a la imatge adjunta.

![alt text](Imatges/7.png)

Seguidament, donem l'ordre per afegir una nova partició al disc.

![alt text](Imatges/8.png)

**Verificació**: En aquest punt, confirmem que el procés de generació s'ha completat amb èxit.

![alt text](Imatges/9.png)

Seguidament, utilitzarem l'ordre **mkfs.ext4** per donar format a la partició. Aprofitarem per establir manualment una mida de bloc personalitzada de 2048 bytes.

![alt text](Imatges/10.png)

Per corroborar que la configuració s'ha aplicat correctament, executem la següent instrucció.

![alt text](Imatges/11.png)

Pel que fa a la segona partició, li assignarem el format **NTFS**. Aquest pas és essencial per garantir la plena compatibilitat amb sistemes Windows.

![alt text](Imatges/12.png)

Com a conclusió, executarem la utilitat gràfica GParted per realitzar una validació visual de l'estat final de les particions.

![alt text](Imatges/13.png)

### Muntatge

Per fer aquest apartat primerament començarem creant una carpeta i arxiu a la ruta **/mnt**.

![alt text](Imatges/16.png)

* Muntem temporalment amb ```mount -t ext4 /dev/sdb1``` **/mnt/particio1**, i afegim un arxiu dintre.

![alt text](Imatges/15.png)

* Si reiniciem la particio que acabem de muntar ja no es trobara, pero els arxius que hem creat no se han borrat ja que encara estan emmagatzemades al disc.

* A continuació podem fer-ho de manera persistent. Per fer-ho editarem el fitxer **/etc/fstab**.

![alt text](Imatges/18.png)

* Si ara reiniciem amb aquest cas es persistent.

![alt text](Imatges/19.png)

## Gestió de procesos

Un procés és la instància d'un programa en execució. A cadascun se li assigna un identificador únic (PID), està associat a un usuari propietari i pot trobar-se en diversos estats (com ara en execució, en espera o aturat). El sistema operatiu és el responsable de la planificació i la distribució del temps de CPU entre tots els processos.

### Eines bàsiques de gestió de processos

Per gestionar els processos, disposem d'unes eines fonamentals:

    Per visualitzar-los:

        ps, top, htop → Mostren els processos actius.

    Per finalitzar-los:

        kill, pkill → Tanquen un procés pel seu PID o nom.

    Per gestionar la prioritat:

        nice, renice → Ajusten la prioritat d'execució.

    Per controlar serveis (daemons):

        systemctl, service → Inicien, aturen o reinicien serveis del sistema.

**Aspectes pràctics**: Cal recordar que un procés hereta els permisos de l'usuari que l'ha llançat i pot estar associat tant a un servei del sistema com a una sessió d'usuari.

* A continuació, veurem com utilitzar aquestes eines a nivell bàsic.

## Gestió d'usuaris i grups i permisos

El model de seguretat de Linux es basa en els conceptes d'usuaris i grups, que defineixen de manera precisa qui pot accedir, modificar o executar arxius i processos al sistema.

### Tipus d'Usuaris

**Usuari Normal**: Un usuari estàndard que pot iniciar sessió i treballar dins del seu entorn i espai personal. Els seus permisos són limitats per protegir la integritat del sistema.

**Superusuari (root)**: L'administrador del sistema. Té accés i control absolut sobre totes les operacions i arxius. S'ha d'utilitzar amb extrema cura.

**Usuari de Servei (Daemon)**: Comptes especials creats per a l'execució de serveis o aplicacions (com www-data per a un servidor web o mysql per a la base de dades). No poden iniciar sessió interactiva.

**Usuari de Sistema**: Són similars als usuaris de servei i solen tenir un UID (User ID) baix (normalment per sota de 1000). Estan reservats per a processos i funcions internes del sistema operatiu.

### Grups

Un grup és una col·lecció d'usuaris que comparteixen els mateixos permisos sobre certs arxius o directoris. Cada usuari pertany a:

    Un grup principal, que es defineix al crear l'usuari.

    Múltiples grups secundaris, als quals es pot afegir posteriorment.

Els grups són una eina essencial per a la gestió eficient de permisos, ja que permeten, per exemple, concedir accés a una carpeta compartida a tot un equip de treball d'una sola vegada, en lloc de configurar els permisos per a cada usuari individualment.

## Fitxers importants

* En Linux, la informació d'usuaris i grups es gestiona de manera centralitzada mitjançant fitxers de configuració de text ubicats dins del directori /etc.

Explicació **/etc/passwd**:

![alt text](<Gestió d'usuaris i grups i permisos img/1.png>)

Cada línia representa un usuari i conté 7 camps separats per dos punts:

**nom_usuari:x:UID:GID:GECOS:directori_home:shell**

Descripció detallada de cada camp

1. **nom_usuari**

    Exemple: root, anna, mysql

    Descripció:

        Nom únic que identifica l'usuari al sistema

        És el que s'utilitza per iniciar sessió

        Normalment té un màxim de 32 caràcters

2. **x (camp de contrasenya)**

    Exemple: x, *, !

    Descripció:

        x indica que la contrasenya està emmagatzemada a /etc/shadow

        * o ! vol dir que el compte està blocat

        Si està buit, l'usuari no té contrasenya (insicur)

3. **UID (User ID)**

    Exemple: 0, 1000, 33

    Descripció:

        Número d'identificació únic de l'usuari

        0 = usuari root (superusuari)

        1-999 = usuaris del sistema (serveis)

        1000+ = usuaris normals

4. **GID (Group ID)**

    Exemple: 0, 1000, 33

    Descripció:

        Número del grup principal de l'usuari

        Defineix els permisos per defecte per a nous arxius

5. **GECOS (Informació addicional)**

    Exemple: Anna Garcia,,,, Pere Lopez,Vendes,555-1234

    Descripció:

        Informació opcional sobre l'usuari

        Normalment només s'inclou el nom complet

        Format: Nom complet,Despatx,Telefon,Altres

6. **directori_home**

    Exemple: /home/anna, /root, /var/www

    Descripció:

        Directori personal de l'usuari

        On s'emmagatzemen els seus arxius personals

        Directori per defecte en iniciar sessió

7. **shell**

    Exemple: /bin/bash, /bin/sh, /usr/sbin/nologin

    Descripció:

        Intèrpret d'ordres que s'executa en iniciar sessió

        /bin/bash = shell Bash normal

        /usr/sbin/nologin o /bin/false = no permet inici de sessió (comptes de servei)

Explicació **/etc/shadow**:

![alt text](<Gestió d'usuaris i grups i permisos img/2.png>)

L'arxiu /etc/shadow conté la informació de les contrasenyes dels usuaris i les polítiques d'expiració. És un arxiu segur que només pot llegir l'usuari root.

Cada línia representa un usuari i conté 9 camps separats per dos punts:

**nom_usuari:contrasenya_encryptada:darrers_canvis:minims:maxims:avis:inactiu:caducitat:camp_reserva**

Descripció detallada de cada camp

1. **nom_usuari**

    Exemple: root, anna, mysql

    Descripció:

        Nom de l'usuari (ha de coincidir amb /etc/passwd)

        Serveix com a clau d'enllaç entre els dos arxius

2. **contrasenya_encryptada**

    Exemple: $6$rounds=5000$t..., *, !!

    Descripció:

        Contrasenya encryptada amb hash

        * o !! = compte blocat o sense contrasenya

        Format: $algoritme$salt$hash

        Algoritmes comuns: $1$ (MD5), $5$ (SHA-256), $6$ (SHA-512)

3. **darrers_canvis (last change)**

    Exemple: 19157, 0

    Descripció:

        Data de l'últim canvi de contrasenya en dies des de l'1/1/1970

        0 = ha de canviar-la en el proper login

        19157 = 19,157 dies des de l'1/1/1970

4. **minims (minimum days)**

    Exemple: 0, 7

    Descripció:

        Dies mínims que han de passar abans de poder canviar la contrasenya

        0 = es pot canviar en qualsevol moment

5. **maxims (maximum days)**

    Exemple: 99999, 90

    Descripció:

        Dies màxims que la contrasenya és vàlida

        99999 = quasi etern (273 anys)

        90 = ha de canviar-la cada 90 dies

6. **avis (warning days)**

    Exemple: 7, 0

    Descripció:

        Quants dies abans de la caducitat s'envia un avís

        7 = avisa 7 dies abans que caduqui

7. **inactiu (inactive days)**

    Exemple: -1, 30

    Descripció:

        Dies de gràcia després de caducar abans que el compte es desactivi

        -1 = sense període d'inactivitat

8. **caducitat (expiration date)**

    Exemple: ``, 20000

    Descripció:

        Data absoluta de caducitat del compte en dies des de l'1/1/1970

        Buit = el compte no caduca mai

9. **camp_reserva (reserved field)**

    Exemple: (buit)

    Descripció:

        Camp reservat per a ús futur

        Normalment està buit

Explicació **/etc/group**:

![alt text](<Gestió d'usuaris i grups i permisos img/14.png>)

L'arxiu /etc/group conté la informació dels grups del sistema i els seus membres. Defineix els grups d'usuaris i les seves relacions.

Estructura de cada línia

Cada línia representa un grup i conté 4 camps separats per dos punts:

**nom_grup:contrasenya_grup:GID:llista_membres**

Descripció detallada de cada camp

1. **nom_grup**

    Exemple: root, users, sudo, www-data

    Descripció:

        Nom del grup

        Ha de ser únic al sistema

        Normalment en minúscules

2. **contrasenya_grup**

    Exemple: x, *

    Descripció:

        x indica que la contrasenya del grup està a /etc/gshadow

        * o buit = no hi ha contrasenya de grup

        Rarament s'utilitza en sistemes moderns

3. **GID (Group ID)**

    Exemple: 0, 100, 1000, 33

    Descripció:

        Número d'identificació únic del grup

        0 = grup root

        1-999 = grups del sistema

        1000+ = grups d'usuaris normals

4. **llista_membres**

    Exemple: anna,pere,marta, root, (buit)

    Descripció:

        Llista d'usuaris que són membres del grup, separats per comes

        No inclou l'usuari que té aquest grup com a grup primari

        Buit = cap usuari addicional al grup

Explicació **/etc/gshadow**:

![alt text](<Gestió d'usuaris i grups i permisos img/15.png>)

L'arxiu **/etc/gshadow** conté la informació segura dels grups, incloent contrasenyes de grup i administradors. És la contrapart segura de **/etc/group**.

Estructura de cada línia

Cada línia representa un grup i conté 4 camps separats per dos punts:

**nom_grup:contrasenya_encryptada:administradors:membres**

Descripció detallada de cada camp

1. **nom_grup**

    Exemple: root, sudo, developers

    Descripció:

        Nom del grup (ha de coincidir amb /etc/group)

        Serveix com a clau d'enllaç entre els dos arxius

2. **contrasenya_encryptada**

    Exemple: !, $6$rounds=5000$..., *

    Descripció:

        Contrasenya encryptada per canviar al grup amb newgrp

        ! o * = no hi ha contrasenya de grup

        Contrasenya vàlida = hash encryptat

        Rarament s'utilitza en sistemes moderns

3. **administradors**

    Exemple: anna,root, pere, (buit)

    Descripció:

        Llista d'usuaris que poden gestionar el grup

        Poden afegir/eliminar membres i canviar la contrasenya del grup

        Separats per comes

4. **membres**

    Exemple: marta,jordi, user1,user2, (buit)

    Descripció:

        Llista d'usuaris que són membres del grup

        Ha de coincidir amb el camp de membres de /etc/group

        Separats per comes

També tenim l’utilitat que ve en instal·lar **gnome-system-tools**. Que permet un poquet més.

![alt text](<Gestió d'usuaris i grups i permisos img/16.png>)

## Comandes bàsiques

### Adduser

![alt text](<Gestió d'usuaris i grups i permisos img/3.png>)

### Userdel

* Aqui elimino el usuari.

![alt text](<Gestió d'usuaris i grups i permisos img/4.png>)

* Aqui creo l'usuari amb useradd i faig les comprovacions adients. Tambe canvio el tipus de shell.

![alt text](<Gestió d'usuaris i grups i permisos img/5.png>)

* Aqui canvio la contraseña amb la comanada **passwd**.

![alt text](<Gestió d'usuaris i grups i permisos img/6.png>)

* De aquest manera podem bloquejar el usuari, si observem el shadow podem veure que es veu de manera diferent.

![alt text](<Gestió d'usuaris i grups i permisos img/7.png>)

* I amb el parametre "-U" ho podem llevar.

![alt text](<Gestió d'usuaris i grups i permisos img/8.png>)

* Comanda addgroup per crear un grup. I amb groupmod podem canviar coses dels grups.

![alt text](<Gestió d'usuaris i grups i permisos img/9.png>)

* De aquesta manera podem assignar usuaris a grups.

![alt text](<Gestió d'usuaris i grups i permisos img/10.png>)

* Comprovació

![alt text](<Gestió d'usuaris i grups i permisos img/11.png>)

* Amb **deluser** podem també eliminar o desasignar del grup un usuari.

![alt text](<Gestió d'usuaris i grups i permisos img/12.png>)

* Amb **su** ens podem connectar al usuari.

![alt text](<Gestió d'usuaris i grups i permisos img/13.png>)

### Permisos

* Per fer aquest apartat he creat la carpeta palomes. Tambe un arxiu prova i he cambiat el grup propietari.

![alt text](Permisos/1.png)

* Aqui podem observar la jerarquia dels permisos

![alt text](Permisos/2.png)

Aqui estic configurant els permisos de la carpeta /var/palomes:

    Canvio el grup propietari de nick a paloma

    Afegeixo l'usuari ctre al grup paloma

    Estableixo permisos 750 a la carpeta

![alt text](Permisos/3.png)

Si em connecto desde el usuari nick puc comprovar que puc fer de tot.

![alt text](Permisos/4.png)

En canvi si ho faig desde el usuari cire no puc fer-ho.

![alt text](Permisos/5.png)

I si ho faig desde el usuari ferran no puc ni entrar dins.

![alt text](Permisos/6.png)

Aqui estic ampliant els permisos de la carpeta /var/palomes:

    Afegeixo l'usuari ferran al grup paloma

    Afegeixo l'usuari detvy al grup paloma

    Dono permisos d'escriptura al grup amb chmod g+w

![alt text](Permisos/7.png)

Aqui estic verificant els permisos de la carpeta /var/palomes:

    Com l'usuari deivy (del grup paloma):

        Puc accedir a la carpeta

        Puc crear fitxers (touch ptl)

    Com l'usuari ferran (també del grup paloma):

        Puc accedir a la carpeta i llistar contingut

        Puc eliminar fitxers (demana confirmació perquè el fitxer és protegit)

Resultat que confirmo:

    Tots dos usuaris del grup paloma poden accedir i gestionar fitxers

    Els fitxers existents tenen permisos restringits (protegits contra escriptura)

    La carpeta té permisos correctes perquè el grup pugui treballar

![alt text](Permisos/8.png)

Aqui estic comprovant els permisos de la carpeta **/var/palomes** amb diferents usuaris del grup:

Com a deivy (grup paloma):

    Puc entrar a la carpeta

    Puc crear fitxers nous (ptl)

Com a ferran (grup paloma):

    Puc entrar i veure el contingut

    Puc eliminar fitxers (demana confirmació)

Confirmo que:

    La carpeta té els permisos correctes per al grup

    Tots els membres del grup poden crear i eliminar fitxers

    Els fitxers estan protegits contra escriptura (per això demana confirmació)

![alt text](Permisos/9.png)

Aqui estic veient un problema amb els permisos de la carpeta **/var/palomes**:

El que funciona:

    deivy pot crear fitxers (ptlvaras)

    ferran pot crear fitxers (ddd)

    Tots dos poden accedir a la carpeta

El problema:

    ferran NO pot eliminar el fitxer ptlvaras de deivy

    La carpeta mostra drwxrwx--T (el sticky bit T està actiu)

La causa:

El sticky bit (la T al final dels permisos) està activat. Això vol dir que:

    Els usuaris només poden eliminar els seus propis fitxers

    No poden eliminar fitxers d'altres usuaris, encara que siguin del mateix grup

El sticky bit està limitant la capacitat dels usuaris del grup per eliminar fitxers d'altres membres.

![alt text](Permisos/10.png)

Finalment per als permisos **SUID**, he fet un metode que serveix per a escalar privilegis amb aquest cas he donat permisos SUID a la /bin/bash, el que em permet aixo es desde qualsevol usuari no privilegiat poden aconseguir una shell privilegiada, ja que quan ho executem ho estem fent com a root.

## ACL

### Importància de les ACL a Ubuntu

Raons principals per utilitzar ACL

1. Flexibilitat en gestió de permisos

    Superen les limitacions del model usuari/grup/altres

    Permeten assignar múltiples usuaris i grups al mateix recurs

    Ofereixen control granular d'accés

2. Escalabilitat en entorns complexos

    Necessàries en sistemes amb múltiples usuaris i grups

    Essencials en servidors compartits

    Importants en entorns corporatius

3. Seguretat més precisa

    Permeten implementar polítiques d'accés detallades

    Milloren el principi de mínim privilegi

    Faciliten l'auditoria d'accés

![alt text](ACL/1.png)

![alt text](ACL/2.png)

![alt text](ACL/3.png)

## Umask

Què és la umask?

Màscara que determina els permisos per defecte per a nous arxius i directoris.

On es configura a Ubuntu?

Arxius principals:

    Sistema: /etc/profile i /etc/bash.bashrc

    Usuari: ~/.bashrc

Comprovar umask actual:

**umask**

![alt text](<Gestió d'usuaris i grups i permisos img/17.png>)

Valors per defecte a Ubuntu

**Usuaris normals:**
    umask: 0002

    Arxius: 664 (rw-rw-r--)

    Directoris: 775 (rwxrwxr-x)

**Usuari root:**

    umask: 0022

    Arxius: 644 (rw-r--r--)

    Directoris: 755 (rwxr-xr-x)

![alt text](<Gestió d'usuaris i grups i permisos img/18.png>)

**Com funciona el càlcul?**

Permisos base:

    Arxius: 666 (rw-rw-rw-)

    Directoris: 777 (rwxrwxrwx)

Exemple umask 002:

Arxiu:   666 - 002 = 664 (rw-rw-r--)
Directori: 777 - 002 = 775 (rwxrwxr-x)

* Aqui he canviat temporalment el umask i he fet una provat creant una carpeta i un arxiu.

![alt text](<Gestió d'usuaris i grups i permisos img/19.png>)

* Per a fer-ho permanenment podem fer-ho modificant l'arxiu **login.defs**

![alt text](<Gestió d'usuaris i grups i permisos img/21.png>)

## Gestió avançada

## PAM

## Còpies de seguretat i automatització de tasques
## Quotes d'usuari
