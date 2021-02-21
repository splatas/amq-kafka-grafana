# amq-kafka-grafana

Instalación base en https://api-ocp-rp.cloudteco.com.ar/console/project/amq-kafka-dev

1. Componentes preinstalados. Antes de la instalación de Prometheus y Grafana se encontraban instalados los siguientes componentes: 
    - Deployments (namespace 'amq-kafka-dev'):
        1. kafka-cluster-entity-operator (./00.Preinstalados/kafka-cluster-entity-operator.yaml)
        2. strimzi-cluster-operator (./00.Preinstalados/strimzi-cluster-operator.yaml)


2. Componentes a instalar.
    1. prometheus-operator.yaml
        1. Necesita una service account (Error creating: pods "prometheus-operator-859f6d4f97-" is forbidden: error looking up service account amq-kafka-test/prometheus: serviceaccount "prometheus" not found).
        
        - oc create -f sa-prometheus.yaml -n amq-kafka-test
    
            Si es necesario, crear los secrets con:
            a. oc create -f secret-prometheus-dockercfg.yaml -n amq-kafka-test
            b. oc create -f secret-prometheus-token.yaml -n amq-kafka-test
    
    2. prometheus-prometheus-statefulset.yaml
        Genero un pv para almacenamiento de Prometheus
    
        - oc create -f pv-prometheus-data.yaml -n amq-kafka-test
        - oc create -f prometheus-prometheus-statefulset.yaml -n amq-kafka-test
   
        Lo crea pero no deploya porque necesita la serviceaccount 'prometheus-server'
        - oc create -f prometheus-server-sa.yaml -n amq-kafka-test
        - oc create -f prometheus-prometheus-rulefiles-0-cm.yaml -n amq-kafka-test
        - oc create -f prometheus-prometheus-secret.yaml -n amq-kafka-test
        - oc create -f prometheus-svc.yaml -n amq-kafka-test
        - oc create -f prometheus-route.yaml -n amq-kafka-test
        - oc create -f kafka-service-monitor.yaml -n amq-kafka-test

3. Kafka Exporter
    - oc create -f kafka-cluster-kafka-exporter-certs.yaml -n amq-kafka-test
    - oc create -f kafka-cluster-kafka-exporter-sa.yaml -n amq-kafka-test
    - oc create -f kafka-cluster-kafka-exporter.yaml -n amq-kafka-test


4. Grafana. Genero un pv para almacenar la configuración (dashboards) de Grafana y lanzo la instalación:
    - oc create -f pv-grafana-data.yaml -n amq-kafka-test
    - oc create -f grafana.yaml -n amq-kafka-test
    
        Genero el service y la route, por default no las crea:
        - oc create -f grafana-svc.yaml -n amq-kafka-test
        - oc create -f grafana-route.yaml -n amq-kafka-test

5. Configuración de los Dashboards
Los json con los Dashboards predefinidos se encuentran en ./amq-streams-1.6.2-ocp-install-examples/examples/metrics/grafana-dashboards
    - strimzi-cruise-control.json
    - strimzi-kafka-bridge.json
    - strimzi-kafka-connect.json
    - strimzi-kafka-exporter.json
    - strimzi-kafka-mirror-maker-2.json
    - strimzi-kafka.json
    - strimzi-operators.json
    - strimzi-zookeeper.json