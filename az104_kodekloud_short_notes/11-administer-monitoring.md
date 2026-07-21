# Module 11 - Administer Monitoring

## Azure Monitor model

Azure Monitor collects telemetry from applications, operating systems, Azure resources, subscriptions, tenants, on-premises systems, and other clouds.

```text
Data sources
  -> collection (platform, diagnostic settings, AMA/DCR, APIs)
    -> data platform (metrics, logs, traces, changes)
      -> analysis/visualization/response
```

Consumers include Metrics Explorer, Log Analytics, Workbooks, Insights, dashboards, Grafana, alerts, autoscale, Logic Apps, Functions, and other integrations.

## Metrics vs logs

| Metrics | Logs |
|---|---|
| Numeric time-series data | Structured records in tables |
| Near real-time and efficient for alerting | Rich context and flexible correlation |
| Common for CPU, latency, requests, capacity | Common for events, diagnostics, audit, and application records |
| Queried with metric tools | Queried with Kusto Query Language (KQL) |

Use metrics for fast threshold detection and logs for deeper investigation.

## Activity Log and resource logs

- **Azure Activity Log:** Subscription-level control-plane events such as create, update, delete, policy, service health, and administrative actions.
- **Resource logs:** Service-specific data-plane/operational logs. They are not all retained centrally unless diagnostic settings route them.
- **Entra logs:** Sign-in and audit events from Microsoft Entra ID.

A diagnostic setting can send supported logs/metrics to Log Analytics, Storage, Event Hubs, or partner destinations.

## Log Analytics workspace

A workspace stores and queries Azure Monitor Logs.

Design considerations:

- Region, data residency, access model, retention, cost, and network access.
- Centralized vs distributed workspace strategy.
- Table-level ingestion and retention needs.
- RBAC for workspace and resource-context access.

### KQL basics

```kusto
Heartbeat
| summarize LastHeartbeat=max(TimeGenerated) by Computer
| order by LastHeartbeat desc
```

```kusto
AzureActivity
| where TimeGenerated > ago(24h)
| where ActivityStatusValue == "Failure"
| project TimeGenerated, OperationNameValue, ResourceGroup, Caller
| order by TimeGenerated desc
```

```kusto
Perf
| where TimeGenerated > ago(1h)
| where CounterName == "% Processor Time"
| summarize AvgCPU=avg(CounterValue) by Computer, bin(TimeGenerated, 5m)
| render timechart
```

Common operators: `where`, `project`, `extend`, `summarize`, `count`, `distinct`, `join`, `sort/order`, `top`, and `render`.

## Azure Monitor Agent and DCRs

Use **Azure Monitor Agent (AMA)** for supported VM/server telemetry. AMA uses **Data Collection Rules (DCRs)** to define:

- Which machines/resources are associated.
- Which data sources to collect.
- Transformations where supported.
- Destination workspaces/endpoints.

For non-Azure servers, Azure Arc commonly provides the Azure resource representation needed for management.

The legacy Log Analytics Agent/Microsoft Monitoring Agent (MMA) was retired on August 31, 2024. Microsoft states that data upload can stop after March 2, 2026. Do not design new monitoring around MMA.

## Azure Alerts

An alert rule contains:

1. **Scope:** Resource(s) being monitored.
2. **Condition:** Signal and trigger logic.
3. **Actions:** One or more action groups.
4. **Details:** Name, severity, enablement, identity, and other settings.

Alert types include metric, log search, activity log, and service health alerts.

Action groups can send email/SMS/push/voice notifications or trigger webhooks, Functions, Logic Apps, Automation, ITSM, and other integrations.

Alert processing rules can suppress notifications or apply action groups during maintenance and other scenarios without rewriting every alert rule.

## Insights and network monitoring

Azure Monitor Insights provides curated monitoring for VMs, storage, networks, containers, and other supported services. For networking, know Network Watcher and Connection Monitor for reachability, latency, topology, and troubleshooting.

## Troubleshooting missing telemetry

1. Confirm the resource supports the desired metric/log.
2. Check diagnostic settings or DCR association.
3. Verify AMA/extension health and managed identity permissions.
4. Check destination workspace and table.
5. Expand the query time range and verify filters.
6. Check ingestion latency, network restrictions, and data caps/transformations.

## Exam cues

- Activity Log = subscription control-plane events; resource logs = service-specific logs.
- Metrics are numerical time series; logs are queried with KQL.
- AMA + DCR is the current agent model.
- Alert rule decides when; action group decides what happens.
- Diagnostic settings route data; a Log Analytics workspace stores/query logs.
