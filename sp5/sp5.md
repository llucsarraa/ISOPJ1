---
layout: default
title: "Sprint 5: Monitoratge, Auditories i Programari Client/Servidor"
---

# Monitoratge del sistema

Supervisar el rendiment en Ubuntu implica analitzar i quantificar el consum de recursos de l'equip o servidor en viu. Aquesta pràctica resulta vital per diagnosticar la salut del sistema i anticipar-se a possibles situacions de saturació.

<img width="447" height="257" alt="image" src="https://github.com/user-attachments/assets/cfedd6e3-25fd-4e17-9f77-0ba32b89ecc0" />

En executar l'aplicació, es visualitzen tots els processos actius en el sistema. Tal com vam practicar en entregues anteriors, aquest entorn gràfic ofereix una funcionalitat equivalent a la d'eines de terminal com htop, etop o btop.

<img width="1190" height="770" alt="image" src="https://github.com/user-attachments/assets/1ced7a4c-1f80-4472-bbc5-6a1dc0bc394c" />

Com s'observa a la interfície, tenim la possibilitat de finalitzar processos, tancar-los o ajustar paràmetres com l'afinitat de la CPU i la prioritat d'execució. Aquestes operacions ja s'han tractat detalladament en seccions prèvies.

<img width="440" height="373" alt="image" src="https://github.com/user-attachments/assets/7e29efbc-5c00-47af-899d-90ee972962ff" />

<img width="791" height="261" alt="image" src="https://github.com/user-attachments/assets/e8723348-bcfc-4945-95c6-44ec455d3347" />

Així mateix, tal com s'ha esmentat prèviament, disposem d'una visió global del rendiment de tots els recursos de la màquina. Els indicadors principals són:

CPU: Reflecteix el volum de treball que està processant la unitat central. Si el percentatge d'ús es manté constantment al màxim, el sistema perdrà fluïdesa en no poder gestionar totes les tasques de manera simultània.

Memòria RAM: Representa l'entorn on s'executen les aplicacions actives. En cas d'esgotar-se la memòria física, Ubuntu utilitzarà l'espai d'intercanvi (Swap) al disc dur; aquest recurs d'emergència provoca una caiguda dràstica en el rendiment general de l'equip.

Xarxa: Monitoritza el flux de dades (entrada i sortida) del dispositiu, a més de supervisar l'estat de les connexions vigents i els ports que romanen oberts.

Emmagatzematge (Disc): Supervisa tant l'ocupació de l'espai disponible com la taxa de transferència en les operacions de lectura i escriptura. Una activitat excessiva del disc pot generar colls d'ampolla que afectin la velocitat del sistema.

<img width="1201" height="756" alt="image" src="https://github.com/user-attachments/assets/08d9bdc1-458c-4c6e-a510-10f54ca162e7" />

<img width="1210" height="121" alt="image" src="https://github.com/user-attachments/assets/a5208d73-cb41-4bb0-8110-b891ea2cdd2a" />

# Logs - Lluc i Manu

Per consultar l'historial d'esdeveniments, visualitzarem el contingut de l'arxiu syslog mitjançant la comanda cat, cosa que ens permetrà revisar tots els registres del sistema.

<img width="1200" height="668" alt="2026-03-05_13-24" src="https://github.com/user-attachments/assets/86dcd351-9f35-453f-bbf8-4aa489de0097" />

En aquest directori podem presonalitzar la rotació dels logs.

<img width="958" height="70" alt="2026-03-05_13-26" src="https://github.com/user-attachments/assets/2f210a51-698f-4186-b999-d20a5c628fd8" />

Tenim la possibilitat d'editar aquest fitxer per configurar i ajustar els paràmetres de rotació dels registres segons les nostres necessitats.

<img width="500" height="483" alt="2026-03-05_13-26_1" src="https://github.com/user-attachments/assets/c55ecb28-0215-49bc-b41e-71a0ee9e8d4e" />

Aquest fitxer ens indica la ruta de la configuració per defecte dels registres; a continuació, ens hi desplaçarem per realitzar les modificacions oportunes.

<img width="942" height="704" alt="2026-03-05_13-27" src="https://github.com/user-attachments/assets/ad30daea-147c-4dd6-8668-347e7d267f24" />

En primer lloc, realitzarem un test per analitzar l'impacte d'una notificació de correu i identificar en quins fitxers de registre queda reflectida. Mitjançant una simulació, verificarem si l'enviament genera una entrada immediata al syslog i si, posteriorment, es registra a l'arxiu mail.log. Per fer-ho, utilitzarem dues terminals: una per enviar el missatge i l'altra per monitorar el syslog en temps real.

<img width="1209" height="725" alt="2026-03-05_13-30" src="https://github.com/user-attachments/assets/e5e7a95f-c91d-438b-ae25-f5cd938dd785" />

Realitzarem una segona prova modificant la configuració del servei de correu. L'objectiu és restringir el fitxer mail.log perquè només enregistri els missatges de nivell error, descartant tant els nivells inferiors com els superiors. Cal recordar que, si utilitzéssim el comodí *, el sistema tornaria a desar tots els registres sense distinció.

<img width="990" height="638" alt="2026-03-05_13-31" src="https://github.com/user-attachments/assets/5fa4e89e-d0ba-4c6a-b2f4-9a3ee889bbb6" />

Quan el modifiquem, hem de fer un restart al servei.

<img width="621" height="39" alt="2026-03-05_13-36" src="https://github.com/user-attachments/assets/f60fac00-8463-4fb5-ab15-f8f256dc040d" />

En repetir la prova anterior, observem que el registre no s'emmagatzema al fitxer mail.log. Això es deu al fet que la notificació enviada té una prioritat de tipus notice, la qual queda exclosa pel filtre que hem configurat exclusivament per a nivells err.

<img width="1213" height="759" alt="2026-03-05_13-37" src="https://github.com/user-attachments/assets/00e0f8f3-490a-4a97-96c6-ff1065a7d765" />

Finalment, si substituïm la prioritat mail.notice per mail.err, comprovarem que el registre s'emmagatzema correctament a l'arxiu de logs, ja que ara sí que coincideix amb el filtre establert.

<img width="1202" height="452" alt="2026-03-05_13-38" src="https://github.com/user-attachments/assets/7d1ad8e3-ea8d-4f5b-93f2-0e7b155ee077" />

Procedirem a modificar novament el filtratge dels registres de correu, aquest cop eliminant el signe =. Amb aquest canvi, el sistema emmagatzemarà no només les entrades de tipus err, sinó també totes les que tinguin una prioritat superior.

<img width="936" height="724" alt="2026-03-05_13-38_1" src="https://github.com/user-attachments/assets/3c16831d-bce9-4b69-8d52-96117582d730" />

Reiniciem el servei i seguim.

Per validar aquest funcionament, canviarem el nivell d'alerta a crit (criticitat superior) i comprovarem que el sistema el registra correctament al fitxer de logs.

<img width="1211" height="478" alt="2026-03-05_13-40" src="https://github.com/user-attachments/assets/c5259d84-7d40-492b-887f-e217ec5990e9" />

És possible definir una ruta personalitzada per emmagatzemar els registres que considerem més rellevants. En aquest cas, configurarem el sistema per capturar totes les entrades de tipus crit, hi indicarem el directori de destinació desitjat i, finalment, reiniciarem el servei per aplicar els canvis.

<img width="1215" height="773" alt="2026-03-05_13-41" src="https://github.com/user-attachments/assets/8e534e00-cf2c-49a0-805b-c96f1645aea5" />

En enviar una notificació de tipus cron.crit, observem que s'ha generat automàticament un nou fitxer anomenat mireia.log. Aquesta és la ruta específica que havíem definit en el pas de configuració anterior.

<img width="1202" height="386" alt="2026-03-05_13-42" src="https://github.com/user-attachments/assets/21276dd7-e3c6-45a6-8bae-353d2bdbac9c" />

Amb aquesta comanda podem veure tots els logs de tipus crit.

<img width="1205" height="185" alt="2026-03-05_13-43" src="https://github.com/user-attachments/assets/8a125f3b-5893-4e0c-b198-63202aadb3d6" />

Depenent dels paràmetres que hi afegim, podem filtrar les cerques per acotar els resultats. En aquest cas, ens centrarem a consultar els registres de tipus mail que hem generat anteriorment.

<img width="588" height="164" alt="2026-03-05_13-44" src="https://github.com/user-attachments/assets/4e26257b-89d6-4699-ad8d-fbda0a04fbfe" />

## Exercici Logs

Per a aquesta pràctica, configurarem un entorn amb dues màquines Ubuntu: un client encarregat d'enviar els seus registres a la xarxa (mentre els conserva localment) i un servidor que actuarà com a receptor centralitzat de tota la informació.

### Màquina Servidor

En primer lloc, a la màquina servidor (l'encarregada de rebre i emmagatzemar els registres), procedirem a configurar la redirecció. Crearem un nou fitxer de configuració per desviar tots els logs remots cap a una carpeta específica, la qual generarem en aquest mateix pas.

<img width="242" height="53" alt="image" src="https://github.com/user-attachments/assets/7bf3735f-2939-4b30-9602-e52360bdd99b" />

<img width="824" height="37" alt="image" src="https://github.com/user-attachments/assets/98294a48-7cd7-45a5-9033-00c8e3e29e69" />

Dins del fitxer de nova creació, hem d'afegir les línies següents per habilitar la recepció de registres mitjançant els protocols UDP i/o TCP.

<img width="817" height="250" alt="image" src="https://github.com/user-attachments/assets/d725df1f-269e-47c1-b6d8-2c6bee319107" />

Finalment, permitim el pas de tcp i udp al firewall.

<img width="161" height="37" alt="image" src="https://github.com/user-attachments/assets/807548eb-85da-40b5-82a7-cf615db3562d" />

<img width="143" height="36" alt="image" src="https://github.com/user-attachments/assets/5578e789-7d1d-4530-81ae-698d0043523f" />

Un cop completats aquests passos de configuració, hem de reiniciar el servei per tal que el sistema apliqui i activi els nous paràmetres.

<img width="227" height="38" alt="image" src="https://github.com/user-attachments/assets/b4528e88-0f98-4681-8a31-43f75289dc7d" />

### Màquina Client

Dins d'un nou fitxer anomenat 90-forward.conf, hi afegirem l'adreça IP del servidor per indicar cap a on s'han de reenviar els registres.

<img width="664" height="55" alt="image" src="https://github.com/user-attachments/assets/9c49c5da-af25-4343-8901-c4aedc2a23b4" />

Fem un restart de rsyslog.

<img width="635" height="51" alt="image" src="https://github.com/user-attachments/assets/ae7c064b-2cb4-4902-b7b8-21a2ed167f38" />

#### Comprobació logger

Un cop configurat el reenviaments, executarem un logger des del client per generar un missatge de prova i verificar que arriba correctament al servidor.

<img width="485" height="25" alt="image" src="https://github.com/user-attachments/assets/4ecc8852-5202-477f-937b-006d94b390c1" />

Podem comprovar que s'han generat diversos fitxers dins del directori remote. Entre ells, trobem la carpeta corresponent al ClientSP5, confirmant que el servidor ha organitzat correctament els registres rebuts per cada node.

<img width="287" height="66" alt="image" src="https://github.com/user-attachments/assets/535f1f6f-6409-4251-9c39-b85d2d36041d" />

<img width="1215" height="535" alt="image" src="https://github.com/user-attachments/assets/02807902-269b-4fd7-98cb-8bd8fd085885" />




# Servidor d'actualitzacions

## Servidor


## Client

## Exercici

### Servidor


### Client




























