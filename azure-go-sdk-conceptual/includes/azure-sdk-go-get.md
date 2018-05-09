---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 02/14/2018
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: ddd58efdbc0c2d3ded068a9bebf2466db702566f
ms.sourcegitcommit: f08abf902b48f8173aa6e261084ff2cfc9043305
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/03/2018
---
<span data-ttu-id="295d8-101">Zestaw [Azure SDK dla języka Go](https://github.com/Azure/azure-sdk-for-go) jest zgodny z językiem Go w wersji 1.8 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="295d8-101">The [Azure SDK for Go](https://github.com/Azure/azure-sdk-for-go) is compatible with Go versions 1.8 and later.</span></span> <span data-ttu-id="295d8-102">W przypadku środowisk korzystających z [profilów Azure Stack](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-version-profiles) wymagana jest wersja 1.9 lub nowsza.</span><span class="sxs-lookup"><span data-stu-id="295d8-102">For environments using [Azure Stack Profiles](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-version-profiles), Go version 1.9 is the minimum requirement.</span></span>
<span data-ttu-id="295d8-103">Jeśli w danym systemie nie ma języka Go, wykonaj [instrukcje instalacji języka Go](https://golang.org/doc/install).</span><span class="sxs-lookup"><span data-stu-id="295d8-103">If you do not have Go available on your system, follow [the Go installation instructions](https://golang.org/doc/install).</span></span>

<span data-ttu-id="295d8-104">Zestaw Azure SDK dla języka Go i jego zależności można uzyskać za pomocą polecenia `go get`.</span><span class="sxs-lookup"><span data-stu-id="295d8-104">You can obtain the Azure SDK for Go and its dependencies via `go get`.</span></span>

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> <span data-ttu-id="295d8-105">Ciąg `Azure` w adresie URL musi zaczynać się wielką literą.</span><span class="sxs-lookup"><span data-stu-id="295d8-105">Make sure that you capitalize `Azure` in the URL.</span></span> <span data-ttu-id="295d8-106">W przeciwnym razie podczas pracy z zestawem SDK mogą wystąpić problemy związane z wielkością liter.</span><span class="sxs-lookup"><span data-stu-id="295d8-106">Doing otherwise can cause case-related import problems when working with the SDK.</span></span> <span data-ttu-id="295d8-107">Ciąg `Azure` musi zaczynać się wielką literą także w Twoich instrukcjach importu.</span><span class="sxs-lookup"><span data-stu-id="295d8-107">You also need to capitalize `Azure` in your import statements.</span></span>

