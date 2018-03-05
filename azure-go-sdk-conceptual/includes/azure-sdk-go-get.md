Zestaw [Azure SDK dla języka Go](https://github.com/Azure/azure-sdk-for-go) jest zgodny z językiem Go w wersji 1.8 lub nowszej. W przypadku środowisk korzystających z [profilów Azure Stack](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-version-profiles) wymagana jest wersja 1.9 lub nowsza.
Jeśli w danym systemie nie ma języka Go, wykonaj [instrukcje instalacji języka Go](https://golang.org/doc/install).

Zestaw Azure SDK dla języka Go i jego zależności można uzyskać za pomocą polecenia `go get`.

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> Ciąg `Azure` w adresie URL musi zaczynać się wielką literą. W przeciwnym razie podczas pracy z zestawem SDK mogą wystąpić problemy związane z wielkością liter. Ciąg `Azure` musi zaczynać się wielką literą także w Twoich instrukcjach importu.

