---
title: Wdrażanie maszyny wirtualnej platformy Azure z poziomu kodu Go
description: Wdróż maszynę wirtualną za pomocą zestawu Azure SDK dla języka Go.
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: quickstart
ms.technology: azure-sdk-go
ms.service: virtual-machines
ms.devlang: go
ms.openlocfilehash: a7970be0857fd414d776241b033af0c23457790c
ms.sourcegitcommit: 8b9e10b960150dc08f046ab840d6a5627410db29
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44059139"
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a><span data-ttu-id="fbb91-103">Szybki start: wdrażanie maszyny wirtualnej platformy Azure za pomocą szablonu i zestawu Azure SDK dla języka Go</span><span class="sxs-lookup"><span data-stu-id="fbb91-103">Quickstart: Deploy an Azure virtual machine from a template with the Azure SDK for Go</span></span>

<span data-ttu-id="fbb91-104">Ten przewodnik Szybki start przedstawia sposób wdrażania zasobów z szablonu usługi Azure Resource Manager przy użyciu zestawu Azure SDK dla języka Go.</span><span class="sxs-lookup"><span data-stu-id="fbb91-104">This quickstart shows you how to deploy resources from an Azure Resource Manager template, using the Azure SDK for Go.</span></span> <span data-ttu-id="fbb91-105">Szablony są migawkami wszystkich zasobów zawartych w [grupie zasobów platformy Azure](/azure/azure-resource-manager/resource-group-overview).</span><span class="sxs-lookup"><span data-stu-id="fbb91-105">Templates are snapshots of all of the resources within an [Azure resource group](/azure/azure-resource-manager/resource-group-overview).</span></span> <span data-ttu-id="fbb91-106">W ramach tej procedury poznasz funkcje i konwencje zestawu SDK.</span><span class="sxs-lookup"><span data-stu-id="fbb91-106">Along the way, you'll become familiar with the functionality and conventions of the SDK.</span></span>

<span data-ttu-id="fbb91-107">Na końcu tego przewodnika znajdują się dane działającej maszyny wirtualnej, na której możesz się zalogować się przy użyciu nazwy użytkownika i hasła.</span><span class="sxs-lookup"><span data-stu-id="fbb91-107">At the end of this quickstart, you have a running VM that you log into with a username and password.</span></span>

> [!NOTE]
> <span data-ttu-id="fbb91-108">Aby zobaczyć tworzenie maszyny wirtualnej w języku Go bez użycia szablonu usługi Resource Manager, przejdź do [przykładu](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm.go) przedstawiającego sposób tworzenia i konfigurowania wszystkich zasobów maszyn wirtualnych za pomocą zestawu SDK.</span><span class="sxs-lookup"><span data-stu-id="fbb91-108">To see the creation of a VM in Go without the use of a Resource Manager template, there is an [imperative sample](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm.go) demonstrating how to build and configure all VM resources with the SDK.</span></span> <span data-ttu-id="fbb91-109">Zastosowanie szablonu w tym przykładzie umożliwia skupienie się na konwencjach zestawu SDK bez zagłębiania się w zbyt wiele szczegółów dotyczących architektury usługi platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="fbb91-109">Using a template in this sample allows a focus on SDK conventions without getting into too many details about Azure service architecture.</span></span>

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

<span data-ttu-id="fbb91-110">W przypadku instalacji lokalnej interfejsu wiersza polecenia platformy Azure ten przewodnik Szybki start wymaga interfejsu wiersza polecenia w wersji __2.0.28__ lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="fbb91-110">If you use a local install of the Azure CLI, this quickstart requires CLI version __2.0.28__ or later.</span></span> <span data-ttu-id="fbb91-111">Uruchom polecenie `az --version`, aby upewnić się, że dany interfejs wiersza polecenia spełnia ten wymóg.</span><span class="sxs-lookup"><span data-stu-id="fbb91-111">Run `az --version` to make sure your CLI install meets this requirement.</span></span> <span data-ttu-id="fbb91-112">Jeśli konieczna będzie instalacja lub uaktualnienie interfejsu, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="fbb91-112">If you need to install or upgrade, see [Install the Azure CLI](/cli/azure/install-azure-cli).</span></span>

## <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="fbb91-113">Instalowanie zestawu Azure SDK dla języka Go</span><span class="sxs-lookup"><span data-stu-id="fbb91-113">Install the Azure SDK for Go</span></span>

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a><span data-ttu-id="fbb91-114">Tworzenie nazwy głównej usługi</span><span class="sxs-lookup"><span data-stu-id="fbb91-114">Create a service principal</span></span>

<span data-ttu-id="fbb91-115">Aby można było zalogować się do platformy Azure za pomocą aplikacji w sposób nieinterakcyjny, potrzebna jest jednostka usługi.</span><span class="sxs-lookup"><span data-stu-id="fbb91-115">To sign in non-interactively to Azure with an application, you need a service principal.</span></span> <span data-ttu-id="fbb91-116">Jednostki usługi stanowią część kontroli dostępu na podstawie ról (RBAC) tworzącej unikatową tożsamość użytkownika.</span><span class="sxs-lookup"><span data-stu-id="fbb91-116">Service principals are part of role-based access control (RBAC), which creates a unique user identity.</span></span> <span data-ttu-id="fbb91-117">Aby utworzyć nową jednostkę usługi za pomocą interfejsu wiersza polecenia, uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="fbb91-117">To create a new service principal with the CLI, run the following command:</span></span>

```azurecli-interactive
az ad sp create-for-rbac --sdk-auth > quickstart.auth
```

<span data-ttu-id="fbb91-118">Ustaw zmienną środowiskową `AZURE_AUTH_LOCATION` tak, aby była pełną ścieżką do tego pliku.</span><span class="sxs-lookup"><span data-stu-id="fbb91-118">Set the environment variable `AZURE_AUTH_LOCATION` to be the full path to this file.</span></span> <span data-ttu-id="fbb91-119">Następnie zestaw SDK zlokalizuje i odczyta poświadczenia bezpośrednio z tego pliku, bez konieczności wprowadzania zmian ani rejestrowania informacji z jednostki usługi.</span><span class="sxs-lookup"><span data-stu-id="fbb91-119">Then the SDK locates and reads the credentials directly from this file, without you having to make any changes or record information from the service principal.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="fbb91-120">Uzyskiwanie kodu</span><span class="sxs-lookup"><span data-stu-id="fbb91-120">Get the code</span></span>

<span data-ttu-id="fbb91-121">Kod szybkiego startu oraz wszystkie jego zależności można pobrać, korzystając z polecenia `go get`.</span><span class="sxs-lookup"><span data-stu-id="fbb91-121">Get the quickstart code and all of its dependencies with `go get`.</span></span>

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm/...
```

<span data-ttu-id="fbb91-122">Nie trzeba wprowadzać modyfikacji kodu źródłowego, jeśli zmienna `AZURE_AUTH_LOCATION` została poprawnie ustawiona.</span><span class="sxs-lookup"><span data-stu-id="fbb91-122">You don't need to make any source code modifications if the `AZURE_AUTH_LOCATION` variable is properly set.</span></span> <span data-ttu-id="fbb91-123">Po uruchomieniu program ładuje wszystkie niezbędne informacje dotyczące uwierzytelniania z tego miejsca.</span><span class="sxs-lookup"><span data-stu-id="fbb91-123">When the program runs, it loads all the necessary authentication information from there.</span></span>

## <a name="running-the-code"></a><span data-ttu-id="fbb91-124">Wykonywanie kodu</span><span class="sxs-lookup"><span data-stu-id="fbb91-124">Running the code</span></span>

<span data-ttu-id="fbb91-125">Uruchom szybki start za pomocą polecenia `go run`.</span><span class="sxs-lookup"><span data-stu-id="fbb91-125">Run the quickstart with the `go run` command.</span></span>

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm
go run main.go
```

<span data-ttu-id="fbb91-126">Jeśli wdrażanie zakończy się pomyślnie, zostanie wyświetlony komunikat zawierający nazwę użytkownika, adres IP i hasło umożliwiające zalogowanie się na nowo utworzonej maszynie wirtualnej.</span><span class="sxs-lookup"><span data-stu-id="fbb91-126">If the deployment is successful, you see a message giving the username, IP address, and password for logging into the newly created virtual machine.</span></span> <span data-ttu-id="fbb91-127">Spróbuj zalogować się do tej maszyny wirtualnej za pomocą protokołu SSH, aby sprawdzić, czy jest uruchomiona i działa.</span><span class="sxs-lookup"><span data-stu-id="fbb91-127">SSH into this machine to see if it's up and running.</span></span> 

## <a name="cleaning-up"></a><span data-ttu-id="fbb91-128">Czyszczenie</span><span class="sxs-lookup"><span data-stu-id="fbb91-128">Cleaning up</span></span>

<span data-ttu-id="fbb91-129">Wyczyść zasoby utworzone w ramach tego szybkiego startu, usuwając grupę zasobów przy użyciu interfejsu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="fbb91-129">Clean up the resources created during this quickstart by deleting the resource group with the CLI.</span></span>

```azurecli-interactive
az group delete -n GoVMQuickstart
```

<span data-ttu-id="fbb91-130">Usuń również utworzoną wcześniej jednostkę usługi.</span><span class="sxs-lookup"><span data-stu-id="fbb91-130">Also delete the service principal that was created.</span></span> <span data-ttu-id="fbb91-131">W pliku `quickstart.auth` znajduje się klucz JSON dla elementu `clientId`.</span><span class="sxs-lookup"><span data-stu-id="fbb91-131">In the `quickstart.auth` file, there's a JSON key for `clientId`.</span></span> <span data-ttu-id="fbb91-132">Skopiuj tę wartość do zmiennej środowiskowej `CLIENT_ID_VALUE` i uruchom następujące polecenie w wierszu polecenia platformy Azure:</span><span class="sxs-lookup"><span data-stu-id="fbb91-132">Copy this value to the `CLIENT_ID_VALUE` environment variable and run the following Azure CLI command:</span></span>

```azurecli-interactive
az ad sp delete --id ${CLIENT_ID_VALUE}
```

<span data-ttu-id="fbb91-133">Tu podaj wartość `CLIENT_ID_VALUE` pobraną z pliku `quickstart.auth`.</span><span class="sxs-lookup"><span data-stu-id="fbb91-133">Where you supply the value for `CLIENT_ID_VALUE` from `quickstart.auth`.</span></span>

> [!WARNING]
> <span data-ttu-id="fbb91-134">Nieusunięcie jednostki usługi dla tej aplikacji pozostawi ją aktywną w Twojej dzierżawie usługi Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="fbb91-134">Failing to delete the service principal for this application leaves it active in your Azure Active Directory tenant.</span></span>
> <span data-ttu-id="fbb91-135">Chociaż nazwa i hasło dla jednostki usługi są generowane jako identyfikatory UUID, ze względów bezpieczeństwa usuń wszystkie nieużywane jednostki usługi i aplikacje platformy Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="fbb91-135">While both the name and password for the service principal are generated as UUIDs, make sure that you follow good security practices by deleting any unused service principals and Azure Active Directory Applications.</span></span>

## <a name="code-in-depth"></a><span data-ttu-id="fbb91-136">Szczegóły kodu</span><span class="sxs-lookup"><span data-stu-id="fbb91-136">Code in depth</span></span>

<span data-ttu-id="fbb91-137">Działanie kodu szybkiego startu jest podzielone na bloki zmiennych i kilka małych funkcji. Opisano je poniżej.</span><span class="sxs-lookup"><span data-stu-id="fbb91-137">What the quickstart code does is broken down into a block of variables and several small functions, each of which are discussed here.</span></span>

### <a name="variables-constants-and-types"></a><span data-ttu-id="fbb91-138">Zmienne, stałe i typy</span><span class="sxs-lookup"><span data-stu-id="fbb91-138">Variables, constants, and types</span></span>

<span data-ttu-id="fbb91-139">Ponieważ przewodnik Szybki start jest używany jako samodzielny dokument, są w nim stosowane stałe i zmienne.</span><span class="sxs-lookup"><span data-stu-id="fbb91-139">Since quickstart is self-contained, it uses global constants and variables.</span></span>

```go
const (
    resourceGroupName     = "GoVMQuickstart"
    resourceGroupLocation = "eastus"

    deploymentName = "VMDeployQuickstart"
    templateFile   = "vm-quickstart-template.json"
    parametersFile = "vm-quickstart-params.json"
)

// Information loaded from the authorization file to identify the client
type clientInfo struct {
    SubscriptionID string
    VMPassword     string
}

var (
    ctx        = context.Background()
    clientData clientInfo
    authorizer autorest.Authorizer
)
```

<span data-ttu-id="fbb91-140">Deklarowane wartości zapewniają nazwy utworzonych zasobów.</span><span class="sxs-lookup"><span data-stu-id="fbb91-140">Values are declared which give the names of created resources.</span></span> <span data-ttu-id="fbb91-141">Podano tutaj także lokalizację. Można ją zmienić, aby sprawdzić zachowanie wdrożeń w innych centrach danych.</span><span class="sxs-lookup"><span data-stu-id="fbb91-141">The location is also specified here, which you can change to see how deployments behave in other datacenters.</span></span> <span data-ttu-id="fbb91-142">Nie każde centrum danych ma dostępne wszystkie wymagane zasoby.</span><span class="sxs-lookup"><span data-stu-id="fbb91-142">Not every datacenter has all of the required resources available.</span></span>

<span data-ttu-id="fbb91-143">Typ `clientInfo` przechowuje informacje załadowane z pliku uwierzytelniania w celu skonfigurowania klientów w zestawie SDK i ustawienia hasła maszyny wirtualnej.</span><span class="sxs-lookup"><span data-stu-id="fbb91-143">The `clientInfo` type holds the information loaded from the authentication file to set up clients in the SDK and set the VM password.</span></span>

<span data-ttu-id="fbb91-144">Stałe `templateFile` i `parametersFile` wskazują pliki potrzebne do wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="fbb91-144">The `templateFile` and `parametersFile` constants point to the files needed for deployment.</span></span> <span data-ttu-id="fbb91-145">Element `authorizer` zostanie skonfigurowany przez zestaw SDK języka Go w celu uwierzytelnienia, a zmienna `ctx` to [kontekst języka Go](https://blog.golang.org/context) dla operacji sieciowych.</span><span class="sxs-lookup"><span data-stu-id="fbb91-145">The `authorizer` will be configured by the Go SDK for authentication, and the `ctx` variable is a [Go context](https://blog.golang.org/context) for the network operations.</span></span>

### <a name="authentication-and-initialization"></a><span data-ttu-id="fbb91-146">Uwierzytelnianie i inicjowanie</span><span class="sxs-lookup"><span data-stu-id="fbb91-146">Authentication and initialization</span></span>

<span data-ttu-id="fbb91-147">Funkcja `init` konfiguruje uwierzytelnianie.</span><span class="sxs-lookup"><span data-stu-id="fbb91-147">The `init` function sets up authentication.</span></span> <span data-ttu-id="fbb91-148">Uwierzytelnianie jest warunkiem wstępnym w każdym procesie szybkiego startu, a zatem warto uczynić je częścią inicjowania.</span><span class="sxs-lookup"><span data-stu-id="fbb91-148">Since authentication is a precondition for everything in the quickstart, it makes sense to have it as part of initialization.</span></span> <span data-ttu-id="fbb91-149">Powoduje ono również pobieranie niektórych potrzebnych informacji z pliku uwierzytelniania w celu skonfigurowania klientów i maszyny wirtualnej.</span><span class="sxs-lookup"><span data-stu-id="fbb91-149">It also loads some information needed from the authentication file to configure clients and the VM.</span></span>

```go
func init() {
    var err error
    authorizer, err = auth.NewAuthorizerFromFile(azure.PublicCloud.ResourceManagerEndpoint)
    if err != nil {
        log.Fatalf("Failed to get OAuth config: %v", err)
    }

    authInfo, err := readJSON(os.Getenv("AZURE_AUTH_LOCATION"))
    clientData.SubscriptionID = (*authInfo)["subscriptionId"].(string)
    clientData.VMPassword = (*authInfo)["clientSecret"].(string)
}
```

<span data-ttu-id="fbb91-150">Najpierw plik [auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) jest wywoływany w celu załadowania informacji dotyczących uwierzytelniania z pliku znajdującego się w lokalizacji `AZURE_AUTH_LOCATION`.</span><span class="sxs-lookup"><span data-stu-id="fbb91-150">First, [auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) is called to load the authentication information from the file located at `AZURE_AUTH_LOCATION`.</span></span> <span data-ttu-id="fbb91-151">Następnie ten plik jest ładowany ręcznie przez funkcję `readJSON` (pominiętą w tym miejscu) w celu ściągnięcia dwóch wartości potrzebnych do uruchomienia pozostałej części programu: identyfikator subskrypcji klienta i wpis tajny jednostki usługi, który jest również używany na potrzeby hasła maszyny wirtualnej.</span><span class="sxs-lookup"><span data-stu-id="fbb91-151">Next, this file is loaded manually by the `readJSON` function (omitted here) to pull the two values needed to run the rest of the program: The subscription ID of the client, and the service principal's secret, which is also used for the VM's password.</span></span>

> [!WARNING]
> <span data-ttu-id="fbb91-152">W celu uproszczenia przewodnika Szybki start zostanie ponownie użyte hasło jednostki usługi.</span><span class="sxs-lookup"><span data-stu-id="fbb91-152">To keep the quickstart simple, the service principal password is reused.</span></span> <span data-ttu-id="fbb91-153">Pamiętaj, aby w środowisku produkcyjnym __nigdy__ nie używać ponownie hasła zapewniającego dostępu do zasobów platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="fbb91-153">In production, take care to __never__ reuse a password which gives access to your Azure resources.</span></span>

### <a name="flow-of-operations-in-main"></a><span data-ttu-id="fbb91-154">Przepływ operacji w funkcji main()</span><span class="sxs-lookup"><span data-stu-id="fbb91-154">Flow of operations in main()</span></span>

<span data-ttu-id="fbb91-155">Funkcja `main` jest prosta — wskazuje tylko przepływ operacji i przeprowadza sprawdzanie błędów.</span><span class="sxs-lookup"><span data-stu-id="fbb91-155">The `main` function is simple, only indicating the flow of operations and performing error-checking.</span></span>

```go
func main() {
    group, err := createGroup()
    if err != nil {
        log.Fatalf("failed to create group: %v", err)
    }
    log.Printf("Created group: %v", *group.Name)

    log.Printf("Starting deployment: %s", deploymentName)
    result, err := createDeployment()
    if err != nil {
        log.Fatalf("Failed to deploy: %v", err)
    }
    if result.Name != nil {
        log.Printf("Completed deployment %v: %v", deploymentName, *result.Properties.ProvisioningState)
    } else {
        log.Printf("Completed deployment %v (no data returned to SDK)", deploymentName)
    }
    getLogin()
}
```

<span data-ttu-id="fbb91-156">Oto działania wykonywane przez kod (z zachowaniem kolejności):</span><span class="sxs-lookup"><span data-stu-id="fbb91-156">The steps that the code runs through are, in order:</span></span>

* <span data-ttu-id="fbb91-157">Utworzenie grupy zasobów, w której ma zostać przeprowadzone wdrażanie (`createGroup`)</span><span class="sxs-lookup"><span data-stu-id="fbb91-157">Create the resource group to deploy to (`createGroup`)</span></span>
* <span data-ttu-id="fbb91-158">Utworzenie wdrożenia w tej grupie (`createDeployment`)</span><span class="sxs-lookup"><span data-stu-id="fbb91-158">Create the deployment within this group (`createDeployment`)</span></span>
* <span data-ttu-id="fbb91-159">Uzyskiwanie i wyświetlanie danych logowania dotyczących wdrożonej maszyny wirtualnej (`getLogin`)</span><span class="sxs-lookup"><span data-stu-id="fbb91-159">Get and display login information for the deployed VM (`getLogin`)</span></span>

### <a name="create-the-resource-group"></a><span data-ttu-id="fbb91-160">Tworzenie grupy zasobów</span><span class="sxs-lookup"><span data-stu-id="fbb91-160">Create the resource group</span></span>

<span data-ttu-id="fbb91-161">Funkcja `createGroup` umożliwia utworzenie grupy zasobów.</span><span class="sxs-lookup"><span data-stu-id="fbb91-161">The `createGroup` function creates the resource group.</span></span> <span data-ttu-id="fbb91-162">Przepływ wywołań i argumenty pozwalają zapoznać się ze strukturą interakcji z usługami w zestawie SDK.</span><span class="sxs-lookup"><span data-stu-id="fbb91-162">Looking at the call flow and arguments demonstrates the way that service interactions are structured in the SDK.</span></span>

```go
func createGroup() (group resources.Group, err error) {
    groupsClient := resources.NewGroupsClient(clientData.SubscriptionID)
    groupsClient.Authorizer = authorizer

        return groupsClient.CreateOrUpdate(
                ctx,
                resourceGroupName,
                resources.Group{
                        Location: to.StringPtr(resourceGroupLocation)})
}
```

<span data-ttu-id="fbb91-163">Ogólny przebieg interakcji z usługą platformy Azure wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="fbb91-163">The general flow of interacting with an Azure service is:</span></span>

* <span data-ttu-id="fbb91-164">Utworzenie klienta przy użyciu metody `service.New*Client()`, gdzie `*` to typ zasobu `service`, z którym ma nastąpić interakcja.</span><span class="sxs-lookup"><span data-stu-id="fbb91-164">Create the client using the `service.New*Client()` method, where `*` is the resource type of the `service` that you want to interact with.</span></span> <span data-ttu-id="fbb91-165">Ta funkcja zawsze pobiera identyfikator subskrypcji.</span><span class="sxs-lookup"><span data-stu-id="fbb91-165">This function always takes a subscription ID.</span></span>
* <span data-ttu-id="fbb91-166">Ustawienie metody autoryzacji klienta z uwzględnieniem interakcji ze zdalnym interfejsem API.</span><span class="sxs-lookup"><span data-stu-id="fbb91-166">Set the authorization method for the client, allowing it to interact with the remote API.</span></span>
* <span data-ttu-id="fbb91-167">Wywołanie metody na kliencie odpowiadającym zdalnemu interfejsowi API.</span><span class="sxs-lookup"><span data-stu-id="fbb91-167">Make the method call on the client corresponding to the remote API.</span></span> <span data-ttu-id="fbb91-168">Metody klienta usługi zazwyczaj pobierają nazwę zasobu i obiektu metadanych.</span><span class="sxs-lookup"><span data-stu-id="fbb91-168">Service client methods usually take the name of the resource and a metadata object.</span></span>

<span data-ttu-id="fbb91-169">Funkcja [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) służy tutaj do przeprowadzania konwersji typów.</span><span class="sxs-lookup"><span data-stu-id="fbb91-169">The [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) function is used to perform a type conversion here.</span></span> <span data-ttu-id="fbb91-170">Parametry metod zestawu SDK przyjmują niemal wyłącznie wskaźniki, dlatego są udostępniane są metody ułatwiające konwersję typów.</span><span class="sxs-lookup"><span data-stu-id="fbb91-170">The parameters for SDK methods almost exclusively take pointers, so convenience methods are provided to make the type conversions easy.</span></span> <span data-ttu-id="fbb91-171">Pełną listę konwerterów i opis ich zachowania można znaleźć w dokumentacji modułu [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to).</span><span class="sxs-lookup"><span data-stu-id="fbb91-171">See the documentation for the [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) module for the complete list of convenience converters and their behavior.</span></span>

<span data-ttu-id="fbb91-172">Metoda `groupsClient.CreateOrUpdate` zwraca wskaźnik do typu danych reprezentującego grupę zasobów.</span><span class="sxs-lookup"><span data-stu-id="fbb91-172">The `groupsClient.CreateOrUpdate` method returns a pointer to a data type representing the resource group.</span></span> <span data-ttu-id="fbb91-173">Bezpośrednia zwracana wartość tego rodzaju wskazuje krótkotrwałą operację, która ma być synchroniczna.</span><span class="sxs-lookup"><span data-stu-id="fbb91-173">A direct return value of this kind indicates a short-running operation that is meant to be synchronous.</span></span> <span data-ttu-id="fbb91-174">W następnej sekcji przedstawimy przykład długotrwałej operacji i sposób interakcji z nią.</span><span class="sxs-lookup"><span data-stu-id="fbb91-174">In the next section, you'll see an example of a long-running operation and how to interact with it.</span></span>

### <a name="perform-the-deployment"></a><span data-ttu-id="fbb91-175">Wykonywanie wdrożenia</span><span class="sxs-lookup"><span data-stu-id="fbb91-175">Perform the deployment</span></span>

<span data-ttu-id="fbb91-176">Po utworzeniu grupy zasobów można uruchomić wdrożenie.</span><span class="sxs-lookup"><span data-stu-id="fbb91-176">Once the resource group is created, it's time to run the deployment.</span></span> <span data-ttu-id="fbb91-177">Ten kod został podzielony na mniejsze części w celu wyróżnienia różnych części logiki kodu.</span><span class="sxs-lookup"><span data-stu-id="fbb91-177">This code is broken up into smaller sections to emphasize different parts of its logic.</span></span>

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
    (*params)["vm_password"] = map[string]string{
        "value": clientData.VMPassword,
    }
        // ...
```

<span data-ttu-id="fbb91-178">Pliki wdrożenia są ładowane przez element `readJSON` (szczegóły zostały tutaj pominięte).</span><span class="sxs-lookup"><span data-stu-id="fbb91-178">The deployment files are loaded by `readJSON`, the details of which are skipped here.</span></span> <span data-ttu-id="fbb91-179">Ta funkcja zwraca typ `*map[string]interface{}` używany podczas tworzenia metadanych dla wywołania wdrażania zasobów.</span><span class="sxs-lookup"><span data-stu-id="fbb91-179">This function returns a `*map[string]interface{}`, the type used in constructing the metadata for the resource deployment call.</span></span> <span data-ttu-id="fbb91-180">Hasło maszyny wirtualnej jest również ustawiane ręcznie w obszarze parametrów wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="fbb91-180">The VM's password is also set manually on the deployment parameters.</span></span>

```go
        // ...

    deploymentsClient := resources.NewDeploymentsClient(clientData.SubscriptionID)
    deploymentsClient.Authorizer = authorizer

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
        return
    }
```

<span data-ttu-id="fbb91-181">Ten kod jest zgodny z tym samym wzorcem używanym podczas tworzenia grupy zasobów.</span><span class="sxs-lookup"><span data-stu-id="fbb91-181">This code follows the same pattern as creating the resource group.</span></span> <span data-ttu-id="fbb91-182">Zostaje utworzony nowy klient (pod warunkiem, że uwierzytelnienie w usłudze Azure jest możliwe), a następnie zostaje wywołana metoda.</span><span class="sxs-lookup"><span data-stu-id="fbb91-182">A new client is created, given the ability to authenticate with Azure, and then a method is called.</span></span>
<span data-ttu-id="fbb91-183">Metoda ma nawet taką samą nazwę (`CreateOrUpdate`) co analogiczna metoda dotycząca grupy zasobów.</span><span class="sxs-lookup"><span data-stu-id="fbb91-183">The method even has the same name (`CreateOrUpdate`) as the corresponding method for resource groups.</span></span> <span data-ttu-id="fbb91-184">Ten wzorzec jest widoczny w zestawie SDK.</span><span class="sxs-lookup"><span data-stu-id="fbb91-184">This pattern is seen throughout the SDK.</span></span>
<span data-ttu-id="fbb91-185">Metody, które mają podobne działanie, mają zwykle taką samą nazwę.</span><span class="sxs-lookup"><span data-stu-id="fbb91-185">Methods that perform similar work normally have the same name.</span></span>

<span data-ttu-id="fbb91-186">Największa różnica dotyczy wartości zwracanej przez metodę `deploymentsClient.CreateOrUpdate`.</span><span class="sxs-lookup"><span data-stu-id="fbb91-186">The biggest difference comes in the return value of the `deploymentsClient.CreateOrUpdate` method.</span></span> <span data-ttu-id="fbb91-187">Ta wartość ma typ [Future](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future) zgodny z [wzorcem projektowym obiektu future](https://en.wikipedia.org/wiki/Futures_and_promises).</span><span class="sxs-lookup"><span data-stu-id="fbb91-187">This value is of the [Future](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future) type, which follows the [future design pattern](https://en.wikipedia.org/wiki/Futures_and_promises).</span></span> <span data-ttu-id="fbb91-188">Obiekty future reprezentują długotrwałą operację na platformie Azure, którą można sondować, anulować lub blokować po ich wykonaniu.</span><span class="sxs-lookup"><span data-stu-id="fbb91-188">Futures represent a long-running operation in Azure that you can poll, cancel, or block on their completion.</span></span>

```go
        //...
    err = deploymentFuture.Future.WaitForCompletion(ctx, deploymentsClient.BaseClient.Client)
    if err != nil {
        return
    }
    return deploymentFuture.Result(deploymentsClient)
}
```

<span data-ttu-id="fbb91-189">W przypadku tego przykładu najlepiej zaczekać na ukończenie operacji.</span><span class="sxs-lookup"><span data-stu-id="fbb91-189">For this example, the best thing to do is to wait for the operation to complete.</span></span> <span data-ttu-id="fbb91-190">Oczekiwanie na obiekt future wymaga zarówno [obiektu kontekstu](https://blog.golang.org/context), jak i klienta, który utworzył obiekt `Future`.</span><span class="sxs-lookup"><span data-stu-id="fbb91-190">Waiting on a future requires both a [context object](https://blog.golang.org/context) and the client that created the `Future`.</span></span> <span data-ttu-id="fbb91-191">Istnieją tutaj dwa możliwe źródła błędu: błąd występujący po stronie klienta podczas próby wywołania metody oraz odpowiedź serwera z informacją o błędzie.</span><span class="sxs-lookup"><span data-stu-id="fbb91-191">There are two possible error sources here: An error caused on the client side when trying to invoke the method, and an error response from the server.</span></span> <span data-ttu-id="fbb91-192">Ta druga jest zwracana jako część wywołania `deploymentFuture.Result`.</span><span class="sxs-lookup"><span data-stu-id="fbb91-192">The latter is returned as part of the `deploymentFuture.Result` call.</span></span>

### <a name="get-the-assigned-ip-address"></a><span data-ttu-id="fbb91-193">Uzyskiwanie przypisanego adresu IP</span><span class="sxs-lookup"><span data-stu-id="fbb91-193">Get the assigned IP address</span></span>

<span data-ttu-id="fbb91-194">Aby można było korzystać z nowo utworzonej maszyny wirtualnej, trzeba mieć przypisany adres IP.</span><span class="sxs-lookup"><span data-stu-id="fbb91-194">To do anything with the newly created VM, you need the assigned IP address.</span></span> <span data-ttu-id="fbb91-195">Adresy IP stanowią oddzielne zasoby platformy Azure powiązane z zasobami kontrolera interfejsu sieciowego.</span><span class="sxs-lookup"><span data-stu-id="fbb91-195">IP addresses are their own separate Azure resource, bound to Network Interface Controller (NIC) resources.</span></span>

```go
func getLogin() {
    params, err := readJSON(parametersFile)
    if err != nil {
        log.Fatalf("Unable to read parameters. Get login information with `az network public-ip list -g %s", resourceGroupName)
    }

    addressClient := network.NewPublicIPAddressesClient(clientData.SubscriptionID)
    addressClient.Authorizer = authorizer
    ipName := (*params)["publicIPAddresses_QuickstartVM_ip_name"].(map[string]interface{})
    ipAddress, err := addressClient.Get(ctx, resourceGroupName, ipName["value"].(string), "")
    if err != nil {
        log.Fatalf("Unable to get IP information. Try using `az network public-ip list -g %s", resourceGroupName)
    }

    vmUser := (*params)["vm_user"].(map[string]interface{})

    log.Printf("Log in with ssh: %s@%s, password: %s",
        vmUser["value"].(string),
        *ipAddress.PublicIPAddressPropertiesFormat.IPAddress,
        clientData.VMPassword)
}
```

<span data-ttu-id="fbb91-196">Ta metoda korzysta z informacji przechowywanych w pliku parametrów.</span><span class="sxs-lookup"><span data-stu-id="fbb91-196">This method relies on information that is stored in the parameters file.</span></span> <span data-ttu-id="fbb91-197">Kod może wysłać zapytanie bezpośrednio do maszyny wirtualnej, aby uzyskać dane jej kontrolera interfejsu sieciowego, wysłać zapytanie do kontrolera interfejsu sieciowego, aby uzyskać jego adres IP, a następnie wysłać zapytanie bezpośrednio do zasobu z danym adresem IP.</span><span class="sxs-lookup"><span data-stu-id="fbb91-197">The code could query the VM directly to get its NIC, query the NIC to get its IP resource, and then query the IP resource directly.</span></span> <span data-ttu-id="fbb91-198">Jest to długi łańcuch zależności. Wymaga on wykonania wielu operacji, przez co jest kosztowny.</span><span class="sxs-lookup"><span data-stu-id="fbb91-198">That's a long chain of dependencies and operations to resolve, making it expensive.</span></span> <span data-ttu-id="fbb91-199">Ponieważ dane JSON są lokalne, można je załadować zamiast wykonywania powyższych operacji.</span><span class="sxs-lookup"><span data-stu-id="fbb91-199">Since the JSON information is local, it can be loaded instead.</span></span>

<span data-ttu-id="fbb91-200">Wartość dla użytkownika maszyny wirtualnej jest również ładowana z kodu JSON.</span><span class="sxs-lookup"><span data-stu-id="fbb91-200">The value for the VM user is also loaded from the JSON.</span></span> <span data-ttu-id="fbb91-201">Hasło maszyny wirtualnej zostało wcześniej załadowane z pliku uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="fbb91-201">The VM password was loaded earlier from the authentication file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fbb91-202">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="fbb91-202">Next steps</span></span>

<span data-ttu-id="fbb91-203">W tym przewodniku Szybki start istniejący szablon został wdrożony za pomocą języka Go.</span><span class="sxs-lookup"><span data-stu-id="fbb91-203">In this quickstart, you took an existing template and deployed it through Go.</span></span> <span data-ttu-id="fbb91-204">Następnie nastąpiło nawiązanie połączenia z nowo utworzoną maszyną wirtualną za pośrednictwem protokołu SSH.</span><span class="sxs-lookup"><span data-stu-id="fbb91-204">Then you connected to the newly created VM via SSH.</span></span>

<span data-ttu-id="fbb91-205">Aby uzyskać więcej informacji o pracy z maszynami wirtualnymi w środowisku platformy Azure z użyciem języka Go, zapoznaj się z [przykładami dotyczącymi obliczeń na platformie Azure dla języka Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) lub [przykładami dotyczącymi zarządzania zasobami na platformie Azure dla języka Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span><span class="sxs-lookup"><span data-stu-id="fbb91-205">To continue learning about working with virtual machines in the Azure environment with Go, take a look at the [Azure compute samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) or [Azure resource management samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span></span>

<span data-ttu-id="fbb91-206">Aby dowiedzieć się więcej na temat metod uwierzytelniania dostępnych w zestawie SDK oraz obsługiwanych przez typów uwierzytelniania, zobacz [Uwierzytelnianie przy użyciu zestawu Azure SDK dla języka Go](azure-sdk-go-authorization.md).</span><span class="sxs-lookup"><span data-stu-id="fbb91-206">To learn more about the available authentication methods in the SDK, and which authentication types they support, see [Authentication with the Azure SDK for Go](azure-sdk-go-authorization.md).</span></span>
