# 🔍 Kubernetes Logging with Fluent Bit & Aiven OpenSearch Dashboards

This project sets up a lightweight, Kubernetes-native logging pipeline using:

- **Fluent Bit** as a log collector (DaemonSet)
- **Aiven-hosted OpenSearch** as the managed log storage
- **OpenSearch Dashboards** for real-time log exploration
- Fully deployed via Kubernetes YAML manifests (no Helm or 3rd-party tools)

---

## 🚀 Architecture Overview

[ Pod Logs ] ↓ [ Fluent Bit DaemonSet ] ↓ [ Aiven OpenSearch ] ↓ [ OpenSearch Dashboards ]


---

## 📂 Project Structure

```bash
├── fluent-bit-config.yaml          # ConfigMap for Fluent Bit input/output behavior
├── fluent-bit-daemonset.yaml      # DaemonSet with volume mounts and tolerations
├── fluent-bit-serviceaccount.yaml # ServiceAccount and RBAC rules
├── opensearch-deployment.yaml     # Deployment and service definition for local OpenSearch (if needed)
└── namespace.yaml                 # Creates the 'logging' namespace
⚙️ How It Works
Fluent Bit runs on every node (via DaemonSet) and tails logs from /var/log/containers/

It pushes logs to a remote OpenSearch service hosted on Aiven, using:

TLS encryption

Basic authentication

Logs are indexed in OpenSearch and visualized via OpenSearch Dashboards

# 1. Create namespace
kubectl apply -f namespace.yaml

# 2. Apply Fluent Bit resources
kubectl apply -f fluent-bit-serviceaccount.yaml
kubectl apply -f fluent-bit-config.yaml
kubectl apply -f fluent-bit-daemonset.yaml

# 3. (Optional) Deploy OpenSearch locally (if not using Aiven)
kubectl apply -f opensearch-deployment.yaml

# 4. Restart Fluent Bit to apply config
kubectl delete pod -l app=fluent-bit -n logging

📊 Accessing Logs
If you're using Aiven-hosted OpenSearch:

Login to OpenSearch Dashboards

Select your OpenSearch service → Dashboards

Search logs in the fluentbit index

📌 Tech Stack
Kubernetes
Fluent Bit
OpenSearch (via Aiven)
OpenSearch Dashboards
ConfigMap, DaemonSet, YAML
(Kind cluster used locally for testing)
--------------------------------------------------------------------

🙌 Author
Sathwikaa Devarapalli
This project was built to explore observability and secure external log forwarding using open-source tools and managed cloud services.
