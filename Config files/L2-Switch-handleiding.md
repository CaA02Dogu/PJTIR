# L2-Switch configuratie


## Algemene instellingen
### Hostname instellen

Voeg de naam van het apparaat toe:

	hostname <apparaat naam>
Dit maakt het makkelijker te weten om welk apparaat het gaat.

### Privilege mode login

Maak een wachtwoord aan voor privil3ge mode autenticatie:

	enable password <wachtwoord>

Nu is het bewerken van het apparaat beveiligd met een wachtwoord.

## Port-channels
### Loadbalancing instellen
Voordat wij portchannels gaan instellen moeten we alvast tegen de switch zeggen dat er loadbalancing moet komen. Dit kan gedaan worden met:

	port-channel load-balance src-dst-mac
### Configuratie
Voor een etherchannel verbinding:

	interface range FastEthernet <twee of meer interfaces>
	 switchport mode trunk
	 channel-protocol pagp
	 channel-group <group nummer> mode desirable

	interface Port-channel <group nummer>
	 switchport mode trunk
Hier worden de interfaces eerst in trunk mode gezet voor VLAN gebruik. Daarna wordt PagP ingesteld als etherchannel protocol. Als laatste moet er een channel groep nummer toegewezen worden. Let hier op dat groep nummers niet overlappen met andere etherchannel verbindingen.

\pagebreak

## VLANs
### Configuratie

#### Aanmaken

Het creëeren van een VLAN op een switch gaat als volgt:

	vlan <nummer>
	 name <vlan naam>
Het commando `name` hier is niet verplicht, maar maakt het meer overzichtelijk waar elk VLAN voor bedoeld is. Binnen het PoC wordt er alleen gebruik gemaakt van VLAN 40(Management), 60(IT) en 70(Servers).

#### Toewijzen
Zo kunnen we de poorten een VLAN toewijzen.

    interface range FastEthernet <interface nummer>
	 witchport access vlan <nummer>
     switchport mode access

Dit zorgt ervoor dat het apparaat aan deze port toegelaten wordt op zijn aangewezen VLAN.

## PortFast met BPDU-guard
PortFast wil je instellen op porten die naar een enkel werkstation of server, zodat deze gelijk op het netwerk kunnen zonder dat de switch hoeft te leren wat voor een apparaat het is. Doordat wij portfast gaan gebruiken moeten wij dit wel goed gaan beveiligen want wij willen niet dat er een switch aan een van deze poorten met portfast wordt verbonden (dit veroorzaakt loops). Dit kunnen wij voorkomen door bpdu-guard in te stellen op deze poorten. 

### Configuratie

Zo stel je PortFast met BPDU-guard in:

    interface range FastEthernet <interface nummer>
	    spanning-tree portfast
        spanning-tree bpduguard enable

\pagebreak

## Radius

Radius zorgt ervoor dat devices moeten inloggen op het netwerk

### Configuratie
Globale instellingen voor radius:

    aaa new-model
    aaa authentication dot1x default group radius

Radius instellen op de port zelf:

    interface range FastEthernet <interface nummer>
        access-session port-control auto
        dot1x pae authenticator

## Port security

Port sercurity zorgt ervoor dat niet zo maar een device kan ontkoppeld worden en een ander device zich kan koppelen aan het netwerk

### Congfiguratie

Port securtiy stel je per port zo in:

    interface range FastEthernet <interface nummer>
            switchport port-security 
            switchport port-security maximum 1
            switchport port-security mac-address sticky




