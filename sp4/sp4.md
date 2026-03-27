---
layout: default
title: "Sprint 4: Configuració del Programari de Base i Sistemes d’Emmagatzematge en Ubuntu"
---

# RAIDS

## Què és un RAID?

L'acrònim **RAID** (*Redundant Array of Independent Disks*) fa referència a un mètode de virtualització de dades. La seva funció principal és unificar la capacitat de diversos discos durs físics perquè el sistema els gestioni i reconegui com una sola unitat lògica (o un conjunt d'elles).

## Objectius i Avantatges del RAID

Implementar una arquitectura RAID aporta tres grans beneficis al nostre sistema:

- **Tolerància a errors (Redundància):** Garanteix la integritat de la informació encara que pateixi una avaria física o lògica algun dels discs. Si una unitat cau, el sistema es manté operatiu i permet reconstruir les dades en substituir el maquinari afectat.
- **Optimització del rendiment:** En distribuir les operacions de lectura i escriptura de manera simultània (en paral·lel) entre diverses unitats, s'aconsegueix un increment molt notable en la velocitat de transferència.
- **Suma de capacitats:** Facilita l'agrupació de diversos discs de menor mida per conformar un únic bloc d'emmagatzematge de gran volum.

## Nivells RAID i el nostre entorn de proves

Existeixen diverses maneres d'estructurar un RAID, conegudes com a "nivells", que ofereixen diferents compromisos entre seguretat, espai i velocitat. Per a les proves d'aquest repositori, ens centrarem exclusivament en el **RAID 1**. Si vols aprofundir en la resta de configuracions, pots consultar [la documentació detallada sobre els nivells RAID](https://es.wikipedia.org/wiki/RAID).

### RAID 1 (Mode Mirall o Mirroring)

- **Mecanisme d'acció:** Clona la informació en temps real. Qualsevol dada que es desa en el disc principal es replica automàticament en el disc secundari.
- **Punts forts:** Proporciona una alta fiabilitat i redundància. Davant la caiguda d'un disc, el sistema continua treballant amb la còpia de forma completament transparent. A més, permet obtenir bons temps de lectura.
- **Inconvenients:** Es desaprofita el 50% de la capacitat d'emmagatzematge (dos discs d'1 TB en RAID 1 només oferiran 1 TB d'espai útil). D'altra banda, la velocitat d'escriptura no millora respecte a la d'un disc individual.
- **Requisits de maquinari:** Calen, com a mínim, 2 unitats de disc.

---

## Conceptes clau: Emmagatzematge i Volums

Per comprendre bé l'arquitectura de dades, és vital distingir com el sistema operatiu i el maquinari tracten l'espai:

- **Disc Físic:** És el component tangible, la peça de maquinari real que instal·lem a l'equip (ja sigui un HDD tradicional o un SSD).
- **Partició:** Correspon a una fragmentació o divisió lògica dins d'un mateix disc físic. A la pràctica, permet trossejar una unitat en diferents blocs aïllats.
- **Volum:** És la unitat d'emmagatzematge final que el sistema operatiu munta i posa a disposició de l'usuari. Disposa del seu propi sistema de fitxers (com pot ser ext4, NTFS, APFS o exFAT) i s'identifica com un espai funcional i accessible (per exemple, les rutes de muntatge a Linux o les unitats `C:` i `D:` a Windows).

### Interacció entre Volums i configuracions RAID

Cal destacar que un volum no té per què estar limitat a una única unitat física d'emmagatzematge. Gràcies a l'ús de controladors (ja sigui mitjançant maquinari o programari), es pot generar un Volum Lògic que s'estengui a través de múltiples discos físics. 

Per posar un exemple clar: si configurem un RAID 5 utilitzant quatre discos físics, el sistema operatiu no detectarà quatre unitats independents. En el seu lloc, identificarà un únic espai d'emmagatzematge unificat i de gran capacitat, a punt per donar-li format i començar a desar-hi fitxers.

# Cas Pràctic: Implementació d'un RAID 1

Instalem primerament el paquet `mdadm`.

<img width="735" height="440" alt="2026-03-26_13-01" src="https://github.com/user-attachments/assets/4af70a59-d53b-4a70-91c6-3621c0e6902e" />

Com a pas previ abans d'engegar la màquina virtual, li hem assignat dos discos durs nous, tots dos amb una capacitat de 2 GB.

<img width="765" height="219" alt="2026-03-26_13-02" src="https://github.com/user-attachments/assets/e9d59313-fe95-4f8a-b3b2-9c0fdc283c9f" />

A continuació, procedim a particionar les dues unitats que acabem d'afegir.

<img width="752" height="704" alt="2026-03-26_13-05" src="https://github.com/user-attachments/assets/bc70c8fd-f439-4137-8ac2-5848969c5e69" />

<img width="1023" height="507" alt="2026-03-26_13-06" src="https://github.com/user-attachments/assets/1d1f143c-1809-40ae-878d-1a982f6f7554" />

Un cop completat el procés, la distribució de les particions es veuria d'aquesta manera:

<img width="699" height="414" alt="2026-03-26_13-07" src="https://github.com/user-attachments/assets/fa6ff6b2-3379-4b74-a113-693cd1425f9f" />

El següent pas és generar el directori que farà de punt de muntatge per al nostre RAID.

<img width="582" height="231" alt="2026-03-26_13-08" src="https://github.com/user-attachments/assets/646b6b20-21ba-43c2-a469-686ff3971698" />

Creem el raid.

<img width="864" height="206" alt="2026-03-26_13-10" src="https://github.com/user-attachments/assets/e9f85ab8-7d93-4192-a311-2bae4151bb8b" />

--create /dev/md0: Especifica la creació del nou dispositiu d'emmagatzematge combinat.

--level=1: Determina l'arquitectura del RAID que s'aplicarà (en aquest cas, mode mirall).

--raid-devices=2 /dev/sdb1 /dev/sdc1: Defineix el nombre total d'unitats implicades i indica les rutes exactes de les particions que el conformaran.

Un cop construït l'arranjament, el següent pas és donar-li format utilitzant el sistema de fitxers ext4.

<img width="682" height="254" alt="2026-03-26_13-11" src="https://github.com/user-attachments/assets/8a538872-303b-497a-adf2-22c38c39d3e3" />

Mitjançant aquesta instrucció, comprovarem l'estat actual de l'arranjament RAID i quines unitats físiques el componen.

<img width="724" height="536" alt="2026-03-26_13-12" src="https://github.com/user-attachments/assets/6c089d68-ee4b-466d-8cc1-f0a53aabc829" />

A continuació, bolquem el resultat obtingut d'aquesta primera instrucció directament dins del fitxer de configuració mdadm.conf.

<img width="803" height="46" alt="2026-03-26_13-13" src="https://github.com/user-attachments/assets/ea7264cf-f6f3-462f-b911-452c04a30c74" />

<img width="678" height="43" alt="2026-03-26_13-14" src="https://github.com/user-attachments/assets/5686a857-0243-44cb-a6ab-bb014f8b639a" />

A continuació, editem l'arxiu amb nano i hi incloem la línia següent per registrar els dispositius que acabem de configurar.

<img width="863" height="128" alt="2026-03-26_13-15" src="https://github.com/user-attachments/assets/21471dba-579a-4f23-a782-2f14d770238b" />

Per garantir que el punt de muntatge situat a /mnt/ es mantingui operatiu després de reiniciar l'equip, hem d'enregistrar la unitat dins del fitxer /etc/fstab.

<img width="896" height="312" alt="2026-03-26_13-16" src="https://github.com/user-attachments/assets/d5264a1b-ebb0-49af-baa2-2c8db031abcd" />

Ara montem i actualitzem el kernel.

<img width="722" height="125" alt="2026-03-26_13-17" src="https://github.com/user-attachments/assets/2fbed8e0-9935-49db-acc8-b6bb34f6808c" />

Un cop reiniciat el sistema, executem la instrucció següent per comprovar la persistència de les unitats; si els discos apareixen llistats, la configuració del RAID ha estat un èxit.

<img width="807" height="566" alt="2026-03-26_13-23" src="https://github.com/user-attachments/assets/76fb393a-3b14-4cb5-b0ab-a6ba45d8ad6c" />

## Prova funcionament RAID 1

Dins del punt de muntatge del RAID 1 que acabem de configurar, procedirem a generar l'estructura de carpetes i fitxers següent.

<img width="448" height="131" alt="2026-03-26_13-28" src="https://github.com/user-attachments/assets/d6a2dc95-88de-4abe-94fb-184160628910" />

Ara fem fallar el disco i el traem.

<img width="562" height="65" alt="2026-03-26_13-29" src="https://github.com/user-attachments/assets/583b16e3-5230-4d70-8a8a-c0badacc7220" />

Ara mirem i veem que ja està tret i que només funciona un.

<img width="682" height="554" alt="2026-03-26_13-29_1" src="https://github.com/user-attachments/assets/0b8ba603-91b2-4f84-a32d-9199254f0dda" />

Malgrat que una de les unitats s'hagi extret, el sistema ens ha de permetre continuar operant amb normalitat i generar nous fitxers dins del directori.

<img width="505" height="196" alt="2026-03-26_13-31" src="https://github.com/user-attachments/assets/758005a9-14f8-498a-afe7-bb8c46f6db45" />

Ara tornem a afegir el disc.

<img width="1121" height="405" alt="2026-03-26_13-44" src="https://github.com/user-attachments/assets/6d68123b-8516-4693-b0f9-9904ff829699" />

Ara que l'hem afegit, podem veure que ja està actiu.

<img width="629" height="543" alt="2026-03-26_13-44_1" src="https://github.com/user-attachments/assets/f8cdbb54-2b93-4557-9a53-00803e420e00" />

<img width="717" height="512" alt="2026-03-26_13-45" src="https://github.com/user-attachments/assets/913eb3d1-3332-44af-8b6d-f02133fd0bf8" />

Ara si tornem a entrar al directori que hem fet de /raid1, veem que segueix los directoris i fitxers que hem afegit abans quan només teniem 1 disc.

<img width="568" height="209" alt="image" src="https://github.com/user-attachments/assets/614fcdc3-23e0-406d-b433-7a831ef16cb9" />

