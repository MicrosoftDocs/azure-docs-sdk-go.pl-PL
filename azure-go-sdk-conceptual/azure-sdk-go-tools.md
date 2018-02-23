---
title: "Narzędzia dla deweloperów języka Go"
description: "Narzędzia do pracy z zestawem Azure SDK dla Go i usługami platformy Azure"
keywords: azure, go, golang, azure, visual studio, visual studio code
author: sptramer
ms.author: sttramer
ms.date: 01/30/2018
ms.topic: article
ms.devlang: go
manager: routlaw
ms.openlocfilehash: 4753775e608b39c6da43d64fd08c1532e03d5810
ms.sourcegitcommit: aaa8c37880332625f858a38f5918e6cf581bf48d
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/15/2018
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a>Narzędzia dla deweloperów korzystających z zestawu Azure SDK dla języka Go

Oto kilka zalecanych narzędzi, które pomagają w sprawnym tworzeniu kodu w języku Go i jego bezproblemowym współdziałaniu z usługami platformy Azure.

## <a name="azure-cli-20"></a>Interfejs wiersza polecenia platformy Azure 2.0

Interfejs wiersza polecenia platformy Azure 2.0 umożliwia tworzenie i konfigurowanie zasobów platformy Azure w ramach posiadanych subskrypcji. Interfejs wiersza polecenia ułatwia szybkie rozpoczęcie tworzenia typowych udostępnionych zasobów platformy Azure, dzięki czemu można się skupić na bardziej złożonych zastosowaniach usług. Interfejs wiersza polecenia obejmuje funkcje zapytań i filtrowania umożliwiające skierowanie danych wyjściowych bezpośrednio do ulubionych narzędzi wiersza polecenia. Interfejs wiersza polecenia można zainstalować w systemie lokalnym jako obraz platformy Docker lub za pośrednictwem usługi [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview).

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
> [Pobierz menedżera zależności dep](https://github.com/tools/godep)

## <a name="telemetry-with-application-insights"></a>Telemetria za pomocą usługi Application Insights

Usługa [Application Insights](https://azure.microsoft.com/en-us/services/application-insights/) to produkt z zakresu analizy, który umożliwia łatwe zbieranie danych telemetrycznych z aplikacji oraz integrację z ekosystemem platformy Azure, usługami Visual Studio Team Services i GitHub. Usługa może być używana w wielu aplikacjach. Firma Microsoft oferuje zestaw Go SDK umożliwiający pracę z usługą Application Insights.

> [!div class="nextstepaction"]
> [Pobierz zestaw SDK Application Insights dla języka Go](https://github.com/Microsoft/ApplicationInsights-Go) 
