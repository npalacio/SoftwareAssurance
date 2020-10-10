
#  Assurance Case 2

**Assurance Case:** The web portal (*entity*) minimizes (*property*) information disclosed during communication (*value*).

  

![Assurance Case 2](./Images/Claim2.png)

  

**Assessment:** The availablility of the required evidence:

| Evidence | Description | Availability |

| --------- | -------------------- | ------------ |

| *E1* - Elasticsearch SSL/TLS encryption | Required to prove C2 - Documentation needed to prove Elasticsearch can be configured to encrypt traffic to, from, and within a cluster| **PASS** - [Encryption] (https://www.elastic.co/guide/en/elasticsearch/reference/current/ssl-tls.html) can be configured |

| *E2* - Elasticsearch Requires SSL certificate to authenticate new nodes | Required to prove C3 - Documentation needed to prove Elasticsearch can be configured to require SSL certificates for new nodes| **PASS** - [New node SSL Certificate Authentication](https://www.elastic.co/guide/en/elasticsearch/reference/7.9/configuring-tls.html#node-certificates) can be configured |

