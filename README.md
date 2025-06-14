# Smart Home with K8s and Microservices | Application Helm Chart

This repository is part of my thesis project, "Enhancing Integration Process and Manageability of a Microservices-Based Home Automation Application with Kubernetes", and contains the Helm chart that deploys the microservices-based home automation application on Kubernetes. The chart includes all necessary components for running a complete home automation system with MQTT broker, database, and web interface.

## Prerequisites

- Kubernetes cluster (version 1.16.0 or later)
- Helm 3.x
- PV provisioner support in the underlying infrastructure

## Components

The chart deploys the following components:

- **MQTT Broker**: Message broker for IoT device communication
- **PostgreSQL**: Database for storing application and device data
- **InfluxDB**: Time-series database for metrics and sensor data
- **Web Application**: Django-based API interface for device management
- **Streamlit GUI**: User interface for data visualization

## Installation

Install the chart:
```bash
helm install home-automation ./home-automation-chart
```

## Configuration

The following table lists the configurable parameters of the chart and their default values.

### Persistent Volume Claims

| PVC Name | Storage Size | Purpose |
|----------|--------------|---------|
| mqtt-log-pvc | 200Mi | MQTT broker logs |
| mqtt-data-pvc | 1Gi | MQTT broker data |
| postgres-pvc | 1Gi | PostgreSQL data |
| influxdb-data-pvc | 1Gi | InfluxDB data |
| influxdb-config-pvc | 200Mi | InfluxDB configuration |

### Services

| Service Name | Port | NodePort | Description |
|--------------|------|----------|-------------|
| mqttbroker | 1883 | 30883 | MQTT broker service |
| web | 8000 | 30800 | Web application |
| influxdb | 8086 | 30886 | InfluxDB service |
| db | 5432 | - | PostgreSQL service |
| streamlit | 8501 | 30851 | Streamlit GUI |

### Secrets

The chart creates the following secrets:

- InfluxDB admin token, username, and password
- PostgreSQL username and password
- Django superuser credentials

## Customization

To customize the deployment, you can override the values in `values.yaml`:

```bash
helm install home-automation ./home-automation-chart \
  --set pvc[0].storage=500Mi \
  --set services[0].nodePort=30000
```

## Security Considerations

- All sensitive data is stored as Kubernetes secrets
- NodePorts are used for external access to services
- Consider implementing network policies for additional security
