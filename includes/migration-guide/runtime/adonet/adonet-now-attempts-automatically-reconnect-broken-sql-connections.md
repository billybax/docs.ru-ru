---
ms.openlocfilehash: 4c65cd62b2d84ef2b852d10a04e5f6ce0cc82d3a
ms.sourcegitcommit: 0be8a279af6d8a43e03141e349d3efd5d35f8767
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2019
ms.locfileid: "59804896"
---
### <a name="adonet-now-attempts-to-automatically-reconnect-broken-sql-connections"></a>ADO.NET теперь пытается автоматически восстановить прерванное соединение SQL

|   |   |
|---|---|
|Подробные сведения|Начиная с .NET Framework 4.5.1 .NET Framework будет пытаться автоматически восстанавливать прерванные соединения SQL. Хотя это повысит надежность приложений, существуют крайние случаи, в которых приложению должно быть известно, что соединение было потеряно, чтобы можно было выполнить определенное действие после восстановления подключения.|
|Предложение|Если эта функция нежелательна из-за проблем совместимости, ее можно отключить, установив для свойства <xref:System.Data.SqlClient.SqlConnectionStringBuilder.ConnectRetryCount?displayProperty=name> строки соединения (или <xref:System.Data.SqlClient.SqlConnectionStringBuilder?displayProperty=name>) значение 0.|
|Область|Пограничный случай|
|Версия|4.5.1|
|Тип|Среда выполнения|
|Затронутые API|<ul><li><xref:System.Data.IDbConnection.ConnectionString?displayProperty=nameWithType></li><li><xref:System.Data.SqlClient.SqlConnection.ConnectionString?displayProperty=nameWithType></li><li><xref:System.Configuration.ConnectionStringSettings.ConnectionString?displayProperty=nameWithType></li><li><xref:System.Data.Common.DbConnection.ConnectionString?displayProperty=nameWithType></li><li><xref:System.Data.Common.DbConnectionStringBuilder.ConnectionString?displayProperty=nameWithType></li><li><xref:System.Data.SqlClient.SqlConnectionStringBuilder.%23ctor?displayProperty=nameWithType></li><li><xref:System.Data.SqlClient.SqlConnectionStringBuilder.%23ctor(System.String)?displayProperty=nameWithType></li><li><xref:System.Data.Common.DbConnectionStringBuilder.%23ctor?displayProperty=nameWithType></li><li><xref:System.Data.Common.DbConnectionStringBuilder.%23ctor(System.Boolean)?displayProperty=nameWithType></li></ul>|
