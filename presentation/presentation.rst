:title: SSH
:author: Susanne Kießling
:description: The Hovercraft! tutorial.
:keywords: presentation, backend, impress.js, hovercraft, yubikey
:css: presentation.css

----

.. utility roles

.. role:: underline
    :class: underline

.. role:: blocky
   :class: blocky

.. role:: tiny
   :class: tiny

.. role:: colored
   :class: colored

          
:id: first 

SSH - 
Secure Shell

:tiny:`Susanne Kießling (MIN)`

.. note::

   - note 

----

:blocky:`SSH - Old School?`

.. note::
   - Erste Version des Protokolls von 1995
   - Mächtiges Werkzeug
   - Einfache Anwendung 
   - Lohnt sich näher damit zu beschäftigen !


.. image:: images/confused.png
   :align: center
   :height: 450px

----

:blocky:`Was Euch erwartet`

.. note::
   - note 

**Was ist SSH?**

**Grundprinzip von SSH**

**Protokolle: SSH-1, SSH-2**

**Elementare Funktionen (ssh, scp, sshfs)**

**Ablauf und Authentifizierung**

**SSH-Härtung**
  
----

:blocky:`Was ist SSH?`

- Abkürzung für Secure Shell
- TCP/IP - Protokoll
- Hauptanwendung: Verschlüsselte Netzwerkverbindung zu einem entfernten Gerät
  herstellen
- Vorgänger: Telnet, RSH (Remote Shell)

.. note::
   - Remote Shell unter Unix, unsicher

----

:blocky:`Telnet - SSH`

.. note::
   - note

.. image:: images/ssh_telnet.png
   :align: left
   :height: 500px

----

:blocky:`Grundprinzip von SSH`

Sichere Kommunikation über ein Netzwerk durch:

- Verschlüsselung der Daten
- Integrität
- Authentifizierung

.. note::
   - note

.. image:: images/grundprinzip.png
   :align: left
   :height: 400px

----

:blocky:`SSH-1 und SSH-2`

**SSH-1:**

 + 1995: Entwickelt von Tatu Ylönen (University of Technology, Helsinki)
 + SSH1 (Implementierung) als Freie Software veröffentlicht
 + :colored:`--> nicht mehr empfohlen,` enthält sicherheitskritische Schwachstellen
   
 

.. note::
   - Implementierung, weil an seiner Uni Passwörter gesnifft wurden
   - Vorerst nur für sich selbst, später veröffentlicht
   - weil es Ad-hoc Implementierung war, traten zahlreiche Probleme auf
   - Konnten ohne die Kompatibilität zu früheren Versionen zu verlieren,
     nicht gefixt werden
   - SSH-1 nutzt CRC=Cyclic redundancy check
   - HMAC= hash message authentication code

     
----

:blocky:`SSH-1 und SSH-2`

 
 **SSH-2:**

 + 1996/1997 veröffentlicht
 + Sicherheitslücken von SSH-1 behoben
 + Zusätzliche Funktionen
 
 + 1998, SSH2 (Implementierung) mit strenger Lizenz, keine Freie
   Software
 + 1999, OpenBSD-Entwickler entwickeln mit OpenSSH eine freie Implementierung für
   SSH-2-Protokoll


.. note::
   - CRC=Cyclic redundancy check
   - HMAC= hash message authentication code
   - Statt CRC wird HMAC verwendet
   - Wahlmöglichkeit zwischen verschiedenen symmetrischen
     Verschlüsselungsverfahren
 
----

:blocky:`SSH-Implementierungen`

- OpenSSH
- Dropbear
- Mosh
- Lsh
- PuTTY

.. note::
   - Dropbear, MIT-Lizenz, Implementierung SSH2-Protokol,
     für Linux, Mac OS X, FreeBSD ...
   - Mosh: weitere Funktionalitäten, vorallem für mobile Nutzer,
     Verbindung wird bei Roaming aufrecht erhalten, optimierte Latenz (sofort
     zeigen, welche Befehle eingegeben wurden)
   - Lsh ebenfalls freie Impl. von SSH2-Protokoll, GPL
   - PuTTY, MIT-Lizenz, überwiegend für Windows



.. image:: images/openssh.gif
   :align: left
   :height: 200px

----


:blocky:`Let's start ... mit OpenSSH`

 - Installation:
   
   - sudo apt-get install openssh
   - sudo dnf install openssh
 - Kaum Konfiguration notwendig
 - ssh (Client)
 - sshd (Server, d=daemon)
 
 **Im Folgenden:**

 - Blick in die manpages von OpenSSH

.. note::
   - Konfiguration: Manches sicherlich sinnvoll, dazu später mehr 


----


:blocky:`Remote Terminal Session`

.. code-block:: bash

   [sue@kaktus]$ ssh micra@login.rz.hs-augsburg.de
   micra@login.rz.hs-augsburg.de's password:
   Linux bug 3.2.0-4-amd64 #1 SMP Debian 3.2.65-1+deb7u1 x86_64
   Plan your installation, and FAI installs your plan.
   
   Last login: Mon Apr 25 22:38:45 2016 
   from p5088ff5b.dip0.t-ipconnect.de 
   micra@bug:~$ 

.. note::
   - note

----

:blocky:`Demo: Remote-Session`

``ssh micra@login.rz.hs-augsburg.de``


----

:blocky:`Datenübertragung mit scp`

.. code-block:: bash  
  
   [sue@kaktus ~]$ scp hello_all.txt
   micra@login.rz.hs-augsburg.de:~ 
   micra@login.rz.hs-augsburg.de's password:
   hello_all.txt        100% 6297     6.2KB/s   00:00


.. note::
   - note

----

:blocky:`Demo: scp`

``scp hello_all.txt micra@login.rz.hs-augsburg.de:~``

----

:blocky:`sshfs - Dateisystem einhängen`

- Secure Shell File System
- Ermöglicht, entferntes Dateisystem per SSH einzuhängen
- FUSE basierend (Filesystem in User Space)
- **Mounting:**
   + sshfs [user@]host:[dir] mountpoint [options]
- **Unmounting:**
   + fusermount -u mountpoint


.. code-block:: bash  


    [sue@kaktus ~]$ sshfs micra@login.rz.hs-augsburg.de:
    /rz2home/micra/Dokumente mount_rz
    micra@login.rz.hs-augsburg.de's password:

    [sue@kaktus ~]$ fusermount -u ~/mount_rz

.. note::
   - note

----

:blocky:`Demo: sshfs`

``sshfs micra@login.rz.hs-augsburg.de:~ mount_rz``
    
----

:blocky:`Grober Ablauf`

1. Client sendet Anfrage an Server (Port 22)
2. Server gibt seine Identität, verwendetes Protokoll etc. bekannt
3. Client erhält Warnung, falls er das erste Mal mit Server kommuniziert
   --> Eintrag der Host-ID in known_hosts
4. Erzeugung eines Session-Keys mit Diffie-Hellman-Verfahren
5. Client wählt eine der vorgeschlagenen symmetrischen Verschlüsselungen (AES, Blowfish, 3DES, ...)

.. note::
   - note 

----

:blocky:`Authentifizierung`

 **Bisher:** Benutzername und Passwort 
 **Empfehlenswert:** Public-Key-Authentifizierung

 - Schlüsselpaar, bestehend aus privatem + öffentlichem Schlüssel
 - Schlüssel generieren: ssh-keygen
 - Öffentlicher Schlüssel muss auf Remote-PC abgelegt werden
 - Server generiert Zufalls-String mit öffentlichem Schlüssel
 - Client entschlüsselt String mit privatem Schlüssel
 - Client kombiniert String mit Session-Key und generiert daraus eine
   md5-Hashsumme
 - Server führt dies ebenfalls durch und vergleicht md5-Summe
 - Abgleich okay --> Client authentifiziert


.. note::
   - Um nur einen Grund zu nennen: Anforderungen an sicheres Passwort werden oft nicht eingehalten bzw.
     Passwörter mit hoher Entropie sind häufig schwer zu merken
   - z.B. bei SSH-Key keine Wörterbuchattacken

----

:blocky:`Demo: Public-Key-Authentifizierung`

``ssh-copy-id``: Im OpenSSH Paket enthalten, schreibt Public-Key in ``~/.ssh/authorized_keys`` auf dem Server

``ssh -v micra@login.rz.hs-augsburg.de``
    

----


:blocky:`Port-Forwarding`

- Verschlüsselung von Datenströmen anderer TCP-Andwendungen
- Wird auch Tunneling genannt
- Beispiel: Sichere Verbindung zu einem Mail-Server


:blocky:`X-Forwarding`

- X-Protokoll, Window-System für UNIX
- Anwendungs-Fenster von Remote-Rechner erscheint auf Client
  
----


:blocky:`SSH-Härtung`

- Grundkonfiguration auf manchen Systemen nur bedingt sinnvoll/sicher
- In config abzuändern:

  - PermitRootLogin **no**
  - AllowUsers **UserA, UserB**
  - Port **22** evtl. auf XY setzen, um Skriptkiddie-Attacken ins Leere laufen
    zu lassen
  - X11Forwarding **no**
  - Protocol **2**
  - PubkeyAuthentication **yes**

  
.. note::
    - AllowUsers, nur diese User haben Zugriff
    - Das sollte für den Großteil der Anwender ausreichend sicher sein.


----

:blocky:`More on SSH`

+ ssh-agent
+ auto-ssh 
+ Verschlüsselungsalgorithmen
+ ...

.. note::
    - ssh-agent: Verwaltung von privaten Schlüsseln für Public-Key-Auth. auf dem
      Server

---------------------



**Vielen Dank ... und nutzt SSH ;-)**

