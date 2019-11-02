---

## Rola Orkiestratora

Zarządzanie kontenerami w ramach klastra, dbanie o zasoby w klastrze, deployment, skalowanie, monitoring czy dostęp.

## Jakie problemy adresuje K8s

- Service discovery i load balancing
- Skalowanie na różnych poziomach
- Self healing
- Rollout i rollback - możliwość deploymentu aplikacji i wycofania deploymentu aplikacji jeżeli ten się nie powiódł. Możliwość trzymaniu kilku różnych wersji aplikacji na tym samym klastrze.
- Zarządzanie konfiguracją
- Storage orchestration

## Jakie problemy adresuje K8s w wersji zarządzanej w środowisku cloud?

- Niemalże automatyczne patchowane i upgradowane - musimy tylko określić kiedy to upgrade ma być zastosowany
- Nie zarządzamy klastrem master bo on jest zarządzany przez vendora
- Bardzo łatwo monitorować
-

---

### Master

- `Kube-apiserver`

- `etcd` - key/value baza danych w której utrzymywany jest stan oraz historia zdarzeń w ramach klastra.

- `Kube-scheduler`

- `Kube-controller-manager`

- `Cloud-controller-manager`

### Node

- `kubelet` - odpowiada za uruchomienie podów, zarządza kontenerami na node
- `kube-proxy` - odpowiada za aspekty sieciowe

--

## CLI

`kubectl get pods` - pobierz pody działające w ramach mojego klastra

`kubectl get pods --all-namespaces` - pobierz pody działające w ramach mojego klastra ze wszystkich namespaces

`kubectl get nodes` - informacje o naszym klastrze
`kubectl get nodes -o wide`

`kubectl cluster-info` - pobieramy informacje o tym jak nasz klaster jest dostępny np. adres naszego klastra master

`kubectl describe node/<nazwa node>` -

`kubectl api-versions`

`kubectl config view` - może pokazać m.in informację do jakich klastrów mam dostęp

`kubectl config set-context aks02` - pozwala przełączać się miedzy różnymi klastrami

---

## Kubeconfig

Lokalny plik używany przez `kubectl` który zawiera wszystkie informacje niezbędne do połączenia z moim klastrem.

Domyślna lokalizacja: `$HOME/.kube/config`

--

## Configure kubectl to access Remote Kubernetes Cluster

`kubectl config set-credentials k8s_user/myk8s.kubernetes.com --username=k8s_user`

`kubectl config set-cluster myk8s.kubernetes.com --insecure-skip-tls-verify=true --server=https://myk8s.kubernetes.com`

`kubectl config set-context default/myk8s.kubernetes.com/k8s_user --user=kubeuser/myk8s.kubernetes.com --namespace=default --cluster=myk8s.kubernetes.com`

`kubectl config use-context default/myk8s.kubernetes.com/k8s_user`

--

## Verbose

W `kubectl` mamy flagę `-v[0-9]` prezentującą szczegółowe informacje nt wykonywanej komendy. Przydatne przy debugowaniu.

--

## Przydatne komendy

`kubectl cluster-info` - sprawdź czy kubectl jest poprawnie skonfigurowany
`kubectl version --short` - pokaż wersje klastra z którym jestem połączony i wersję klienta
`kubectl config get-contexts` - pokaż wszystkie konteksty w pliku kubeconfig
`kubectl edit` - edycja obiektów kubernetesa na żywo

--

# Tworzenie i konfiguracja klastra AKS

## Azure CLI

`az login` - otworzy przeglądarkę i pozwoli zalogować się do swojego konta

`az aks ...` - tak zaczynają się komendy dla AKS

`az aks install-cli` - opcjonalnie pozwala nam zainstalować `kubectl` jeżeli nie mamy go lokalnie

`az account list` - wylistuje listę kont które posiadam

`az account set -s <subscriptionID>` - ustawia aktywną subskrypcję do pracy w ramach Azure CLI

https://docs.microsoft.com/en-us/cli/azure/aks?view=azure-cli-latest

`az aks browse` - utworzenia połączenie do dashbordu Kubernetesa

`az aks create` - tworzymy klaster Kubernetes

`az aks delete` - tworzymy klaster Kubernetes

`az aks get-credentials` - pozwala zaktualizować kubeconfig i w łatwy sposób dostać się do swojego klastra

`az aks scale` - skalowanie na poziomie nodów

## Ćwiczenie - Tworzymy klaster

1. Tworzymy `resource group`

`az group create --name CloudStateRG --location westeurope`

2. Tworzymy klaster Kubernetesa w stworzonej w punkcie 1 `resource group`

`--node-count -c` - liczba nodów która zostanie powołana do życia. liczba nodów w puli nodów. Po utworzeniu klastra można zmienić rozmiar puli za pomocą `az aks scale`. Domyślna wartość `3`.

`--enable-addons -a` - lista dodatków wylistowanych po przecinku do włączenia

`--generate-ssh-keys` - wygeneruj klucz publiczny i prywatny jeżeli nie istnieją. Klucze przechowywane są w `~/.ssh`

`az aks create --resource-group CloudStateRG --name CloudStateAKSCluster --node-count 3 --enable-addons monitoring --generate-ssh-keys`

`az aks create -g CloudStateRG -n CloudStateAKSCluster -c 3 --enable-addons monitoring`

> Po stworzeniu klastra musimy ręcznie ustawić kontekst dla naszego klastra w kubectl, pomoże nam w tym az aks get-credentials

`az aks get-credentials -g CloudStateRG -n CloudStateAKSCluster`

Polecenie `az aks get-credentials` ustawia w kubeconfig `current-context` na klaster AKS.

Ograniczamy liczbę nodów z 3 na 1

`az aks scale -g CloudStateRG -n CloudStateAKSCluster -c 1`

Usuwamy klaster

`az aks delete -g CloudStateRG -n CloudStateAKSCluster`

> Po usunięcia klastra muszę sam odświeżyć kubeconfig, bo ciągle wskazuje na klaster AKS

`kubectl config delete-context CloudStateAKSCluster`

I ustawić inny klaster

`kubectl config use-context docker-desktop`
