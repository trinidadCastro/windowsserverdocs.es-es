---
title: 'Servicios de Escritorio remoto: creación en cualquier lugar'
description: Información de planeamiento para ayudarte a determinar dónde hospedar tu implementación de RDS.
ms.topic: article
ms.assetid: c803a383-0eea-4e11-bca5-d204ab758048
author: lizap
ms.author: elizapo
ms.date: 09/07/2016
manager: dongill
ms.openlocfilehash: 0956fec88fc0bc07fb964d0eb4c2ff0b976628bf
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87946378"
---
# <a name="remote-desktop-services---build-anywhere"></a>Servicios de Escritorio remoto: creación en cualquier lugar

>Se aplica a: Windows Server (Canal semianual), Windows Server 2019 y Windows Server 2016

Impleméntalo en el entorno local, en la nube o en un híbrido de los dos. Modifica la implementación a medida que cambien tus necesidades empresariales.

Independientemente de dónde te encuentres, la [arquitectura](desktop-hosting-logical-architecture.md) subyacente del entorno de Servicios de Escritorio remoto sigue siendo la misma:
- Todavía debes tener un servidor con conexión a Internet para utilizar Acceso web de Escritorio remoto y Puerta de enlace de Escritorio remoto para usuarios externos.
- Todavía debes tener una instancia de Active Directory y, para los entornos de alta disponibilidad, una base de datos SQL para hospedar las propiedades del usuario y del Escritorio remoto.
- Aún debes tener acceso a la comunicación entre los roles de infraestructura de Escritorio remoto (Agente de conexión a Escritorio remoto, Puerta de enlace de Escritorio remoto, Licencia de Escritorio remoto y Acceso web a Escritorio remoto) y los hosts RDSH o RDVH finales para poder conectar a los usuarios finales con sus escritorios o aplicaciones.

Esta flexibilidad te permite obtener lo mejor de ambos mundos:
- La simplicidad y los métodos de pago por uso asociados a la nube y al mundo conectado.
- La familiaridad y la manera sencilla de aprovechar los recursos pesados que ya existen en el entorno local.

Para más información, echa un vistazo a cómo [crear y poner en marcha la implementación de Servicios de Escritorio remoto](rds-build-and-deploy.md).