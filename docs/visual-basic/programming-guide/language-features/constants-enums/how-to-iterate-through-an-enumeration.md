---
title: Пошаговое руководство. перебор перечислений
ms.date: 07/20/2015
helpviewer_keywords:
- arrays [Visual Basic], iterating
- enumerations [Visual Basic], iterating
- ListBox control [Windows Forms], populating from an enumeration
ms.assetid: e5aa10eb-cfcd-4a3b-8e76-f06b8f2002be
ms.openlocfilehash: 6e8fd6760565a73d9d3b3d1d02fc872992eea354
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/22/2019
ms.locfileid: "74354026"
---
# <a name="how-to-iterate-through-an-enumeration-in-visual-basic"></a>Практическое руководство. Перебор элементов перечисления в Visual Basic
Перечисления — это удобный способ работать с наборами связанных констант и связывать постоянные значения с именами. Чтобы выполнить итерацию по перечислению, можно переместить его в массив с помощью метода <xref:System.Enum.GetValues%2A>. Можно также выполнить итерацию перечисления с помощью оператора `For...Each`, используя метод <xref:System.Enum.GetNames%2A> или <xref:System.Enum.GetValues%2A> для извлечения строкового или числового значения.  
  
### <a name="to-iterate-through-an-enumeration"></a>Перебор перечислений  
  
- Объявите массив и преобразуйте перечисление в него с помощью метода <xref:System.Enum.GetValues%2A>, прежде чем передавать массив, как и любую другую переменную. В следующем примере каждый элемент перечисления выводится <xref:Microsoft.VisualBasic.FirstDayOfWeek> в процессе итерации по перечислению.  
  
     [!code-vb[VbEnumsTask#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#7)]  
  
## <a name="see-also"></a>См. также

- [Общие сведения о перечислениях](../../../../visual-basic/programming-guide/language-features/constants-enums/enumerations-overview.md)
- [Инструкции. Объявление перечисления](../../../../visual-basic/programming-guide/language-features/constants-enums/how-to-declare-enumerations.md)
- [Когда следует использовать перечисление](../../../../visual-basic/programming-guide/language-features/constants-enums/when-to-use-an-enumeration.md)
- [Практическое руководство. Определение строки, связанной со значением из перечисления](../../../../visual-basic/programming-guide/language-features/constants-enums/how-to-determine-the-string-associated-with-an-enumeration-value.md)
- [Практическое руководство. Ссылка на элемент перечисления](../../../../visual-basic/programming-guide/language-features/constants-enums/how-to-refer-to-an-enumeration-member.md)
- [Перечисления и уточнение имен](../../../../visual-basic/programming-guide/language-features/constants-enums/enumerations-and-name-qualification.md)
- [Массивы](../../../../visual-basic/programming-guide/language-features/arrays/index.md)
