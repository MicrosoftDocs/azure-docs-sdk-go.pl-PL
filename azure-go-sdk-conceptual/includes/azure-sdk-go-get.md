---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: 5df14f939efdd0550b49261c88c8dc6518ada459
ms.sourcegitcommit: 8b9e10b960150dc08f046ab840d6a5627410db29
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44059275"
---
<span data-ttu-id="64087-101">Zestaw [Azure SDK dla języka Go](https://github.com/Azure/azure-sdk-for-go) jest zgodny z językiem Go w wersji 1.8 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="64087-101">The [Azure SDK for Go](https://github.com/Azure/azure-sdk-for-go) is compatible with Go versions 1.8 and higher.</span></span> <span data-ttu-id="64087-102">W przypadku środowisk korzystających z [profilów Azure Stack](/azure/azure-stack/user/azure-stack-version-profiles-go) wymagana jest wersja 1.9 lub nowsza.</span><span class="sxs-lookup"><span data-stu-id="64087-102">For environments using [Azure Stack Profiles](/azure/azure-stack/user/azure-stack-version-profiles-go), Go version 1.9 is the minimum requirement.</span></span>
<span data-ttu-id="64087-103">Jeśli chcesz zainstalować język Go, wykonaj [instrukcje instalacji języka Go](https://golang.org/doc/install).</span><span class="sxs-lookup"><span data-stu-id="64087-103">If you need to install Go, follow [the Go installation instructions](https://golang.org/doc/install).</span></span>

<span data-ttu-id="64087-104">Zestaw Azure SDK dla języka Go i jego zależności można pobrać za pomocą polecenia `go get`.</span><span class="sxs-lookup"><span data-stu-id="64087-104">You can download the Azure SDK for Go and its dependencies via `go get`.</span></span>

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> <span data-ttu-id="64087-105">Ciąg `Azure` w adresie URL musi zaczynać się wielką literą.</span><span class="sxs-lookup"><span data-stu-id="64087-105">Make sure that you capitalize `Azure` in the URL.</span></span> <span data-ttu-id="64087-106">W przeciwnym razie podczas pracy z zestawem SDK mogą wystąpić problemy związane z wielkością liter.</span><span class="sxs-lookup"><span data-stu-id="64087-106">Doing otherwise can cause case-related import problems when working with the SDK.</span></span> <span data-ttu-id="64087-107">Ciąg `Azure` musi zaczynać się wielką literą także w Twoich instrukcjach importu.</span><span class="sxs-lookup"><span data-stu-id="64087-107">You also need to capitalize `Azure` in your import statements.</span></span>
