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

# External Workspace Manager Plugin for Jenkins Pipeline
Interesting... Check this out...

# Ownership Plugin
Interesting way to manage roles

# Pipelines for Building and Deploying Android Apps
Interesting Pipeline scripts he presented

# Continuous Delivery Pipeline - Patterns and Anti-Patterns
* Discuss Domain Driven Design (DDD) for CD

# How to Do Continuous Delivery with Jenkins Pipeline, Docker, and Kubernetes
* talking about lots of teams probably wanting different 
master, going at different speeds
* fabric8 allows teams to use what they want and deploy stack easily

## fabric8 features
Stands on Jenkins, Docker, Kubernetes
* Create: microservice wizards
* Build: package immutable containers
* Release: rolling upgrades across environments (he pleaded to use Kubernetes)
* Runtime: service discovery, elastic scaling, failover, load balancing
* Manage: centralize logs, metrics, alerts, tracing, circuit breakers
* Feedback: dashboards & metrics to get feedback!
* Platform: on premise, public or hybrid cloud

## Deep dive into fabric8
* Prometheus is grabbing metrics from containers in cluster 
* Basically aggregators of tooling and makes it easy for a team to roll
necessary services out
  * e.g. Gogs, Kubernetes, Nexus, Jenkins, etc...
* They have library of Pipeline templates for technologies
  * various languages
  * can customize environment names 
    * Kubernetes environments using namespaces
  * shows code and has plugins for visualizing things like Camel
* generates mvn sites, javadoc
* generates dashboards

## Demo
Grab from https://github.com/fabric8io/gofabric8/releases
  * gofabric8 start

## Get started
They listed a bunch... site references them all
* Amazon, Azure, DigitalOcean
  * https://stackpoint.io

## Maven plugin
https://maven.fabric8.io
* mvn fabric8:cluster-start
* mvn fabric8:run

## Architecture
* Jenkins for Pipelines
* Tools are all packaged in Docker
* Kubernetes for orchestrating containers
  * keep containers running across number of machines
  * deal with software/hardware/network failures
...

## Lessons Learned
* Docker instead of Jenkins Tools Install
  * mount secrets into containers using kubernetes Secrets

# So, you want to build the world's biggest Jenkins Cluster?
* 4GB of heap reasonably decent based on their data
* 200 executors should be ok (though Jenkins will handle 2k+)

## Context switching: bad
* Devs have to do this twice for broken build (notification and return to task)
* Time in Queue important... 
  * Managing the Design Factory book talks about this
* Save developer cost by keeping builds fast (low time in queue/turnaround)

## How big should my Jenkins be?
Based on Little's Law we should probably target 30-50% utilization... 
can derive # executors

* At least 1 master per 200 devs

## Scaling approaches
* Jenkins jobs are basically serverless microservices

## How do we manage 
* Great example: Netflix's Simian Army
* So how do we do that? Chaos Butler plugin!!!

## Measuring memory usage in Java
https://github.com/jbellis/jamm
* JAMM walks object graph to measure memory
* Stephen wrote a plugin to do this per job and will be releasing it

## Load testing Jenkins
Used Mock load plugin for testing

# The Need For Speed: Building Pipelines to be Faster
* Background
* Tooling
* Best Practices

# What does fast really mean?
* Throughput: solved by scale-out or separation of concerns
  * Distributed builds: many build agents per master
  * Multiple masters (one per team)
* Resource Use:
  * AWS c4.large: 4 vCPU, 7.5 GB RAM
    * ~$153/month on-demand ($95 reserved)
    * Each may support dozens of engineers
  * Software Engineer: 
    * ~$3000-17000/mo
* Latency: the King! amount of time waiting for build/deploy/etc...
  * Low turn around time: ship faster
  * Low turn around: less context switching for engineers
  * Context switching = lost time & mistakes
  * Staff time > resource time

## Components of Latency for Pipelines
My note: These seem like nice metrics to track...
1. Triggering Delay: time from commit until a build is queued
   * Use web hooks from SCM
1. On-master overheads: orchestration and tracking
   * no executors on master
   * clean up builds
   * don't write GB to logs
1. Queueing time: waiting for an executor slot
   * Additional build agents
   * Dynamic agents are easy (cloud agents, Docker agents, etc)
1. Executor time: how long it takes to build, test, deploy
1. Feedback delay: time until someone that cares sees the key result (pass/fail)
   * Limit the spam! Only culprits
   * Make it meaningful
   * Use better systems: IM not email CHATOPS!!!

## Shell step
* There's a impact to the build by calling multiple shell steps
* Running echo was drastically faster than sh "echo blah"

## Block-level stages
* Jenkins will show time spent everywhere
* Blue Ocean has per-step timing

## How Long Does Pipeline Take? (Roughly)
* Echo Step - 0.75 ms
  * Storage, etc...
* 'node' step: Obtain a workspace - 20 ms
* Run a shell step - 250ms
  * Will survive a restart of Jenkins master, hence overhead
* Quick build (1 min) - 60,000ms (1 min)

## Best Practices
### DON'T DO THESE
* Input step in a node usage (locks up an executor)
* waitUntil surrounding something that will never 
return true wrapping a node usage
  * also bad because each block is going to create 2 
    nodes (start/end) for tracking so looping on this would 
    be bad
### Better Practice: Bounded Loops
* retry inside try/catch with good info

### Better Practice: Timeouts
* Subtler version of retry case
* Are you deploying? Are you doing network calls?
  * You need a timeout somewhere
* Lets you safely recover from hangups

### Big Time Saver: Effective Use of Parallel
NOTE: Make sure you don't chew up too many executor slots
* Tests that might fail faster up front before fanning out

### Biggest Time Saver: Effective Notifications
His slide had the Closure and good code
* Wrap branches of your pipeline in closure that will notify right away

### Optimization: Consolidate!
* Node blocks: Giving up a workspace lease means someone else might grab it!
* Shell/batch steps
  * Remember that ~0.25s overhead for each shell step???
* Complex processing logic (XML parsing)
  * CPS has some significant overheads for tracking everything
  * @NonCPS works well for lowering things that don't need to be tracked
    and don't need to be serialized... anything that takes more than a second

## Conclusions
* Focus on latency
* Tools
  * Stage View -> Blue Ocean for top level
  * Pipeline will track and show timings!!
* Use echo statements!!!
