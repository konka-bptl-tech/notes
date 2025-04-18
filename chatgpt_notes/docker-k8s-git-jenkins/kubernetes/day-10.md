# HELM
❓ Problem Statement:

In Kubernetes, managing deployments for multiple environments like dev, qa, stage, and prod usually means having separate YAML files for each environment.  
This leads to:

🔁 Duplication of code — same deployment logic repeated with minor changes (image tags, replicas, env vars)

⚠️ Error-prone maintenance — changes need to be updated in multiple places

📉 Lack of scalability — becomes hard to manage as environments and microservices grow

---
✅ How Helm Solves This:

Helm is a package manager for Kubernetes that introduces a powerful templating system. It allows you to:

📄 Templatize common resources like Deployment, Service, Ingress 

📁 Maintain a single set of templates for all environments

⚙️ Pass environment-specific values using separate files like `values-dev.yaml`, `values-qa.yaml`, etc.

For example:
```bash
helm upgrade --install my-app ./charts/my-app -f values-dev.yaml
```
With Helm, you:
🧹 Eliminate YAML duplication  
🔄 Improve reusability and maintainability  
🚀 Simplify promotions between environments  
📦 Make your deployment strategy clean, DRY, and scalable
---
Helm is a **package manager for Kubernetes** that helps templatize, deploy, and manage applications efficiently.
#### **Why Use Helm?**
✅ **Templatized Manifests** – No need to write YAML files repeatedly
✅ **Reusable & Shareable** – Version-controlled and easy to modify
✅ **Customizable** – Use `values.yaml` for configuration flexibility
✅ **Simplifies Deployment** – Automates Kubernetes manifest creation
✅ **Easy Upgrades & Rollbacks** – Seamless version management

Just **pass required values** and deploy your application effortlessly! 🎯
### Commands
helm install <release-name> <chart-path> -f values.yaml    # Install a Helm chart with custom values
helm upgrade <release-name> <chart-path> -f values.yaml    # Upgrade an existing release
helm upgrade --install <release-name> <chart-path> -f values.yaml    # Install if not present, otherwise upgrade

helm list    # List all Helm releases
helm status <release-name>    # Show the status of a specific release
helm get all <release-name>    # Get all information about a release
helm template <chart-path> -f values.yaml    # Render templates locally for debugging

helm uninstall <release-name>    # Uninstall/delete a release

helm repo add <repo-name> <repo-url>    # Add a chart repository
helm repo update    # Update chart repository cache
helm search repo <chart-name>    # Search for a chart in repositories

helm install <release-name> <chart-path> -f values.yaml --dry-run    # Simulate an install without applying to the cluster
helm upgrade <release-name> <chart-path> -f values.yaml --dry-run    # Simulate an upgrade without applying changes
helm template <chart-path> -f values.yaml    # Render chart templates locally and output Kubernetes YAML

