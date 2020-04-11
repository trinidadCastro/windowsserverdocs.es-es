---
title: Introducción a Windows Server Update Services (WSUS)
description: 'Tema de Windows Server Update Services (WSUS): información general sobre el rol de servidor y sus aplicaciones prácticas'
ms.prod: windows-server
ms.technology: manage-wsus
ms.topic: get-started article
ms.assetid: 90e3464c-49d8-4861-96db-ee6f8a09ec5b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 5/22/2017
ms.openlocfilehash: 3bb365c627840d4152b0dc450e24dd83d82103ca
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80828638"
---
# <a name="windows-server-update-services-wsus"></a>Windows Server Update Services (WSUS)

>Se aplica a: Windows Server (Canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Windows Server Update Services (WSUS) permite a los administradores de tecnologías de la información implementar las actualizaciones de productos de Microsoft más recientes. Puedes usar WSUS para administrar por completo la distribución de actualizaciones que se publican a través de Microsoft Update a los equipos de la red. En este tema se proporciona información general sobre este rol de servidor y más información sobre cómo implementar y mantener WSUS.

## <a name="wsus-server-role-description"></a>Descripción del rol de servidor WSUS
Un servidor WSUS proporciona las características que puedes usar para administrar y distribuir actualizaciones mediante una consola de administración. Un servidor WSUS también puede ser el origen de actualización para otros servidores WSUS de la organización. El servidor WSUS que actúa como origen de actualización es el servidor que precede en la cadena. En una implementación de WSUS, al menos un servidor WSUS de la red debe tener la capacidad de conectarse a Microsoft Update para obtener información sobre las actualizaciones disponibles. Como administrador puedes determinar, en función de la configuración y seguridad de red, la cantidad de servidores WSUS que quieres que se conecten directamente a Microsoft Update.

### <a name="practical-applications"></a>Aplicaciones prácticas
La administración de actualizaciones es el proceso por el cual se controla la implementación y el mantenimiento de versiones provisionales de software en entornos de producción. Le permite preservar la eficiencia operativa, superar vulnerabilidades de seguridad y mantener la estabilidad del entorno de producción. Si su organización no puede determinar y mantener un nivel conocido de confianza en sus sistemas operativos y software de aplicación, podría tener una serie de vulnerabilidades de seguridad que, si se explotan, afectarían los ingresos y la propiedad intelectual. Minimizar esta amenaza requiere que tenga sistemas correctamente configurados, que use el software más reciente y que instale las actualizaciones de software recomendadas.

Los escenarios principales en los que WSUS favorece a su negocio son los siguientes:

-   Administración centralizada de actualizaciones

-   Automatización de administración de actualizaciones

### <a name="new-and-changed-functionality"></a>Funcionalidad nueva y modificada

> [!NOTE]
> Para proceder a la actualización desde cualquier versión de Windows Server que sea compatible con WSUS 3.2 a Windows Server 2012 R2 es preciso desinstalar primero WSUS 3.2.
> 
> En Windows Server 2012, se bloqueará la actualización desde cualquier versión de Windows Server con WSUS 3.2 instalado durante el proceso de instalación si se detecta WSUS 3.2. En ese caso, se te pedirá que desinstales primero Windows Server Update Services antes de actualizar el servidor.
> 
> Sin embargo, debido a los cambios en esta versión de Windows Server y Windows Server 2012 R2, al proceder a la actualización desde cualquier versión de Windows Server y WSUS 3.2, la instalación no se bloquea. El hecho de no desinstalar WSUS 3.2 antes de realizar una actualización de Windows Server 2012 R2 provocará un error en las tareas de instalación posterior de WSUS en Windows Server 2012 R2. En ese caso, la única solución conocida consiste en formatear el disco duro y reinstalar Windows Server.

Windows Server Update Services es un rol del servidor integrado que incluye las siguientes mejoras:

-   Puede agregarse o quitarse con el Administrador del servidor.

-   Incluye cmdlets de Windows PowerShell para administrar las tareas administrativas más importantes en WSUS.

-   Agrega la capacidad hash SHA256 para una mayor seguridad.

-   Proporciona separación entre cliente y servidor: las versiones del Agente de Windows Update (WUA) pueden entregarse independientemente de WSUS.

### <a name="using-windows-powershell-to-manage-wsus"></a>Uso de Windows PowerShell para administrar WSUS
Para administrar sus operaciones, los administradores necesitan cobertura en la automatización de línea de comandos. El objetivo principal es facilitar la administración de WSUS al permitir que los administradores del sistema automaticen sus operaciones cotidianas.

**¿Qué valor aporta este cambio?**

Al exponer operaciones principales de WSUS mediante Windows PowerShell, los administradores del sistema pueden aumentar su productividad, reducir la curva de aprendizaje de herramientas nuevas y disminuir los errores debido a expectativas no cumplidas por incoherencia en operaciones similares.

**¿Qué funciona de manera diferente?**

En versiones anteriores del sistema operativo Windows Server, no había cmdlets de Windows PowerShell y la automatización de la administración de actualizaciones era un desafío. Los cmdlets de Windows PowerShell para operaciones de WSUS agregan flexibilidad y agilidad al administrador del sistema.

## <a name="in-this-collection"></a>En esta colección
Las siguientes guías para planear, implementar y administrar WSUS se encuentran en esta colección:

-   [Implementación de Windows Server Update Services](../deploy/deploy-windows-server-update-services.md)

-   [Administrar las actualizaciones con Windows Server Update Services](../manage/update-management-with-windows-server-update-services.md)


