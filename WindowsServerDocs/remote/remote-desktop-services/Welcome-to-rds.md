---
title: Servicios de escritorio remota en Windows Server 2016
description: Proporciona una visión general de servicios de escritorio remoto
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: chrimo
ms.date: 02/22/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 52b9e09f-39e0-41a9-9d3b-4d5f4eacf3e0
author: christianmontoya
manager: scottman
ms.localizationpriority: medium
ms.openlocfilehash: cd00f92254f9e55f83442f5e68e344e0aa7579a2
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "1708598"
---
# <a name="welcome-to-remote-desktop-services"></a>Servicios de escritorio remota 

Servicios de escritorio remoto (RDS) es la plataforma de elección para crear soluciones de virtualización para cada cliente final necesita, incluidos ofrecer aplicaciones virtualizadas individuales, proporcionar acceso seguro de escritorio móvil y remoto y proporcionar a los usuarios finales la capacidad para ejecutar las aplicaciones y escritorios desde la nube.

![Introducción a servicios de escritorio remoto](.\media\rds-overview.png)

RDS ofrece flexibilidad para la implementación, eficacia y extensibilidad de costo: ofrecida a través de una gran variedad de opciones de implementación, incluyendo Windows Server 2016 para las implementaciones locales, Microsoft Azure para las implementaciones de la nube y una matriz sólida de socio soluciones.

Según su entorno y preferencias, puede configurar la solución RDS para la virtualización basada en sesión, como una infraestructura de escritorio virtual (VDI), o bien como una combinación de ambas:

- **Virtualización basada en sesión**: aproveche las ventajas de compute de Windows Server para proporcionar un entorno de múltiples sesiones rentable para las cargas de trabajo cotidiano de los usuarios de la unidad
- **VDI**: cliente aprovechar Windows para proporcionar el alto rendimiento, la compatibilidad de la aplicación y la familiaridad que los usuarios han llegado a esperar de su experiencia de escritorio de Windows.

Dentro de estos entornos de virtualización, tiene flexibilidad adicional en publicar para los usuarios:

- **Equipos de sobremesa**: ofrecer a los usuarios una experiencia de escritorio completo con una variedad de aplicaciones que puede instala y administrar. Ideal para los usuarios que se basan en estos equipos como sus estaciones de trabajo principales o que son procedentes de clientes ligeros, como con los servicios de multipunto.
- **RemoteApps**: especifique las aplicaciones individuales que están hospedados o ejecutar en el equipo virtualizado pero aparecen como si se están ejecutando en el escritorio del usuario como aplicaciones locales. Las aplicaciones tiene su propia entrada de la barra de tareas y se pueden cambia el tamaño de y moverse a través de monitores. Ideal para implementar y administrar aplicaciones clave en el entorno seguro, remoto al tiempo que permite a los usuarios trabajar desde y personalizar sus propios escritorios.

Para entornos donde la rentabilidad es crucial y desea ampliar las ventajas de implementar escritorios completos en un entorno de virtualización basada en sesión, puede usar [Servicios multipunto](../multipoint-services/multipoint-services.md) para ofrecer el mejor valor. 

Con estas opciones y configuraciones, tiene la flexibilidad para implementar los escritorios y aplicaciones de que los usuarios necesitan un remoto, seguro, y una forma rentable.

## <a name="next-steps"></a>Pasos siguientes

Estos son algunos pasos que le ayudarán a obtener una mejor comprensión de RDS e incluso iniciar la implementación de su propio entorno:
-   Comprender las [configuraciones admitidas](rds-supported-config.md) para RDS con las distintas versiones de Windows y Windows Server
-   [Planeación y diseño de](rds-plan-and-design.md) un entorno de RDS para dar cabida a los diversos requisitos, como una alta disponibilidad y la autenticación multifactor.
-   Revise los [modelos de arquitectura de servicios de escritorio remoto](desktop-hosting-logical-architecture.md) que funcionan mejor para su entorno deseado.
-   Iniciar la [implementación de su entorno de RDS con ARM y Azure Marketplace](rds-in-azure.md).
