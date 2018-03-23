---
title: Wdrażanie maszyny wirtualnej platformy Azure z poziomu kodu Go
description: Dowiedz się, jak wdrożyć maszynę wirtualną za pomocą zestawu Azure SDK dla języka Go.
author: sptramer
ms.author: sttramer
ms.date: 02/08/2018
ms.topic: quickstart
ms.devlang: go
manager: carmonm
ms.openlocfilehash: 46a1243ff2ff6bfcf3831e2cea3137c1f6051c78
ms.sourcegitcommit: fcc1786d59d2e32c97a9a8e0748e06f564a961bd
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/23/2018
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a>Szybki start: wdrażanie maszyny wirtualnej platformy Azure za pomocą szablonu i zestawu Azure SDK dla języka Go

Ten przewodnik Szybki start prezentuje sposób wdrożenia zasobów z szablonu za pomocą zestawu Azure SDK dla języka Go. Szablony to migawki poszczególnych zasobów zawarte w [grupie zasobów platformy Azure](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview). W ramach tej procedury, podczas wykonywania przydatnego zadania, poznasz funkcje i konwencje zestawu SDK.

Na końcu tego przewodnika znajdują się dane działającej maszyny wirtualnej, na której możesz się zalogować się przy użyciu nazwy użytkownika i hasła.

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

W przypadku instalacji lokalnej interfejsu wiersza polecenia platformy Azure ten przewodnik wymaga interfejsu wiersza polecenia w wersji 2.0.24 lub nowszej. Uruchom polecenie `az --version`, aby upewnić się, że dany interfejs wiersza polecenia spełnia ten wymóg. Jeśli konieczna będzie instalacja lub uaktualnienie interfejsu, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0](/cli/azure/install-azure-cli).

## <a name="install-the-azure-sdk-for-go"></a>Instalowanie zestawu Azure SDK dla języka Go 

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a>Tworzenie nazwy głównej usługi

Aby można było zalogować się w aplikacji w sposób nieinterakcyjny, potrzebna jest jednostka usługi. Jednostki usługi stanowią część kontroli dostępu na podstawie ról (RBAC) tworzącej unikatową tożsamość użytkownika. Aby utworzyć nową jednostkę usługi za pomocą interfejsu wiersza polecenia, uruchom następujące polecenie:

```azurecli-interactive
az ad sp create-for-rbac --name az-go-vm-quickstart
```

__Koniecznie__ zapisz wartości `appId`, `password` i `tenant` w danych wyjściowych. Za pomocą tych wartości aplikacja przeprowadza uwierzytelnianie na platformie Azure.

Aby uzyskać więcej informacji na temat tworzenia jednostek usługi i zarządzania nimi za pomocą interfejsu wiersza polecenia platformy Azure 2.0, zobacz [Tworzenie jednostki usługi platformy Azure za pomocą interfejsu wiersza polecenia platformy Azure 2.0](/cli/azure/create-an-azure-service-principal-azure-cli).

## <a name="get-the-code"></a>Uzyskiwanie kodu

Kod szybkiego startu oraz wszystkie jego zależności można pobrać, korzystając z polecenia `go get`.

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm/...
```

Ten kod jest kompilowany, ale nie działa poprawnie, dopóki nie podasz informacji dotyczących swojego konta platformy Azure i nie utworzysz jednostki usługi. W poleceniu `main.go` występuje zmienna `config`, która zawiera strukturę `authInfo`. Aby zapewnić poprawne uwierzytelnianie, w strukturze tej trzeba zastąpić wartości pól.

```go
    config = authInfo{ // Your application credentials
        TenantID:               "", // Azure account tenantID
        SubscriptionID:         "", // Azure subscription subscriptionID
        ServicePrincipalID:     "", // Service principal appId
        ServicePrincipalSecret: "", // Service principal password/secret
    }
```

* `SubscriptionID`: Twój identyfikator subskrypcji, który można uzyskać za pomocą polecenia interfejsu CLI

  ```azurecli-interactive
  az account show --query id -o tsv
  ```

* `TenantID`: Twój identyfikator dzierżawy, wartość `tenant` zarejestrowana podczas tworzenia jednostki usługi
* `ServicePrincipalID`: wartość `appId` zarejestrowana podczas tworzenia jednostki usługi
* `ServicePrincipalSecret`: wartość `password` zarejestrowana podczas tworzenia jednostki usługi

Należy również poddać edycji wartość w pliku `vm-quickstart-params.json`.

```json
    "vm_password": {
        "value": "_"
    }
```

* `vm_password`: hasło konta użytkownika na maszynie wirtualnej. Hasło musi zawierać od 12 do 72 znaków i 3 spośród następujących typów znaków:
  * Mała litera
  * Wielka litera
  * Cyfra
  * Symbol

## <a name="running-the-code"></a>Wykonywanie kodu

Uruchom szybki start za pomocą polecenia `go run`.

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm
go run main.go
```

Jeśli we wdrożeniu jest błąd, zostanie wyświetlony komunikat, który informuje, że wystąpił problem, ale nie zawiera szczegółowych informacji. Korzystając z interfejsu wiersza polecenia platformy Azure, pobierz szczegóły dotyczące błędu wdrożenia przy użyciu następującego polecenia:

```azurecli-interactive
az group deployment show -g GoVMQuickstart -n VMDeployQuickstart
```

Jeśli wdrażanie zakończy się pomyślnie, zostanie wyświetlony komunikat zawierający nazwę użytkownika, adres IP i hasło umożliwiające zalogowanie się na nowo utworzonej maszynie wirtualnej. Logując się za pomocą protokołu SSH, sprawdź, czy maszyna wirtualna faktycznie jest uruchomiona i gotowa.

## <a name="cleaning-up"></a>Czyszczenie

Wyczyść zasoby utworzone w ramach tego szybkiego startu, usuwając grupę zasobów przy użyciu interfejsu wiersza polecenia.

```azurecli-interactive
az group delete -n GoVMQuickstart
```

## <a name="code-in-depth"></a>Szczegóły kodu

Działanie kodu szybkiego startu jest podzielone na bloki zmiennych i kilka małych funkcji. Opisano je poniżej.

### <a name="variable-assignments-and-structs"></a>Struktury i przypisania zmiennych

Ponieważ szybki start jest niezależny, używa zmiennych globalnych, a nie opcji wiersza polecenia czy też zmiennych środowiskowych.

```go
type authInfo struct {
        TenantID               string
        SubscriptionID         string
        ServicePrincipalID     string
        ServicePrincipalSecret string
}
```

Struktura `authInfo` jest deklarowana w celu hermetyzowania wszystkich informacji potrzebnych do autoryzowania w usługach platformy Azure.

```go
const (
    resourceGroupName     = "GoVMQuickstart"
    resourceGroupLocation = "eastus"

    deploymentName = "VMDeployQuickstart"
    templateFile   = "vm-quickstart-template.json"
    parametersFile = "vm-quickstart-params.json"
)

var (
    config = authInfo{ // Your application credentials
        TenantID:               "", // Azure account tenantID
        SubscriptionID:         "", // Azure subscription subscriptionID
        ServicePrincipalID:     "", // Service principal appId
        ServicePrincipalSecret: "", // Service principal password/secret
    }

    ctx = context.Background()

    token *adal.ServicePrincipalToken
)
```

Deklarowane wartości zapewniają nazwy utworzonych zasobów. Podano tutaj także lokalizację. Można ją zmienić, aby sprawdzić zachowanie wdrożeń w innych centrach danych. Nie każde centrum danych ma dostępne wszystkie wymagane zasoby.

Stałe `templateFile` i `parametersFile` wskazują pliki potrzebne do wdrożenia. Token jednostki usługi jest podawany później, a zmienna `ctx` stanowi [kontekst języka Go](https://blog.golang.org/context) dla operacji sieciowych.

### <a name="init-and-authorization"></a>Metoda init() i autoryzacja

Metoda `init()` umożliwia skonfigurowanie autoryzacji w kodzie. Autoryzacja jest warunkiem wstępnym w każdym procesie szybkiego startu, a zatem warto uczynić ją częścią inicjowania. 

```go
// Authenticate with the Azure services over OAuth, using a service principal.
func init() {
    oauthConfig, err := adal.NewOAuthConfig(azure.PublicCloud.ActiveDirectoryEndpoint, config.TenantID)
    if err != nil {
        log.Fatalf("Failed to get OAuth config: %v\n", err)
    }
    token, err = adal.NewServicePrincipalToken(
        *oauthConfig,
        config.ServicePrincipalID,
        config.ServicePrincipalSecret,
        azure.PublicCloud.ResourceManagerEndpoint)
    if err != nil {
        log.Fatalf("faled to get token: %v\n", err)
    }
}
```

Ten kod obejmuje dwa etapy autoryzacji:

* Pobranie informacji o konfiguracji uwierzytelniania OAuth dla elementu `TenantID` dzięki komunikacji z usługą Azure Active Directory. Obiekt [`azure.PublicCloud`](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#PublicCloud) zawiera punkty końcowe używane w standardowej konfiguracji platformy Azure.
* Wywołanie funkcji [`adal.NewServicePrincipalToken()`](https://godoc.org/github.com/Azure/go-autorest/autorest/adal#NewServicePrincipalToken). Ta funkcja przyjmuje informacje dotyczące uwierzytelniania OAuth wraz z danymi logowania jednostki usługi oraz używanego stylu zarządzania platformą Azure. Jeśli nie istnieją specyficzne wymagania i dobrze wiesz, co robisz, ta wartość powinna mieć zawsze postać `.ResourceManagerEndpoint`.

### <a name="flow-of-operations-in-main"></a>Przepływ operacji w funkcji main()

Funkcja `main()` jest prosta — wskazuje tylko przepływ operacji i przeprowadza sprawdzanie błędów.

```go
func main() {
    group, err := createGroup()
    if err != nil {
        log.Fatalf("failed to create group: %v", err)
    }
    log.Printf("created group: %v\n", *group.Name)

    log.Println("starting deployment")
    result, err := createDeployment()
    if err != nil {
        log.Fatalf("Failed to deploy correctly: %v", err)
    }
    log.Printf("Completed deployment: %v", *result.Name)
    getLogin()
}
```

Oto działania wykonywane przez kod (z zachowaniem kolejności):

* Utworzenie grupy zasobów, w której ma zostać przeprowadzone wdrażanie (`createGroup()`)
* Utworzenie wdrożenia w tej grupie (`createDeployment()`)
* Uzyskanie i wyświetlenie danych logowania dotyczących wdrożonej maszyny wirtualnej (`getLogin()`)

### <a name="creating-the-resource-group"></a>Tworzenie grupy zasobów

Funkcja `createGroup()` umożliwia utworzenie grupy zasobów. Przepływ wywołań i argumenty pozwalają zapoznać się ze strukturą interakcji z usługami w zestawie SDK.

```go
func createGroup() (group resources.Group, err error) {
        groupsClient := resources.NewGroupsClient(config.SubscriptionID)
        groupsClient.Authorizer = autorest.NewBearerAuthorizer(token)

        return groupsClient.CreateOrUpdate(
                ctx,
                resourceGroupName,
                resources.Group{
                        Location: to.StringPtr(resourceGroupLocation)})
}
```

Ogólny przebieg interakcji z usługą platformy Azure wygląda następująco:

* Utworzenie klienta przy użyciu metody `service.NewXClient()`, gdzie `X` to typ zasobu `service`, z którym ma nastąpić interakcja. Ta funkcja zawsze pobiera identyfikator subskrypcji.
* Ustawienie metody autoryzacji klienta z uwzględnieniem interakcji ze zdalnym interfejsem API.
* Wywołanie metody na kliencie odpowiadającym zdalnemu interfejsowi API. Metody klienta usługi zazwyczaj pobierają nazwę zasobu i obiektu metadanych.

Funkcja [`to.StringPtr()`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) służy tutaj do przeprowadzania konwersji typów. Struktury parametrów dla metod zestawu SDK przyjmują niemal wyłącznie wskaźniki. Metody te mają zatem za zadanie ułatwienie konwersji typów. Pełną listę i zachowanie konwerterów można znaleźć w dokumentacji modułu [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to).

Operacja `groupsClient.CreateOrUpdate()` zwraca wskaźnik do struktury danych reprezentującej grupę zasobów. Bezpośrednia zwracana wartość tego rodzaju wskazuje krótkotrwałą operację, która ma być synchroniczna. W następnej sekcji przedstawimy przykład długotrwałej operacji i sposób odpowiedniej interakcji.

### <a name="performing-the-deployment"></a>Przeprowadzanie wdrażania

Po utworzeniu grupy przeznaczonej na zasoby można już uruchomić wdrożenie. Ten kod został podzielony na mniejsze części w celu wyróżnienia różnych części logiki kodu.


```go
func createDeployment() (deployment resources.DeploymentExtended, err error) {
    template, err := readJSON(templateFile)
    if err != nil {
        return
    }
    params, err := readJSON(parametersFile)
    if err != nil {
        return
    }

        // ...
```

Pliki wdrożenia są ładowane przez element `readJSON` (szczegóły zostały tutaj pominięte). Ta funkcja zwraca typ `*map[string]interface{}` używany podczas tworzenia metadanych dla wywołania wdrażania zasobów.

```go
        // ...
        
        deploymentsClient := resources.NewDeploymentsClient(config.SubscriptionID)
        deploymentsClient.Authorizer = autorest.NewBearerAuthorizer(token)

        deploymentFuture, err := deploymentsClient.CreateOrUpdate(
                ctx,
                resourceGroupName,
                deploymentName,
                resources.Deployment{
                        Properties: &resources.DeploymentProperties{
                                Template:   template,
                                Parameters: params,
                                Mode:       resources.Incremental,
                        },
                },
        )
        if err != nil {
                log.Fatalf("Failed to create deployment: %v", err)
        }
        //...
```

Ten kod jest zgodny z tym samym wzorcem, który jest używany podczas tworzenia grupy zasobów. Zostaje utworzony nowy klient (pod warunkiem, że uwierzytelnienie w usłudze Azure jest możliwe), a następnie zostaje wywołana metoda. Metoda ma nawet taką samą nazwę (`CreateOrUpdate`) co analogiczna metoda dotycząca grupy zasobów. Ten wzorzec jest używany w zestawie SDK wielokrotnie. Metody, które mają podobne działanie, mają zwykle taką samą nazwę.

Największa różnica dotyczy wartości zwracanej przez metodę `deploymentsClient.CreateOrUpdate()`. Ta wartość to obiekt `Future` zgodny z [wzorcem projektowym obiektu future](https://en.wikipedia.org/wiki/Futures_and_promises). W kontekście platformy Azure obiekty future reprezentują długotrwałą operację, którą można sporadycznie sondować podczas wykonywania innych zadań.

```go
        //...
        err = deploymentFuture.Future.WaitForCompletion(ctx, deploymentsClient.BaseClient.Client)
        if err != nil {
                log.Fatalf("Error while waiting for deployment creation: %v", err)
        }
        return deploymentFuture.Result(deploymentsClient)
}
```

W przypadku tego przykładu najlepiej zaczekać na ukończenie operacji. Oczekiwanie na obiekt future wymaga zarówno [obiektu kontekstu](https://blog.golang.org/context), jak i klienta, który utworzył obiekt future. Istnieją tutaj dwa możliwe źródła błędu: błąd występujący po stronie klienta podczas próby wywołania metody oraz odpowiedź serwera z informacją o błędzie. Ta druga jest zwracana jako część wywołania `deploymentFuture.Result()`.

### <a name="obtaining-the-assigned-ip-address"></a>Uzyskiwanie przypisanego adresu IP

Aby można było korzystać z nowo utworzonej maszyny wirtualnej, trzeba mieć przypisany adres IP. Adresy IP stanowią oddzielne zasoby platformy Azure powiązane z zasobami kontrolera interfejsu sieciowego.

```go
func getLogin() {
        params, err := readJSON(parametersFile)
        if err != nil {
                log.Fatalf("Unable to read parameters. Get login information with `az network public-ip list -g %s", resourceGroupName)
        }

        addressClient := network.NewPublicIPAddressesClient(config.SubscriptionID)
        addressClient.Authorizer = autorest.NewBearerAuthorizer(token)
        ipName := (*params)["publicIPAddresses_QuickstartVM_ip_name"].(map[string]interface{})
        ipAddress, err := addressClient.Get(ctx, resourceGroupName, ipName["value"].(string), "")
        if err != nil {
                log.Fatalf("Unable to get IP information. Try using `az network public-ip list -g %s", resourceGroupName)
        }

        vmUser := (*params)["vm_user"].(map[string]interface{})
        vmPass := (*params)["vm_password"].(map[string]interface{})

        log.Printf("Log in with ssh: %s@%s, password: %s",
                vmUser["value"].(string),
                *ipAddress.PublicIPAddressPropertiesFormat.IPAddress,
                vmPass["value"].(string))
}
```

Ta metoda korzysta z informacji przechowywanych w pliku parametrów. Kod może wysłać zapytanie bezpośrednio do maszyny wirtualnej, aby uzyskać dane jej kontrolera interfejsu sieciowego, wysłać zapytanie do kontrolera interfejsu sieciowego, aby uzyskać jego adres IP, a następnie wysłać zapytanie bezpośrednio do zasobu z danym adresem IP. Jest to długi łańcuch zależności. Wymaga on wykonania wielu operacji, przez co jest kosztowny. Ponieważ dane JSON są lokalne, można je załadować zamiast wykonywania powyższych operacji.

Wartości dotyczące użytkownika maszyny wirtualnej i jego hasła również są ładowane z danych JSON.

## <a name="next-steps"></a>Następne kroki

W tym przewodniku Szybki start istniejący szablon został wdrożony za pomocą języka Go. Następnie nastąpiło nawiązanie połączenia z nowo utworzoną maszyną wirtualną za pośrednictwem protokołu SSH. To pozwoliło upewnić się, że maszyna jest uruchomiona.

Aby uzyskać więcej informacji o pracy z maszynami wirtualnymi w środowisku platformy Azure z użyciem języka Go, zapoznaj się z [przykładami dotyczącymi obliczeń na platformie Azure dla języka Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) lub [przykładami dotyczącymi zarządzania zasobami na platformie Azure dla języka Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).
