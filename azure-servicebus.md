
# Azure Service Bus Notes

## ServerBusyException: The request was terminated because the entity is being throttled

```
Microsoft.Azure.WebJobs.Host.FunctionInvocationException: Microsoft.Azure.WebJobs.Host.FunctionInvocationException: Exception while executing function: ExperianScoreUpdateJob.Execute ---> System.Transactions.TransactionInDoubtException: The transaction is in doubt. ---> Microsoft.ServiceBus.Messaging.ServerBusyException: The request was terminated because the entity is being throttled. Error code : 50002. Please wait 10 seconds and try again.
``` 


### Why?
* This issue can happen on standard Azure Service Bus because it is multi-tenanted. This means that your instance is running on the same backend/namespace as other tenants.
* Azure currently implements **resource based throttling**. If one customer has a big uptake then it can cause your connection to be throttled.
* This problem is referred to as the **Noisy Neighbor**
* This message is not out of the ordinary.
* Azure are currently working on other techniques to avoid this scenario, but they are still a WIP.

### Solutions
#### Paritioned entity
* Separates the entity over many data shares
* If your message/messages are sent in a transaction scope, you will need to use a ParitionKey
* Uses round robin to send the messages to many data shares
* The trade off with this is that you lose message ordering
* More details can be found [here](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-partitioning) and [here](https://azure.microsoft.com/en-us/blog/partitioned-service-bus-queues-and-topics/)


#### Retry Policy
* If you're using C# You can use something like [Polly](https://github.com/App-vNext/Polly) to catch the ```ServerBusyException``` exception, wait, and then rety.

#### Active / Passive Replication
* You could run 2 instances in different regions. You could try Instance 1 for X number of minutes, if it fails, then fail over use instance 2.

#### Premium Namespace
* This will run your instance on your own dedicated hardware. It's costly, but you will not incur the Noisy Neighbor issue.
