# Week 7 [13022023 - 17022023:]

## ================= What is the Klathyn ? =================

https://docs.klaytn.foundation/content/klaytn/why-klaytn

## ============= Types of (Progressive) Deliveries =============

[![Progressive Delivery Explained - Big Bang (Recreate), Blue-Green, Rolling Updates, Canaries](https://img.youtube.com/vi/HKkhD6nokC8/0.jpg)](https://www.youtube.com/watch?v=HKkhD6nokC8 "Progressive Delivery Explained - Big Bang (Recreate), Blue-Green, Rolling Updates, Canaries")

- Big Bang Delivery; shut version 1 down first, then spin version 2 up in its place.

- Blue-Green Delivery; re-route your old application (green) to your running new application (blue)(pods), then update your old application to your newer application (version 1 -> version 3) and do the same when upgrading. At all times, there should be a cluster of your old application ready for a rollback if needed, AND you should not cut the existing connections that exists when re-routing (let the established connections naturally die off).

- Rolling Update (default deployment strategy for kubernetes); spin up 1 pod of your new application, and re-route 1 pod of your old application to that new pod, then shut down that old application pod when all established connections to that pod dies off. Continue doing so until your entire cluster size reaches the intended size.

    - Benefits: Firstly, you do not need to run both versions in parallel, unlike Blue-Green delivery. Therefore not requiring to double the size of your required infrastructure.

    - Secondly (more importantly), you can rely on Health-Checks to access this delivery process. Example, if whichever provisioned newly versioned pod is deemed 'unhealthy', the delivery process will naturally come to a halt without human intervention.

- Canary Delivery/Deployment; involves running two versions of the application simultaneously. We’ll call the old version “the stable” and the new “the canary.” We have two ways of deploying the update: rolling deployments and side-by-side deployments. We use an agent to help collect metrics, and route by percentages according to that metrics. 

    - Think of your GCP load balancing, where you access health-checks (via CPU usage, network connectivity, existing RAM left, etc.) to route a certain percentage of your users accross different applications. 

https://semaphoreci.com/blog/what-is-canary-deployment#:~:text=In%20software%20engineering%2C%20canary%20deployment,the%20rest%20of%20the%20users.

## ===== Monorepo vs Multirepo Design as your Source-of-Truth =====

Benefits of Monorepo

<ul>
    <li>Lowers Barriers of Entry (easier to assign required permissions / policies)</li>
    <li>Centrally Located Code Management (single source of truth to look at)</li>
    <li>Painless Application-Wide Refactorings; When creating an application-wide refactoring of the code, multiple libraries will be affected. If you’re hosting them via multiple repositories, managing all the different pull requests to keep them synchronized with each other can prove to be a challenge. A monorepo makes it easy to perform all modifications to all code for all libraries and submit it under a single pull request.</li>
</ul>

Downsides to a Monorepo

<ul>
    <li>Slower Development Cycles; The more tests to run, the more time it takes to run them, slowing down how fast we can iterate on our code. We do not want to run tests for code that are not highly correlated with one another (mildly related to code can skip tests).</li>
    <li>Requires Download of Entire Codebase; Dealing with a vast codebase implies a poor use of space on our hard drives and slower interactions with it. For instance, everyday actions such as executing git status or searching in the codebase with a regex may take many seconds or even minutes longer than they would with multiple repos.</li>
    <li>Unmodified Libraries May Be Newly Versioned; When we tag the monorepo, all code within is assigned the new tag. If this action triggers a new release, then all libraries hosted in the repository will be newly released with the version number from the tag, even though many of those libraries may not have had any change.</li>
</ul>

Benefits of Multirepo

<ul>
    <li>Independent Library Versioning; When tagging a repository, its whole codebase is assigned the “new” tag. Since only the code for a specific library is on the repository, the library can be tagged and versioned independently of all other libraries hosted elsewhere.</li>
    <li>Helps Define Access Control Across the Organization; Only the team members involved with developing a library need to be added to the corresponding repository and download its code.</li>
</ul>

Downsides to a Multirepo

<ul>
    <li>Libraries Must Constantly Be Resynced; When a new version of a library containing breaking changes is released, libraries depending on this library will need to be adapted to start using the latest version. If the release cycle of the library is faster than that of its dependent libraries, they could quickly become out of sync with each other.</li>
</ul>

https://kinsta.com/blog/monorepo-vs-multi-repo/

## =============== Terraforming GKE Clusters ==============

https://learnk8s.io/terraform-gke

GKE nodes run on their proprietary OS optimized for docker containers. Container-Optimized OS does not include a package manager, you can use the pre-installed toolbox utility to install any additional packages or tools you require.

```bash
    $ /usr/bin/toolbox
```

https://cloud.google.com/container-optimized-os/docs/how-to/toolbox

### --------------------------- Why deploy with Terraform? ---------------------------

<ul>
    <li>Full Lifecycle Management; Terraform doesn't only create resources, it updates, and deletes tracked resources without requiring you to inspect the API to identify those resources.</li>
    <li>Graph of Relationships; Terraform understands dependency relationships between resources. For example, if you require a separately managed node pool, Terraform won't attempt to create the node pool if the GKE cluster failed to create.</li>
    <li>Best Practices: 3 nodes in each region (1 node in each AZ) is for high availability, nothing to do with software. 3 regions because you want prevent multi-regional failure -> total is 9 fullnode (and respective validator) replicas running at one time.</li>
</ul>

Sync local env with GCP: (install kubectl and gke-gcloud-auth-plugin via gcloud cli locally and bind)

```bash
    $ curl https://sdk.cloud.google.com | bash

    $ gcloud config set project PROJECT_ID

    $ ln -s /usr/local/Caskroom/google-cloud-sdk/latest/google-cloud-sdk/bin/kubectl /usr/local/bin/

    $ gke-gcloud-auth-plugin --version

    $ gcloud container clusters get-credentials <Cluster Name> \--region=<compute region>    
```

https://github.com/argoproj/argo-cd/issues/1063