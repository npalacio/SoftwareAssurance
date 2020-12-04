# Code Review
## Strategy
Our code review strategy was to follow a scenario based and risk based hybrid approach. Some challenges we were up against included the fact that the Elasticsearch codebase is quite large so trying to analyze everything is not realistic. Another challenge was that most of our team does not have a lot of experience working in large Java projects/repositories. In order to address these challenges we decided to focus our efforts on very specific areas of the Elasticsearch codebase. Specifically, we decided to focus on the **node discovery** and **authentication** parts of Elasticsearch. These areas were deemed high risk based on our findings from scenarios in previous assignments.

## Phase 1 - Automated Scanning
We originally intended to use [PMD](https://pmd.github.io) to scan the codebase, but we were not able to find quality rulesets. Instead we used [SpotBugs](https://spotbugs.github.io/) with the [Find Security Bugs plugin](https://find-sec-bugs.github.io). This plugin advertised both OWASP TOP 10 and CWE coverage, and the [final report](SpotBugsReport.html) contains useful links to these resources for each finding.
 
## Phase 2 - Manual Reviews
Informed by the results of our automated scans and our currated lists of applicable CWE's, we manually evaluated two critical areas in Elasticsearch:

### Node Discovery

### Authentication

# Key Findings and Contributions
## Key Findings
The table below outlines those CWE's of particular concern and the results of further investigation.
| CWE | Areas Evaluated | Description | Risk Level |
|---|-----|---|---|
| [CWE-259 Use of Hard-coded Password](https://cwe.mitre.org/data/definitions/259.html) | Found by SpotBugs in [org.elasticsearch.common.settings.KeyStoreWrapper.java:line 431](https://github.com/elastic/elasticsearch/blob/e7d06843f9cce7ea3f3e22606cf1dc196658f801/server/src/main/java/org/elasticsearch/common/settings/KeyStoreWrapper.java#L431) | Hard-coded passwords are a great concern to our application if they unlock critical resources and cannot be overridden by configuration. In this case, the hard-coded password (empty string) was just used for legacy key stores that did not allow passwords. | **NONE** |
| [CWE-78](https://cwe.mitre.org/data/definitions/78.html)/[OWASP Top 10 - Injection](https://owasp.org/www-project-top-ten/2017/A1_2017-Injection) | Found by SpotBugs in several places in Elasticsearch's startup code: [org.elasticsearch.bootstrap.Spawner.java](https://github.com/elastic/elasticsearch/blob/842e94a4e0a97f6678dab23c1d1aa590dd997360/server/src/main/java/org/elasticsearch/bootstrap/Spawner.java). | The code in that class is used to execute native commands to start up Elasticsearch's modules (by searching for Jar files in a particular directory). External user input is not used in these spots, so command injection is not a risk. However, if a physical machine is compromised, there is a slight risk that a malicious actor could place their own Jar file in the modules directory and Elasticsearch would run it with its own priviledges. | **LOW** |