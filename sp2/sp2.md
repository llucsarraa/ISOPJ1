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

Per iniciar aquesta secció, procedim a generar un nou directori i un fitxer dins de la ubicació **/mnt**.

![alt text](Imatges/16.png)

Realitzem un muntatge manual de la unitat /dev/sdb1 al directori /mnt/particio1, especificant explícitament el sistema de fitxers ext4. Un cop la partició està accessible, procedim a generar un fitxer al seu interior.

![alt text](Imatges/15.png)

Cal tenir present que el muntatge manual és volàtil: si reiniciem el sistema, la connexió es perdrà (tot i que les dades segueixen emmagatzemades físicament al disc).

Per solucionar això i aconseguir que el muntatge sigui persistent i automàtic a l'arrencada, és necessari editar l'arxiu de configuració systema: /etc/fstab.

![alt text](Imatges/18.png)

Si procedim a reiniciar el sistema, comprovarem que el muntatge es realitza de manera automàtica, confirmant així que la configuració és permanent.

![alt text](Imatges/19.png)

## Gestió de procesos

Tècnicament, definim un procés com l'entitat dinàmica resultant d'executar un codi o programa. Per a la seva gestió, el sistema vincula cada procés a un usuari concret i li atorga una matrícula numèrica exclusiva anomenada PID. Al llarg del seu cicle de vida, un procés transita per diferents fases (actiu, en espera, zombi...), i és funció del Sistema Operatiu actuar com a àrbitre, administrant la planificació i l'ús de la CPU per a cadascun d'ells.

### Eines bàsiques de gestió de processos

### Eines de Gestió de Processos

Per administrar el sistema disposem de diverses utilitats de línia de comandes, classificades segons la seva finalitat:

| Acció | Comandes | Detalls |
| :--- | :--- | :--- |
| **Visualitzar** | `ps`, `top`, `htop` | Mostren la llista de tasques actives i el seu consum. |
| **Finalitzar** | `kill`, `pkill` | Permeten matar processos específics (mitjançant ID o nom). |
| **Prioritzar** | `nice`, `renice` | Ajusten la prioritat de la CPU ("niceness"). |
| **Serveis** | `systemctl`, `service` | Controlen els daemons del sistema (start/stop/restart). |

> **Conceptes clau:** Un procés sempre està vinculat a un usuari (del qual n'hereta els permisos) i pot funcionar com a part d'una sessió o com un servei de fons.

Tot seguit, posarem en pràctica aquests conceptes bàsics.

## Gestió d'usuaris i grups i permisos

L'arquitectura de seguretat en sistemes Linux es fonamenta en la distinció entre **usuaris i grups**. Aquests dos elements són la clau per gestionar els **privilegis**, determinant qui té autorització per interactuar (llegir, editar o llançar) amb els recursos del sistema, ja siguin fitxers o processos en marxa.

### Tipus d'Usuaris

Compte Convencional: Es tracta de l'usuari final que disposa de credencials per accedir al sistema. La seva capacitat d'acció està acotada al seu propi espai de treball, prevenint així danys accidentals a la configuració general.

Superusuari (Root): És la màxima autoritat dins l'entorn Linux. Disposa de permisos il·limitats per gestionar qualsevol recurs. El seu ús s'ha de restringir a tasques d'administració degut al risc que comporta.

Comptes de Dimonis (Serveis): Són identitats virtuals assignades a programari específic (ex: www-data, mysql) per executar-se de manera aïllada. Estan configurats per denegar l'inici de sessió manual (no tenen shell).

Comptes del Sistema: Similars als anteriors però vinculats a processos essencials del mateix Sistema Operatiu. Es poden identificar fàcilment perquè el seu UID sol estar reservat en el rang 0-999.

#### En forma de taula per a que quedi més clar.

| Rol | Funció | Detalls |
| :--- | :--- | :--- |
| **Usuari Normal** | Accés estàndard limitat al seu espai personal. | No pot modificar fitxers crítics del sistema. |
| **Root (Superusuari)** | Administrador amb control total. | Té accés irrestricte a tots els fitxers i comandes. |
| **Usuari de Servei** | Execució d'aplicacions en segon pla (Daemons). | Comptes sense accés a shell interactiva (ex: servidors web). |
| **Usuari de Sistema** | Gestió de processos interns del SO. | Es caracteritzen per tenir un **UID baix** (<1000). |

### Grups

Els **grups** funcionen com a contenidors lògics que permeten unificar diversos usuaris sota uns mateixos criteris de seguretat. Dins d'aquesta jerarquia, distingim dos tipus d'assignació:

1. Grup Primari: S'assigna automàticament en generar el compte d'usuari (sol tenir el mateix nom).

2. Grups Secundaris: Són agrupacions addicionals a les quals l'usuari s'uneix per guanyar privilegis extra.

**Utilitat**: L'ús de grups és fonamental per optimitzar l'administració de sistemes. En lloc de gestionar permisos usuari per usuari (cosa que seria lenta i propensa a errors), l'administrador pot assignar drets a un grup sencer, aplicant els canvis de manera instantània a tots els seus membres.

## Fitxers importants

En l'entorn Linux, la gestió d'identitats no depèn d'una base de dades complexa, sinó que es basa en fitxers de text pla emmagatzemats al directori /etc. Aquests fitxers actuen com a registre centralitzat per a tots els usuaris i grups del sistema.

**Anàlisi** de /etc/passwd: Aquest és el fitxer principal de referència. Conté la definició de tots els comptes d'usuari registrats al sistema. És un fitxer públic (llegible per tothom) que associa cada usuari amb els seus atributs bàsics.

![alt text](<Gestió d'usuaris i grups i permisos img/1.png>)

Cada línia del fitxer defineix un usuari mitjançant 7 camps delimitats per dos punts (`:`):

| # | Camp | Funció i Detalls |
| :--- | :--- | :--- |
| 1 | **Nom d'usuari** | L'identificador únic utilitzat per al login (ex: `root`, `anna`). |
| 2 | **Contrasenya (x)** | La `x` indica que la contrasenya xifrada es troba protegida a `/etc/shadow`. Signes com `*` o `!` denoten un compte bloquejat. |
| 3 | **UID** | ID numèric de l'usuari. <br>• **0:** Root.<br>• **<1000:** Sistema.<br>• **>1000:** Usuaris normals. |
| 4 | **GID** | ID del grup principal assignat a l'usuari. |
| 5 | **GECOS** | Informació descriptiva opcional (Nom complet, telèfon, etc.). |
| 6 | **Directori Home** | La ruta de la carpeta personal de l'usuari (ex: `/home/anna`). |
| 7 | **Shell** | L'intèrpret de comandes. Per a usuaris reals sol ser `/bin/bash`; per a serveis, `/usr/sbin/nologin`. |

### Explicació: /etc/shadow

![alt text](<Gestió d'usuaris i grups i permisos img/2.png>)

Aquest fitxer és crític per a la seguretat del sistema. Emmagatzema les credencials xifrades i les polítiques de caducitat. A diferència del `passwd`, aquest arxiu té permisos restringits i **només pot ser llegit per l'usuari root**.

Cada línia es compon de 9 camps separats per dos punts (`:`):

| # | Camp | Funció i Detalls |
| :--- | :--- | :--- |
| 1 | **Nom d'usuari** | Coincideix amb el del fitxer `/etc/passwd`. Serveix d'enllaç entre tots dos arxius. |
| 2 | **Contrasenya (Hash)** | Conté la clau xifrada (format `$id$salt$hash`).<br>• Si comença per `$6$`, és SHA-512.<br>• Si conté `*` o `!!`, el compte està bloquejat/deshabilitat. |
| 3 | **Últim canvi** | Data de l'última modificació de la contrasenya, comptada en **dies** des de l'1 de gener de 1970 (Època Unix). |
| 4 | **Mínim (Dies)** | Temps mínim que ha de passar entre canvis de contrasenya (evita canvis constants). `0` permet canvis immediats. |
| 5 | **Màxim (Dies)** | Període de validesa de la contrasenya. En arribar a aquesta xifra (ex: `90`), l'usuari està obligat a canviar-la. `99999` equival a "mai". |
| 6 | **Avís** | Nombre de dies previs a la caducitat en què el sistema començarà a notificar l'usuari (ex: `7` dies abans). |
| 7 | **Inactivitat** | Dies de marge ("gràcia") un cop la contrasenya ha caducat abans de bloquejar totalment el compte. |
| 8 | **Caducitat del compte** | Data absoluta (en dies des de 1970) en què el compte quedarà inhabilitat, independentment de la contrasenya. |
| 9 | **Reservat** | Camp sense ús actual, reservat per a futures funcionalitats del sistema. |

### Explicació: /etc/group

![alt text](<Gestió d'usuaris i grups i permisos img/14.png>)

Aquest fitxer defineix els grups existents al sistema i determina quins usuaris hi tenen accés. És essencial per a la gestió de permisos col·lectius.

Cada línia consta de 4 camps separats per dos punts (`:`):

| # | Camp | Descripció i Detalls |
| :--- | :--- | :--- |
| 1 | **Nom del Grup** | L'etiqueta única que identifica el grup al sistema (ex: `sudo`, `docker`). Per convenció, s'escriu en minúscules. |
| 2 | **Contrasenya** | Habitualment conté una `x` (referència a `/etc/gshadow`) o un asterisc. En l'administració moderna de Linux, les contrasenyes de grup estan en desús. |
| 3 | **GID** | Identificador numèric del grup.<br>• **0:** Root.<br>• **1-999:** Grups de sistema.<br>• **1000+:** Grups d'usuari estàndard. |
| 4 | **Membres** | Llista d'usuaris (separats per comes) que pertanyen a aquest grup com a **grup secundari**. <br>*Nota: Si és el grup principal d'un usuari, el seu nom no sol aparèixer en aquesta llista.* |

### Explicació: /etc/gshadow

![alt text](<Gestió d'usuaris i grups i permisos img/15.png>)

Aquest fitxer actua com a complement de seguretat per a `/etc/group`. S'utilitza per emmagatzemar les claus xifrades dels grups i definir-ne els administradors, protegirnt aquesta informació sensible de mirades indiscretes (només root el pot llegir).

Cada línia segueix l'esquema: `nom:password:admins:membres`.

| # | Camp | Funció i Detalls |
| :--- | :--- | :--- |
| 1 | **Nom del Grup** | L'identificador del grup. Ha de tenir una correspondència exacta amb una entrada existent a `/etc/group`. |
| 2 | **Contrasenya (Hash)** | Clau xifrada utilitzada per accedir al grup (mitjançant la comanda `newgrp`).<br>• Si conté `!` o `*`, l'accés per contrasenya està deshabilitat.<br>• Actualment és una funció poc utilitzada. |
| 3 | **Administradors** | Llista d'usuaris (separats per comes) amb privilegis per gestionar el grup. Poden afegir o expulsar membres i canviar la contrasenya del grup sense necessitat de ser *root*. |
| 4 | **Membres** | Llista d'usuaris regulars que pertanyen al grup. Aquesta informació s'ha de mantenir sincronitzada amb el fitxer `/etc/group`. |

## Comandes bàsiques

### Adduser

![alt text](<Gestió d'usuaris i grups i permisos img/3.png>)

### Userdel

Arribats a aquest punt, procedim a donar de baixa el compte d'usuari del sistema.

![alt text](<Gestió d'usuaris i grups i permisos img/4.png>)

Seguidament, procedim a donar d'alta l'usuari mitjançant la comanda **useradd**. Durant aquest procés, definim el seu intèrpret de comandes (shell) personalitzat i finalitzem realitzant les validacions pertinents per confirmar que el compte és operatiu.

![alt text](<Gestió d'usuaris i grups i permisos img/5.png>)

Per establir o actualitzar la clau d'accés de l'usuari, executem l'eina **passwd**. El sistema ens demanarà introduir la nova credencial dues vegades per seguretat.

![alt text](<Gestió d'usuaris i grups i permisos img/6.png>)

Amb aquesta acció aconseguim inhabilitar el compte. Si inspeccionem el fitxer **/etc/shadow**, notarem que el camp de la contrasenya ha estat modificat (habitualment amb un signe **!**) per impedir l'accés.

![alt text](<Gestió d'usuaris i grups i permisos img/7.png>)

En cas de voler revertir el bloqueig i restablir l'accés al compte, farem servir el paràmetre **-U (Unlock)**.

![alt text](<Gestió d'usuaris i grups i permisos img/8.png>)

Per donar d'alta nous grups al sistema, fem servir l'ordre **addgroup**. D'altra banda, si necessitem modificar els atributs d'un grup existent (com el nom o el GID), utilitzarem la utilitat **groupmod**.

![alt text](<Gestió d'usuaris i grups i permisos img/9.png>)

Mitjançant aquest procediment, podem **vincular** els usuaris existents als diferents grups del sistema.

![alt text](<Gestió d'usuaris i grups i permisos img/10.png>)

**Validació del procés: Finalment**, verifiquem que els canvis s'han aplicat correctament al sistema.

![alt text](<Gestió d'usuaris i grups i permisos img/11.png>)

D'altra banda, la comanda **deluser** també ens permet revocar la pertinença d'un usuari a un grup específic, excloent-lo d'aquest sense eliminar el seu compte.

![alt text](<Gestió d'usuaris i grups i permisos img/12.png>)

Per canviar de sessió i operar sota la identitat d'un altre usuari sense tancar el terminal actual, utilitzem l'ordre **su (Switch User)**.

![alt text](<Gestió d'usuaris i grups i permisos img/13.png>)

### Permisos

Per iniciar aquest bloc, procedim a generar el directori palomes i un fitxer anomenat prova. Un cop creats els recursos, en modifiquem l'assignació per canviar-ne el grup propietari.

![alt text](Permisos/1.png)

A la següent captura analitzem l'estructura dels permisos. Podem diferenciar clarament els nivells d'accés assignats al propietari, al grup i a la resta d'usuaris.

![alt text](Permisos/2.png)

Configuració de seguretat a /var/palomes:

1. **Transferència de propietat**: Modifiquem el grup propietari del directori, substituint nick per paloma.

2. **Assignació de membres**: Integrem l'usuari ctre dins del grup paloma perquè hi tingui accés.

3. **Restricció de permisos**: Apliquem la màscara 750 (Total per al propietari, Lectura/Execució per al grup, Cap accés per a altres).

![alt text](Permisos/3.png)

Accedint amb les credencials de l'usuari nick, verifiquem que disposem de privilegis complets. Comprovem que podem llegir, crear i modificar fitxers sense restriccions (permisos rwx).

![alt text](Permisos/4.png)

Per contra, si intentem realitzar la mateixa acció des de la sessió de l'usuari cire, el sistema ens denegarà l'accés, confirmant que aquest usuari no disposa dels permisos d'escriptura necessaris.

![alt text](Permisos/5.png)

Finalment, pel que fa a l'usuari ferran (que pertany a la categoria "Altres"), comprovem que té l'accés completament restringit. No disposa de permís d'execució, per la qual cosa no pot ni tan sols accedir al directori.

![alt text](Permisos/6.png)

Ampliació de l'accés al directori /var/palomes:

- **Incorporació de membres**: Afegim els usuaris ferran i detvy al grup paloma perquè puguin accedir als recursos compartits.

- **Modificació de permisos**: Executem chmod g+w per atorgar permisos d'escriptura al grup. Ara, tots els membres del grup podran crear i modificar fitxers, no només llegir-los.

![alt text](Permisos/7.png)

**Validació funcional dels permisos de grup**:

Hem realitzat proves d'accés amb dos membres del grup paloma per confirmar la nova configuració:

1. **Usuari deivy**: Confirma que disposa de permís d'escriptura al directori, ja que ha pogut accedir-hi i generar un nou arxiu (touch ptl) sense errors.

2. **Usuari ferran**: Verifica la capacitat de llistar i eliminar contingut. És important notar que, en intentar esborrar un fitxer, el sistema demana confirmació; això succeeix perquè el fitxer en si està protegit contra escriptura, però els permisos del directori permeten la seva supressió.

**Conclusió**: La configuració és correcta per a un entorn col·laboratiu. Els membres del grup tenen llibertat per treballar dins la carpeta, tot i les alertes de seguretat en manipular fitxers existents.

![alt text](Permisos/8.png)

**Validació dels privilegis de grup a /var/palomes**:

Hem realitzat proves creuades amb diferents membres del grup paloma per confirmar la configuració:

1. **Usuari deivy**: Confirma que disposa de permisos d'escriptura sobre el directori, ja que ha pogut accedir-hi i generar nous arxius (ex: touch ptl) sense incidències.

2. **Usuari ferran**: Verifica la capacitat de llistar i eliminar contingut.

Nota: En intentar esborrar un fitxer, el sistema sol·licita confirmació. Això succeeix perquè el fitxer té permisos restringits, però l'operació és permesa gràcies als permisos d'escriptura de la carpeta contenidora.

**Conclusió**: La carpeta disposa dels permisos correctes per al treball en grup, permetent la creació i gestió de fitxers a tots els membres.

![alt text](Permisos/9.png)

**Diagnòstic de permisos: L'efecte de l'Sticky Bit**

Durant les proves, hem detectat una limitació en la col·laboració. Tot i que ambdós usuaris (deivy i ferran) tenen accés al directori i poden crear fitxers, **l'usuari Ferran no pot eliminar els arxius creats per Deivy**.

**Causa tècnica**: Analitzant els permisos del directori, observem la marca T al final (drwxrwx--T). Això indica que el Sticky Bit (bit de permanència) està actiu.

**Conseqüència**: Aquest bit afegeix una capa de seguretat que restringeix l'esborrat de fitxers: només el propietari del fitxer (o root) pot eliminar-lo, independentment dels permisos de grup. Això impedeix que els usuaris s'esborrin fitxers entre ells accidentalment.

![alt text](Permisos/10.png)

Finalment, analitzem el permís especial SUID (Set User ID). Per demostrar-ne el potencial (i el risc), hem configurat aquest bit sobre l'executable /bin/bash.

Aquesta configuració permet una escalada de privilegis: com que el propietari de bash és root, quan un usuari estàndard l'executa, el sistema inicia la shell amb els privilegis del propietari en lloc dels de l'usuari. Això atorga efectivament una terminal de root a qualsevol usuari sense necessitat de contrasenya.

## ACL

### Importància de les ACL a Ubuntu

**Motius clau per a la implementació de les ACL**:

1. **Granularitat i Versatilitat**: Trenquen amb la rigidesa del model clàssic (UGO), permetent definir permisos específics per a múltiples usuaris o grups sobre un mateix recurs, sense haver de alterar l'estructura de grups del sistema.

2. **Adaptabilitat a Entorns Complexos**: Són imprescindibles en infraestructures corporatives o servidors compartits, on la gestió bàsica de permisos seria insuficient per cobrir totes les casuístiques d'accés.

3. **Reforç de la Seguretat**: Faciliten l'aplicació estricta del Principi de Mínim Privilegi (PoLP), assegurant que cada usuari tingui exactament els permisos que necessita, ni més ni menys, i millorant la traçabilitat de l'accés.

![alt text](ACL/1.png)

![alt text](ACL/2.png)

![alt text](ACL/3.png)

## Umask

**Definició**: La umask (User Mask) és el mecanisme del sistema que estableix els permisos inicials per defecte. Actua com un filtre restrictiu: determina quins permisos no tindran els nous arxius o directoris en el moment de crear-se.

**Configuració a Ubuntu**: La definició d'aquests valors es troba jerarquitzada:

* **Nivell Global (Sistema)**: Es defineix als fitxers /etc/profile o /etc/bash.bashrc.

* **Nivell Local (Usuari)**: Cada usuari pot sobreescriure-la al seu fitxer personal ~/.bashrc.

**Verificació**: Per consultar el valor actual actiu, executem la comanda umask.

![alt text](<Gestió d'usuaris i grups i permisos img/17.png>)

### Valors per defecte (Ubuntu)

| Tipus de Compte | Umask | Resultat en Fitxers | Resultat en Directoris |
| :--- | :--- | :--- | :--- |
| **Usuari Estàndard** | `0002` | **664** `(rw-rw-r--)`<br>*(Grup amb escriptura)* | **775** `(rwxrwxr-x)` |
| **Root (Admin)** | `0022` | **644** `(rw-r--r--)`<br>*(Grup només lectura)* | **755** `(rwxr-xr-x)` |

![alt text](<Gestió d'usuaris i grups i permisos img/18.png>)

### Funcionament del càlcul

El sistema determina els permisos finals restant la umask als valors base:
* **Permisos Base (Arxius):** `666`
* **Permisos Base (Directoris):** `777`

**Exemple d'aplicació (Umask 002):**
* **Arxiu:** `666` - `002` = **664** `(rw-rw-r--)`
* **Directori:** `777` - `002` = **775** `(rwxrwxr-x)`

* En l'exemple següent, he **modificat temporalment** la umask i he realitzat una prova de creació (carpeta i arxiu) per verificar els nous permisos.

![alt text](<Gestió d'usuaris i grups i permisos img/19.png>)

Per aconseguir la persistència d'aquesta configuració (que s'apliqui sempre), cal editar els paràmetres dins del fitxer login.defs.

![alt text](<Gestió d'usuaris i grups i permisos img/21.png>)

## Gestió avançada

## PAM

## Còpies de seguretat i automatització de tasques
## Quotes d'usuari
