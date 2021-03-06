# Code Review
## Strategy
Our code review strategy was to follow a scenario based and risk based hybrid approach. Some challenges we were up against included the fact that the Elasticsearch codebase is quite large so trying to analyze everything is not realistic. Another challenge was that most of our team does not have a lot of experience working in large Java projects/repositories. In order to address these challenges we decided to focus our efforts on very specific areas of the Elasticsearch codebase. Specifically, we decided to focus on the **node discovery** and **authentication** parts of Elasticsearch. These areas were deemed high risk based on our findings from scenarios in previous assignments.

## Phase 1 - Automated Scanning
We originally intended to use [PMD](https://pmd.github.io) to scan the codebase, but we were not able to find quality rulesets. Instead we used [SpotBugs](https://spotbugs.github.io/) with the [Find Security Bugs plugin](https://find-sec-bugs.github.io). This plugin advertised both OWASP TOP 10 and CWE coverage, and the [final report](SpotBugsReport.pdf) contains useful links to these resources for each finding.
 
## Phase 2 - Manual Reviews
Informed by the results of our automated scans and our currated lists of applicable CWE's, we manually evaluated two critical areas in Elasticsearch:

### Node Discovery
In order to review the parts of elasticsearch that handle discovering other nodes in a cluster we selected the following 5 CWEs to search this part of the code for:
- [CWE-117](https://cwe.mitre.org/data/definitions/117.html): **Improper output neutralization for Logs**
   - This CWE was selected for manual review because it showed up in our automated scan as a potential security weakness.
   - After manual inspection we found that in the TransportService.java on line 882 there is a method that validates a given string representing an action name. If this string is determined to be invalid a message is logged that includes the invalid action name. This could be a vulnerability because an attacker could be able to get any invalid action name logged which could compromise the log file.
- [CWE-306](https://cwe.mitre.org/data/definitions/306.html): **Missing Authentication for Critical Function**
   - This CWE was selected for manual review because our data flow diagram revealed that there might be a security weakness with the way Elasticsearch finds new nodes for a cluster.
   - After manual inspection we found that in PeerFinder.java on line 311 there is a method that probes an IP address to find other nodes in the cluster without any validation on that IP address creating a vulnerability that an attacker might be able to exploit in order to add a rogue node.
- [CWE-522](https://cwe.mitre.org/data/definitions/522.html): **Insufficiently Protected Credentials**
   - This CWE was selected for manual review because our data flow diagram revealed that there might be a security weakness with the way Elasticsearch finds new nodes for a cluster and insufficiently protecting credentials for another node is one of the ways this weakness might manifest itself.
   - After manual inspection we did not find any reason to believe this weakness existed in the Elasticsearch codebase.
- [CWE-366](https://cwe.mitre.org/data/definitions/366.html): **Race condition within a thread**
   - This CWE was selected for manual review because Elasticsearch runs with multiple threads going at once which means a race condition could occur if the threads are not managed properly.
   - After manual review we did not find any instances of a potential race condition occuring in the node discovery code. In fact, we found many good uses of a mutex to put locks on shared resources in order to avoid race conditions.
- [CWE-567](https://cwe.mitre.org/data/definitions/567.html): **Unsynchronized Access to Shared Data in a Multithreaded Context**
   - This CWE was selected for manual review because Elasticsearch runs with multiple threads going at once which means improper access of shared data could occur if the threads are not managed properly.
   - After manual review we did not find any instances of unsynchronized access to shared data. In fact, we found many good uses of a mutex to put locks on shared resources in order to prevent unsychronized access to them.

### Authentication
In this section in order to validate the effectiveness of the authentication functions of elasticsearch, we found 5 CWE's and looked for issues in the codebase:
- [CWE-287](https://cwe.mitre.org/data/definitions/287.html): **Improper Authentication**
   - We selected this CWE as it is one of the main potential issues with authentication in a software system like elasticsearch.
   - After manual review, it seems the basic authentication feature is secure at creating and authorizing unique user credentials within nodes.
- [CWE-290](https://cwe.mitre.org/data/definitions/290.html): **Authentication Bypass by Spoofing**
   - Spoofing is another common misuse of software systems, so it felt important to include this CWE in this section.
   - After manual review, we found that it is possible to enable IP filtering but it only allows for blacklisting certain addresses, so spoofing could be an issue in elasticsearch. There is also the potential vulnerabilities of spoofing what device you are accessing with.
- [CWE-294](https://cwe.mitre.org/data/definitions/294.html): **Authentication Bypass by Capture-replay**
   - Capturing information is a danger of an information storage software, so finding vulnerabilities here is important.
   - After manual review, elasticsearch does not allow for data sniffing except under specific circumstances, enabling on specific nodes. This appears to not be an issue.
- [CWE-307](https://cwe.mitre.org/data/definitions/307.html): **Improper Restriction of Excessive Authentication Attempts**
   - Brute force authentication is also another common misuse practice and so it made this list of potential vulnerabilities.
   - After manual review, it seems like elasticsearch allows for multiple authentication attempts, but elasticsearch has audit logging features that can be enabled to track failed attempts. This would be a slight vulnerability.
- [CWE-603](https://cwe.mitre.org/data/definitions/603.html): **Use of Client-Side Authentication**
   - Being able to avoid authentication by modifying the client is a potential vulnerability and was worth adding to our list of CWE's.
   - After manual review, elasticsearch allows you to disable client-side authentication, meaning this is not an issue.

# Key Findings and Contributions
## Key Findings
The table below outlines those CWE's of particular concern and the results of further investigation.
| CWE | Areas Evaluated | Description | Risk Level |
|---|-----|---|---|
| [CWE-259 Use of Hard-coded Password](https://cwe.mitre.org/data/definitions/259.html) | Found by SpotBugs in [org.elasticsearch.common.settings.KeyStoreWrapper.java:line 424](https://github.com/elastic/elasticsearch/blob/v7.10.0/server/src/main/java/org/elasticsearch/common/settings/KeyStoreWrapper.java#L424) | Hard-coded passwords are a great concern to our application if they unlock critical resources and cannot be overridden by configuration. In this case, the hard-coded password (empty string) was just used for legacy key stores that did not allow passwords. | **NONE** |
| [CWE-78](https://cwe.mitre.org/data/definitions/78.html)/[OWASP Top 10 - Injection](https://owasp.org/www-project-top-ten/2017/A1_2017-Injection) | Found by SpotBugs in several places in Elasticsearch's startup code: [org.elasticsearch.bootstrap.Spawner.java](https://github.com/elastic/elasticsearch/blob/v7.10.0/server/src/main/java/org/elasticsearch/bootstrap/Spawner.java). | The code in that class is used to execute native commands to start up Elasticsearch's modules (by searching for Jar files in a particular directory). External user input is not used in these spots, so command injection is not a risk. However, if a physical machine is compromised, there is a slight risk that a malicious actor could place their own Jar file in the modules directory and Elasticsearch would run it with its own priviledges. | **LOW** |
| [CWE-117 Improper output neutralization for Logs](https://cwe.mitre.org/data/definitions/117.html) | Found by manual inspection in [org.elasticsearch.transport.TransportService.java:line 882](https://github.com/elastic/elasticsearch/blob/v7.10.0/server/src/main/java/org/elasticsearch/transport/TransportService.java#L882)| There is a method that validates a given string representing an action name. If this string is determined to be invalid a message is logged that includes the invalid action name. This could be a vulnerability because an attacker could be able to get any invalid action name logged which could compromise the log file.| **MEDIUM** |
| [CWE-306 Missing Authentication for Critical Function](https://cwe.mitre.org/data/definitions/306.html) | Found by manual inspection in [org.elasticsearch.discovery.PeerFinder.java:line 311](https://github.com/elastic/elasticsearch/blob/v7.10.0/server/src/main/java/org/elasticsearch/discovery/PeerFinder.java#L311)| There is a method that probes an IP address to find other nodes in the cluster without any validation on that IP address creating a vulnerability that an attacker might be able to exploit in order to add a rogue node.| **HIGH** |

## Contributions
Our team does not currently plan on submitting any contributions to the Elasticsearch project. We decided that we would need to spend more time with the Elasticsearch codebase in order to truly validate some of our security findings before we would feel comfortable submitting anything to the project. Elasticsearch has a massive codebase and our strategy of focusing on specific parts of it means that we lack a lot of context around the codebase as a whole.

## Team Reflection
- **What went well**
   - Everyone on the team contributing
   - Meeting the professor gave us some good advice on starting with CWEs to report on for our manual code review
   - Joseph on our team getting the automated scan setup also helped guide our manual code review to potential issues
   - Working in 2 pairs for the manual code review part of the assignment helped to focus on quality and depth of review rather than trying to cover more ground in the codebase which would not have been realistic
- **What could have gone better**
   - Team (mostly) lacking experience in Java. Only 1 member of our team has experience working in a large Java codebase which made our manual code review difficult.
   - We struggled early on in the assignment understanding what exactly we should do for the code review. The meeting with the professor helped provide direction.
- **What we plan to change**
   - N/A, continue to work as a team

## [Project Board](https://github.com/npalacio/SoftwareAssurance/projects/5)