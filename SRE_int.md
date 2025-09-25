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

I will start by checking debug mode  checking logs cpu & memory consuptions. 
Checking the request time (http request duration )  
Then review logs for timeout or exceptions by using APM tools like NEWRelic for pinpoint whcih function causing delays.  
looking for backend like DB issue or high traffic Overwhelming or backend service checking recent changes that could have introduce performance, and checking idile time and time request is taking and start checking journey of user request ,
origin of user latency rolling back if necessary.  

## user report slowness of application how would you approach this? 

1: use top and htop to check server load cpu memory if there is any process is least important i will depriotize the 
process if not required then killing it.  
2: checking network lateny using ping or traceroute cmd if its related to this then will implemet CDN.  
3: Bandwidth using iftop and nload   
4: Error of log application log may be database talk to upstream server that could be reason  
5: I will use Netstat if there is too many open connection wil close the conn which is not useful.  

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

## you have configured alert but got woken at 2 am by false alarm? what is your stragey to reduce nosie 

If I get a false alarm at 2 AM, my strategy is to tune thresholds, focus on SLO-based alerts tied to user impact, and use aggregation to suppress noise. I also categorize alerts by severity so only critical issues page at night.




