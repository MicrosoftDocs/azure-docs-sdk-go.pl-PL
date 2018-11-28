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
ms.openlocfilehash: 7990ec8bde5622078aa822fc7e66ba5c4384d682
ms.sourcegitcommit: 3d26b464f196f8675c636ae792637d4c882fb92c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/27/2018
ms.locfileid: "52337147"
---
# <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="5af85-103">Instalowanie zestawu Azure SDK dla języka Go</span><span class="sxs-lookup"><span data-stu-id="5af85-103">Install the Azure SDK for Go</span></span>

<span data-ttu-id="5af85-104">Zestaw Azure SDK dla języka Go — Zapraszamy!</span><span class="sxs-lookup"><span data-stu-id="5af85-104">Welcome to the Azure SDK for Go!</span></span> <span data-ttu-id="5af85-105">Zestaw SDK umożliwia zarządzanie usługami platformy Azure z poziomu aplikacji w języku Go, a także interakcje z tymi usługami.</span><span class="sxs-lookup"><span data-stu-id="5af85-105">The SDK allows you to manage and interact with Azure services from your Go applications.</span></span>

## <a name="get-the-azure-sdk-for-go"></a><span data-ttu-id="5af85-106">Pobieranie zestawu Azure SDK dla języka Go</span><span class="sxs-lookup"><span data-stu-id="5af85-106">Get the Azure SDK for Go</span></span>

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

<span data-ttu-id="5af85-107">Niektóre usługi platformy Azure mają własne zestawy SDK dla języka Go, których nie ma w podstawowym pakiecie zestawu Azure SDK dla języka Go.</span><span class="sxs-lookup"><span data-stu-id="5af85-107">Some Azure services have their own Go SDK and aren't included in the core Azure SDK for Go package.</span></span> <span data-ttu-id="5af85-108">W poniższej tabeli wymieniono usługi z własnymi zestawami SDK i ich nazwy pakietów.</span><span class="sxs-lookup"><span data-stu-id="5af85-108">The following table lists the services with their own SDKs and their package names.</span></span> <span data-ttu-id="5af85-109">Wszystkie te pakiety są uważane za będące w wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="5af85-109">These packages are all considered to be in preview.</span></span>

| <span data-ttu-id="5af85-110">Usługa</span><span class="sxs-lookup"><span data-stu-id="5af85-110">Service</span></span> | <span data-ttu-id="5af85-111">Pakiet</span><span class="sxs-lookup"><span data-stu-id="5af85-111">Package</span></span> |
|---------|---------|
| <span data-ttu-id="5af85-112">Blob Storage</span><span class="sxs-lookup"><span data-stu-id="5af85-112">Blob Storage</span></span> | [<span data-ttu-id="5af85-113">github.com/Azure/azure-storage-blob-go</span><span class="sxs-lookup"><span data-stu-id="5af85-113">github.com/Azure/azure-storage-blob-go</span></span>](https://github.com/Azure/azure-storage-blob-go) |
| <span data-ttu-id="5af85-114">File Storage</span><span class="sxs-lookup"><span data-stu-id="5af85-114">File Storage</span></span> | [<span data-ttu-id="5af85-115">github.com/Azure/azure-storage-file-go</span><span class="sxs-lookup"><span data-stu-id="5af85-115">github.com/Azure/azure-storage-file-go</span></span>](https://github.com/Azure/azure-storage-file-go) |
| <span data-ttu-id="5af85-116">Kolejka magazynu</span><span class="sxs-lookup"><span data-stu-id="5af85-116">Storage Queue</span></span> | [<span data-ttu-id="5af85-117">github.com/Azure/azure-storage-queue-go</span><span class="sxs-lookup"><span data-stu-id="5af85-117">github.com/Azure/azure-storage-queue-go</span></span>](https://github.com/Azure/azure-storage-queue-go) |
| <span data-ttu-id="5af85-118">Centrum zdarzeń</span><span class="sxs-lookup"><span data-stu-id="5af85-118">Event Hub</span></span> | [<span data-ttu-id="5af85-119">github.com/Azure/azure-event-hubs-go</span><span class="sxs-lookup"><span data-stu-id="5af85-119">github.com/Azure/azure-event-hubs-go</span></span>](https://github.com/Azure/azure-event-hubs-go) |
| <span data-ttu-id="5af85-120">Service Bus</span><span class="sxs-lookup"><span data-stu-id="5af85-120">Service Bus</span></span> | [<span data-ttu-id="5af85-121">github.com/Azure/azure-service-bus-go</span><span class="sxs-lookup"><span data-stu-id="5af85-121">github.com/Azure/azure-service-bus-go</span></span>](https://github.com/Azure/azure-service-bus-go) |
| <span data-ttu-id="5af85-122">Application Insights</span><span class="sxs-lookup"><span data-stu-id="5af85-122">Application Insights</span></span> | [<span data-ttu-id="5af85-123">github.com/Microsoft/ApplicationInsights-go</span><span class="sxs-lookup"><span data-stu-id="5af85-123">github.com/Microsoft/ApplicationInsights-go</span></span>](https://github.com/Microsoft/ApplicationInsights-go) |

## <a name="vendor-the-azure-sdk-for-go"></a><span data-ttu-id="5af85-124">Zapewnianie zasobów zestawu Azure SDK dla języka Go</span><span class="sxs-lookup"><span data-stu-id="5af85-124">Vendor the Azure SDK for Go</span></span>

<span data-ttu-id="5af85-125">Zestaw Azure SDK dla języka Go umożliwia zapewnianie zasobów za pośrednictwem narzędzia [dep](https://github.com/golang/dep).</span><span class="sxs-lookup"><span data-stu-id="5af85-125">The Azure SDK for Go may be vendored through [dep](https://github.com/golang/dep).</span></span> <span data-ttu-id="5af85-126">Ze względu na stabilność zapewnianie zasobów jest zalecane.</span><span class="sxs-lookup"><span data-stu-id="5af85-126">For stability reasons, vendoring is recommended.</span></span> <span data-ttu-id="5af85-127">Aby użyć narzędzia `dep` we własnym projekcie, dodaj element `github.com/Azure/azure-sdk-for-go` do sekcji `[[constraint]]` Twojego pliku `Gopkg.toml`.</span><span class="sxs-lookup"><span data-stu-id="5af85-127">To use `dep` in your own project, add `github.com/Azure/azure-sdk-for-go` to a `[[constraint]]` section of your `Gopkg.toml`.</span></span> <span data-ttu-id="5af85-128">Aby na przykład zapewnić zasoby z wersji `14.0.0`, dodaj następujący wpis:</span><span class="sxs-lookup"><span data-stu-id="5af85-128">For example, to vendor on version `14.0.0`, add the following entry:</span></span>

```toml
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="include-the-azure-sdk-for-go-in-your-project"></a><span data-ttu-id="5af85-129">Uwzględnianie w projekcie zestawu Azure SDK dla języka Go</span><span class="sxs-lookup"><span data-stu-id="5af85-129">Include the Azure SDK for Go in your project</span></span>

<span data-ttu-id="5af85-130">Aby zapewnić możliwość korzystania z usług platformy Azure z poziomu kodu Go, zaimportuj wszystkie usługi, z którymi wchodzisz w interakcje, a także wymagane moduły `autorest`.</span><span class="sxs-lookup"><span data-stu-id="5af85-130">To use Azure services from your Go code, import any services you interact with and the required `autorest` modules.</span></span>
<span data-ttu-id="5af85-131">Pełną listę dostępnych modułów można uzyskać w dokumentacji GoDoc dotyczącej [dostępnych usług](https://godoc.org/github.com/Azure/azure-sdk-for-go) i [pakietów AutoRest](https://godoc.org/github.com/Azure/go-autorest).</span><span class="sxs-lookup"><span data-stu-id="5af85-131">You get a complete list of the available modules from GoDoc for [available services](https://godoc.org/github.com/Azure/azure-sdk-for-go) and [AutoRest packages](https://godoc.org/github.com/Azure/go-autorest).</span></span> <span data-ttu-id="5af85-132">Najpopularniejsze pakiety z elementu `go-autorest`, których potrzebujesz, to:</span><span class="sxs-lookup"><span data-stu-id="5af85-132">The most common packages you need from `go-autorest` are:</span></span>

| <span data-ttu-id="5af85-133">Pakiet</span><span class="sxs-lookup"><span data-stu-id="5af85-133">Package</span></span> | <span data-ttu-id="5af85-134">Opis</span><span class="sxs-lookup"><span data-stu-id="5af85-134">Description</span></span> |
|---------|-------------|
| <span data-ttu-id="5af85-135">[github.com/Azure/go-autorest/autorest][autorest]</span><span class="sxs-lookup"><span data-stu-id="5af85-135">[github.com/Azure/go-autorest/autorest][autorest]</span></span> | <span data-ttu-id="5af85-136">Obiekty zapewniające obsługę uwierzytelniania klientów w usłudze</span><span class="sxs-lookup"><span data-stu-id="5af85-136">Objects for handling service client authentication</span></span> |
| <span data-ttu-id="5af85-137">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span><span class="sxs-lookup"><span data-stu-id="5af85-137">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span></span> | <span data-ttu-id="5af85-138">Stałe zapewniające interakcje z usługami platformy Azure</span><span class="sxs-lookup"><span data-stu-id="5af85-138">Constants for interactions with Azure services</span></span> |
| <span data-ttu-id="5af85-139">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span><span class="sxs-lookup"><span data-stu-id="5af85-139">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span></span> | <span data-ttu-id="5af85-140">Mechanizmy uwierzytelniania dostępu do usług platformy Azure</span><span class="sxs-lookup"><span data-stu-id="5af85-140">Authentication mechanisms for accessing Azure services</span></span> |
| <span data-ttu-id="5af85-141">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span><span class="sxs-lookup"><span data-stu-id="5af85-141">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span></span> | <span data-ttu-id="5af85-142">Pomocnicy asercji typów ułatwiający pracę ze strukturami danych zestawu Azure SDK</span><span class="sxs-lookup"><span data-stu-id="5af85-142">Type assertion helpers for working with Azure SDK data structures</span></span> |

[autorest]: https://godoc.org/github.com/Azure/go-autorest/autorest
[autorest/azure]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure
[autorest/adal]: https://godoc.org/github.com/Azure/go-autorest/autorest/adal
[autorest/to]: https://godoc.org/github.com/Azure/go-autorest/autorest/to

<span data-ttu-id="5af85-143">Wersje pakietów języka Go i usług platformy Azure są określane niezależnie.</span><span class="sxs-lookup"><span data-stu-id="5af85-143">Go packages and Azure services are versioned independently.</span></span> <span data-ttu-id="5af85-144">Wersje usługi są częścią ścieżki importu modułu — za modułem `services`.</span><span class="sxs-lookup"><span data-stu-id="5af85-144">The service versions are part of the module import path, underneath the `services` module.</span></span> <span data-ttu-id="5af85-145">Pełna ścieżka modułu obejmuje kolejno nazwę usługi, wersję w formacie `YYYY-MM-DD` i ponownie nazwę usługi.</span><span class="sxs-lookup"><span data-stu-id="5af85-145">The full path for the module is the name of the service, followed by the version in `YYYY-MM-DD` format, followed by the service name again.</span></span> <span data-ttu-id="5af85-146">Przykład importu wersji `2017-03-30` usługi Compute:</span><span class="sxs-lookup"><span data-stu-id="5af85-146">For example, to import the `2017-03-30` version of the Compute service:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

<span data-ttu-id="5af85-147">Zaleca się użycie najnowszej wersji usługi w momencie rozpoczęcia programowania i stosowanie jej spójnie.</span><span class="sxs-lookup"><span data-stu-id="5af85-147">It's recommended that you use the latest version of a service when starting development and keep it consistent.</span></span>
<span data-ttu-id="5af85-148">Wymagania dotyczące usług mogą zmieniać się w zależności od wersji, co może uszkodzić kod, nawet jeśli nie zostaną zainstalowane żadne aktualizacje zestawu SDK dla języka Go w tym czasie.</span><span class="sxs-lookup"><span data-stu-id="5af85-148">Service requirements may change between versions that could break your code, even if there are no Go SDK updates during that time.</span></span>

<span data-ttu-id="5af85-149">Jeśli jest potrzebna zbiorcza migawka usług, możesz również wybrać jedną wersję profilu.</span><span class="sxs-lookup"><span data-stu-id="5af85-149">If you need a collective snapshot of services, you can also select a single profile version.</span></span> <span data-ttu-id="5af85-150">Obecnie jedynym zablokowanym profilem jest wersja `2017-03-09`, która może nie obejmować najnowszych funkcji usługi.</span><span class="sxs-lookup"><span data-stu-id="5af85-150">Right now, the only locked profile is version `2017-03-09`, which may not have the latest features of services.</span></span> <span data-ttu-id="5af85-151">Profile znajdują się w module `profiles` i mają wersję w formacie `YYYY-MM-DD`.</span><span class="sxs-lookup"><span data-stu-id="5af85-151">Profiles are located under the `profiles` module, with their version in the `YYYY-MM-DD` format.</span></span> <span data-ttu-id="5af85-152">Usługi są podzielone na grupy według wersji profilu.</span><span class="sxs-lookup"><span data-stu-id="5af85-152">Services are grouped under their profile version.</span></span> <span data-ttu-id="5af85-153">Aby na przykład zaimportować moduł zarządzania zasobami platformy Azure z profilu `2017-03-09`:</span><span class="sxs-lookup"><span data-stu-id="5af85-153">For example, to import the Azure Resources management module from the `2017-03-09` profile:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/resources/mgmt/resources"
```

> [!WARNING]
> <span data-ttu-id="5af85-154">Dostępne są także profile `preview` i `latest`.</span><span class="sxs-lookup"><span data-stu-id="5af85-154">There are also `preview` and `latest` profiles available.</span></span> <span data-ttu-id="5af85-155">Ich użycie nie jest zalecane.</span><span class="sxs-lookup"><span data-stu-id="5af85-155">Using them is not recommended.</span></span> <span data-ttu-id="5af85-156">Te profile są wersjami ruchomymi, co znaczy, że zachowanie usługi może w dowolnym momencie ulec zmianie.</span><span class="sxs-lookup"><span data-stu-id="5af85-156">These profiles are rolling versions and service behavior may change at any time.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5af85-157">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="5af85-157">Next steps</span></span>

<span data-ttu-id="5af85-158">Aby rozpocząć korzystanie z zestawu Azure SDK dla języka Go, wypróbuj szybki start.</span><span class="sxs-lookup"><span data-stu-id="5af85-158">To begin using the Azure SDK for Go, try out a quickstart.</span></span>

* [<span data-ttu-id="5af85-159">Wdrażanie maszyny wirtualnej na podstawie szablonu</span><span class="sxs-lookup"><span data-stu-id="5af85-159">Deploy a virtual machine from a template</span></span>](azure-sdk-go-qs-vm.md)
* [<span data-ttu-id="5af85-160">Transfer objects to Azure Blob Storage with the Azure Blob SDK for Go (Przenoszenie obiektów do usługi Azure Blob Storage przy użyciu zestawu Azure Blob SDK dla języka Go)</span><span class="sxs-lookup"><span data-stu-id="5af85-160">Transfer objects to Azure Blob Storage with the Azure Blob SDK for Go</span></span>](/azure/storage/blobs/storage-quickstart-blobs-go?toc=%2fgo%2fazure%2ftoc.json)
* [<span data-ttu-id="5af85-161">Łączenie się z usługą Azure Database for PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="5af85-161">Connect to Azure Database for PostgreSQL</span></span>](/azure/postgresql/connect-go?toc=%2fgo%2fazure%2ftoc.json)

<span data-ttu-id="5af85-162">Jeśli chcesz od razu rozpocząć pracę z innymi usługami w zestawie SDK dla języka Go, przyjrzyj się udostępnionym przykładom kodu.</span><span class="sxs-lookup"><span data-stu-id="5af85-162">If you want to get started with other services in the Go SDK immediately, take a look at some of the available sample code.</span></span>

* [<span data-ttu-id="5af85-163">Uwierzytelnianie za pomocą usług platformy Azure</span><span class="sxs-lookup"><span data-stu-id="5af85-163">Authenticate with Azure services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/internal/iam)
* [<span data-ttu-id="5af85-164">Wdrażanie nowych maszyn wirtualnych przy użyciu uwierzytelniania SSH</span><span class="sxs-lookup"><span data-stu-id="5af85-164">Deploy new virtual machines with SSH authentication</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [<span data-ttu-id="5af85-165">Wdrażanie obrazu kontenera w usłudze Azure Container Instances</span><span class="sxs-lookup"><span data-stu-id="5af85-165">Deploy a container image to Azure Container Instances</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerinstance)
* [<span data-ttu-id="5af85-166">Tworzenie klastra w usłudze Azure Kubernetes Service</span><span class="sxs-lookup"><span data-stu-id="5af85-166">Create a cluster in Azure Kubernetes Service</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerservice)
* [<span data-ttu-id="5af85-167">Praca z usługami Azure Storage</span><span class="sxs-lookup"><span data-stu-id="5af85-167">Work with Azure Storage services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage)
* [<span data-ttu-id="5af85-168">Wszystkie przykłady dotyczące zestawu Azure SDK dla języka Go</span><span class="sxs-lookup"><span data-stu-id="5af85-168">All samples for the Azure SDK for Go</span></span>](https://github.com/azure-samples/azure-sdk-for-go-samples)
