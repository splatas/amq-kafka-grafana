# amq-kafka-grafana

Instalación base en https://api-ocp-rp.cloudteco.com.ar/console/project/amq-kafka-dev

1. Componentes preinstalados 
Antes de la instalación de Prometheus y Grafana se encontraban instalados los siguientes componentes: 

- Deployments (namespace 'amq-kafka-dev'):
    1. kafka-cluster-entity-operator (./00.Preinstalados/kafka-cluster-entity-operator.yaml)
    2. strimzi-cluster-operator (./00.Preinstalados/strimzi-cluster-operator.yaml)


2. Componentes a instalar
- prometheus-operator.yaml
- grafana.yaml
- kafka-cluster-kafka-exporter.yaml