---
title: 'Servicios de escritorio remoto: compilar en cualquier lugar'
description: Información de planeación para ayudarle a determinar dónde desea hospedar su implementación de RDS.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c803a383-0eea-4e11-bca5-d204ab758048
author: lizap
ms.author: elizapo
ms.date: 09/07/2016
manager: dongill
ms.openlocfilehash: cbb8e73d753b1fe4f0293cf4427c634020a23a42
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869516"
---
# <a name="remote-desktop-services---build-anywhere"></a>Servicios de escritorio remoto: compilar en cualquier lugar

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Implementar de forma local en la nube o un híbrido de los dos. Modificar la implementación según sus necesidades empresariales cambien.

Independientemente de dónde se encuentre, subyacente [arquitectura](desktop-hosting-logical-architecture.md) de los servicios de escritorio remoto entorno sigue siendo el mismo:
- Todavía debe tener un servidor con conexión a internet para usar acceso Web de escritorio remoto y puerta de enlace de escritorio remoto para los usuarios externos
- Todavía debe tener un Active Directory y--para entornos de alta disponibilidad, una instancia de SQL de base de datos al usuario de casa y propiedades de escritorio remoto
- Todavía debe tener acceso de comunicación entre los roles de infraestructura de escritorio remoto (RD Connection Broker, puerta de enlace de escritorio remoto, licencias de escritorio remoto y acceso Web de RD) y el extremo RDSH o hosts RDVH para poder conectarse a los usuarios finales a sus equipos de escritorio o aplicaciones.

Esta flexibilidad le permite obtener el mejor de ambos mundos:
- Los métodos de pago por uso y simplicidad asociados a la nube y el mundo en línea.
- La familiaridad y las complicaciones de la manera de aprovechar muchos recursos que ya existen en el entorno local.

Para obtener más información, veamos cómo [compilar e implementar la implementación de servicios de escritorio remoto](rds-build-and-deploy.md).