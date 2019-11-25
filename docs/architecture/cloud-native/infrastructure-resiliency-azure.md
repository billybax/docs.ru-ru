---
title: Устойчивость платформы Azure
description: Создание архитектуры облачных приложений .NET для Azure | Устойчивость облачной инфраструктуры к Azure
ms.date: 06/30/2019
ms.openlocfilehash: 02d661952c860da25442b0fa9fed0d5f93abe023
ms.sourcegitcommit: 4f4a32a5c16a75724920fa9627c59985c41e173c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/17/2019
ms.locfileid: "73841261"
---
# <a name="azure-platform-resiliency"></a>Устойчивость платформы Azure

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

Создание надежного приложения в облаке отличается от традиционной разработки локальных приложений. Хотя вы приобрели высокопроизводительное аппаратное обеспечение для увеличения масштаба в облачной среде, масштабируемой. Вместо того чтобы попытаться предотвратить сбои, цель состоит в том, чтобы свести к сведению их влияние и обеспечить стабильность системы.

С другой стороны, надежные облачные приложения отображают различные характеристики:

- Они устойчивы, устраняют проблемы и продолжают работать.
- Они являются высокодоступными (HA) и работают в нормальном состоянии без значительного простоя.

Понимание принципов совместной работы этих характеристик и их влияние на стоимость очень важно для создания надежного облачного приложения. Далее мы рассмотрим способы обеспечения устойчивости и доступности в собственных облачных приложениях, использующих функции из облака Azure.

## <a name="design-with-redundancy"></a>Проектирование с избыточностью

Ошибки различаются в области влияния. Сбой оборудования, например неисправный диск, может повлиять на отдельный узел в кластере. Сбой сетевого коммутатора может повлиять на всю серверную стойку. Менее распространенные сбои, например потери электроэнергии, могут нарушить работу всего центра обработки данных. В редких случаях весь регион становится недоступным.

[Избыточность](https://docs.microsoft.com/azure/architecture/guide/design-principles/redundancy) — один из способов обеспечения устойчивости приложений. Точный уровень избыточности зависит от бизнес-требований и влияет на стоимость и сложность системы. Например, развертывание в нескольких регионах является более затратным и сложным для управления, чем развертывание в одном регионе. Для управления отработкой отказа и восстановлением размещения потребуются рабочие процедуры. Дополнительные затраты и сложность могут быть выравниваются для некоторых бизнес-сценариев, а не для других.

Для обеспечения избыточности архитектуры необходимо определить критические пути в приложении, а затем определить, есть ли избыточность в каждой точке пути? Если произойдет сбой подсистемы, будет ли приложение переключаться на другое? Наконец, вам потребуются четкое понимание функций, встроенных в облачную платформу Azure, которые можно использовать в соответствии с требованиями к избыточности. Ниже приведены рекомендации по проектированию избыточности.

- *Развертывание нескольких экземпляров служб.* Если приложение зависит от одного экземпляра службы, оно создает единую точку отказа. Подготовка нескольких экземпляров повышает устойчивость и масштабируемость. При размещении в службе Kubernetes Azure можно декларативно настроить избыточные экземпляры (наборы реплик) в файле манифеста Kubernetes. Значение счетчика реплик можно управлять программно, на портале или с помощью функций автомасштабирования, которые будут обсуждаться далее в.

- *Использование балансировщика нагрузки.* Балансировка нагрузки распределяет запросы приложения на работоспособные экземпляры службы и автоматически удаляет неработоспособные экземпляры из вращения. При развертывании в Kubernetes можно указать балансировку нагрузки в файле манифеста Kubernetes в разделе службы.

- *Спланируйте развертывание в многорегионом.* Если приложение развернуто в одном регионе и регион становится недоступным, приложение также станет недоступным. Это может быть неприемлемым в соответствии с условиями соглашений об уровне обслуживания вашего приложения. Вместо этого рассмотрите возможность развертывания приложения и его служб в нескольких регионах. Например, кластер Azure Kubernetes Service (AKS) развертывается в одном регионе. Чтобы защитить систему от региональных сбоев, можно развернуть приложение на нескольких кластерах AKS в разных регионах и использовать функцию [парных регионов](https://buildazure.com/2017/01/06/azure-region-pairs-explained/) для координации обновлений платформы и определения приоритетов при восстановлении.

- *Включите [георепликацию](https://docs.microsoft.com/azure/sql-database/sql-database-active-geo-replication).* Георепликация для таких служб, как база данных SQL Azure и Cosmos DB, создает вторичные реплики данных в нескольких регионах. Хотя обе службы будут автоматически реплицировать данные в пределах одного региона, Георепликация защищает вас от регионального сбоя, позволяя выполнять отработку отказа в дополнительный регион. Еще одна рекомендация для центров георепликации, связанных с хранением образов контейнеров. Чтобы развернуть службу в AKS, необходимо сохранить образ и извлечь его из репозитория. Реестр контейнеров Azure интегрируется с AKS и может безопасно хранить образы контейнеров. Чтобы повысить производительность и доступность, рассмотрите возможность георепликации образов в реестр в каждом регионе, где имеется кластер AKS. Затем каждый кластер AKS извлекает образы контейнеров из локального реестра контейнеров в своем регионе, как показано на рисунке 6-6:

![Реплицированные ресурсы в разных регионах](./media/replicated-resources.png)

**Рис. 6-6**. Реплицированные ресурсы в разных регионах

- *Реализуйте подсистему балансировки нагрузки для трафика DNS.* [Диспетчер трафика Azure](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview) обеспечивает высокую доступность критически важных приложений с помощью балансировки нагрузки на уровне DNS. Он может направить трафик в разные регионы на основе географии, времени отклика кластера и даже работоспособности конечной точки приложения. Например, диспетчер трафика Azure может направлять клиентов в ближайший кластер AKS и экземпляр приложения. Если у вас несколько кластеров AKS в разных регионах, используйте диспетчер трафика для управления передачей трафика в приложения, выполняемые в каждом кластере. Этот сценарий показан на рис. 6-7.

![AKS и диспетчер трафика Azure](./media/aks-traffic-manager.png)

**Рис. 6-7**. AKS и диспетчер трафика Azure

## <a name="design-for-scalability"></a>Проектирование для масштабируемости

Облако не зависит от масштабирования. Возможность увеличения или уменьшения системных ресурсов для повышения или снижения загрузки системы — ключевой принцип облака Azure. Но для эффективного масштабирования приложения необходимо понимать возможности масштабирования каждой службы Azure, включенной в приложение.  Ниже приведены рекомендации по эффективному внедрению масштабирования в системе.

- *Проектирование для масштабирования.* Приложение должно быть спроектировано для масштабирования. Для начала службы должны быть без отслеживания состояния, чтобы запросы могли маршрутизироваться к любому экземпляру. Наличие служб без отслеживания состояния также означает, что добавление или удаление экземпляра не оказывает негативного воздействия на текущих пользователей.

- *Рабочие нагрузки секционирования*. Разбиение доменов на независимые, автономные микрослужбы позволяют каждой службе масштабироваться независимо от других. Как правило, службы будут иметь различные потребности и требования к масштабируемости. Секционирование позволяет масштабировать только то, что нужно масштабировать, без затрат на масштабирование всего приложения.

- *Предпочитать развертывание.* Облачные приложения имеют приоритетное масштабирование ресурсов, а не масштабирование. Масштабирование (также называемое горизонтальным масштабированием) включает в себя добавление дополнительных ресурсов службы в существующую систему для удовлетворения и предоставления общего доступа к желаемому уровню производительности. Увеличение масштаба (также известное как вертикальное масштабирование) подразумевает замену существующих ресурсов более мощным оборудованием (большим объемом диска, памяти и вычислительных ядер). Масштабирование можно вызывать автоматически с помощью функций автомасштабирования, доступных в некоторых облачных ресурсах Azure. Масштабирование по нескольким ресурсам также увеличивает избыточность всей системы. Наконец, увеличение масштаба одного ресурса обычно является более затратным, чем масштабирование по многим небольшим ресурсам. На рис. 6-8 показаны два подхода:

![Увеличение масштаба и масштабирование](./media/scale-up-scale-out.png)

**Рис. 6-8.** Увеличение масштаба и масштабирование

- *Пропорциональное масштабирование.* При масштабировании службы следует подумать о *наборах ресурсов*. Если вы хотите радикально масштабировать определенную службу, каково влияние на серверные хранилища данных, кэши и зависимые службы? Некоторые ресурсы, такие как Cosmos DB, могут масштабироваться пропорционально, а многие другие — нет. Необходимо убедиться, что вы не масштабируете ресурс до точки, в которой он будет исчерпан другие связанные ресурсы.

- *Избегайте сходства.* Рекомендуется убедиться, что узел не требует локального сходства, часто называемый *прикрепленным сеансом*. Запрос должен иметь возможность маршрутизироваться к любому экземпляру. Если необходимо сохранить состояние, его следует сохранить в распределенном кэше, например в [кэше Redis для Azure](https://azure.microsoft.com/services/cache/).

- *Воспользуйтесь преимуществами функций автомасштабирования платформы.* Используйте встроенные функции автомасштабирования, когда это возможно, а не пользовательские или сторонние механизмы. Там, где это возможно, используйте правила запланированного масштабирования, чтобы обеспечить доступность ресурсов без задержки запуска, но при необходимости добавьте автоматическое масштабирование для соответствующих правил, чтобы справиться с непредвиденными изменениями по запросу. Дополнительные сведения см. в разделе [руководство по автомасштабированию](https://docs.microsoft.com/azure/architecture/best-practices/auto-scaling).

- *Агрессивный горизонтальное масштабирование.* Последняя практика заключается в необходимости горизонтального масштабирования, чтобы можно было быстро удовлетворить немедленные всплески трафика без потери бизнеса. А затем масштабировать (то есть удалять ненужные экземпляры) консервативно, чтобы обеспечить стабильность системы. Простой способ реализации этого — задать период охлаждения, который является временем ожидания между операциями масштабирования, до пяти минут для добавления ресурсов и до 15 минут для удаления экземпляров.

## <a name="built-in-retry-in-services"></a>Встроенные повторные попытки в службах

Рекомендуется реализовать программные операции повторного выполнения в предыдущем разделе. Помните, что многие службы Azure и соответствующие клиентские пакеты SDK также включают механизмы повтора. В следующем списке перечислены функции повтора во многих службах Azure, обсуждаемых в этой книге:

- *Azure Cosmos DB.* Класс <xref:Microsoft.Azure.Documents.Client.DocumentClient> из API клиента автоматически выполняет неудачные попытки. Количество повторных попыток и максимальное время ожидания можно настроить. Исключения, вызываемые API клиента, — это запросы, превышающие политику повтора или невременные ошибки.

- *Кэш Redis для Azure.* Клиент Redis StackExchange использует класс диспетчера соединений, который включает повторные попытки при неудачных попытках. Количество повторных попыток, политика повтора и время ожидания являются настраиваемыми.

- *Служебная шина Azure.* Клиент служебной шины предоставляет [класс RetryPolicy](xref:Microsoft.ServiceBus.RetryPolicy) , для которого можно настроить интервал обратной передачи, число повторных попыток и <xref:Microsoft.ServiceBus.RetryExponential.TerminationTimeBuffer>, который указывает максимальное время, которое может занять операция. Политика по умолчанию — девять максимальных повторных попыток с 30-секундным периодом пересчета между попытками.

- *База данных SQL Azure.* Поддержка повторных попыток предоставляется при использовании библиотеки [Entity Framework Core](https://docs.microsoft.com/ef/core/miscellaneous/connection-resiliency) .

- *Служба хранилища Azure* Клиентская библиотека хранилища поддерживает операции повтора. Стратегии различаются для таблиц, больших двоичных объектов и очередей службы хранилища Azure. Кроме того, при включении функции геоизбыточности можно также переключаться между расположениями служб основного и дополнительного хранилищ.

- *Концентраторы событий Azure.* Клиентская библиотека концентратора событий поддерживает свойство RetryPolicy, которое включает в себя настраиваемую функцию экспоненциальной задержки.

>[!div class="step-by-step"]
>[Назад](application-resiliency-patterns.md)
>[Вперед](resilient-communications.md)