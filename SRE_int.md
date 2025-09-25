## What is SLA SLO & SLI

SLA >> The agrement we make with our clinet,   SLO >> The objevtive your team must hit to meet the agrement.  
Sli >> The real numbers on performance.

## What is Run book?
Runbook is a documents that written a step to fix a issue which helps to on call Engineer to take consistent action without escalating.  
Eg: Disk space issue checking the disk usage removing old log and restarting service.  

## Monitoring and Observability?
Monitoring is about tracking predefined metrics and alerts to know if something is wrong. Observability goes deeper by using metrics, logs,
and traces to understand why it went wrong, even for unknown issues.

## You web application is loading slowly? what steps would you take.

Starts by checking metric latency & ERROR rates.  
Then review logs for timeout or exceptions by using APM tools like NEWRelic for pinpoint whcih function causing delays.  
looking for backend like DB issue or high traffic Overwhelming at last checking recent changes that could have introduce performance 
regression & rolling back if necessary.  

## You recive an Alert at 3 AM what steps would you take?
  
First acknowledge the alert to avoid duplications, checking system dashboard and logs To access scope & root cause of problems.  
Determing if the issue is user impacting or interneral service to prioritize response if it is critical try to apply a quick fix or 
apply a rollback of service as quick as possible, once issues fixed i will make a documents for future reference including observation 
& resolutions for postmortem process and future incident improvements.  

## Deployment startegies. 

For larger update we use BLUE GREEN deployment in k8s use readiness and liveness probe ensure proper health check and rollback 
if any requrements to minimize service interruption.  

## Database latency is increased ? what would you do?  

By checking DB CPU & MEMORY usage to determine the issue in depth using connection pool how it hitting.

## Can you describe a time where you improve system reliability?  

We used canary deployment starategies which reduce the impcat of failure as result system uptime improved by 15% 
incident response time decreased significatly as well.  

## what is capacity planning

capacity planning ensures infrastructure can handle projected load, use past usage data ,use seasonal trend & growth projection.  

## Grafana vs prometheus.

Grafana is used for dashbaord and Alert its says what is an issue.  
Prometheus is used for log alert & metrics its basically says why this issues occur.  

## What kind of metrics do you work with. 

We use prometheus as monitoring we use different type of metric we scrape. 

1: Infrastrucure related metrics we use node exporter 
CPU usage per core , FREE MEMORY , DISK IO LATENCY , NETWORK TRAFFIC.  

2: WE ALSO SCRAPE metric related to kubelet is critical component of k8s 
container memory usage , container cpu usage , NO.of restart of container. 

3: we also scrape API server of k8s just to undertand state of API server we use kubestate metric to get this info.  




