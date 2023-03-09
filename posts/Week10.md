# Week 10 [06032023 - 10032023:]

## == Deploying containerized apps using Terraform's Helm Providers ==

Why should you use Helm provider instead of using a separate pipeline or a simple bash script? Pros:

- bash scripts are not developer-friendly (same reason as to why we automate tests with Go).

- a single source of truth helps visibility, maintainability and increases durability and/or stability.

Cons:

- complete charts produces loads of changes and output of Terraform plan can get difficult to interpret (applies to all approaches)
