---
title: Introducción a Servicios de Escritorio remoto en Windows Server 2016
description: Proporciona información general acerca de Servicios de Escritorio remoto
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
ms.openlocfilehash: 1fcb3fb7e2989399d908a1b5bed7bd21240efeab
ms.sourcegitcommit: 2ec5e779c00481b13f186e2c56d207007897cfd4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/16/2019
ms.locfileid: "71021652"
---
# <a name="welcome-to-remote-desktop-services"></a>Bienvenida a Servicios de Escritorio remoto 

Servicios de Escritorio remoto (RDS) es la plataforma elegida para la creación de soluciones de virtualización que cubran todas las necesidades del cliente final, lo que incluye la entrega de aplicaciones virtualizadas individuales, proporcionar acceso seguro al escritorio remoto y a dispositivos móviles, y proporcionar a los usuarios finales el capacidad para ejecutar sus aplicaciones y escritorios desde la nube.

![Introducción a Servicios de Escritorio remoto](./media/rds-overview.png)

RDS ofrece flexibilidad de implementación, reducción de gastos y capacidad de ampliación (y todo ello se ofrece a través de varias de opciones de implementación, que incluyen Windows Server 2016 para implementaciones locales, Microsoft Azure para implementaciones en la nube y una sólida matriz de soluciones de asociados).

En función de tu entorno y preferencias, puedes configurar la solución RDS para la virtualización basada en sesión, como una infraestructura de escritorio virtual (VDI), o como una combinación de ambos:

- **Virtualización basada en sesión**: Aprovecha la eficacia de proceso de Windows Server para proporcionar un entorno multisesión rentable que impulse las cargas de trabajo diarias de los usuarios.
- **VDI**: Aprovecha el cliente de Windows para proporcionar el alto rendimiento, la compatibilidad de aplicaciones y la familiaridad que los usuarios esperan de su experiencia de escritorio de Windows.

En estos entornos de virtualización tienes mayor flexibilidad con respecto a lo que publicas para los usuarios:

- **Escritorios**: Ofrece a los usuarios una experiencia de escritorio completa con varias aplicaciones que se instala y administran. Es ideal para aquellos usuarios que usan estos equipos como sus estaciones de trabajo principales o que proceden de clientes ligeros, como con MultiPoint Services.
- **RemoteApps**: Especifica aplicaciones individuales que se hospedan o ejecutan en la máquina virtualizada, pero aparecen como si se ejecutaran en el escritorio del usuario como aplicaciones locales. Las aplicaciones tiene su propia entrada de la barra de tareas y se pueden cambiar de tamaño y moverse de un monitor a otro. Es ideal para implementar y administrar aplicaciones clave en un entorno remoto seguro, al tiempo que permite a los usuarios no solo trabajar desde sus escritorios, sino también personalizarlos.

En aquellos entornos en los que la contención de costos es crucial y en los que quieres aumentar las ventajas de implementar de escritorios completos en un entorno de virtualización basado en sesión, puedes usar [MultiPoint Services](../multipoint-services/multipoint-services.md) para ofrecer el máximo valor. 

Con estas opciones y configuraciones, tienes la flexibilidad de implementar los escritorios y aplicaciones que necesitan los usuarios de forma remota, segura y rentable.

## <a name="next-steps"></a>Pasos siguientes

Estos son los pasos que te ayudarán a conocer mejor RDS, e incluso a empezar a implementar tu propio entorno:
-   Conocer las [configuraciones admitidas](rds-supported-config.md) para RDS en las distintas versiones de Windows y Windows Server
-   [Planear y diseñar](rds-plan-and-design.md) un entorno de RDS que se adapte a distintos requisitos, como una alta disponibilidad y autenticación multifactor.
-   Examinar los [modelos de arquitectura de Servicios de Escritorio remoto](desktop-hosting-logical-architecture.md) que mejor funcionan para un entorno concreto.
-   Empezar a [implementar un entorno de RDS con ARM and Azure Marketplace](rds-in-azure.md).
