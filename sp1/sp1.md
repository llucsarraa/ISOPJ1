---
layout: default
title: SP1. Instal·lació, Configuració Inicial i Programari de Base
---

## Virtualitzacio i instal·lació del SO Ubuntu
# Instal·lació d’una màquina virtual amb Ubuntu

Per començar la instal·lació vaig obrir l'aplicació VirtualBox per a crear una nova màquina virtual. Li vaig posar el nom **UbuntuSP1**, vaig seleccionar el sistema operatiu **Ubuntu (64-bit)** i vaig carregar la imatge ISO corresponent (`ubuntu-22.04.3-desktop-amd64.iso`).  

<img width="1920" height="1080" alt="inici dels noms de la MV" src="https://github.com/user-attachments/assets/ba101a60-3b4a-4564-89a7-2e2f7acccc14" />

Un cop definits aquests paràmetres inicials, vaig passar a assignar els recursos de la màquina. Li vaig donar **4096 MB de memòria RAM** i **3 processadors**, suficient per poder treballar de manera fluida.  
<img width="1920" height="1080" alt="configuracio abans d'arrancar la MV" src="https://github.com/user-attachments/assets/3074f227-9cb7-4368-a07c-a35ce4828648" />

Després vaig configurar el disc dur virtual. Vaig crear un disc de **80 GB** de tipus dinàmic, de manera que només ocupa l’espai real que utilitza el sistema.  
![Paràmetres finals de VirtualBox](Quan%20acabes%20dels%20parametres%20de%20VirtualBox%20%22finish%22.png)

Amb la màquina configurada, vaig iniciar la instal·lació d’Ubuntu. Durant aquest procés es mostra un resum de les dades de la màquina virtual i, tot seguit, comença el particionament del disc.  
![Dades de la MV](MV%20amb%20les%20dades.png)

Per al particionament vaig decidir fer un **dual boot**, repartint els **80 GB totals en dues parts**: **40 GB per a Ubuntu** i **40 GB lliures** per a futurs usos o instal·lacions.  
![Dual boot](dual%20boot%20que%20es%20lo%20de%20la%20reparticio.png)  
![Repartició d'espais](Reparticio%20d'espais.png)

Durant aquest pas va sorgir un avís: el sistema em demanava crear una petita partició d’uns **1 MB reservada per al BIOS boot area**. Aquesta partició és necessària perquè el carregador d’arrencada funcioni correctament. Vaig afegir-la per resoldre el problema.  
![Problema Reserved BIOS boot area](Problema%20reserved%20BIOS%20boot%20area.png)

Finalment, la taula de particions va quedar de la següent manera:  
- `/` → 12 GB  
- `/home` → 25 GB  
- `/boot` → 1 GB  
- `swap` → 2 GB  
- `efi` → 399 MB  

![Repartició final](Reparticio%20d'espais.png)

Per acabar, vaig haver d’executar una comanda des del terminal relacionada amb els mòduls de VirtualBox, concretament:  

## Llicènciament
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

## Gestors d'arrancada per a instal·lacions DUALS
## Punts de restrauració
## Configuració de la xarxa
## Comandes generals i instal·lacions
