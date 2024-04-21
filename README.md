# Extending kubectl

1. Install and configure kubectl plugin manager Krew:
```sh
(
  set -x; cd "$(mktemp -d)" &&
  OS="$(uname | tr '[:upper:]' '[:lower:]')" &&
  ARCH="$(uname -m | sed -e 's/x86_64/amd64/' -e 's/\(arm\)\(64\)\?.*/\1\2/' -e 's/aarch64$/arm64/')" &&
  KREW="krew-${OS}_${ARCH}" &&
  curl -fsSLO "https://github.com/kubernetes-sigs/krew/releases/latest/download/${KREW}.tar.gz" &&
  tar zxvf "${KREW}.tar.gz" &&
  ./"${KREW}" install krew
)

$ nano ~/.zshrc

# Add this row: export PATH="${KREW_ROOT:-$HOME/.krew}/bin:$PATH"

$ source ~/.zshrc

$ k krew      
krew is the kubectl plugin manager.
You can invoke krew through kubectl: "kubectl krew [command]..."

Usage:
  kubectl krew [command]

Available Commands:
  help        Help about any command
  index       Manage custom plugin indexes
  info        Show information about an available plugin
  install     Install kubectl plugins
  list        List installed kubectl plugins
  search      Discover kubectl plugins
  uninstall   Uninstall plugins
  update      Update the local copy of the plugin index
  upgrade     Upgrade installed plugins to newer versions
  version     Show krew version and diagnostics

Flags:
  -h, --help      help for krew
  -v, --v Level   number for the log level verbosity

Use "kubectl krew [command] --help" for more information about a command.

```

2. Check the list of existing plugins

```sh
$ kubectl plugin list

/root/.krew/bin/kubectl-krew
/root/.krew/bin/kubectl-ns
/usr/local/bin/kubectl-kubeplugin

```

3. Plugin usage:

```sh
$ k kubeplugin pod kube-system
pod, kube-system, coredns-77ccd57875-k2grb, 2m, 32Mi
pod, kube-system, local-path-provisioner-957fdf8bc-q5cgd, 1m, 21Mi
pod, kube-system, metrics-server-648b5df564-6fk8k, 4m, 43Mi
pod, kube-system, svclb-traefik-e2f577a8-cw9nb, 0m, 1Mi
pod, kube-system, traefik-64f55bb67d-fk484, 1m, 51Mi

$ k kubeplugin node kube-system
node, kube-system, k3d-argo-server-0, 98m, 1%
```