#Use Case #4
![Use Case 4](./Images/UseCase4v2.png)

**Use Case:** Bank Employee updating customer's account info.

**Description:** The bank employee must be able to change information on a customer's account, this info could be personal verifying information of the customer like a name or address, and this also includes financial transactions such as depositing or withdrawing money.

**Misuse Case:** The bad actor in this misuse case is a new hire with malign plans. The new hire could attempt to set the account owner to themself, allowing them access to the money in the account. 

This can be detected through Audit Logging, or prevented by requiring higher level employee access to change the account. The bad actor then decides to wait for this higher level access privilege, but can again be detected/mitigated through audit logging actions of insiders to reduce insider threats.

**Security Requirements:**
[Security Overview](https://www.elastic.co/guide/en/elasticsearch/reference/current/elasticsearch-security.html) gives detailed summaries of security methods included with elasticsearch including unauthorized access and audit logging.

[Restricting Access](https://www.elastic.co/guide/en/elasticsearch/reference/current/authorization.html): Elasticsearch allows for authorizing users, setting roles, and setting privileges and permissions to groups, users, and roles. This allows for preventing users from accessing information a bad actor might misuse.

[Audit Logging](https://www.elastic.co/guide/en/elasticsearch/reference/current/enable-audit-logging.html): Elasticstack (which elasticsearch is a part of) allows for enabling audit logging, which can display failed authentification attempts, failed connections, and provides evidence to help mitigate bad actors' misuse.

**Assessment:** Elasticsearch provides both features that would detect, mitigate, and prevent insider bad actors from having access to tools that would harm customer accounts.