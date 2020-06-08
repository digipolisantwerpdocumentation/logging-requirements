# Structuur event log

```json
{
    "timestamp": "2020-05-29T08:09:34.539Z",
    "type": "application",
    "correlationId": "d80db7ea-fe4c-4df5-afe1-1b675e19921f",
    "payload": { ... },
    "headers": {
        "key": "value",
        "key2": "value2"
    },
    "level": "INFO",
    "topic": "topicName"
}
```

De event structuur wordt enkel toegepast op appliacties die rechtstreeks met **Kafka** communiceren. Applicaties die de Digipolis Event Handler gebruiken (die achterliggend RabbitMQ gebruikt) communiceren via API calls en gebruiken daarvoor dus [de API-call structuur](api-call.md).
*Headers* en *payload* zijn **opt-in** en worden standaard niet gelogd. *Topic* wordt gebruikt om het soort event te **identificeren** (bijvoorbeeld user_logged_in)