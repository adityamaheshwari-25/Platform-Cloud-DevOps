# Module 11 — Monitoring Tools

## Monitoring services compared

| Service | Main purpose | Typical question |
|---|---|---|
| Azure Advisor | Personalized recommendations | How can I improve cost, security, reliability, performance, or operations? |
| Azure Service Health | Personalized Azure service-impact information | Is an Azure incident or planned maintenance affecting me? |
| Azure Monitor | Collect and analyze telemetry | What is happening inside my applications and resources? |

## Azure Advisor

Analyzes deployed resources and provides personalized recommendations in five categories:

- Cost
- Security
- Reliability
- Performance
- Operational excellence

Advisor recommends improvements; it does not normally apply them automatically.

## Azure Service Health

Provides a personalized view of Azure platform events that may affect your subscriptions and resources.

| View | Scope |
|---|---|
| Azure Status | Public, broad status of Azure services and regions |
| Service Health | Personalized incidents, planned maintenance, and health advisories |
| Resource Health | Health of an individual Azure resource |

Service Health alerts can notify teams about relevant incidents or planned maintenance.

## Azure Monitor

Azure Monitor collects, analyzes, visualizes, and acts on telemetry from applications, Azure resources, operating systems, and supported external environments.

### Core data types and features

- **Metrics:** numeric time-series values, such as CPU percentage or request count.
- **Logs:** detailed records queried through Log Analytics, commonly with Kusto Query Language (KQL).
- **Application Insights:** application performance monitoring for requests, failures, dependencies, and traces.
- **Alerts:** notify or trigger actions when conditions are met.
- **Action groups:** reusable notification or automation targets for alerts.
- **Workbooks and dashboards:** visualize and combine monitoring data.

### Typical flow

`Collect telemetry → Analyze/query → Visualize → Alert or automate`

## Choosing the right tool

- Need recommendations to reduce cost? **Azure Advisor.**
- Need notice of planned Azure maintenance? **Azure Service Health.**
- Need to inspect a VM’s CPU trend? **Azure Monitor metrics.**
- Need to query detailed event records? **Log Analytics logs.**
- Need application request and dependency performance? **Application Insights.**
- Need the health of one VM right now? **Resource Health.**

## Exam traps

- Advisor suggests optimizations; Monitor observes telemetry.
- Azure Status is global/public; Service Health is personalized.
- Metrics are lightweight numeric time series; logs contain richer records.
- Resource Health focuses on one resource, not the entire Azure platform.

## Quick check

1. Which tool gives personalized optimization recommendations? **Azure Advisor.**
2. Which tool reports planned maintenance affecting your subscription? **Azure Service Health.**
3. Which service contains metrics, logs, alerts, and Application Insights? **Azure Monitor.**
4. Which feature queries detailed log data? **Log Analytics.**

## References

- [KodeKloud — Monitoring Tools](https://notes.kodekloud.com/docs/Microsoft-Azure-Fundamentals-AZ-900/Monitoring-Tools/Introduction)
- [KodeKloud — Azure Advisor](https://notes.kodekloud.com/docs/Microsoft-Azure-Fundamentals-AZ-900/Monitoring-Tools/Azure-Advisor)
- [KodeKloud — Azure Service Health](https://notes.kodekloud.com/docs/Microsoft-Azure-Fundamentals-AZ-900/Monitoring-Tools/Azure-Service-Health)

