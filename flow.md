Elastic Company products:
- Elasticsearch is the search & analytics engine.
- Filebeat is a lightweight log shipper.
- Kibana
- Logstash

# How the **classic ELK pipeline** works

---

## 1️⃣ Filebeat → Logstash → Elasticsearch

**Filebeat** is a lightweight log shipper. Its main purpose is to **collect logs from files** (or other sources) and send them **to Logstash or directly to Elasticsearch**.

**Pipeline diagram:**

```
[App Logs / Files] ---> [Filebeat] ---> ([Logstash]) ---> [Elasticsearch] ---> [Kibana]
```

---

## 2️⃣ How Logstash receives logs

Logstash runs as a service (usually JVM-based) and has a **plugin-based pipeline**:

```
input { ... } → filter { ... } → output { ... }
```

### Example Logstash pipeline (`pipeline.conf`):

```conf
input {
  beats {
    port => 5044  # Filebeat ships logs here
  }
}

filter {
  grok {
    match => { "message" => "%{COMMONAPACHELOG}" }
  }
  date {
    match => [ "timestamp" , "dd/MMM/yyyy:HH:mm:ss Z" ]
  }
}

output {
  elasticsearch {
    hosts => ["http://elasticsearch:9200"]
    index => "app-logs-%{+YYYY.MM.dd}"
  }
}
```

**Explanation:**

* **Input:** Logstash listens on a port for logs from Filebeat (or other shippers).
* **Filter:** Optional parsing, enrichment, transformations (like `grok`, `mutate`, `date`, `geoip`).
* **Output:** Sends processed logs to Elasticsearch (or any other storage).

---

## 3️⃣ Filebeat → Logstash vs Filebeat → Elasticsearch

| Setup                    | Pros                                                     | Cons                                        |
| ------------------------ | -------------------------------------------------------- | ------------------------------------------- |
| Filebeat → Logstash → ES | Can transform logs, enrich data, handle multiple sources | Logstash consumes more resources            |
| Filebeat → ES directly   | Lightweight, simpler setup                               | Limited processing, minimal transformations |

---

💡 **Key takeaway:**

* **Filebeat** just **ships raw logs**.
* **Logstash** receives those logs via a **plugin input (beats, tcp, http, etc.)**, processes them through filters, and forwards them to Elasticsearch.
