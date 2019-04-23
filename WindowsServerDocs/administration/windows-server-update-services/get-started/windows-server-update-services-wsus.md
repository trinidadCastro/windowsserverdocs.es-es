---
title: Empezar a trabajar con Windows Server Update Services (WSUS)
description: 'Tema de Windows Server Update Service (WSUS): información general sobre el rol de servidor y sus aplicaciones prácticas'
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: get-started article
ms.assetid: 90e3464c-49d8-4861-96db-ee6f8a09ec5b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 5/22/2017
ms.openlocfilehash: 7a6c64e0a4321553162b426e3d6857ff6ac3581c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830266"
---
# <a name="windows-server-update-services-wsus"></a>Windows Server Update Services (WSUS)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Windows Server Update Services (WSUS) permite a los administradores de tecnologías de la información implementar las actualizaciones de productos de Microsoft más recientes. Puede utilizar WSUS para administrar por completo la distribución de actualizaciones que se publican en Microsoft Update para los equipos de su red. En este tema se proporciona información general sobre este rol de servidor y más información sobre cómo implementar y mantener WSUS.

## <a name="wsus-server-role-description"></a>Descripción del rol de servidor de WSUS
Un servidor WSUS proporciona características que puede usar para administrar y distribuir actualizaciones a través de una consola de administración. Un servidor WSUS también puede ser el origen de actualización para otros servidores WSUS dentro de la organización. El servidor WSUS que actúa como origen de actualización es el servidor que precede en la cadena. En una implementación de WSUS, al menos un servidor WSUS en la red debe ser capaz de conectarse a Microsoft Update para obtener información sobre actualizaciones disponibles. Como administrador, puede determinar - basándose en la seguridad de red y configuración - cuántos otros servidores WSUS conectan directamente a Microsoft Update.

### <a name="practical-applications"></a>Aplicaciones prácticas
La administración de actualizaciones es el proceso por el cual se controla la implementación y el mantenimiento de versiones provisionales de software en entornos de producción. Le permite preservar la eficiencia operativa, superar vulnerabilidades de seguridad y mantener la estabilidad del entorno de producción. Si su organización no puede determinar y mantener un nivel conocido de confianza en sus sistemas operativos y software de aplicación, podría tener una serie de vulnerabilidades de seguridad que, si se explotan, afectarían los ingresos y la propiedad intelectual. Minimizar esta amenaza requiere que tenga sistemas correctamente configurados, que use el software más reciente y que instale las actualizaciones de software recomendadas.

Los escenarios principales en los que WSUS favorece a su negocio son los siguientes:

-   Administración centralizada de actualizaciones

-   Automatización de administración de actualizaciones

### <a name="new-and-changed-functionality"></a>Funcionalidad nueva y modificada

> [!NOTE]
> Actualización desde cualquier versión de Windows Server que sea compatible con WSUS 3.2 a Windows Server 2012 R2 requiere que desinstalar primero WSUS 3.2.
> 
> En Windows Server 2012, actualización desde cualquier versión de Windows Server con WSUS 3.2 instalado se bloquea durante el proceso de instalación si se detecta WSUS 3.2. En ese caso, se le pedirá que desinstale Windows Server Update Services antes de actualizar el servidor.
> 
> Sin embargo, debido a cambios en esta versión de Windows Server y Windows Server 2012 R2, al actualizar desde cualquier versión de Windows Server y WSUS 3.2, no se bloquea la instalación. Error al desinstalar WSUS 3.2 antes de realizar una actualización de Windows Server 2012 R2 hará que la entrada de tareas de instalación de WSUS en Windows Server 2012 R2 a un error. En este caso, la única medida correctiva conocida es formatear el disco duro y volver a instalar Windows Server.

Windows Server Update Services es un rol del servidor integrado que incluye las siguientes mejoras:

-   Puede agregarse o quitarse con el Administrador del servidor.

-   Incluye cmdlets de Windows PowerShell para administrar las tareas administrativas más importantes en WSUS

-   Agrega la capacidad hash SHA256 para una mayor seguridad.

-   Proporciona separación entre cliente y servidor: las versiones de Windows Update Agent (WUA) pueden entregarse independientemente de WSUS

### <a name="using-windows-powershell-to-manage-wsus"></a>Uso de Windows PowerShell para administrar WSUS
Para administrar sus operaciones, los administradores necesitan cobertura en la automatización de línea de comandos. El objetivo principal es facilitar la administración de WSUS al permitir que los administradores del sistema automaticen sus operaciones cotidianas.

**¿Qué valor aporta este cambio?**

Al exponer operaciones principales de WSUS mediante Windows PowerShell, los administradores del sistema pueden aumentar su productividad, reducir la curva de aprendizaje de herramientas nuevas y disminuir los errores debido a expectativas no cumplidas por incoherencia en operaciones similares.

**¿Qué funciona de manera diferente?**

En versiones anteriores del sistema operativo Windows Server, no había cmdlets de Windows PowerShell y la automatización de la administración de actualizaciones era un desafío. Los cmdlets de Windows PowerShell para operaciones de WSUS agregan flexibilidad y agilidad al administrador del sistema.

## <a name="in-this-collection"></a>En esta colección
En esta colección son las siguientes guías para la planeación, implementación y administración de WSUS:

-   [Implementar Windows Server Update Services](../deploy/deploy-windows-server-update-services.md)

-   [Administrar actualizaciones con Windows Server Update Services](../manage/update-management-with-windows-server-update-services.md)


