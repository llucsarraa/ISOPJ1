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














