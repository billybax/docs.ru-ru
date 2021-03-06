---
title: Обзор 3D трансформаций
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- 3D transformations
- transformations [WPF], 3D
ms.assetid: e45e555d-ac1e-4b36-aced-e433afe7f27f
ms.openlocfilehash: 0efd6d247f23760c878c82cc92e99f1b83c1b9d2
ms.sourcegitcommit: 267d092663aba36b6b2ea853034470aea493bfae
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/21/2020
ms.locfileid: "80112353"
---
# <a name="3d-transformations-overview"></a>Обзор 3D трансформаций
Эта тема описывает, как применять преобразования к [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] 3D-моделям в графической системе. Преобразования позволяют разработчикам перемещать модели, изменять их размер и направление, при этом не затрагивая их базовые определяющие значения.  

## <a name="3d-coordinate-space"></a>3D Координация пространства  
 3D графический [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] контент в инкапсулируется в элементе, <xref:System.Windows.Controls.Viewport3D>который может участвовать в двухмерной структуре элемента. Графическая система рассматривает Viewport3D как двумерный визуальный элемент, подобный многим другим в [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)]. Viewport3D функционирует как окно — окно просмотра — трехмерной сцены. Точнее, это поверхность, на которой проецируется 3D-сцена.  Хотя вы можете использовать Viewport3D с другими 2D-объектами рисования на том же графике сцены, вы не можете пересекать 2D и 3D объекты внутри Viewport3D. В следующем обсуждении описанное координатное пространство расположено в элементе Viewport3D.  
  
 Система [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] координат для 2D графики находит происхождение в верхнем левом углу поверхности рендеринга (обычно экран). В 2D-системе положительные значения x-оси идут вправо, а положительные значения y-оси продолжаются вниз. В системе 3D-координат, однако, происхождение находится в центре экрана, с положительными значениями x-оси, протекающими вправо, но положительные значения y-оси, протекающие вверх, и положительные значения z-оси, исходящими наружу от происхождения, к зритель.  
  
 ![Системы координат](./media/coordsystem-1.png "CoordSystem-1")  
Сравнение системы координат  
  
 Пространство, определяемое этими осями, является [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)]стационарной рамкой отсчета для 3D-объектов в . При построении моделей в этом пространстве и создании источников света и камер для их отображения необходимо отличать стационарную систему отсчета координат ("мировую систему координат") от локальной системы отсчета, которая создается для каждой модели при применении к ней преобразований. Помните, что в зависимости от освещения и настроек камеры объекты в мировой системе координат могут выглядеть различным образом или быть полностью невидимыми. При этом положение камеры не изменяет расположения объектов в мировой системе координат.  
  
## <a name="transforming-models"></a>Преобразование моделей  
 При создании моделей в сцене им задается определенное местоположение. Для поворота моделей, изменения их размера или перемещения внутри сцены не следует изменять вершины, определяющие сами модели. Вместо этого, как и в 2D, вы применяете преобразования к моделям.  
  
 Каждый объект <xref:System.Windows.Media.Media3D.Model3D.Transform%2A> модели имеет свойство, с помощью которого можно перемещать, переориентировать или изменить размер модели. При применении преобразования все точки модели фактически смещаются с помощью определенного вектора или значения, заданного преобразованием. Другими словами, выполняется преобразование координатного пространства, в котором определена модель ("пространство модели"), при этом значения, составляющие геометрию модели в системе координат всей сцены ("мировое пространство"), не изменяются.  
  
## <a name="translation-transformations"></a>Преобразования перевода  
 3D-преобразования наследуют от <xref:System.Windows.Media.Media3D.Transform3D>абстрактного базового класса; они включают в себя <xref:System.Windows.Media.Media3D.TranslateTransform3D> <xref:System.Windows.Media.Media3D.ScaleTransform3D>классы <xref:System.Windows.Media.Media3D.RotateTransform3D>преобразования affine, и . 3D-система [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] также <xref:System.Windows.Media.Media3D.MatrixTransform3D> предоставляет класс, который позволяет указывать те же преобразования в более кратких матричных операциях.  
  
 <xref:System.Windows.Media.Media3D.TranslateTransform3D>перемещает все точки в Model3D в направлении офсетного <xref:System.Windows.Media.Media3D.TranslateTransform3D.OffsetX%2A> <xref:System.Windows.Media.Media3D.TranslateTransform3D.OffsetY%2A>вектора, указанного с помощью, и <xref:System.Windows.Media.Media3D.TranslateTransform3D.OffsetZ%2A> свойств. Например, если задать вершине куба с координатами (2, 2, 2) вектор смещения (0, 1,6, 1), то вершина (2, 2, 2) будет перемещена в точку (2, 3,6, 3). В пространстве модели вершина куба останется в точке (2, 2, 2), но, поскольку связь пространства модели с мировым пространством изменилась, координата пространства модели (2, 2, 2) будет находиться в точке (2, 3,6, 3) мирового пространства.  
  
 ![Фигура перевода](./media/transforms-translate.png "Преобразования-перевод")  
Перевод со смещением  
  
 В следующем примере кода показано, как применить перевод.  
  
 [!code-xaml[animation3dgallery_snip#Translation3DAnimationExampleWholePage](~/samples/snippets/csharp/VS_Snippets_Wpf/Animation3DGallery_snip/CS/Translation3DAnimationExample.xaml#translation3danimationexamplewholepage)]  
  
## <a name="scale-transformations"></a>Преобразования масштаба  
 <xref:System.Windows.Media.Media3D.ScaleTransform3D>изменение масштаба модели на определенный вектор масштаба со ссылкой на центральную точку. Укажите единый масштаб, который масштабирует модель по осям X, Y и Z для пропорционального изменения ее размера. Например, установка преобразования <xref:System.Windows.Media.ScaleTransform.ScaleX%2A> <xref:System.Windows.Media.ScaleTransform.ScaleY%2A>и <xref:System.Windows.Media.Media3D.ScaleTransform3D.ScaleZ%2A> свойств на 0,5 половины размера модели; установка одних и тех же свойств до 2 удваивает его масштаб во всех трех осях.  
  
 ![Равномерный ScaleTransform3D](./media/threecubes-uniformscale-1.png "threecubes_uniformscale_1")  
Пример ScaleVector  
  
 Указание преобразования неоднородной шкалы (т. е. шкалы, значения X, Y и Z которой отличаются) может привести к растягиванию или сжатию модели в одном или двух измерениях без изменения по остальным. Например, <xref:System.Windows.Media.ScaleTransform.ScaleX%2A> установка до <xref:System.Windows.Media.ScaleTransform.ScaleY%2A> 1, <xref:System.Windows.Media.Media3D.ScaleTransform3D.ScaleZ%2A> до 2 и 1 приведет к удвоению модели в высоту, но останется неизменной вдоль осей X и No.  
  
 По умолчанию ScaleTransform3D растягивает или сжимает вершины по отношению к началу координат (0, 0, 0). Но если преобразуемая модель строится не от начала координат, то при ее масштабировании от начала координат она будет находиться "не на своем месте" В то же время, если вершины модели умножаются на вектор масштабирования, операция масштабирования приведет и к преобразованию модели, и к ее масштабированию.  
  
 ![Три куба, масштабированные с указанной центральной точкой](./media/threecubes-scaledwithcenter-1.png "threecubes_scaledwithcenter_1")  
Пример центра масштабирования  
  
 Чтобы масштабировать модель "на месте", укажите центр модели, установив <xref:System.Windows.Media.ScaleTransform.CenterX%2A>ScaleTransform3D, <xref:System.Windows.Media.ScaleTransform.CenterY%2A>и <xref:System.Windows.Media.Media3D.ScaleTransform3D.CenterZ%2A> свойства. Это гарантирует, что графическая система масштабирует пространство модели, а <xref:System.Windows.Media.Media3D.Point3D>затем переводит его в центр указанного. В обратном случае, если для построенной относительно начала координат модели указана другая центральная точка, модель будет преобразована из начала координат.  
  
## <a name="rotation-transformations"></a>Преобразования поворота  
 Можно повернуть модель в 3D несколькими способами. При обычном преобразовании поворота указываются ось и угол поворота вокруг этой оси. Класс <xref:System.Windows.Media.Media3D.RotateTransform3D> позволяет определить с <xref:System.Windows.Media.Media3D.Rotation3D> его <xref:System.Windows.Media.Media3D.RotateTransform3D.Rotation%2A> свойством. Затем вы <xref:System.Windows.Media.Media3D.AxisAngleRotation3D.Axis%2A> <xref:System.Windows.Media.Media3D.AxisAngleRotation3D.Angle%2A> указываете и свойства на Rotation3D, в данном <xref:System.Windows.Media.Media3D.AxisAngleRotation3D>случае, для определения преобразования. В следующих примерах модель поворачивается на 60 градусов вокруг оси Y.  
  
 [!code-xaml[animation3dgallery_snip#Rotate3DUsingAxisAngleRotation3DExampleWholePage](~/samples/snippets/csharp/VS_Snippets_Wpf/Animation3DGallery_snip/CS/Rotat3DUsingAxisAngleRotation3DExample.xaml#rotate3dusingaxisanglerotation3dexamplewholepage)]  
  
 Примечание:[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] 3D является правой системой, что означает, что положительное значение угла для вращения приводит к вращению против часовой стрелки вокруг оси.  
  
 Вращения axis-angle предполагают вращение о происхождении, <xref:System.Windows.Media.Media3D.RotateTransform3D.CenterY%2A>если <xref:System.Windows.Media.Media3D.RotateTransform3D.CenterZ%2A> значение не определено для <xref:System.Windows.Media.Media3D.RotateTransform3D.CenterX%2A>, и свойства на RotateTransform3D. Как и при масштабировании, следует помнить, что при повороте преобразуется все координатное пространство модели целиком. Если модель была создана не от начала координат или была преобразована ранее, то поворот может быть произведен относительно начала координат, а не относительно местоположения модели.  
  
 ![Поворот с новой центральной точкой](./media/threecubes-rotationwithcenter-1.png "threecubes_rotationwithcenter_1")  
Поворот с указанием новой центральной точки  
  
 Чтобы повернуть модель "на месте", следует указать фактический центр модели в качестве центра поворота. Геометрия обычно моделируется от начала координат, поэтому для получения ожидаемого результата от набора преобразований необходимо вначале изменить размер модели (т. е. отмасштабировать), задать ее направление (повернуть), а затем перенести ее в нужное место (преобразовать).  
  
 ![Поворот на 60 градусов по оси Х и У](./media/twocubes-rotation2axes-1.png "twocubes_rotation2axes_1")  
Пример поворота  
  
 Осе-угловой поворот подходит для статических преобразований и некоторых анимаций. Тем не менее рассмотрим поворот модели куба на 60 градусов вокруг оси X, а затем на 45 градусов вокруг оси Z. Это преобразование можно описать как два отдельных аффинных преобразования или как матрицу. Тем не менее плавно анимировать поворот, определенный таким образом, может быть достаточно трудно. Несмотря на то что начальные и конечные позиции вычисляемой модели совпадают, промежуточные положения модели не определены при вычислении. Кватернионы представляют собой альтернативный способ вычисления интерполяции между начальной и конечной точками поворота.  
  
 Кватернион представляет ось в 3D-пространстве и поворот вокруг этой оси. Например, кватернион может быть представлен осью (1, 1, 2) и поворотом на 50 градусов. Возможности определения поворотов с помощью кватернионов зависят от двух операций, которые могут быть выполнены над ними: композиции и интерполяции. Композиция из двух кватернионов, применяемая к геометрическому объекту, означает "поворот геометрического объекта вокруг axis2 на угол rotation2, затем — вокруг axis1 на угол rotation1". С помощью композиции можно объединить два поворота геометрического объекта для получения одного квантериона, представляющего собой результат. Поскольку интерполяция кватернионов позволяет вычислить плавный и наиболее подходящий путь от одной оси и ориентацию относительно другой, разработчик может выполнить интерполяцию из исходного кватерниона в результирующий для достижения плавного перехода от одного к другому. Это позволит анимировать преобразование. Для моделей, которые вы хотите анимировать, вы можете указать пункт назначения <xref:System.Windows.Media.Media3D.Quaternion> для вращения, используя <xref:System.Windows.Media.Media3D.QuaternionRotation3D> для <xref:System.Windows.Media.Media3D.RotateTransform3D.Rotation%2A> свойства.  
  
## <a name="using-transformation-collections"></a>Использование коллекции преобразований  
 При построении сцены обычно применяется несколько преобразований модели. Добавьте преобразования <xref:System.Windows.Media.Media3D.Transform3DGroup.Children%2A> в коллекцию <xref:System.Windows.Media.Media3D.Transform3DGroup> класса, чтобы группировать преобразования удобно для применения к различным моделям в сцене. Часто удобно повторно использовать преобразование в нескольких различных группах (подобно повторному использованию модели путем применения разных наборов преобразований к экземплярам). Обратите внимание, что порядок добавления преобразований в коллекцию имеет значение: преобразования в коллекции применяются начиная с первого.  
  
## <a name="animating-transformations"></a>Анимация преобразований  
 3D-реализация [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] участвует в той же системе синхронизации и анимации, что и 2D-графика. Другими словами, чтобы оживить 3D-сцену, оживить свойства своих моделей. Можно непосредственно анимировать свойства примитивов, но обычно проще анимировать преобразования, изменяющие позицию или внешний вид моделей. Поскольку преобразования могут <xref:System.Windows.Media.Media3D.Model3DGroup> быть применены как к объектам, так и к отдельным моделям, можно применить один набор анимаций к детям Model3Dgroup и другой набор анимаций к группе объектов.  Дополнительные сведения о системе времени и анимации [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] см. в разделах [Общие сведения об эффектах анимации](animation-overview.md) и [Общие сведения о Storyboard](storyboards-overview.md).  
  
 Для анимации объекта в приложении [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] создайте временную шкалу, определите анимацию (которая изменяет значение некоторого свойства во времени) и укажите свойство, к которому применяется анимация. Это свойство должно быть свойством элемента FrameworkElement. Поскольку все объекты в 3D-сцене являются детьми Viewport3D, свойства, на которые нацелена любая анимация, которую вы хотите применить к сцене, являются свойствами свойств Viewport3D. Важно правильно указать путь к свойству для анимации, так как синтаксис может быть подробным.  
  
 Предположим, требуется повернуть объект на месте, а также применить качательное движение, чтобы лучше показать объект. Можно применить к модели класс RotateTransform3D и анимировать ее ось вращения от одного вектора к другому. Следующий пример кода демонстрирует <xref:System.Windows.Media.Animation.Vector3DAnimation> применение к свойству Axis вращения 3D преобразования, предполагая, что RotateTransform3D будет одним <xref:System.Windows.Media.TransformGroup>из нескольких преобразований, применяемых к модели с помощью .  
  
 [!code-csharp[3doverview#3DOverview3DN1](~/samples/snippets/csharp/VS_Snippets_Wpf/3DOverview/CSharp/Window1.xaml.cs#3doverview3dn1)]
 [!code-vb[3doverview#3DOverview3DN1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/3DOverview/visualbasic/window1.xaml.vb#3doverview3dn1)]  
  
 [!code-csharp[3doverview#3DOverview3DN3](~/samples/snippets/csharp/VS_Snippets_Wpf/3DOverview/CSharp/Window1.xaml.cs#3doverview3dn3)]
 [!code-vb[3doverview#3DOverview3DN3](~/samples/snippets/visualbasic/VS_Snippets_Wpf/3DOverview/visualbasic/window1.xaml.vb#3doverview3dn3)]  
  
 Используйте аналогичный синтаксис для других свойств преобразования, чтобы переместить или отмасштабировать объект.  Например, можно применить <xref:System.Windows.Media.Animation.Point3DAnimation> свойство ScaleCenter в преобразовании масштаба, чтобы заставить модель плавно исказить ее форму.  
  
 Хотя предыдущие примеры преобразуют <xref:System.Windows.Media.Media3D.GeometryModel3D>свойства, также возможно трансформировать свойства других моделей в сцене.  Например, применяя преобразования анимации к объектам Light, можно создать перемещение световых и теневых эффектов, которые могут значительным образом изменить внешний вид модели.  
  
 Поскольку камеры также являются моделями, их свойства также могут быть преобразованы.  Хотя внешний вид сцены можно изменить путем преобразования местоположения камеры или расстояний на плоскости (т. е. преобразованием всей проекции сцены целиком), обратите внимание, что многие эффекты, созданные таким способом, не создадут такого же впечатления, как преобразования, примененные к расположению или положению моделей в сцене.  
  
## <a name="see-also"></a>См. также раздел

- [3D Графика Обзор](3-d-graphics-overview.md)
- [Общие сведения о классах Transform](transforms-overview.md)
- [2D Преобразует образец](https://github.com/Microsoft/WPF-Samples/tree/master/Graphics/2DTransforms)
