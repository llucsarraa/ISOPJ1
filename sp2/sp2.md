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

<img width="1098" height="330" alt="Captura de pantalla de 2025-11-30 00-58-43" src="https://github.com/user-attachments/assets/50c943aa-2763-425e-8412-8a43bc05a566" />

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

<img width="1195" height="534" alt="Captura de pantalla de 2025-11-30 01-00-06" src="https://github.com/user-attachments/assets/5acb829f-6f8d-42c7-aac2-bd08f9afbbe8" />

Per aïllar la informació relativa a la mida del bloc, canalitzem la sortida cap a grep, indicant-li que mostri només les línies que coincideixin amb el terme "Block".

<img width="1194" height="249" alt="Captura de pantalla de 2025-11-30 01-01-21" src="https://github.com/user-attachments/assets/3b8ded91-c23e-4898-b236-e731f919e2a7" />

Per diagnosticar la fragmentació externa, fem ús de l'eina e4defrag. Aquesta ens generarà un informe indicant si la partició té un nivell de dispersió que requereixi ser desfragmentat.

<img width="810" height="366" alt="Captura de pantalla de 2025-11-30 01-08-25" src="https://github.com/user-attachments/assets/b64b7a4b-1348-420a-b638-7226e6a498e1" />

Si volem aplicar la desfragmentació (en lloc de només simular-la o comprovar-la), haurem d'executar la instrucció ometent el paràmetre -c.

<img width="1043" height="205" alt="Captura de pantalla de 2025-11-30 01-09-52" src="https://github.com/user-attachments/assets/47af31b6-ab40-46f5-ad8d-6e2af4425770" />

### GPARTED

GParted és l'eina gràfica per excel·lència de l'entorn GNOME destinada a la gestió integral de discos, permetent operacions com generar, moure o suprimir particions. Tot i que ofereix una gran flexibilitat per formatar en diversos sistemes de fitxers (com ara NTFS, EXT4 o FAT32), té una limitació important: no permet personalitzar la mida del bloc (cluster size), ja que aquest paràmetre s'assigna automàticament.

#### Via interficie gràfica (GPARTED)

El primer pas requereix iniciar el programa. Un cop dins, cal localitzar el selector d'unitats situat a la part superior dreta de la finestra i assegurar-nos que tenim marcat el disc correcte abans de fer cap operació.

<img width="1057" height="325" alt="Captura de pantalla de 2025-11-30 02-12-45" src="https://github.com/user-attachments/assets/0101a991-ba8a-4b1d-8dea-def23a87345b" />

A continuació, ens dirigim a la barra de menús superior. Cliquem sobre la pestanya "Dispositivo" i, dins del desplegable, seleccionem l'opció "Crear tabla de particiones".

<img width="1057" height="325" alt="Captura de pantalla de 2025-11-30 02-13-08" src="https://github.com/user-attachments/assets/62173879-3f8e-4b10-ab5a-de3c5498fd3a" />

El sistema mostrarà una finestra d'advertència (ja que s'esborraran les dades). Dins d'aquest quadre de diàleg, hem de desplegar la llista de tipus de taula i seleccionar específicament l'opció gpt abans de continuar.

<img width="1057" height="446" alt="Captura de pantalla de 2025-11-30 02-13-21" src="https://github.com/user-attachments/assets/7f7c0eea-34f6-443b-b51a-577e36da121f" />

Amb la taula ja definida, visualitzarem l'espai com a "no assignat". Fem clic amb el botó dret sobre aquesta àrea i seleccionem l'opció per generar una nova partició.

<img width="1057" height="446" alt="Captura de pantalla de 2025-11-30 02-13-42" src="https://github.com/user-attachments/assets/b8a47720-4bda-4881-b65e-79a63bb6745d" />

En aquesta finestra de configuració, hem d'establir el sistema de fitxers com a NTFS. A més, tenim l'oportunitat de definir manualment la capacitat (mida) que volem assignar a aquest nou volum.

<img width="1057" height="518" alt="Captura de pantalla de 2025-11-30 02-14-05" src="https://github.com/user-attachments/assets/2ec15020-5387-4bd9-89ba-6cd8c81fad60" />

Com a pas últim, cal fer efectives totes les operacions pendents. Per fer-ho, premem la icona de validació (el vist o "tick" verd) de la barra d'eines i confirmem l'execució a la finestra emergent que apareix.

<img width="1057" height="448" alt="Captura de pantalla de 2025-11-30 02-14-21" src="https://github.com/user-attachments/assets/7e7cd42b-5149-4a00-972a-c6ae0a9af613" />

#### Via CLI (Command Line Interface)

Per dur a terme aquesta tasca utilitzarem la utilitat **fdisk**. Assumint que en el pas anterior ja hem localitzat el dispositiu objectiu, executem la comanda i reproduïm la seqüència d'accions que s'il·lustra a la imatge adjunta.

<img width="1093" height="665" alt="Captura de pantalla de 2025-11-30 02-25-10" src="https://github.com/user-attachments/assets/3b33f08c-8547-48d5-b0b9-307ca6351b21" />

Seguidament, donem l'ordre per afegir una nova partició al disc.

<img width="1093" height="665" alt="Captura de pantalla de 2025-11-30 02-27-04" src="https://github.com/user-attachments/assets/10ade084-16d9-4243-a5da-922b3f0ca887" />

**Verificació**: En aquest punt, confirmem que el procés de generació s'ha completat amb èxit.

<img width="656" height="249" alt="image" src="https://github.com/user-attachments/assets/ff2f3b22-5f7d-4825-9fea-4ce5bf0cc89d" />

Seguidament, utilitzarem l'ordre **mkfs.ext4** per donar format a la partició. Aprofitarem per establir manualment una mida de bloc personalitzada de 2048 bytes.

<img width="809" height="296" alt="image" src="https://github.com/user-attachments/assets/16993287-64cf-4632-98bb-80779054fdaf" />

Per corroborar que la configuració s'ha aplicat correctament, executem la següent instrucció.

<img width="430" height="71" alt="image" src="https://github.com/user-attachments/assets/7144d1a3-6135-4370-a99a-179e325ecf15" />

Pel que fa a la segona partició, li assignarem el format **NTFS**. Aquest pas és essencial per garantir la plena compatibilitat amb sistemes Windows.

<img width="553" height="95" alt="image" src="https://github.com/user-attachments/assets/9dfb371b-4491-4ef3-8f98-3027365eeb51" />

Com a conclusió, executarem la utilitat gràfica GParted per realitzar una validació visual de l'estat final de les particions.

<img width="766" height="286" alt="image" src="https://github.com/user-attachments/assets/2ded3032-1faf-4a77-b1ba-4dd4c929ae8b" />

### Muntatge

Per iniciar aquesta secció, procedim a generar un nou directori i un fitxer dins de la ubicació **/mnt**.

<img width="306" height="72" alt="image" src="https://github.com/user-attachments/assets/c6d5d733-3efb-4ca4-952d-a8700c2603bb" />

<img width="172" height="28" alt="image" src="https://github.com/user-attachments/assets/73946abd-fbec-4a0c-b9a4-84468d645431" />

Realitzem un muntatge manual de la unitat /dev/sdb1 al directori /mnt/particio1, especificant explícitament el sistema de fitxers ext4. Un cop la partició està accessible, procedim a generar un fitxer al seu interior.

<img width="560" height="137" alt="image" src="https://github.com/user-attachments/assets/97eeb831-ffc4-4ca2-9b03-1281bbd6f02e" />

Cal tenir present que el muntatge manual és volàtil: si reiniciem el sistema, la connexió es perdrà (tot i que les dades segueixen emmagatzemades físicament al disc).

Per solucionar això i aconseguir que el muntatge sigui persistent i automàtic a l'arrencada, és necessari editar l'arxiu de configuració systema: /etc/fstab.

<img width="522" height="193" alt="Captura de pantalla de 2025-11-30 06-23-44" src="https://github.com/user-attachments/assets/34be75fd-3388-41d8-9bab-45a7ef99c0bb" />

Si procedim a reiniciar el sistema, comprovarem que el muntatge es realitza de manera automàtica, confirmant així que la configuració és permanent.

<img width="278" height="46" alt="image" src="https://github.com/user-attachments/assets/9a6eb9cc-660d-4a27-a076-c74f63583746" />

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

<img width="1050" height="775" alt="Captura de pantalla de 2025-11-30 02-36-56" src="https://github.com/user-attachments/assets/20f2f57f-9687-41cb-b4f2-c6c3df9ad449" />

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

<img width="1050" height="775" alt="Captura de pantalla de 2025-11-30 02-37-37" src="https://github.com/user-attachments/assets/e365c06e-5fda-4fd9-bde8-d297957de005" />

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

<img width="1050" height="775" alt="Captura de pantalla de 2025-11-30 02-39-58" src="https://github.com/user-attachments/assets/c7be0420-37e7-48c0-9c7d-d30def3ad589" />

Aquest fitxer defineix els grups existents al sistema i determina quins usuaris hi tenen accés. És essencial per a la gestió de permisos col·lectius.

Cada línia consta de 4 camps separats per dos punts (`:`):

| # | Camp | Descripció i Detalls |
| :--- | :--- | :--- |
| 1 | **Nom del Grup** | L'etiqueta única que identifica el grup al sistema (ex: `sudo`, `docker`). Per convenció, s'escriu en minúscules. |
| 2 | **Contrasenya** | Habitualment conté una `x` (referència a `/etc/gshadow`) o un asterisc. En l'administració moderna de Linux, les contrasenyes de grup estan en desús. |
| 3 | **GID** | Identificador numèric del grup.<br>• **0:** Root.<br>• **1-999:** Grups de sistema.<br>• **1000+:** Grups d'usuari estàndard. |
| 4 | **Membres** | Llista d'usuaris (separats per comes) que pertanyen a aquest grup com a **grup secundari**. <br>*Nota: Si és el grup principal d'un usuari, el seu nom no sol aparèixer en aquesta llista.* |

### Explicació: /etc/gshadow

<img width="1050" height="775" alt="Captura de pantalla de 2025-11-30 02-41-10" src="https://github.com/user-attachments/assets/53030428-9c10-4216-bcf1-06010be7bc1c" />

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

<img width="994" height="676" alt="Captura de pantalla de 2025-11-30 03-00-35" src="https://github.com/user-attachments/assets/c4b5e313-1ced-4b0f-9a1a-1577f3c93f1b" />

### Userdel

Arribats a aquest punt, procedim a donar de baixa el compte d'usuari del sistema.

<img width="1055" height="246" alt="Captura de pantalla de 2025-11-30 03-01-26" src="https://github.com/user-attachments/assets/1bf9475e-d354-4a98-a1e0-61a48dd09cf1" />

Seguidament, procedim a donar d'alta l'usuari mitjançant la comanda **useradd**. Durant aquest procés, definim el seu intèrpret de comandes (shell) personalitzat i finalitzem realitzant les validacions pertinents per confirmar que el compte és operatiu.

<img width="785" height="569" alt="Captura de pantalla de 2025-11-30 03-05-53" src="https://github.com/user-attachments/assets/0a1ff888-8bc1-48c3-8f62-a6d87a72a050" />

Per establir o actualitzar la clau d'accés de l'usuari, executem l'eina **passwd**. El sistema ens demanarà introduir la nova credencial dues vegades per seguretat.

<img width="1416" height="261" alt="Captura de pantalla de 2025-11-30 03-06-35" src="https://github.com/user-attachments/assets/9cbc2736-3849-4f00-9023-271231124aee" />

Amb aquesta acció aconseguim inhabilitar el compte. Si inspeccionem el fitxer **/etc/shadow**, notarem que el camp de la contrasenya ha estat modificat (habitualment amb un signe **!**) per impedir l'accés.

<img width="1416" height="261" alt="Captura de pantalla de 2025-11-30 03-08-08" src="https://github.com/user-attachments/assets/f720971b-0a78-4aa4-bc2b-ef01497a205f" />

En cas de voler revertir el bloqueig i restablir l'accés al compte, farem servir el paràmetre **-U (Unlock)**.

<img width="1416" height="261" alt="Captura de pantalla de 2025-11-30 03-08-54" src="https://github.com/user-attachments/assets/86519a90-1159-487b-9b44-c82666fe5844" />

Per donar d'alta nous grups al sistema, fem servir l'ordre **addgroup**. D'altra banda, si necessitem modificar els atributs d'un grup existent (com el nom o el GID), utilitzarem la utilitat **groupmod**.

<img width="1416" height="346" alt="Captura de pantalla de 2025-11-30 03-11-05" src="https://github.com/user-attachments/assets/09c2f0b0-95ce-46a2-80db-76bfcd8407d7" />

Mitjançant aquest procediment, podem **vincular** els usuaris existents als diferents grups del sistema.

<img width="870" height="413" alt="Captura de pantalla de 2025-11-30 03-13-00" src="https://github.com/user-attachments/assets/f2176e2a-5177-482d-99f1-a7e75f984806" />

**Validació del procés**: Finalment, verifiquem que els canvis s'han aplicat correctament al sistema.

<img width="870" height="238" alt="Captura de pantalla de 2025-11-30 03-16-03" src="https://github.com/user-attachments/assets/41f79f7e-13a5-4114-98cd-c601a2b2d961" />

D'altra banda, la comanda **deluser** també ens permet revocar la pertinença d'un usuari a un grup específic, excloent-lo d'aquest sense eliminar el seu compte.

<img width="870" height="238" alt="Captura de pantalla de 2025-11-30 03-16-29" src="https://github.com/user-attachments/assets/dacb10f5-0e54-428d-82b7-4d48094e3cf7" />

Per canviar de sessió i operar sota la identitat d'un altre usuari sense tancar el terminal actual, utilitzem l'ordre **su (Switch User)**.

<img width="870" height="238" alt="Captura de pantalla de 2025-11-30 03-17-04" src="https://github.com/user-attachments/assets/702399ce-de55-40c4-8bfb-a8ad2535c984" />

### Permisos

Per iniciar aquest bloc, procedim a generar el directori palomes i un fitxer anomenat prova. Un cop creats els recursos, en modifiquem l'assignació per canviar-ne el grup propietari.

<img width="846" height="369" alt="Captura de pantalla de 2025-11-30 04-53-35" src="https://github.com/user-attachments/assets/581593b3-4c29-46c0-84d5-dbe9cb1bbcc4" />

A la següent captura analitzem l'estructura dels permisos. Podem diferenciar clarament els nivells d'accés assignats al propietari, al grup i a la resta d'usuaris.

<img width="846" height="441" alt="Captura de pantalla de 2025-11-30 04-57-18" src="https://github.com/user-attachments/assets/730dd2c6-314c-4f59-9594-bd4cc5af99b4" />

Configuració de seguretat a /var/palomes:

1. **Transferència de propietat**: Modifiquem el grup propietari del directori, substituint nick per paloma.

2. **Assignació de membres**: Integrem l'usuari ctre dins del grup paloma perquè hi tingui accés.

3. **Restricció de permisos**: Apliquem la màscara 750 (Total per al propietari, Lectura/Execució per al grup, Cap accés per a altres).

<img width="846" height="441" alt="Captura de pantalla de 2025-11-30 04-57-18" src="https://github.com/user-attachments/assets/796c5f9d-4090-4dcb-b957-10dc5a0c095d" />

Accedint amb les credencials de l'usuari nick, verifiquem que disposem de privilegis complets. Comprovem que podem llegir, crear i modificar fitxers sense restriccions (permisos rwx).

<img width="621" height="202" alt="Captura de pantalla de 2025-11-30 05-09-59" src="https://github.com/user-attachments/assets/f1a6fc21-6d3f-49fd-8411-fe81618fb86d" />

Per contra, si intentem realitzar la mateixa acció des de la sessió de l'usuari cire, el sistema ens denegarà l'accés, confirmant que aquest usuari no disposa dels permisos d'escriptura necessaris.

<img width="1002" height="285" alt="Captura de pantalla de 2025-11-30 05-13-14" src="https://github.com/user-attachments/assets/586e96e5-57bc-4643-90c2-b30b06850b17" />

Finalment, pel que fa a l'usuari ferran (que pertany a la categoria "Altres"), comprovem que té l'accés completament restringit. No disposa de permís d'execució, per la qual cosa no pot ni tan sols accedir al directori.

<img width="1002" height="194" alt="Captura de pantalla de 2025-11-30 05-15-10" src="https://github.com/user-attachments/assets/af1c6d67-2bd1-4f62-97fd-58e2adc4e192" />

Ampliació de l'accés al directori /var/palomes:

- **Incorporació de membres**: Afegim els usuaris ferran i detvy al grup paloma perquè puguin accedir als recursos compartits.

- **Modificació de permisos**: Executem chmod g+w per atorgar permisos d'escriptura al grup. Ara, tots els membres del grup podran crear i modificar fitxers, no només llegir-los.

<img width="1002" height="280" alt="Captura de pantalla de 2025-11-30 05-17-51" src="https://github.com/user-attachments/assets/37a1d730-df6f-4229-bd94-b2425f474152" />

**Validació funcional dels permisos de grup**:

Hem realitzat proves d'accés amb dos membres del grup paloma per confirmar la nova configuració:

1. **Usuari deivy**: Confirma que disposa de permís d'escriptura al directori, ja que ha pogut accedir-hi i generar un nou arxiu (touch ptl) sense errors.

2. **Usuari ferran**: Verifica la capacitat de llistar i eliminar contingut. És important notar que, en intentar esborrar un fitxer, el sistema demana confirmació; això succeeix perquè el fitxer en si està protegit contra escriptura, però els permisos del directori permeten la seva supressió.

**Conclusió**: La configuració és correcta per a un entorn col·laboratiu. Els membres del grup tenen llibertat per treballar dins la carpeta, tot i les alertes de seguretat en manipular fitxers existents.

<img width="1002" height="364" alt="Captura de pantalla de 2025-11-30 05-20-46" src="https://github.com/user-attachments/assets/19ebd78d-3182-4d12-8aff-83bb7fb438f4" />

**Validació dels privilegis de grup a /var/palomes**:

Hem realitzat proves creuades amb diferents membres del grup paloma per confirmar la configuració:

1. **Usuari deivy**: Confirma que disposa de permisos d'escriptura sobre el directori, ja que ha pogut accedir-hi i generar nous arxius (ex: touch ptl) sense incidències.

2. **Usuari ferran**: Verifica la capacitat de llistar i eliminar contingut.

Nota: En intentar esborrar un fitxer, el sistema sol·licita confirmació. Això succeeix perquè el fitxer té permisos restringits, però l'operació és permesa gràcies als permisos d'escriptura de la carpeta contenidora.

**Conclusió**: La carpeta disposa dels permisos correctes per al treball en grup, permetent la creació i gestió de fitxers a tots els membres.

<img width="1011" height="518" alt="Captura de pantalla de 2025-11-30 05-34-42" src="https://github.com/user-attachments/assets/70a62499-acc4-422b-9a5b-0f4c39303042" />

**Diagnòstic de permisos: L'efecte de l'Sticky Bit**

Durant les proves, hem detectat una limitació en la col·laboració. Tot i que ambdós usuaris (deivy i ferran) tenen accés al directori i poden crear fitxers, **l'usuari Ferran no pot eliminar els arxius creats per Deivy**.

**Causa tècnica**: Analitzant els permisos del directori, observem la marca **T** al final (drwxrwx--T). Això indica que el Sticky Bit (bit de permanència) està actiu.

**Conseqüència**: Aquest bit afegeix una capa de seguretat que restringeix l'esborrat de fitxers: només el propietari del fitxer (o root) pot eliminar-lo, independentment dels permisos de grup. Això impedeix que els usuaris s'esborrin fitxers entre ells accidentalment.

<img width="784" height="119" alt="Captura de pantalla de 2025-11-30 05-30-26" src="https://github.com/user-attachments/assets/1ffbd164-3ccc-4242-bb25-403bc16af04d" />

<img width="714" height="149" alt="Captura de pantalla de 2025-11-30 05-35-53" src="https://github.com/user-attachments/assets/363e859d-0ffa-48b4-ac72-47576e8558f7" />

Finalment, analitzem el permís especial SUID (Set User ID). Per demostrar-ne el potencial (i el risc), hem configurat aquest bit sobre l'executable /bin/bash.

Aquesta configuració permet una escalada de privilegis: com que el propietari de bash és root, quan un usuari estàndard l'executa, el sistema inicia la shell amb els privilegis del propietari en lloc dels de l'usuari. Això atorga efectivament una terminal de root a qualsevol usuari sense necessitat de contrasenya.

## ACL

### Importància de les ACL a Ubuntu

**Motius clau per a la implementació de les ACL**:

1. **Granularitat i Versatilitat**: Trenquen amb la rigidesa del model clàssic (UGO), permetent definir permisos específics per a múltiples usuaris o grups sobre un mateix recurs, sense haver de alterar l'estructura de grups del sistema.

2. **Adaptabilitat a Entorns Complexos**: Són imprescindibles en infraestructures corporatives o servidors compartits, on la gestió bàsica de permisos seria insuficient per cobrir totes les casuístiques d'accés.

3. **Reforç de la Seguretat**: Faciliten l'aplicació estricta del Principi de Mínim Privilegi (PoLP), assegurant que cada usuari tingui exactament els permisos que necessita, ni més ni menys, i millorant la traçabilitat de l'accés.

<img width="798" height="76" alt="Captura de pantalla de 2025-11-30 05-57-04" src="https://github.com/user-attachments/assets/7f094e24-e6f6-4a74-8133-641ccc35c71b" />
<img width="355" height="193" alt="Captura de pantalla de 2025-11-30 05-57-43" src="https://github.com/user-attachments/assets/e52275ca-dc78-41b7-92cc-c6418c5dfb75" />

<img width="601" height="32" alt="Captura de pantalla de 2025-11-30 05-58-00" src="https://github.com/user-attachments/assets/393732ff-6011-4d6a-9982-5d9e4e06a782" />
<img width="420" height="134" alt="Captura de pantalla de 2025-11-30 05-58-18" src="https://github.com/user-attachments/assets/3dee477b-85f1-48a9-a432-9b96bcea2616" />
<img width="591" height="26" alt="image" src="https://github.com/user-attachments/assets/f7133618-185e-4d20-a012-f948ab977d2f" />
<img width="749" height="134" alt="Captura de pantalla de 2025-11-30 05-58-48" src="https://github.com/user-attachments/assets/b1929259-65c7-4d79-b7c4-04b03fb2d26b" />

<img width="758" height="242" alt="Captura de pantalla de 2025-11-30 05-59-30" src="https://github.com/user-attachments/assets/669c8e50-27df-4279-820a-aa0e73ef5674" />

## Umask

**Definició**: La umask (User Mask) és el mecanisme del sistema que estableix els permisos inicials per defecte. Actua com un filtre restrictiu: determina quins permisos no tindran els nous arxius o directoris en el moment de crear-se.

**Configuració a Ubuntu**: La definició d'aquests valors es troba jerarquitzada:

* **Nivell Global (Sistema)**: Es defineix als fitxers /etc/profile o /etc/bash.bashrc.

* **Nivell Local (Usuari)**: Cada usuari pot sobreescriure-la al seu fitxer personal ~/.bashrc.

**Verificació**: Per consultar el valor actual actiu, executem la comanda umask.

<img width="686" height="108" alt="Captura de pantalla de 2025-11-30 06-01-19" src="https://github.com/user-attachments/assets/88efcc1d-6e50-41fe-bc53-4b6dab0a0e34" />

### Valors per defecte (Ubuntu)

| Tipus de Compte | Umask | Resultat en Fitxers | Resultat en Directoris |
| :--- | :--- | :--- | :--- |
| **Usuari Estàndard** | `0002` | **664** `(rw-rw-r--)`<br>*(Grup amb escriptura)* | **775** `(rwxrwxr-x)` |
| **Root (Admin)** | `0022` | **644** `(rw-r--r--)`<br>*(Grup només lectura)* | **755** `(rwxr-xr-x)` |

<img width="686" height="108" alt="Captura de pantalla de 2025-11-30 06-00-49" src="https://github.com/user-attachments/assets/abef3296-c8ef-4045-b6fb-2438c8b4494d" />

### Funcionament del càlcul

El sistema determina els permisos finals restant la umask als valors base:
* **Permisos Base (Arxius):** `666`
* **Permisos Base (Directoris):** `777`

**Exemple d'aplicació (Umask 002):**
* **Arxiu:** `666` - `002` = **664** `(rw-rw-r--)`
* **Directori:** `777` - `002` = **775** `(rwxrwxr-x)`

* En l'exemple següent, he **modificat temporalment** la umask i he realitzat una prova de creació (carpeta i arxiu) per verificar els nous permisos.

<img width="791" height="253" alt="Captura de pantalla de 2025-11-30 06-03-04" src="https://github.com/user-attachments/assets/054fd425-b40d-4f21-83b0-1a6d55d7e879" />

Per aconseguir la persistència d'aquesta configuració (que s'apliqui sempre), cal editar els paràmetres dins del fitxer login.defs.

<img width="1011" height="515" alt="Captura de pantalla de 2025-11-30 06-03-52" src="https://github.com/user-attachments/assets/34c23387-598e-4525-a72c-6af9184a402e" />

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

**top / htop / btop**

Serveixen per visualitzar els processos actius al moment. A diferència del top clàssic, l'eina btop ofereix un entorn molt més visual, organitzat i fàcil d'utilitzar.

top

<img width="915" height="728" alt="image" src="https://github.com/user-attachments/assets/ed573f25-7816-4cd2-8e52-dbef221d70e0" />

htop

<img width="915" height="728" alt="image" src="https://github.com/user-attachments/assets/30bf0ca0-2252-47e4-bbea-f769ad9a39fb" />

btop

<img width="1189" height="730" alt="image" src="https://github.com/user-attachments/assets/673554c7-33b9-40e3-8f40-14c2fbad1474" />

### Gestió de Prioritats (PR i NI)

En el monitoratge de processos, la prioritat es defineix mitjançant dos valors clau:

* **PR (Priority):** Prioritat real gestionada pel Kernel. L'usuari no la modifica directament.
* **NI (Nice):** Valor de "cortesia" modificable per l'usuari (-20 a +19). El sistema calcula la PR basant-se en aquest valor.

**Escala de valors NI**
Funciona a la inversa: quan més baix és el número, més prioritat té el procés.

* **-20:** Màxima prioritat (Acapara la CPU).
* **0:** Valor estàndard.
* **+19:** Mínima prioritat (Cedeix recursos a la resta).

**Comandes de gestió**

* `nice -n [valor] [comanda]`: Executar un programa nou amb una prioritat específica.
* `renice -n [valor] -p [PID]`: Canviar la prioritat d'un procés ja actiu.

<img width="918" height="780" alt="image" src="https://github.com/user-attachments/assets/0e11c3b3-4dc1-4c97-b10a-a1e7810feba2" />

### Visualització jeràrquica (pstree)

Aquesta comanda organitza els processos actius en format d'arbre genealògic. És l'eina ideal per entendre les relacions "pare-fill" i identificar ràpidament quin procés n'ha executat un altre.

**Paràmetres principals:**

* `pstree -p`: Afegeix el **PID** al costat del nom de cada branca.
* `pstree -u`: Indica l'**usuari** (propietari) de cada procés.
* `pstree -h`: Ressalta en negreta el procés des d'on s'està executant la comanda (el procés actual).

<img width="781" height="1003" alt="Captura de pantalla de 2025-12-02 13-10-01" src="https://github.com/user-attachments/assets/d1425eb8-30f5-4b39-b126-0aa38031fd77" />

**Permisos d'administrador:** Si s'executa la comanda amb `sudo` o com a usuari **root**, es mostraran també els processos del sistema que normalment estan ocults per als usuaris estàndard.

<img width="844" height="690" alt="image" src="https://github.com/user-attachments/assets/2d7b4251-ee2e-4356-ad3d-7a590479898c" />

**Filtratge de resultats:**
Per a cerques específiques (un usuari o programa concret), podem canalitzar la sortida utilitzant `grep`.

* **Sintaxi:** `pstree | grep <nom_a_buscar>`
* **Exemple:** `pstree -p | grep firefox` (Mostraria només la branca del Firefox).

<img width="905" height="188" alt="image" src="https://github.com/user-attachments/assets/00fb2980-3d09-408c-85f2-95155406ef74" />

### Instantània de processos (ps)

La comanda `ps` genera una "fotografia" fixa de l'estat actual del sistema. És essencial per analitzar detalladament el consum de recursos.

La combinació més utilitzada és **`ps aux`**, que uneix tres paràmetres per donar la visió global:

* **a (All):** Visualitza els processos de tots els usuaris (sempre que tinguin un terminal/TTY).
* **u (User-oriented):** Afegeix columnes vitals com `%CPU`, `%MEM` i l'hora d'inici (`START`).
* **x:** Fonamental per veure processos de fons ("daemons" o serveis) que no estan lligats a cap terminal.

<img width="805" height="535" alt="Captura de pantalla de 2025-12-02 13-13-52" src="https://github.com/user-attachments/assets/70cf6861-d8cb-4a0c-9d5d-ec8fbc57ef70" />

### Interpretació de les dades (Columnes clau)

Quan analitzem la sortida de `ps aux`, les dades s'organitzen en dos grups principals:

**Identificació i Estat**
* **USER / PID:** Qui executa el procés i el seu identificador únic (DNI del procés).
* **COMMAND:** L'ordre exacta que s'ha llançat.
* **TTY:** El terminal on s'executa (`?` significa que és un servei de fons).
* **STAT:** L'estat actual (veure taula de codis més avall).
* **START / TIME:** Quan va començar i quants minuts de processador ha consumit en total.

**Consum de Recursos**
| Columna | Significat | Diferència clau |
| :--- | :--- | :--- |
| **%CPU / %MEM** | Càrrega actual | Ús instantani del processador i la RAM. |
| **VSZ** (Virtual) | Memòria assignada | Tot el que el programa *podria* arribar a usar (inclou llibreries compartides). |
| **RSS** (Resident) | Memòria real | La RAM física que està ocupant *ara mateix*. Aquesta és la xifra important. |

### Codis d'Estat (STAT)

Indiquen què està fent el procés en aquell precís moment:

* **R (Running):** Actiu. Està treballant a la CPU o fent cua per entrar-hi.
* **S (Sleeping):** Dormint. Espera que passi alguna cosa (un clic, rebre dades, etc.). És l'estat més comú.
* **D (Disk Sleep):** Bloquejat per maquinari. Està esperant una lectura/escriptura de disc crítica. No es pot interrompre.
* **Z (Zombie):** Procés mort. Ha acabat, però el seu "pare" encara no ha estat notificat. No consumeix recursos, només un PID.
* **T (Stopped):** Pausat manualment (congelat).

### Gestió de Senyals (Kill)

La comanda `kill` no només serveix per tancar programes, sinó per comunicar-s'hi enviant senyals al Kernel (PID).

**Per finalitzar processos:**
* **SIGTERM (15):** *La forma educada.* Demana al programa que tanqui i guardi els canvis. És l'opció per defecte.
* **SIGINT (2):** *Interrupció.* El mateix que fer `Ctrl+C`. Talla l'execució des del terminal.
* **SIGKILL (9):** *La forma dràstica.* El Kernel elimina el procés immediatament. No guarda res. Útil quan el sistema no respon (el programa "s'ha penjat").

**Per controlar l'execució:**
* **SIGHUP (1):** *Reload.* Reinicia la configuració sense aturar el servei (molt usat en servidors web).
* **SIGSTOP (19) / SIGCONT (18):** *Pause & Play.* El 19 congela el procés (com un `Ctrl+Z` forçat) i el 18 el reprèn on estava.

### Dreceres de teclat (Interactivitat)

Quan treballem en terminal amb eines com `top`, és vital distingir entre tancar i pausar:

> **Ctrl + C (SIGINT)**
> * **Acció:** Tanca el programa definitivament.
> * **Resultat:** Allibera la memòria i torna al terminal.

> **Ctrl + Z (SIGSTOP)**
> * **Acció:** Posa el programa en "Pausa" (Background).
> * **Atenció:** El programa NO s'ha tancat. Continua ocupant memòria RAM però està congelat (Estat T). Cal recuperar-lo (`fg`) o matar-lo després.

<img width="328" height="37" alt="image" src="https://github.com/user-attachments/assets/bbebd846-397d-42ed-81fb-91fe76e28582" />

### Execució en segon pla (&)

Si volem llançar un programa sense que ens bloquegi la terminal (per poder seguir escrivint comandes), afegim el símbol `&` al final de la línia.

* **Sintaxi:** `comanda &`
* **Exemple:** `firefox &`
* **Resultat:** El programa s'obre, el sistema ens mostra el PID assignat i recuperem el control de la consola immediatament.

<img width="360" height="37" alt="image" src="https://github.com/user-attachments/assets/e226275c-ad46-40c5-ad62-60dcecf7329e" />

### Recuperació de processos (Jobs i fg)

Si hem enviat un procés al segon pla (o està aturat amb `Ctrl+Z`), per recuperar-lo seguim dos passos:

1.  **Llistar:** Executem `jobs` per veure la llista de tasques i el seu número d'identificació (Job ID), que apareix entre claudàtors, per exemple `[1]`.
2.  **Recuperar:** Utilitzem la comanda `fg` (foreground) seguida del número.

**Exemple pràctic:**
* `jobs` -> Ens mostra: `[1]+ Stopped vim text.txt`
* `fg %1` -> Recupera l'editor de text al primer pla.

<img width="363" height="77" alt="image" src="https://github.com/user-attachments/assets/29b66155-496c-4072-bbab-b30b13089429" />

**Retorn al primer pla (fg):**
Per recuperar el control d'un programa aturat (com el `top` que hem pausat abans), utilitzem la comanda `fg`. Això restaura la finestra immediatament on la vam deixar.

### Mètodes de finalització

**Sortida neta (Tecla `q`)**
Dins de la interfície de `top`, prémer **`q`** és el mètode correcte per finalitzar la sessió. Això tanca el programa ordenadament sense generar errors.

**Sortida forçosa (`kill -9`)**
Si un procés no respon, l'opció `-9` envia el senyal **SIGKILL**.
* **Funció:** És una ordre directa i innegociable al Kernel.
* **Efecte:** El procés s'elimina immediatament de la memòria sense tenir temps de guardar dades ni tancar fitxers. S'ha d'usar amb precaució.

**Cas Pràctic: VirtualBox**
Per gestionar una aplicació pesada com VirtualBox:
1. Obrim `top` i busquem la fila corresponent a *VirtualBox*.
2. Anotem el seu número **PID** (primera columna).
3. Si es bloquegés, podríem tancar-lo des d'un altre terminal amb: `kill -9 <PID_trobat>`.

<img width="1029" height="695" alt="image" src="https://github.com/user-attachments/assets/a5d77d7d-59f5-4bf8-9d7a-8dbab160dab6" />

<img width="348" height="31" alt="image" src="https://github.com/user-attachments/assets/6b221cb0-b036-4b03-b691-69b2f39f90a7" />

Com es pot veure a la imatge, el procés objectiu ja no figura a la taula d'activitat, indicant que el sistema ha forçat el seu tancament amb èxit.

<img width="751" height="186" alt="image" src="https://github.com/user-attachments/assets/e676f109-ad2f-4c31-a362-6d2b4189ac83" />

## Còpies de seguretat i automatització de tasques

### Còpies de Seguretat

#### Objectiu i Amenaces
La missió principal d'un backup no és guardar dades, sinó **garantir-ne la recuperació**. Ens protegeixen davant quatre vectors de risc:

* **Falles físiques:** Trencament de discos o hardware.
* **Errors lògics:** Esborrat accidental humà.
* **Amenaces externes:** Ransomware, virus i atacs.
* **Desastres:** Incendis o robatoris.

### Estratègies de Còpia (Comparativa)
No existeix la còpia perfecta; triem segons l'equilibri entre espai ocupat i temps de recuperació.

| Tipus | Què es copia? | Avantatges (+) | Inconvenients (-) |
| :--- | :--- | :--- | :--- |
| **COMPLETA** (Full) | Tot (100% de les dades). | Recuperació ràpida i senzilla (només cal 1 fitxer). | Molt lenta de generar i ocupa molt disc. |
| **INCREMENTAL** | Només el que ha canviat des de l'*últim backup* (sigui quin sigui). | Molt ràpida i lleugera (ocupa poc). | Recuperació lenta (cal reconstruir la cadena sencera). |
| **DIFERENCIAL** | Tot el que ha canviat des de l'última *Completa*. | Punt mig: recuperació més àgil que la incremental. | Duplica dades i ocupa més espai progressivament. |

### Eines de Còpia (Teoria de Comandes)

A Linux treballem a dos nivells diferents:

**A. Nivell de Fitxer (`cp` i `rsync`)**

* **`cp` (Còpia Estàndard):**
    * *Mecànica:* Llegeix l'origen i sobreescriu el destí cegament.
    * *Problema:* No verifica si el fitxer ja existeix o és idèntic, malgastant recursos.

* **`rsync` (Sincronització Intel·ligent):**
    * *Mecànica:* Utilitza l'algoritme *Delta Encoding*. Compara origen i destí.
    * *Avantatge:* Només transfereix les parts modificades (blocs nous), ideal per xarxes i backups eficients.

**B. Nivell de Bloc / Clonatge (`dd`)**

* **`dd` (Disk Dump):**
    * *Mecànica:* Còpia "bit a bit" sense entendre de fitxers.
    * *Detall important:* Si clonem un disc de 500GB amb només 1GB de dades, **copiarà els 500GB sencers** (incloent l'espai buit). És una imatge exacta del dispositiu físic.

### 4. Preparació de l'entorn (Pràctica)

Per realitzar les proves, configurem la màquina virtual amb el següent maquinari:

1.  Afegir **2 Discos Durs** de 1 GB cadascun (Virtuals).
2.  Iniciar el sistema i identificar els nous dispositius amb:
    `fdisk -l`

<img width="745" height="260" alt="Captura de pantalla de 2025-12-18 13-19-55" src="https://github.com/user-attachments/assets/6c7c8591-8372-4c67-b1f9-494bc0fb1bb8" />

**Configuració de volums:**
Per utilitzar els nous discs, primer generem una taula de particions on assignem tot l'espai disponible a una sola partició primària per disc.
Seguidament, mitjançant la comanda `mkfs.ext4`, donem format als nous volums per fer-los compatibles amb el sistema de fitxers de Linux.

<img width="499" height="38" alt="Captura de pantalla de 2025-12-18 13-22-58" src="https://github.com/user-attachments/assets/d0220b3e-cc6c-4782-a36a-ca39d8fa0f2c" />

<img width="786" height="542" alt="Captura de pantalla de 2025-12-18 13-23-30" src="https://github.com/user-attachments/assets/90027348-b310-471b-bf19-bac626d591e3" />

**Generació de dades de test**

Per verificar el funcionament de les còpies, necessitem dades d'origen. Creem una estructura simple:

1.  Generem el directori: `mkdir prova`
2.  Creem un fitxer de text a l'interior: `touch prova/prova2`

<img width="566" height="98" alt="Captura de pantalla de 2025-12-18 13-25-24" src="https://github.com/user-attachments/assets/54dcac52-c3a2-4c6e-9645-b2fba7889f28" />




## Quotes d'usuari
