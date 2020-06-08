# Structuur techniche log

```json
{
    "timestamp": "2020-05-29T08:09:34.539Z",
    "type": "technical",
    "correlationId": "d80db7ea-fe4c-4df5-afe1-1b675e19921f",
    "message": "SQL statement/Stacktrace/DEBUG info/Startup info/Shutdown info/Executing method X with params Y/...",
    "level": "DEBUG/INFO/WARN/ERROR/FATAL/TRACE"
}
```

De enige aanvulling bovenop [de standaard structuur](https://github.com/digipolisantwerpdocumentation/logging-requirements#structuur) is de *message-key*. Hierin kan het effectief te loggen bericht als String meegegeven worden.
**Gebruik het correlationId enkel waar logisch**. Startup info en shutdown info is uiteraard niet gecorreleerd aan een bepaalde transactie. SQL statements en stacktraces zijn dat bijvoorbeeld wel.

#### Voorbeelden message key

```json
"message": "org.cloud.sampl.music.web.ErrorController:23 [log_framework=log4j2;app_name=spring-music;app_version=1.0;instance_id=${env:INSTANCE_INDEX}] Forcing an exception to be thrown and sent to logging framework MULTIEXCEPTION java.lang.NullPointerException: Forcing an exception to be thrownu2028 at org.cloudfoundry.samples.music.web.ErrorController.throwException(ErrorController.java:22)"
```

```json
"message": " SELECT * FROM Customers WHERE Last_Name='Smith';"
```

```json
"message": "Handling event user_logged_in..."
```

