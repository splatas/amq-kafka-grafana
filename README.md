# amq-kafka-grafana

Instalación base en https://api-ocp-rp.cloudteco.com.ar/console/project/amq-kafka-dev

1. Componentes preinstalados 
Antes de la instalación de Prometheus y Grafana se encontraban instalados los siguientes componentes: 

- Deployments (namespace 'amq-kafka-dev'):
    1. kafka-cluster-entity-operator (./00.Preinstalados/kafka-cluster-entity-operator.yaml)
    2. strimzi-cluster-operator (./00.Preinstalados/strimzi-cluster-operator.yaml)


2. Componentes a instalar
2.1 prometheus-operator.yaml
    a. Necesita una service account:
    Error creating: pods "prometheus-operator-859f6d4f97-" is forbidden: error looking up service account amq-kafka-test/prometheus: serviceaccount "prometheus" not found.

    2.1.1 oc create -f sa-prometheus.yaml -n amq-kafka-test
    Si es necesario, crear los secrets con:
        a. oc create -f secret-prometheus-dockercfg.yaml -n amq-kafka-test
        b. oc create -f secret-prometheus-token.yaml -n amq-kafka-test
    
2.2. prometheus-prometheus-statefulset.yaml
    oc create -f prometheus-prometheus-statefulset.yaml -n amq-kafka-test
    Lo crea pero no deploya porque necesita la serviceaccount 'prometheus-server'
        a. oc create -f prometheus-server-sa.yaml -n amq-kafka-test
        b. oc create -f prometheus-prometheus-rulefiles-0-cm.yaml -n amq-kafka-test
        c. oc create -f prometheus-prometheus-secret.yaml -n amq-kafka-test
        d. oc create -f prometheus-svc.yaml -n amq-kafka-test
        d. oc create -f prometheus-route.yaml -n amq-kafka-test

- Grafana
2.2. oc create -f grafana.yaml -n amq-kafka-test

2.3. kafka-cluster-kafka-exporter.yaml