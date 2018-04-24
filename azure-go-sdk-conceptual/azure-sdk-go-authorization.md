---
title: Uwierzytelnianie przy użyciu zestawu Azure SDK dla języka Go
description: Informacje na temat metod uwierzytelniania dostępnych w zestawie Azure SDK dla języka Go i sposobu ich używania.
services: azure
author: sptramer
ms.author: sttramer
ms.date: 04/03/2018
ms.topic: article
ms.service: azure
ms.devlang: go
manager: carmonm
ms.openlocfilehash: 39f9dc5a7cdf9ab84cfd9264446bacb31392ca80
ms.sourcegitcommit: 59d2b4c9d8da15fbbd15e36551093219fdaf256e
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/18/2018
---
# <a name="authentication-methods-in-the-azure-sdk-for-go"></a>Metody uwierzytelniania w zestawie Azure SDK dla języka Go

Zestaw Azure SDK dla języka Go oferuje różne typy i metody uwierzytelniania do użycia w aplikacji. Obsługiwane metody uwierzytelniania obejmują zarówno ściąganie informacji ze zmiennych środowiskowych, jak i interaktywne uwierzytelnianie oparte na Internecie. W tym artykule przedstawiono dostępne typy uwierzytelniania w zestawie SDK oraz metody ich używania. Opisano w nim również najlepsze rozwiązania dotyczące wybierania typu uwierzytelniania odpowiedniego dla aplikacji.

## <a name="available-authentication-types-and-methods"></a>Dostępne metody i typy uwierzytelniania

Zestaw Azure SDK dla języka Go oferuje kilka różnych typów uwierzytelniania korzystających z różnych zestawów poświadczeń. Każdy z tych typów uwierzytelniania jest udostępniany za pośrednictwem innych metod uwierzytelniania, które przedstawiają sposób traktowania tych poświadczeń jako danych wejściowych w zestawie SDK. W poniższej tabeli opisano dostępne typy uwierzytelniania i sytuacje, w których zalecamy ich użycie w aplikacji.

| Typ uwierzytelniania | Zalecane, gdy... |
|---------------------|---------------------|
| Uwierzytelnianie oparte na certyfikatach | Masz certyfikat X509 skonfigurowany dla jednostki lub użytkownika usługi Azure Active Directory (AAD). Aby dowiedzieć się więcej, zobacz [Get started with certificate-based authentication in Azure Active Directory (Wprowadzenie do uwierzytelniania opartego na certyfikacie w usłudze Azure Active Directory)]. |
| Poświadczenia klienta | Masz skonfigurowaną jednostkę usługi dla tej aplikacji lub klasy aplikacji, do której ona należy. Aby dowiedzieć się więcej, zobacz [Create a service principal with Azure CLI 2.0 (Tworzenie jednostki usługi przy użyciu interfejsu wiersza polecenia platformy Azure w wersji 2.0)]. |
| Tożsamość usługi zarządzanej (MSI, Managed Service Identity) | Aplikacja działa w obrębie zasobu platformy Azure, który został skonfigurowany przy użyciu tożsamości usługi zarządzanej (MSI). Aby dowiedzieć się więcej, zobacz [Managed Service Identity (MSI) for Azure resources (Tożsamość usługi zarządzanej (MSI) dla zasobów platformy Azure)]. |
| Token urządzenia | Aplikacja jest przeznaczona __tylko__ do użycia w trybie interaktywnym i ma różnych użytkowników potencjalnie z wielu dzierżaw usługi AAD. Użytkownicy mogą logować się za pomocą przeglądarki internetowej. Aby uzyskać więcej informacji, zobacz [Używanie tokenu urządzenia](#use-device-token-authentication).|
| Nazwa użytkownika/hasło | Masz interaktywną aplikację, która nie może używać innej metody uwierzytelniania. Użytkownicy nie mają włączonego uwierzytelniania wieloskładnikowego na potrzeby logowania do usługi AAD. |

> [!IMPORTANT]
> Jeśli używasz typu uwierzytelniania innego niż poświadczenia klienta, aplikacja musi zostać zarejestrowana w usłudze Azure Active Directory. Aby dowiedzieć się, jak to zrobić, zobacz [Integrating applications with Azure Active Directory (Integrowanie aplikacji za pomocą usługi Azure Active Directory)](/azure/active-directory/develop/active-directory-integrating-applications).

> [!NOTE]
> Jeśli nie masz specjalnych wymagań, unikaj uwierzytelniania nazwy użytkownika/hasła. W sytuacjach, w których logowanie oparte na użytkowniku jest właściwym rozwiązaniem, przeważnie można w zamian użyć uwierzytelniania tokenu urządzenia.

[Get started with certificate-based authentication in Azure Active Directory (Wprowadzenie do uwierzytelniania opartego na certyfikacie w usłudze Azure Active Directory)]: /azure/active-directory/active-directory-certificate-based-authentication-get-started
[Create a service principal with Azure CLI 2.0 (Tworzenie jednostki usługi przy użyciu interfejsu wiersza polecenia platformy Azure w wersji 2.0)]: /cli/azure/create-an-azure-service-principal-azure-cli
[Managed Service Identity (MSI) for Azure resources (Tożsamość usługi zarządzanej (MSI) dla zasobów platformy Azure)]: /azure/active-directory/managed-service-identity/overview

Te typy uwierzytelniania są dostępne za pośrednictwem różnych metod. [_Uwierzytelnianie oparte na środowisku_](#use-environment-based-authentication) polega na odczytywaniu poświadczeń bezpośrednio ze środowiska programu. [_Uwierzytelnianie na podstawie pliku_](#use-file-based-authentication) polega na załadowaniu pliku zawierającego poświadczenia jednostki usługi. [_Uwierzytelnianie oparte na kliencie_](#use-an-authentication-client) polega na użyciu obiektu w kodzie języka Go i wyznaczenie użytkownika jako osoby odpowiedzialnej za podawanie poświadczeń podczas wykonywania programu. Ponadto [_uwierzytelnianie tokenu urządzenia_](#use-device-token-authentication) wymaga od użytkowników interaktywnego logowania za pośrednictwem przeglądarki internetowej przy użyciu tokenu i nie może być używane z uwierzytelnianiem w oparciu o środowisko lub plik.

Wszystkie typy i funkcje uwierzytelniania są dostępne w pakiecie `github.com/Azure/go-autorest/autorest/azure/auth`.

> [!NOTE]
> Jeśli nie masz specjalnych wymagań, unikaj uwierzytelniania opartego na kliencie. Ta metoda uwierzytelniania upraszcza stosowanie niewłaściwych rozwiązań. W szczególności użycie uwierzytelniania opartego na kliencie zachęca do umieszczania poświadczeń w kodzie. Pisanie kodu niestandardowego na potrzeby uwierzytelniania może zostać również wyeliminowane w przyszłych wydaniach zestawu SDK, jeśli zmienią się wymagania dotyczące uwierzytelniania.

## <a name="use-environment-based-authentication"></a>Używanie uwierzytelniania opartego na środowisku

Jeśli korzystasz z aplikacji w środowisku ściśle kontrolowanym, takim jak kontener, uwierzytelnianie oparte na środowisku jest naturalnym wyborem. Możesz skonfigurować środowisko powłoki przed uruchomieniem aplikacji. Zestaw SDK dla języka Go odczyta te zmienne środowiskowe w czasie wykonywania w celu uwierzytelnienia przy użyciu platformy Azure. 

Uwierzytelnianie oparte na środowisku oferuje obsługę wszystkich metod uwierzytelniania, z wyjątkiem tokenów urządzeń, a obliczenia będą wykonywane w następującej kolejności: poświadczenia klienta, certyfikaty, nazwa użytkownika/hasło i tożsamość usługi zarządzanej (MSI). Jeśli nie ustawiono wymaganej zmiennej środowiskowej lub zestaw SDK otrzyma odmowę z usługi uwierzytelniania, zostanie podjęta próba użycia kolejnego typu uwierzytelniania. Jeśli zestaw SDK nie może uwierzytelnić ze środowiska, zwraca błąd.

W poniższej tabeli przedstawiono zmienne środowiskowe, które należy ustawić dla każdego typu uwierzytelniania obsługiwanego podczas uwierzytelniania opartego na środowisku.

| Typ uwierzytelniania | Zmienna środowiskowa | Opis |
| ------------------- | -------------------- | ----------- |
| __Poświadczenia klienta__ | `AZURE_TENANT_ID` | Identyfikator dzierżawy usługi Active Directory, do której należy jednostka usługi. |
| | `AZURE_CLIENT_ID` | Nazwa lub identyfikator jednostki usługi. |
| | `AZURE_CLIENT_SECRET` | Wpis tajny skojarzony z jednostką usługi. |
| __Certyfikat__ | `AZURE_TENANT_ID` | Identyfikator dzierżawy usługi Active Directory, przy użyciu której zarejestrowano certyfikat. |
| | `AZURE_CLIENT_ID` | Identyfikator klienta aplikacji skojarzony z certyfikatem. |
| | `AZURE_CERTIFICATE_PATH` | Ścieżka do pliku certyfikatu klienta. |
| | `AZURE_CERTIFICATE_PASSWORD` | Hasło certyfikatu klienta. |
| __Nazwa użytkownika/hasło__ | `AZURE_TENANT_ID` | Identyfikator dzierżawy usługi Active Directory, do której należy użytkownik. |
| | `AZURE_CLIENT_ID` | Identyfikator klienta aplikacji. |
| | `AZURE_USERNAME` | Nazwa użytkownika służąca do logowania. |
| | `AZURE_PASSWORD` | Hasło służące do logowania. |
| __MSI__ | | Usługa MSI nie wymaga ustawienia żadnych poświadczeń. Aplikacja musi działać w obrębie zasobu platformy Azure skonfigurowanego do użycia usługi MSI. Aby uzyskać szczegółowe informacje, zobacz [Managed Service Identity (MSI) for Azure resources (Tożsamość usługi zarządzanej (MSI) dla zasobów platformy Azure)]. |

Jeśli musisz nawiązać połączenie z punktem końcowym chmury lub zarządzania innym niż domyślna chmura platformy Azure, możesz również ustawić poniższe zmienne środowiskowe. Najbardziej typowe przyczyny ich ustawiania to korzystanie z usługi Azure Stack, chmury w innym regionie geograficznym lub klasycznego modelu wdrożenia platformy Azure.

| Zmienna środowiskowa | Opis  |
|----------------------|--------------|
| `AZURE_ENVIRONMENT` | Nazwa środowiska chmury, z którym będzie nawiązywane połączenie. |
| `AZURE_AD_RESOURCE` | Identyfikator zasobu usługi Active Directory do użycia podczas łączenia. Powinien być to identyfikator URI wskazujący na punkt końcowy zarządzania. |

W przypadku korzystania z uwierzytelniania opartego na środowisku należy wywołać funkcję [NewAuthorizerFromEnvironment](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromEnvironment), aby pobrać obiekt autoryzatora. Ten obiekt jest następnie ustawiany na właściwość `Authorizer` klientów, aby umożliwić im dostęp do platformy Azure.

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
authorizer, err := auth.NewAuthorizerFromEnvironment()
```

## <a name="use-file-based-authentication"></a>Używanie uwierzytelniania opartego na pliku

Uwierzytelnianie oparte na pliku działa tylko z poświadczeniami klienta przechowywanymi w formacie pliku lokalnego generowanym przez [interfejs wiersza polecenia platformy Azure w wersji 2.0](/cli/azure). Można łatwo utworzyć ten plik, tworząc nową jednostkę usługi przy użyciu parametru `--sdk-auth`. Jeśli planujesz użycie uwierzytelniania opartego na pliku, upewnij się, że ten argument zostanie podany podczas tworzenia jednostki usługi. Ponieważ interfejs wiersza polecenia wyświetla dane wyjściowe w lokalizacji `stdout`, przekieruj dane wyjściowe do pliku.

```azurecli
az ad sp create-for-rbac --sdk-auth > azure.auth
```

Ustaw zmienną środowiskową `AZURE_AUTH_LOCATION` na lokalizację, w której znajduje się plik autoryzacji. Ta zmienna środowiskowa jest odczytywana przez aplikację, a poświadczenia w jej obrębie są analizowane. Jeśli musisz wybrać plik autoryzacji w czasie wykonywania, zmodyfikuj środowisko programu przy użyciu funkcji [os.Setenv](https://golang.org/pkg/os/#Setenv).

Aby załadować informacje o uwierzytelnianiu, wywołaj funkcję [NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile). W przeciwieństwie do autoryzacji opartej na środowisku autoryzacja na podstawie pliku wymaga punktu końcowego zasobu.

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
authorizer, err := NewAuthorizerFromFile(azure.PublicCloud.ResourceManagerEndpoint)
```

Aby uzyskać więcej informacji na temat używania jednostek usług i zarządzania ich uprawnieniami dostępu, zobacz [Create a service principal with Azure CLI 2.0 (Tworzenie jednostki usługi przy użyciu interfejsu wiersza polecenia platformy Azure w wersji 2.0)].

## <a name="use-device-token-authentication"></a>Używanie uwierzytelniania tokenu urządzenia

Jeśli chcesz, aby użytkownicy logowali się interaktywnie, najlepszym sposobem zaoferowania tej możliwości jest uwierzytelnianie tokenu urządzenia. Ten przebieg uwierzytelniania przekazuje użytkownikowi token do wklejenia w witrynie logowania firmy Microsoft. Następnie użytkownik może się w niej zalogować za pomocą konta usługi Azure Active Directory (AAD). Ta metoda uwierzytelniania obsługuje konta z włączonym uwierzytelnianiem wieloskładnikowym,w przeciwieństwie do standardowego uwierzytelniania za pomocą nazwy użytkownika/hasła.

Aby użyć uwierzytelniania tokenu urządzenia, należy utworzyć autoryzator [DeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig) przy użyciu funkcji [NewDeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewDeviceFlowConfig). Wywołaj element [Authorizer](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig.Authorizer) w obiekcie wynikowym, aby rozpocząć proces uwierzytelniania. Uwierzytelnianie przepływu urządzenia blokuje wykonywanie programu do momentu ukończenia całego przepływu uwierzytelniania.

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
deviceConfig := auth.NewDeviceFlowConfig(applicationID, tenantID)
authorizer, err := deviceConfig.Authorizer()
```

## <a name="use-an-authentication-client"></a>Używanie klienta uwierzytelniania

Jeśli potrzebujesz określonego typu uwierzytelniania i chcesz, aby program wykonał zadania związane z ładowaniem informacji o uwierzytelnianiu od użytkownika, możesz użyć dowolnego klienta zgodnego z interfejsem [auth.AuthorizerConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#AuthorizerConfig). Użyj typu, który implementuje ten interfejs, jeśli potrzebujesz programu interaktywnego, używasz specjalnych plików konfiguracji lub masz wymaganie, które uniemożliwia korzystanie z innej metody uwierzytelniania.

> [!WARNING]
> Nigdy nie umieszczaj poświadczeń platformy Azure w kodzie aplikacji. Wprowadzanie wpisów tajnych do pliku binarnego aplikacji ułatwia osobie atakującej ich wyodrębnienie, bez względu na to, czy aplikacja została uruchomiona. Powoduje to narażenie na ryzyko wszystkich zasobów platformy Azure z autoryzowanymi poświadczeniami!

W poniższej tabeli przedstawiono listę typów w zestawie SDK, które są zgodne z interfejsem `AuthorizerConfig`.

| Typ uwierzytelniania | Typ autoryzatora |
|---------------------|-----------------------|
| Uwierzytelnianie oparte na certyfikacie | [ClientCertificateConfig] |
| Poświadczenia klienta | [ClientCredentialsConfig] |
| Tożsamość usługi zarządzanej (MSI, Managed Service Identity) | [MSIConfig] |
| Nazwa użytkownika/hasło | [UsernamePasswordConfig] |

[ClientCertificateConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#ClientCertificateConfig
[ClientCredentialsConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#ClientCredentialsConfig
[MSIConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#MSIConfig
[DeviceFlowConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig
[UsernamePasswordConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#UsernamePasswordConfig

Utwórz wystawcę uwierzytelniania przy użyciu skojarzonej funkcji `New`, a następnie wywołaj instrukcję `Authorize` w obiekcie wynikowym w celu przeprowadzenia uwierzytelniania. Aby na przykład używać uwierzytelniania opartego na certyfikacie:

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
certificateAuthorizer := auth.NewClientCertificateConfig(certificatePath, certificatePassword, clientID, tenantID)
authorizerToken, err := certificateAuthorizer.Authorize()
```
