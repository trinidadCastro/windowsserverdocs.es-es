---
title: Uso de directivas de restricción de software para ayudar a proteger equipos frente a virus de correo electrónico
description: Obtenga información acerca de cómo establecer directivas de control de aplicaciones mediante directivas de restricción de software (SRP) para ayudar a proteger el equipo frente a virus de correo electrónico a partir de Windows Server 2008 y Windows Vista.
ms.topic: article
ms.assetid: 02f23979-f832-4e46-bdea-21fd77db35b2
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/12/2016
ms.openlocfilehash: bf7da1388ecbe65269ee6d847c86e643b3bc39bd
ms.sourcegitcommit: d42b80f947dbfa8660d982be67d77745a28081e5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/12/2021
ms.locfileid: "98113461"
---
# <a name="use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus"></a>Uso de directivas de restricción de software para ayudar a proteger equipos frente a virus de correo electrónico

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se proporciona información sobre cómo establecer directivas de control de aplicaciones mediante directivas de restricción de software (SRP) para ayudar a proteger el equipo frente a virus de correo electrónico a partir de Windows Server 2008 y Windows Vista.

## <a name="introduction"></a>Introducción
Las directivas de restricción de software (SRP) son una característica basada en directivas de grupo que identifica los programas de software que se ejecutan en los equipos de un dominio y controla la capacidad de ejecución de dichos programas. Puede usar las directivas de restricción de software para crear una configuración muy restringida para los equipos, en los que solamente pueden ejecutarse aquellas aplicaciones específicamente identificadas. Se integran con Microsoft Active Directory Domain Services y directiva de grupo, pero también se pueden configurar en equipos independientes. Para obtener un punto de partida de SRP, consulte las [directivas de restricción de software](software-restriction-policies.md).

A partir de Windows Server 2008 R2 y Windows 7, se puede usar Windows AppLocker en lugar de o junto con SRP para una parte de la estrategia de control de la aplicación.

#### <a name="configure-srp-to-help-protect-against-an-e-mail-virus"></a>Configuración de SRP para ayudar a protegerse frente a un virus de correo electrónico

1.  Revise los procedimientos recomendados para las directivas de restricción de software para comprender el funcionamiento de SRP.

    -   [procedimientos recomendados](software-restriction-policies-technical-overview.md#BKMK_Best_Practices)

    -   [Cómo funcionan las directivas de restricción de software](/previous-versions/windows/it-pro/windows-server-2003/cc786941(v=ws.10))

2.  Abra Directivas de restricción de software.

    -   [Para el equipo local](administer-software-restriction-policies.md#BKMK_1)

    -   [Para un dominio, un sitio o una unidad organizativa, y se encuentra en un servidor miembro o en una estación de trabajo que está unida a un dominio](administer-software-restriction-policies.md#BKMK_2)

3.  Si no ha definido previamente las directivas de restricción de software, cree nuevas directivas de restricción de software.

    -   [Para crear nuevas directivas de restricción de software](administer-software-restriction-policies.md#BKMK_Create_SRP)

4.  Cree una regla de ruta de acceso para la carpeta que utiliza su programa de correo electrónico para ejecutar datos adjuntos de correo electrónico y, a continuación, establezca el nivel de seguridad en no **permitido**.

    -   [Trabajar con reglas de ruta de acceso](work-with-software-restriction-policies-rules.md#BKMK_Path_Rules)

5.  Especifique los tipos de archivo a los que se aplica la regla.

    -   [Para agregar o eliminar un tipo de archivo designado](administer-software-restriction-policies.md#BKMK_Add_Del)

6.  Modifique la configuración de directiva para que se aplique a los usuarios y grupos que desee:

    -   Especifique los usuarios o grupos a los que no desea que se aplique la configuración de directiva (GPO) del objeto directiva de grupo.

    -   Excluya los administradores locales de las directivas de restricción de software de una configuración de directiva específica en directiva de grupo y siga teniendo el resto de directiva de grupo se apliquen a los administradores.

        -   [Para evitar que las directivas de restricción de software se apliquen a los administradores locales](administer-software-restriction-policies.md#BKMK_Prevent_Admin)

7.  Pruebe la Directiva.
