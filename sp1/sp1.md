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

El problema es va originar en instal·lar Windows en una màquina on ja existia Ubuntu. El procés d'instal·lació de Windows sobreescriu el gestor d'arrencada principal (MBR o partició EFI), eliminant l'entrada al GRUB d'Ubuntu.

<img width="1642" height="923" alt="Captura de pantalla de 2025-09-30 13-54-34" src="https://github.com/user-attachments/assets/02677f58-bdd8-47f2-8df5-f067194c4974" />

Un cop finalitzada la instal·lació, Windows arrencava correctament, però l'opció per iniciar Ubuntu havia desaparegut.

<img width="1642" height="923" alt="Captura de pantalla de 2025-09-30 13-56-39" src="https://github.com/user-attachments/assets/78d5516c-ccc6-429d-8c95-760a40e67686" />

## Pas 2: Identificació del Problema

En intentar arrencar el sistema operatiu original (Ubuntu), el sistema es quedava bloquejat a la pantalla de càrrega, confirmant que el gestor d'arrencada estava corrupte o era inaccessible.

<img width="1642" height="923" alt="Captura de pantalla de 2025-09-30 13-49-11" src="https://github.com/user-attachments/assets/0c2960cc-b3ec-4a89-92e1-5c754d974ddb" />

## Pas 3: Preparació de l'Eina de Rescat

Per solucionar-ho, es va utilitzar l'eina **Super Grub2 Disk**, una imatge ISO d'arrencada dissenyada per detectar i arrencar sistemes operatius encara que el seu gestor d'arrencada estigui danyat.

<img width="601" height="35" alt="Captura de pantalla de 2025-10-07 11-35-21" src="https://github.com/user-attachments/assets/d701f8ce-8ac9-4478-8f92-72293bad1c52" />

## Pas 4: Arrencant des de l'Eina de Rescat

Es va muntar la imatge ISO a la màquina virtual i es va configurar per arrencar des d'aquesta unitat. Al menú d'arrencada UEFI de la màquina, es va seleccionar l'opció `UEFI VBOX CD-ROM`.

<img width="816" height="298" alt="Captura de pantalla de 2025-10-07 11-37-29" src="https://github.com/user-attachments/assets/49a06049-6e93-43e1-9ef9-a11cb3777ca3" />

## Pas 5: Detecció i Arrencada del Sistema

Un cop iniciat Super Grub2 Disk, es va seleccionar l'opció principal: **"Detect and show boot methods"**.

<img width="816" height="298" alt="Captura de pantalla de 2025-10-07 11-37-39" src="https://github.com/user-attachments/assets/993cba34-e509-427f-b4f1-79e2a72365b2" />

L'eina va escanejar els discs i va trobar el fitxer de configuració de GRUB d'Ubuntu (`(hd0,gpt3)/grub/grub.cfg`). Seleccionar aquesta opció va permetre iniciar el sistema Ubuntu de forma temporal per poder-lo reparar.

<img width="816" height="298" alt="Captura de pantalla de 2025-10-07 11-38-07" src="https://github.com/user-attachments/assets/4a2068b8-9bd3-4df5-990c-d6caf4ce0ec0" />

## Pas 6: Reinstal·lació del GRUB (Primer Intent)

Ja dins d'Ubuntu, es va obrir un terminal i es van executar dues comandes clau:
1. `sudo grub-install /dev/sda` - Per reinstal·lar el GRUB al disc principal.
2. `sudo update-grub` - Per actualitzar la configuració i detectar els sistemes operatius.

Aquest primer intent va mostrar un avís important: `Warning: os-prober will not be executed`. Això indica que GRUB no va buscar altres sistemes operatius (com Windows).

<img width="1090" height="596" alt="Captura de pantalla de 2025-10-07 11-40-28" src="https://github.com/user-attachments/assets/5aa1f95b-da1c-4e25-a445-8178782cb0b8" />

## Pas 7: Activació de 'os-prober'

Per solucionar l'avís anterior, es va editar el fitxer de configuració `/etc/default/grub` amb l'editor `nano`.

Es va localitzar la línia `GRUB_DISABLE_OS_PROBER` i es va assegurar que el seu valor fos `false`. Això activa la capacitat de GRUB per detectar altres sistemes operatius.

<img width="1090" height="596" alt="Captura de pantalla de 2025-10-07 11-42-23" src="https://github.com/user-attachments/assets/b6992d2f-08ef-445d-9cc2-aaf7225e4166" />

## Pas 8: Actualització Final i Verificació

Després de guardar els canvis al fitxer, es va executar de nou la comanda `update-grub` (en aquest cas, com a usuari `root`).

La sortida va confirmar l'èxit del procés: la línia **"Found Windows Boot Manager"** va indicar que el GRUB s'havia generat correctament i ara incloïa una entrada per iniciar Windows, solucionant definitivament el problema.

<img width="1090" height="596" alt="Captura de pantalla de 2025-10-07 11-43-09" src="https://github.com/user-attachments/assets/f89f4a03-68fc-4a55-b382-eccdb74fc6c3" />

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
# Punts de restrauració
# Configuració de la xarxa
# Comandes generals i instal·lacions
