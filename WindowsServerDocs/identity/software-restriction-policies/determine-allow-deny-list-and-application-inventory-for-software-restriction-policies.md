---
title: Determinación de la lista de permisos y denegaciones y del inventario de aplicaciones para directivas de restricción de software
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847296"
---
# <a name="determine-allow-deny-list-and-application-inventory-for-software-restriction-policies"></a>Determinación de la lista de permisos y denegaciones y del inventario de aplicaciones para directivas de restricción de software

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema para profesionales de TI ofrece instrucciones de cómo crear un permitir y denegar la lista de aplicaciones administrarlos con directivas de restricción de Software (SRP) a partir de Windows Server 2008 y Windows Vista.

## <a name="introduction"></a>Introducción
Las directivas de restricción de software (SRP) son una característica basada en directivas de grupo que identifica los programas de software que se ejecutan en los equipos de un dominio y controla la capacidad de ejecución de dichos programas. Puede usar las directivas de restricción de software para crear una configuración muy restringida para los equipos, en los que solamente pueden ejecutarse aquellas aplicaciones específicamente identificadas. Se integran con Microsoft Active Directory Domain Services y directiva de grupo, pero también puede configurarse en equipos independientes. Para un punto de partida para SRP, consulte el [Software Restriction Policies](software-restriction-policies.md).

Empezando por Windows Server 2008 R2 y Windows 7, Windows AppLocker puede usarse en lugar de o junto con SRP para una parte de su estrategia de control de la aplicación.

Para obtener información acerca de cómo realizar tareas específicas con SRP, consulte lo siguiente:

-   [Trabajar con reglas de directivas de restricción de Software](work-with-software-restriction-policies-rules.md)

-   [Utilizar directivas de restricción de Software para ayudar a proteger su equipo contra Virus de correo electrónico](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

### <a name="what-default-rule-to-choose-allow-or-deny"></a>¿Qué regla predeterminada para elegir: Permitir o denegar
Las directivas de restricción de software se pueden implementar en uno de dos modos son la base de la regla predeterminada: Lista de permitidos o lista de denegación. Puede crear una directiva que identifica todas las aplicaciones que se pueden ejecutar en su entorno; la regla predeterminada dentro de la directiva está restringido y se bloquean las aplicaciones que no se permite explícitamente para ejecutar. O bien puede crear una directiva que identifica todas las aplicaciones que no se pueden ejecutar; la regla predeterminada es Unrestricted y restringe a solo las aplicaciones que ha enumerado explícitamente.

> [!IMPORTANT]
> El modo de lista de denegación podría ser una estrategia de mantenimiento para su organización con respecto a control de la aplicación. Crear y mantener una lista en constante evolución que prohíbe todo el malware y otras aplicaciones problemáticas sería mucho tiempo y susceptible a errores.

### <a name="create-an-inventory-of-your-applications-for-the-allow-list"></a>Crear un inventario de aplicaciones en la lista de permitidos
Para usar eficazmente la regla de permiso predeterminado, deberá determinar exactamente qué aplicaciones son necesarias en su organización. Hay herramientas diseñadas para generar un inventario de aplicaciones, como el recopilador de inventario en Microsoft Application Compatibility Toolkit. Pero SRP tiene una característica de registro avanzada que le ayudarán a comprender exactamente qué aplicaciones se ejecutan en su entorno.

##### <a name="to-discover-which-applications-to-allow"></a>Para detectar qué aplicaciones para permitir

1.  En un entorno de pruebas, implementar la directiva de restricción de Software con el conjunto de reglas predeterminado para Unrestricted y quite todas las reglas adicionales. Si habilita SRP sin obliga a que las aplicaciones, SPR será capaz de supervisar las aplicaciones que se ejecute.

2.  Cree el siguiente valor del registro con el fin de habilitar la característica de registro avanzado y establecer la ruta de acceso donde se debe escribir el archivo de registro.

    **"HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Safer\ CodeIdentifiers"**

    Valor de cadena: *Ruta de acceso NameLogFile NameLogFile*

    Porque SRP es evaluar todas las aplicaciones cuando se ejecutan, se escribe una entrada en el archivo de registro *NameLogFile* cada vez que se ejecuta la aplicación.

3.  Evaluar el archivo de registro

    Estados de cada entrada de registro:

    -   el llamador de la directiva de restricción de software y el identificador de proceso (PID) del proceso que realiza la llamada

    -   el destino que se va a evaluar

    -   la regla SRP que se produjo al ejecutar la aplicación

    -   identificador de la regla SRP.

    Un ejemplo de la salida escrita en un archivo de registro:

**Explorer.exe (PID = 4728) identifiedC:\Windows\system32\onenote.exe regla usingpath sin restricciones, Guid = {320bd852-aa7c-4674-82c5-9a80321670a3}** se anotará en el registro de todas las aplicaciones y el código asociado que SRP comprueba y configurado para bloquear archivo, que, a continuación, puede usar para determinar qué archivos ejecutables que deben considerarse para la lista de permitidas.


