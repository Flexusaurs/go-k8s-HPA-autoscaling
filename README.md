# go-k8s-HPA-autoscaling
a kubernetes config for HPA


This whole example is based around a simple http service that generates tiny bits of cpu load for every request it gets,
in addition to a stress tester (traffic-generator service) that runs on a seperate pod under the same node, orchestrated
with _kind_.

I created a base docker image of the go service, then depended on it in the Deployment.yaml file,
then installed the metric server from yaml and modified my kubeconfig to allow the metrics server to run in the background.

After applying all yaml files, I exec'd into the traffic generator, and installed wrk for stress testing the service.
and then:
``` wrk -c 5 -t 5 -d 99999 -H "connection: close" http://10.96.253.93:80 ```

after starting the stress test i ran ```  kubectl autoscale deploy/application-cpu --cpu-percent=95 --min=1 --max=10 ``` 
in order to autoscale the number of pods according to the load, with casually checking the number of pods with 

```kubectl get pods```
and 
``` kubectl top pods```

the final result is not much, but a simple hardware based load balance

