# Structuur API-call log



```json
{
    "timestamp": "2020-05-29T08:09:34.539Z",
    "type": ["application"],
    "correlationId": "d80db7ea-fe4c-4df5-afe1-1b675e19921f",
    "level": "INFO",
    "request": {
        "headers": {
            "key": "value",
            "key2": "value2"
    	},
      	"host": "http://www.api.com",
    	"path": "/resource/id?requestParam=value&requestParam2=value",
    	"payload": "{ ... }",
    	"protocol": "HTTP",
    	"method": "POST",     
    },
    "response": {
         "headers": {
            "key": "value",
            "key2": "value2"
    	},
        "payload": "{ ... }",
    	"status": 200,
    	"duration": 25,
    },
    "protocol": "HTTP"
}
```

**Request** en **response** worden samen gelogd, maar worden elks een top level property om het onderscheid duidelijk te maken.

Zowel request als response hebben een *header-key*. Hierin bevinden zich enkel de headers die belangrijk zijn om context te scheppen voor de uitgevoerde call. Zaken als API keys en andere **confidientiële headers moeten weggelaten worden**. We kiezen hier best voor een **opt-in** strategy.

*Payload* van zowel request als response kan **optioneel** meegegeven worden. Dit wordt **standaard niet** gedaan omwille van GDPR en resource overwegingen. Wanneer de status in de **4XX** range zit wordt de body mee gelogd om support en debuggen eenvoudiger te maken.

*duration* weergegeven in **milliseconden** (ms).

*log-level* is altijd info, dit in overeenstemming met de regel. info-logs geven blijk van een **state change** binnen de applicatie.