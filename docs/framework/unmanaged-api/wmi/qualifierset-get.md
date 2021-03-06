---
title: QualifierSet_Get функция (неуправляемая справка API)
description: Функция QualifierSet_Get получает названный квалификатор.
ms.date: 11/06/2017
api_name:
- QualifierSet_Get
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- QualifierSet_Get
helpviewer_keywords:
- QualifierSet_Get function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: 2f4e2d4518e01f3415b8f17ce5778dd98b2a45c3
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/12/2020
ms.locfileid: "79174892"
---
# <a name="qualifierset_get-function"></a>Функция QualifierSet_Get
Получает указанный именованный квалификатор.  

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]
  
## <a name="syntax"></a>Синтаксис  
  
```cpp  
HRESULT QualifierSet_Get (
   [in] int                  vFunc,
   [in] IWbemQualifierSet*   ptr,
   [in] LPCWSTR              wszName,
   [in] LONG                 lFlags,
   [out] VARIANT*            pVal,
   [out] LONG*               plFlavor
);
```  

## <a name="parameters"></a>Параметры

`vFunc`(в) Этот параметр не используется.

`ptr`(в) Указатель на экземпляр [IWbemqualifierSet.](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemqualifierset)

`wszName`(в) Имя квалификатора, значение которого запрашивается.

`lFlags`(в) Защищены. Этот параметр должен быть 0.

`pVal`(ваут) В случае успеха правильный тип и значение для квалификатора. Если функция выходит `VARIANT` из строя, указанная `pVal` не изменяется. Если этот `null`параметр, параметр игнорируется.

`plFlavor`(ваут) Указатель на LONG, который получает квалификатор вкусбитые биты для запрошенного квалификатора. Если информация о вкусе не `null`желательна, этот параметр может быть.

## <a name="return-value"></a>Возвращаемое значение

Следующие значения, возвращенные этой функцией, определяются в файле заголовка *WbemCli.h* или вы можете определить их как константы в коде:

|Постоянно  |Значение  |Описание  |
|---------|---------|---------|
|`WBEM_E_INVALID_PARAMETER` | 0x80041008 | Недействительный параметр. |
|`WBEM_E_NOT_FOUND` | 0x80041002 | Указанный квалификатор не существует. |
|`WBEM_S_NO_ERROR` | 0 | Вызов функции был успешным.  |
  
## <a name="remarks"></a>Remarks

Эта функция обертывает вызов [iWbemqualifierSet::Get](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemqualifierset-get) метод.

## <a name="requirements"></a>Требования  
 **Платформы:** см. раздел [Требования к системе](../../get-started/system-requirements.md).  
  
 **Заголовок:** WMINet_Utils.idl  
  
 **Версии платформы .NET Framework:** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]  
  
## <a name="see-also"></a>См. также раздел

- [WMI и счетчики производительности (справочник по неуправляемым API)](index.md)
