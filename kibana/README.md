# Kibana

Je applicatie logs voldoen aan de [standaarden](../README.md) en worden [correct gevalideerd](../schema/README.md) in de juiste index afgeleverd, en nu?

## Het log object in Kibana

Niet enkel stdout van de applicatie stroomt door tot in Elasticsearch (en dus Kibana).
Ook Kubernetes en Elasticsearch zelf voegen extra metadata toe aan het log object.
