---
title: Ajuste del rendimiento para HTTP 1.1/2
description: Recomendaciones para la optimización del rendimiento de HTTP 1.1/2
ms.topic: article
ms.author: ivanpash; gmonte
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 362ceea398e13c5e537d1d828b86eec8b5d66a8f
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87896030"
---
# <a name="performance-tuning-http-112"></a>Ajuste del rendimiento de HTTP 1.1/2

HTTP/2 está diseñado para mejorar el rendimiento en el lado del cliente (por ejemplo, el tiempo de carga de la página en un explorador). En el servidor, puede representar un leve aumento en el costo de la CPU. Mientras que el servidor ya no requiere una única conexión TCP para cada solicitud, parte de ese estado se conservará ahora en la capa HTTP. Además, HTTP/2 tiene compresión de encabezado, que representa la carga de CPU adicional.

Algunas situaciones requieren una reserva HTTP/1.1 (restableciendo la conexión HTTP/2 y, en su lugar, el establecimiento de una conexión nueva para usar HTTP/1.1). En concreto, la renegociación de TLS y la autenticación HTTP (excepto básica e implícita) requieren la reserva HTTP/1.1. Aunque esto agrega sobrecarga, estas operaciones ya implican cierto retraso y, por tanto, no son especialmente sensibles al rendimiento.

## <a name="additional-references"></a>Referencias adicionales
- [Ajuste del rendimiento del servidor Web](index.md)
- [Optimización del rendimiento de IIS 10.0](tuning-iis-10.md)