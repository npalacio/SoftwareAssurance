# Assurance Case 5
**Assurance Case:** Elasticsearch's snapshot and restore feature (*entity*) acceptably (*value*) supports business continuity in the event of a disaster (*property*).

![Assurance Case 5](./Images/Claim5.png)  
**Assessment:** The availablility of the required evidence:
| Evidence  | Description          | Availability  |
| --------- | -------------------- | ------------ |
| *E1* - Elasticsearch restore snapshot API procedures | Required to prove C4 - Documentation needed to prove Elasticsearch restore snapshot API can restore snapshot of a cluster.  | **Pass** - [Restore a Snapshot](https://www.elastic.co/guide/en/elasticsearch/reference/current/snapshots-restore-snapshot.html) |
| *E2* - Logs of system activity | Required to prove C5 - Documentation needed to prove Elasticsearch logs all activity happening on the system  | **PASS** - [Logs Monitoring](https://www.elastic.co/guide/en/logs/guide/7.9/logs-overview.html) |
