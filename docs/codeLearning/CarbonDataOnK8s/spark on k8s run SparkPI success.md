## 运行脚本：

	  spark-submit \
	    --deploy-mode cluster \
	    --class org.apache.spark.examples.SparkPi \
	    --master k8s://https://192.168.0.188:5443 \
	    --kubernetes-namespace default \
	    --conf spark.executor.instances=3 \
	    --conf spark.app.name=spark-pi \
	    --conf spark.kubernetes.driver.docker.image=100.125.0.198:20202/kernel/spark-driver:v2.2.0-kubernetes-0.5.0 \
	    --conf spark.kubernetes.executor.docker.image=100.125.0.198:20202/kernel/spark-executor:v2.2.0-kubernetes-0.5.0 \
	    --conf spark.kubernetes.initcontainer.docker.image=100.125.0.198:20202/kernel/spark-init:v2.2.0-kubernetes-0.5.0 \
	    local:///root/xubo/git/k8s/original-spark-examples_2.11-2.3.1-SNAPSHOT.jar
	    #--master k8s://https://192.168.0.188:5443 \
	    #--master k8s:// 127.0.0.1:8001 \
	    #--conf spark.kubernetes.driver.docker.image=kubespark/spark-driver:v2.2.0-kubernetes-0.5.0 \
	    #--conf spark.kubernetes.executor.docker.image=kubespark/spark-executor:v2.2.0-kubernetes-0.5.0 \

    
记录：

    [root@sparkonk8s-46929-p4jdl ~]# netstat -apn |grep 10248
    tcp        0      0 127.0.0.1:10248         0.0.0.0:*               LISTEN      453/localkube
    [root@sparkonk8s-46929-p4jdl ~]# minikube
    Minikube is a CLI tool that provisions and manages single-node Kubernetes clusters optimized for development workflows.

    Usage:
      minikube [command]

    Available Commands:
      addons           Modify minikube's kubernetes addons
      cache            Add or delete an image from the local cache.
      completion       Outputs minikube shell completion for the given shell (bash or zsh)
      config           Modify minikube config
      dashboard        Opens/displays the kubernetes dashboard URL for your local cluster
      delete           Deletes a local kubernetes cluster
      docker-env       Sets up docker env variables; similar to '$(docker-machine env)'
      get-k8s-versions Gets the list of Kubernetes versions available for minikube when using the localkube bootstrapper
      ip               Retrieves the IP address of the running cluster
      logs             Gets the logs of the running localkube instance, used for debugging minikube, not user code
      mount            Mounts the specified directory into minikube
      profile          Profile sets the current minikube profile
      service          Gets the kubernetes URL(s) for the specified service in your local cluster
      ssh              Log into or run a command on a machine with SSH; similar to 'docker-machine ssh'
      ssh-key          Retrieve the ssh identity key path of the specified cluster
      start            Starts a local kubernetes cluster
      status           Gets the status of a local kubernetes cluster
      stop             Stops a running local kubernetes cluster
      update-check     Print current and latest version number
      update-context   Verify the IP address of the running cluster in kubeconfig.
      version          Print the version of minikube

    Flags:
          --alsologtostderr                  log to standard error as well as files
      -b, --bootstrapper string              The name of the cluster bootstrapper that will set up the kubernetes cluster. (default "localkube")
      -h, --help                             help for minikube
          --log_backtrace_at traceLocation   when logging hits line file:N, emit a stack trace (default :0)
          --log_dir string                   If non-empty, write log files in this directory
          --loglevel int                     Log level (0 = DEBUG, 5 = FATAL) (default 1)
          --logtostderr                      log to standard error instead of files
      -p, --profile string                   The name of the minikube VM being used.
            This can be modified to allow for multiple minikube instances to be run independently (default "minikube")
          --stderrthreshold severity         logs at or above this threshold go to stderr (default 2)
      -v, --v Level                          log level for V logs
          --vmodule moduleSpec               comma-separated list of pattern=N settings for file-filtered logging

    Use "minikube [command] --help" for more information about a command.
    [root@sparkonk8s-46929-p4jdl ~]# minikube status
    minikube: Running
    cluster: Running
    kubectl: Correctly Configured: pointing to minikube-vm at 192.168.0.202
    [root@sparkonk8s-46929-p4jdl ~]# minikube delete
    Deleting local Kubernetes cluster...
    Machine deleted.
    [root@sparkonk8s-46929-p4jdl ~]# minikube status
    minikube:
    cluster:
    kubectl:
    [root@sparkonk8s-46929-p4jdl ~]# netstat -apn |grep 10248
    [root@sparkonk8s-46929-p4jdl ~]# netstat -apn |grep 10255
    [root@sparkonk8s-46929-p4jdl ~]# kubectl get pods
    NAME            READY     STATUS    RESTARTS   AGE
    icagent-2fhzc   0/0       Running   0          3d
    icagent-d84d7   0/0       Running   0          3d
    icagent-dnfmw   0/0       Running   0          3d
    icagent-f765n   0/0       Running   0          3d
    icagent-wl12q   0/0       Running   0          3d
    [root@sparkonk8s-46929-p4jdl ~]# kubectl get node
    NAME            STATUS    ROLES     AGE       VERSION
    192.168.0.120   Ready     <none>    6d        v1.7.3-CCE2.0.4-3.5.2
    192.168.0.124   Ready     <none>    6d        v1.7.3-CCE2.0.4-3.5.2
    192.168.0.174   Ready     <none>    6d        v1.7.3-CCE2.0.4-3.5.2
    192.168.0.202   Ready     <none>    6d        v1.7.3-CCE2.0.4-3.5.2
    192.168.0.29    Ready     <none>    6d        v1.7.3-CCE2.0.4-3.5.2
    [root@sparkonk8s-46929-p4jdl ~]# cd k8s/
    [root@sparkonk8s-46929-p4jdl k8s]# ls
    backup  build201803061753.txt  count.py  k8s2.sh  k8s.sh  namespace  nginxv4.yaml  nginxv5.yaml  nginxv6.yaml  nginx.yaml  rundockerspark.sh  runproxy.sh  run.sh
    [root@sparkonk8s-46929-p4jdl k8s]# ./k8s.sh
    2018-03-08 07:16:59 WARN  Utils:66 - Your hostname, sparkonk8s-46929-p4jdl.novalocal resolves to a loopback address: 127.0.0.1; using 192.168.0.202 instead (on interface eth0)
    2018-03-08 07:16:59 WARN  Utils:66 - Set SPARK_LOCAL_IP if you need to bind to another address
    2018-03-08 07:17:00 INFO  LoggingPodStatusWatcherImpl:54 - State changed, new state:
             pod name: spark-pi-1520493418441-driver
             namespace: default
             labels: spark-app-selector -> spark-f371f545b10f4174ad3b1b67ff4b6e90, spark-role -> driver
             pod uid: b16fc160-22a0-11e8-89bc-fa163e44a675
             creation time: 2018-03-08T07:17:00Z
             service account name: default
             volumes: spark-local-dir-0-spark-8335835b-3758-436b-b0cf-6997919f07d5, default-token-8blbc
             node name: N/A
             start time: N/A
             container images: N/A
             phase: Pending
             status: []
    2018-03-08 07:17:00 INFO  LoggingPodStatusWatcherImpl:54 - State changed, new state:
             pod name: spark-pi-1520493418441-driver
             namespace: default
             labels: spark-app-selector -> spark-f371f545b10f4174ad3b1b67ff4b6e90, spark-role -> driver
             pod uid: b16fc160-22a0-11e8-89bc-fa163e44a675
             creation time: 2018-03-08T07:17:00Z
             service account name: default
             volumes: spark-local-dir-0-spark-8335835b-3758-436b-b0cf-6997919f07d5, default-token-8blbc
             node name: 192.168.0.202
             start time: N/A
             container images: N/A
             phase: Pending
             status: []
    2018-03-08 07:17:00 INFO  LoggingPodStatusWatcherImpl:54 - State changed, new state:
             pod name: spark-pi-1520493418441-driver
             namespace: default
             labels: spark-app-selector -> spark-f371f545b10f4174ad3b1b67ff4b6e90, spark-role -> driver
             pod uid: b16fc160-22a0-11e8-89bc-fa163e44a675
             creation time: 2018-03-08T07:17:00Z
             service account name: default
             volumes: spark-local-dir-0-spark-8335835b-3758-436b-b0cf-6997919f07d5, default-token-8blbc
             node name: 192.168.0.202
             start time: 2018-03-08T07:17:00Z
             container images: 100.125.0.198:20202/kernel/spark-driver:v2.2.0-kubernetes-0.5.0
             phase: Pending
             status: [ContainerStatus(containerID=null, image=100.125.0.198:20202/kernel/spark-driver:v2.2.0-kubernetes-0.5.0, imageID=, lastState=ContainerState(running=null, terminated=null, waiting=null, additionalProperties={}), name=spark-kubernetes-driver, ready=false, restartCount=0, state=ContainerState(running=null, terminated=null, waiting=ContainerStateWaiting(message=null, reason=ContainerCreating, additionalProperties={}), additionalProperties={}), additionalProperties={})]
    2018-03-08 07:17:00 INFO  Client:54 - Waiting for application spark-pi to finish...
    2018-03-08 07:17:02 INFO  LoggingPodStatusWatcherImpl:54 - State changed, new state:
             pod name: spark-pi-1520493418441-driver
             namespace: default
             labels: spark-app-selector -> spark-f371f545b10f4174ad3b1b67ff4b6e90, spark-role -> driver
             pod uid: b16fc160-22a0-11e8-89bc-fa163e44a675
             creation time: 2018-03-08T07:17:00Z
             service account name: default
             volumes: spark-local-dir-0-spark-8335835b-3758-436b-b0cf-6997919f07d5, default-token-8blbc
             node name: 192.168.0.202
             start time: 2018-03-08T07:17:00Z
             container images: 100.125.0.198:20202/kernel/spark-driver:v2.2.0-kubernetes-0.5.0
             phase: Running
             status: [ContainerStatus(containerID=docker://78f5819a0414212aa053e634fbd2a847882b2d3c92dc633d3de87355f77c7831, image=100.125.0.198:20202/kernel/spark-driver:v2.2.0-kubernetes-0.5.0, imageID=docker://sha256:38c277999de5620cfaf071cd2677a2242d3997c833044a02164dba001681c48f, lastState=ContainerState(running=null, terminated=null, waiting=null, additionalProperties={}), name=spark-kubernetes-driver, ready=true, restartCount=0, state=ContainerState(running=ContainerStateRunning(startedAt=Time(time=2018-03-08T07:17:02Z, additionalProperties={}), additionalProperties={}), terminated=null, waiting=null, additionalProperties={}), additionalProperties={})]
    2018-03-08 07:17:04 INFO  LoggingPodStatusWatcherImpl:54 - State changed, new state:
             pod name: spark-pi-1520493418441-driver
             namespace: default
             labels: spark-app-selector -> spark-f371f545b10f4174ad3b1b67ff4b6e90, spark-role -> driver
             pod uid: b16fc160-22a0-11e8-89bc-fa163e44a675
             creation time: 2018-03-08T07:17:00Z
             service account name: default
             volumes: spark-local-dir-0-spark-8335835b-3758-436b-b0cf-6997919f07d5, default-token-8blbc
             node name: 192.168.0.202
             start time: 2018-03-08T07:17:00Z
             container images: 100.125.0.198:20202/kernel/spark-driver:v2.2.0-kubernetes-0.5.0
             phase: Failed
             status: [ContainerStatus(containerID=docker://78f5819a0414212aa053e634fbd2a847882b2d3c92dc633d3de87355f77c7831, image=100.125.0.198:20202/kernel/spark-driver:v2.2.0-kubernetes-0.5.0, imageID=docker://sha256:38c277999de5620cfaf071cd2677a2242d3997c833044a02164dba001681c48f, lastState=ContainerState(running=null, terminated=null, waiting=null, additionalProperties={}), name=spark-kubernetes-driver, ready=false, restartCount=0, state=ContainerState(running=null, terminated=ContainerStateTerminated(containerID=docker://78f5819a0414212aa053e634fbd2a847882b2d3c92dc633d3de87355f77c7831, exitCode=1, finishedAt=Time(time=2018-03-08T07:17:04Z, additionalProperties={}), message=null, reason=Error, signal=null, startedAt=Time(time=2018-03-08T07:17:02Z, additionalProperties={}), additionalProperties={}), waiting=null, additionalProperties={}), additionalProperties={})]
    2018-03-08 07:17:04 INFO  LoggingPodStatusWatcherImpl:54 - Container final statuses:


             Container name: spark-kubernetes-driver
             Container image: 100.125.0.198:20202/kernel/spark-driver:v2.2.0-kubernetes-0.5.0
             Container state: Terminated
             Exit code: 1
    2018-03-08 07:17:04 INFO  Client:54 - Application spark-pi finished.
    [root@sparkonk8s-46929-p4jdl k8s]#
