# Week 6 [06022023 - 10022023:]

## ================= What is the Fantom ? =================

https://www.quicknode.com/docs/fantom

## ====================== ArgoCD ======================

Before we start on ArgoCD. What is the difference between Continuous Delivery and Continuous Deployment? The difference between continuous delivery and continuous deployment is the presence of a manual approval to update to production.

There can be multiple, parallel test stages before a production deployment.  With continuous deployment, production happens automatically without explicit approval. 

<img src="http://35.240.250.152/wp-content/uploads/2023/03/image30.png" alt="CICDpipeline" width="100%">

Continuous delivery automates the entire software release process. With continuous delivery, every code change is built, tested, and then pushed to a non-production testing or staging environment. Every revision that is committed triggers an automated flow that builds, tests, and then stages the update. The final decision to deploy to a live production environment is then triggered by the developer manually.

https://aws.amazon.com/devops/continuous-delivery/#:~:text=The%20difference%20between%20continuous%20delivery,the%20entire%20software%20release%20process.

Challenges faced with a push-based CD tool (e.g. classic Jenkins)

<ul>
    <li>Requires to install and setup tools (kubectl, helm, etc.) on your CD tool to access and execute changes on your kubernetes cluster.</li>
    <li>Requires to configure access from your CD tool to your cluster. Configuring security keys through your CSP firewall, cluster policies etc.). </li>
    <li>Not only is the above an additional effort, you are configuring security keys to an external tool outside of your cluster, imposing a security concern.</li>
    <li>Once Jenkins deploys any changes into the environment, no visibility of deployment status (do not know if deployment failed etc.).</li>
</ul>

<img src="http://35.240.250.152/wp-content/uploads/2023/03/image31.png" alt="ArgoPull" width="100%">

ArgoCD is a Kubernetes-native continuous DELIVERY (CD) tool. Unlike external CD tools that only enable push-based deliveries/deployments, ArgoCD agents PULL updated code from Git repositories and deploy it (if configured instead of deliver) directly to Kubernetes resources.

It is a Best Practice to separate the git repository for your application source code and your application configuration files (k8 manifest files). Why?

<ul>
    <li>You do not want to trigger a full CI pipeline when your application source code has not changed.</li>
    <li>You do not want complex logics in your CI pipeline.</li>
</ul>

<img src="http://35.240.250.152/wp-content/uploads/2023/03/image32.png" alt="GitOps" width="100%">

As described above, when someone manually changes the state of the cluster, ArgoCD will simply re-deploy the cluster into its actual state by tracking and regarding the Git repository as its Single Source of Truth (when Actual and Desired states have diverged).

<img src="http://35.240.250.152/wp-content/uploads/2023/03/image33.png" alt="GitOps with Multiple Clusters 0" width="100%">

Note: you only need 1 ArgoCD instance to manage a fleet of K8s clusters.

<img src="http://35.240.250.152/wp-content/uploads/2023/03/image34.png" alt="GitOps with Multiple Clusters 1" width="100%">

When looking to implement a sequence of phases into your CD pipeline (e.g. Deploying to a development before a staging, and lastly the production environment.), you can use git branches, but is better recommended to use overlays with kustomize.

https://codefresh.io/learn/argo-cd/

Installing of ArgoCD services and application (inclusive of Agent and UI):

<ul>
    <li>You do not want to trigger a full CI pipeline when your application source code has not changed.</li>
    <li>You do not want complex logics in your CI pipeline.</li>
</ul>

```bash
	$ kubectl create namespace argocd

    $ kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

    $ kubectl port-forward --address 0.0.0.0 svc/argocd-server -n argocd 8080:443

    $ kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

https://argo-cd.readthedocs.io/en/stable/getting_started/

ArgoCD YouTube tutorial: (click to go to video)

[![ArgoCD Tutorial for Beginners | GitOps CD for Kubernetes](https://img.youtube.com/vi/MeU5_k9ssrs/0.jpg)](https://www.youtube.com/watch?v=MeU5_k9ssrs "ArgoCD Tutorial for Beginners | GitOps CD for Kubernetes")

### ------ Using ArgoCD to manage GitOps when working with Kubernetes Clusters ------

to be input, your gitops different environments test case

https://docs.quay.io/guides/create-repo.html
https://quay.io/repository/wongkanghong/workshop1?tab=tags

https://blog.kintone.io/entry/production-grade-delivery-workflow-using-argocd

https://bluelight.co/blog/managing-a-gitops-flow-for-kubernetes-clusters-with-argocd

https://medium.com/@eladbitton/gitops-application-promotion-using-argo-cd-887500d2d14

## ================= Kubernetes Recap 1: =================

Considerations to start multi-node clusters on minikube:

- https://minikube.sigs.k8s.io/docs/tutorials/multi_node/

Note: driver=none DOES NOT mean multi-node cluster, it simply means to skip VM creation, and use local docker environment.

https://minikube.sigs.k8s.io/docs/drivers/none/
https://github.com/kubernetes/minikube/issues/14410

### ---------------------------------- Namespaces ----------------------------------

In Kubernetes, namespaces provides a mechanism for isolating groups of resources within a single cluster. Names of resources need to be unique within a namespace, but not across namespaces. Namespace-based scoping is applicable only for namespaced objects (e.g. Deployments, Services, etc) and not for cluster-wide objects (e.g. StorageClass, Nodes, PersistentVolumes, etc).

Deleting everything from a specific namespace: (Note that deleting a namespace itself (and re-creating) also deletes everything from that specific namespace)
    
```bash
	$ kubectl delete all --all -n <namespace> 

    OR

    $ kubectl delete namespace <namespace>

    $ kubectl create namespace <namespace>
```

### ------------------------------ Pod Security Standards ------------------------------

The Pod Security admission controller replaces PodSecurityPolicy (legacy solution), making it easier to enforce predefined Pod Security Standards by simply adding a label to a namespace. The Pod Security Standards are maintained by the K8s community, which means you automatically get updated security policies whenever new security-impacting Kubernetes features are introduced.

<ul>
    <li>Privileged - Unrestricted policy, providing the widest possible level of permissions. This policy allows for known privilege escalations.</li>
    <li>Baseline - Minimally restrictive policy which prevents known privilege escalations. Allows the default (minimally specified) Pod configuration.</li>
    <li>Restricted - Heavily restricted policy, following current Pod hardening best practices.</li>
</ul>

https://kubernetes.io/blog/2022/08/25/pod-security-admission-stable/ 
https://kubernetes.io/docs/tutorials/security/ns-level-pss/

## ==== Pod Security Standards implemented as Kyverno policies ====

Kyverno is a policy engine designed specifically for Kubernetes. Kyverno runs as a dynamic admission controller in a Kubernetes cluster. Kyverno receives validating and mutating admission webhook HTTP callbacks from the kube-apiserver and applies matching policies to return results that enforce admission policies or reject requests.

<img src="http://35.240.250.152/wp-content/uploads/2023/03/image11.png" alt="KyvernoHighLevelArch" width="100%">

https://kyverno.io/docs/introduction/
https://kyverno.io/policies/pod-security/
https://medium.com/@charled.breteche/kubernetes-security-pod-security-standards-using-kyverno-cc5d9042b79a