---
title: Narzędzia dla deweloperów języka Go
description: Narzędzia do pracy z zestawem Azure SDK dla Go i usługami platformy Azure
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 07/13/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.openlocfilehash: dfa3912ac13e6f6d52d607f9dcc150f3a5b57602
ms.sourcegitcommit: d1790b317a8fcb4d672c654dac2a925a976589d4
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39039509"
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a>Narzędzia dla deweloperów korzystających z zestawu Azure SDK dla języka Go

Oto kilka zalecanych narzędzi, które pomagają w sprawnym tworzeniu kodu w języku Go i jego bezproblemowym współdziałaniu z usługami platformy Azure.

## <a name="azure-cli"></a>Interfejs wiersza polecenia platformy Azure

Interfejs wiersza polecenia platformy Azure umożliwia tworzenie i konfigurowanie zasobów platformy Azure w ramach posiadanych subskrypcji. Interfejs wiersza polecenia ułatwia szybkie rozpoczęcie tworzenia typowych udostępnionych zasobów platformy Azure, dzięki czemu można się skupić na bardziej złożonych zastosowaniach usług. Interfejs wiersza polecenia obejmuje funkcje zapytań i filtrowania umożliwiające skierowanie danych wyjściowych bezpośrednio do ulubionych narzędzi wiersza polecenia. Interfejs wiersza polecenia można zainstalować w systemie lokalnym jako obraz platformy Docker lub za pośrednictwem usługi [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).

> [!div class="nextstepaction"]
> [Zainstalowanie interfejsu wiersza polecenia platformy Azure](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a>Visual Studio Code

Visual Studio Code to uproszczony edytor zapewniający kompleksową obsługę języka Go za pośrednictwem rozszerzeń. Rozszerzenia zapewniają obsługę funkcji, takich jak autouzupełnianie, szablony `impl`, refaktoryzacja i debugowanie. Narzędzie Visual Studio Code oferuje także wiele rozszerzeń dla popularnych narzędzi dla deweloperów (np. kontrolę źródła), a nawet rozszerzenia przeznaczone do bezpośredniej interakcji z usługami Azure. Firma Microsoft udostępnia oficjalne metarozszerzenie obejmujące te rozszerzenia platformy Azure (w tym interaktywny interfejs wiersza polecenia platformy Azure).

* [Instalowanie narzędzia Visual Studio Code](https://code.visualstudio.com/Download)
* [Pobieranie rozszerzenia Visual Studio Code Go](https://code.visualstudio.com/docs/languages/go)
* [Pobieranie rozszerzenia Azure Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="cicd-with-azure-devops-project"></a>Ciągła integracja/ciągłe wdrażanie za pomocą projektu DevOps platformy Azure

Przy użyciu potoku projektu DevOps platformy Azure możesz skonfigurować ciągłe kompilowanie i wdrażanie aplikacji w języku Go. Jeśli tylko masz dostępne repozytorium git, możesz skonfigurować wdrażanie i testowanie bezpośrednio na zasobach platformy Azure. Potok konfiguracji jest łatwy do utworzenia i zarządzania, a ponieważ jest aprowizowany bezpośrednio na platformie Azure, możesz kontrolować go w taki sam sposób, jak inne zasoby platformy Azure.

> [!div class="nextstepaction"]
> [Dowiedz się, jak utworzyć potok ciągłej integracji/ciągłego wdrażania za pomocą usługi projektu DevOps platformy Azure](/devops-project/azure-devops-project-go)

## <a name="dependency-management-with-dep"></a>Zarządzanie zależnościami za pomocą narzędzia dep

Ponieważ nie istnieje jeszcze żadne oficjalne rozwiązanie, dostępnych jest wiele sposobów zarządzania zależnościami pakietów i zapewniania zasobów w języku Go. Zalecanym sposobem obsługi zarządzania jest korzystanie z menedżera zależności `dep`. Zestaw Azure SDK dla języka Go zapewnia zasoby za pomocą narzędzia dep i gwarantuje prawidłowe pobieranie zależności w przypadku każdego projektu korzystającego z narzędzia dep.

> [!div class="nextstepaction"]
> [Pobierz menedżera zależności dep](https://github.com/golang/dep)
