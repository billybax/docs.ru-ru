---
title: Сетевые протоколы — gRPC для разработчиков WCF
description: Общие сведения о сетевых протоколах gRPC.
ms.date: 09/02/2019
ms.openlocfilehash: 5e837738bd345608ca7119d04c9221acb220c276
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/12/2019
ms.locfileid: "73971708"
---
# <a name="network-protocols"></a>Сетевые протоколы

В отличие от WCF, gRPC использует HTTP/2 в качестве основы для своей сети. Это обеспечивает значительные преимущества по сравнению с WCF и SOAP, которые работают только с HTTP/1.1. Для разработчиков, желающих использовать gRPC, учитывая, что нет альтернативы HTTP/2, кажется идеальным моментом исследовать HTTP/2 более подробно и определять дополнительные преимущества для использования gRPC.

HTTP/2, вызываемая веб-задачей инженерной разработки в 2015, была получена из экспериментального протокола SPDY, который уже использовался в Google. Он был специально разработан для более эффективного, быстрого и более безопасного, чем HTTP/1.1.

## <a name="key-features-of-http2"></a>Основные функции HTTP/2

В следующем списке показаны некоторые основные функции и преимущества HTTP/2:

### <a name="binary-protocol"></a>Двоичный протокол

Циклы запросов и ответов больше не требуют текстовых команд. Это упрощает и ускоряет реализацию команд. В частности, анализ данных выполняется быстрее и использует меньше памяти, задержка сети сокращается с помощью очевидных улучшений, а также обеспечивается общее лучшее использование сетевых ресурсов.

### <a name="streams"></a>Потоки

Потоки позволяют создавать долгосрочные соединения между отправителем и получателем, при котором несколько сообщений или кадров могут быть отправлены асинхронно. Несколько потоков могут обрабатываться независимо друг от друга через одно соединение HTTP/2.

### <a name="request-multiplexing-over-a-single-tcp-connection"></a>Запрос мультиплексирования по одному TCP-соединению

Эта функция является одним из наиболее важных нововведений HTTP/2. Разрешая несколько параллельных запросов данных, теперь можно загружать веб-файлы параллельно с одного сервера. Веб-сайты быстрее загружаются, и потребность в оптимизации уменьшается. Блокировка до конца строки (HOL), где готовые ответы должны ожидать отправки до тех пор, пока не завершится предыдущий запрос, также будет устранена (хотя блокировка на уровне ХОЛЬ может выполняться на транспортном протоколе TCP).

### <a name="nettcp-like-performance-cross-platform"></a>Производительность NetTCP-Like, кросс-платформенные

По сути, сочетание gRPC и HTTP/2 предоставляет разработчикам по крайней мере эквивалентную скорость и эффективность привязок NetTCP для WCF, а в некоторых случаях еще более высокую скорость и эффективность. Однако, в отличие от NetTCP, gRPC через HTTP/2 не ограничены приложениями .NET.

>[!div class="step-by-step"]
>[Назад](interface-definition-language.md)
>[Вперед](why-grpc.md)