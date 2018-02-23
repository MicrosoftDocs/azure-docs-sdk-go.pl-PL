<span data-ttu-id="3aa85-101">Zestaw Azure SDK dla języka Go jest zgodny z językiem Go w wersji 1.8 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="3aa85-101">The Azure SDK for Go is compatible with Go versions 1.8 and later.</span></span> <span data-ttu-id="3aa85-102">W przypadku środowisk korzystających z [profilów Azure Stack](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-version-profiles) wymagana jest wersja 1.9 lub nowsza.</span><span class="sxs-lookup"><span data-stu-id="3aa85-102">For environments using [Azure Stack Profiles](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-version-profiles), Go version 1.9 is the minimum requirement.</span></span> <span data-ttu-id="3aa85-103">Jeśli w danym systemie nie ma języka Go, wykonaj [instrukcje instalacji języka Go](https://golang.org/doc/install).</span><span class="sxs-lookup"><span data-stu-id="3aa85-103">If you do not have Go available on your system, follow [the Go installation instructions](https://golang.org/doc/install).</span></span>

<span data-ttu-id="3aa85-104">Zestaw Azure SDK dla języka Go i jego zależności można uzyskać za pomocą polecenia `go get`.</span><span class="sxs-lookup"><span data-stu-id="3aa85-104">You can obtain the Azure SDK for Go and its dependencies via `go get`.</span></span>

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> <span data-ttu-id="3aa85-105">Ciąg `Azure` w adresie URL musi zaczynać się wielką literą.</span><span class="sxs-lookup"><span data-stu-id="3aa85-105">Make sure that you capitalize `Azure` in the URL.</span></span> <span data-ttu-id="3aa85-106">W przeciwnym razie podczas pracy z zestawem SDK mogą wystąpić problemy związane z wielkością liter.</span><span class="sxs-lookup"><span data-stu-id="3aa85-106">Doing otherwise can cause case-related import problems when working with the SDK.</span></span> <span data-ttu-id="3aa85-107">Ciąg `Azure` musi zaczynać się wielką literą także w Twoich instrukcjach importu.</span><span class="sxs-lookup"><span data-stu-id="3aa85-107">You also need to capitalize `Azure` in your import statements.</span></span>

