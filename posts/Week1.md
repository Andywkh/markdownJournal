# Week 1 [03012023 (Tuesday) - 06012023:]

## Learning Courses Completed

<ul>
	<li>Security Awareness</li>
	<li>Privacy and Data Protection Basics</li>
	<li>Anti-Money Laundering and Counter Terrorism</li>
	<li>Staff Dealing Policy</li>
</ul>

<img src="http://35.240.250.152/wp-content/uploads/2023/03/image38.png" alt="Binance Culture" width="100%">

<ul>
	<li>New Employee Training (done in Week 8)</li>
	<li>Anti Bribery and Corruption Training 2023 (done in Week 8)</li>
</ul>

## ============= What is the Arbitrum Protocol ? =============

Arbitrum is an EVM-compatible Layer 2 scaling solution for the Ethereum network that leverages optimistic rollups to introduce several efficiency improvements to transaction processing and their costs. To do this, Arbitrum bundles several off-chain transactions together in a batch before parsing the result to the Ethereum network.

<img src="http://35.240.250.152/wp-content/uploads/2023/03/image1.png" alt="BlockchainLayer2" width="100%">

Arbitrum aims to reduce transaction fees and congestion by moving as much computation and data storage off of Ethereum’s main blockchain (layer 1) as it can. Storing data off of Ethereum’s blockchain is known as Layer 2 scaling solutions. This is because it is built on top of Layer 1 (the main Ethereum network) and retains the security of Ethereum.

Difference between Arbitrum One, Nova and Nitro.

<ul>
	<li>Nitro is an upgrade of One’s technology stack, not a network independent of One. The full name of Nitro after the upgrade is still Arbitrum One; while Nova, which was launched in early August, is a network independent of One.</li>
	<li>The core difference is data availability. One’s data availability is on-chain (Ethereum mainnet), and Nova’s data availability is off-chain (Data Availability Committee DAC).</li>
	<li>One stores the off-chain execution data on the Ethereum main network, and Nova stores the data on the off-chain Data Availability Committee. Compared with One, Nova improves performance by sacrificing certain security. Dapps that require high-frequency interaction such as games and social networking are suitable for deployment on Nova.</li>
</ul>

https://ethereum.org/en/developers/docs/
https://docs.chainstack.com/api/arbitrum/arbitrum-api-reference#what-is-arbitrum-protocol
https://www.quicknode.com/docs/arbitrum
https://www.benzinga.com/money/what-is-arbitrum

## ==== Evolution of Application Designs, What are Microservices? ====

<img src="http://35.240.250.152/wp-content/uploads/2023/03/image35.png" alt="Monolithic" width="100%">

Monolithic applications are hard to maintain and scale as such applications are highly tangled together. Note: we still use monolithic applications when considering a smaller user-base as they are easier to manage and monitor, and are more secure.

<img src="http://35.240.250.152/wp-content/uploads/2023/03/image36.png" alt="MultiTier" width="100%">

Even with a Multi-Tier application design (Presentation Layer managed by Frontend Devs, Logic Layer managed by Backend Devs and Data Layer managed by Data Admins), you overall application is still Monolithic by design when considering complex integrations with systems.

<img src="http://35.240.250.152/wp-content/uploads/2023/03/image37.png" alt="Microservices" width="100%">

With a microservice application design, each microservice is responsible for a single business function, end-to-end, and independent from other microservices. They communicate with other microservices through APIs via lightweight protocols like HTTP. 

Therefore, each team responsible for their microservices are able to maintain and scale without having to impact other teams and their respective business functions.

With the flexibility of microservices, teams can use a multitude of different CSPs and frameworks/languages to serve their microservices. However, we commonly stick to using the same technology because of the following reasons:

<ul>
	<li>Cost Reduction</li>
	<li>Efficiency Improvement</li>
	<li>Operational Optimization</li>
</ul>

We use tools to manage these highly distributed microservice-based systems. Containerization is the process of packaging software code into minimalist self-contained runtimes (with just the OS libraries and required dependencies to run their respective microservices). Example runtimes include Docker, Podman and Singularity, etc..

Finally, tools to manage, more specifically, to orchestrate these containerized microservices, include Kubernetes and OpenShift, etc..

## ===== React.JS Recap, Spun up an Intern Accountability Blog =====

The option to link multiple frontend channels to the backend API for what’s often called a “headless” architecture. In those cases, there is no default frontend system that defines how the content will appear to users.

Benefits of a decoupled (or headless) architecture (frontend from backend):

<ul>
	<li>Swift Solution Delivery; Decoupled architecture facilitates frontend and backend developers to collaborate and work in parallel without any dependencies and bottlenecks. The application gets broken down at the functional level, and backend teams can independently work on specific delivering the APIs, whereas the frontend team shall develop the interfaces bases on mock APIs.</li>
	<li>Drive Innovations; This architecture enables building frontend and backend on different tech stacks that suit the requirement. This flexibility gives the freedom to choose best-in-class technology independently for the frontend and backend components without any limitations. Also, it empowers developers to quickly switch to new technologies and frameworks to keep up with innovations without much cost implications.</li>
	<li>Faster website performance and better scalability; Your site’s frontend runs more efficiently when freed from the full database that constitutes your website backend and its technical debt. A decoupled frontend better prepares your website to handle traffic spikes and deliver a consistent user experience.</li>
</ul>

Note: Headless architectures have your backend and frontends on different servers. Decoupled architectures are a superset of your Headless architecture, just with a presentation layer integrated.

<img src="http://35.240.250.152/wp-content/uploads/2023/03/image41.png" alt="HeadlessVsDecoupled" width="100%">

What I have developed with regards to this Accountability Blog, is a DECOUPLED architecture. The WordPress CMS (tied to a running local mariaDB) has an exposed presentation layer to have users excess the CMS, and add blog posts manually via UI. The WP RESTful API then exposes these posts to external React.JS frontend(s).

==========================================================================

Technologies that were hinted, and I am reading up on (with their parallels):

<ul>
	<li>IaC -> Crossplane (Kube-Native) vs Terraform</li>
	<li>CI/CD -> Tekton (CI) [Covered in Week 8] and ArgoCD (CD) [Covered in Week 6] vs Jenkins</li>
	<li>Mathematically-Intuitive (taking aggregate) Kube-Native resource Autoscaler -> Karpenter [Covered in Week 3]</li>
	<li>Kube-Native Package Manager -> Helm (apt/yum/homebrew for Kubernetes) [Covered in Week 5] vs Kustomize [Covered in Week 8]</li>
<ul>
	