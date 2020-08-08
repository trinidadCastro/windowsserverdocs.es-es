---
title: Directivas de restricción de software
description: Seguridad de Windows Server
ms.topic: article
ms.assetid: 5c0befad-07c3-4262-b418-372d01850305
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 44f917beaa7b1e13171d2c8ade6f0172b450350d
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87953022"
---
# <a name="software-restriction-policies"></a>Directivas de restricción de software

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema para profesionales de TI se describen las directivas de restricción de software (SRP) en Windows Server 2012 y Windows 8, y se proporcionan vínculos a información técnica sobre SRP a partir de Windows Server 2003.

Para conocer los procedimientos y las sugerencias para solucionar problemas, vea [Administrar directivas de restricción de software](administer-software-restriction-policies.md) y [solucionar problemas de directivas de restricción de software](troubleshoot-software-restriction-policies.md).

## <a name="software-restriction-policies-description"></a><a name="BKMK_OVER"></a>Descripción de las directivas de restricción de software
Las directivas de restricción de software (SRP) son una característica basada en directivas de grupo que identifica los programas de software que se ejecutan en los equipos de un dominio y controla la capacidad de ejecución de dichos programas. Las directivas de restricción de software forman parte de la estrategia de administración y seguridad de Microsoft para ayudar a las empresas a aumentar la confiabilidad, la integridad y la capacidad de administración de los equipos.

También puede usar las directivas de restricción de software para crear una configuración altamente restringida para los equipos, en los que solamente pueden ejecutarse aquellas aplicaciones específicamente identificadas. Las directivas de restricción de software están integradas en Microsoft Active Directory y la directiva de grupo. También puede crear directivas de restricción de software en equipos independientes. Las directivas de restricción de software son directivas de confianza, las cuáles son normativas establecidas por un administrador para restringir los scripts y otros códigos que no son de confianza plena.

Puede definir estas directivas a través de la extensión de las directivas de restricción de software del Editor de directivas del grupo local o el complemento de directivas de seguridad local para Microsoft Management Console (MMC).

Para obtener información detallada sobre SRP, consulte [Información técnica de directivas de restricción de software](software-restriction-policies-technical-overview.md).

## <a name="practical-applications"></a><a name="BKMK_APP"></a>Aplicaciones prácticas
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

## <a name="new-and-changed-functionality"></a><a name="BKMK_NEW"></a>Funcionalidad nueva y modificada
No hay cambios de funcionalidad en las directivas de restricción de software.

## <a name="removed-or-deprecated-functionality"></a><a name="BKMK_DEP"></a>Funcionalidad eliminada o en desuso
No hay funcionalidades eliminadas ni desusadas en las directivas de restricción de software.

## <a name="software-requirements"></a><a name="BKMK_SOFT"></a>Requisitos de software
Se puede acceder a la extensión de las directivas de restricción de software para el Editor de directivas de grupo local a través de MMC.

Se necesitan las siguientes características para crear y mantener las directivas restricción de software en el equipo local:

-   Editor de directivas de grupo local

-   Windows Installer

-   Authenticode y WinVerifyTrust

Si el diseño requiere la implementaciones de estas directivas en el dominio, además de la lista anterior se necesitarán las siguientes características:

-   Active Directory Domain Services

-   Directiva de grupo

## <a name="server-manager-information"></a><a name="BKMK_INSTALL"></a>Información sobre el Administrador del servidor
Las directivas de restricción de software son una extensión del Editor de directivas de grupo local y no se instalan a través de Administrador del servidor, Agregar roles y características.

## <a name="see-also"></a><a name="BKMK_LINKS"></a>Vea también
En la tabla siguiente se proporcionan vínculos a recursos importantes para comprender y usar SRP.

|Tipo de contenido|Referencias|
|--------|-------|
|**Evaluación del producto**|[Bloqueo de la aplicación con directivas de restricción de software](/previous-versions/technet-magazine/cc510322(v=msdn.10)?pr=blog)|
|**Planeamiento**|[Información técnica de directivas de restricción de software](software-restriction-policies-technical-overview.md) (Windows Server 2012)<p>[Referencia técnica de directivas de restricción de software](/previous-versions/windows/it-pro/windows-server-2003/cc728085(v=ws.10)) (Windows Server 2003)|
|**Implementación**|No hay recursos disponibles.|
|**Operaciones**|[Administrar directivas de restricción de software](administer-software-restriction-policies.md) (Windows Server 2012)<p>[Ayuda del producto de las directivas de restricción de software](/previous-versions/windows/it-pro/windows-server-2003/cc779607(v=ws.10)) (Windows Server 2003)|
|**Solución de problemas**|[Solucionar problemas de directivas de restricción de software](troubleshoot-software-restriction-policies.md) (Windows Server 2012)<p>[Solución de problemas de directivas de restricción de software](/previous-versions/windows/it-pro/windows-server-2003/cc737011(v=ws.10)) (Windows Server 2003)|
|**Seguridad**|[Amenazas y contramedidas para las directivas de restricción de software](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349795(v=ws.10)) (Windows Server 2008)<p>[Amenazas y contramedidas para las directivas de restricción de software](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/hh125926(v=ws.10)) (Windows Server 2008 R2)|
|**Herramientas y configuración**|[Herramientas y configuración de las directivas de restricción de software](/previous-versions/windows/it-pro/windows-server-2003/cc782454(v=ws.10)) (Windows Server 2003)|
|**Recursos de la comunidad**|[Bloqueo de la aplicación con directivas de restricción de software](/previous-versions/technet-magazine/cc510322(v=msdn.10)?pr=blog)|
