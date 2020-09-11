# Security History

## Security Features
Elasticsearch contains a robust security feature set. [Configuring Security in Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/current/configuring-security.html) outlines the high-level process to turn on security features. Security features are completely disabled by default for trial and basic licenses; but, they can be turned on with the `xpack.security.enabled` setting. The software can be configured for [FIPS 140-2](https://www.elastic.co/guide/en/elasticsearch/reference/current/fips-140-compliance.html). When using Elasticsearch across distributed systems, inter-node communication can be secured with [TLS](https://www.elastic.co/guide/en/elasticsearch/reference/current/configuring-tls.html). The TLS feature includes all of expected options (specifying certificates, trust stores, supported protocols, and cipher suites). In addition to  built-in user accounts, the software supports user authentication via Active Directory, File, Kerberos, LDAP, Native, PKI, and SAML "realms". Permissions can be assigned to users or bundled in roles providing granular control over indicies, [documents, and fields](https://www.elastic.co/guide/en/elasticsearch/reference/7.3/field-and-document-access-control.html). Lastly, the *Configuring Security in Elasticsearch* guide notes that [Audit Logging](https://www.elastic.co/guide/en/elasticsearch/reference/current/auditing-settings.html) can be enabled. Furthermore, the [Security Settings page](https://www.elastic.co/guide/en/elasticsearch/reference/7.3/security-settings.html) provides full documentation on every security setting. This page illustrates settings in addition to those covered in the configuration guide to configure [anonymous access](https://www.elastic.co/guide/en/elasticsearch/reference/7.3/security-settings.html#anonymous-access-settings), [IP filtering](https://www.elastic.co/guide/en/elasticsearch/reference/7.3/security-settings.html#ip-filtering-settings), and [password hashing](https://www.elastic.co/guide/en/elasticsearch/reference/7.3/security-settings.html#hashing-settings).

## Known Vulnerabilities
The [Security Issues page](https://www.elastic.co/community/security) tracks known vulnerabilities and notes that new discoveries should be sent to security@elastic.co. The page tracks issues for all of "[Elastic's](https://www.elastic.co/) open-source and commercial products". The Elasticsearch section is well-maintained: The most recent disclosure [CVE-2020-7019](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-7019) occurred on August 18, 2020 less than a month before this writing. Two other issues - [CVE-2020-7014](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-7014) and [CVE-2020-7009](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-7009) were noted for Elasticsearch in 2020. Each disclosure provides a vulnerability summary and notes for remediation. For example, the table entry for the latest vulnerability contains the following:

ESA ID | CVE | Date Disclosed | Vulnerability Summary | Remediation Summary
------ | --- | -------------- | --------------------- | -------------------
ESA-2020-12 | [CVE-2020-7019](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-7019) | 2020-08-18 | A field disclosure flaw was found in Elasticsearch when running a scrolling search with Field Level Security. If a user runs the same query another more privileged user recently ran, the scrolling search can leak fields that should be hidden. This could result in an attacker gaining additional permissions against a restricted index. | Users should upgrade to Elasticsearch version 7.9.0 or 6.8.12.

## Security Engineering
The Elasticsearch Github project contains nine security related labels:

Label | Description | Open Issues/PRs
----- | ----------- | ---------------
`:Security/Security`  | *Security issues without another label* | 92
`:Security/Client`  | *Security in clients (Transport, Rest)* | 2
`Team:Security` | *Meta label for security team* | 378
`:Security/Network` | *SSL/TLS, Security for NIO/Netty* | 39
`:Security/Audit` | *X-Pack Audit logging* | 30
`:Security/Authentication` | *Logging in, Usernames/passwords, Realms (Native/LDAP/AD/SAML/PKI/etc)* | 116
`:Security/Authorization` | *Roles, Privileges, DLS/FLS, RBAC/ABAC* | 87
`:Security/License` | *License functionality for commercial features* | 20
`:Security/IdentityProvider` | *dentity Provider (SSO) project in X-Pack* | 0

Taking open issues that contain multiple security labels into account, there are 384 open security-related issues and pull requests at the time of this publication. This represents **13%** of the 2996 open issues.

Previous releases' notes highlight regular security activity (the current version is 7.9):
Release | Security
------- | --------
7.8.0 | [Cert Utils Fix](https://www.elastic.co/guide/en/elasticsearch/reference/7.8/release-notes-7.8.0.html)
7.7.1 | [Authorization Enhancements & Authentication Fixes](https://www.elastic.co/guide/en/elasticsearch/reference/7.8/release-notes-7.7.1.html)
7.7.0 | [New API Creation Feature, Several Auth Enchancements, Security Error Fix](https://www.elastic.co/guide/en/elasticsearch/reference/7.8/release-notes-7.7.0.html#feature-7.7.0)
7.6.2 | [API Key Update](https://www.elastic.co/guide/en/elasticsearch/reference/7.8/release-notes-7.6.2.html)
7.6.1 | [Update oauth2-oidc-sdk to 7.0](https://www.elastic.co/guide/en/elasticsearch/reference/7.8/release-notes-7.6.1.html)
7.6.0 | [Keystore Password Protection](https://www.elastic.co/guide/en/elasticsearch/reference/7.7/release-highlights.html#_password_protection_for_the_keystore), [Roles in Custom Realms, Auth Debug Logging, Several Bug Fixes](https://www.elastic.co/guide/en/elasticsearch/reference/7.8/release-notes-7.6.0.html)
7.4 | [TLS Email Notification Settings](https://www.elastic.co/guide/en/elasticsearch/reference/7.4/release-highlights-7.4.0.html#_tls_settings_for_email_notifications)
