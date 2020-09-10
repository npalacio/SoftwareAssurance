# Environment
One environment where users would expect security functionality from ElasticSearch is in an enterprise environment. If a company were using ElasticSearch to store company assets they would want some level of confidence that those assets are safe. The level of confidence they would require would be directly proportional to the value of the company asset as well as the consequences of losing that asset. A company would want to be able to control what users have access to that asset as well as what each user is able to do with that asset (create, read, update, delete).

A company might want to store an asset in ElasticSearch that gives them an edge over their competitors in their given market. This asset could be a large dataset that they need to be able to query very quickly and efficiently in order to support a consuming system. The company would want a high level of confidence that this asset is safe from malicious attackers who might want to gain access to this asset in order to sell it on the black market for profit. The company would want to be able to configure controls over who has access to read or update this asset to prevent this.

# Threats
- Unauthorized access to company assets in ElasticSearch index
    - A malicious (or non-malicious) user might gain access to sensitive company assets stored in the index. The consequences of that unauthorized access might be severe depending on the data stored in the index.
- A malicious user might attempt to destroy or make unusable an ElasticSearch index.
    - The consequences of asset loss from the index might be severe depending on the data stored in the index.
- A malicious user might try to modify or update an ElasticSearch index in order to cause unintended behavior in a consuming system.
    - A malicious user with knowledge of consuming systems might try to update or modify an ElasticSearch index in order to cause specific behaviour in the consuming system.
- Unintentional data loss or corruption
    - Data could become corrupt or unusable during normal operation of ElasticSearch.

# Security Features
- User authentication
    - ElasticSearch provides functionality to authenticate users.
- User authorization
    - ElasticSearch provides role based access control in order to provide granular controls over what users have access to. This is done by assigning specific permissions to roles and then putting users into those roles.
- Encrypted communication
    - ElasticSearch provides functionality to encrypt communication between different nodes as well as the consuming system in order to prevent unauthorized access to potentially sensitive information stored in an ElasticSearch index.
- Audit Logging
    - ElasticSearch provides logging so system administrators can monitor things like failed authentication attempts, user access denied, etc.