---
title: Optimizar el rendimiento de HTTP 1.1/2
description: Para HTTP 1.1/2 de recomendaciones de optimización del rendimiento
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: IvanPash; GMonte
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: bf85efa88e377966135c23a548119f19c39cceba
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866376"
---
# <a name="performance-tuning-http-112"></a>HTTP 1.1/2 de optimización del rendimiento

HTTP/2 está pensado para mejorar el rendimiento del cliente (p. ej., página tiempo de carga en un explorador). En el servidor, puede representar un leve aumento en el costo de CPU. Mientras que el servidor ya no requiere una conexión TCP única para cada solicitud, algunos de los que el estado ahora se mantienen en el nivel de HTTP. Además, HTTP/2 tiene compresión de encabezados, que representa la carga de CPU adicional.

Algunas situaciones requieren una reserva (restablecer la conexión HTTP/2 y en su lugar, establecer una nueva conexión para usar HTTP/1.1) de HTTP/1.1. En concreto, renegociación de TLS y autenticación HTTP (distintos de básica e implícita) requieren reserva HTTP/1.1. Aunque esto agrega sobrecarga, estas operaciones ya implican cierto retraso y por lo que no son especialmente sensibles al rendimiento.

## <a name="see-also"></a>Vea también
- [Optimización del rendimiento de servidor de Web](index.md) 
- [Optimización del rendimiento de IIS 10.0](tuning-iis-10.md)