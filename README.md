RopeTeam
========

distributed events and coordination for jgroups clusters in spring applications.

Features
--------
  * Cluster member task limiting
  * Clustered Event Bus
  
Task Limiting
-------------

Limit a task or method call to one cluster member. This is useful for scheduled tasks that you 
only want running on one member. Flock allows dynamic assignment of which cluster member runs a task. 
A task will stay assigned to the same member unless it dies, in which case it will failover to another host.  

**Example**

    @OncePerCluster
    @Scheduled(fixedDelay=1000)
    public void someScheduledTask() {
      // method will only actually be called on one host per cluster
      // the same host will always run the task unless it dies 
    }

Clustered Event Bus
-------------------

Publish events to all members of the cluster. Events can be any object that can be marshalled to JSON with Jackson.

**Publishing Events**
    
    @Autowired
    EventPublisher publisher;
    
    public void generateEvent() {
       publisher.publish(new HelloEvent("greetings"));
    }
    
**Subscribing to Events**

    @EventSubscriber
    public void handleHelloEvent(HelloEvent e) {
       System.out.println(e.getGreeting())
    }

*This project is a fork/rename of jeffskj/flock*


** Native Clustering **

  * plugin based on server side (servlet, undertow handler?)
  * async nio2 http client (netty? undertow?)
  * pure java config
  * simple cluster view, list of peers
  * pluggable discovery (sherpa? generic rest service? etcd? aws api?)
  * lease based cluster membership
  * thread pool parallel message sending
  * pure http messages
  * JSON payloads for meta-messages
  * don't try to solve reliability or fancy cluster balancing
  * completely peer-to-peer
  * separate internal messages vs client messages to different endpoints