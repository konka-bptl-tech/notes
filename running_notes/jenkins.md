Dasher technolgies is a software provider which possibility of transitioning  leverages

Jenkins is a opensource automation server that accerate software development it orchestrates entire software delivary pipeline from building testing and deployment CI/CD pipelines quickly identitfies code issues enabling faster development and higher quality software 

developers regulalry push their code to repo webhook triggers jenkins to executes CI/CD pipelines it will executes sequence of steps if anything goes wrong developer quickly notified developers works on the error again push to code to central repo again pipeline started

This loop keeps a code fresh minamal manaul intervention the process is keeps on repeating itself ensuring smooth and automated development workflow

- Job
- builds
- freestyle job
- pipelines
- stages
- nodes[controlplane+build-agents][controlplane responsible to coordniating builds,distribute the tasks to build agents]
- plugins

pros:
- opensource
- highly cistomizable software
- using groovy scripted pipelies write advanced[complex-workflows] pipelines
- using pipelines as a code

cons:
- maintenance overhead
- performace issues
- security take

DevOps:

DevSecOps:

Need Of CI/CD:

Jenkins levelrages distributed architecuture to efficiently manages CI/CD pipelines efficiently this architecutre offers scalability allow you automate complex workflows across multiple machines

control plane = Responsible for Coordninating entire CI/CD pipeline
- `Management Tasks` plugins installation,user auth&autho, integration with others, secrets management
- `Job Management` define,scheduling and monitoring jobs

Build Agents = Responsible to excute pipelines

Types of Projects:
1. Freestyle
   - Using Web interface configure the Jobs
   - Limited workflows
   - Non code based configuration
   - Limited Functionality
   - Cannot resume after failure
2. Pipeline
   - Writes pipelines as code either declarative syntax or groovy[scripted] syntax
   - versioning pipelines and improve the collaboration

3. Multi Branch pipeline
4. Maven Project
5. Multi Branch Configuration
   - 
6. Organizations Folders
   - Organize Pipelines in highrarchy structure

Plugins:
- copy artifact plugin
- yet another build visualizer
- Blue Ocean
- RBAC
- Mock Security realm
- matrix authorization strategy
Jenkins finger print

if not variable substitution use single quote if you have variable sunstituion use double quotes

Automating Jenkins using CLI and API

Jenkins CSRF CRUMB 

pipeline script from scm

Why do we need parametrize builds
In Jenkins a parametized builds are used to create flexible and customizable builds jobs by defining parameters that can be passed while triggering a build this parameters can be used to make your builds more dynamic 

BRANCH - String Parameter
APP_PORT - String Parameter
SLEEP_TIME - Choice parameter

Branch Specifier
*/${BRANCH_NAME}
