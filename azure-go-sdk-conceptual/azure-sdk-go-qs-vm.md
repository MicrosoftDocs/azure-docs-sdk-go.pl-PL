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
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a><span data-ttu-id="08205-103">Szybki start: wdrażanie maszyny wirtualnej platformy Azure za pomocą szablonu i zestawu Azure SDK dla języka Go</span><span class="sxs-lookup"><span data-stu-id="08205-103">Quickstart: Deploy an Azure virtual machine from a template with the Azure SDK for Go</span></span>

<span data-ttu-id="08205-104">Ten przewodnik Szybki start prezentuje sposób wdrożenia zasobów z szablonu za pomocą zestawu Azure SDK dla języka Go.</span><span class="sxs-lookup"><span data-stu-id="08205-104">This quickstart focuses on deploying resources from a template with the Azure SDK for Go.</span></span> <span data-ttu-id="08205-105">Szablony to migawki poszczególnych zasobów zawarte w [grupie zasobów platformy Azure](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span><span class="sxs-lookup"><span data-stu-id="08205-105">Templates are snapshots of all of the resources contained within an [Azure resource group](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span></span> <span data-ttu-id="08205-106">W ramach tej procedury, podczas wykonywania przydatnego zadania, poznasz funkcje i konwencje zestawu SDK.</span><span class="sxs-lookup"><span data-stu-id="08205-106">Along the way, you'll become familiar with the functionality and conventions of the SDK while performing a useful task.</span></span>

<span data-ttu-id="08205-107">Na końcu tego przewodnika znajdują się dane działającej maszyny wirtualnej, na której możesz się zalogować się przy użyciu nazwy użytkownika i hasła.</span><span class="sxs-lookup"><span data-stu-id="08205-107">At the end of this quickstart, you have a running VM that you log into with a username and password.</span></span>

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

<span data-ttu-id="08205-108">W przypadku instalacji lokalnej interfejsu wiersza polecenia platformy Azure ten przewodnik wymaga interfejsu wiersza polecenia w wersji 2.0.24 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="08205-108">If you use a local install of the Azure CLI, this quickstart requires CLI version 2.0.24 or later.</span></span> <span data-ttu-id="08205-109">Uruchom polecenie `az --version`, aby upewnić się, że dany interfejs wiersza polecenia spełnia ten wymóg.</span><span class="sxs-lookup"><span data-stu-id="08205-109">Run `az --version` to make sure your CLI install meets this requirement.</span></span> <span data-ttu-id="08205-110">Jeśli konieczna będzie instalacja lub uaktualnienie interfejsu, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="08205-110">If you need to install or upgrade, see [Install the Azure CLI 2.0](/cli/azure/install-azure-cli).</span></span>

## <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="08205-111">Instalowanie zestawu Azure SDK dla języka Go</span><span class="sxs-lookup"><span data-stu-id="08205-111">Install the Azure SDK for Go</span></span> 

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a><span data-ttu-id="08205-112">Tworzenie nazwy głównej usługi</span><span class="sxs-lookup"><span data-stu-id="08205-112">Create a service principal</span></span>

<span data-ttu-id="08205-113">Aby można było zalogować się w aplikacji w sposób nieinterakcyjny, potrzebna jest jednostka usługi.</span><span class="sxs-lookup"><span data-stu-id="08205-113">To log in non-interactively with an application, you need a service principal.</span></span> <span data-ttu-id="08205-114">Jednostki usługi stanowią część kontroli dostępu na podstawie ról (RBAC) tworzącej unikatową tożsamość użytkownika.</span><span class="sxs-lookup"><span data-stu-id="08205-114">Service principals are part of role-based access control (RBAC), which creates a unique user identity.</span></span> <span data-ttu-id="08205-115">Aby utworzyć nową jednostkę usługi za pomocą interfejsu wiersza polecenia, uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="08205-115">To create a new service principal with the CLI, run the following command:</span></span>

```azurecli-interactive
az ad sp create-for-rbac --name az-go-vm-quickstart
```

<span data-ttu-id="08205-116">__Koniecznie__ zapisz wartości `appId`, `password` i `tenant` w danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="08205-116">__Make sure__ to record the `appId`, `password`, and `tenant` values in the output.</span></span> <span data-ttu-id="08205-117">Za pomocą tych wartości aplikacja przeprowadza uwierzytelnianie na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="08205-117">These values are used by the application to authenticate with Azure.</span></span>

<span data-ttu-id="08205-118">Aby uzyskać więcej informacji na temat tworzenia jednostek usługi i zarządzania nimi za pomocą interfejsu wiersza polecenia platformy Azure 2.0, zobacz [Tworzenie jednostki usługi platformy Azure za pomocą interfejsu wiersza polecenia platformy Azure 2.0](/cli/azure/create-an-azure-service-principal-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="08205-118">For more information on creating and managing service principals with the Azure CLI 2.0, see [Create an Azure service principal with Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli).</span></span>

## <a name="get-the-code"></a><span data-ttu-id="08205-119">Uzyskiwanie kodu</span><span class="sxs-lookup"><span data-stu-id="08205-119">Get the code</span></span>

<span data-ttu-id="08205-120">Kod szybkiego startu oraz wszystkie jego zależności można pobrać, korzystając z polecenia `go get`.</span><span class="sxs-lookup"><span data-stu-id="08205-120">Get the quickstart code and all of its dependencies with `go get`.</span></span>

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm/...
```

<span data-ttu-id="08205-121">Ten kod jest kompilowany, ale nie działa poprawnie, dopóki nie podasz informacji dotyczących swojego konta platformy Azure i nie utworzysz jednostki usługi.</span><span class="sxs-lookup"><span data-stu-id="08205-121">This code compiles, but doesn't run correctly until you provide it information about your Azure account and the created service principal.</span></span> <span data-ttu-id="08205-122">W poleceniu `main.go` występuje zmienna `config`, która zawiera strukturę `authInfo`.</span><span class="sxs-lookup"><span data-stu-id="08205-122">In `main.go` there is a variable, `config`, which contains an `authInfo` struct.</span></span> <span data-ttu-id="08205-123">Aby zapewnić poprawne uwierzytelnianie, w strukturze tej trzeba zastąpić wartości pól.</span><span class="sxs-lookup"><span data-stu-id="08205-123">This struct needs to have its field values replaced in order to authenticate correctly.</span></span>

```go
    config = authInfo{ // Your application credentials
        TenantID:               "", // Azure account tenantID
        SubscriptionID:         "", // Azure subscription subscriptionID
        ServicePrincipalID:     "", // Service principal appId
        ServicePrincipalSecret: "", // Service principal password/secret
    }
```

* <span data-ttu-id="08205-124">`SubscriptionID`: Twój identyfikator subskrypcji, który można uzyskać za pomocą polecenia interfejsu CLI</span><span class="sxs-lookup"><span data-stu-id="08205-124">`SubscriptionID`: Your subscription ID, which can be obtained from the CLI command</span></span>

  ```azurecli-interactive
  az account show --query id -o tsv
  ```

* <span data-ttu-id="08205-125">`TenantID`: Twój identyfikator dzierżawy, wartość `tenant` zarejestrowana podczas tworzenia jednostki usługi</span><span class="sxs-lookup"><span data-stu-id="08205-125">`TenantID`: Your tenant ID, the `tenant` value recorded when creating the service principal</span></span>
* <span data-ttu-id="08205-126">`ServicePrincipalID`: wartość `appId` zarejestrowana podczas tworzenia jednostki usługi</span><span class="sxs-lookup"><span data-stu-id="08205-126">`ServicePrincipalID`: The `appId` value recorded when creating the service principal</span></span>
* <span data-ttu-id="08205-127">`ServicePrincipalSecret`: wartość `password` zarejestrowana podczas tworzenia jednostki usługi</span><span class="sxs-lookup"><span data-stu-id="08205-127">`ServicePrincipalSecret`: The `password` value recorded when creating the service principal</span></span>

<span data-ttu-id="08205-128">Należy również poddać edycji wartość w pliku `vm-quickstart-params.json`.</span><span class="sxs-lookup"><span data-stu-id="08205-128">You also need to edit a value in the `vm-quickstart-params.json` file.</span></span>

```json
    "vm_password": {
        "value": "_"
    }
```

* <span data-ttu-id="08205-129">`vm_password`: hasło konta użytkownika na maszynie wirtualnej.</span><span class="sxs-lookup"><span data-stu-id="08205-129">`vm_password`: The password for the VM user account.</span></span> <span data-ttu-id="08205-130">Hasło musi zawierać od 12 do 72 znaków i 3 spośród następujących typów znaków:</span><span class="sxs-lookup"><span data-stu-id="08205-130">It must be 12-72 characters in length and contain 3 of the following characters:</span></span>
  * <span data-ttu-id="08205-131">Mała litera</span><span class="sxs-lookup"><span data-stu-id="08205-131">A lowercase letter</span></span>
  * <span data-ttu-id="08205-132">Wielka litera</span><span class="sxs-lookup"><span data-stu-id="08205-132">An uppercase letter</span></span>
  * <span data-ttu-id="08205-133">Cyfra</span><span class="sxs-lookup"><span data-stu-id="08205-133">A number</span></span>
  * <span data-ttu-id="08205-134">Symbol</span><span class="sxs-lookup"><span data-stu-id="08205-134">A symbol</span></span>

## <a name="running-the-code"></a><span data-ttu-id="08205-135">Wykonywanie kodu</span><span class="sxs-lookup"><span data-stu-id="08205-135">Running the code</span></span>

<span data-ttu-id="08205-136">Uruchom szybki start za pomocą polecenia `go run`.</span><span class="sxs-lookup"><span data-stu-id="08205-136">Run the quickstart with the `go run` command.</span></span>

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm
go run main.go
```

<span data-ttu-id="08205-137">Jeśli we wdrożeniu jest błąd, zostanie wyświetlony komunikat, który informuje, że wystąpił problem, ale nie zawiera szczegółowych informacji.</span><span class="sxs-lookup"><span data-stu-id="08205-137">If there is a failure in the deployment, you get a message indicating that there was an issue, but without any specific details.</span></span> <span data-ttu-id="08205-138">Korzystając z interfejsu wiersza polecenia platformy Azure, pobierz szczegóły dotyczące błędu wdrożenia przy użyciu następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="08205-138">Using the Azure CLI, get the details of the deployment failure with the following command:</span></span>

```azurecli-interactive
az group deployment show -g GoVMQuickstart -n VMDeployQuickstart
```

<span data-ttu-id="08205-139">Jeśli wdrażanie zakończy się pomyślnie, zostanie wyświetlony komunikat zawierający nazwę użytkownika, adres IP i hasło umożliwiające zalogowanie się na nowo utworzonej maszynie wirtualnej.</span><span class="sxs-lookup"><span data-stu-id="08205-139">If the deployment is successful, you see a message giving the username, IP address, and password for logging into the newly created virtual machine.</span></span> <span data-ttu-id="08205-140">Logując się za pomocą protokołu SSH, sprawdź, czy maszyna wirtualna faktycznie jest uruchomiona i gotowa.</span><span class="sxs-lookup"><span data-stu-id="08205-140">SSH into this machine to confirm that it is up and running.</span></span>

## <a name="cleaning-up"></a><span data-ttu-id="08205-141">Czyszczenie</span><span class="sxs-lookup"><span data-stu-id="08205-141">Cleaning up</span></span>

<span data-ttu-id="08205-142">Wyczyść zasoby utworzone w ramach tego szybkiego startu, usuwając grupę zasobów przy użyciu interfejsu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="08205-142">Clean up the resources created during this quickstart by deleting the resource group with the CLI.</span></span>

```azurecli-interactive
az group delete -n GoVMQuickstart
```

## <a name="code-in-depth"></a><span data-ttu-id="08205-143">Szczegóły kodu</span><span class="sxs-lookup"><span data-stu-id="08205-143">Code in depth</span></span>

<span data-ttu-id="08205-144">Działanie kodu szybkiego startu jest podzielone na bloki zmiennych i kilka małych funkcji. Opisano je poniżej.</span><span class="sxs-lookup"><span data-stu-id="08205-144">What the quickstart code does is broken down into a block of variables and several small functions, each of which are discussed here.</span></span>

### <a name="variable-assignments-and-structs"></a><span data-ttu-id="08205-145">Struktury i przypisania zmiennych</span><span class="sxs-lookup"><span data-stu-id="08205-145">Variable assignments and structs</span></span>

<span data-ttu-id="08205-146">Ponieważ szybki start jest niezależny, używa zmiennych globalnych, a nie opcji wiersza polecenia czy też zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="08205-146">Since quickstart is self-contained, it uses global variables rather than command-line options or environment variables.</span></span>

```go
type authInfo struct {
        TenantID               string
        SubscriptionID         string
        ServicePrincipalID     string
        ServicePrincipalSecret string
}
```

<span data-ttu-id="08205-147">Struktura `authInfo` jest deklarowana w celu hermetyzowania wszystkich informacji potrzebnych do autoryzowania w usługach platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="08205-147">The `authInfo` struct is declared to encapsulate all of the information needed for authorization with Azure services.</span></span>

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

<span data-ttu-id="08205-148">Deklarowane wartości zapewniają nazwy utworzonych zasobów.</span><span class="sxs-lookup"><span data-stu-id="08205-148">Values are declared which give the names of created resources.</span></span> <span data-ttu-id="08205-149">Podano tutaj także lokalizację. Można ją zmienić, aby sprawdzić zachowanie wdrożeń w innych centrach danych.</span><span class="sxs-lookup"><span data-stu-id="08205-149">The location is also specified here, which you can change to see how deployments behave in other datacenters.</span></span> <span data-ttu-id="08205-150">Nie każde centrum danych ma dostępne wszystkie wymagane zasoby.</span><span class="sxs-lookup"><span data-stu-id="08205-150">Not every datacenter has all of the required resources available.</span></span>

<span data-ttu-id="08205-151">Stałe `templateFile` i `parametersFile` wskazują pliki potrzebne do wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="08205-151">The `templateFile` and `parametersFile` constants point to the files needed for deployment.</span></span> <span data-ttu-id="08205-152">Token jednostki usługi jest podawany później, a zmienna `ctx` stanowi [kontekst języka Go](https://blog.golang.org/context) dla operacji sieciowych.</span><span class="sxs-lookup"><span data-stu-id="08205-152">The service principal token is covered later, and the `ctx` variable is a [Go context](https://blog.golang.org/context) for the network operations.</span></span>

### <a name="init-and-authorization"></a><span data-ttu-id="08205-153">Metoda init() i autoryzacja</span><span class="sxs-lookup"><span data-stu-id="08205-153">init() and authorization</span></span>

<span data-ttu-id="08205-154">Metoda `init()` umożliwia skonfigurowanie autoryzacji w kodzie.</span><span class="sxs-lookup"><span data-stu-id="08205-154">The `init()` method for the code sets up authorization.</span></span> <span data-ttu-id="08205-155">Autoryzacja jest warunkiem wstępnym w każdym procesie szybkiego startu, a zatem warto uczynić ją częścią inicjowania.</span><span class="sxs-lookup"><span data-stu-id="08205-155">Since authorization is a precondition for everything in the quickstart, it makes sense to have it as part of initialization.</span></span> 

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

<span data-ttu-id="08205-156">Ten kod obejmuje dwa etapy autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="08205-156">This code completes two steps for authorization:</span></span>

* <span data-ttu-id="08205-157">Pobranie informacji o konfiguracji uwierzytelniania OAuth dla elementu `TenantID` dzięki komunikacji z usługą Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="08205-157">OAuth configuration information for the `TenantID` is retrieved by interfacing with Azure Active Directory.</span></span> <span data-ttu-id="08205-158">Obiekt [`azure.PublicCloud`](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#PublicCloud) zawiera punkty końcowe używane w standardowej konfiguracji platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="08205-158">The [`azure.PublicCloud`](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#PublicCloud) object contains endpoints used in the standard Azure configuration.</span></span>
* <span data-ttu-id="08205-159">Wywołanie funkcji [`adal.NewServicePrincipalToken()`](https://godoc.org/github.com/Azure/go-autorest/autorest/adal#NewServicePrincipalToken).</span><span class="sxs-lookup"><span data-stu-id="08205-159">The [`adal.NewServicePrincipalToken()`](https://godoc.org/github.com/Azure/go-autorest/autorest/adal#NewServicePrincipalToken) function is called.</span></span> <span data-ttu-id="08205-160">Ta funkcja przyjmuje informacje dotyczące uwierzytelniania OAuth wraz z danymi logowania jednostki usługi oraz używanego stylu zarządzania platformą Azure.</span><span class="sxs-lookup"><span data-stu-id="08205-160">This function takes the OAuth information along with the service principal login, as well as which style of Azure management is being used.</span></span> <span data-ttu-id="08205-161">Jeśli nie istnieją specyficzne wymagania i dobrze wiesz, co robisz, ta wartość powinna mieć zawsze postać `.ResourceManagerEndpoint`.</span><span class="sxs-lookup"><span data-stu-id="08205-161">Unless you have specific requirements and know what you're doing, this value should always be `.ResourceManagerEndpoint`.</span></span>

### <a name="flow-of-operations-in-main"></a><span data-ttu-id="08205-162">Przepływ operacji w funkcji main()</span><span class="sxs-lookup"><span data-stu-id="08205-162">Flow of operations in main()</span></span>

<span data-ttu-id="08205-163">Funkcja `main()` jest prosta — wskazuje tylko przepływ operacji i przeprowadza sprawdzanie błędów.</span><span class="sxs-lookup"><span data-stu-id="08205-163">The `main()` function is simple, only indicating the flow of operations and performing error-checking.</span></span>

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

<span data-ttu-id="08205-164">Oto działania wykonywane przez kod (z zachowaniem kolejności):</span><span class="sxs-lookup"><span data-stu-id="08205-164">The steps that the code runs through are, in order:</span></span>

* <span data-ttu-id="08205-165">Utworzenie grupy zasobów, w której ma zostać przeprowadzone wdrażanie (`createGroup()`)</span><span class="sxs-lookup"><span data-stu-id="08205-165">Create the resource group to deploy to (`createGroup()`)</span></span>
* <span data-ttu-id="08205-166">Utworzenie wdrożenia w tej grupie (`createDeployment()`)</span><span class="sxs-lookup"><span data-stu-id="08205-166">Create the deployment within this group (`createDeployment()`)</span></span>
* <span data-ttu-id="08205-167">Uzyskanie i wyświetlenie danych logowania dotyczących wdrożonej maszyny wirtualnej (`getLogin()`)</span><span class="sxs-lookup"><span data-stu-id="08205-167">Obtain and display login information for the deployed VM (`getLogin()`)</span></span>

### <a name="creating-the-resource-group"></a><span data-ttu-id="08205-168">Tworzenie grupy zasobów</span><span class="sxs-lookup"><span data-stu-id="08205-168">Creating the resource group</span></span>

<span data-ttu-id="08205-169">Funkcja `createGroup()` umożliwia utworzenie grupy zasobów.</span><span class="sxs-lookup"><span data-stu-id="08205-169">The `createGroup()` function creates the resource group.</span></span> <span data-ttu-id="08205-170">Przepływ wywołań i argumenty pozwalają zapoznać się ze strukturą interakcji z usługami w zestawie SDK.</span><span class="sxs-lookup"><span data-stu-id="08205-170">Looking at the call flow and arguments demonstrates the way that service interactions are structured in the SDK.</span></span>

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

<span data-ttu-id="08205-171">Ogólny przebieg interakcji z usługą platformy Azure wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="08205-171">The general flow of interacting with an Azure service is:</span></span>

* <span data-ttu-id="08205-172">Utworzenie klienta przy użyciu metody `service.NewXClient()`, gdzie `X` to typ zasobu `service`, z którym ma nastąpić interakcja.</span><span class="sxs-lookup"><span data-stu-id="08205-172">Create the client using the `service.NewXClient()` method, where `X` is the resource type of the `service` that you want to interact with.</span></span> <span data-ttu-id="08205-173">Ta funkcja zawsze pobiera identyfikator subskrypcji.</span><span class="sxs-lookup"><span data-stu-id="08205-173">This function always takes a subscription ID.</span></span>
* <span data-ttu-id="08205-174">Ustawienie metody autoryzacji klienta z uwzględnieniem interakcji ze zdalnym interfejsem API.</span><span class="sxs-lookup"><span data-stu-id="08205-174">Set the authorization method for the client, allowing it to interact with the remote API.</span></span>
* <span data-ttu-id="08205-175">Wywołanie metody na kliencie odpowiadającym zdalnemu interfejsowi API.</span><span class="sxs-lookup"><span data-stu-id="08205-175">Make the method call on the client corresponding to the remote API.</span></span> <span data-ttu-id="08205-176">Metody klienta usługi zazwyczaj pobierają nazwę zasobu i obiektu metadanych.</span><span class="sxs-lookup"><span data-stu-id="08205-176">Service client methods usually take the name of the resource and a metadata object.</span></span>

<span data-ttu-id="08205-177">Funkcja [`to.StringPtr()`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) służy tutaj do przeprowadzania konwersji typów.</span><span class="sxs-lookup"><span data-stu-id="08205-177">The [`to.StringPtr()`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) function is used to perform a type conversion here.</span></span> <span data-ttu-id="08205-178">Struktury parametrów dla metod zestawu SDK przyjmują niemal wyłącznie wskaźniki. Metody te mają zatem za zadanie ułatwienie konwersji typów.</span><span class="sxs-lookup"><span data-stu-id="08205-178">The parameters structs for methods of the SDK almost exclusively take pointers, so these methods are provided to make the type conversions easy.</span></span> <span data-ttu-id="08205-179">Pełną listę i zachowanie konwerterów można znaleźć w dokumentacji modułu [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to).</span><span class="sxs-lookup"><span data-stu-id="08205-179">See the documentation for the [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) module for the complete list and behavior of convenience converters.</span></span>

<span data-ttu-id="08205-180">Operacja `groupsClient.CreateOrUpdate()` zwraca wskaźnik do struktury danych reprezentującej grupę zasobów.</span><span class="sxs-lookup"><span data-stu-id="08205-180">The `groupsClient.CreateOrUpdate()` operation returns a pointer to a data struct representing the resource group.</span></span> <span data-ttu-id="08205-181">Bezpośrednia zwracana wartość tego rodzaju wskazuje krótkotrwałą operację, która ma być synchroniczna.</span><span class="sxs-lookup"><span data-stu-id="08205-181">A direct return value of this kind indicates a short-running operation that is meant to be synchronous.</span></span> <span data-ttu-id="08205-182">W następnej sekcji przedstawimy przykład długotrwałej operacji i sposób odpowiedniej interakcji.</span><span class="sxs-lookup"><span data-stu-id="08205-182">In the next section, you'll see an example of a long-running operation and how to interact with them.</span></span>

### <a name="performing-the-deployment"></a><span data-ttu-id="08205-183">Przeprowadzanie wdrażania</span><span class="sxs-lookup"><span data-stu-id="08205-183">Performing the deployment</span></span>

<span data-ttu-id="08205-184">Po utworzeniu grupy przeznaczonej na zasoby można już uruchomić wdrożenie.</span><span class="sxs-lookup"><span data-stu-id="08205-184">Once the group to contain its resources is created, it's time to run the deployment.</span></span> <span data-ttu-id="08205-185">Ten kod został podzielony na mniejsze części w celu wyróżnienia różnych części logiki kodu.</span><span class="sxs-lookup"><span data-stu-id="08205-185">This code is broken up into smaller sections to emphasize different parts of its logic.</span></span>


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

<span data-ttu-id="08205-186">Pliki wdrożenia są ładowane przez element `readJSON` (szczegóły zostały tutaj pominięte).</span><span class="sxs-lookup"><span data-stu-id="08205-186">The deployment files are loaded by `readJSON`, the details of which are skipped here.</span></span> <span data-ttu-id="08205-187">Ta funkcja zwraca typ `*map[string]interface{}` używany podczas tworzenia metadanych dla wywołania wdrażania zasobów.</span><span class="sxs-lookup"><span data-stu-id="08205-187">This function returns a `*map[string]interface{}`, the type used in constructing the metadata for the resource deployment call.</span></span>

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

<span data-ttu-id="08205-188">Ten kod jest zgodny z tym samym wzorcem, który jest używany podczas tworzenia grupy zasobów.</span><span class="sxs-lookup"><span data-stu-id="08205-188">This code follows the same pattern as with creating the resource group.</span></span> <span data-ttu-id="08205-189">Zostaje utworzony nowy klient (pod warunkiem, że uwierzytelnienie w usłudze Azure jest możliwe), a następnie zostaje wywołana metoda.</span><span class="sxs-lookup"><span data-stu-id="08205-189">A new client is created, given the ability to authenticate with Azure, and then a method is called.</span></span> <span data-ttu-id="08205-190">Metoda ma nawet taką samą nazwę (`CreateOrUpdate`) co analogiczna metoda dotycząca grupy zasobów.</span><span class="sxs-lookup"><span data-stu-id="08205-190">The method even has the same name (`CreateOrUpdate`) as the corresponding method for resource groups.</span></span> <span data-ttu-id="08205-191">Ten wzorzec jest używany w zestawie SDK wielokrotnie.</span><span class="sxs-lookup"><span data-stu-id="08205-191">This pattern is seen again and again in the SDK.</span></span> <span data-ttu-id="08205-192">Metody, które mają podobne działanie, mają zwykle taką samą nazwę.</span><span class="sxs-lookup"><span data-stu-id="08205-192">Methods that perform similar work normally have the same name.</span></span>

<span data-ttu-id="08205-193">Największa różnica dotyczy wartości zwracanej przez metodę `deploymentsClient.CreateOrUpdate()`.</span><span class="sxs-lookup"><span data-stu-id="08205-193">The biggest difference comes in the return value of the `deploymentsClient.CreateOrUpdate()` method.</span></span> <span data-ttu-id="08205-194">Ta wartość to obiekt `Future` zgodny z [wzorcem projektowym obiektu future](https://en.wikipedia.org/wiki/Futures_and_promises).</span><span class="sxs-lookup"><span data-stu-id="08205-194">This value is a `Future` object, which follows the [future design pattern](https://en.wikipedia.org/wiki/Futures_and_promises).</span></span> <span data-ttu-id="08205-195">W kontekście platformy Azure obiekty future reprezentują długotrwałą operację, którą można sporadycznie sondować podczas wykonywania innych zadań.</span><span class="sxs-lookup"><span data-stu-id="08205-195">Futures represent a long-running operation in Azure that you may want to occasionally poll while performing other work.</span></span>

```go
        //...
        err = deploymentFuture.Future.WaitForCompletion(ctx, deploymentsClient.BaseClient.Client)
        if err != nil {
                log.Fatalf("Error while waiting for deployment creation: %v", err)
        }
        return deploymentFuture.Result(deploymentsClient)
}
```

<span data-ttu-id="08205-196">W przypadku tego przykładu najlepiej zaczekać na ukończenie operacji.</span><span class="sxs-lookup"><span data-stu-id="08205-196">For this example, the best thing to do is to wait for the operation to complete.</span></span> <span data-ttu-id="08205-197">Oczekiwanie na obiekt future wymaga zarówno [obiektu kontekstu](https://blog.golang.org/context), jak i klienta, który utworzył obiekt future.</span><span class="sxs-lookup"><span data-stu-id="08205-197">Waiting on a future requires both a [context object](https://blog.golang.org/context) and the client that created the Future object.</span></span> <span data-ttu-id="08205-198">Istnieją tutaj dwa możliwe źródła błędu: błąd występujący po stronie klienta podczas próby wywołania metody oraz odpowiedź serwera z informacją o błędzie.</span><span class="sxs-lookup"><span data-stu-id="08205-198">There are two possible error sources here: An error caused on the client side when trying to invoke the method, and an error response from the server.</span></span> <span data-ttu-id="08205-199">Ta druga jest zwracana jako część wywołania `deploymentFuture.Result()`.</span><span class="sxs-lookup"><span data-stu-id="08205-199">The latter is returned as part of the `deploymentFuture.Result()` call.</span></span>

### <a name="obtaining-the-assigned-ip-address"></a><span data-ttu-id="08205-200">Uzyskiwanie przypisanego adresu IP</span><span class="sxs-lookup"><span data-stu-id="08205-200">Obtaining the assigned IP address</span></span>

<span data-ttu-id="08205-201">Aby można było korzystać z nowo utworzonej maszyny wirtualnej, trzeba mieć przypisany adres IP.</span><span class="sxs-lookup"><span data-stu-id="08205-201">To do anything with the newly created VM, you need the assigned IP address.</span></span> <span data-ttu-id="08205-202">Adresy IP stanowią oddzielne zasoby platformy Azure powiązane z zasobami kontrolera interfejsu sieciowego.</span><span class="sxs-lookup"><span data-stu-id="08205-202">IP addresses are their own separate Azure resource, bound to Network Interface Controller (NIC) resources.</span></span>

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

<span data-ttu-id="08205-203">Ta metoda korzysta z informacji przechowywanych w pliku parametrów.</span><span class="sxs-lookup"><span data-stu-id="08205-203">This method relies on information that is stored in the parameters file.</span></span> <span data-ttu-id="08205-204">Kod może wysłać zapytanie bezpośrednio do maszyny wirtualnej, aby uzyskać dane jej kontrolera interfejsu sieciowego, wysłać zapytanie do kontrolera interfejsu sieciowego, aby uzyskać jego adres IP, a następnie wysłać zapytanie bezpośrednio do zasobu z danym adresem IP.</span><span class="sxs-lookup"><span data-stu-id="08205-204">The code could query the VM directly to get its NIC, query the NIC to get its IP resource, and then query the IP resource directly.</span></span> <span data-ttu-id="08205-205">Jest to długi łańcuch zależności. Wymaga on wykonania wielu operacji, przez co jest kosztowny.</span><span class="sxs-lookup"><span data-stu-id="08205-205">That's a long chain of dependencies and operations to resolve, making it expensive.</span></span> <span data-ttu-id="08205-206">Ponieważ dane JSON są lokalne, można je załadować zamiast wykonywania powyższych operacji.</span><span class="sxs-lookup"><span data-stu-id="08205-206">Since the JSON information is local, it can be loaded instead.</span></span>

<span data-ttu-id="08205-207">Wartości dotyczące użytkownika maszyny wirtualnej i jego hasła również są ładowane z danych JSON.</span><span class="sxs-lookup"><span data-stu-id="08205-207">The values for the VM user and password are likewise loaded from the JSON.</span></span>

## <a name="next-steps"></a><span data-ttu-id="08205-208">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="08205-208">Next steps</span></span>

<span data-ttu-id="08205-209">W tym przewodniku Szybki start istniejący szablon został wdrożony za pomocą języka Go.</span><span class="sxs-lookup"><span data-stu-id="08205-209">In this quickstart, you took an existing template and deployed it through Go.</span></span> <span data-ttu-id="08205-210">Następnie nastąpiło nawiązanie połączenia z nowo utworzoną maszyną wirtualną za pośrednictwem protokołu SSH. To pozwoliło upewnić się, że maszyna jest uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="08205-210">Then you connected to the newly created VM via SSH to ensure that it's running.</span></span>

<span data-ttu-id="08205-211">Aby uzyskać więcej informacji o pracy z maszynami wirtualnymi w środowisku platformy Azure z użyciem języka Go, zapoznaj się z [przykładami dotyczącymi obliczeń na platformie Azure dla języka Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) lub [przykładami dotyczącymi zarządzania zasobami na platformie Azure dla języka Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span><span class="sxs-lookup"><span data-stu-id="08205-211">To continue learning about working with virtual machines in the Azure environment with Go, take a look at the [Azure compute samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) or [Azure resource management samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span></span>
