---
title: Directivas de restricción de software
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-software-restriction-policies
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5c0befad-07c3-4262-b418-372d01850305
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: ab44013b947d33adc12c54b527415bf16c46a4c6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875826"
---
# <a name="software-restriction-policies"></a>Directivas de restricción de software

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema para profesionales de TI describe las directivas de restricción de Software (SRP) en Windows Server 2012 y Windows 8 y proporciona vínculos a información técnica sobre SRP a partir Windows Server 2003.

Para los procedimientos y sugerencias para solucionar problemas, consulte [administrar directivas de restricción de Software](administer-software-restriction-policies.md) y [solucionar problemas de directivas de restricción de Software](troubleshoot-software-restriction-policies.md).

## <a name="BKMK_OVER"></a>Descripción de las directivas de restricción de software
Las directivas de restricción de software (SRP) son una característica basada en directivas de grupo que identifica los programas de software que se ejecutan en los equipos de un dominio y controla la capacidad de ejecución de dichos programas. Las directivas de restricción de software forman parte de la estrategia de administración y seguridad de Microsoft para ayudar a las empresas a aumentar la confiabilidad, la integridad y la capacidad de administración de los equipos.

También puede usar las directivas de restricción de software para crear una configuración altamente restringida para los equipos, en los que solamente pueden ejecutarse aquellas aplicaciones específicamente identificadas. Las directivas de restricción de software están integradas en Microsoft Active Directory y la directiva de grupo. También puede crear directivas de restricción de software en equipos independientes. Las directivas de restricción de software son directivas de confianza, las cuáles son normativas establecidas por un administrador para restringir los scripts y otros códigos que no son de confianza plena.

Puede definir estas directivas a través de la extensión de las directivas de restricción de software del Editor de directivas del grupo local o el complemento de directivas de seguridad local para Microsoft Management Console (MMC).

Para obtener información detallada sobre SRP, consulte la [Software Restriction Policies Technical Overview](software-restriction-policies-technical-overview.md).

## <a name="BKMK_APP"></a>Aplicaciones prácticas
Los administradores pueden usar las directivas de restricción de software para realizar las siguientes tareas:

-   Definir el código de confianza

-   Diseñar una directiva de grupo flexible para regular scripts, archivos ejecutables y controles ActiveX

El sistema operativo y las aplicaciones (como las aplicaciones de scripting) que cumplen con las directivas de restricción de software aplican las directivas de restricción de software.

Concretamente, los administradores pueden usar las directivas de restricción de software con los siguientes propósitos:

-   Especificar qué software (archivos ejecutables) pueden ejecutarse en los clientes.

-   Evitar que los usuarios ejecuten programas específicos en equipos compartidos.

-   Especificar qué personas pueden agregar editores de confianza en los clientes.

-   Especificar el alcance de las directivas de restricción de software (especificar si las directivas afectarán a todos los usuarios o a un subconjunto de usuarios en los clientes).

-   Evitar que los archivos ejecutables se ejecuten en el equipo local, la unidad organizativa (OU), el sitio o el dominio. Esto resultaría apropiado cuando no está usando directivas de restricción de software para tratar problemas potenciales con usuarios malintencionados.

## <a name="BKMK_NEW"></a>Funcionalidad nueva y modificada
No hay cambios de funcionalidad en las directivas de restricción de software.

## <a name="BKMK_DEP"></a>Funcionalidad desusada o eliminada
No hay funcionalidades eliminadas ni desusadas en las directivas de restricción de software.

## <a name="BKMK_SOFT"></a>Requisitos de software
Se puede acceder a la extensión de las directivas de restricción de software para el Editor de directivas de grupo local a través de MMC.

Se necesitan las siguientes características para crear y mantener las directivas restricción de software en el equipo local:

-   Editor de directivas de grupo local

-   Windows Installer

-   Authenticode y WinVerifyTrust

Si el diseño requiere la implementaciones de estas directivas en el dominio, además de la lista anterior se necesitarán las siguientes características:

-   Active Directory Domain Services

-   Directiva de grupo

## <a name="BKMK_INSTALL"></a>Información del administrador del servidor
Las directivas de restricción de software son una extensión del Editor de directivas de grupo local y no se instalan a través de Administrador del servidor, Agregar roles y características.

## <a name="BKMK_LINKS"></a>Vea también
En la tabla siguiente se proporcionan vínculos a recursos importantes para comprender y usar SRP.

|Tipo de contenido|Referencias|
|--------|-------|
|**Evaluación del producto**|[Bloqueo de la aplicación con directivas de restricción de Software](https://technet.microsoft.com/magazine/2008.06.srp.aspx?pr=blog)|
|**Planeamiento**|[Introducción técnica de directivas de restricción de software](software-restriction-policies-technical-overview.md) (Windows Server 2012)<br /><br />[Referencia técnica de directivas de restricción de software](https://technet.microsoft.com/library/cc728085(v=WS.10).aspx) (Windows Server 2003)|
|**Implementación**|No hay recursos disponibles.|
|**Operaciones**|[Administrar las directivas de restricción de Software](administer-software-restriction-policies.md) (Windows Server 2012)<br /><br />[Ayuda del producto de las directivas de restricción de software](https://technet.microsoft.com/library/cc779607(v=WS.10).aspx) (Windows Server 2003)|
|**Solución de problemas**|[Solucionar problemas de directivas de restricción de Software](troubleshoot-software-restriction-policies.md) (Windows Server 2012)<br /><br />[Solución de problemas de las directivas de restricción de software](https://technet.microsoft.com/library/cc737011(v=WS.10).aspx) (Windows Server 2003)|
|**Seguridad**|[Amenazas y contramedidas para las directivas de restricción de software](https://technet.microsoft.com/library/dd349795(v=WS.10).aspx) (Windows Server 2008)<br /><br />[Amenazas y contramedidas para las directivas de restricción de software](https://technet.microsoft.com/library/hh125926(v=WS.10).aspx) (Windows Server 2008 R2)|
|**Herramientas y configuración**|[Herramientas y configuraciones de las directivas de restricción de software](https://technet.microsoft.com/library/cc782454(v=WS.10).aspx) (Windows Server 2003)|
|**Recursos de la comunidad**|[Bloqueo de la aplicación con directivas de restricción de Software](https://technet.microsoft.com/magazine/2008.06.srp.aspx?pr=blog)|



