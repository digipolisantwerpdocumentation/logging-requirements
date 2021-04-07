# Log validatie

Door beperkingen in Elasticsearch is het noodzakelijk dat alle log properties tot een minimale mapping behoren.
Logs die niet voldoen aan het schema in deze folder `logschema.json` zullen door de pipeline naar een andere index gestuurd worden.

Eén schema omvat alle mogelijke properties van de 3 mogelijke soorten logs:

* API call
* Event
* Technisch

## Output valideren

Om te **testen** of de stdout output van een applicatie voldoet aan de eisen opgelegd in `logschema.json` kan het schema gedownload worden en in combinatie met jsonschema en één of meerdere voorbeeldfiles kan het script uitgevoerd worden.

### jsonschema installeren

```bash
pip install jsonschema
```

### jsonschema uitvoeren

```bash
jsonschema -i example.json logschema.json
```

