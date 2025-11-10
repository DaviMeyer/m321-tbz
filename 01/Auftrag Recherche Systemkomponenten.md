# Recherche: Proxy-Systeme, Message Broker und RPC (Kurzüberblick & Vergleich)

Ich habe mich für die Kategorie "Proxy-Systeme, Message broker, RPC" entschieden, weil sie in verteilten Systemen eine zentrale Rolle für Entkopplung, Resilienz und Skalierung spielt. Im Folgenden findest du eine strukturierte Recherche, die bekannte Softwarelösungen aufführt, ihre wichtigsten Eigenschaften vergleicht, typische Einsatzszenarien beschreibt und eine Entscheidungshilfe bietet. Wenn du möchtest, kann ich daraus anschliessend eine Präsentation (Foliensatz) oder eine kurze Demo-Anleitung für 1–2 ausgewählte Systeme erstellen.

Inhalt
- Kurzbeschreibung der Kategorie
- Wichtige Anforderungen / Auswahlkriterien
- Übersicht der gängigen Lösungen (Kurzportraits)
- Vergleichstabelle (Eigenschaften, Pros/Cons)
- Empfehlungen nach Anwendungsfall
- Vorschlag für Bearbeitungs-/Präsentationsplan (45–90 Min)
- Quellen & weiterführende Links

---

## 1. Kurzbeschreibung der Kategorie
Proxy-Systeme, Message Broker und RPC-Komponenten dienen dazu, Kommunikation zwischen verteilten Komponenten zu vermitteln, zu vermitteln oder zu vereinfachen. Ziele sind:
- Entkopplung von Sender und Empfänger (Zeitlich & logisch)
- Asynchrone Kommunikation (Fire-and-forget, Queues, Pub/Sub)
- Lastspitzen puffern und glätten
- Routing, Transformation und Protokollwechsel
- Unterstützung für synchrone Aufrufe über Netzwerk (RPC) mit Frameworks

Beispiele für typische Anforderungen:
- Garantierte Auslieferung (at-least-once / at-most-once / exactly-once)
- Persistenz vs. rein Speicherbasiert (in-memory)
- Partitionierung / Skalierbarkeit
- Ordering (Globale Reihenfolge vs. Partition-basierte Reihenfolge)
- Latenz / Durchsatz
- Betriebsaufwand / Obsorgeaufwand
- Integrationsprotokolle (AMQP, MQTT, HTTP, gRPC, STOMP, Kafka protocol)

---

## 2. Wichtige Auswahlkriterien (Anforderungen)
Bei der Auswahl einer Lösung solltest du prüfen:
- Nachrichtenmodell: Queue vs. Pub/Sub vs. Stream
- Persistenz und Retention: wie lange bleiben Nachrichten erhalten?
- Durchsatz und Latenz: Verarbeitung hoher Lasten vs. niedrige Latenz
- Zustellgarantien: at-least-once / at-most-once / exactly-once
- Ordering-Garantien: global, partitioniert, none
- Skalierbarkeit: horizontale Skalierung (Sharding/Partitions)
- Betriebsaufwand: Setup, Monitoring, Replikation, Upgrades
- Ökosystem & Integrationen: Connectors, Clients, Tooling
- Sicherheit & Multi-Tenancy: AuthN/AuthZ, TLS, ACLs
- Geo-Replikation / Multi-Region-Fähigkeiten
- Protokollunterstützung (AMQP, MQTT, gRPC, HTTP, Kafka-protocol)

---

## 3. Übersicht der gängigsten Lösungen (Kurzportraits)

- Apache Kafka
  - Paradigma: verteilter Commit-Log / Stream. Pub/Sub mit Partitionen.
  - Stärken: sehr hoher Durchsatz, long-term retention, gutes Ökosystem (Kafka Connect, Streams), genau-einmal Transaktionen (mit Einschränkungen).
  - Schwächen: Betriebskomplexität, Latenzgrenze höher als bei manchen in-memory Brokern.

- Apache Pulsar
  - Paradigma: Pub/Sub + persistent log, Trennung von Compute (Brokern) & Storage (BookKeeper).
  - Stärken: Multi-tenancy, geo-replication, Topic-Level Latenz/Retention, gute Skalierbarkeit, native Funktionen für Queue/Stream.
  - Schwächen: noch komplexere Architektur, kleineres Community-Ökosystem als Kafka (aber wachsend).

- RabbitMQ
  - Paradigma: Message broker basierend auf AMQP (Queue, Exchange, Routing).
  - Stärken: Flexibles Routing (Exchange-Types), viele Protokolle (AMQP, MQTT, STOMP), gutes Management UI, geeignet für klassische Messaging-Patterns.
  - Schwächen: Bei sehr hohem Durchsatz schlechter skalierbar als Kafka; Persistenzmodell ist nicht für große Long-Term-Streaming-Workloads optimiert.

- ActiveMQ / ActiveMQ Artemis
  - Paradigma: klassischer JMS/Broker, unterstützt AMQP/STOMP.
  - Stärken: JMS-API für Java-Apps, solide Features für traditionelle Enterprise-Messaging.
  - Schwächen: Skalierbarkeit & Performance im Vergleich zu neueren Lösungen limitiert.

- NATS / NATS Streaming (JetStream)
  - Paradigma: leichtgewichtiger Pub/Sub- und Request/Reply-Broker (JetStream bringt Persistenz).
  - Stärken: sehr geringe Latenz, einfache Architektur, leichtgewichtig, gut für Service-Mesh / Microservices Signaling.
  - Schwächen: Persistenz-Feature (JetStream) ist jünger; Ökosystem nicht so groß wie Kafka.

- Redis Streams
  - Paradigma: log-basierter Stream innerhalb Redis (in-memory mit Persistenzoptionen).
  - Stärken: Einfache Nutzung, sehr niedrige Latenz, gut für kurze Lebensdauer-Queues, einfache Consumer-Gruppen.
  - Schwächen: Skalierbarkeit & dauerhafte Speicherung erfordern Redis-Cluster-Planung; Memory-basiert -> Kosten.

- Apache ActiveMQ Apollo (veraltet) / HornetQ (eingestellt) — historisch relevant, weniger empfohlen neu zu starten.

- Google Pub/Sub (Managed)
  - Paradigma: managed Pub/Sub as-a-service.
  - Stärken: vollständig managed, automatische Skalierung, einfache Integration in GCP.
  - Schwächen: Vendor-Lockin, Preismodell, Latenz- & Feature-Unterschiede vs. self-hosted.

- AWS SQS / SNS / Kinesis (Managed)
  - Paradigma: SQS = Queue, SNS = Pub/Sub, Kinesis = Streaming (ähnlich Kafka).
  - Stärken: managed, nahtlose Integration in AWS-Ökosystem.
  - Schwächen: Limitierungen bei ordering (SQS FIFO existiert mit Einschränkungen), Preis, Vendor-Lockin.

- Apache Kafka-Alternativen / Ergänzungen: Redpanda (Kafka-API-kompatibel, Single-binary), Confluent Platform (kommerziell + zusätzliche Tools).

- gRPC & Thrift (RPC-Frameworks)
  - Paradigma: synchrone Remote Procedure Calls, oft mit Protobuf/IDL.
  - Stärken: sehr niedrige Latenz, starke Typisierung, einfache Sync-API.
  - Schwächen: tighter coupling; weniger gut für asynchrone, resiliente Systeme ohne Broker.

---

## 4. Vergleichstabelle (Kurzfassung)

| System | Modell | Persistenz | Durchsatz | Latenz | Ordering | Exactly-once | Skalierung | Betriebsaufwand | Typische Use-Cases |
|---|---:|---:|---:|---:|---|---:|---:|---:|---|
| Kafka | Stream (log) | Hoch (Retentions) | Sehr hoch | Mittel | Partition-basiert | Ja (Transaktionen, kompliziert) | Sehr gut | Mittel–hoch | Event sourcing, Analytics, Streaming ETL |
| Pulsar | Stream/Queue | Hoch | Sehr hoch | Mittel | Partition/Shard | Ja (fortschrittlich) | Sehr gut | Hoch | Multi-tenant Streaming, geo-replication |
| RabbitMQ | Broker (AMQP) | Persistenz möglich | Mittel | Niedrig–Mittel | Queue/exchange-basiert | Nein (nur at-least-once) | Mittel | Mittel | Task queues, komplexes Routing, Integration |
| NATS / JetStream | Pub/Sub + Persistenz | Optional (JetStream) | Hoch (NATS) | Sehr niedrig | Partitioniert/Subject | JetStream bietet stronger guarantees | Gut | Niedrig–Mittel | Low-latency messaging, Control planes |
| Redis Streams | Stream (in-memory) | Ja (AOF/RDB) | Hoch (Memory) | Sehr niedrig | Consumer Groups | Limitiert | Mittel (Cluster nötig) | Mittel | Lightweight streams, transient queues |
| AWS Kinesis | Stream (managed) | Hoch | Hoch | Mittel | Shard-based | Teilweise | Sehr gut | Niedrig (managed) | Cloud analytics, streaming |
| Google Pub/Sub | Pub/Sub (managed) | Hoch | Hoch | Mittel | Ordering optional | Nein/Begrenzt | Sehr gut | Niedrig | Cloud-native pub/sub |

Kurz-Interpretation:
- Für hohe Durchsatz-Streaming-Workloads: Kafka oder Pulsar.
- Für low-latency Service-Kommunikation & Request/Reply: NATS, gRPC.
- Für flexible Routing & klassische MQ-Use-Cases: RabbitMQ.
- Für einfache/budget-freundliche Streams/Queues mit geringem Ops-Aufwand: managed cloud services (SQS/SNS, Pub/Sub) oder Redis Streams in kleineren Setups.

---

## 5. Vor- und Nachteile (Ausgewählte Systeme)

Apache Kafka
- Vorteile: extrem skalierbar, robustes Ecosystem (Connect, Streams), gute Latenz bei hohem Durchsatz, genau-einmal möglich.
- Nachteile: komplexe Betriebsführung (Zookeeper bei älteren Versionen, Confluent/Redpanda erleichtern), nicht ideal für millisekunden-latenz-kritische Request/Reply.

RabbitMQ
- Vorteile: sehr gutes Routing, viele Protokolle, einfach verständliches MQ-Modell, gutes UI.
- Nachteile: horizontale Skalierung anspruchsvoller; nicht für massive Streams/Analytics optimiert.

NATS / JetStream
- Vorteile: sehr geringe Latenz, einfache Architektur, leicht zu betreiben; JetStream bringt Persistenz/Retention.
- Nachteile: Persistente Features sind jünger, ökosystem kleiner als Kafka.

Apache Pulsar
- Vorteile: klare Trennung Broker/Storage, native Geo-Replication, Multi-Tenancy, gut für Cloud-Native multi-tenant Umgebungen.
- Nachteile: Architekturkomplexität (BookKeeper), Betriebswissen nötig.

Redis Streams
- Vorteile: super niedrige Latenz, einfach einzusetzen wenn Redis bereits vorhanden.
- Nachteile: Memory-basiert → Kosten; Nicht ideal als langfristiges Event-Log bei großen Datenmengen.

Cloud Managed (SQS/SNS/Kinesis/PubSub)
- Vorteile: sehr geringer Betriebsaufwand, automatische Skalierung, einfache Integration in Cloud.
- Nachteile: Vendor-Lockin, Kostenmodell, teilweise eingeschränkte Garantien/Ordering.

gRPC / Thrift (RPC)
- Vorteile: effiziente synchrone Kommunikation, stark typisiert, Streaming möglich (gRPC).
- Nachteile: stärkere Kopplung, Fehlerbehandlung/Retry-Strategien selbst bauen; nicht Ersatz für asynchrone Broker.

---

## 6. Empfehlungen nach Anwendungsfall

- Event Sourcing / Stream Processing / Data-Analytics Pipeline:
  - Empfehlung: Apache Kafka oder Apache Pulsar (Kafka als Default, Pulsar bei Multi-Tenant oder wenn BookKeeper-Storage-Vorteile wichtig).

- Task Queue / Work Distribution (Worker Pools):
  - Empfehlung: RabbitMQ (flexibles Routing), alternativ Redis Streams oder managed SQS (wenn auf AWS).

- IoT / Telemetrie mit sehr vielen kleinen Messages:
  - Empfehlung: MQTT-Broker (z. B. Mosquitto) oder NATS für sehr niedrige Latenz; für grosse Retention Kafka/Pulsar in der Backend-Pipeline.

- Low-latency Control Plane / Service Mesh Messaging:
  - Empfehlung: NATS (leichtgewichtig) oder gRPC für direkte Sync-Aufrufe.

- Multi-Region / Geo-Replication nötig:
  - Empfehlung: Pulsar (native geo-replication) oder Kafka mit MirrorMaker / Confluent Replicator (mehr Ops-Aufwand).

- Cloud-native, minimaler Ops-Aufwand:
  - Empfehlung: Cloud-managed Dienste (Pub/Sub, Kinesis, SQS/SNS).

- Exactly-once Processing erforderlich (z. B. Finanztransaktionen):
  - Empfehlung: Kafka mit Transactions & carefully designed consumers; alternativ prüfen Pulsar-Transaktionen (Stand prüfen).

---

## 7. Vorschlag für Bearbeitungs- und Präsentationsplan (45–90 Minuten)
- 0–10 Min: Auswahl von 2–3 Systemen (z. B. Kafka, RabbitMQ, NATS) und Zieldefinition (welche Aspekte verglichen werden).
- 10–40 Min: Kurzrecherche: Architektur, Guarantees, typische Use-Cases, Links zu Dokus.
- 40–60 Min: Erstellen der Vergleichstabelle und Kurzbewertung (Vor-/Nachteile).
- 60–75 Min: Fazit + Empfehlung für die Klasse / Einsatzszenarien.
- 75–90 Min (optional): Kurze Demo-Vorbereitung oder Folien für 5-min Präsentation.

---

## 8. Quellen & weiterführende Links
- Apache Kafka: https://kafka.apache.org/
- Apache Pulsar: https://pulsar.apache.org/
- RabbitMQ: https://www.rabbitmq.com/
- NATS: https://nats.io/
- Redis Streams: https://redis.io/docs/data-types/streams/
- Google Cloud Pub/Sub: https://cloud.google.com/pubsub
- AWS SQS / SNS / Kinesis: https://aws.amazon.com/sqs/ https://aws.amazon.com/sns/ https://aws.amazon.com/kinesis/
- Redpanda: https://vectorized.io/redpanda
- Confluent Platform: https://www.confluent.io/
- Vergleichsartikel und Übersichten:
  - "Kafka vs RabbitMQ vs ActiveMQ" (versch. Blogposts)
  - Pulsar vs Kafka: https://pulsar.apache.org/docs/overview/

---