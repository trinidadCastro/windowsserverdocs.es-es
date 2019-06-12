---
title: Servicios de escritorio remota en Windows Server 2016
description: Proporciona información general de servicios de escritorio remoto
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
ms.openlocfilehash: 3d148c99911be0cebfc29429d93241f24c2b9606
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66453009"
---
# <a name="welcome-to-remote-desktop-services"></a>Bienvenida a Servicios de Escritorio remoto 

Servicios de escritorio remoto (RDS) es la plataforma de elección para la creación de soluciones de virtualización para cada necesidad del cliente final, incluyendo la entrega de aplicaciones virtualizadas individuales, proporcionar acceso seguro de escritorio remoto y móvil y proporcionar a los usuarios finales el capacidad para ejecutar sus aplicaciones y escritorios desde la nube.

![Información general de servicios de escritorio remoto](./media/rds-overview.png)

RDS ofrece flexibilidad de implementación, extensibilidad y la eficacia de costos: ofrecida a través de una variedad de opciones de implementación, incluido Windows Server 2016 para las implementaciones locales, Microsoft Azure para implementaciones en la nube y una matriz sólida de asociados soluciones.

Dependiendo de su entorno y preferencias, puede configurar la solución RDS para la virtualización basada en sesión, como una infraestructura de escritorio virtual (VDI), o como una combinación de ambos:

- **Virtualización basada en sesión**: Aproveche la potencia de proceso de Windows Server para proporcionar un entorno multisesión rentable para cargas de trabajo de usuarios diario de unidad
- **VDI**: Aproveche el cliente de Windows para proporcionar el alto rendimiento, la compatibilidad de aplicaciones y la familiaridad que los usuarios han llegado a esperar de su experiencia de escritorio de Windows.

En estos entornos de virtualización tienen mayor flexibilidad en lo que publica para los usuarios:

- **Escritorios**: Ofrecer una experiencia de escritorio completa con una variedad de aplicaciones que instalar y administrar a los usuarios. Ideal para los usuarios que se basan en estos equipos como sus estaciones de trabajo principales o que proceden de clientes ligeros, como con MultiPoint Services.
- **RemoteApps**: Especifique las aplicaciones individuales que están hospedados o ejecutar en el equipo virtualizado, pero aparecen como si se están ejecutando en el escritorio del usuario, como las aplicaciones locales. Las aplicaciones tiene su propia entrada de la barra de tareas y se pueden cambiar el tamaño y moverse en varios monitores. Ideal para implementar y administrar las aplicaciones clave en el entorno remoto seguro al tiempo que permite a los usuarios trabajar desde y personalizar sus propios escritorios.

Para entornos donde la rentabilidad es crucial y desea extender las ventajas de la implementación de escritorios completas en un entorno de virtualización basada en sesión, puede usar [MultiPoint Services](../multipoint-services/multipoint-services.md) para ofrecer el mejor valor. 

Con estas opciones y configuraciones, tiene la flexibilidad de implementar los escritorios y aplicaciones que necesitan los usuarios en un control remoto, seguro y forma rentable.

## <a name="next-steps"></a>Pasos siguientes

Estos son algunos pasos que le ayudarán a obtener una mejor comprensión de RDS e incluso empezar a implementar su propio entorno:
-   Comprender la [configuraciones admitidas](rds-supported-config.md) para RDS con las versiones de distintos de Windows y Windows Server
-   [Planear y diseñar](rds-plan-and-design.md) un entorno de RDS para adaptarse a distintos requisitos, como alta disponibilidad y la autenticación multifactor.
-   Revise el [los modelos de arquitectura de servicios de escritorio remoto](desktop-hosting-logical-architecture.md) que funcionará mejor en su entorno deseado.
-   Empezar a [implementar el entorno de RDS con ARM y Azure Marketplace](rds-in-azure.md).
