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
