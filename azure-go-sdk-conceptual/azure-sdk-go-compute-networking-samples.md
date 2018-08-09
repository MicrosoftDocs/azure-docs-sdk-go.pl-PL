---
title: Przykłady z zestawu Azure SDK dla języka Go dla zasobów obliczeniowych i sieci
description: Wybrane przykłady do pracy z zasobami obliczeniowymi, takimi jak maszyny wirtualne i sieci wirtualne, z zestawu Azure SDK dla języka Go.
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 03/21/2018
ms.topic: sample
ms.prod: azure
ms.technology: azure-sdk-go
ms.service: virtual-machines
ms.devlang: go
ms.openlocfilehash: 3b31716ee42c638bab4a6dd99b9eb0d7c07e51a4
ms.sourcegitcommit: 0f581979216f7c9d4913681a6d9f6fe09af26e43
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39475793"
---
# <a name="azure-sdk-for-go-samples-for-compute-and-networking"></a><span data-ttu-id="0b42b-103">Przykłady z zestawu Azure SDK dla języka Go dla zasobów obliczeniowych i sieci</span><span class="sxs-lookup"><span data-stu-id="0b42b-103">Azure SDK for Go samples for compute and networking</span></span>

<span data-ttu-id="0b42b-104">Poniższa tabela zawiera linki do wybranych przykładów kodu źródłowego języka Go, których możesz użyć do zarządzania maszynami wirtualnymi, sieciami wirtualnymi i podsieciami na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="0b42b-104">The following table links to selected samples of Go source code that you can use to manage VMs, virtual networks, and subnets in Azure.</span></span> 

<span data-ttu-id="0b42b-105">Wszystkie przykłady z zestawu Azure SDK dla języka Go są dostępne w serwisie [GitHub](https://github.com/Azure-Samples/azure-sdk-for-go-samples).</span><span class="sxs-lookup"><span data-stu-id="0b42b-105">All samples for the Azure SDK for Go are available on [GitHub](https://github.com/Azure-Samples/azure-sdk-for-go-samples).</span></span>

| <span data-ttu-id="0b42b-106">Name (Nazwa)</span><span class="sxs-lookup"><span data-stu-id="0b42b-106">Name</span></span> | <span data-ttu-id="0b42b-107">Opis</span><span class="sxs-lookup"><span data-stu-id="0b42b-107">Description</span></span> |
|------|-------------|
| [<span data-ttu-id="0b42b-108">network/network</span><span class="sxs-lookup"><span data-stu-id="0b42b-108">network/network</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/network/network.go) | <span data-ttu-id="0b42b-109">Tworzenie, aktualizacja i usuwanie zasobów sieciowych, w tym sieci wirtualnych, podsieci i grup zabezpieczeń sieci, oraz wykonywanie względem nich zapytań.</span><span class="sxs-lookup"><span data-stu-id="0b42b-109">Create, update, delete, and query network resources including virtual networks, subnets, and network security groups.</span></span> |
| [<span data-ttu-id="0b42b-110">compute/vm_disk</span><span class="sxs-lookup"><span data-stu-id="0b42b-110">compute/vm_disk</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm_disk.go) | <span data-ttu-id="0b42b-111">Tworzenie, dołączanie, odłączanie, aktualizowanie i szyfrowanie dysków z danymi dla maszyny wirtualnej.</span><span class="sxs-lookup"><span data-stu-id="0b42b-111">Create, attach, detatch, update, and encrypt data disks for a VM.</span></span> |
| [<span data-ttu-id="0b42b-112">compute/vm</span><span class="sxs-lookup"><span data-stu-id="0b42b-112">compute/vm</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm.go) | <span data-ttu-id="0b42b-113">Tworzenie, aktualizowanie i dezaktywowanie maszyn wirtualnych oraz zarządzanie nimi.</span><span class="sxs-lookup"><span data-stu-id="0b42b-113">Create, update, deactivate, and manage VMs.</span></span> |
| [<span data-ttu-id="0b42b-114">compute/vm_with_availabilityset</span><span class="sxs-lookup"><span data-stu-id="0b42b-114">compute/vm_with_availabilityset</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm_with_availabilityset.go) | <span data-ttu-id="0b42b-115">Tworzenie zestawów dostępności i modułów równoważenia obciążenia dla maszyn wirtualnych.</span><span class="sxs-lookup"><span data-stu-id="0b42b-115">Create availability sets and load balancers for VMs.</span></span> |
| [<span data-ttu-id="0b42b-116">compute/vm_with_identity</span><span class="sxs-lookup"><span data-stu-id="0b42b-116">compute/vm_with_identity</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm_with_identity.go) | <span data-ttu-id="0b42b-117">Tworzenie tożsamości usługi zarządzanej (MSI, Managed Service Identity) dla maszyn wirtualnych.</span><span class="sxs-lookup"><span data-stu-id="0b42b-117">Create and manage Managed Service Identities (MSIs) for VMs.</span></span> |
