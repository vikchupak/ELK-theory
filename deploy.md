# Deploy plan

- Amazon OpenSearch Service + Logstash + Filebeat > deploy to AWS EKS.

### **1️⃣ Create EKS cluster**

* Make sure **OIDC provider is enabled** for IRSA (needed for pods to authenticate to OpenSearch via IAM).
* Create **namespaces** for apps, logging, etc.

### **2️⃣ Create OpenSearch domain (before shipping logs)**

* Better to create the **OpenSearch domain first**, so Logstash/Filebeat can be configured to send logs immediately.
* Configure **VPC access** if your EKS is private.
* Set **master user** (IAM role via IRSA or internal credentials).

### **3️⃣ Deploy your microservices app**

* Make sure pods are running and producing logs.
* Label pods/namespaces if you want selective logging.

### **4️⃣ Deploy Logstash**

* Configure it with **OpenSearch endpoint + credentials**.
* Optional: configure **filters, grok patterns, enrichments**.
* Expose a **beats input port** (5044) so Filebeat can send logs.

### **5️⃣ Deploy Filebeat (or Fluent Bit) as DaemonSet**

* Collect logs from all pods/nodes.
* Point **output to Logstash** (or directly to OpenSearch if you skip Logstash).
* Add **Kubernetes metadata** (pod name, namespace, labels).

### **6️⃣ Verify logs flow**

* Make sure logs appear in OpenSearch.
* Test queries and dashboards in OpenSearch Dashboards.

---

## **Optional / recommended additions**

1. **IAM Role for Service Account (IRSA)**

   * Assign to Logstash/Filebeat pods if using IAM auth for OpenSearch.

2. **Kubernetes secrets**

   * If using internal username/password, store in secrets instead of plain text.

3. **Metrics collection**

   * Use Metricbeat or Prometheus exporters for pod/cluster metrics.

4. **Retention & index management**

   * Configure OpenSearch index rollover policies for log retention.

5. **Alerting & dashboards**

   * OpenSearch Dashboards allows alerts and visualizations.

---

✅ **Corrected plan (recommended order)**

1. Create EKS cluster + enable OIDC for IRSA
2. Create OpenSearch domain + configure access
3. Deploy microservices app
4. Deploy Logstash (with pipeline config pointing to OpenSearch)
5. Deploy Filebeat DaemonSet pointing to Logstash
6. Verify logs, dashboards, metrics, retention
