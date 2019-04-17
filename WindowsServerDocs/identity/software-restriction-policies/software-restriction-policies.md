---
title: "Directivas de restricción de software"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="software-restriction-policies"></a>Directivas de restricción de software

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema para profesionales de TI describe las directivas de restricción de Software (SRP) en Windows Server 2012 y Windows 8 y proporciona vínculos a información técnica sobre Windows Server 2003 a partir de SRP.

Para los procedimientos y sugerencias para solucionar problemas, consulta [administrar las directivas de restricción de Software](administer-software-restriction-policies.md) y [solucionar problemas de las directivas de restricción de Software](troubleshoot-software-restriction-policies.md).

## <a name="BKMK_OVER"></a>Descripción de las directivas de restricción de software
Directivas de restricción de software (SRP) es la característica de directiva de grupo que identifica los programas de software que se ejecutan en los equipos de un dominio y controla la capacidad de esos programas para ejecutarse. Las directivas de restricción de software forman parte de la seguridad de Microsoft y la estrategia de administración para ayudar a las empresas a aumentar la confiabilidad, la integridad y la facilidad de uso de sus equipos.

También puedes usar las directivas de restricción de software para crear una configuración muy restringida para equipos, en los que permites solo específicamente identificadas aplicaciones que se ejecutan. Las directivas de restricción de software están integradas con la directiva de grupo y Microsoft Active Directory. También puedes crear las directivas de restricción de software en equipos independientes. Las directivas de restricción de software son las directivas de confianza, que son normativa establecida por un administrador para restringir los scripts y otros códigos que no es de plena confianza de ejecución.

Puedes definir estas directivas a través de la extensión de directivas de restricción de Software del Editor de directivas de grupo Local o el directivas de seguridad Local complemento Microsoft Management Console (MMC).

Para obtener información detallada acerca de SRP, consulta el [Introducción técnica de las directivas de restricción de Software](software-restriction-policies-technical-overview.md).

## <a name="BKMK_APP"></a>Aplicaciones prácticas
Los administradores pueden usar las directivas de restricción de software para las siguientes tareas:

-   Definir qué es el código de confianza

-   Diseñar una directiva de grupo flexible para regular scripts, archivos ejecutables y los controles ActiveX

Las directivas de restricción de software se aplican por el sistema operativo y las aplicaciones (como aplicaciones de scripts) que cumplan con las directivas de restricción de software.

En concreto, los administradores pueden usar las directivas de restricción de software para los fines siguientes:

-   Especificar qué software (archivos ejecutables) se puede ejecutar en los clientes

-   Impedir que los usuarios ejecuten programas específicos en equipos compartidos

-   Especifica quién puede agregar editores de confianza a los clientes

-   Establecer el ámbito de las directivas de restricción de software (especificar si las directivas afectan a todos los usuarios o un subconjunto de usuarios de los clientes)

-   Impedir que se ejecuten en el equipo local, unidad organizativa (OU), sitio o dominio archivos ejecutables. Esto sería adecuado en los casos cuando no estás usando las directivas de restricción de software para solucionar posibles problemas con los usuarios malintencionados.

## <a name="BKMK_NEW"></a>Funcionalidades nuevas y modificadas
No hay ningún cambio en la funcionalidad de las directivas de restricción de Software.

## <a name="BKMK_DEP"></a>Funcionalidad desusada o eliminada
No hay ninguna funcionalidad eliminó o desusada para las directivas de restricción de Software.

## <a name="BKMK_SOFT"></a>Requisitos de software
La extensión de directivas de restricción de Software en el Editor de directivas de grupo Local puede tener acceso a través de la consola MMC.

Las siguientes características son necesarias para crear y mantener las directivas de restricción de software en el equipo local:

-   Editor de directivas de grupo local

-   Windows Installer

-   Authenticode y WinVerifyTrust

Si se llama a tu diseño para la implementación de dominio de estas directivas, además de la lista anterior, las siguientes características son obligatorios:

-   Servicios de dominio de Active Directory

-   Directiva de grupo

## <a name="BKMK_INSTALL"></a>Información del administrador del servidor
Las directivas de restricción de software es una extensión del Editor de directivas de grupo Local y no se instala mediante el administrador del servidor, agregar Roles y características.

## <a name="BKMK_LINKS"></a>Consulta también
La siguiente tabla proporciona vínculos a recursos más importantes comprender y usar SRP.

|Tipo de contenido|Referencias|
|--------|-------|
|**Evaluación del producto**|[Bloqueo de la aplicación con las directivas de restricción de Software](https://technet.microsoft.com/magazine/2008.06.srp.aspx?pr=blog)|
|**Planeación**|[Introducción técnica de las directivas de restricción de software](software-restriction-policies-technical-overview.md) (Windows Server 2012)<br /><br />[Referencia técnica de las directivas de restricción de software](https://technet.microsoft.com/library/cc728085(v=WS.10).aspx) (Windows Server 2003)|
|**Implementación**|No hay recursos disponibles.|
|**Operaciones**|[Administrar las directivas de restricción de Software](administer-software-restriction-policies.md) (Windows Server 2012)<br /><br />[Ayuda del producto de las directivas de restricción de software](https://technet.microsoft.com/library/cc779607(v=WS.10).aspx) (Windows Server 2003)|
|**Solución de problemas**|[Solucionar problemas de las directivas de restricción de Software](troubleshoot-software-restriction-policies.md) (Windows Server 2012)<br /><br />[Solución de problemas de las directivas de restricción de software](https://technet.microsoft.com/library/cc737011(v=WS.10).aspx) (Windows Server 2003)|
|**Seguridad**|[Las directivas de amenazas y contramedidas de restricción de Software](https://technet.microsoft.com/library/dd349795(v=WS.10).aspx) (Windows Server 2008)<br /><br />[Las directivas de amenazas y contramedidas de restricción de Software](https://technet.microsoft.com/library/hh125926(v=WS.10).aspx) (Windows Server 2008 R2)|
|**Herramientas y opciones de configuración**|[Las directivas de restricción de software herramientas y opciones de configuración](https://technet.microsoft.com/library/cc782454(v=WS.10).aspx) (Windows Server 2003)|
|**Recursos de la Comunidad**|[Bloqueo de la aplicación con las directivas de restricción de Software](https://technet.microsoft.com/magazine/2008.06.srp.aspx?pr=blog)|



