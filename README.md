# jlip
All about JLIP (Joint Level Interface Protocol) / J-Terminal / JVC Camera Remote
I searched for a possible way to control my jvc cams from a local remote controller.

First of all a collection of ressources that i've found about jvc remote control:
- jvc-remote Project
Repository 1:
https://gitlab.ags.etc.tu-bs.de/e.lab/jvc-remote
Repository 2:
https://github.com/ags-tubs/jvc-remote
- http://www.johnwillis.com/2018/09/jvc-jlip-joint-level-interface-protocol.html
- JLIP Protocol Documentation
- pinout

Related Hardware
---------

Cameras
- JVC GY-HM70E
- JVC GY-HM150U
- JVC GY-HMQ10

Remote Controls
- JVC HZ-HM150VZR http://pro.jvc.com/prof/attributes/features.jsp?model_id=MDL102139&feature_id=01
- VariZoom VZROCK-J150 https://www.varizoom.com/product/vzrock-j150/
- JVC HZ-ZOE-HM150 http://www.jvcpro.gr/products/controllers/1000004751-HZ-ZOE-HM150

Untersuchung
---------
JVC GY-HM70E (=CAM)
VariZoom VZROCK-J150 (=RC)

RC an CAM angeschlossen, Messung parallel an den PINs
nach dem Einschalten liegt auf
PIN 1 RX etwa +10V
PIN 2 TX etwa +12V
PIN 3 GND
PIN 4 folgt PIN 1

Die Spannungen gehen nach etwa 7 Sekunden zurück auf 0V, wenn keine Fernbedienung angeschlossen ist.
Vermutlich ist die Vorraussetzung, dass die Fernbedienung laufend eine Befehlsfolge schickt, so dass die Spannung/Schnittstelle an bleibt.

Die Verbindung bricht ab, sobald man einen der PINs löst.
Die Funktion von PIN 4 ist unklar, wenn man ihn mit PIN 3 (=GND) kurz schließt,
kann nichts mehr Empfangen werden. PIN 1 geht dann nämlich auch mit auf GND!
(??)
Senden kann man trotzdem.
Empfangen wird nur für den REC Status gebraucht. Ist PIN 4 auf GND, und damit auch PIN 1, geht die REC Lampe der RC halt nicht an.
Seltsam ist, dass es nicht mehr funktioniert wenn PIN 4 in der Luft hängt.

Die Kommunikation ist seriell. Untersuchungen ergeben folgende Schnittstellenparamter:
19.200 Baud, 8 Datenbits, 1 Stopbit, Parity Even
kurz
19200 8E1

Bei diesen Parametern werden die Bytes decodiert und das Parity-Bit ist jeweils gültig.

Interface (unbestätigt)
---------
Signallevel: 3.3V
Format: 9600baud 1E8 (8 daten, 1 stop-bit, even-parity)

Protocol
---------

#### valid commands ####

|cmd|Byte 1|Byte 2|Byte 3|Byte 4|Byte 5|Byte 6|Byte 7|Byte 8|Byte 9|Byte 10|Byte 11|
|---|------|------|------|------|------|------|------|------|------|------|------|
|cmd|0xFF|0xFF|0x00|0x00|0x00|0x00|0x00|0x00|0x00|0x00|chk|
|||||||||||
|||||||||||
