---
title: Ajuste del rendimiento para HTTP 1.1/2
description: Recomendaciones para la optimización del rendimiento de HTTP 1.1/2
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: IvanPash; GMonte
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: f7d7bd5145a0804b9ec86438602dfed7c75a2b02
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384971"
---
# <a name="performance-tuning-http-112"></a>Ajuste del rendimiento de HTTP 1.1/2

HTTP/2 está diseñado para mejorar el rendimiento en el lado del cliente (por ejemplo, el tiempo de carga de la página en un explorador). En el servidor, puede representar un leve aumento en el costo de la CPU. Mientras que el servidor ya no requiere una única conexión TCP para cada solicitud, parte de ese estado se conservará ahora en la capa HTTP. Además, HTTP/2 tiene compresión de encabezado, que representa la carga de CPU adicional.

Algunas situaciones requieren una reserva HTTP/1.1 (restableciendo la conexión HTTP/2 y, en su lugar, el establecimiento de una conexión nueva para usar HTTP/1.1). En concreto, la renegociación de TLS y la autenticación HTTP (excepto básica e implícita) requieren la reserva HTTP/1.1. Aunque esto agrega sobrecarga, estas operaciones ya implican cierto retraso y, por tanto, no son especialmente sensibles al rendimiento.

## <a name="see-also"></a>Vea también
- [Ajuste del rendimiento del servidor Web](index.md) 
- [Optimización del rendimiento de IIS 10.0](tuning-iis-10.md)