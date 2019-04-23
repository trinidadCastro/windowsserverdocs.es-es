---
title: Uso de directivas de restricción de software para ayudar a proteger equipos frente a virus de correo electrónico
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-software-restriction-policies
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 02f23979-f832-4e46-bdea-21fd77db35b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 41b4c2399a86ef96d34b62295eda4a1ce9300609
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850676"
---
# <a name="use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus"></a>Uso de directivas de restricción de software para ayudar a proteger equipos frente a virus de correo electrónico

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tema proporciona información de directivas de cómo establecer el control de aplicaciones mediante directivas de restricción de Software (SRP) para ayudar a proteger su equipo contra el principio de virus de correo electrónico con Windows Server 2008 y Windows Vista.

## <a name="introduction"></a>Introducción
Las directivas de restricción de software (SRP) son una característica basada en directivas de grupo que identifica los programas de software que se ejecutan en los equipos de un dominio y controla la capacidad de ejecución de dichos programas. Puede usar las directivas de restricción de software para crear una configuración muy restringida para los equipos, en los que solamente pueden ejecutarse aquellas aplicaciones específicamente identificadas. Se integran con Microsoft Active Directory Domain Services y directiva de grupo, pero también puede configurarse en equipos independientes. Para un punto de partida para SRP, consulte el [Software Restriction Policies](software-restriction-policies.md).

Empezando por Windows Server 2008 R2 y Windows 7, Windows AppLocker puede usarse en lugar de o junto con SRP para una parte de su estrategia de control de la aplicación. 

#### <a name="configure-srp-to-help-protect-against-an-e-mail-virus"></a>Configurar SRP para ayudar a proteger contra los virus de correo electrónico

1.  Revise las prácticas recomendadas para las directivas de restricción de software comprender el funcionamiento de SRP.

    -   [Procedimientos recomendados](software-restriction-policies-technical-overview.md#BKMK_Best_Practices)

    -   [Cómo funcionan las directivas de restricción de Software](https://technet.microsoft.com/library/cc786941(v=WS.10).aspx)

2.  Abra Directivas de restricción de software.

    -   [Para el equipo local](administer-software-restriction-policies.md#BKMK_1)

    -   [Para un dominio, sitio o unidad organizativa y está en un servidor miembro o una estación de trabajo que se ha unido a un dominio](administer-software-restriction-policies.md#BKMK_2)

3.  Si no se ha definido previamente las directivas de restricción de software, crear nuevas directivas de restricción de software.

    -   [Para crear nuevas directivas de restricción de software](administer-software-restriction-policies.md#BKMK_Create_SRP)

4.  Crear una regla de ruta de acceso de la carpeta que el programa de correo electrónico se usa para ejecutar los datos adjuntos de correo electrónico y, a continuación, establezca la seguridad de nivel a **no permitido**.

    -   [Trabajar con reglas de ruta de acceso](work-with-software-restriction-policies-rules.md#BKMK_Path_Rules)

5.  Especificar los tipos de archivo al que se aplica la regla.

    -   [Para agregar o eliminar un tipo de archivo designado](administer-software-restriction-policies.md#BKMK_Add_Del)

6.  Modificar la configuración de directiva para que se apliquen a los usuarios y grupos que desee:

    -   Especificar usuarios o grupos a los que no desea que el objeto de directiva de grupo (GPO) que se aplican configuraciones de directiva.

    -   Excluir los administradores locales de las directivas de restricción de software de una configuración de directiva específica en la directiva de grupo y seguir teniendo el resto de la directiva de grupo se aplican a los administradores.

        -   [Para impedir que los administradores locales apliquen las directivas de restricción de software](administer-software-restriction-policies.md#BKMK_Prevent_Admin)

7.  Probar la directiva.


