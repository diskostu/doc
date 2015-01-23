## Neuer Service-Provider und -Consumer
### In a nutshell
#### Provider
 1. Vorlage erstellen (Interface, Implementierung, Models)
 2. WSDL erzeugen lassen
 3. WSDL prüfen (Validierung und Auge) und evtl. anpassen
 4. Java-Klassen anhand der WSDL generieren. Vorlage aus dem Classpath entfernen (oder ganz löschen)
 5. Webservice mit den generierten Dateien starten

#### Consumer
 1. WSDL vom Provider holen (Browser, Service-Endpoint + `?wsdl`)

### Details
#### Service-Interface und -Implementierung
 - neues Java-Projekt (normal)
 - neues Interface ...Service
 - 2 Methoden:
	 - `berechneAlter(int jahr) throws ValidierungsException`
	 - `berechneRate(int alter) throws ValidierungsException` 
 - einfache Implementierung ...ServiceImpl
 - annotieren:
	 - ...Service mit `@WebService` annotieren (keine Attribute)
	 - ...ServiceImpl mit `@WebService` annotieren und Attribute: `serviceName`, `name`
 - Publisher bauen
	 - Adresse des WS als String-Konstante
	 - `main`-Methode
		 - 	neue Instanz von ...ServiceImpl
		 - `Endpoint.publish`
		 - WSDL lässt sich jetzt im Browser aufrufen

#### WSDL und XSD anpassen
 - WSDL und XSD aus dem Browser heraus lokal speichern (URL + `?wsdl`) und diese nun bearbeiten

##### WSDL anpassen
 - Ort der XSD anpassen (lokal!)

##### XSD anpassen
 - Variablen- und Methodennamen anpassen
 - alternativ: vorher schon in der Java-Klasse Methoden und Variablen annotieren

#### Java-Klassen generieren
 - Resultat prüfen
 - Folder `src` **unterhalb `generate`** als Source-Folder markieren
 - jetzt werden doppelt vorhandene Klassen angemeckert. Die Klassen innerhalb `generate` werden behalten, die anderen müssen gelöscht werden

#### Consumer erstellen
 - neues Java-Projekt (normal)
 - `generate.bat` übernehmen
 - den Publisher des anderen Projektes starten
 - die WSDL und XSD aus dem Browser nehmen und lokal speichern
 - Klassen anhand der WSDL generieren
 - Folder `src` **unterhalb `generate`** als Source-Folder markieren (*Build Path* --> *Use as SourceFolder*)
 - Testklasse (`Tester`) in eigenem package anlegen
	 - `main`-Methode
	 - neue Instanz der generierten Hilfsklasse `...ServiceService`
	 - mit `getPort(<Interface.class>)` das Interface holen
	 - darauf können dann die Methoden, die das Interface anbietet, aufgerufen werden

#### Test mit TcpMon
**TcpMon** fungiert als *Man in the Middle*. Damit man dort die Nachrichten sieht, muss man in seinem Consumer einen anderen Port angeben, einen, auf den **TcpMon** lauschen kann.
__Beispiel__: der Provider stellt den Webservice an Port 9090 bereit. In der WSDL des Consumer steht bisher dieser Port im Tag `soap:address`.  Dort muss der Port auf den gestellt werden, auf dem **TcpMon** lauscht. **TcpMon** fängt den Request ab, zeigt ihn an und schickt ihn dann an den eigentlichen Endpoint.

