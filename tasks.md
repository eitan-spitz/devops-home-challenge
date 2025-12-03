1. Fork, run & understand (dependency: none)

    Fork the sample app repo to your GitHub account (create repo your-org/app-name). — 5–10 min

    Clone locally, run the app per upstream README. Confirm it works end-to-end (curl / browser). — 15–45 min

    Run unit tests (if present). Note failing tests / test gaps. — 15–45 min

    Read Dockerfile and existing packaging. Note missing things. — 10–20 min

2. Choose Git workflow & repo layout (dependency: fork)

    Decide git workflow: GitHub Flow (main branch protected + feature PRs + PR→merge→deploy) — chosen here. Document it in CONTRIBUTING.md. — 15–30 min

    Create two repos (if not already):

    app-repo (forked app, source, Dockerfile, Helm chart inside /helm/<chart> or separate repo)

    gitops-repo (separate repo to hold Helm values / kustomize / argocd app manifests). — 15–30 min

3. Local Docker build & quick security run (dependency: Dockerfile)

    Build image locally: docker build -t yourname/app:dev . — 10–15 min

    Run container and smoke test. — 10–20 min

    Run Trivy locally against built image to surface immediate vulnerabilities. Record results. — 10–20 min

4. Helm chart authoring (dependency: app runs locally)

    Create Helm chart scaffold: helm create myapp under /helm/myapp or new chart repo. — 10–15 min

    Replace scaffold container image with your image placeholders and values. Add values.yaml with image repo/name/tag. — 10–20 min

    Add readinessProbe and livenessProbe to the Deployment templates (HTTP or TCP tests) — 15–30 min

    Add resources.requests and resources.limits in values (reasonable defaults: CPU 100m/250m, memory 128Mi/256Mi) — 10–15 min

    Add Service (ClusterIP) and Ingress config in templates with host/value placeholders. — 15–30 min

    Add ConfigMap and Secret templates (use placeholders, optional sealed-secrets later). — 15–25 min

    Parameterize probes, resources, replica count in values.yaml. — 10–15 min

    Test install locally on minikube/kind: helm install --dry-run --debug then helm template → inspect manifests. — 15–30 min

5. Kubernetes cluster choice & quick deploy (dependency: helm chart)

    Decide cluster: use existing cloud k8s / or create dev cluster (kind/minikube) for validation. If using cloud, ensure kubeconfig is set. — 15–30 min

    Deploy Helm chart to cluster for functional validate: helm upgrade --install myapp ./helm/myapp -f helm/myapp/values.dev.yaml — 20–40 min

    Verify pods, logs, readiness/liveness, service, Ingress. Capture screenshots or curl the ingress host. — 30–45 min

6. CI: GitHub Actions - skeleton & tests (dependency: code in repo)

    Add workflow file .github/workflows/ci.yml with steps:

    checkout

    run unit tests

    run Semgrep SAST (fail on critical)

    build-stage (optional cache)

    Do not push image yet. — 30–60 min

    Add Semgrep config rules or use default policies; treat error/critical as failure. — 15–30 min

    Ensure test and lint steps pass locally/CI. — 15–45 min

7. CI: Build & push container (dependency: successful CI)

    Add build/push step in workflow using docker/build-push-action or kaniko if using remote runner. Use GitHub secrets for DOCKERHUB_USERNAME and DOCKERHUB_TOKEN. — 20–40 min

    Tagging/version-bumping: add action to bump version/tag on merge to main (semantic versioning automated or patch bump), e.g., actions/create-release or peter-evans/semver. Choose: automatic semver patch on merge. — 20–40 min

    Push image to Docker Hub: yourname/app:${{ steps.tag.outputs.version }}. — 10–20 min

8. CI/CD: Image vulnerability scan & block deploy (dependency: pushed image)

    After push, run Trivy against the pushed image in a workflow job. Fail pipeline if high severity vulnerabilities exist. — 20–40 min

    Add caching for Trivy DB to speed runs. — 10–20 min

9. CD automation + ArgoCD (dependency: push/pipeline)

    Decide GitOps pattern: ArgoCD watches a separate gitops-repo that contains HelmRelease or Helm values. (Reason: separation of application code from environment deployment manifests; safer/reviewable). — 10–15 min

    In CI pipeline on success, update gitops-repo (commit & push updated values.yaml containing the new image tag) — use a bot account/token (GH actor). This is the "push to gitops" step. — 30–60 min

    Setup ArgoCD in cluster (if not already): install ArgoCD (Helm or manifest), expose UI (port-forward or ingress). — 20–40 min

    Create ArgoCD Application that points at gitops-repo path and the Helm chart (or chart repo + values). Confirm ArgoCD sync succeeds. — 20–40 min

    (Alternative) If you choose ArgoCD to pull from the app repo directly, document tradeoffs. (We chose separate repo.) — 10 min

10. End-to-end verification & screenshots (dependency: deployment)

    Access the service via Ingress domain or port-forward and capture screenshot showing app running. — 15–30 min

    Export Kubernetes resource statuses (kubectl get pods,svc,ing -n <ns> -o wide) and screenshot or copy outputs. — 15–20 min

11. Documentation & deliverables (dependency: everything)

    Write README describing how to run locally, CI flow, GitOps flow, how to reproduce scans, and how to access cluster/ArgoCD. Include links to repos and screenshots. — 45–90 min

    Add CONTRIBUTING.md describing Git workflow and version bump rules. — 15–30 min

    Add SECURITY.md describing SAST/Trivy config and how to run scans locally. — 20–30 min

12. Optional bonuses (time-permitting)

    Add SealedSecrets or HashiCorp Vault integration for real secret management. — 1–3 hours

    Add automated Canary/A/B rollout (Argo Rollouts). — 2–3 hours

    Add monitoring: Prometheus/Service monitor, or liveness logs. — 1–2 hours

    Add e2e tests that run in CI against deployed environment. — 2–4 hours