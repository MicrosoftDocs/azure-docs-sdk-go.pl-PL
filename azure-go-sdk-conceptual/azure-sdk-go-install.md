---
title: Instalowanie zestawu Azure SDK dla języka Go
description: Jak zainstalować i skonfigurować zestaw Azure SDK dla języka Go oraz zapewnić w nim zasoby.
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 03/14/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.openlocfilehash: 8423b3fedd89e57662bf96f777a5b30926914da9
ms.sourcegitcommit: b81b17cbb934399c195bfdcb87137aee935f5234
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34755519"
---
# <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="44249-103">Instalowanie zestawu Azure SDK dla języka Go</span><span class="sxs-lookup"><span data-stu-id="44249-103">Install the Azure SDK for Go</span></span>

<span data-ttu-id="44249-104">Zestaw Azure SDK dla języka Go — Zapraszamy!</span><span class="sxs-lookup"><span data-stu-id="44249-104">Welcome to the Azure SDK for Go!</span></span> <span data-ttu-id="44249-105">Zestaw SDK umożliwia zarządzanie usługami platformy Azure z poziomu aplikacji w języku Go, a także interakcje z tymi usługami.</span><span class="sxs-lookup"><span data-stu-id="44249-105">The SDK allows you to manage and interact with Azure services from your Go applications.</span></span>

## <a name="get-the-azure-sdk-for-go"></a><span data-ttu-id="44249-106">Pobieranie zestawu Azure SDK dla języka Go</span><span class="sxs-lookup"><span data-stu-id="44249-106">Get the Azure SDK for Go</span></span>

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

<span data-ttu-id="44249-107">Niektóre usługi platformy Azure mają własne zestawy SDK dla języka Go, których nie ma w podstawowym pakiecie zestawu Azure SDK dla języka Go.</span><span class="sxs-lookup"><span data-stu-id="44249-107">Some Azure services have their own Go SDK and aren't included in the core Azure SDK for Go package.</span></span> <span data-ttu-id="44249-108">W poniższej tabeli wymieniono usługi z własnymi zestawami SDK i ich nazwy pakietów.</span><span class="sxs-lookup"><span data-stu-id="44249-108">The following table lists the services with their own SDKs and their package names.</span></span> <span data-ttu-id="44249-109">Wszystkie te pakiety są uważane za będące w wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="44249-109">These packages are all considered to be in preview.</span></span>

| <span data-ttu-id="44249-110">Usługa</span><span class="sxs-lookup"><span data-stu-id="44249-110">Service</span></span> | <span data-ttu-id="44249-111">Pakiet</span><span class="sxs-lookup"><span data-stu-id="44249-111">Package</span></span> |
|---------|---------|
| <span data-ttu-id="44249-112">Blob Storage</span><span class="sxs-lookup"><span data-stu-id="44249-112">Blob Storage</span></span> | [<span data-ttu-id="44249-113">github.com/Azure/azure-storage-blob-go</span><span class="sxs-lookup"><span data-stu-id="44249-113">github.com/Azure/azure-storage-blob-go</span></span>](https://github.com/Azure/azure-storage-blob-go) |
| <span data-ttu-id="44249-114">File Storage</span><span class="sxs-lookup"><span data-stu-id="44249-114">File Storage</span></span> | [<span data-ttu-id="44249-115">github.com/Azure/azure-storage-file-go</span><span class="sxs-lookup"><span data-stu-id="44249-115">github.com/Azure/azure-storage-file-go</span></span>](https://github.com/Azure/azure-storage-file-go) |
| <span data-ttu-id="44249-116">Centrum zdarzeń</span><span class="sxs-lookup"><span data-stu-id="44249-116">Event Hub</span></span> | [<span data-ttu-id="44249-117">github.com/Azure/azure-event-hubs-go</span><span class="sxs-lookup"><span data-stu-id="44249-117">github.com/Azure/azure-event-hubs-go</span></span>](https://github.com/Azure/azure-event-hubs-go) |
| <span data-ttu-id="44249-118">Application Insights</span><span class="sxs-lookup"><span data-stu-id="44249-118">Application Insights</span></span> | [<span data-ttu-id="44249-119">github.com/Microsoft/ApplicationInsights-go</span><span class="sxs-lookup"><span data-stu-id="44249-119">github.com/Microsoft/ApplicationInsights-go</span></span>](https://github.com/Microsoft/ApplicationInsights-go) |

## <a name="vendor-the-azure-sdk-for-go"></a><span data-ttu-id="44249-120">Zapewnianie zasobów zestawu Azure SDK dla języka Go</span><span class="sxs-lookup"><span data-stu-id="44249-120">Vendor the Azure SDK for Go</span></span>

<span data-ttu-id="44249-121">Zestaw Azure SDK dla języka Go umożliwia zapewnianie zasobów za pośrednictwem narzędzia [dep](https://github.com/golang/dep).</span><span class="sxs-lookup"><span data-stu-id="44249-121">The Azure SDK for Go may be vendored through [dep](https://github.com/golang/dep).</span></span> <span data-ttu-id="44249-122">Ze względu na stabilność zapewnianie zasobów jest zalecane.</span><span class="sxs-lookup"><span data-stu-id="44249-122">For stability reasons, vendoring is recommended.</span></span> <span data-ttu-id="44249-123">Aby można było korzystać z narzędzia `dep`, dodaj ciąg `github.com/Azure/azure-sdk-for-go` do sekcji `[[constraint]]` w pliku `Gopkg.toml`.</span><span class="sxs-lookup"><span data-stu-id="44249-123">In order to use `dep` support, add `github.com/Azure/azure-sdk-for-go` to a `[[constraint]]` section of your `Gopkg.toml`.</span></span> <span data-ttu-id="44249-124">Aby na przykład zapewnić zasoby z wersji `14.0.0`, dodaj następujący wpis:</span><span class="sxs-lookup"><span data-stu-id="44249-124">For example, to vendor on version `14.0.0`, add the following entry:</span></span>

```
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="include-the-azure-sdk-for-go-in-your-project"></a><span data-ttu-id="44249-125">Uwzględnianie w projekcie zestawu Azure SDK dla języka Go</span><span class="sxs-lookup"><span data-stu-id="44249-125">Include the Azure SDK for Go in your project</span></span>

<span data-ttu-id="44249-126">Aby zapewnić możliwość korzystania z usług platformy Azure z poziomu kodu Go, zaimportuj wszystkie usługi, z którymi wchodzisz w interakcje, a także wymagane moduły `autorest`.</span><span class="sxs-lookup"><span data-stu-id="44249-126">To use Azure services from your Go code, import any services you interact with and the required `autorest` modules.</span></span>
<span data-ttu-id="44249-127">Pełną listę dostępnych modułów można uzyskać w dokumentacji GoDoc dotyczącej [dostępnych usług](https://godoc.org/github.com/Azure/azure-sdk-for-go) i [pakietów AutoRest](https://godoc.org/github.com/Azure/go-autorest).</span><span class="sxs-lookup"><span data-stu-id="44249-127">You get a complete list of the available modules from GoDoc for [available services](https://godoc.org/github.com/Azure/azure-sdk-for-go) and [AutoRest packages](https://godoc.org/github.com/Azure/go-autorest).</span></span> <span data-ttu-id="44249-128">Najpopularniejsze pakiety z elementu `go-autorest`, których potrzebujesz, to:</span><span class="sxs-lookup"><span data-stu-id="44249-128">The most common packages you need from `go-autorest` are:</span></span>

| <span data-ttu-id="44249-129">Pakiet</span><span class="sxs-lookup"><span data-stu-id="44249-129">Package</span></span> | <span data-ttu-id="44249-130">Opis</span><span class="sxs-lookup"><span data-stu-id="44249-130">Description</span></span> |
|---------|-------------|
| <span data-ttu-id="44249-131">[github.com/Azure/go-autorest/autorest][autorest]</span><span class="sxs-lookup"><span data-stu-id="44249-131">[github.com/Azure/go-autorest/autorest][autorest]</span></span> | <span data-ttu-id="44249-132">Obiekty zapewniające obsługę uwierzytelniania klientów w usłudze</span><span class="sxs-lookup"><span data-stu-id="44249-132">Objects for handling service client authentication</span></span> |
| <span data-ttu-id="44249-133">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span><span class="sxs-lookup"><span data-stu-id="44249-133">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span></span> | <span data-ttu-id="44249-134">Stałe zapewniające interakcje z usługami platformy Azure</span><span class="sxs-lookup"><span data-stu-id="44249-134">Constants for interactions with Azure services</span></span> |
| <span data-ttu-id="44249-135">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span><span class="sxs-lookup"><span data-stu-id="44249-135">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span></span> | <span data-ttu-id="44249-136">Mechanizmy uwierzytelniania dostępu do usług platformy Azure</span><span class="sxs-lookup"><span data-stu-id="44249-136">Authentication mechanisms for accessing Azure services</span></span> |
| <span data-ttu-id="44249-137">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span><span class="sxs-lookup"><span data-stu-id="44249-137">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span></span> | <span data-ttu-id="44249-138">Pomocnicy asercji typów ułatwiający pracę ze strukturami danych zestawu Azure SDK</span><span class="sxs-lookup"><span data-stu-id="44249-138">Type assertion helpers for working with Azure SDK data structures</span></span> |

[autorest]: https://godoc.org/github.com/Azure/go-autorest/autorest
[autorest/azure]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure
[autorest/adal]: https://godoc.org/github.com/Azure/go-autorest/autorest/adal
[autorest/to]: https://godoc.org/github.com/Azure/go-autorest/autorest/to

<span data-ttu-id="44249-139">Moduły usług platformy Azure mają numery wersji niezależne od przeznaczonych dla nich interfejsów API zestawu SDK.</span><span class="sxs-lookup"><span data-stu-id="44249-139">Modules for Azure services are versioned independently from the SDK APIs for them.</span></span> <span data-ttu-id="44249-140">Wersje te stanowią część ścieżki importu modułu i pochodzą od _wersji usługi_ lub _profilu_.</span><span class="sxs-lookup"><span data-stu-id="44249-140">These versions are part of the module import path, and are from either a _service version_ or a _profile_.</span></span> <span data-ttu-id="44249-141">Obecnie zalecamy używanie określonej wersji usługi zarówno na etapie tworzenia, jak i udostępniania.</span><span class="sxs-lookup"><span data-stu-id="44249-141">Currently, it is recommended that you use a specific service version for both development and release.</span></span> <span data-ttu-id="44249-142">Usługi znajdują się w module `services`.</span><span class="sxs-lookup"><span data-stu-id="44249-142">Services are located under the `services` module.</span></span> <span data-ttu-id="44249-143">Pełna ścieżka importu obejmuje kolejno nazwę usługi, wersję w formacie `YYYY-MM-DD` i ponownie nazwę usługi.</span><span class="sxs-lookup"><span data-stu-id="44249-143">The full path for the import is the name of the service, followed by the version in `YYYY-MM-DD` format, followed by the service name again.</span></span> <span data-ttu-id="44249-144">Na przykład, aby uwzględnić wersję `2017-03-30` usługi Compute:</span><span class="sxs-lookup"><span data-stu-id="44249-144">For example, to include the `2017-03-30` version of the Compute service:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

<span data-ttu-id="44249-145">Obecnie zalecamy używanie najnowszej wersji usługi, chyba że istnieje powód, aby tego nie robić.</span><span class="sxs-lookup"><span data-stu-id="44249-145">Right now it is recommended that you use the latest version of a service, unless you have a reason to do otherwise.</span></span>

<span data-ttu-id="44249-146">Jeśli jest potrzebna zbiorcza migawka usług, możesz również wybrać jedną wersję profilu.</span><span class="sxs-lookup"><span data-stu-id="44249-146">If you need a collective snapshot of services, you can also select a single profile version.</span></span> <span data-ttu-id="44249-147">Obecnie jedynym zablokowanym profilem jest wersja `2017-03-09`, która może nie obejmować najnowszych funkcji usługi.</span><span class="sxs-lookup"><span data-stu-id="44249-147">Right now, the only locked profile is version `2017-03-09`, which may not have the latest features of services.</span></span> <span data-ttu-id="44249-148">Profile znajdują się w module `profiles` i mają wersję w formacie `YYYY-MM-DD`.</span><span class="sxs-lookup"><span data-stu-id="44249-148">Profiles are located under the `profiles` module, with their version in the `YYYY-MM-DD` format.</span></span> <span data-ttu-id="44249-149">Usługi są podzielone na grupy według wersji profilu.</span><span class="sxs-lookup"><span data-stu-id="44249-149">Services are grouped under their profile version.</span></span> <span data-ttu-id="44249-150">Aby na przykład zaimportować moduł zarządzania zasobami platformy Azure z profilu `2017-03-09`:</span><span class="sxs-lookup"><span data-stu-id="44249-150">For example, to import the Azure Resources management module from the `2017-03-09` profile:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/resources/mgmt/resources"
```

> [!WARNING]
> <span data-ttu-id="44249-151">Dostępne są także profile `preview` i `latest`.</span><span class="sxs-lookup"><span data-stu-id="44249-151">There are also `preview` and `latest` profiles available.</span></span> <span data-ttu-id="44249-152">Ich użycie nie jest zalecane.</span><span class="sxs-lookup"><span data-stu-id="44249-152">Using them is not recommended.</span></span> <span data-ttu-id="44249-153">Te profile są wersjami ruchomymi, co znaczy, że zachowanie usługi może w dowolnym momencie ulec zmianie.</span><span class="sxs-lookup"><span data-stu-id="44249-153">These profiles are rolling versions and service behavior may change at any time.</span></span>

## <a name="next-steps"></a><span data-ttu-id="44249-154">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="44249-154">Next steps</span></span>

<span data-ttu-id="44249-155">Aby rozpocząć korzystanie z zestawu Azure SDK dla języka Go, wypróbuj szybki start.</span><span class="sxs-lookup"><span data-stu-id="44249-155">To begin using the Azure SDK for Go, try out a quickstart.</span></span>

* [<span data-ttu-id="44249-156">Wdrażanie maszyny wirtualnej na podstawie szablonu</span><span class="sxs-lookup"><span data-stu-id="44249-156">Deploy a virtual machine from a template</span></span>](azure-sdk-go-qs-vm.md)
* [<span data-ttu-id="44249-157">Transfer objects to Azure Blob Storage with the Azure Blob SDK for Go (Przenoszenie obiektów do usługi Azure Blob Storage przy użyciu zestawu Azure Blob SDK dla języka Go)</span><span class="sxs-lookup"><span data-stu-id="44249-157">Transfer objects to Azure Blob Storage with the Azure Blob SDK for Go</span></span>](/azure/storage/blobs/storage-quickstart-blobs-go?toc=%2fgo%2fazure%2ftoc.json)
* [<span data-ttu-id="44249-158">Łączenie się z usługą Azure Database for PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="44249-158">Connect to Azure Database for PostgreSQL</span></span>](/azure/postgresql/connect-go?toc=%2fgo%2fazure%2ftoc.json)

<span data-ttu-id="44249-159">Jeśli chcesz od razu rozpocząć pracę z innymi usługami w zestawie SDK dla języka Go, przyjrzyj się udostępnionym przykładom kodu.</span><span class="sxs-lookup"><span data-stu-id="44249-159">If you want to get started with other services in the Go SDK immediately, take a look at some of the available sample code.</span></span>

* [<span data-ttu-id="44249-160">Uwierzytelnianie za pomocą usług platformy Azure</span><span class="sxs-lookup"><span data-stu-id="44249-160">Authenticate with Azure services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/iam)
* [<span data-ttu-id="44249-161">Wdrażanie nowych maszyn wirtualnych przy użyciu uwierzytelniania SSH</span><span class="sxs-lookup"><span data-stu-id="44249-161">Deploy new virtual machines with SSH authentication</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [<span data-ttu-id="44249-162">Wdrażanie obrazu kontenera w usłudze Azure Container Instances</span><span class="sxs-lookup"><span data-stu-id="44249-162">Deploy a container image to Azure Container Instances</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerinstance)
* [<span data-ttu-id="44249-163">Tworzenie klastra w usłudze Azure Kubernetes Service</span><span class="sxs-lookup"><span data-stu-id="44249-163">Create a cluster in Azure Kubernetes Service</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerservice)
* [<span data-ttu-id="44249-164">Praca z usługami Azure Storage</span><span class="sxs-lookup"><span data-stu-id="44249-164">Work with Azure Storage services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage)
* [<span data-ttu-id="44249-165">Wszystkie przykłady dotyczące zestawu Azure SDK dla języka Go</span><span class="sxs-lookup"><span data-stu-id="44249-165">All samples for the Azure SDK for Go</span></span>](https://github.com/azure-samples/azure-sdk-for-go-samples)
