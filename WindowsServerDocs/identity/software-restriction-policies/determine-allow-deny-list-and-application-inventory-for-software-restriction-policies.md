---
title: "Determinar permitir y denegar lista y un inventario de la aplicación para las directivas de restricción de Software"
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-software-restriction-policies
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0abb73b6-b5d8-4505-8ab1-2f29e4bf0411
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 60e78912284715649938567d66ffb90b9890b1b9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="determine-allow-deny-list-and-application-inventory-for-software-restriction-policies"></a>Determinar permitir y denegar lista y un inventario de la aplicación para las directivas de restricción de Software

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema para profesionales de TI proporciona instrucciones cómo crear un permitir y denegar lista para aplicaciones administrarse mediante Windows Server 2008 y Windows Vista a partir de las directivas de restricción de Software (SRP).

## <a name="introduction"></a>Introducción
Directivas de restricción de software (SRP) es la característica de directiva de grupo que identifica los programas de software que se ejecutan en los equipos de un dominio y controla la capacidad de esos programas para ejecutarse. Usa las directivas de restricción de software para crear una configuración muy restringida para equipos, en los que permites solo específicamente identificadas aplicaciones que se ejecutan. Estos se integran con Microsoft Active Directory Domain Services y la directiva de grupo, pero también pueden configurarse en equipos independientes. Para un punto de partida para SRP, consulta el [las directivas de restricción de Software](software-restriction-policies.md).

A partir de Windows Server 2008 R2 y Windows 7, Windows AppLocker puede usarse en lugar de, o conjuntamente con SRP para una parte de la estrategia de control de la aplicación.

Para obtener información acerca de cómo llevar a cabo tareas específicas con SRP, consulta los siguientes temas:

-   [Trabajar con reglas de directivas de restricción de Software](work-with-software-restriction-policies-rules.md)

-   [Usar las directivas de restricción de Software para ayudar a proteger el equipo contra los Virus de correo electrónico](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

### <a name="what-default-rule-to-choose-allow-or-deny"></a>¿Qué regla predeterminada para elegir: permitir o denegar
Las directivas de restricción de software pueden implementarse en uno de dos modos son la base de la regla predeterminada: lista de permitir o denegar. Puedes crear una directiva que identifica cada aplicación que se puede ejecutar en tu entorno; la regla predeterminada de la directiva está restringido y se bloquean todas las aplicaciones que no se permite explícitamente para ejecutar. También puedes crear una directiva que identifica cada aplicación que no se puede ejecutar; la regla predeterminada es ilimitado y restringe solo las aplicaciones que se enumeran explícitamente.

> [!IMPORTANT]
> El modo Denegar lista puede ser una estrategia de mantenimiento de alto para la organización en relación con el control de la aplicación. Crear y mantener una lista en constante evolución que prohíbe todo el malware y otras aplicaciones problemáticos sería mucho tiempo y susceptible a errores.

### <a name="create-an-inventory-of-your-applications-for-the-allow-list"></a>Crear un inventario de las aplicaciones de la lista de permitidos
Para utilizar eficazmente la regla de permiso de forma predeterminada, debes determinar exactamente qué aplicaciones son necesarias en la organización. Hay herramientas diseñadas para generar un inventario de aplicaciones, como el recopilador de inventario en el Kit de herramientas de compatibilidad de aplicaciones de Microsoft. Pero SRP tiene una función de registro avanzado que te ayudarán a entender exactamente qué aplicaciones se ejecutan en su entorno.

##### <a name="to-discover-which-applications-to-allow"></a>Para descubrir qué aplicaciones permitir

1.  En un entorno de prueba, implementar la directiva de restricción de Software con el conjunto de reglas predeterminadas para Unrestricted y quitar cualquier reglas adicionales. Si habilitas SRP sin obligar a restringir las aplicaciones, SPR podrán supervisar lo que las aplicaciones que se ejecute.

2.  Crea el siguiente valor del registro para habilitar la característica de registro avanzada y establecer la ruta de acceso a dónde debe escribirse el archivo de registro.

    **"HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Safer\ CodeIdentifiers"**

    Valor de cadena: *NameLogFile ruta de acceso NameLogFile*

    Porque SRP es evaluar todas las aplicaciones cuando se ejecutan, se escribe una entrada en el archivo de registro *NameLogFile* cada vez que se ejecuta la aplicación.

3.  Evaluar el archivo de registro

    Cada entrada de registro, los Estados:

    -   El llamador de la directiva de restricción de software y el identificador de proceso (PID) del proceso de llamada

    -   El objetivo evaluado

    -   La regla SRP que se detectaron cuando se ejecutó la aplicación

    -   Un identificador para la regla SRP.

    Un ejemplo de los resultados que se escriben en un archivo de registro:

**Explorer.exe (PID = 4728) identifiedC:\Windows\system32\onenote.exe regla usingpath sin restricciones, Guid = {320bd852-aa7c-4674-82c5-9a80321670a3}** todas las aplicaciones y el código asociado que SRP comprueba y configurado para bloquear se anotarán en el archivo de registro, que puede utilizar para determinar qué archivos ejecutables deben considerarse para la lista de permitidos.


