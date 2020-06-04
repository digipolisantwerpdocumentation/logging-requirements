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
    "correlation": {
        "id": "d80db7ea-fe4c-4df5-afe1-1b675e19921f",
        "sourceId": "e27ce2ff-4cf1-40e8-8d70-fe6b105e6490",
        "sourceName": "appName",
        "instanceId": "8d0e2382-0540-4229-bc6d-8cdc9c2294ab",
        "instanceName": "appName-instanceName",
        "userId": "userid",
        "ipAddress": "194.25.76.122"
    },
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
    "correlation": {
        "id": "d80db7ea-fe4c-4df5-afe1-1b675e19921f",
        "sourceId": "e27ce2ff-4cf1-40e8-8d70-fe6b105e6490",
        "sourceName": "appName",
        "instanceId": "8d0e2382-0540-4229-bc6d-8cdc9c2294ab",
        "instanceName": "appName-instanceName",
        "userId": "userid",
        "ipAddress": "194.25.76.122"
    },
    "payload": "{ ... }",
    "protocol": "HTTP",
    "status": 200,
    "duration": 25,
    "level": "INFO"
}
```

Het eerste noemenswaardige veld voor API-call logs is de *header-key*. Hierin bevinden zich enkel de headers die belangrijk zijn om context te scheppen voor de uitgevoerde call. Zaken als API keys en andere **confidientiële headers moeten weggelaten worden**.

*Payload* van zowel request als response kan **optioneel** meegegeven worden. Dit wordt standaard niet gedaan omwille van GDPR en resource overwegingen. Wanneer de status in de **4XX** range zit wordt de body mee gelogd om support en debuggen eenvoudiger te maken.

*duration* weergegeven in **milliseconden** (ms).

*log-level* is altijd info, dit in overeenstemming met de regel. info-logs geven blijk van een **state change** binnen de applicatie.