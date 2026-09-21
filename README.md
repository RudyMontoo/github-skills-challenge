Service: payment-service
Metrics: "response_time_ms" "cpu_percent" "memory_percent"
Logs: log-level, message
Timestamps identify event time and order
10:05: high response time and error log
10:06: high response time, CPU, memory, and error log

Flow-> detector-> producer->topic->consumer->AIOPS

Issues Solved->
Error 1-> we are detecting only warning but error also have anomaloies
sol-># both warning and  error are detected now 

Error 2 and 3->producer and consumer is calling different topics due to which consumer will not able to get relative data of producer 
sol->same topics will used by producer and consumer 



