# Rechnernetze

## 4.1 IP/ICMP analysis
#### 4.1.1 Node configuration

```
Windows-IP-Konfiguration


Ethernet-Adapter IPV4-priv:

   Verbindungsspezifisches DNS-Suffix:
   IPv4-Adresse  . . . . . . . . . . : 192.168.31.15
   Subnetzmaske  . . . . . . . . . . : 255.255.255.0
   Standardgateway . . . . . . . . . :

Ethernet-Adapter IPV4-pub:

   Verbindungsspezifisches DNS-Suffix: rznt.rzdir.fht-esslingen.de
   Verbindungslokale IPv6-Adresse  . : fe80::7d3d:f0b9:bbff:474c%11
   IPv4-Adresse  . . . . . . . . . . : 134.108.8.54
   Subnetzmaske  . . . . . . . . . . : 255.255.252.0
   Standardgateway . . . . . . . . . : 134.108.11.254

```

#### 4.1.2 Subnet internal IP destination

##### a)

Mit einem Ping paket mit der länge 64 Bytes kommt folgendes zu Stande:
```
Frame 1: 106 bytes on wire (848 bits), 106 bytes captured (848 bits)
Ethernet II, Src: 00:0a:f7:0f:67:02, Dst: 00:0a:f7:0f:68:b2
Internet Protocol Version 4, Src: 192.168.31.15, Dst: 192.168.31.16
    0100 .... = Version: 4
    .... 0101 = Header Length: 20 bytes (5)
    Differentiated Services Field: 0x00 (DSCP: CS0, ECN: Not-ECT)
    Total Length: 92
    Identification: 0x01a0 (416)
    Flags: 0x00
    Fragment offset: 0
    Time to live: 128
    Protocol: ICMP (1)
    Header checksum: 0x0000 [validation disabled]
    [Header checksum status: Unverified]
    Source: 192.168.31.15
    Destination: 192.168.31.16
Internet Control Message Protocol

Frame 2: 106 bytes on wire (848 bits), 106 bytes captured (848 bits)
Ethernet II, Src: 00:0a:f7:0f:68:b2, Dst: 00:0a:f7:0f:67:02
Internet Protocol Version 4, Src: 192.168.31.16, Dst: 192.168.31.15
    0100 .... = Version: 4
    .... 0101 = Header Length: 20 bytes (5)
    Differentiated Services Field: 0x00 (DSCP: CS0, ECN: Not-ECT)
    Total Length: 92
    Identification: 0x00e8 (232)
    Flags: 0x00
    Fragment offset: 0
    Time to live: 128
    Protocol: ICMP (1)
    Header checksum: 0x7a49 [validation disabled]
    [Header checksum status: Unverified]
    Source: 192.168.31.16
    Destination: 192.168.31.15
Internet Control Message Protocol
```

##### b)
Mit einem fragmentierten Ping paket mit der länge 2000 Bytes kommt folgendes zu Stande:
```
Frame 111: 1514 bytes on wire (12112 bits), 1514 bytes captured (12112 bits) on interface 0
Ethernet II, Src: 00:0a:f7:0f:67:02, Dst: 00:0a:f7:0f:68:b2
Internet Protocol Version 4, Src: 192.168.31.15, Dst: 192.168.31.16
    0100 .... = Version: 4
    .... 0101 = Header Length: 20 bytes (5)
    Differentiated Services Field: 0x00 (DSCP: CS0, ECN: Not-ECT)
    Total Length: 1500
    Identification: 0x01c8 (456)
    Flags: 0x01 (More Fragments)
        0... .... = Reserved bit: Not set
        .0.. .... = Don't fragment: Not set
        ..1. .... = More fragments: Set
    Fragment offset: 0
    Time to live: 128
    Protocol: ICMP (1)
    Header checksum: 0x0000 [validation disabled]
    [Header checksum status: Unverified]
    Source: 192.168.31.15
    Destination: 192.168.31.16
Internet Control Message Protocol

Frame 112: 562 bytes on wire (4496 bits), 562 bytes captured (4496 bits) on interface 0
Ethernet II, Src: 00:0a:f7:0f:67:02, Dst: 00:0a:f7:0f:68:b2
Internet Protocol Version 4, Src: 192.168.31.15, Dst: 192.168.31.16
    0100 .... = Version: 4
    .... 0101 = Header Length: 20 bytes (5)
    Differentiated Services Field: 0x00 (DSCP: CS0, ECN: Not-ECT)
    Total Length: 548
    Identification: 0x01c8 (456)
    Flags: 0x00
        0... .... = Reserved bit: Not set
        .0.. .... = Don't fragment: Not set
        ..0. .... = More fragments: Not set
    Fragment offset: 1480
    Time to live: 128
    Protocol: ICMP (1)
    Header checksum: 0x0000 [validation disabled]
    [Header checksum status: Unverified]
    Source: 192.168.31.15
    Destination: 192.168.31.16
Data (528 bytes)
```

### c)
Wireshark hat nichts angezeigt da das Paket zwar ausgeführt wurde aber der -f Flag gesetzt wurde. Dadurch wurde das Paket nicht übermittelt da es zu groß war.
```
C:\Users\rn-labor>ping -n 1 -l 2000 -f 192.168.31.16

Ping wird ausgeführt für 192.168.31.16 mit 2000 Bytes Daten:
Paket müsste fragmentiert werden, DF-Flag ist jedoch gesetzt.

Ping-Statistik für 192.168.31.16:
    Pakete: Gesendet = 1, Empfangen = 0, Verloren = 1
    (100% Verlust)
```

#### 4.1.3 Subnet external IP destination

### d)
Ping ist erfolgreich, da nur ein Router zwischen den beiden Rechnern liegt.
```
Ping wird ausgeführt für 134.108.190.10 mit 32 Bytes Daten:
Antwort von 134.108.190.10: Bytes=32 Zeit=3ms TTL=63
    Route: 134.108.190.14 ->
           134.108.190.10 ->
           134.108.190.10 ->
           134.108.11.254

Ping-Statistik für 134.108.190.10:
    Pakete: Gesendet = 1, Empfangen = 1, Verloren = 0
    (0% Verlust),
Ca. Zeitangaben in Millisek.:
    Minimum = 3ms, Maximum = 3ms, Mittelwert = 3ms
```

### e)
Time to live wurde hierbei überschritten.
```
Ping wird ausgeführt für 134.108.190.10 mit 32 Bytes Daten:
Antwort von 134.108.11.254: Die Gültigkeitsdauer wurde bei der Übertragung überschritten.

Ping-Statistik für 134.108.190.10:
    Pakete: Gesendet = 1, Empfangen = 1, Verloren = 0
    (0% Verlust),
```

### f)

```
Ping wird ausgeführt für 134.108.190.10 mit 32 Bytes Daten:
Antwort von 134.108.190.10: Bytes=32 Zeit=1ms TTL=63
    Zeitstempel: 134.108.190.14 : 50579032 ->
               134.108.190.10 : 50579031 ->
               134.108.190.10 : 50579031 ->
               134.108.11.254 : 50579033

Ping-Statistik für 134.108.190.10:
    Pakete: Gesendet = 1, Empfangen = 1, Verloren = 0
    (0% Verlust),
Ca. Zeitangaben in Millisek.:
    Minimum = 1ms, Maximum = 1ms, Mittelwert = 1ms
```

#### Gerneral Questions

Do you identify the existence of any LAN repeaters or LAN switches in your Wireshark trace ?
* Nein, Wireshark erkennt keine Repeater auf Mac-Adressen-Ebene.

Does IP know about (or not) ?
* Nein, weil wir auf der Mac-Ebene sind.

## 4.2 ARP analysis

Nachdem wir den ARP cache gelöscht haben und die IP 192.168.31.16 angepingt haben.

```
Schnittstelle: 134.108.8.54 --- 0xb
  Internetadresse       Physische Adresse     Typ
  134.108.11.254        00-23-04-52-1c-00     dynamisch

Schnittstelle: 192.168.31.15 --- 0xd
  Internetadresse       Physische Adresse     Typ
  192.168.31.16         00-0a-f7-0f-68-b2     dynamisch

```

#### a)
How many Ethernet frames are transmitted ?
*  Es werden 4 Frames übertragen.

Briefly explain the purpose of each message.
* Bei der übertragung wird zwischen Request und Repy abgewechselt.

```
Ping wird ausgeführt für 192.168.31.16 mit 32 Bytes Daten:
Antwort von 192.168.31.16: Bytes=32 Zeit<1ms TTL=128
Antwort von 192.168.31.16: Bytes=32 Zeit<1ms TTL=128

Ping-Statistik für 192.168.31.16:
    Pakete: Gesendet = 2, Empfangen = 2, Verloren = 0
    (0% Verlust),
Ca. Zeitangaben in Millisek.:
    Minimum = 0ms, Maximum = 0ms, Mittelwert = 0ms

```

#### b)
Hier werden 4 ICMP Frames übertragen. Da die MAC-Adressen noch unbekannt sind werden auch 4 ARP Nachrichten verschickt. Request und Reply wechseln sich hier auch ab.
```
Ping wird ausgeführt für 192.168.31.12 mit 32 Bytes Daten:
Antwort von 192.168.31.12: Bytes=32 Zeit<1ms TTL=128
Antwort von 192.168.31.12: Bytes=32 Zeit<1ms TTL=128

Ping-Statistik für 192.168.31.12:
    Pakete: Gesendet = 2, Empfangen = 2, Verloren = 0
    (0% Verlust),
Ca. Zeitangaben in Millisek.:
    Minimum = 0ms, Maximum = 0ms, Mittelwert = 0ms
```

## 4.3 IP MULTICAST ADDRESSING

```
Frame 32: 216 bytes on wire (1728 bits), 216 bytes captured (1728 bits) on interface 0
Ethernet II, Src: 90:1b:0e:a6:1c:dd, Dst: 01:00:5e:7f:ff:fa
    Destination: 01:00:5e:7f:ff:fa
        Address: 01:00:5e:7f:ff:fa
        .... ..0. .... .... .... .... = LG bit: Globally unique address (factory default)
        .... ...1 .... .... .... .... = IG bit: Group address (multicast/broadcast)
    Source: 90:1b:0e:a6:1c:dd
        Address: 90:1b:0e:a6:1c:dd
        .... ..0. .... .... .... .... = LG bit: Globally unique address (factory default)
        .... ...0 .... .... .... .... = IG bit: Individual address (unicast)
    Type: IPv4 (0x0800)
Internet Protocol Version 4
User Datagram Protocol
Simple Service Discovery Protocol
```
## 4.4 IP TUNNELING 

Trotz mehrfachen Versuchen konnten wir mit Wireshark keine Pakete aufzeichnen obwohl das Pingen zwischen den beiden Virtuellen Maschienen ohne Verlust erfolgreich durchgeführt wurde.




