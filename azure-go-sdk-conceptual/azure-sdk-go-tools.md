---
title: Narzędzia dla deweloperów języka Go
description: Narzędzia do pracy z zestawem Azure SDK dla Go i usługami platformy Azure
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 01/30/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.openlocfilehash: 25b46e3a1636c39e261ba11c6f8939d8721cc693
ms.sourcegitcommit: 79d216c6b0442d0f3b3660ff2a34dc8b2049390c
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37093162"
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a><span data-ttu-id="5b073-103">Narzędzia dla deweloperów korzystających z zestawu Azure SDK dla języka Go</span><span class="sxs-lookup"><span data-stu-id="5b073-103">Tools for developers using the Azure SDK for Go</span></span>

<span data-ttu-id="5b073-104">Oto kilka zalecanych narzędzi, które pomagają w sprawnym tworzeniu kodu w języku Go i jego bezproblemowym współdziałaniu z usługami platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="5b073-104">To be effective at writing Go code and have it work seamlessly with Azure services, here are some recommended tools.</span></span>

## <a name="azure-cli-20"></a><span data-ttu-id="5b073-105">Interfejs wiersza polecenia platformy Azure 2.0</span><span class="sxs-lookup"><span data-stu-id="5b073-105">Azure CLI 2.0</span></span>

<span data-ttu-id="5b073-106">Interfejs wiersza polecenia platformy Azure 2.0 umożliwia tworzenie i konfigurowanie zasobów platformy Azure w ramach posiadanych subskrypcji.</span><span class="sxs-lookup"><span data-stu-id="5b073-106">The Azure 2.0 CLI provides a command-line interface to create and configure Azure resources in your subscriptions.</span></span> <span data-ttu-id="5b073-107">Interfejs wiersza polecenia ułatwia szybkie rozpoczęcie tworzenia typowych udostępnionych zasobów platformy Azure, dzięki czemu można się skupić na bardziej złożonych zastosowaniach usług.</span><span class="sxs-lookup"><span data-stu-id="5b073-107">The CLI can help you get started building common and shared Azure resources quickly, so that you can focus on more complex usage of services.</span></span> <span data-ttu-id="5b073-108">Interfejs wiersza polecenia obejmuje funkcje zapytań i filtrowania umożliwiające skierowanie danych wyjściowych bezpośrednio do ulubionych narzędzi wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="5b073-108">The CLI has query and filtering features so you can pipe output directly to your favorite command-line tools.</span></span> <span data-ttu-id="5b073-109">Interfejs wiersza polecenia można zainstalować w systemie lokalnym jako obraz platformy Docker lub za pośrednictwem usługi [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="5b073-109">The CLI is available for installation on your local system, as a Docker image, or through [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview).</span></span>

> [!div class="nextstepaction"]
> <span data-ttu-id="5b073-110">Zainstaluj [interfejs wiersza polecenia platformy Azure 2.0](/cli/azure/install-azure-cli)</span><span class="sxs-lookup"><span data-stu-id="5b073-110">[Install the Azure CLI 2.0](/cli/azure/install-azure-cli)</span></span>

## <a name="visual-studio-code"></a><span data-ttu-id="5b073-111">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5b073-111">Visual Studio Code</span></span>

<span data-ttu-id="5b073-112">Visual Studio Code to uproszczony edytor zapewniający kompleksową obsługę języka Go za pośrednictwem rozszerzeń.</span><span class="sxs-lookup"><span data-stu-id="5b073-112">Visual Studio Code is a lightweight editor that has comprehensive support for the Go language through extensions.</span></span> <span data-ttu-id="5b073-113">Rozszerzenia zapewniają obsługę funkcji, takich jak autouzupełnianie, szablony `impl`, refaktoryzacja i debugowanie.</span><span class="sxs-lookup"><span data-stu-id="5b073-113">These extensions include support for features like autocomplete, `impl` templates, refactoring, and debugging.</span></span> <span data-ttu-id="5b073-114">Narzędzie Visual Studio Code oferuje także wiele rozszerzeń dla popularnych narzędzi dla deweloperów (np. kontrolę źródła), a nawet rozszerzenia przeznaczone do bezpośredniej interakcji z usługami Azure.</span><span class="sxs-lookup"><span data-stu-id="5b073-114">Visual Studio Code also offers many extensions for common developer tools such as source control, and even offers extensions for direct interactions with Azure services.</span></span> <span data-ttu-id="5b073-115">Firma Microsoft udostępnia oficjalne metarozszerzenie obejmujące te rozszerzenia platformy Azure (w tym interaktywny interfejs wiersza polecenia platformy Azure).</span><span class="sxs-lookup"><span data-stu-id="5b073-115">Microsoft maintains an official meta-extension including these Azure extensions, including an interactive interface for the Azure CLI.</span></span>

* [<span data-ttu-id="5b073-116">Instalowanie narzędzia Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5b073-116">Install Visual Studio Code</span></span>](https://code.visualstudio.com/Download)
* [<span data-ttu-id="5b073-117">Pobieranie rozszerzenia Visual Studio Code Go</span><span class="sxs-lookup"><span data-stu-id="5b073-117">Get the Visual Studio Code Go extension</span></span>](https://code.visualstudio.com/docs/languages/go)
* [<span data-ttu-id="5b073-118">Pobieranie rozszerzenia Azure Tools</span><span class="sxs-lookup"><span data-stu-id="5b073-118">Get the Azure Tools extension</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="dependency-management-with-dep"></a><span data-ttu-id="5b073-119">Zarządzanie zależnościami za pomocą narzędzia dep</span><span class="sxs-lookup"><span data-stu-id="5b073-119">Dependency management with dep</span></span>

<span data-ttu-id="5b073-120">Ponieważ nie istnieje jeszcze żadne oficjalne rozwiązanie, dostępnych jest wiele sposobów zarządzania zależnościami pakietów i zapewniania zasobów w języku Go.</span><span class="sxs-lookup"><span data-stu-id="5b073-120">There are many ways to manage your package dependencies and do vendoring with Go, since there is no official solution yet.</span></span> <span data-ttu-id="5b073-121">Zalecanym sposobem obsługi zarządzania jest korzystanie z menedżera zależności `dep`.</span><span class="sxs-lookup"><span data-stu-id="5b073-121">The recommended way to perform this management is with the `dep` dependency manager.</span></span> <span data-ttu-id="5b073-122">Zestaw Azure SDK dla języka Go zapewnia zasoby za pomocą narzędzia dep i gwarantuje prawidłowe pobieranie zależności w przypadku każdego projektu korzystającego z narzędzia dep.</span><span class="sxs-lookup"><span data-stu-id="5b073-122">The Azure SDK for Go uses dep for its vendoring, and is guaranteed to correctly get dependencies for any other project using dep.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="5b073-123">Pobierz menedżera zależności dep</span><span class="sxs-lookup"><span data-stu-id="5b073-123">Get the dep dependency manager</span></span>](https://github.com/golang/dep)
