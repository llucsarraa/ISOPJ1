---
layout: default
title: SP1. Instal·lació, Configuració Inicial i Programari de Base
---

# Virtualitzacio i instal·lació del SO Ubuntu
# Instal·lació d’una màquina virtual amb Ubuntu

Per començar la instal·lació vaig obrir l'aplicació VirtualBox per a crear una nova màquina virtual. Li vaig posar el nom **UbuntuSP1**, vaig seleccionar el sistema operatiu **Ubuntu 22.03.04 (64-bit)** i vaig carregar la imatge ISO corresponent.  

<img width="1920" height="1080" alt="inici dels noms de la MV" src="https://github.com/user-attachments/assets/ba101a60-3b4a-4564-89a7-2e2f7acccc14" />

Un cop definits aquests paràmetres inicials, vaig passar a assignar els recursos de la màquina. Li vaig donar **4096 MB de memòria RAM** i **3 processadors**, suficient per poder treballar de manera fluida.  
<img width="1920" height="1080" alt="configuracio abans d'arrancar la MV" src="https://github.com/user-attachments/assets/3074f227-9cb7-4368-a07c-a35ce4828648" />

Després vaig configurar el disc dur virtual. Vaig crear un disc de **80 GB** de tipus dinàmic, de manera que només ocupa l’espai real que utilitza el sistema.  
<img width="1920" height="1080" alt="Quan acabes dels parametres de VirtualBox  finish" src="https://github.com/user-attachments/assets/efd1b4a0-cd62-4c0b-ba5b-59176b3e0dfc" />

Amb la màquina configurada, vaig iniciar la instal·lació d’Ubuntu. Durant aquest procés es mostra un resum de les dades de la màquina virtual i, tot seguit, comença el particionament del disc.  
<img width="1913" height="955" alt="MV amb les dades" src="https://github.com/user-attachments/assets/636274e1-0ca3-40f0-a2a8-f4d149812aff" />

Per al particionament vaig decidir fer un **dual boot**, repartint els **80 GB totals en dues parts**: **40 GB per a Ubuntu** i **40 GB lliures** per a futurs usos o instal·lacions.  
<img width="1920" height="1080" alt="Quan acabes dels parametres de VirtualBox  finish" src="https://github.com/user-attachments/assets/87bd6829-6b0d-4366-a853-f3313141426d" />
<img width="1920" height="1080" alt="Reparticio d'espais" src="https://github.com/user-attachments/assets/b7e843fa-6a8c-4654-a99e-e5d35b81cd08" />

Durant aquest pas va sorgir un avís: el sistema em demanava crear una petita partició d’uns **1 MB reservada per al BIOS boot area**. Aquesta partició és necessària perquè el carregador d’arrencada funcioni correctament. Vaig afegir-la per resoldre el problema.  
<img width="1920" height="1080" alt="Problema reserved BIOS boot area" src="https://github.com/user-attachments/assets/7e851cb4-207a-41cc-bac1-402acd7d5a6a" />

Finalment, la taula de particions va quedar de la següent manera:  
- `/` → 12 GB  
- `/home` → 25 GB  
- `/boot` → 1 GB  
- `swap` → 2 GB  
- `efi` → 399 MB  

<img width="1920" height="1080" alt="Reparticio d'espais" src="https://github.com/user-attachments/assets/222e9e60-a848-49e6-9b43-3a13c5acab36" />

Una vegada fetes les particions vaig instal·lar el sistema i va quedar llest per a utilitzar.


# Llicenciament
## Llicències a Ubuntu  

Ubuntu és una distribució de GNU/Linux basada en programari lliure i de codi obert. El seu ús està regulat principalment per la **Llicència Pública General de GNU (GPL)** i altres llicències lliures compatibles.  

- **GPL (GNU General Public License):** garanteix les 4 llibertats del programari lliure: utilitzar, estudiar, modificar i redistribuir. A més, obliga que les obres derivades es distribueixin sota la mateixa llicència (*copyleft*).  
- **LGPL (Lesser GPL):** utilitzada en algunes llibreries, permet la seva integració amb programari propietari.  
- **Llicències Permissives (MIT, Apache, BSD):** presents en certs components, permeten més flexibilitat en la redistribució del codi.  

### Aspectes legals  
El programari es troba protegit pels **drets d’autor (copyright)**. En el cas del programari lliure com Ubuntu, aquests drets es gestionen mitjançant llicències que asseguren tant la protecció dels creadors com les llibertats dels usuaris.  

Més informació:  
- [GNU Llicències](https://www.gnu.org/licenses/licenses.es.html)  
- [Llicències Ubuntu](https://ubuntu.com/licensing)  

# Gestors d'arrancada per a instal·lacions DUALS
## Pas 1: Context - Instal·lació de Windows

Per començar, és necessari accedir als ajustos de la nostra màquina virtual. Un cop dins, hem de dirigir-nos a la secció de "Sistema" i habilitar l'opció "Activa EFI (només SO especials)". Aquest pas és crucial, ja que ens donarà la possibilitat d'instal·lar Windows 10 a la mateixa unitat de disc. Si ometem aquesta acció, el sistema no permetrà la instal·lació a causa de la incompatibilitat entre el sistema MBR de la BIOS que utilitza Windows i el sistema GPT UEFI d'Ubuntu.

<img width="1071" height="584" alt="1" src="https://github.com/user-attachments/assets/b047363d-35ef-48e0-8b12-e81e18b00ed3" />

Per carregar la imatge de Windows a la unitat de disc virtual, cal dirigir-se a l'apartat "Emmagatzematge" dins de la configuració. Allà, es visualitzarà una unitat òptica amb l'etiqueta "Buit". S'ha de seleccionar aquesta unitat i, posteriorment, prémer la icona del disc blau per obrir l'explorador d'arxius i seleccionar la imatge ISO del sistema operatiu.

<img width="1310" height="547" alt="2" src="https://github.com/user-attachments/assets/f420b0b3-e3c6-468c-a5e4-252408aecfdc" />

Un cop completats aquests passos, ja podem procedir a iniciar la màquina virtual per començar amb la instal·lació de Windows 10. Durant el procés, quan s'hagi d'escollir el tipus d'instal·lació, és necessari seleccionar l'opció "Personalitzada".

<img width="1642" height="923" alt="instalació windows 2" src="https://github.com/user-attachments/assets/adca4ec5-fabe-418a-af33-6e34be2098c3" />

A continuació, cal escollir la unitat de disc que figura com a disponible. Des d'aquí, es pot procedir a la creació d'una nova partició en aquest espai.

<img width="1642" height="923" alt="instalacio windows 3" src="https://github.com/user-attachments/assets/d9258878-47b6-42d0-a3d1-81edbcdf4d66" />

Un cop la màquina virtual es reiniciï automàticament, començarà la fase de configuració estàndard de Windows, on caldrà seguir les indicacions del sistema operatiu.

<img width="1642" height="923" alt="Captura de pantalla de 2025-09-30 13-49-11" src="https://github.com/user-attachments/assets/74ea6395-6c9a-4e36-98b5-620ab9ce0069" />
<img width="1642" height="923" alt="Captura de pantalla de 2025-09-30 13-54-34" src="https://github.com/user-attachments/assets/fbed92d4-49f8-4a47-b1b3-706ccbbee46c" />

Un cop completats aquests passos finals, el sistema operatiu quedarà correctament instal·lat i a punt per al seu ús.

<img width="1642" height="923" alt="Captura de pantalla de 2025-09-30 13-56-39" src="https://github.com/user-attachments/assets/0af992e5-13eb-4a7d-b1ea-37ad4770aa84" />

## Pas 2: Identificació del Problema i Preparació de l'Eina de Rescat

El problema es va originar en instal·lar Windows en una màquina on ja existia Ubuntu. El procés d'instal·lació de Windows sobreescriu el gestor d'arrencada principal (MBR o partició EFI), eliminant l'entrada al GRUB d'Ubuntu.
Un cop finalitzada la instal·lació, Windows arrencava correctament, però l'opció per iniciar Ubuntu havia desaparegut. Per a solucionar el problema primer de tot és apagar la màquina virtual. A continuació, és necessari accedir novament a la configuració per retirar la imatge del disc d'instal·lació (ISO). Aquesta acció és indispensable per evitar que el sistema intenti arrencar des d'aquest mitjà en el proper inici.

<img width="1159" height="551" alt="Captura de pantalla de 2025-10-22 19-13-26" src="https://github.com/user-attachments/assets/91138701-134c-4ead-bd17-a385c085f779" />

Després de retirar la imatge de Windows, el següent pas consisteix a muntar una nova imatge ISO a la unitat òptica virtual. En aquesta ocasió, s'ha de carregar un arxiu destinat a la recuperació del gestor d'arrencada GRUB, que en aquest cas concret és el fitxer "super_grub2_disk_hybrid_2.02s3.iso".

<img width="601" height="35" alt="Captura de pantalla de 2025-10-07 11-35-21" src="https://github.com/user-attachments/assets/5c8ba713-acb9-48f4-bc16-4fe08529e7b7" />

<img width="1061" height="544" alt="Captura de pantalla de 2025-10-22 19-18-41" src="https://github.com/user-attachments/assets/fbe26afe-e965-46dc-a8f2-5030283c0132" />

## Pas 3: Arrencant des de l'Eina de Rescat

Per tal d'iniciar el sistema des d'aquesta imatge ISO, cal arrencar la màquina virtual i, immediatament, prémer la tecla "ESC" per accedir al menú d'arrencada. Un cop dins d'aquest menú, s'ha de seleccionar l'opció "Boot Manager".

<img width="1259" height="631" alt="f12 dins la maquina part 1" src="https://github.com/user-attachments/assets/49ac141a-057a-43fb-b940-f20173ca1fbd" />

Per iniciar des del mitjà de recuperació, procedirem a escollir la unitat de CD-ROM, ja que és el dispositiu on hem muntat prèviament la imatge ISO.

<img width="1259" height="631" alt="f12 maquina virtual part 2" src="https://github.com/user-attachments/assets/29d28855-c424-456b-b3eb-2217fd0b47c2" />

A continuació, es mostrarà el menú principal de la imatge ISO. Aquesta eina s'utilitzarà per accedir al sistema Ubuntu i reinstal·lar el gestor d'arrencada GRUB. Dins del menú, s'ha d'escollir l'opció "Detect and show boot methods".

<img width="816" height="298" alt="Captura de pantalla de 2025-10-07 11-37-39" src="https://github.com/user-attachments/assets/1bc8b127-85ef-499d-b323-0a820665f8e1" />

A la pantalla a la qual accedim, el pas següent és identificar l'opció corresponent, tal com es mostra a la captura de pantalla adjunta.

<img width="816" height="298" alt="Captura de pantalla de 2025-10-07 11-38-07" src="https://github.com/user-attachments/assets/0fcf9b4f-454c-4e5c-972f-bbedb494b440" />

Aquesta acció iniciarà el sistema operatiu Ubuntu. Un cop hagi carregat l'entorn d'escriptori, caldrà obrir una finestra de terminal i executar les següents ordres de manera seqüencial:

- sudo grub-install /dev/sda
- sudo update-grub2

<img width="1090" height="596" alt="Captura de pantalla de 2025-10-07 11-40-28" src="https://github.com/user-attachments/assets/36de2cf9-d9f3-4d14-9358-da13ae96b302" />

És possible que la sortida de la darrera comanda mostri un avís referent a la variable GRUB_DISABLE_OS_PROBER. Si no es gestiona aquesta configuració, el gestor d'arrencada GRUB no detectarà altres sistemes operatius, impedint l'accés a Windows. Per corregir-ho, és necessari editar el seu fitxer de configuració. Primer, elevarem els privilegis a superusuari i, seguidament, executarem l'ordre per obrir l'arxiu:

- sudo su
- nano /etc/default/grub

<img width="1090" height="596" alt="Captura de pantalla de 2025-10-07 11-42-23" src="https://github.com/user-attachments/assets/07677a11-0b71-433c-9a37-4fc72f79278d" />

Dins de l'editor, cal localitzar la línia GRUB_DISABLE_OS_PROBER=false. Si aquesta línia està "comentada" (precedida per un símbol '#'), s'ha d'eliminar aquest símbol. En cas que la línia no existeixi, s'ha d'afegir manualment. Un cop la directiva estigui activa, desarem els canvis (amb Ctrl+O) i tancarem l'editor (Ctrl+X). Finalment, per aplicar la nova configuració, executarem de nou la comanda update-grub.

<img width="1090" height="596" alt="Captura de pantalla de 2025-10-07 11-43-09" src="https://github.com/user-attachments/assets/11f65162-5aff-4724-90e4-588563653a81" />

En reiniciar la màquina virtual, el gestor d'arrencada GRUB hauria de mostrar un menú amb les diferents opcions de sistema operatiu. Per verificar que el procés s'ha completat amb èxit, es recomana accedir a cadascun d'ells per comprovar la seva correcta funcionalitat.

<img width="1090" height="596" alt="Captura de pantalla de 2025-10-07 11-43-49" src="https://github.com/user-attachments/assets/274f3964-32d2-4e4a-811f-b9d08ae884d1" />

## Pas 4: Verificació

S'accedeix al sistema operatiu Windows per verificar que s'inicia i funciona correctament després de la configuració del menú d'arrencada.

(imatge)

Posteriorment, es reinicia el sistema i s'accedeix a Ubuntu per realitzar la mateixa comprovació i verificar que també funciona correctament.

(imatge)

# Punts de restrauració


# Gestor de paquets
dpkg
instal·la
obrir l'app
desinstalar
obrir l'error de que no hi ha app
apt
instal·la
obrir l'app
desinstalar
obrir l'error de que no hi ha app
aptitude
instal·la
obrir l'app
desinstalar
obrir l'error de que no hi ha app
repositori
instal·la
obrir l'app
desinstalar
obrir l'error de que no hi ha app
# Configuració de la xarxa
# Comandes generals i instal·lacions
