# jenkins-world-2016

Some notes & questions/answers from Jenkins World 2016 in Santa Clara.

# Fundamentals of Jenkins Pipelines w/ Docker

https://github.com/kishorebhatia/pipeline-as-code-demo

can use failFast with parallel jobs

Questions:
* Can node wrap everything or be under stages?
  * It can be done either way... Depends on your project... you may have multiple stages under a single node


# Keynote

## Fabric8
RedHat led initiative for Jenkins Pipelines, Kubernetes, Docker

## Project Voltron
New plugin site to make it easier to find plugins
* Has categories
* Elasticsearch instead of using page find
* Better layout

## Blue Ocean
* they said part of the Blue Ocean initiative is going to be to help 
productivity for people who like to click around on the interface all 
the way to people who like to use vim/emacs 
* Eventually want to make storage pluggable
* UX for pipelines first and then continue to push productivity changes

## CloudBees
### 4 Quandrants of Maturity
Y-Axis: Team / Workgroup / Enterprise
X-Axis: Idea / Code / Build / Test / Stage / Deploy

### Private SaaS Edition
* Works with AWS or OpenStack currently
* Config via files / command-line tooling bees-pse
  * Cluster config / workers
  * UX looks like Blue Ocean... has visualization
  * Helps with density / efficiency... Have 100+ but only using 43 VMs
    * 151 masters / 700 executors
  * They have a 2000 master / 8000 executors cluster set up
  * ElasticSearch integrated 
  * Using containers (assuming Docker given the Docker-heavy topics here)
* Thousands of plugins.. discussion about how do you ensure stability / migrations
  * unequal plugin maturity
  * no extensive compatibility check
* CloudBees Jenkins Enterprise
  * selection of enterprise and mature plugins
  * LOL... Quote on slide "Two words: Excitingly BORING!"
* DevOps Express
  * Companies that sometimes overlap/compete in main architecture
    * DevOps Essential: Atlassian, GitHub, SauceLabs, Chef, SonarSource, BlazeMeter, 
JFrog, CloudBees, Puppet, Sonatype (no particular order)
    * Joint Go to Market / Cooperation
    * Best Practices / Ref Architecture
    * Integrated Support
    * Connectivity Assurance

# Secure Container Development Pipelines with Jenkins
* DataDog survey showed that, from those surveyed, seemed that Enterprises 
led pack in Docker adoption
* Mostly talked about SDLC and how they can help audit Docker containers for 
security concerns
* FlawCheck is an on-prem or hosted solution... is a registry?

# Continuously Deploying Containers with Jenkins Pipeline to Docker Swarm Cluster
* Docker compose is supposed go away???
* http://vfarcic.github.io/jenkins-swarm/index.html#/cover

# Continuous Delivery of Infrastructure w/Jenkins
* Docker container: docker run -ti rtyler/cd-preso

# JenkinsOps: An Initiative to Streamline & Automate Jenkins
* 10+ year codebases
* Multiple Scrum teams
* Major investments in testing going forward
* PHPUnit / JUnit tests
* Jenkins at the center

# No, You Shouldn't Do That! Lessons from Using Pipeline
* @Library for loading a lib
* Migrating to Pipelines: Make what you have work and then improve
* Rule #1: Don't put build logic in the pipeline
  * They wrote plugin to handle Windows: 
    https://github.com/jenkinsci/pipeline-utility-steps-plugin
* Test parallelization
  * https://github.com/jenkinsci/plugin-compat-tester (test compat of plugins w/ Jenkins version)
  * GroovyCPS won't run all DefaultGroovyMethods (e.g. each, collect, findAll)
  * Default whitelist in sandbox is *far* from complete.
    * Map.size() isn't in it
  * Sandbox also has other issues 
    * Anonymous properties in XmlSlurper no supported
  * @NonCPS turns off GroovyCPS engine
    * code will still be processed by sandbox
  * if you call build steps from @NonCPS function, the @NonCPS annotation is voided
  * Solution:
    * Don't try to be fancy (KISS)
    * Multiple jobs are current workaround for Matrix
* No good saying build failed at end of email
  * Flow isn't just doing one thing
  * use try catch/finally for why and when
* Track library version
  * Use changelog: true with your checkouts
  * echo the library version when loaded
* Report flaky tests so you can fix/remove them
* Use smaller functions in your library to make it quicker to test
* *Don't do inputs inside a node (you lock an executor)*
* Similar with timeouts outside of nodes when script is coming back from restart
* stage "concurrency" -> great in theory

# Speed up Your Continuous Delivery Pipeline with Jenkins
## Problems with CD/Microservices
* Problem #1: Tons of logs slows us down
* Problem #2: Ripple effect - It was working until 5 mins ago... someone changed something
* Problem #3: Dependencies
* Problem #4: Cycle time
  * Releasing small changes and every change often is good
  * But how do I get the whole released quickly?
## BigData to the Rescue
RabbitMQ, Flume, HDFS, Spark, Kibana listed... LogStash plugin for Jenkins 
an option but they preferred Flume because mature and could do more advanced
things?
* Collect review events
* Collect logs
* Channel to central store
* Never delete
* Process, inspect, learn 
## Analytics Dimensions
1. Project Commits
1. People Reviews
1. System Metrics
