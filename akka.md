## Actors and Messages
### Actor
* does the processing in the system
* Building blocks
* Performs small well defined tasks
Big pieces of work broken down into smaller pieces
actor code is the smae if if it local or distributed
* Every actor has an address
* communicates via messages
* Actors instances are lazy
* Process 1 message at a time
* Considerate and efficient
* Functionally cohesive
* Don't expose mutable states
* Use them in heirarchies


### ActorRef
* Actors reference each other
* Actors reference them ```self```
* Children Ref
* Parent Ref
#### Create new actor
```
ActorOf()
```

#### Look up existing
```
ActorSelection()
```


#### Can for 4 things
* Receives & react to messages
* Change behaviour for next message
* Create more actors
* Send messages to other actors

#### Parts
* **Incoming Message Mailbox** - Messages sent to the actor get queued here for processing
* **Behaviour** - React to an incoming message and perform defined action
* **State** - The current state of the actor (user ID, request, counter, etc)
* **0,1,m Children** - The other child actors that are being supervised
* **Supervisory Strategy** = Handle faults in children



### Messages
* Simple POCO class
* Actors can change state when responding to messages
* Message instances should be immutable
* Async
* Immutable
* Don't pass mutable state between actors
* 

### Actor System
* Can have multiple Actor System Instances
* **Local** - send message in local system instance
* **Remote** - send to a different system instance

### Types of Message Sending 
#### Tell
* Send a message - no response
* Fire and Forget
* No Blocking
* Better scaling and concurrency

#### Ask
* Send a message - get response
* Only use when absolutely necessary

#### Forward
* Route messages through the system

### Props
* Recipe of how to make an instance of a particular type of actor
* Use ```factory``` methods of akka.net to create props and actors
* Do not just use them up



### Actor Instance Lifecycle
* **Starting** - ```PreStart()```
* **Receive Messages**
* **Stopping** - ```PostStop()```
* **Termnated** - ```PostStop()```
* **Restarting** - ```PreRestart()``` and ```PostStop()```

### PreStart()
* Called before actor instance receives first message

### PostStop()
* Called after the actor has been stoped and is not receiving message
es

### PreRestart()
* Called before actor begins restarting

### Terminating Actor
* PoisonPill.Instance
* actorRef.GracefulStop()

### Behaviour Switching
* **Become()**
* **BecomeStacked()**
* **UnbecomeStacked()** - pops current behaviour off the stack
* Only done on next message that comes in


