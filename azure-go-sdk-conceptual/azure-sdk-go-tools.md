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
ms.openlocfilehash: 006d140bffb66fdd769a14511232d4ea5081811d
ms.sourcegitcommit: 181d4e0b164cf39b3feac346f559596bd19c94db
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38066986"
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a>Narzędzia dla deweloperów korzystających z zestawu Azure SDK dla języka Go

Oto kilka zalecanych narzędzi, które pomagają w sprawnym tworzeniu kodu w języku Go i jego bezproblemowym współdziałaniu z usługami platformy Azure.

## <a name="azure-cli-20"></a>Interfejs wiersza polecenia platformy Azure 2.0

Interfejs wiersza polecenia platformy Azure 2.0 umożliwia tworzenie i konfigurowanie zasobów platformy Azure w ramach posiadanych subskrypcji. Interfejs wiersza polecenia ułatwia szybkie rozpoczęcie tworzenia typowych udostępnionych zasobów platformy Azure, dzięki czemu można się skupić na bardziej złożonych zastosowaniach usług. Interfejs wiersza polecenia obejmuje funkcje zapytań i filtrowania umożliwiające skierowanie danych wyjściowych bezpośrednio do ulubionych narzędzi wiersza polecenia. Interfejs wiersza polecenia można zainstalować w systemie lokalnym jako obraz platformy Docker lub za pośrednictwem usługi [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).

> [!div class="nextstepaction"]
> Zainstaluj [interfejs wiersza polecenia platformy Azure 2.0](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a>Visual Studio Code

Visual Studio Code to uproszczony edytor zapewniający kompleksową obsługę języka Go za pośrednictwem rozszerzeń. Rozszerzenia zapewniają obsługę funkcji, takich jak autouzupełnianie, szablony `impl`, refaktoryzacja i debugowanie. Narzędzie Visual Studio Code oferuje także wiele rozszerzeń dla popularnych narzędzi dla deweloperów (np. kontrolę źródła), a nawet rozszerzenia przeznaczone do bezpośredniej interakcji z usługami Azure. Firma Microsoft udostępnia oficjalne metarozszerzenie obejmujące te rozszerzenia platformy Azure (w tym interaktywny interfejs wiersza polecenia platformy Azure).

* [Instalowanie narzędzia Visual Studio Code](https://code.visualstudio.com/Download)
* [Pobieranie rozszerzenia Visual Studio Code Go](https://code.visualstudio.com/docs/languages/go)
* [Pobieranie rozszerzenia Azure Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="dependency-management-with-dep"></a>Zarządzanie zależnościami za pomocą narzędzia dep

Ponieważ nie istnieje jeszcze żadne oficjalne rozwiązanie, dostępnych jest wiele sposobów zarządzania zależnościami pakietów i zapewniania zasobów w języku Go. Zalecanym sposobem obsługi zarządzania jest korzystanie z menedżera zależności `dep`. Zestaw Azure SDK dla języka Go zapewnia zasoby za pomocą narzędzia dep i gwarantuje prawidłowe pobieranie zależności w przypadku każdego projektu korzystającego z narzędzia dep.

> [!div class="nextstepaction"]
> [Pobierz menedżera zależności dep](https://github.com/golang/dep)
