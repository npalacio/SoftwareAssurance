# Assurance Case 4
**Assurance Case:** Elasticsearch (*entity*) responds to all user requests (*property*) witin an acceptable amount of time (*value*).

![Assurance Case 4](./Images/Claim4.png)  

**Assessment:** The availablility of the required evidence:
| Evidence  | Description          | Availability  |
| --------- | -------------------- | ------------ |
| *E1* - Request prioritzation configuration | Required to prove C4 - Documentation needed to prove Elasticsearch can be configured to respond to high priority requests sooner than low priority | **FAIL** - No such feature could be found |
| *E2* - Cross cluster replication procedure | Required to prove C5 - Report needed to prove that cluster can be configured detailing the procedures that will be taken | **PASS** - [Cross cluster replication](https://www.elastic.co/guide/en/elasticsearch/reference/current/xpack-ccr.html) can be configured |
| *E3* - Rate limit configuration | Required to prove C6 - Documentation needed to prove Elasticearch has a configurable rate limit detailing reasoning for the configured threshold | **WARN** - There is a [Rate Limit](https://www.elastic.co/guide/en/cloud/current/ec-api-rate-limiting.html) feature but documentation on configuring thresholds could not be found |
| *E4* - IP filter configuration to reject external | Required to prove C7 - Documentation needed to prove Elasticearch has a configurable IP filter that can reject external IP addresses | **PASS** - There is a highly configurable [IP filter](https://www.elastic.co/guide/en/elasticsearch/reference/current/ip-filtering.html) feature that can be used to suit the business needs |