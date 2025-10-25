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
Explicació de què és:
Un punt de restauració consisteix en una còpia de seguretat de l'estat i la configuració del sistema operatiu en un moment específic. La seva finalitat principal és permetre la reversió de canvis per solucionar incidències. Per exemple, si la instal·lació posterior d'un programa, controlador o actualització causa un funcionament incorrecte de l'equip, es pot utilitzar aquest punt per retornar el sistema a l'estat previ a la instal·lació, resolent així el problema.

### Instal·lació del TimeShift

Per a la gestió de punts de restauració en sistemes Ubuntu, una de les eines més recomanades és TimeShift. Aquesta aplicació, que permet capturar i restaurar l'estat del sistema, requereix una instal·lació prèvia mitjançant el gestor de paquets APT.

<img width="1291" height="618" alt="Captura de pantalla de 2025-10-07 12-51-09" src="https://github.com/user-attachments/assets/cf2d4e05-2aa0-4118-bd76-58623a0cb8e1" />

### Configuració de la partició

Un cop instal·lada l'aplicació, el pas següent consisteix a preparar el disc on s'emmagatzemaran les còpies de seguretat. Per a això, és necessari crear una nova partició al disc virtual afegit prèviament, ja que, tal com s'il·lustra a la imatge adjunta, aquest es troba actualment sense particions.

<img width="1291" height="618" alt="Captura de pantalla de 2025-10-07 12-54-23" src="https://github.com/user-attachments/assets/da4c2e58-6a98-433c-9f48-fff47f95f133" />

A continuació, s'inicia el procés per a la creació de la nova partició.

<img width="1291" height="618" alt="Captura de pantalla de 2025-10-07 12-51-52" src="https://github.com/user-attachments/assets/da5f6e7b-e298-427f-bb75-54f4d4e6459e" />

Durant el procés, s'ha de configurar la nova partició com a "Primària" i assignar-li la totalitat de l'espai disponible al disc dur.

<img width="1291" height="618" alt="Captura de pantalla de 2025-10-07 12-53-13" src="https://github.com/user-attachments/assets/e22eaacc-b495-4b44-8577-4d84724f0a74" />

Per verificar que el procés s'ha completat correctament, executem la comanda sudo fdisk -l. La sortida d'aquesta ordre ens hauria de confirmar visualment que la nova partició ja figura al llistat del disc.

<img width="1291" height="618" alt="Captura de pantalla de 2025-10-07 12-54-23" src="https://github.com/user-attachments/assets/bbd4f75c-9687-4d52-acf1-c8d1edf60f28" />

Un cop la partició ha estat creada, el següent pas és aplicar-li un sistema de fitxers. Per a això, l'hem de formatar mitjançant l'ordre mkfs.ext4 /dev/sdb1.

<img width="1291" height="618" alt="Captura de pantalla de 2025-10-07 12-55-59" src="https://github.com/user-attachments/assets/b4a7d0f6-dbaf-4bd9-8a28-5cc48420d600" />

### Com crear un punt de restauració

Amb la partició ja creada i degudament formatada, estem en disposició de generar un punt de restauració.

- Per tal d'il·lustrar el procés, simularem un canvi al sistema: procedirem a crear un arxiu de text (touch hola) i un nou directori (mkdir adeu) dins de la carpeta "Baixades".

<img width="1291" height="618" alt="Captura de pantalla de 2025-10-07 12-56-36" src="https://github.com/user-attachments/assets/2520f2e3-455c-4660-9ac1-1a7c806f8d98" />

Executem l'aplicació TimeShift, el pas inicial consisteix a seleccionar el tipus d'instantània. Per a la majoria de casos, s'ha d'escollir l'opció "RSYNC".

<img width="1291" height="618" alt="Captura de pantalla de 2025-10-07 12-57-24" src="https://github.com/user-attachments/assets/f174d5c8-e273-496f-ac74-b21f7691f1be" />

A continuació, hem de seleccionar la ubicació de destí per a les nostres còpies de seguretat. En aquesta configuració, designarem la partició que s'ha creat i formatat en els passos previs.

<img width="1291" height="618" alt="Captura de pantalla de 2025-10-07 12-57-53" src="https://github.com/user-attachments/assets/d682b29f-f190-4905-8f37-688505310088" />

Ara s'ha de definir la freqüència amb què es crearan les instantànies (per exemple, diàriament, setmanalment) i establir el nombre màxim de còpies que es desitja conservar.

<img width="1291" height="618" alt="Captura de pantalla de 2025-10-07 12-58-58" src="https://github.com/user-attachments/assets/02df4393-a3f1-4387-82ff-5b4e18d06ce9" />

En aquest punt de la configuració, hem de definir l'àmbit de la còpia de seguretat. Per al nostre exemple, especificarem que s'incloguin tots els fitxers continguts al directori personal de l'usuari.

<img width="1560" height="611" alt="Captura de pantalla de 2025-10-07 12-59-56" src="https://github.com/user-attachments/assets/49f38e7b-1a84-4e0a-ace0-135e160df441" />

Un cop hàgim premut el botó "Finalitza" ("Finish"), l'assistent de configuració es tancarà i accedirem a la interfície principal de l'aplicació. Aquesta pantalla és on es gestionen i es visualitzen els punts de restauració.

<img width="1560" height="611" alt="Captura de pantalla de 2025-10-07 13-03-01" src="https://github.com/user-attachments/assets/1c1306f3-f652-4c60-a9fa-34a19b431441" />

En aquesta pantalla, podrem visualitzar el detall dels canvis que s'han produït des que es va crear el punt de restauració. Un cop revisada la informació, farem clic a "Next" (Següent).

<img width="1560" height="611" alt="Captura de pantalla de 2025-10-07 13-04-33" src="https://github.com/user-attachments/assets/b5df97fe-15a1-4dcb-8736-fe2987732e79" />

Ara cal esperar que el procés de restauració finalitzi. Un cop s'hagi completat, podrem verificar que els fitxers que havien estat eliminats s'han recuperat i tornen a ser al seu lloc original.

# Gestor de paquets
La gestió i instal·lació de paquets de programari es durà a terme mitjançant diverses eines del sistema, com ara dpkg, apt i aptitude, a més de la instal·lació directa des de repositoris.

## DPKG 

Dpkg és la utilitat fonamental per a la gestió de paquets en sistemes operatius basats en Debian (com és el cas d'Ubuntu). Aquesta eina permet realitzar operacions com instal·lar, suprimir i administrar fitxers de paquet .deb de manera local, sense requerir una connexió a Internet activa.

Ordres principals de dpkg:

- Instal·lar un paquet: sudo dpkg -i paquet.deb

- Eliminar un paquet (desinstal·la): sudo dpkg -r paquet

- Purgar un paquet (desinstal·la i esborra configuració): sudo dpkg -P paquet

- Mostrar l'estat del paquet: dpkg -s paquet

### Instal·lació amb DPKG

Per a fer la instal·lació amb dpkg farem servir el joc pacman.

<img width="808" height="266" alt="instal·lacio" src="https://github.com/user-attachments/assets/38298625-6cde-487f-92b9-b2837d9b7b46" />

### Executar el joc per veure que funciona

<img width="670" height="502" alt="joc" src="https://github.com/user-attachments/assets/ae4cd174-b7aa-4ec7-b121-24e503d8a5d0" />

### Eliminar un paquet amb DPKG i obrir-lo per veure que ja no existeix

<img width="670" height="502" alt="Desinstal·lacio i obrir error" src="https://github.com/user-attachments/assets/c8231511-fc68-41dc-aa84-2fa4d30083a0" />

## APT

Apt és una potent eina de línia d'ordres per a sistemes derivats de Debian (com Ubuntu), dissenyada per simplificar la gestió del programari. A diferència de dpkg, apt gestiona automàticament les dependències, la qual cosa facilita enormement la instal·lació i actualització de paquets des de repositoris en línia.

Ordres fonamentals d'apt:

- Instal·lar un paquet: sudo apt install paquet

- Eliminar un paquet (conservant fitxers de configuració): sudo apt remove paquet

- Purgar un paquet (eliminant també la configuració): sudo apt purge paquet

### Instal·lació amb APT

<img width="808" height="362" alt="image" src="https://github.com/user-attachments/assets/023f7c34-fc8a-465d-b823-ecbadca94870" />

Per tal de verificar que la instal·lació s'ha completat amb èxit, executarem la comanda vlc a la terminal. Atès que el paquet que hem descarregat correspon a aquest programa, l'aplicació hauria d'iniciar-se.

<img width="818" height="577" alt="image" src="https://github.com/user-attachments/assets/50431186-0864-44fa-88e8-dea060df294f" />

### Eliminar un paquet amb APT

<img width="795" height="309" alt="image" src="https://github.com/user-attachments/assets/94db893e-5057-49d2-b053-4cbdba987870" />

Ara, utilitzarem l'ordre sudo apt remove [paquet] per desinstal·lar-lo. Si intentem executar el programa un cop eliminat, el sistema ens indicarà que l'ordre no es troba, verificant així que el paquet principal s'ha suprimit correctament.

No obstant això, és probable que les dependències que es van instal·lar amb ell (altres paquets que ja no són necessaris) encara romanguin al sistema. Per efectuar una neteja completa d'aquests paquets "orfes", s'ha d'executar l'ordre sudo apt autoremove.

<img width="811" height="463" alt="image" src="https://github.com/user-attachments/assets/0a51dd66-70d7-417b-a43c-bd6cac93a283" />

### Purgar un paquet amb APT

<img width="802" height="117" alt="image" src="https://github.com/user-attachments/assets/72eab821-5cd3-4e3e-978e-e6258ff25922" />

Aquesta ordre s'utilitza per a una eliminació completa, suprimint no només el paquet sinó també tots els seus fitxers de configuració associats al sistema.

## APTITUDE

Aptitude funciona com un frontend o una interfície d'alt nivell per al sistema de gestió de paquets APT, utilitzat en sistemes basats en Debian i Ubuntu.

La seva funció principal és proveir l'usuari d'un mètode interactiu i eficient per explorar, instal·lar, actualitzar i suprimir programari, destacant sovint per la seva gestió avançada de dependències.

Ordres bàsiques d'aptitude:

- Instal·lar un paquet: sudo aptitude install paquet

- Eliminar un paquet (conservant configuració): sudo aptitude remove paquet

- Purgar un paquet (eliminant configuració): sudo aptitude purge paquet

Per poder utilitzar aptitude, primer és necessari procedir amb la seva instal·lació al nostre sistema operatiu, ja que no sempre ve inclòs per defecte.

<img width="804" height="354" alt="Captura de pantalla de 2025-10-24 18-31-14" src="https://github.com/user-attachments/assets/3771d613-353e-4587-829a-f9a4c0f48e2a" />

### Instal·lació amb aptitude

<img width="806" height="531" alt="image" src="https://github.com/user-attachments/assets/c6c314df-e721-4eea-a20e-cff4d5903496" />

<img width="805" height="575" alt="image" src="https://github.com/user-attachments/assets/c6d58a2f-dc54-4a8b-88b5-31d40322bf36" />

Per tal de verificar que la instal·lació s'ha completat amb èxit, executarem la comanda vlc a la terminal. Atès que el paquet que hem descarregat correspon a aquest programa, l'aplicació hauria d'iniciar-se.

### Eliminar un paquet amb aptitude

<img width="805" height="575" alt="image" src="https://github.com/user-attachments/assets/f935da31-6650-4878-ad68-cd6b766a552e" />

<img width="811" height="138" alt="image" src="https://github.com/user-attachments/assets/568b43d8-c67a-4912-9422-0e0d952588f1" />

Procedim a desinstal·lar el paquet amb aptitude. En intentar executar el programa, podrem verificar que ha estat eliminat correctament. Cal destacar que, a diferència d'apt, aptitude realitza una gestió de dependències més proactiva: en eliminar un paquet, també suprimeix automàticament les dependències que es van instal·lar amb ell i que ja no tenen ús, fent innecessària l'execució posterior d'una ordre autoremove.

### Purgar un paquet amb aptitude

<img width="811" height="138" alt="image" src="https://github.com/user-attachments/assets/3428c2d1-e047-40d0-aad8-b9d3a3c5b62d" />

Aquesta comanda (purge) s'utilitza per a una eliminació exhaustiva: suprimeix el paquet i, a diferència de remove, també elimina els seus fitxers de configuració del sistema. Encara que el resultat immediat és idèntic (l'aplicació ja no es podrà obrir), purge realitza una neteja més profunda.

## Repositoris PPA

Pot ocórrer que un paquet de programari no estigui disponible als repositoris estàndard del sistema, o que la versió que s'hi troba no sigui la més recent. En aquestes circumstàncies, tenim l'opció d'incorporar un repositori de programari extern, conegut habitualment com a PPA (Personal Package Archive).

Per il·lustrar aquest mètode, el paquet que seleccionarem serà VLC.

Instal·lació de VLC des d'un repositori PPA

Inicialment, cal cercar a Internet el PPA corresponent al paquet desitjat. Una estratègia de cerca efectiva és utilitzar el nom del programari (p. ex., "VLC") seguit dels termes "repositori PPA", ja que això facilita la localització de les instruccions correctes.

<img width="818" height="533" alt="image" src="https://github.com/user-attachments/assets/43ccce95-b99e-425f-bc45-9eaa6b9ea54a" />

<img width="819" height="516" alt="image" src="https://github.com/user-attachments/assets/7c7428c4-6350-4673-b8c9-dd62221bdfa4" /> 

<img width="813" height="539" alt="image" src="https://github.com/user-attachments/assets/3a4bc6b5-72da-4bb6-a622-21aca54bea8c" />

Finalment, executem una actualització completa de la distribució (dist-upgrade) per assegurar-nos que totes les dependències essencials del sistema s'actualitzen a les seves versions més recents.

<img width="813" height="539" alt="image" src="https://github.com/user-attachments/assets/2f4833e7-c00a-4200-8592-ade503294487" />

Com a darrera verificació, procedim a iniciar el programa per assegurar-nos de la seva correcta funcionalitat.

<img width="813" height="539" alt="image" src="https://github.com/user-attachments/assets/e4666b25-c3f7-4641-a313-7ace4f94a282" />

# Configuració de la xarxa
Inicialment, és necessari verificar l'adreça IP actual del sistema mitjançant l'execució de la comanda ip a. Un cop obtinguda aquesta informació, s'ha d'accedir al panell de configuració de la xarxa per establir de forma manual els paràmetres de l'adreça IP, la màscara de subxarxa i la porta d'enllaç (gateway).

<img width="1221" height="464" alt="Captura de pantalla de 2025-10-07 13-44-44" src="https://github.com/user-attachments/assets/5ea80d2d-54c8-4ff2-b594-99d4ceee3d47" />

Per verificar que la configuració de xarxa és correcta i tenim sortida a Internet, realitzarem una prova de connectivitat. Executarem una comanda ping a l'adreça 8.8.8.8 i, posteriorment, al domini google.es.

<img width="1211" height="530" alt="Captura de pantalla de 2025-10-07 13-45-54" src="https://github.com/user-attachments/assets/fd4c3bcb-8415-48f5-8323-3271cdf382ca" />

Ara, per fer aquesta configuració persistent, és necessari modificar el fitxer de configuració de netplan.

Per editar aquest arxiu, executarem la següent ordre, prement la tecla TAB on s'indica per autocompletar el nom del fitxer .yaml, ja que aquest pot variar:

nano /etc/netplan/ (prémer TAB)

Un cop a l'editor, afegirem els paràmetres corresponents a la configuració manual (adreça IP, porta d'enllaç, etc.).

<img width="1211" height="530" alt="Captura de pantalla de 2025-10-07 13-54-50" src="https://github.com/user-attachments/assets/e7999f1e-0184-4a03-a8fc-c76388345af4" />

Per aplicar els canvis de configuració emmagatzemats, executarem la comanda netplan apply. Immediatament després, realitzarem una comprovació de connectivitat fent ping a 8.8.8.8 i a google.es per verificar que els canvis han estat exitosos.

<img width="1211" height="530" alt="Captura de pantalla de 2025-10-07 13-55-12" src="https://github.com/user-attachments/assets/73155739-4aec-48da-85f1-9b5fd2af6b5a" />
