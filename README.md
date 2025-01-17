# Weaviate with Prometheus Monitoring & Grafana Dashboards
# Weaviate, Prometheus, and Grafana Setup Guide

This guide provides step-by-step instructions for deploying and configuring Weaviate, Prometheus, and Grafana on a Kubernetes cluster.

## Prerequisites
- Kubernetes cluster
- kubectl configured to interact with your cluster
- Helm installed

## Deploying Weaviate

1. **Clone the Weaviate Helm Chart**
   ```bash
   git clone https://github.com/semi-technologies/weaviate-helm.git
   cd weaviate-helm
   ```

2. **Install Weaviate using Helm**
   ```bash
   helm install weaviate ./weaviate
   ```

3. **Apply the HTTP and gRPC Services**
   ```bash
   kubectl apply -f weaviate-http-service.yaml
   kubectl apply -f weaviate-grpc-service.yaml
   ```

4. **Verify the Services**
   ```bash
   kubectl get svc weaviate-http weaviate-grpc
   ```

## Deploying Prometheus

1. **Add the Prometheus Helm Repository**
   ```bash
   helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
   helm repo update
   ```

2. **Install Prometheus**
   ```bash
   helm install prometheus prometheus-community/prometheus
   ```

3. **Verify Prometheus Installation**
   ```bash
   kubectl get pods -l app=prometheus
   ```

## Deploying Grafana

1. **Add the Grafana Helm Repository**
   ```bash
   helm repo add grafana https://grafana.github.io/helm-charts
   helm repo update
   ```

2. **Install Grafana**
   ```bash
   helm install grafana grafana/grafana
   ```

3. **Get Grafana Admin Password**
   ```bash
   kubectl get secret --namespace default grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
   ```

4. **Access Grafana**
   - Forward the Grafana service to your local machine:
     ```bash
     kubectl port-forward svc/grafana 3000:80
     ```
   - Open your browser and go to `http://localhost:3000`
   - Login with username `admin` and the password retrieved above

## Configuring Grafana

1. **Add Prometheus as a Data Source**
   - Go to Grafana UI -> Configuration -> Data Sources -> Add data source
   - Select Prometheus
   - Set the URL to `http://prometheus-server`
   - Save & Test

2. **Import Dashboards**
   - Go to Grafana UI -> Dashboards -> Import
   - Paste the JSON configuration for the Weaviate dashboard
   - Select Prometheus as the data source

## Inserting Data into Weaviate

1. **Install Python Requirements**
   ```bash
   pip install -r requirements.txt
   ```

2. **Run the Data Insertion Script**
   ```bash
   python insert_data.py
   ```

3. **Verify Data Insertion**
   - Use the Weaviate console or API to query the inserted data

## Monitoring and Verification

- Use Grafana to monitor Weaviate's performance and operations
- Use Prometheus queries to explore specific metrics

## Additional Configurations
- Modify service configurations as needed for your environment
- Scale the setup by adding more classes, data, and monitoring dashboards

For further customization or specific requirements, feel free to reach out for assistance!




