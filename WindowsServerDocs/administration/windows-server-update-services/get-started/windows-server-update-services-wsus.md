---
title: Introducción a Windows Server Update Services (WSUS)
description: 'Tema de Windows Server Update Service (WSUS): información general sobre el rol de servidor y sus aplicaciones prácticas'
ms.prod: windows-server
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
ms.openlocfilehash: 89247f91f616233fc6e4967a0457ff34fac221da
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361649"
---
# <a name="windows-server-update-services-wsus"></a>Windows Server Update Services (WSUS)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Windows Server Update Services (WSUS) permite a los administradores de tecnologías de la información implementar las actualizaciones de productos de Microsoft más recientes. Puede usar WSUS para administrar por completo la distribución de actualizaciones que se publican a través de Microsoft Update en los equipos de la red. En este tema se proporciona información general sobre este rol de servidor y más información sobre cómo implementar y mantener WSUS.

## <a name="wsus-server-role-description"></a>Descripción del rol de servidor WSUS
Un servidor WSUS proporciona características que puede usar para administrar y distribuir actualizaciones a través de una consola de administración. Un servidor WSUS también puede ser el origen de actualización para otros servidores WSUS de la organización. El servidor WSUS que actúa como origen de actualización es el servidor que precede en la cadena. En una implementación de WSUS, al menos un servidor WSUS de la red debe ser capaz de conectarse a Microsoft Update para obtener la información de actualización disponible. Como administrador, puede determinar en función de la seguridad y la configuración de la red: Cuántos servidores WSUS se conectan directamente a Microsoft Update.

### <a name="practical-applications"></a>Aplicaciones prácticas
La administración de actualizaciones es el proceso por el cual se controla la implementación y el mantenimiento de versiones provisionales de software en entornos de producción. Le permite preservar la eficiencia operativa, superar vulnerabilidades de seguridad y mantener la estabilidad del entorno de producción. Si su organización no puede determinar y mantener un nivel conocido de confianza en sus sistemas operativos y software de aplicación, podría tener una serie de vulnerabilidades de seguridad que, si se explotan, afectarían los ingresos y la propiedad intelectual. Minimizar esta amenaza requiere que tenga sistemas correctamente configurados, que use el software más reciente y que instale las actualizaciones de software recomendadas.

Los escenarios principales en los que WSUS favorece a su negocio son los siguientes:

-   Administración centralizada de actualizaciones

-   Automatización de administración de actualizaciones

### <a name="new-and-changed-functionality"></a>Funcionalidad nueva y modificada

> [!NOTE]
> La actualización desde cualquier versión de Windows Server que admita WSUS 3,2 a Windows Server 2012 R2 requiere que desinstale primero WSUS 3,2.
> 
> En Windows Server 2012, la actualización desde cualquier versión de Windows Server con WSUS 3,2 instalado se bloquea durante el proceso de instalación si se detecta WSUS 3,2. En ese caso, se le pedirá que desinstale primero Windows Server Update Services antes de actualizar el servidor.
> 
> Sin embargo, debido a los cambios en esta versión de Windows Server y Windows Server 2012 R2, al actualizar desde cualquier versión de Windows Server y WSUS 3,2, la instalación no se bloquea. Si no se desinstala WSUS 3,2 antes de realizar una actualización de Windows Server 2012 R2, se producirá un error en las tareas posteriores a la instalación de WSUS en Windows Server 2012 R2. En este caso, la única medida correctiva conocida consiste en formatear el disco duro y reinstalar Windows Server.

Windows Server Update Services es un rol del servidor integrado que incluye las siguientes mejoras:

-   Puede agregarse o quitarse con el Administrador del servidor.

-   Incluye cmdlets de Windows PowerShell para administrar las tareas administrativas más importantes en WSUS

-   Agrega la capacidad hash SHA256 para una mayor seguridad.

-   Proporciona separación entre cliente y servidor: las versiones del agente de Windows Update (WUA) pueden entregarse independientemente de WSUS

### <a name="using-windows-powershell-to-manage-wsus"></a>Uso de Windows PowerShell para administrar WSUS
Para administrar sus operaciones, los administradores necesitan cobertura en la automatización de línea de comandos. El objetivo principal es facilitar la administración de WSUS al permitir que los administradores del sistema automaticen sus operaciones cotidianas.

**¿Qué valor aporta este cambio?**

Al exponer operaciones principales de WSUS mediante Windows PowerShell, los administradores del sistema pueden aumentar su productividad, reducir la curva de aprendizaje de herramientas nuevas y disminuir los errores debido a expectativas no cumplidas por incoherencia en operaciones similares.

**¿Qué funciona de manera diferente?**

En versiones anteriores del sistema operativo Windows Server, no había cmdlets de Windows PowerShell y la automatización de la administración de actualizaciones era un desafío. Los cmdlets de Windows PowerShell para operaciones de WSUS agregan flexibilidad y agilidad al administrador del sistema.

## <a name="in-this-collection"></a>En esta colección
Las siguientes guías para planear, implementar y administrar WSUS se encuentran en esta colección:

-   [Implementación de Windows Server Update Services](../deploy/deploy-windows-server-update-services.md)

-   [Administrar actualizaciones con Windows Server Update Services](../manage/update-management-with-windows-server-update-services.md)


