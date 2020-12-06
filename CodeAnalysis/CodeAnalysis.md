# Code Review
## Strategy
Our code review strategy was to follow a scenario based and risk based hybrid approach. Some challenges we were up against included the fact that the Elasticsearch codebase is quite large so trying to analyze everything is not realistic. Another challenge was that most of our team does not have a lot of experience working in large Java projects/repositories. In order to address these challenges we decided to focus our efforts on very specific areas of the Elasticsearch codebase. Specifically, we decided to focus on the **node discovery** and **authentication** parts of Elasticsearch. These areas were deemed high risk based on our findings from scenarios in previous assignments.

## Phase 1 - Automated Scanning
We originally intended to use [PMD](https://pmd.github.io) to scan the codebase, but we were not able to find quality rulesets. Instead we used [SpotBugs](https://spotbugs.github.io/) with the [Find Security Bugs plugin](https://find-sec-bugs.github.io). This plugin advertised both OWASP TOP 10 and CWE coverage, and the [final report](SpotBugsReport.html) contains useful links to these resources for each finding.
 
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

# Key Findings and Contributions
## Key Findings
The table below outlines those CWE's of particular concern and the results of further investigation.
| CWE | Areas Evaluated | Description | Risk Level |
|---|-----|---|---|
| [CWE-259 Use of Hard-coded Password](https://cwe.mitre.org/data/definitions/259.html) | Found by SpotBugs in [org.elasticsearch.common.settings.KeyStoreWrapper.java:line 431](https://github.com/elastic/elasticsearch/blob/e7d06843f9cce7ea3f3e22606cf1dc196658f801/server/src/main/java/org/elasticsearch/common/settings/KeyStoreWrapper.java#L431) | Hard-coded passwords are a great concern to our application if they unlock critical resources and cannot be overridden by configuration. In this case, the hard-coded password (empty string) was just used for legacy key stores that did not allow passwords. | **NONE** |
| [CWE-78](https://cwe.mitre.org/data/definitions/78.html)/[OWASP Top 10 - Injection](https://owasp.org/www-project-top-ten/2017/A1_2017-Injection) | Found by SpotBugs in several places in Elasticsearch's startup code: [org.elasticsearch.bootstrap.Spawner.java](https://github.com/elastic/elasticsearch/blob/842e94a4e0a97f6678dab23c1d1aa590dd997360/server/src/main/java/org/elasticsearch/bootstrap/Spawner.java). | The code in that class is used to execute native commands to start up Elasticsearch's modules (by searching for Jar files in a particular directory). External user input is not used in these spots, so command injection is not a risk. However, if a physical machine is compromised, there is a slight risk that a malicious actor could place their own Jar file in the modules directory and Elasticsearch would run it with its own priviledges. | **LOW** |
|[CWE-117 Improper output neutralization for Logs](https://cwe.mitre.org/data/definitions/117.html) | Found by manual inspection in [org.elasticsearch.transport.TransportService.java:line 882](https://github.com/elastic/elasticsearch/blob/master/server/src/main/java/org/elasticsearch/transport/TransportService.java#L882)| There is a method that validates a given string representing an action name. If this string is determined to be invalid a message is logged that includes the invalid action name. This could be a vulnerability because an attacker could be able to get any invalid action name logged which could compromise the log file.| **MEDIUM** |
|[CWE-306 Missing Authentication for Critical Function](https://cwe.mitre.org/data/definitions/306.html) | Found by manual inspection in [org.elasticsearch.discovery.PeerFinder.java:line 311](https://github.com/elastic/elasticsearch/blob/8b39992bf879e702ac4be854822fae47dc44ebb6/server/src/main/java/org/elasticsearch/discovery/PeerFinder.java#L311)| There is a method that probes an IP address to find other nodes in the cluster without any validation on that IP address creating a vulnerability that an attacker might be able to exploit in order to add a rogue node.| **HIGH** |