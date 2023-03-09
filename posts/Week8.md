# Week 8 [20022023 - 24022023:]

## === Raising PRs in a Mono Repo env via a CLI coded with GoLang ===

An example of how we used it here, tekton doesnt support mono repos natively so you have to write your own code to ensure that the tekton event listener can understand specific github events. I have done that with golang: its also used to raise prs for auto incrementing blockchain versions, ensures that we dont have to do any manual work beyond creating a new tag and ive bundled it up into a command line tool that can be called in our pipelines.

### -------------------------------- What is Kustomize? --------------------------------

In computer science, declarative programming is a programming paradigm (a style of building the structure and elements of computer programs that expresses the logic of a computation without describing its control flow.)

<ul>
    <li>Kubernetes is Complex: which means there are a lot of possible permutations to get from the start to the destination.</li>
    <li>Kubernetes is Declarative: which means that given the final manifest kubernetes will determine how to get the cluster to the point where it matches your wants. Declarative meaning the config is unambiguous, deterministic and not system dependent.</li>
</ul>

In computer science, imperative programming is a programming paradigm that uses statements that change a program’s state. In much the same way that the imperative mode in natural languages expresses commands, an imperative program consists of commands for the computer to perform.

Kustomize: is a declarative tool, which works with yaml directly and works as <strong>a stream editor like sed</strong>. It traverses a Kubernetes manifest to add, remove or update configuration options without forking.

Pros for Helm:

<ul>
    <li>The helm template functions are powerful in their own right; you have conditionals, loops over ranges, you can define your own helpers, and then you have the whole sprig library at your fingertips, with this comes complexity and imperative templating.</li>
    <li>It boosts productivity due to the large amount of existing helm charts already out there.</li>
</ul>

Cons for Helm:

<ul>
    <li>More abstraction layers and steep-learning curve required.</li>
    <li>Not natively supported and therefore requires an external dependency.</li>
    <li>Even though the package manager works well, it still requires a decent set of customizations applied at runtime to handle image updates and other specifics, which increases the complexity of the ci/cd process.</li>
</ul>

Pros for Kustomize:

<ul>
    <li>Declarative, so it matches with the same ideologies of kubernetes itself.</li>
    <li>Native in as of kubectl v1.14, which means no external tools needed.</li>
    <li>Every artifact that Kustomize validates and processes uses plain YAML.</li>
    <li>Kustomize encourages a fork/modify/rebase workflow.</li>
    <li><strong>It’s not a templating engine, but a yaml patching system.</strong></li>
</ul>

Helm is a CLI tool that is used to templatize kubernetes manifests. Kustomize is a CLI tool that is used to patch kubernetes manifests (adding configurations to existing manifests, most preferably ones that can be found online from open-sourced projects).

https://foghornconsulting.com/2022/01/25/helm-versus-kustomize/

### ------------------------ What is Tekton as a CI tool? ------------------------

[![Tekton - Kubernetes Cloud-Native CI/CD Pipelines And Workflows](https://img.youtube.com/vi/7mvrpxz_BfE/0.jpg)](https://www.youtube.com/watch?v=7mvrpxz_BfE "Tekton - Kubernetes Cloud-Native CI/CD Pipelines And Workflows")

## ======== Automating Tests via a CLI coded with GoLang ========

Bash scripting is generally difficult to maintain. As a general rule more than 5 lines of bash is too much for any given task. When you get to hundreds of lines of bash then it becomes an unmaintainable mess that no-one dares touch. All depends on what youre trying to accomplish but golang is lightweight, easy to code, easy to maintain, compiles almost instantly and has great support for command line tool creation.

### ------- Unit vs Fuzz Testing -------

Unit testing are tests that are written to automatically run with every build of the application. They are written close to the code and should pass when running a build of the application. What type of code coverage is required for the APIs depends on the risk the API carries and what functionalities it holds. Good unit testing is like a good foundation and this aspect should be well thought over as it will carry the rest of the testing effort later down the line.

As a final test before we validate our application we need to fuzz all the endpoints of our APIs. When fuzzing we will send random data to those API endpoints and we need to carefully inspect the results. Our server should not crash from this unexpected traffic and it should not display any odd behavior. Based on a risk analysis, fuzz testing might be performed much more structured or not at all.

https://www.wallarm.com/what/what-is-api-testing-benefits-types-how-to-start

## ======== Project Innovation 1: a new Testing System ========

### ------- Part 1: Unit Testing via GoLang CLI within Tekton CI -------

The purpose of this component is to trigger unit tests when the team pushes their new code into the QA environment. This increases visibility via a quick feedback to check if there are any high-level issues with regards to the exposed API of a new node.

### ------- Part 2: Fuzz Testing via WordPress GraphQL API -------

The purpose of this component is to provide a centralized system to test edge cases before passing the API over to wallet team for validation testing.

