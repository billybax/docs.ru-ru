---
title: Модульное тестирование кода C# с использованием NUnit и .NET Core
description: Сведения о концепциях модульного тестирования в C# и .NET Core в рамках пошаговой процедуры по созданию примера решения с помощью команды dotnet test и NUnit.
author: rprouse
ms.date: 08/31/2018
ms.openlocfilehash: 283aa5a28ed213d4290eb3c73a98af56ec074ad0
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/14/2020
ms.locfileid: "78240887"
---
# <a name="unit-testing-c-with-nunit-and-net-core"></a>Модульное тестирование кода C# с использованием NUnit и .NET Core

Этот учебник описывает пошаговую процедуру по созданию примера решения для изучения концепций модульного тестирования. Если при изучении учебника вы предпочитаете использовать готовое решение, [просмотрите или скачайте пример кода](https://github.com/dotnet/samples/blob/master/core/getting-started/unit-testing-using-nunit/) перед началом работы. Инструкции по загрузке см. в разделе [Просмотр и скачивание примеров](../../samples-and-tutorials/index.md#viewing-and-downloading-samples).

[!INCLUDE [testing an ASP.NET Core project from .NET Core](../../../includes/core-testing-note-aspnet.md)]

## <a name="prerequisites"></a>Prerequisites

- [Пакет SDK для .NET Core 2.1](https://dotnet.microsoft.com/download) или более поздней версии.
- Текстовый редактор или редактор кода по вашему выбору.

## <a name="creating-the-source-project"></a>Создание исходного проекта

Откройте окно оболочки. Создайте каталог с именем *unit-testing-using-nunit* для хранения решения. В этом каталоге выполните следующую команду, чтобы создать файл решения для библиотеки классов и тестового проекта:

```dotnetcli
dotnet new sln
```

Затем создайте каталог *PrimeService*. Ниже приведена актуальная структура каталогов и файлов:

```console
/unit-testing-using-nunit
    unit-testing-using-nunit.sln
    /PrimeService
```

Перейдите в каталог *PrimeService* и выполните следующую команду, чтобы создать исходный проект:

```dotnetcli
dotnet new classlib
```

Переименуйте *Class1.cs* в *PrimeService.cs*. Создайте сбойную реализацию класса `PrimeService`:

```csharp
using System;

namespace Prime.Services
{
    public class PrimeService
    {
        public bool IsPrime(int candidate)
        {
            throw new NotImplementedException("Please create a test first.");
        }
    }
}
```

Вернитесь в каталог *unit-testing-using-nunit*. Чтобы добавить проект библиотеки классов в решение, выполните следующую команду:

```dotnetcli
dotnet sln add PrimeService/PrimeService.csproj
```

## <a name="creating-the-test-project"></a>Создание тестового проекта

Затем создайте каталог *PrimeService.Tests*. Ниже представлена структура каталогов:

```console
/unit-testing-using-nunit
    unit-testing-using-nunit.sln
    /PrimeService
        Source Files
        PrimeService.csproj
    /PrimeService.Tests
```

Перейдите в каталог *PrimeService.Tests* и создайте проект, выполнив следующую команду:

```dotnetcli
dotnet new nunit
```

Команда [dotnet new](../tools/dotnet-new.md) создает тестовый проект, который использует NUnit в качестве библиотеки тестов. Созданный шаблон настраивает средство запуска тестов в файле *PrimeServiceTests.csproj*:

[!code-xml[Packages](~/samples/snippets/core/testing/unit-testing-using-nunit/csharp/PrimeService.Tests/PrimeService.Tests.csproj#Packages)]

Тестовый проект требует других пакетов для создания и выполнения модульных тестов. Команда `dotnet new` на предыдущем шаге добавляет пакет SDK тестирования от Майкрософт, платформу тестирования NUnit и адаптер тестирования NUnit. Теперь добавьте в проект библиотеку классов `PrimeService` в качестве еще одной зависимости. Используйте команду [`dotnet add reference`](../tools/dotnet-add-reference.md):

```dotnetcli
dotnet add reference ../PrimeService/PrimeService.csproj
```

Все содержимое файла можно просмотреть в [репозитории образцов](https://github.com/dotnet/samples/blob/master/core/getting-started/unit-testing-using-nunit/PrimeService.Tests/PrimeService.Tests.csproj) на сайте GitHub.

Ниже показан окончательный макет решения:

```console
/unit-testing-using-nunit
    unit-testing-using-nunit.sln
    /PrimeService
        Source Files
        PrimeService.csproj
    /PrimeService.Tests
        Test Source Files
        PrimeService.Tests.csproj
```

Выполните следующую команду в каталоге *unit-testing-using-nunit*:

```dotnetcli
dotnet sln add ./PrimeService.Tests/PrimeService.Tests.csproj
```

## <a name="creating-the-first-test"></a>Создание первого теста

Напишите один тест сбоя теста, запустите его, а затем повторите этот процесс. В каталоге *PrimeService.Tests* переименуйте файл *UnitTest1.cs* в *PrimeService_IsPrimeShould.cs* и замените его содержимое следующим кодом:

[!code-csharp[Sample_FirstTest](~/samples/snippets/core/testing/unit-testing-using-nunit/csharp/PrimeService.Tests/PrimeService_IsPrimeShould.cs?name=Sample_FirstTest)]

Атрибут `[TestFixture]` обозначает класс, содержащий модульные тесты. Атрибут `[Test]` указывает, что метод — это метода теста.

Сохраните этот файл и выполните [`dotnet test`](../tools/dotnet-test.md), чтобы создать тесты и библиотеку классов, а затем запустите тесты. Средство запуска тестов NUnit содержит точку входа в программу для выполнения тестов. `dotnet test` запускает средство выполнения тестов с помощью проекта модульного теста, который вы создали.

Тест не будет пройден. Вы еще не создали реализацию. Чтобы тест был пройден, напишите простейший код в классе `PrimeService`, который работает:

```csharp
public bool IsPrime(int candidate)
{
    if (candidate == 1)
    {
        return false;
    }
    throw new NotImplementedException("Please create a test first.");
}
```

Выполните команду *еще раз в каталоге*unit-testing-using-nunit`dotnet test`. Команда `dotnet test` запускает сборку для проекта `PrimeService` и затем для проекта `PrimeService.Tests`. После сборки обоих проектов она запускает этот отдельный тест. Он выполняется.

## <a name="adding-more-features"></a>Добавление дополнительных возможностей

Теперь, когда тест проходит успешно, пора создать дополнительные тесты. Есть еще ряд элементарных случаев с простыми числами: 0, -1. Можно добавить новые тесты с помощью атрибута `[Test]`, но это скоро станет утомительным. Есть другие атрибуты NUnit, которые позволяют создавать наборы похожих тестов.  Атрибут `[TestCase]` используется для создания набора тестов, которые выполняют один и тот же код, но имеют разные входные аргументы. С помощью атрибута `[TestCase]` можно указать значения для этих входных аргументов.

Вместо того чтобы создавать новые тесты, используйте этот атрибут, чтобы создать единый управляемый данными тест, который проверяет несколько значений меньше 2, то есть наименьшего простого числа.

[!code-csharp[Sample_TestCode](~/samples/snippets/core/testing/unit-testing-using-nunit/csharp/PrimeService.Tests/PrimeService_IsPrimeShould.cs?name=Sample_TestCode)]

Выполните команду `dotnet test`, и два из этих тестов завершаются ошибкой. Для успешного выполнения всех тестов нужно изменить предложение `if` в начале метода `Main` в файле *PrimeService.cs*:

```csharp
if (candidate < 2)
```

Продолжайте итерации, добавляя тесты, алгоритмы и код в главной библиотеке. В результате вы получите [готовую версию тестов](https://github.com/dotnet/samples/blob/master/core/getting-started/unit-testing-using-nunit/PrimeService.Tests/PrimeService_IsPrimeShould.cs) и [полную реализацию библиотеки](https://github.com/dotnet/samples/blob/master/core/getting-started/unit-testing-using-nunit/PrimeService/PrimeService.cs).

Вы создали небольшую библиотеку и набор модульных тестов для нее. Вы структурировали решение, чтобы сделать добавление новых пакетов и тестов частью обычного рабочего процесса и получить возможность сосредоточиться на задачах приложения.
