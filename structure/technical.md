# Structuur techniche log

```json
{
    "timestamp": "2020-05-29T08:09:34.539Z",
    "type": "technical",
    "correlation": {
        "id": "d80db7ea-fe4c-4df5-afe1-1b675e19921f",
        "sourceId": "e27ce2ff-4cf1-40e8-8d70-fe6b105e6490",
        "sourceName": "appName",
        "instanceId": "8d0e2382-0540-4229-bc6d-8cdc9c2294ab",
        "instanceName": "appName-instanceName",
        "userId": "userid",
        "ipAddress": "194.25.76.122"
    },
    "log": "SQL statement/Stacktrace/DEBUG info/Startup info/Shutdown info",
    "level": "DEBUG/INFO/WARN/ERROR/FATAL/TRACE"
}
```

De enige aanvulling bovenop [de standaard structuur](https://github.com/digipolisantwerpdocumentation/logging-requirements#structuur) is de *log-key*. Hierin kan het effectief te loggen bericht als String meegegeven worden.
**Gebruik het correlation-object enkel waar logisch**. Startup info en shutdown info is uiteraard niet gecorreleerd aan een bepaalde transactie. SQL statements en stacktraces zijn dat wel.

#### Voorbeelden log key

```json
"log": "org.cloud.sampl.music.web.ErrorController:23 [log_framework=log4j2;app_name=spring-music;app_version=1.0;instance_id=${env:INSTANCE_INDEX}] Forcing an exception to be thrown and sent to logging framework MULTIEXCEPTION java.lang.NullPointerException: Forcing an exception to be thrownu2028 at org.cloudfoundry.samples.music.web.ErrorController.throwException(ErrorController.java:22)"
```

```json
"log": " SELECT * FROM Customers WHERE Last_Name='Smith';"
```