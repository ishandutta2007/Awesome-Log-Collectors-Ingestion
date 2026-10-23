# Awesome-Log-Collectors-Ingestion

# Top Log Collectors & Ingestion Ecosystem

**Curated List of SaaS Products & Open-Source GitHub Projects**  
*Focused on Log Forwarding, Pipeline Transformation & Self-Hosted Collection Agents*  
**Last updated: October 2026**

This repository tracks notable **SaaS platforms** and **open-source projects** for **Log Collectors & Ingestion**. These tools sit at the edge of your infrastructure—tailing files, reading container logs, receiving syslog, and forwarding events to a central store—transforming messy raw output into structured, routable telemetry before it reaches your analytics backend.

**Examples** include Datadog, Cribl Stream, Splunk Forwarder, AWS CloudWatch Logs, New Relic Logs, Papertrail, and the open-source leaders Fluentd, Fluent Bit, Vector, and Logstash (the category leaders).

**Open-source emphasis**: This section is heavily expanded with every major active collector and pipeline for self-hosting, avoiding per-GB forwarding fees, and controlling what leaves your network. The open-source landscape is anchored by **Vector** (the Rust-based performance leader with VRL transformation), **Fluent Bit** (the lightweight C collector for Kubernetes and edge), **Fluentd** (the CNCF unified logging layer with the largest plugin ecosystem), and **Logstash** (the Elastic Stack's heavyweight transformation engine), with **Promtail** (deprecated) succeeded by **Grafana Alloy** for Loki-native collection.

Contributions welcome! Open a PR to add/update entries. Keep descriptions factual and link to official sites.

## Table of Contents
- [SaaS/Hosted Platforms](#saas-hosted-platforms)
- [Open-Source GitHub Projects](#open-source-github-projects)
- [How to Contribute](#how-to-contribute)
- [Disclaimer](#disclaimer)

## SaaS/Hosted Platforms

| Product | Description | Pricing | Free Tier Limit |
| :--- | :--- | :--- | :--- |
| **[Cribl Stream](https://cribl.io/)** | Vendor-agnostic observability pipeline for routing, reducing, and transforming telemetry before backend ingestion. | Credit-based: 12 MB/s throughput = 1.3–2.0 credits/hr depending on tier; 60 MB/s = 6.5–10.2 credits/hr . | **Cribl.Cloud Free**: 1 TB/day ingest, single instance, community support. No time limit. |
| **[Datadog](https://www.datadoghq.com/)** | Agent-based log collection with built-in processing pipelines, Sensitive Data Scanner, and routing to Datadog or archives. | Ingest billed per GB; indexing billed per event. Overage at **50% premium**; daily quotas stop indexing when hit . | **Infrastructure Free**: 5 hosts, 1-day metric retention. Log collection requires paid Log Management. |
| **[Sumo Logic](https://www.sumologic.com/)** | Cloud-native collector with ingest budgets, processing rules, and Flex pricing for unlimited ingest. | Essentials: per-GB ingest pricing. **Flex**: $0 ingest, **$3.14/TB scanned** . | **60-day free trial** with full access. No permanent free tier. |
| **[Splunk Forwarder](https://www.splunk.com/)** | Universal and heavy forwarders for collecting and routing data to Splunk indexers. | Free forwarder software; costs incurred at the Splunk indexing tier ($150–$225/GB/day) . | Forwarder itself is free. **Splunk Free** (500 MB/day) applies to the indexing tier, not the forwarder. |
| **[AWS CloudWatch Logs](https://aws.amazon.com/cloudwatch/)** | Native AWS log collection via CloudWatch Agent, VPC Flow Logs, and subscription filters. | **$0.50/GB ingested**, **$0.03/GB/month archived**, **$0.005/GB scanned** by Logs Insights (US regions) . | **5 GB data/month** free (ingestion, archive, and Insights scans combined) . |
| **[New Relic Logs](https://newrelic.com/)** | Log collection integrated with New Relic's observability platform, with 8/15/30-day retention tiers. | **8-day**: $55/GB/day; **15-day**: $65/GB/day; **30-day**: $75/GB/day (annual) . | **100 GB/month free** for the first 100 GB ingested; 8-day retention. |
| **[Papertrail (SolarWinds)](https://www.papertrail.com/)** | Simple syslog-based log aggregation with live tail, search, and alerting. | Starting at **$7/month** . Scales with volume (e.g., 1 GB/month ~$10). | **Papertrail Free**: 50 MB/month, 48-hour searchable retention, 1 user. |

## Open-Source GitHub Projects

- **[Vector](https://github.com/vectordotdev/vector)** [![GitHub Stars](https://img.shields.io/github/stars/vectordotdev/vector?style=social&color=white)](https://github.com/vectordotdev/vector/stargazers)  
  **The Rust-based performance and transformation leader.** Datadog-owned but fully open-source (MPL 2.0). **Throughput**: reached **76.7 MiB/s** disk throughput in the official Vector test harness—highest among all collectors tested, with **188 MiB** peak memory . **VRL (Vector Remap Language)** handles parsing, enrichment, and routing in one place. **v0.55.0** added native `windows_event_log` source with pull-mode subscriptions and bookmark checkpointing . **Caveat**: the Windows Event Log source is maturing—a freeze bug shipped in v0.55 and was fixed in v0.56 . Best for high-volume Linux/container pipelines with heavy transformation needs.

- **[Fluentd](https://github.com/fluent/fluentd)** [![GitHub Stars](https://img.shields.io/github/stars/fluent/fluentd?style=social&color=white)](https://github.com/fluent/fluentd/stargazers)  
  **The CNCF unified logging layer.** ~14k stars . Ruby-based with the **largest plugin ecosystem** (1,000+ plugins). **Throughput**: ~26.1 MiB/s disk throughput, 890 MiB peak memory in the Vector harness . **Weaknesses**: no fleet management layer; the project's own docs warn that "any fluentd plugin can unknowingly break fluentd completely" . Best for teams already standardized on Fluentd's plugin ecosystem and willing to vet plugins carefully.

- **[Fluent Bit](https://github.com/fluent/fluent-bit)** [![GitHub Stars](https://img.shields.io/github/stars/fluent/fluent-bit?style=social&color=white)](https://github.com/fluent/fluent-bit/stargazers)  
  **The lightweight C collector for Kubernetes and edge.** ~5.7k stars . **Performance**: **31,300 logs/second** in the VictoriaMetrics benchmark—6x Fluentd's ceiling under identical constraints . **Kubernetes-native**: the default collector in many distributions, deployed as a DaemonSet. **Caveats**: exceeded 1 GiB memory under peak load and was OOM-killed in one benchmark; produced **34 malformed records/hour** during containerd file rotation . **No fleet management layer** . Best for Kubernetes DaemonSet collection where low footprint and high throughput matter more than transformation power.

- **[Logstash](https://github.com/elastic/logstash)** [![GitHub Stars](https://img.shields.io/github/stars/elastic/logstash?style=social&color=white)](https://github.com/elastic/logstash/stargazers)  
  **The Elastic Stack's heavyweight transformation engine.** ~14.5k stars . **Strongest transformation catalog**: Grok, conditionals, and mature filters for pulling structure from messy text . **v9.4.4** requires **Java 21+** with a bundled JDK—the heaviest per-node footprint of any collector here . **Durability caveats**: persistent queues and dead letter queues are **disabled by default** . **Throughput**: 3.1 MiB/s in the Vector harness—lowest of the group, with 46.5 CPU sys max . Best for teams committed to Elasticsearch that need Grok parsing as a central aggregation tier, not on endpoints.

### Additional Strong Open-Source Options

- **Grafana Alloy** — The successor to Promtail (which is deprecated). Vendor-neutral OpenTelemetry collector distribution with Loki-native log collection, built-in Prometheus pipelines, and a unified config language. Best for teams moving off Promtail that want a future-proof collector within the Grafana ecosystem.
- **OpenTelemetry Collector** — Vendor-neutral collector for logs, metrics, and traces. Receivers for filelog, syslog, journald, and Windows Event Log; processors for filtering and transformation. Best for teams standardizing on OTel and wanting a single agent for all telemetry signals.
- **Vector + Fluentd side-by-side** — Vector ships a `fluent` source that speaks Fluentd's forward protocol, enabling incremental migration rather than a flag-day cutover .

## How to Contribute

1. Fork the repo.
2. Add/edit entries in `README.md` (follow existing format).
3. Include: name, link, 1–2 sentence description, and whether it's SaaS or open-source.
4. Submit PR with a short explanation.

Star the repo if you find it useful!

## Disclaimer

- This is a **community-curated** list — not exhaustive and not an endorsement.
- **Collector performance benchmarks are workload-specific**: Vector led disk throughput (76.7 MiB/s) while Fluent Bit led log throughput (31,300 logs/s); Logstash was slowest on disk (3.1 MiB/s) but has the strongest transformation catalog . Match the tool to your bottleneck.
- **"Free" forwarders still incur backend costs**: Splunk Forwarder and CloudWatch Agent are free software, but the indexing/storage tier charges per GB . Model the full pipeline, not just the collector.
- **Fluent Bit and Fluentd lack fleet management**: configuration changes and upgrades happen per-host or via external tooling—a real operational burden at thousands of endpoints .
- **Promtail is deprecated**: migrate to Grafana Alloy for Loki-native collection. Do not select Promtail for new deployments.

---

**Made for SREs, platform engineers, and DevOps teams building telemetry pipelines.**  
Let's make log collection more open, performant, and appropriately scoped.
