live editor : https://mermaid.live/edit

```mermaid
flowchart TD
    A[Flink] --> B(Secor)
    B --> C{Kafka}
    C -->E[Dynamic_attributes_hourly]
    C -->F[Dynamic_attributes_hourly_gdpr]
```