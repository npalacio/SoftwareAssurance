# Environment
One hypothetical environment where users would expect security functionality from ElasticSearch is in an enterprise environment, specifically a bank. A bank could use ElasticSearch to support an internal system that allows users to search for and view customers' account information. The system would need to support fuzzy/partial search functionality where users can put in part of a customer's name or some sort of internal customer ID used by the bank. The system would also need to be able to search across any number of other fields related to a customer that a user might have. The system would need to be able to search across millions of customer records in near real time. This is a perfect use case for ElasticSearch.

Given that there would be personally identifiable information (PII) for customers stored in the ElasticSearch indices the business would want a high level of confidence that this information is safe. The business would want to know that only authorized users of the system would have access to this sensitive customer data. Failure to secure the customer assets in this system could result in a data breach that would cost the company their reputation as a reliable and safe place for customers to give their confidential information to.

The business would also want a high level of confidence that the data stored in ElasticSearch is safe from corruption in the event of a hardware or software failure. Given this requirement the implementers of this system might decide to use multiple nodes inside one ElasticSearch cluster in order to provide redundancy and resiliance to hardware or software failures.

# Threats
- Unauthorized access to customer PII in ElasticSearch
    - A malicious (or non-malicious) user might gain access to customer PII stored in the index. The consequences of that unauthorized access would be severe because it can result in a loss of trust between the customer and the bank.
- A malicious user might attempt to destroy or make unusable the ElasticSearch data.
    - The consequences of this informational loss could be severe if there are no other backups of that data.
- A malicious user might try to modify or update an ElasticSearch index in order to cause unintended behavior in the system.
    - A malicious user with knowledge of the system might try to update or modify an ElasticSearch index in order to cause specific behaviour in the system.
- Unintentional data loss or corruption
    - Data could become corrupt or unusable during normal operation of ElasticSearch due to a severe hardware or software failure.

# Security Features
- User authentication
    - ElasticSearch provides functionality to authenticate users.
- User authorization
    - ElasticSearch provides role based access control in order to provide granular controls over what users have access to. This is done by assigning specific permissions to roles and then putting users into those roles.
- Encrypted communication
    - ElasticSearch provides functionality to encrypt communication between different nodes as well as the consuming system in order to prevent unauthorized access to potentially sensitive information stored in an ElasticSearch index.
- Audit Logging
    - ElasticSearch provides logging so system administrators can monitor things like failed authentication attempts, user access denied, etc.