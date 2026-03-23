### Key Concepts and Components of Splunk

- **Splunk** is both the name of the company and its product, used primarily as a **SIEM tool** akin to QRadar, LogRhythm, Sentinel, and Sumo Logic.
- The name “Splunk” originates from the term “spelunking,” meaning **cave exploration**, symbolizing the deep exploration of logs and data.

---

### Splunk Architecture: Three Major Components

| Component       | Description                                                                                       | Key Details                                                                                                                               |
| --------------- | ------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| **Forwarders**  | Agents responsible for collecting logs/data from source devices and sending them forward.         | Two types: **Universal Forwarder** (lightweight, just forwards data) and **Heavy Forwarder** (parses and filters data before forwarding). |
| **Indexer**     | Receives data from forwarders, parses, normalizes, indexes (stores) and labels data.              | Indexing = labeling/log categorization + writing data to disk/database storage.                                                           |
| **Search Head** | User interface for querying indexed data, writing search queries, creating alerts and dashboards. | Communicates with indexers to fetch and display search results.                                                                           |

- **Universal Forwarder** acts like a delivery agent: it collects specified logs and forwards them to an indexer or heavy forwarder.
- **Heavy Forwarder** performs additional tasks such as **parsing** and **normalization** (filtering unwanted data, mapping fields), reducing data volume before sending it to the indexer.
- The **Indexer** stores, parses, normalizes, and indexes the incoming data, assigning meaningful labels (indexes) for easier retrieval.
- The **Search Head** is where users (analysts) write queries (using SPL - Search Processing Language) to explore and analyze the data.

---

### Data Collection Methods

- **Agents (Forwarders):** Installed on source devices to collect logs.
- **APIs:** Used to fetch data from third-party applications or tools where agents cannot be installed. APIs provide targeted queries to obtain specific log data, such as login activities.

---

### Indexing and Data Storage Details

- **Indexing** involves **normalizing** and **parsing** raw logs, tagging them with index names for easy retrieval.
- Index names are customizable and help categorize logs (e.g., “Network,” “Firewall,” “DNS”).
- Data is stored in **buckets**, which represent different lifecycle stages of stored data:

|Bucket Type|Description|Additional Info|
|---|---|---|
|**Hot Bucket**|Stores the most recent and actively indexed logs.|Fresh data, actively written and queried.|
|**Warm Bucket**|Receives older logs rolled over from hot buckets after configured time/size threshold.|Slightly older data still searchable.|
|**Cold Bucket**|Stores older data rolled over from warm buckets.|Long-term storage, still searchable but older.|
|**Frozen**|Logs older than a defined retention period; by default, deleted automatically by Splunk.|Can be archived externally instead of deleted.|
|**Archive**|External storage for logs preserved beyond frozen stage, not searchable unless restored.|Archived logs require **thawing** (restoring) to be searchable again.|
|**Thawed Bucket**|Data restored from archive to be searchable again.|Enables querying of very old logs when needed.|

- Rolling data from hot → warm → cold buckets is configurable by size or age (e.g., 30 days, 90 days thresholds).
- Archiving and thawing provide flexibility to store logs long-term without consuming expensive indexed storage.

---

### Core Concepts Summary

- **Splunk Architecture:** Forwarders → Indexers → Search Heads
- **Forwarders:** Universal (lightweight, data collection & forwarding), Heavy (parsing/filtering before forwarding)
- **Indexer:** Normalize, parse, index (store) logs with meaningful labels (indexes)
- **Search Head:** Query interface for retrieving and analyzing indexed data
- **Data Lifecycle:** Hot → Warm → Cold → Frozen → Archive → Thawed (for old log management)
- **Licensing:** Based on data indexed; heavy forwarders can reduce volume and license cost
- **Data Collection:** Agents preferred; APIs used when agents cannot be installed
- **Practical Setup:** Splunk Enterprise for indexing/searching; Universal Forwarders on source devices

---

### Important Terminology

|Term|Definition|
|---|---|
|**Splunk Universal Forwarder**|Lightweight agent to collect and forward logs without parsing/filtering|
|**Heavy Forwarder**|Forwarder with parsing and filtering capabilities to reduce data sent to indexers|
|**Indexer**|Component responsible for parsing, normalizing, indexing (storing) logs|
|**Search Head**|User interface for querying indexed data and creating alerts/dashboards|
|**Index**|A labeled category or container for logs, customizable by the user|
|**Bucket**|Physical storage unit for logs categorized by age: hot, warm, cold, frozen, archive, thawed|
|**Parsing**|Process of structuring and cleaning raw log data for easier analysis|
|**Normalization**|Standardizing log formats for consistent indexing and searching|
|**API (Application Programming Interface)**|Code-based interface to fetch specific data from third-party applications without agents|
|**SPL (Search Processing Language)**|Query language used in Splunk to search and analyze log data|

