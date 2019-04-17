---
title: "Usar las directivas de restricción de Software para ayudar a proteger el equipo contra los Virus de correo electrónico"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus"></a>Usar las directivas de restricción de Software para ayudar a proteger el equipo contra los Virus de correo electrónico

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tema proporciona información de directivas de cómo establecer el control de la aplicación con las directivas de restricción de Software (SRP) para ayudar a proteger el equipo contra el correo electrónico virus a partir de Windows Server 2008 y Windows Vista.

## <a name="introduction"></a>Introducción
Directivas de restricción de software (SRP) es la característica de directiva de grupo que identifica los programas de software que se ejecutan en los equipos de un dominio y controla la capacidad de esos programas para ejecutarse. Usa las directivas de restricción de software para crear una configuración muy restringida para equipos, en los que permites solo específicamente identificadas aplicaciones que se ejecutan. Estos se integran con Microsoft Active Directory Domain Services y la directiva de grupo, pero también pueden configurarse en equipos independientes. Para un punto de partida para SRP, consulta el [las directivas de restricción de Software](software-restriction-policies.md).

A partir de Windows Server 2008 R2 y Windows 7, Windows AppLocker puede usarse en lugar de, o conjuntamente con SRP para una parte de la estrategia de control de la aplicación. 

#### <a name="configure-srp-to-help-protect-against-an-e-mail-virus"></a>Configurar SRP para ayudar a proteger contra virus de correo electrónico

1.  Revisa los procedimientos recomendados para las directivas de restricción de software comprender el funcionamiento de SRP.

    -   [Procedimientos recomendados](software-restriction-policies-technical-overview.md#BKMK_Best_Practices)

    -   [Cómo funcionan las directivas de restricción de Software](https://technet.microsoft.com/library/cc786941(v=WS.10).aspx)

2.  Abre las directivas de restricción de Software.

    -   [Para el equipo local](administer-software-restriction-policies.md#BKMK_1)

    -   [Para un dominio, sitio, o unidad organizativa y está en un servidor miembro o en una estación de trabajo que se ha unido a un dominio](administer-software-restriction-policies.md#BKMK_2)

3.  Si no ha definido previamente las directivas de restricción de software, crear nuevas directivas de restricción de software.

    -   [Para crear nuevas directivas de restricción de software](administer-software-restriction-policies.md#BKMK_Create_SRP)

4.  Crear una regla de ruta de acceso para la carpeta que el programa de correo electrónico se usa para ejecutar datos adjuntos de correo electrónico y, a continuación, Establece la seguridad nivel a **no permitido**.

    -   [Trabajar con reglas de ruta de acceso](work-with-software-restriction-policies-rules.md#BKMK_Path_Rules)

5.  Especificar los tipos de archivo al que se aplica la regla.

    -   [Para agregar o eliminar un tipo de archivo designados](administer-software-restriction-policies.md#BKMK_Add_Del)

6.  Modificar la configuración de directiva para que se aplican a los usuarios y grupos que desees:

    -   Especificar usuarios o grupos a los que no desea que el objeto de directiva de grupo (GPO) para aplicar la configuración de directiva de.

    -   Excluir administradores locales de las directivas de restricción de software de una configuración de directiva específica en la directiva de grupo y seguir teniendo el resto de la directiva de grupo se aplican a los administradores.

        -   [Para impedir que las directivas de restricción de software se apliquen a los administradores locales](administer-software-restriction-policies.md#BKMK_Prevent_Admin)

7.  Probar la directiva.


