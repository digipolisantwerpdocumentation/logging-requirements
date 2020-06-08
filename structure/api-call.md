# Structuur API-call log

*Request*

```json
{
    "timestamp": "2020-05-29T08:09:34.539Z",
    "type": "application",
    "headers": {
        "key": "value",
        "key2": "value2"
    },
    "correlationId": "d80db7ea-fe4c-4df5-afe1-1b675e19921f",
    "host": "http://www.api.com",
    "path": "/resource/id?requestParam=value&requestParam2=value",
    "payload": "{ ... }",
    "protocol": "HTTP",
    "method": "POST",
    "level": "INFO"
}
```

*Response*

```json
{
    "timestamp": "2020-05-29T08:09:34.539Z",
    "type": "application",
    "headers": {
        "key": "value",
        "key2": "value2"
    },
    "correlationId": "d80db7ea-fe4c-4df5-afe1-1b675e19921f",
    "payload": "{ ... }",
    "protocol": "HTTP",
    "status": 200,
    "duration": 25,
    "level": "INFO"
}
```

Het eerste noemenswaardige veld voor API-call logs is de *header-key*. Hierin bevinden zich enkel de headers die belangrijk zijn om context te scheppen voor de uitgevoerde call. Zaken als API keys en andere **confidientiële headers moeten weggelaten worden**.

*Payload* van zowel request als response kan **optioneel** meegegeven worden. Dit wordt standaard niet gedaan omwille van GDPR en resource overwegingen. Wanneer de status in de **4XX** range zit wordt de body mee gelogd om support en debuggen eenvoudiger te maken.

*headers* kunnen **optioneel** worden meegegeven.

*duration* weergegeven in **milliseconden** (ms).

*log-level* is altijd info, dit in overeenstemming met de regel. info-logs geven blijk van een **state change** binnen de applicatie.