# Produkte
Bestände werden über einen CSV-Export von Qualibet beim Marktplatz gemeldet. 

Für den Austausch muss die Marke einen FTP-Server zur Verfügung stellen, alternativ kann auch
qualibet einen FTP für den Austausch der Daten bereitstellen.
    
    1. Qualibet übergibt an die Marke einen CSV-Export, der alle verfügbaren EAN-Codes pro Land enthält. 
       => products/qb-ean-export.csv
       
    2. Die Marke liest die CSV-Datei ein und gibt für jede EAN einen Preis pro Land zurück. 
       => products/brand-price-export.csv
       
    3. Nach Erhalt des Preises übergibt qualibet an die Marke alle Bestände die für diesen Preis in
       dem Land zur Verfügung stehen.
       => products/qb-stock-export.csv
    
# Bestellungen
Qualibet stellt zur Bestellverwaltung einen Webservice und einen Webhook zur Verfügung. Die Marke stellt
sicher, dass die Status-Updates die von Qualibet gemeldet werden entgegengenommen werden.

    1. Bestellungen die auf dem Marktplatz der Marke entstehen werden über den Webservice bei qualibet gemeldet
    
    2. Reservierte Bestellungen müssen geupdated werden, sobald diese bezahlt sind und versendet werden dürfen.
    
    3. Jede aktualisierung der Bestellung (Versendung, Retoure, Storno) werden von Qualibet per Webhook
       an die Marke gemeldet. Der Webhook meldet die order_id die aktualisiert wurde.

    4. Die Marke kann jederzeit über den Webservice die aktuellen Daten und Status der Bestellung
       abrufen. Mit jeder Meldung von Qualibet muss die Marke den aktuellen Status der Bestellung
       für sich aktualisieren.
       
### Infos Webhook
Der Webhook von Qualibet kann einen Webservice- / URL-Aufruf bei der Marke durchführen und die sich
geänderte order_id übergeben.

### Infos Webservice
Der Webservice ist unter brand-mp.qb-staging.de/v1 erreichbar. Nach Livegang wird die Live-URL zur Verfügung gestellt.
Die Authentifizierung wird im Header mit einem Token durchgeführt.

    1. Header-Daten für die Kommunikation
       Authorization: Bearer <token>
       Content-Type: application/json
    
    2. Bestellungen müssen über den POST https://brand-mp.qb-staging.de/v1/orders gemeldet werden.
       => orders/post-order.json
       
    3. Bestellungen, die als reserviert angelegt wurden, müssen über einen PATCH-Call als storniert bzw. 
       für den Versandt freigegeben werden.
       => orders/patch-order.json

    4. Der Status der Bestellung kann jederzeit über GET https://brand-mp.qb-staging.de/v1/orders/{order_id} 
       abgerufen werden. Nach einem Webhook muss von der Marke der aktuelle Status abgeholt werden.
       => orders/get-order.json
          
