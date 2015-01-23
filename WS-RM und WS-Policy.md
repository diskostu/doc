# WS-RM und WS-Policy

## WS-RM
### Allgemein
[WS-RM](http://de.wikipedia.org/wiki/WS-Reliable_Messaging) ist eine Spezifikation aus dem Bereich WS-*, die garantiert, dass gesendete Nachrichten auch im Fall von Versagen einzelner Softwarekomponenten beim Empfänger ankommen.

Dazu sind quasi auf Sender- und Empfängerseite je ein Vermittler auf Middlewareebene dazwischengeschaltet.

### Aktivierung
Das Feature wird ==im Provider== aktiviert. Es kann sowohl per Annotation als auch per WSDL aktiviert werden. Die WSDL hat die höhere Priorität.

#### Aktivierung per Annotation
``` Java
@ReliableMessaging(enabled=true)
public class MyServiceImpl implements MyService {
...
```
#### Aktivierung per WSDL
Beispiel für den WSDL-Code siehe Unterlagen Kapitel 6.3.
==Achtung==: es muss darauf geachtet werden, dass **explizit eine WSDL-Location angegeben wird**, damit zur Laufzeit genau diese benutzt und nicht eine WSDL anhand der Klassen generiert wird.
``` Java
@WebService(..., wsdlLocation = "WEB-INF/wsdl/MyService.wsdl")
public class MyServiceImpl implements MyService {
...
```
Der Consumer kann sich darauf verlassen, dass die WSDL, die der Provider bereitstellt (URL + ?wsdl), die richtige ist.

## WS-Policy
### Allgemein
[WS-Policy](http://de.wikipedia.org/wiki/WS-Policy) ist eine Spezifikation des W3C im Rahmen der WS-* Spezifikationen, die es dem Serviceanbieter ermöglicht, die Richtlinien bezüglich Sicherheit, Qualität und Version seines Services in Form von maschinenlesbaren XML-Daten für den Servicenutzer bereitzustellen. Umgekehrt kann der Servicenutzer seine Anforderungen ebenfalls in Form von XML-Daten spezifizieren.
Diese Policies werden dann an entsprechenden Stellen in die WSDL-Beschreibung des Service (oder auch im BPEL-Prozess) eingefügt und gelten als Mindestrichtlinien auch für alle in der Hierarchie tiefer stehenden Elemente. Umgekehrt kann auch ein Servicenutzer seine Anforderung an einen Service in Form von Policies formulieren, so dass dann zur Laufzeit "ausgehandelt" werden muss, was denn nun die effektive Policy wird.

### Aktivierung
Im Gegensatz zu WS-RM kann WS-Policy ==nur per WSDL aktiviert werden==.
Es kann sowohl im Provider als auch im Consumer aktiviert werden. Hier muss darauf geachtet werden, dass **explizit eine WSDL-Location angegeben wird**, damit zur Laufzeit genau diese benutzt und nicht eine WSDL anhand der Klasen generiert wird.
``` Java
@WebService(..., wsdlLocation = "WEB-INF/wsdl/MyService.wsdl")
public class MyServiceImpl implements MyService {
...
```