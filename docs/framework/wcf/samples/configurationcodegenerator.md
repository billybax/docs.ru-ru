---
title: ConfigurationCodeGenerator
ms.date: 03/30/2017
ms.assetid: 3913aae8-165f-4014-9262-7fe426f90cb2
ms.openlocfilehash: f12fae48f1cee198aac22e6f09e616b407b4e9b5
ms.sourcegitcommit: 005980b14629dfc193ff6cdc040800bc75e0a5a5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2019
ms.locfileid: "70990067"
---
# <a name="configurationcodegenerator"></a>ConfigurationCodeGenerator
ConfigurationCodeGenerator - это средство, позволяющее предоставлять системе конфигурации реализации пользовательских каналов. Это позволяет пользователям пользовательских каналов настраивать каналы с помощью файла конфигурации, как это происходит в случае с системными привязками, например `NetTcpBinding`, или пользовательскими привязками с помощью элемента `TcpTransportBindingElement`.  
  
 При разработке пользовательского канала и его предоставлении в модели программирования с помощью нового элемента `BindingElement` или `Binding` необходимо создать набор классов, чтобы `BindingElement` или `Binding` можно было настраивать с помощью файла конфигурации. Средство ConfigurationCodeGenerator позволяет создать эти классы и сделать работу пользователей более удобной.  
  
### <a name="to-build-the-tool"></a>Построение средства  
  
1. Чтобы выполнить сборку решения, следуйте инструкциям в разделе [Создание примеров Windows Communication Foundation](../../../../docs/framework/wcf/samples/building-the-samples.md).  
  
2. При построении решения создается один файл: Конфигуратионкодеженератор. exe. Файл самплерун. cmd содержит образец командной строки, который показывает, как использовать это средство для создания классов для [транспорта. Пример](../../../../docs/framework/wcf/samples/transport-udp.md) протокола UDP.  
  
### <a name="to-run-the-tool"></a>Запуск средства  
  
1. В командной строке введите следующую команду, если имеется пользовательский тип `BindingElement` и пользовательский тип `Binding`:  
  
    ```console  
    ConfigurationCodeGenerator.exe /be:YourCustomBindingElementTypeName /sb:YourCustomStdBindingTypeName /dll:TheAssemblyWhereTheseTypesAreDefined  
    ```  
  
     Либо введите следующую команду, если имеется только пользовательский тип `BindingElement`:  
  
    ```console  
    ConfigurationCodeGenerator.exe /be:YourCustomBindingElementTypeName /dll: TheAssemblyWhereThisTypeIsDefined  
    ```  
  
     Либо введите следующую команду, если имеется только пользовательский тип `Binding`:  
  
    ```console  
    ConfigurationCodeGenerator.exe /sb:YourCustomStdBindingTypeName /dll:TheAssemblyWhereThisTypeIsDefined  
    ```  
  
     Команда создает три файла CS для элемента `BindingElement` (если задан параметр /be:), пять файлов CS для стандартного элемента `Binding` (если задан параметр /sb:), а также XML-файл.  
  
    1. Если используется параметр /be, один из файлов CS реализует `BindingElementExtensionSection` для элемента привязки. Этот код предоставляет элемент `BindingElement` для системы конфигурации, чтобы элементом привязки могли пользоваться другие пользовательские привязки. В других файлах содержатся классы, представляющие значения по умолчанию и константы. В файлах имеются комментарии `//TODO` напоминающие разработчикам о необходимости обновить значения по умолчанию.  
  
    2. Если задан параметр /sb, два файла CS реализуют элементы `StandardBindingElement` и `StandardBindingCollectionElement` соответственно, в результате чего стандартная привязка предоставляется системе конфигурации. В других файлах содержатся классы, представляющие значения по умолчанию и константы. В файлах имеются комментарии `//TODO` напоминающие разработчикам о необходимости обновить значения по умолчанию.  
  
         Если указан параметр/SB:, кодетоаддто\<*йоурстдбиндинг*>. cs содержит код, который необходимо вручную добавить в класс, реализующий стандартную привязку.  
  
     Файл SampleConfig.xml file содержит код конфигурации, который необходимо добавить в файл конфигурации, регистрирующий обработчики, определенные ранее на шагах 1 и 2.  
