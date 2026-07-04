## Kubeconfig & Clusters

| Command | Description |
| :--- | :--- |
| `kubectl config get-contexts` | List contexts (cluster + user + namespace combos) |
| `kubectl config get-clusters` | List just the cluster names in kubeconfig |
| `kubectl config current-context` | Show the currently active context |
| `kubectl config use-context <context-name>` | Switch to another context |

> 📌 **Note:** `kubectl` only sees clusters in `~/.kube/config`. The AWS EKS console shows ALL clusters in the account.

---

## Adding an EKS Cluster to kubeconfig

| Command | Description |
| :--- | :--- |
| `aws eks list-clusters --region us-east-2` | List EKS clusters in the account |
| `aws eks update-kubeconfig --name udx-production --region us-east-2 --alias udx-production` | Add a cluster's credentials to kubeconfig (with a friendly alias) |

> 📌 **Note:** Having the context ≠ having access — your IAM identity must also be granted access inside the cluster (EKS access entry / `aws-auth`). Private API endpoints may require a VPN/Tailscale exit node.

---

## Verifying Access

| Command | Description |
| :--- | :--- |
| `kubectl get nodes` | List nodes to verify your cluster access is working |

---

## Pods & Namespaces

| Command | Description |
| :--- | :--- |
| `kubectl get namespaces` | List namespaces |
| `kubectl get pods` | Pods in current namespace |
| `kubectl get pods -n <namespace>` | Pods in a specific namespace |
| `kubectl get pods -A` | Pods across ALL namespaces |
| `kubectl get pods -o wide` | Adds node / IP info to pod list |
| `kubectl get pods -w` | Watch live pod status |
| `kubectl get pods -A \| grep <keyword>` | Filter long output across all namespaces |
| `kubectl describe pod <pod-name>` | Inspect a single pod |
| `kubectl logs <pod-name>` | View logs (add `-f` to follow) |
| `kubectl logs <pod-name> -n <namespace>` | Logs for a pod in a specific namespace |
| `kubectl logs -f <pod-name>` | Follow (stream) logs live, like `tail -f` |
| `kubectl logs <pod-name> --tail=100` | Only the last 100 lines of logs |
| `kubectl logs <pod-name> --since=1h` | Only logs from the last hour |
| `kubectl logs <pod-name> --previous` | Logs from the previous (crashed) container |
