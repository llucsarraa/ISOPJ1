---
layout: default
title: "Sprint 3: IAdministració de Dominis i Seguretat"
---

# Configuració Servidor - Client

El primer pas consisteix a preparar l'entorn amb dues instàncies d'Ubuntu, assignant-ne una al rol de servidor i la restant al de client. Posteriorment, s'haurà d'establir una xarxa NAT comuna per tal que ambdós sistemes estiguin correctament interconnectats.

<img width="1121" height="171" alt="2026-01-14_20-47" src="https://github.com/user-attachments/assets/4cabaf15-0260-4802-bed5-2fc28dde4fc4" />

<img width="976" height="600" alt="2026-01-14_20-48" src="https://github.com/user-attachments/assets/0945ed07-9d86-440a-901c-6761acae35ea" />

<img width="929" height="593" alt="2026-01-14_20-48_1" src="https://github.com/user-attachments/assets/d9ed9cc8-10f5-4590-abc2-9db6a329e733" />

## Servidor

Un cop el sistema estigui operatiu, el següent pas és configurar una adreça IP estàtica de manera permanent.

<img width="574" height="471" alt="2026-01-14_21-00" src="https://github.com/user-attachments/assets/b60386cd-44db-4716-86bd-362beab4034b" />

Seguidament, executarem l'ordre ping per verificar que hi hagi connectivitat entre ambdós equips.

<img width="590" height="193" alt="2026-01-14_21-01" src="https://github.com/user-attachments/assets/852d7122-27cb-441b-ac25-cd225dad80c2" />

Després d'establir l'adreça IP fixa, procedirem a modificar el nom de l'host (hostname) per identificar l'equip a la xarxa.

<img width="737" height="93" alt="2026-01-14_21-06" src="https://github.com/user-attachments/assets/e8a84016-b116-4a86-b03c-5a522a02e98e" />

A continuació, cal editar el fitxer /etc/hosts. En primer lloc, substituirem el nom antic vinculat a l'adreça de retorn (loopback) pel nou hostname configurat. Tot seguit, afegirem una entrada addicional que vinculi la IP estàtica amb el nom complet del domini (FQDN) i el nom curt de la màquina, seguint l'estructura que es mostra a la imatge.

<img width="732" height="295" alt="2026-01-14_21-08" src="https://github.com/user-attachments/assets/bade8ac0-20ea-4e89-8ffe-0b697883317e" />

### Instal·lació LDAP

Després de validar les modificacions en els fitxers de configuració, procedirem a actualitzar els repositoris mitjançant l'ordre apt update. Aquest pas és indispensable per garantir la descàrrega i instal·lació correcta del següent paquet.

<img width="630" height="70" alt="2026-01-14_21-23" src="https://github.com/user-attachments/assets/448bac53-aa70-445e-b1d9-f86f36bfcfd0" />

Durant el procés d'instal·lació, serà necessari establir una contrasenya de seguretat. Un cop el servei estigui operatiu al servidor, abans d'aplicar qualsevol modificació, executarem l'ordre slapcat per obtenir una visió detallada de la configuració base del sistema.

<img width="472" height="287" alt="2026-01-14_21-23_1" src="https://github.com/user-attachments/assets/039bf1e2-4166-445f-91db-b50ee876fa7f" />

#### Configuració LDAP

Mitjançant l'execució de dpkg-reconfigure, podrem personalitzar els paràmetres de l'LDAP segons les nostres necessitats específiques. A continuació, es detallen els valors que aplicarem per a aquesta configuració:

<img width="713" height="485" alt="2026-01-14_21-26" src="https://github.com/user-attachments/assets/a3b37709-ccdb-42c5-b83e-00a39946450f" />

<img width="722" height="477" alt="2026-01-14_21-27" src="https://github.com/user-attachments/assets/f5b1f17c-226f-4536-a080-412dcc052e32" />

<img width="723" height="483" alt="2026-01-14_21-27_1" src="https://github.com/user-attachments/assets/4a7abeec-77ff-4947-a401-258d3635b0c2" />

<img width="727" height="480" alt="2026-01-14_21-27_2" src="https://github.com/user-attachments/assets/eeeafb50-318c-4e0c-a42e-a7cc39ee8bf7" />

<img width="725" height="484" alt="2026-01-14_21-28" src="https://github.com/user-attachments/assets/e4133c36-835f-4cd8-8775-96506c5e3216" />

<img width="729" height="274" alt="2026-01-14_21-29" src="https://github.com/user-attachments/assets/36e23020-b15f-4f97-ae83-1fbe4dd16cfd" />

Amb el servei base ja establert segons els nostres requeriments, procedirem a incorporar l'estructura d'Unitats Organitzatives (UO), usuaris i grups. Aquests elements es troben emmagatzemats a l'arxiu arxius.zip, de manera que el primer pas serà descomprimir-ne el contingut.

<img width="653" height="122" alt="2026-01-14_21-44" src="https://github.com/user-attachments/assets/7c4616bf-ccb5-4411-a78c-7ade736c64db" />

<img width="560" height="165" alt="2026-01-14_21-45" src="https://github.com/user-attachments/assets/43afb338-fb3a-4b58-a612-52646b9f1a5f" />

Cal adaptar els fitxers uo.ldif, usu.ldif i grup.ldif a la nostra infraestructura. Per fer-ho, editarem cadascun d'aquests arxius per assegurar-nos que les referències al domini coincideixen amb el que hem configurat prèviament.

<img width="731" height="280" alt="2026-01-14_21-47" src="https://github.com/user-attachments/assets/c3eb3218-0dfc-4e3d-a8f0-3aba9c5efcb5" />

<img width="733" height="446" alt="2026-01-14_21-48" src="https://github.com/user-attachments/assets/4605d459-d5eb-41d4-b474-02afba6d5cac" />

<img width="737" height="288" alt="2026-01-14_21-49" src="https://github.com/user-attachments/assets/5276638b-f919-45cf-8cfc-386e8fb3b8ff" />

Un cop editats els fitxers, procedirem a la seva importació al directori LDAP. Per fer-ho, utilitzarem l'eina de línia de comandes per carregar les estructures definides en els arxius LDIF.

<img width="924" height="150" alt="2026-01-14_21-52" src="https://github.com/user-attachments/assets/3fb46b86-af71-4fc4-8da9-b567e46d99e9" />

<img width="920" height="64" alt="2026-01-14_21-53" src="https://github.com/user-attachments/assets/034d857e-8f95-4d66-9587-6db8a424e44c" />

<img width="915" height="68" alt="2026-01-14_21-54" src="https://github.com/user-attachments/assets/a9cdd79d-63b6-4504-b185-a0b48bc10822" />

Per finalitzar, executarem novament l'ordre slapcat amb l'objectiu de verificar que tota l'estructura d'usuaris i grups s'ha integrat correctament en la base de dades del directori.

<img width="789" height="448" alt="2026-01-14_21-55" src="https://github.com/user-attachments/assets/2342fb89-5b2f-4a29-a4b3-d6798a9010c4" />

Amb aquests passos, la configuració de l'entorn del servidor ha quedat finalitzada i optimitzada segons els requeriments establerts.

## Client

De la mateixa manera que hem fet amb el servidor, caldrà establir una adreça IP estàtica a la màquina client. Posteriorment, validarem la connectivitat entre ambdós equips mitjançant una prova de ping per assegurar-nos que la comunicació és estable.

<img width="589" height="480" alt="2026-01-14_21-59" src="https://github.com/user-attachments/assets/ce289c1f-5f8f-46cd-985f-0b948fddd790" />

<img width="734" height="356" alt="2026-01-14_22-00" src="https://github.com/user-attachments/assets/4c7abfc7-afb1-4bfd-979b-c2ab8e3eca59" />

<img width="580" height="229" alt="2026-01-14_22-01" src="https://github.com/user-attachments/assets/55948741-ed35-4669-b8a2-535ca7664631" />

### Instal·lació LDAP

En el client també és necessari instal·lar paquets relacionats amb l'LDAP, tot i que el programari difereix de l'utilitzat al servidor, ja que en aquest cas l'objectiu és l'autenticació i no la gestió del directori.

<img width="726" height="269" alt="2026-01-14_22-04" src="https://github.com/user-attachments/assets/514e515e-e685-4b92-9f24-a1a0161bbaf2" />

### Configuració LDAP

Un cop finalitzada la instal·lació, s'executarà automàticament l'assistent de configuració del client LDAP. Els paràmetres que hem definit per a aquest entorn es detallen a continuació:

<img width="734" height="480" alt="2026-01-14_22-27" src="https://github.com/user-attachments/assets/fe1e50ed-a73f-445a-ba52-a4cc503bab20" />

<img width="732" height="475" alt="2026-01-14_22-27_1" src="https://github.com/user-attachments/assets/0880b6c2-73f2-4d43-b907-dd7396bc8cdb" />

<img width="732" height="481" alt="2026-01-14_22-27_2" src="https://github.com/user-attachments/assets/f9fb09b6-700c-4033-a3a2-dc1cef77dc13" />

<img width="735" height="490" alt="2026-01-14_22-28" src="https://github.com/user-attachments/assets/ed37bdb7-e919-4ebc-97a2-beb6b00509a2" />

<img width="735" height="482" alt="2026-01-14_22-28_1" src="https://github.com/user-attachments/assets/ac571a46-77e3-4c59-8bba-eea1747c454f" />

<img width="734" height="479" alt="2026-01-14_22-29" src="https://github.com/user-attachments/assets/a0565b57-3ecb-4af4-96b0-178e5370527b" />

<img width="729" height="483" alt="2026-01-14_22-29_1" src="https://github.com/user-attachments/assets/861a0f2d-8b51-47c1-9e0c-2cdc5299b69c" />

<img width="734" height="475" alt="2026-01-14_22-30" src="https://github.com/user-attachments/assets/83faed5e-4aaf-42e4-b2f1-3dc01c20574e" />

<img width="733" height="480" alt="2026-01-14_22-30_1" src="https://github.com/user-attachments/assets/937915fa-1eae-4e67-a79c-3f19ae41c532" />

En cas de detectar algun error o si cal realitzar ajustos posteriors, podem tornar a llançar l'assistent mitjançant l'ordre dpkg-reconfigure ldap-auth-config. Això ens permetrà revisar i corregir qualsevol paràmetre de l'autenticació.

<img width="714" height="137" alt="image" src="https://github.com/user-attachments/assets/70715638-b1b6-458c-9abd-92dd46444927" />

### Modificació fitxers

#### nsswitch

Cal incorporar els paràmetres ldap i compat a l'inici de les directives de configuració. D'aquesta manera, el sistema prioritzarà la consulta al directori LDAP per realitzar l'autenticació abans de buscar en els fitxers locals.

<img width="723" height="486" alt="2026-01-14_22-34" src="https://github.com/user-attachments/assets/6a9458fa-914b-496f-90a2-d127fadda20c" />

#### common-session

Finalment, cal incorporar l'última línia que es mostra en el fitxer de configuració per completar la integració del sistema.

<img width="731" height="476" alt="2026-01-15_10-03" src="https://github.com/user-attachments/assets/af88da3f-982c-4b04-862c-91831f85a626" />

#### common-password

Dins d'aquest fitxer de configuració, cal localitzar el paràmetre use_authtok i suprimir-lo de totes les línies on estigui present.

<img width="942" height="472" alt="2026-01-15_10-03_1" src="https://github.com/user-attachments/assets/c4bff522-2cb0-48a9-aab6-99dbd3988731" />

#### 50-ubuntu.conf

Afegim el greeter.

<img width="951" height="486" alt="2026-01-15_10-05" src="https://github.com/user-attachments/assets/dce68808-0c6b-42df-b8dc-efaf373873a9" />

### Comprovació

Un cop finalitzada la configuració, per validar que la connexió amb el servidor s'ha establert correctament, realitzarem una consulta d'un dels usuaris creats prèviament mitjançant els fitxers .ldif. Aquesta comprovació ens permetrà confirmar que el client pot resoldre usuaris que no existeixen localment.

Comprovem que podem entrar.

<img width="539" height="86" alt="2026-01-15_10-06" src="https://github.com/user-attachments/assets/ee83b36a-e331-42b0-82a1-7190cfc60b7f" />

El següent pas consisteix a comprovar l'inici de sessió gràfic. Per fer-ho, seleccionarem l'opció d'entrar amb un usuari nou, n'indicarem el nom (usuari alu1) i n'introduirem la contrasenya corresponent per verificar que el perfil es carrega sense incidències.

<img width="724" height="511" alt="2026-01-15_10-10" src="https://github.com/user-attachments/assets/46c1ee11-3ee8-4ec0-9731-0de15b6508b2" />

Comprovació final:

<img width="496" height="120" alt="2026-01-15_10-11" src="https://github.com/user-attachments/assets/273582d7-6b7f-4e7f-a7ab-4d6486b71fef" />

En aquest punt, la infraestructura de xarxa i el servei de directori estan plenament desplegats. El servidor gestiona les peticions d'autenticació correctament i el client respon de manera transparent, permetent l'accés tant per terminal com per entorn gràfic.

# Servidor Samba

## Teoria

Un servidor Samba permet la compartició de recursos en xarxa, com ara fitxers i impressores, en entorns multiplataforma (Linux i Windows). L'autenticació es gestiona a nivell d'usuari i ofereix la flexibilitat d'utilitzar tant una base de dades d'usuaris pròpia de Samba com una integració centralitzada mitjançant LDAP."

## Servidor

Instalem Samba

<img width="622" height="257" alt="2026-01-29_12-45" src="https://github.com/user-attachments/assets/f64a9428-1e51-454b-a17f-d16f3c3129fa" />

Primerament, procedirem a la creació del directori que volem compartir i n'ajustarem els permisos i la propietat. Definirem l'usuari i el grup corresponents per garantir que només els membres autoritzats tinguin accés al recurs compartit.

<img width="619" height="227" alt="2026-01-29_12-47" src="https://github.com/user-attachments/assets/9cf83b40-9339-4982-b753-b6ce29dc63f7" />

Per a la gestió dels accessos, procedirem a donar d'alta tres nous usuaris i un grup de seguretat. Posteriorment, vincularem aquests comptes al grup per tal de centralitzar els permisos del recurs compartit en una única unitat.

<img width="497" height="78" alt="2026-01-29_12-49" src="https://github.com/user-attachments/assets/e39eb2fe-2800-4965-96cd-30fda1b73131" />

Procedirem a establir les credencials d'accés per als nous usuaris. Aquest pas és indispensable per garantir que es puguin validar correctament en iniciar la sessió des del client.

<img width="495" height="305" alt="2026-01-29_13-12" src="https://github.com/user-attachments/assets/345d105b-4c8a-49c4-801c-d23bf40c225c" />

Procedirem a l'edició del fitxer smb.conf per afegir-hi la declaració del recurs compartit. Això inclou configurar els paràmetres necessaris perquè el directori sigui visible i accessible des de la xarxa per als clients autoritzats.

<img width="732" height="471" alt="2026-01-29_12-55" src="https://github.com/user-attachments/assets/91d04a91-f5fb-421a-9cfa-ff244d79758e" />

Per tal que els canvis realitzats en el fitxer de configuració s'apliquin correctamente, caldrà reiniciar el servei de Samba. Això permetrà que el sistema carregui la nova definició del recurs compartit."

<img width="520" height="113" alt="2026-01-29_12-57" src="https://github.com/user-attachments/assets/0516f1c0-54a5-4899-b28c-df89f024675f" />

<img width="737" height="391" alt="2026-01-29_12-58" src="https://github.com/user-attachments/assets/94de7298-70b7-4e9c-82bf-ee4634357a4d" />

## Client

Abans de procedir amb la connexió al recurs, cal verificar la connectivitat de xarxa entre el client i el servidor. Per fer-ho, utilitzarem l'eina ping per assegurar-nos que ambdues màquines es comuniquen correctament.

<img width="608" height="228" alt="2026-01-29_13-05" src="https://github.com/user-attachments/assets/9936c15e-2d35-443c-97cf-afff8ea3c4f3" />

Instalem smbclient.

<img width="735" height="484" alt="2026-01-29_13-07" src="https://github.com/user-attachments/assets/500656f0-b153-41c9-9ae1-526e65c27316" />

Procedirem a validar l'accés al recurs compartit mitjançant el protocol SMB. Des de l'entorn gràfic, utilitzarem l'opció de connexió a servidors indicant la ruta corresponent (smb://[IP_DEL_SERVIDOR]/[NOM_RECURS]) per establir el vincle.

<img width="900" height="546" alt="2026-01-29_13-08" src="https://github.com/user-attachments/assets/b2e1827e-a087-45e9-89b5-2fdb2343b250" />

Hem realitzat una prova d'accés com a usuari anònim i hem comprovat que és possible crear carpetes satisfactòriament. Aquest èxit es deu a la configuració dels permisos a nivell de sistema de fitxers i a les directives del recurs en Samba, que permeten l'escriptura a usuaris no autenticats.

<img width="891" height="547" alt="2026-01-29_13-09" src="https://github.com/user-attachments/assets/50157ac3-f86b-43dd-8f3c-c6f7bf9a2974" />

Hem validat l'accés nominal amb el compte de l'usuari naim. Les proves d'escriptura i lectura han estat satisfactòries, fet que confirma que les directives de seguretat aplicades al recurs compartit i els permisos del sistema de fitxers estan correctament sincronitzats.

<img width="468" height="431" alt="2026-01-29_13-18" src="https://github.com/user-attachments/assets/71f2816d-590e-4aa1-8c67-b132cf6b5b5a" />

<img width="857" height="484" alt="image" src="https://github.com/user-attachments/assets/b64c7eb1-46c1-4e53-adae-448f2271316b" />

Per comprovar el control d'accés, hem iniciat sessió com a eros. El resultat de la prova ha estat satisfactori: l'usuari pot llistar els fitxers, però en intentar crear un nou directori, el servidor Samba retorna un error de permís denegat, complint amb la configuració establerta.

<img width="461" height="434" alt="2026-01-29_13-19" src="https://github.com/user-attachments/assets/160d3454-9522-4eaf-b0e5-fc595dcc9867" />

<img width="888" height="541" alt="2026-01-29_13-19_1" src="https://github.com/user-attachments/assets/b46bdba5-c6d0-4676-bf7b-b325493d57ec" />

Per concloure les proves de seguretat, hem intentat accedir amb l'usuari edgar. En aquest cas, el sistema ha denegat l'accés al recurs compartit, confirmant que els usuaris que no pertanyen al grup autoritzat no poden ni tan sols llistar el contingut del directori.

<img width="885" height="546" alt="2026-01-29_13-21" src="https://github.com/user-attachments/assets/454e9b3a-0506-4686-857f-71f5891121f6" />

Durant la prova amb l'usuari no autoritzat, hem observat que el sistema no retorna un error explícit de denegació, sinó que sol·licita cíclicament les credencials. Aquest comportament és el mètode estàndard del protocol per indicar que l'usuari no té privilegis d'accés al recurs sol·licitat.

<img width="462" height="432" alt="2026-01-29_13-21_1" src="https://github.com/user-attachments/assets/3d620b51-c818-46fd-a42d-293469259c22" />

## Extra

Iniciem la integració de LDAP creant un recurs compartit de zero. Configurem un nou directori al sistema i assignem els permisos d'usuari i grup necessaris per garantir una base neta abans de la vinculació.

<img width="700" height="391" alt="2026-02-09_09-39" src="https://github.com/user-attachments/assets/437808f8-9294-40d6-833a-a3658611a106" />

Preparem un fitxer .ldif per definir l'estructura de dades a LDAP. Hi especifiquem la creació d'un nou grup i els seus usuaris associats, seguint l'esquema d'atributs necessari per a la integració amb Samba.

<img width="407" height="660" alt="image" src="https://github.com/user-attachments/assets/3f683b8d-e4c0-4c31-91a9-86ecf0423cac" />

Executem la comanda ldapadd per carregar les definicions del fitxer LDIF al servidor. Amb això, l'estructura de grups i usuaris queda integrada i operativa dins del directori LDAP.



# Servidor NFS

Totes les configuracions i instal·lacions descrites a continuació es duran a terme a nivell de servidor. És primordial que la màquina central disposi de l'entorn Java per poder executar les eines de gestió del directori.

# Exercici LDAP

L'administració del directori es realitzarà mitjançant un entorn gràfic especialitzat. Entre les opcions disponibles, farem servir Apache Directory Studio, una eina basada en Eclipse que ens permetrà gestionar els objectes, atributs i esquemes de l'LDAP d'una manera més visual i intuïtiva.

## Instal·lació ADS

Com a requisit previ per a l'execució de l'Apache Directory Studio, és necessari instal·lar l'entorn de l'JRE (Java Runtime Environment) al sistema. Sense aquesta dependència, l'eina d'administració no podria iniciar-se correctament.

<img width="702" height="153" alt="2026-02-01_19-00" src="https://github.com/user-attachments/assets/d35cef1a-fcad-44a2-8527-c7a2e2782d4c" />

Un cop l'entorn Java està operatiu, procedirem a obtenir el paquet d'instal·lació de l'Apache Directory Studio des del seu lloc web oficial. Seleccionarem la versió compatible amb la nostra arquitectura de sistema per garantir una integració òptima.

<img width="1007" height="795" alt="2026-02-01_19-02" src="https://github.com/user-attachments/assets/86ef8e1e-fa72-47b9-b31f-7651e4300a53" />

El descomprimim.

<img width="736" height="188" alt="2026-02-01_19-05" src="https://github.com/user-attachments/assets/59d6f021-1496-471d-a8bd-48bd4797b78e" />

Per tal de mantenir l'ordre en el sistema de fitxers, desplaçarem el directori descomprimit a la ruta /opt/. Aquesta ubicació és la idònia per instal·lar paquets de programari que no formen part de la distribució oficial del sistema operatiu.

<img width="733" height="89" alt="2026-02-01_19-06" src="https://github.com/user-attachments/assets/ac597e72-96f6-4a26-a281-658de013f199" />

Procedirem a modificar els atributs del fitxer executable mitjançant l'ordre chmod. En atorgar el bit d'execució, habilitarem l'accés al programa des de la línia de comandes o des de l'entorn de l'escriptori.

<img width="735" height="61" alt="2026-02-01_19-07" src="https://github.com/user-attachments/assets/44a8bf09-da8e-42f4-b9b8-b67dc50b8c2e" />

Abans d'executar l'aplicació, és necessari realitzar un ajust en el fitxer de configuració .ini per resoldre una incompatibilitat de versions. Atès que l'ADS està preconfigurat per treballar amb Java 11 i el nostre sistema disposa de la versió 17 (LTS), caldrà especificar la ruta correcta de la nostra màquina virtual (JVM).

<img width="730" height="483" alt="2026-02-01_19-14" src="https://github.com/user-attachments/assets/8a2b7ed2-e3cd-4db3-9435-5d2b96b0cfca" />

Executem l’aplicació.

<img width="735" height="181" alt="2026-02-01_19-34" src="https://github.com/user-attachments/assets/5c0f2116-3e6d-443d-a5d0-925b3214c7fc" />

Amb l'aplicació ja operativa, el següent pas consisteix a establir una nova connexió LDAP. En aquest procés, configurarem els paràmetres de xarxa i les credencials d'administrador per poder interactuar directament amb el servei de directori des de la interfície gràfica.

<img width="940" height="715" alt="2026-02-01_19-34_1" src="https://github.com/user-attachments/assets/5f15cd2b-f263-4242-97c7-b8be5c08f31b" />

En la finestra de configuració, assignarem un nom identificatiu a la connexió per facilitar-ne la gestió i especificarem l'adreça IP del nostre servidor. Aquests paràmetres permetran a l'aplicació localitzar el servei LDAP dins la xarxa.

<img width="606" height="501" alt="2026-02-01_19-35" src="https://github.com/user-attachments/assets/0ac5f400-ac30-45af-b6b9-5e99ce2eafe5" />

En la següent fase del configurador, procedirem a l'autenticació del sistema. Introduirem les credencials d'administrador configurades prèviament, especificant el Bind DN (compost pel cn i el dc corresponents) i la contrasenya per tal d'obtenir permisos de gestió sobre el directori.

<img width="605" height="396" alt="2026-02-01_19-36" src="https://github.com/user-attachments/assets/61e094ba-d894-4d42-9a59-7cbef41965ab" />

Com a pas de control, verificarem l'autenticació per assegurar-nos que el Bind DN i la contrasenya han estat introduïts correctament. Un cop validat el punt d'accés, finalitzarem el procés, moment en el qual l'arbre de directori es farà visible a la interfície de l'ADS.

<img width="611" height="255" alt="2026-02-01_19-37" src="https://github.com/user-attachments/assets/523a1686-7cb6-462e-af45-d71b1ded16b5" />

Un cop establerta la connexió, ens dirigirem al panell 'LDAP Browser' situat a la part esquerra de la interfície. Des d'aquí, ja podrem visualitzar la jerarquia del nostre servidor LDAP i confirmar que l'estructura base del domini s'ha carregat correctament.

<img width="1227" height="761" alt="2026-02-01_19-39" src="https://github.com/user-attachments/assets/d4f0c6c8-a38e-45fa-b8df-4d8da60f3433" />

### Proves ADS

A continuació, procedirem a la creació d'un nou usuari en el directori per verificar el cicle complet de gestió. Un cop donat d'alta, realitzarem una prova d'inici de sessió des del client per demostrar que l'autenticació centralitzada i l'accés als recursos funcionen segons el previst.

#### Crear usuari

Iniciarem el procés d'alta d'objectes mitjançant el menú contextual del domini. En triar 'New Entry', podrem escollir entre crear una Unitat Organitzativa (OU) per estructurar el directori o definir directament nous usuaris i grups sota la jerarquia del domini.

<img width="1222" height="760" alt="2026-02-01_19-40" src="https://github.com/user-attachments/assets/532bd691-fd05-43ae-bd3e-b0214f85560f" />

El crearem de 0.

<img width="605" height="581" alt="2026-02-01_19-40_1" src="https://github.com/user-attachments/assets/8489877f-a889-47c7-a575-65ac53844aab" />

El següent pas consisteix a triar l'esquema de l'objecte. Mitjançant la interfície de selecció, afegirem la classe d'objecte organizationalUnit. Aquesta acció ens permetrà establir una nova Unitat Organitzativa, proporcionant un marc jeràrquic on posteriorment integrarem els comptes d'usuari.

<img width="613" height="584" alt="2026-02-01_19-41" src="https://github.com/user-attachments/assets/4270465f-904f-4bb1-89fa-461bc915bdaa" />

Dins del selector d'atributs, cercarem el terme ou (Organizational Unit) per definir la classe d'objecte. Un cop seleccionada, li assignarem un nom identificatiu (com ara 'Usuaris' o 'Grups'), establint així la base jeràrquica on s'allotjaran els futurs registres.ç

<img width="607" height="665" alt="2026-02-01_19-42" src="https://github.com/user-attachments/assets/3aa01b88-1c37-4381-8fa5-d9f70b9da48d" />

Ara que ja tenim la nova UO creada, creem un usuari.

<img width="1220" height="755" alt="2026-02-01_19-43" src="https://github.com/user-attachments/assets/f34f9329-0d5d-4f74-a101-bc89c2957591" />

Procedirem a afegir les Object Classes requerides per a la interoperabilitat del compte. Mitjançant inetOrgPerson definim l'entrada com un objecte de persona estàndard, i amb l'extensió posixAccount habilitem els paràmetres de nivell de sistema (com el UID, GID i el directori Home) que permetran l'inici de sessió en el client.

<img width="606" height="579" alt="2026-02-01_19-44" src="https://github.com/user-attachments/assets/69b420c5-1db0-4178-bdac-8fb2ea95810f" />

Al RDN li ficarem uid i li afegim el nom que volem.

<img width="608" height="583" alt="2026-02-01_19-45" src="https://github.com/user-attachments/assets/713bf295-7e77-44cf-bd78-e797b619f865" />

Per tal de definir les credencials d'accés, haurem d'incorporar un nou atribut a l'entrada de l'usuari. Mitjançant el menú contextual (clic dret) i l'opció 'New Attribute', seleccionarem l'atribut userPassword, el qual ens permetrà assignar una contrasenya segura per a l'autenticació.

<img width="596" height="576" alt="2026-02-01_19-46" src="https://github.com/user-attachments/assets/4765f87c-5b39-4220-9a50-b483f0837a42" />

Busquem userPassword.

<img width="604" height="406" alt="2026-02-01_19-46_1" src="https://github.com/user-attachments/assets/e2dd5887-02c0-4bea-b0dd-dce2951ea3be" />

Un cop seleccionat l'atribut userPassword, procedirem a l'assignació de la clau. L'assistent de l'Apache Directory Studio ens permet, a més, escollir un mètode de hashing (com ara SSHA o SHA). Aquesta pràctica garanteix que, en cas d'un accés no autoritzat a la base de dades LDAP, les credencials dels usuaris romanguin protegides.

<img width="789" height="526" alt="2026-02-01_19-47" src="https://github.com/user-attachments/assets/666ba8e8-7159-4454-98c2-3d9e41178199" />

Haurém d’afegir també l’informació que ens sortia en roig.

<img width="609" height="583" alt="2026-02-01_19-53" src="https://github.com/user-attachments/assets/8d6e9dcf-4954-4ae0-81eb-f9ff523c589b" />

Ara podem veure que ja tenim l’usuari creat.

<img width="1226" height="761" alt="2026-02-01_19-53_1" src="https://github.com/user-attachments/assets/7c5891d3-ed87-4390-b790-fb4f7aba02ab" />

#### Inici sessió client

Un cop configurat l'usuari al servidor, ens dirigirem a l'estació de treball client per verificar l'autenticació. A la pantalla d'inici, seleccionarem l'opció de canviar d'usuari i farem clic sobre 'No està a la llista?'. Això ens permetrà introduir manualment el nom d'usuari emmagatzemat al directori LDAP.

<img width="1338" height="881" alt="2026-02-01_20-05" src="https://github.com/user-attachments/assets/799fe2fb-49c5-4d52-b9d1-a7e8ffb1f29b" />

<img width="640" height="437" alt="2026-02-01_20-07" src="https://github.com/user-attachments/assets/0a017707-1bff-4374-9969-db15e40db972" />

Afegim també la contrasenya i ja ens crearà l’usuari.

Ara ja podem veure a la terminal si està tot correcte.

<img width="525" height="130" alt="2026-02-01_20-08" src="https://github.com/user-attachments/assets/08372051-c1f2-48a4-a8b0-8af64bc31270" />

## Fitxers ldif i comandes ldap (exercici moodle)

### Fes un dpkg-reconfigure slapd al servidor per tal de deixar la base de dades buida i només amb el domini i l’usuari admin creat. Comprova-ho amb un slapcat.

<img width="607" height="18" alt="2026-02-01_20-18" src="https://github.com/user-attachments/assets/a022911c-3d76-4c02-88ca-e591deb218f9" />

<img width="533" height="288" alt="2026-02-01_20-28" src="https://github.com/user-attachments/assets/6b6d6772-131e-4c82-8b20-df18a36104d1" />

### Descarrega l'arxiu dades_pt10.ldif del moodle i amb la comanda ldapadd carrega els usuaris, grups i uos (Compte que el domini és vesper.cat, hauràs de modificar-lo pel teu)

<img width="734" height="299" alt="2026-02-01_20-55" src="https://github.com/user-attachments/assets/4687852f-007f-445d-8d93-f9c1dba2dd4b" />

### Fem un altre slapcat per tal de comprovar que les dades s'han carregat correctament

<img width="418" height="770" alt="2026-02-01_20-56" src="https://github.com/user-attachments/assets/33cf15fa-b7ca-4ba5-bb87-66df3591d1c5" />

### 1. Crea un nou usuari directament al domini

<img width="900" height="320" alt="2026-02-01_21-38" src="https://github.com/user-attachments/assets/e6e2a6b3-8f75-405f-998c-c09ed5436e9a" />

L’afegim.

<img width="982" height="63" alt="2026-02-01_21-39" src="https://github.com/user-attachments/assets/75dba8df-2687-4e6c-be95-c2f6792343ae" />

Ara el podem buscar.

<img width="820" height="277" alt="2026-02-01_21-39_1" src="https://github.com/user-attachments/assets/fb6cb5d9-6389-40a9-aa53-1d9742961645" />

### 2. Crea una nova uo anomenada nòmines

<img width="884" height="151" alt="2026-02-01_21-41" src="https://github.com/user-attachments/assets/5549c3ec-e752-445d-ba3a-a76b16559f4a" />

L’afegim.

<img width="955" height="220" alt="2026-02-01_21-43" src="https://github.com/user-attachments/assets/6ef672e4-26dd-4f68-b186-2626c7a09462" />

### 3. Mou l’usuari que has creat dintre de la uo nòmines

<img width="803" height="194" alt="2026-02-01_21-48" src="https://github.com/user-attachments/assets/d9220f0b-3854-4e6e-8c07-b6929b75f864" />

Afegim i confirmem.

<img width="918" height="383" alt="2026-02-01_21-49" src="https://github.com/user-attachments/assets/07847ccb-06df-430a-b554-998900c9b3ac" />

### 4. Quants grups hi ha al domini vesper.cat?

<img width="932" height="311" alt="2026-02-01_21-51" src="https://github.com/user-attachments/assets/6dd6d23d-a477-4501-8a4b-effabd93590f" />

Hi han 2 grups, informatica i administracio.

### 5. Afegeix l’usuari que has creat dintre d’un dels grups del domini

<img width="792" height="168" alt="2026-02-01_21-52" src="https://github.com/user-attachments/assets/02a7d913-6176-4ac8-8c28-757e582e7e3e" />

<img width="1042" height="399" alt="2026-02-01_21-54" src="https://github.com/user-attachments/assets/5c23a1dc-7778-4762-9025-d4d5937220e3" />

### 6. D’un sol cop: Afegeix un nou atribut opcional a l’usuari sergi, modifica el cognom de l’usuari sergi al valor Pallarés

<img width="783" height="207" alt="2026-02-01_21-57" src="https://github.com/user-attachments/assets/3f3555a7-ddcd-42ba-b767-75883947772c" />

No fiquem accents ja que no apareixerà el valor correctament.

<img width="1020" height="380" alt="2026-02-01_21-58" src="https://github.com/user-attachments/assets/5557eb2b-6ae7-42dc-9f11-d6125660c162" />

### 7. Quants usuaris hi ha dintre de la uo rrhh? Quins són?

<img width="995" height="255" alt="2026-02-01_22-00" src="https://github.com/user-attachments/assets/940dc75d-9172-4ca6-a017-9e6ccebff55b" />

Hi han 3 usuaris, xavier, enric, sergi i manu.

### 8. Esborra el gidNumber del grup informàtica

<img width="792" height="133" alt="2026-02-01_22-02" src="https://github.com/user-attachments/assets/2d81b447-1321-46ce-a15e-b7fad71e1a1c" />

Durant el procés de configuració, observarem que certs atributs no poden ser eliminats a causa de les restriccions de l'esquema. Concretament, la classe d'objecte posixGroup requereix obligatòriament l'atribut gidNumber per complir amb l'estàndard POSIX, garantint així que el sistema operatiu pugui identificar el grup correctament.

<img width="1008" height="132" alt="2026-02-01_22-03" src="https://github.com/user-attachments/assets/d1e1c3ad-5ca1-4f16-84a1-61a2a8472fe6" />

### 9. Quantes uos hi ha al domini vesper.cat?

<img width="1036" height="204" alt="2026-02-01_22-04" src="https://github.com/user-attachments/assets/e8f34ede-e943-4594-817e-75b32355e502" />

Hi han 3 UOs.

### 10. Modifica el cn de Xavier per Francesc Xavier

<img width="807" height="153" alt="2026-02-01_22-06" src="https://github.com/user-attachments/assets/cf172a60-d7c8-4d6b-a494-b9555ad0abe2" />

<img width="1019" height="362" alt="2026-02-01_22-07" src="https://github.com/user-attachments/assets/7b4f90c6-0d61-45a3-aae6-128f71dbf43d" />

### 11. Esborra la uo nòmines

<img width="1059" height="93" alt="2026-02-01_22-09" src="https://github.com/user-attachments/assets/4f8d6fa6-addd-4442-914e-b1771f8de2a1" />

En intentar suprimir la Unitat Organitzativa (UO), el sistema ens retornarà un error de restricció, ja que no es pot eliminar un node que conté objectes dependents. Per procedir, caldrà realitzar un esborrat en cascada manual: primer eliminarem els comptes d'usuari allotjats en el seu interior i, un cop el directori estigui buit, podrem eliminar la UO definitivament.

<img width="752" height="60" alt="2026-02-01_22-14" src="https://github.com/user-attachments/assets/54cd52fb-abd1-4de0-868b-6dafd9e155b6" />

Ara ja no ens apareix ni l’usuari ni el grup.

Els passos anteriors s'hem va peta la terminal i no me surtia el resultat.

### 12. Mostra els usuaris que tinguin com a grup principal el grup administració

Administració té la gid 1002.

<img width="557" height="210" alt="2026-02-01_22-15" src="https://github.com/user-attachments/assets/687a33e9-1038-4dc8-b0ed-f11a01becfa0" />

<img width="793" height="56" alt="2026-02-01_22-16" src="https://github.com/user-attachments/assets/e68e6a1e-0688-4cfd-a6c6-87c113c69738" />

Només està l’usuari Sergi.

### 13. Quin usuari té el uidNumber 1003?

<img width="832" height="51" alt="2026-02-01_22-16_1" src="https://github.com/user-attachments/assets/dd882a2d-beb3-4dfd-9586-24b0588dfff0" />

No hi ha cap en aquest uidNumber.

### 14. Mostra quins són els usuaris on el seu cognom comenci per R i el seu uidNumber sigui més gran que 1003

<img width="893" height="44" alt="2026-02-01_22-18" src="https://github.com/user-attachments/assets/fbdc593b-019f-492f-9a5a-13c896dbfe3c" />

En executar la consulta, observem que el resultat és buit. Això es deu al fet que cap entrada compleix simultàniament els criteris establerts: d'una banda, l'usuari modificat al punt anterior ja no coincideix amb el filtre de cognom (ha passat de 'Reus' a 'Pallares') i, de l'altra, els registres restants queden exclosos en tenir un uidNumber inferior al llindar de 1003.

### 15. Mostra quins usuaris formen part del grup informàtica o aquells usuaris que tinguis de cognom Pallarés

El gidNumber de informàtica és 1001.

<img width="935" height="19" alt="2026-02-01_22-22" src="https://github.com/user-attachments/assets/c95bd0fb-3264-4d0c-9a28-2790fd00f3fe" />

<img width="336" height="684" alt="2026-02-01_22-22_1" src="https://github.com/user-attachments/assets/9caba4e6-71da-445c-acc5-86e8f05b65b9" />

Ens apareix 3 en total, 2 del grup informàtica i 1 que el seu cognom és Pallares.
