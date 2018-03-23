---
title: Narzędzia dla deweloperów języka Go
description: Narzędzia do pracy z zestawem Azure SDK dla Go i usługami platformy Azure
author: sptramer
ms.author: sttramer
ms.date: 01/30/2018
ms.topic: article
ms.devlang: go
manager: carmonm
ms.openlocfilehash: 054965eb1ea4f1a7556e2968dfbe07b2db69d26f
ms.sourcegitcommit: fcc1786d59d2e32c97a9a8e0748e06f564a961bd
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/23/2018
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a><span data-ttu-id="b8615-103">Narzędzia dla deweloperów korzystających z zestawu Azure SDK dla języka Go</span><span class="sxs-lookup"><span data-stu-id="b8615-103">Tools for developers using the Azure SDK for Go</span></span>

<span data-ttu-id="b8615-104">Oto kilka zalecanych narzędzi, które pomagają w sprawnym tworzeniu kodu w języku Go i jego bezproblemowym współdziałaniu z usługami platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="b8615-104">To be effective at writing Go code and have it work seamlessly with Azure services, here are some recommended tools.</span></span>

## <a name="azure-cli-20"></a><span data-ttu-id="b8615-105">Interfejs wiersza polecenia platformy Azure 2.0</span><span class="sxs-lookup"><span data-stu-id="b8615-105">Azure CLI 2.0</span></span>

<span data-ttu-id="b8615-106">Interfejs wiersza polecenia platformy Azure 2.0 umożliwia tworzenie i konfigurowanie zasobów platformy Azure w ramach posiadanych subskrypcji.</span><span class="sxs-lookup"><span data-stu-id="b8615-106">The Azure 2.0 CLI provides a command-line interface to create and configure Azure resources in your subscriptions.</span></span> <span data-ttu-id="b8615-107">Interfejs wiersza polecenia ułatwia szybkie rozpoczęcie tworzenia typowych udostępnionych zasobów platformy Azure, dzięki czemu można się skupić na bardziej złożonych zastosowaniach usług.</span><span class="sxs-lookup"><span data-stu-id="b8615-107">The CLI can help you get started building common and shared Azure resources quickly, so that you can focus on more complex usage of services.</span></span> <span data-ttu-id="b8615-108">Interfejs wiersza polecenia obejmuje funkcje zapytań i filtrowania umożliwiające skierowanie danych wyjściowych bezpośrednio do ulubionych narzędzi wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="b8615-108">The CLI has query and filtering features so you can pipe output directly to your favorite command-line tools.</span></span> <span data-ttu-id="b8615-109">Interfejs wiersza polecenia można zainstalować w systemie lokalnym jako obraz platformy Docker lub za pośrednictwem usługi [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="b8615-109">The CLI is available for installation on your local system, as a Docker image, or through [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview).</span></span>

> [!div class="nextstepaction"]
> <span data-ttu-id="b8615-110">Zainstaluj [interfejs wiersza polecenia platformy Azure 2.0](/cli/azure/install-azure-cli)</span><span class="sxs-lookup"><span data-stu-id="b8615-110">[Install the Azure CLI 2.0](/cli/azure/install-azure-cli)</span></span>

## <a name="visual-studio-code"></a><span data-ttu-id="b8615-111">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b8615-111">Visual Studio Code</span></span>

<span data-ttu-id="b8615-112">Visual Studio Code to uproszczony edytor zapewniający kompleksową obsługę języka Go za pośrednictwem rozszerzeń.</span><span class="sxs-lookup"><span data-stu-id="b8615-112">Visual Studio Code is a lightweight editor that has comprehensive support for the Go language through extensions.</span></span> <span data-ttu-id="b8615-113">Rozszerzenia zapewniają obsługę funkcji, takich jak autouzupełnianie, szablony `impl`, refaktoryzacja i debugowanie.</span><span class="sxs-lookup"><span data-stu-id="b8615-113">These extensions include support for features like autocomplete, `impl` templates, refactoring, and debugging.</span></span> <span data-ttu-id="b8615-114">Narzędzie Visual Studio Code oferuje także wiele rozszerzeń dla popularnych narzędzi dla deweloperów (np. kontrolę źródła), a nawet rozszerzenia przeznaczone do bezpośredniej interakcji z usługami Azure.</span><span class="sxs-lookup"><span data-stu-id="b8615-114">Visual Studio Code also offers many extensions for common developer tools such as source control, and even offers extensions for direct interactions with Azure services.</span></span> <span data-ttu-id="b8615-115">Firma Microsoft udostępnia oficjalne metarozszerzenie obejmujące te rozszerzenia platformy Azure (w tym interaktywny interfejs wiersza polecenia platformy Azure).</span><span class="sxs-lookup"><span data-stu-id="b8615-115">Microsoft maintains an official meta-extension including these Azure extensions, including an interactive interface for the Azure CLI.</span></span>

* [<span data-ttu-id="b8615-116">Instalowanie narzędzia Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b8615-116">Install Visual Studio Code</span></span>](https://code.visualstudio.com/Download)
* [<span data-ttu-id="b8615-117">Pobieranie rozszerzenia Visual Studio Code Go</span><span class="sxs-lookup"><span data-stu-id="b8615-117">Get the Visual Studio Code Go extension</span></span>](https://code.visualstudio.com/docs/languages/go)
* [<span data-ttu-id="b8615-118">Pobieranie rozszerzenia Azure Tools</span><span class="sxs-lookup"><span data-stu-id="b8615-118">Get the Azure Tools extension</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="dependency-management-with-dep"></a><span data-ttu-id="b8615-119">Zarządzanie zależnościami za pomocą narzędzia dep</span><span class="sxs-lookup"><span data-stu-id="b8615-119">Dependency management with dep</span></span>

<span data-ttu-id="b8615-120">Ponieważ nie istnieje jeszcze żadne oficjalne rozwiązanie, dostępnych jest wiele sposobów zarządzania zależnościami pakietów i zapewniania zasobów w języku Go.</span><span class="sxs-lookup"><span data-stu-id="b8615-120">There are many ways to manage your package dependencies and do vendoring with Go, since there is no official solution yet.</span></span> <span data-ttu-id="b8615-121">Zalecanym sposobem obsługi zarządzania jest korzystanie z menedżera zależności `dep`.</span><span class="sxs-lookup"><span data-stu-id="b8615-121">The recommended way to perform this management is with the `dep` dependency manager.</span></span> <span data-ttu-id="b8615-122">Zestaw Azure SDK dla języka Go zapewnia zasoby za pomocą narzędzia dep i gwarantuje prawidłowe pobieranie zależności w przypadku każdego projektu korzystającego z narzędzia dep.</span><span class="sxs-lookup"><span data-stu-id="b8615-122">The Azure SDK for Go uses dep for its vendoring, and is guaranteed to correctly get dependencies for any other project using dep.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="b8615-123">Pobierz menedżera zależności dep</span><span class="sxs-lookup"><span data-stu-id="b8615-123">Get the dep dependency manager</span></span>](https://github.com/tools/godep)

## <a name="telemetry-with-application-insights"></a><span data-ttu-id="b8615-124">Telemetria za pomocą usługi Application Insights</span><span class="sxs-lookup"><span data-stu-id="b8615-124">Telemetry with Application Insights</span></span>

<span data-ttu-id="b8615-125">Usługa [Application Insights](https://azure.microsoft.com/en-us/services/application-insights/) to produkt z zakresu analizy, który umożliwia łatwe zbieranie danych telemetrycznych z aplikacji oraz integrację z ekosystemem platformy Azure, usługami Visual Studio Team Services i GitHub.</span><span class="sxs-lookup"><span data-stu-id="b8615-125">[Application Insights](https://azure.microsoft.com/en-us/services/application-insights/) is an analytics product that allows you to easily collect telemetry information from your applications and integrates with the Azure ecosystem, Visual Studio Team Services, and GitHub.</span></span> <span data-ttu-id="b8615-126">Usługa może być używana w wielu aplikacjach. Firma Microsoft oferuje zestaw Go SDK umożliwiający pracę z usługą Application Insights.</span><span class="sxs-lookup"><span data-stu-id="b8615-126">It can be used in many applications, and Microsoft offers a Go SDK for working with Application Insights.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="b8615-127">Pobierz zestaw SDK Application Insights dla języka Go</span><span class="sxs-lookup"><span data-stu-id="b8615-127">Get the Application Insights for Go SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-Go) 
