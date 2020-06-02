# Structuur API-call log

*Request*

```json
{
    "timestamp": "2020-05-29T08:09:34.539Z",
    "type": "application/technical/privacy",
    "type?": "request",
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
    "type": "application/technical/privacy",
    "type?": "response",
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

