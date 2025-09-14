- Amazon OpenSearch Service + Logstash + Filebeat > deploy to AWS EKS.
- AWS uses a term "domain" to point to "OpenSearch cluster". While OpenSearch itself uses the term cluster internally, AWS adds domain to emphasize:
  - Managed service boundary (you don’t manage individual nodes directly).
  - Billing and resource grouping.
  - Unique endpoint for API access (each domain gets its own URL).

# AWS-managed service
- **Amazon Elasticsearch Service.** Before **2021**. It ran the official `Elasticsearch 7.x and Kibana 7.x` from Elastic under the Apache 2.0 license.
- **Amazon OpenSearch Service.** After the license change in **2021** (Elastic switched to SSPL/Elastic License).
  - AWS could no longer offer the latest Elasticsearch/Kibana under Apache 2.0.
  - AWS forked Elasticsearch and Kibana 7.10 → renamed the service to Amazon OpenSearch Service.
  - Kibana was forked → OpenSearch Dashboards.
  - AWS continued maintaining backward-compatible API and features.

---

- Only Elasticsearch and Kibana were effected by the licence change. Logstash remains the same.
- **Latest Elasticsearch 8.x + Kibana 8.x → require paid subscription.**

# OpenSearch

- OpenSearch is a community-driven, open-source search and analytics engine.
- It is a fork of Elasticsearch 7.10 after Elastic changed their license.
- **Maintained by Amazon Web Services and the OpenSearch community.**

## 1️⃣ Open-source status

* **OpenSearch is fully open-source** under **Apache 2.0 license**.
* This means anyone can use, modify, fork, or redistribute it freely.

---

## 2️⃣ AWS role

* **AWS initiated OpenSearch** in 2021 as a **fork of Elasticsearch 7.10 and Kibana 7.10**.
* Why: Elastic changed the license of Elasticsearch/Kibana to SSPL/Elastic License, making newer versions not fully open-source.
* AWS wanted a **continuation of the open-source project** that would remain free and compatible with existing Elasticsearch 7.10 clients and APIs.

---

## 3️⃣ Maintained by AWS

* **AWS leads the development and release process**, contributing major code, fixes, and new features.
* However, **OpenSearch is community-driven**, so anyone can contribute.
* It’s like: AWS is the **primary steward** and sponsor, but the project is open for the community.

---

💡 **Key takeaway:**

* **Yes, AWS started OpenSearch**, but it’s **fully open-source**.
* Other companies or contributors can also participate in development.
