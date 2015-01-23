## Binärdaten

### MTOM
[MTOM](http://de.wikipedia.org/wiki/SOAP_Message_Transmission_Optimization_Mechanism) = *SOAP Message Transmission Optimization Mechanism*. Dient zur Optimierung beim Versand von Binärdaten. Der Versand klappt auch ohne MTOM, allerdings kann es bei großen Dateigrößen zu Problemen kommen.
Ohne MTOM werden die Daten immer inline im SOAP Body versendet.
MTOM steuert [XOP](http://de.wikipedia.org/wiki/XML-binary_Optimized_Packaging) (*XML-binary Optimized Packaging*).

#### Aktivierung
**Provider-Seite**: es gibt mehrere Möglichkeiten, empfohlen wird die  Annotation zusätzlich zu der `@WebService`-Annotation in der `...ServiceImpl`.
``` Java
@WebService(...)
@MTOM
public class WebserviceImpl {
...
```
Der Annotation kann man u.a. auch einen `Treshold`-Wert mitgeben, welche steuert, ab welcher Binärdaten-Größe die Daten XOP-encoded oder als Anhang verpackt werden.

**Consumer-Seite**: beim Holen des Ports das MTOM-Feature mit angeben.
``` Java
service.getEndpointPort(new MTOMFeature());
```

### Sonstiges
Es ist unerheblich, ob eine Methode einen Rückgabewert vom Typ `byte[]` oder einen komplexen Datentyp, der ein `byte[]`-Feld hat, zurückgibt.

Schulung: Java-Projekt ==WS-04-MTOM==, Unterlagen Kapitel ==4.4==