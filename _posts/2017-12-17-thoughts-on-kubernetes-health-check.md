---
id: 1490
title: Thoughts on Kubernetes Health Check
date: 2017-12-17T19:57:32+00:00
author: raymondchen625
layout: post
guid: https://raymondchen625.wordpress.com/?p=1490
permalink: /2017/12/17/thoughts-on-kubernetes-health-check/
categories:
  - Tech
---
<div>
      We’ve been working on the health check of Kubernees lately. The liveness and readiness checks sometimes seem confusing. Trying to record my thoughts here.
</div>

<div>
</div>

<div style="padding-left:30px;">
  1) Official docs: <a href="https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/">https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/</a>
</div>

<div style="padding-left:30px;">
  2) First of all, liveness and readiness checks are two customizable probing endpoints. Their implementations make a big difference. We can mess it up if we haven’t implemented them in alignment with the expectation of its caller, Kubernetes.
</div>

<div style="padding-left:30px;">
  3) In the eyes of K8s, they serve different purposes:
</div>

<li style="list-style-type:none;">
  <ul>
    <li>
      Liveness check: Decide whether it should restart the container
    </li>
  </ul>
</li>

<li style="list-style-type:none;">
  <ul>
    <li>
      Readiness check: Decide whether to direct the traffic to this pod, technically whether to keep the pod on the service IP list
    </li>
  </ul>
</li>

&nbsp;

<div style="padding-left:30px;">
  4) When k8s works on one of the health checks, it usually doesn&#8217;t care about the status of the other one, unless they are somehow related in the implementation(but the idea is they are <strong>independent</strong>).
</div>

<div>
</div>

<div>
      If liveness check fails, k8s will try to restart it if the restart policy allows. But it won’t do anything about the traffic redirection. That’s not its job.
</div>

<div>
      If readiness check fails, k8s will take it off the service IP list so external request won’t reach this not-ready pod. But it will never restart it.
</div>

<div>
   <img class="alignnone size-full wp-image-1491" src="http://localhost/wp-content/uploads/2017/12/frabz-youre-alive-i-thought-you-was-dead-7d01a5.jpg" alt="frabz-youre-alive-i-thought-you-was-dead-7d01a5" width="490" height="372" srcset="http://localhost/wp-content/uploads/2017/12/frabz-youre-alive-i-thought-you-was-dead-7d01a5.jpg 490w, http://localhost/wp-content/uploads/2017/12/frabz-youre-alive-i-thought-you-was-dead-7d01a5-300x228.jpg 300w" sizes="(max-width: 490px) 100vw, 490px" />
</div>

<div>
      In common sense, we will assume the pod should always be alive if it’s ready. Well, that’s only true if our implementation makes sure of that. In reality there could be scenario when liveness check fails but readiness check passes, when the pod can serve requests although we see red color in liveness check. This might not be a problem depending on the scenario.
</div>

<div>
      I think in ideal environment the health check logic should be an appropriate response to a correct expectation on the problems that will occur in production. Its response should contain decision both from the system level and the application level.
</div>

<div>
      Trying to make up an scenario to express my thought: out of database connection. If we have an application which runs out of the db connection on a pod. We know the reason is that there are many long-running threads which take one of the db connection each. The are not dead and will release the connection once the job is finished.
</div>

<div>
      Apparently we should fail the readiness so it won’t serve any new request because it’s not able to. We might not need to fail the liveness check because we don’t want to restart the container, which might interrupt the running threads. But if the out of db connection is caused by some unknown reason, we will never reclaim the db connection, which will jeopardize the whole system. In that case we might need to restart the container to reclaim those lost connections. In this case we need to fail liveness check.
</div>

<div>
      We need the application to be smart enough to tell the difference between the 2 above scenarios. If it is smart, it should restart the container on scenario #2 by failing the liveness check. It should take the pod off the replica set of the service, one new pod will be created. Of course when the old pod comes back, one of the pods has to be evicted. We might have a pod lifecycle hook to make sure no unfinished work is still running&#8230;
</div>

<div>
</div>