To maximise CPU utilisation, most modern OS support **concurrency** - the ability to run multiple programs concurrently
The **process scheduler** selects among available processes for next execution on CPU
Maintains scheduling queues of processes:
- **Job queue** - (**long term** scheduler) Set of all processes in the **new** state
- **Ready queue** - (**short term** scheduler) Set of all processes in the **ready** state
- **Device queue** - Set of processes **waiting** for an I/O device
###### Ready queue and device queue:

![[Pasted image 20231005110615.png]] 
###### Queueing Diagram

![[Pasted image 20231005110709.png]]
### Schedulers
#### **Short term scheduler**
Selects the next process to be executed **from** the **ready** queue
	- Invoked frequently; at least once every 100ms
	- Must be very fast in order to make sure CPU time isn't wasted
#### **Long term scheduler**
Selects the next process in the **new** state to be brought into main memory **into** the **ready** queue
	- Much less frequent; may be minutes between creating one process and the next
	- Controls the degree of **multiprogramming** (number of processes in memory)
		- If stable, arrival rate of jobs is equal to completion rate
	Processes can be described as either:
	- **I/O Bound** - Spends more time doing **I/O** than computation (**Short** CPU bursts)
	- **CPU Bound** - Spends more time doing **computation** (**Long** CPU bursts)
	Long-term scheduler strives for good process mix