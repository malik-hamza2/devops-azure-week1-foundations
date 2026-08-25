# GitOps Repository Structure

This folder simulates a GitOps-style deployment structure for promoting application configuration across environments: Dev to Stage to Prod.

## Structure

gitops/environments/dev/values.yaml     - Lightweight config for testing
gitops/environments/stage/values.yaml   - Pre-production, closer to real load
gitops/environments/prod/values.yaml    - Production-grade, scaled config

## Relationship to Helm Chart

These values.yaml files are designed to be used with the Helm chart in ../nginx-chart/. Instead of maintaining separate full charts per environment, each environment only stores the values that differ.

Example command to deploy using an environment's values:
helm install myapp ../nginx-chart -f environments/prod/values.yaml

## PR-Based Promotion Flow (Demo)

In a real GitOps setup, changes are never applied directly to production. Instead, they follow this promotion path:

1. Developer makes a change in environments/dev/values.yaml (e.g., updating the image tag after a new build)
2. Pull Request opened targeting the main branch
3. Team reviews the PR - checks resource limits, image tag correctness
4. Once approved and merged, an automation tool (e.g., ArgoCD or FluxCD) detects the Git change and syncs it to the Dev cluster
5. After successful testing in Dev, a new PR is opened to copy the validated change into environments/stage/values.yaml
6. Same review process repeats for Stage
7. Finally, a PR promotes the change into environments/prod/values.yaml, often requiring manual approval from a senior engineer before merge

## Note on Cluster Deployment

This structure is built for demonstration purposes as part of Week 1 DevOps Internship training. No live Kubernetes cluster deployment was performed, as per task instructions (local cluster setup was unavailable). The values.yaml files are valid and can be used with helm install / helm upgrade against any real cluster in the future.

Part of Week 1 - Azure DevOps, Helm & GitOps Foundations
