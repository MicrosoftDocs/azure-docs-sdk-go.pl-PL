---
title: Narzędzia dla deweloperów korzystających z zestawu Azure SDK dla języka Go
description: Narzędzia do pracy z zestawem Azure SDK dla Go i usługami platformy Azure
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.openlocfilehash: 70cf7d645f47df29e8e42599a0acd75858144783
ms.sourcegitcommit: 8b9e10b960150dc08f046ab840d6a5627410db29
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44059207"
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a><span data-ttu-id="54012-103">Narzędzia dla deweloperów korzystających z zestawu Azure SDK dla języka Go</span><span class="sxs-lookup"><span data-stu-id="54012-103">Tools for developers using the Azure SDK for Go</span></span>

<span data-ttu-id="54012-104">Oto kilka zalecanych narzędzi, które pomagają w sprawnym tworzeniu kodu w języku Go i jego bezproblemowym współdziałaniu z usługami platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="54012-104">To be effective at writing Go code and have it work seamlessly with Azure services, here are some recommended tools.</span></span>

## <a name="azure-cli"></a><span data-ttu-id="54012-105">Interfejs wiersza polecenia platformy Azure</span><span class="sxs-lookup"><span data-stu-id="54012-105">Azure CLI</span></span>

<span data-ttu-id="54012-106">Interfejs wiersza polecenia platformy Azure umożliwia tworzenie i konfigurowanie zasobów platformy Azure w ramach posiadanych subskrypcji.</span><span class="sxs-lookup"><span data-stu-id="54012-106">The Azure CLI provides a command-line interface to create and configure Azure resources in your subscriptions.</span></span> <span data-ttu-id="54012-107">Interfejs wiersza polecenia ułatwia szybkie rozpoczęcie tworzenia typowych udostępnionych zasobów platformy Azure, dzięki czemu można się skupić na bardziej złożonych zastosowaniach usług.</span><span class="sxs-lookup"><span data-stu-id="54012-107">The CLI can help you get started building common and shared Azure resources quickly, so that you can focus on more complex usage of services.</span></span> <span data-ttu-id="54012-108">Interfejs wiersza polecenia obejmuje funkcje zapytań i filtrowania umożliwiające skierowanie danych wyjściowych bezpośrednio do ulubionych narzędzi wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="54012-108">The CLI has query and filtering features so you can pipe output directly to your favorite command-line tools.</span></span> <span data-ttu-id="54012-109">Interfejs wiersza polecenia można zainstalować w systemie lokalnym jako obraz platformy Docker lub za pośrednictwem usługi [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="54012-109">The CLI is available for installation on your local system, as a Docker image, or through [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="54012-110">Zainstalowanie interfejsu wiersza polecenia platformy Azure</span><span class="sxs-lookup"><span data-stu-id="54012-110">Install the Azure CLI</span></span>](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a><span data-ttu-id="54012-111">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="54012-111">Visual Studio Code</span></span>

<span data-ttu-id="54012-112">Visual Studio Code jest uproszczonym edytorem zapewniającym obsługę języka Go.</span><span class="sxs-lookup"><span data-stu-id="54012-112">Visual Studio Code is a lightweight editor that offers Go support.</span></span> <span data-ttu-id="54012-113">To rozszerzenie udostępnia funkcje, takie jak autouzupełnianie, szablony `impl`, refaktoryzacja czy debugowanie.</span><span class="sxs-lookup"><span data-stu-id="54012-113">This extension offers features like autocomplete, `impl` templates, refactoring, and debugging.</span></span> <span data-ttu-id="54012-114">Visual Studio Code oferuje również obsługę dostępu w edytorze do kontroli źródła oraz rozszerzenia wspomagające pracę z usługami platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="54012-114">Visual Studio Code also offers support for in-editor access to source control, and extensions for working with Azure services.</span></span>

* [<span data-ttu-id="54012-115">Instalowanie narzędzia Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="54012-115">Install Visual Studio Code</span></span>](https://code.visualstudio.com/Download)
* [<span data-ttu-id="54012-116">Pobieranie rozszerzenia Visual Studio Code Go</span><span class="sxs-lookup"><span data-stu-id="54012-116">Get the Visual Studio Code Go extension</span></span>](https://code.visualstudio.com/docs/languages/go)
* [<span data-ttu-id="54012-117">Pobieranie rozszerzenia Visual Studio Code Azure Tools</span><span class="sxs-lookup"><span data-stu-id="54012-117">Get the Visual Studio Code Azure Tools extension</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="cicd-with-azure-devops-project"></a><span data-ttu-id="54012-118">Ciągła integracja/ciągłe wdrażanie za pomocą projektu DevOps platformy Azure</span><span class="sxs-lookup"><span data-stu-id="54012-118">CI/CD with Azure DevOps Project</span></span>

<span data-ttu-id="54012-119">Potoki projektu DevOps platformy Azure pozwalają na konfigurację systemu ciągłej integracji w aplikacjach języka Go.</span><span class="sxs-lookup"><span data-stu-id="54012-119">Azure DevOps Project pipelines allow you to set up a continuous integration system for your Go applications.</span></span> <span data-ttu-id="54012-120">Wymagane jest jedynie repozytorium Git, a wdrażanie i testowanie można wykonywać bezpośrednio na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="54012-120">All it takes is a git repo, and you can deploy and test directly on Azure.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="54012-121">Dowiedz się, jak utworzyć potok ciągłej integracji/ciągłego wdrażania za pomocą usługi projektu DevOps platformy Azure</span><span class="sxs-lookup"><span data-stu-id="54012-121">Learn how to create a CI/CD pipeline with Azure DevOps Project</span></span>](/azure/devops-project/azure-devops-project-go)

## <a name="dependency-management-with-dep"></a><span data-ttu-id="54012-122">Zarządzanie zależnościami za pomocą narzędzia dep</span><span class="sxs-lookup"><span data-stu-id="54012-122">Dependency management with dep</span></span>

<span data-ttu-id="54012-123">Zestaw Azure SDK dla języka Go używa narzędzia dep do zarządzania zależnościami.</span><span class="sxs-lookup"><span data-stu-id="54012-123">The Azure SDK for Go uses dep for dependency management.</span></span> <span data-ttu-id="54012-124">Polecenie dep umożliwia ściąganie wymagań i zapewnianie im zasobów dla aplikacji języka Go, unikając konfliktów związanych z wersjami i zapewniając prawidłowe działanie projektu.</span><span class="sxs-lookup"><span data-stu-id="54012-124">The dep command allows you to pull and vendor requirements for your Go application, avoiding version conflicts and ensuring that your project works correctly.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="54012-125">Pobierz menedżera zależności dep</span><span class="sxs-lookup"><span data-stu-id="54012-125">Get the dep dependency manager</span></span>](https://github.com/golang/dep)
