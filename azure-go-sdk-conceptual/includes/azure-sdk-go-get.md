---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 02/14/2018
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: 0febc2cc42ad95a1b7a5032c7987e37cc82f374e
ms.sourcegitcommit: 181d4e0b164cf39b3feac346f559596bd19c94db
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38067037"
---
Zestaw [Azure SDK dla języka Go](https://github.com/Azure/azure-sdk-for-go) jest zgodny z językiem Go w wersji 1.8 lub nowszej. W przypadku środowisk korzystających z [profilów Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-version-profiles) wymagana jest wersja 1.9 lub nowsza.
Jeśli w danym systemie nie ma języka Go, wykonaj [instrukcje instalacji języka Go](https://golang.org/doc/install).

Zestaw Azure SDK dla języka Go i jego zależności można uzyskać za pomocą polecenia `go get`.

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> Ciąg `Azure` w adresie URL musi zaczynać się wielką literą. W przeciwnym razie podczas pracy z zestawem SDK mogą wystąpić problemy związane z wielkością liter. Ciąg `Azure` musi zaczynać się wielką literą także w Twoich instrukcjach importu.

