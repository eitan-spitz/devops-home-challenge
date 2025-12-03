Wednesday — Discover, local run, Docker + Helm start

    Goal: Fork, run app, validate Dockerfile, scaffold Helm chart, choose cluster & git workflow.
    Estimate: 6.0–7.5 hours

    Fork repo, clone locally, run app & tests — 45 min

    Inspect Dockerfile, build image locally & run container — 30 min

    Run Trivy locally against image to catch obvious vulns — 20 min

    Decide Git workflow & create gitops-repo skeleton / add CONTRIBUTING.md — 30 min

    Scaffold Helm chart helm create myapp and map image values — 45 min

    Add initial liveness/readiness probes into chart + resources placeholders — 60 min

    Helm dry-run and fix templating issues (helm template / dry-run) — 60 min

    Choose cluster approach (use kind/minikube or cloud) and create cluster or verify access — 30–60 min

    Quick notes and commit everything to feature branch; open PR template. — 20 min

    Buffer / wrap-up: commit pushed, write short plan for Thu — 10–20 min

Thursday — CI skeleton, SAST, build/push image, run on cluster

    Goal: Implement GitHub Actions CI: tests, Semgrep SAST, build & push, Trivy scan. Deploy Helm to cluster for validation.
    Estimate: 7.0–8.5 hours

    Add GH Actions ci.yml with steps: checkout, run tests, lint — 45 min

    Add Semgrep job configuration and action to fail on critical findings — 45 min

    Add Build-and-push job, configure Docker Hub secrets in GH repo — 60–90 min

    Add image scanning job (Trivy) to run after push and fail on high severity — 45–60 min

    Run pipeline, iterate on failures and fix Dockerfile/build issues — 60–90 min

    From pipeline or locally, deploy Helm chart to chosen cluster and verify app runs (Ingress/test). — 45–60 min

    Capture screenshots for verification, update README with CI diagram & secrets setup. — 30 min

    Wrap & commit — 10–15 min

Friday — GitOps & ArgoCD, automate GitOps update in pipeline

    Goal: Set up ArgoCD, GitOps repo, and add pipeline step to update gitops values (image tag). Verify auto-sync.
    Estimate: 6.0–8.0 hours

    Create gitops-repo with values-prod.yaml and ArgoCD app manifest skeleton — 30–45 min

    Install ArgoCD in cluster and expose UI (port-forward or Ingress) — 30–60 min

    Create ArgoCD Application pointing at gitops-repo (path to values/helm chart) — 30–45 min

    Add pipeline job that, on successful build+scan, commits updated image tag to gitops-repo (use bot user token). Test commit. — 60–90 min

    Verify ArgoCD picks up change and syncs; fix any chart/value mismatches. Capture ArgoCD UI screenshot. — 45–60 min

    Add protection: pipeline does not push gitops change if Trivy finds high vulns or Semgrep found critical. — 20–30 min

    Wrap — update docs and commit — 15 min

Sunday — Verification, docs, polish, optional bonuses

    Goal: Final verification, docs, finalize deliverables, do one or two bonuses if time.
    Estimate: 4.5–6.0 hours

    Full run-through: merge PR to main, watch pipeline, image push, gitops commit, ArgoCD sync, app is live — 60–90 min

    Collect artifacts and screenshots: CI logs showing SAST/Trivy steps, ArgoCD app, cluster kubectl outputs, browser screenshot of Ingress route. — 30–45 min

    Finish README, include links to repos, explain choices (Deployment, separate gitops repo, Semgrep, Trivy). — 60–90 min

    Optional: add version bump automation in GH Actions or create a release. — 30–60 min

    Final checklist, compress artifacts, prepare submission message with links. — 15–30 min