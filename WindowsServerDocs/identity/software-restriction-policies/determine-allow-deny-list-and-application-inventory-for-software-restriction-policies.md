---
title: Determinación de la lista de permisos y denegaciones y del inventario de aplicaciones para directivas de restricción de software
description: Seguridad de Windows Server
ms.prod: windows-server
ms.technology: security-software-restriction-policies
ms.topic: article
ms.assetid: 0abb73b6-b5d8-4505-8ab1-2f29e4bf0411
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: c0fdb5c1d7c4b03610a173c6cd0575d39646a7d0
ms.sourcegitcommit: af1cf89632d62a94943d3ad9f6b5234b88499278
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81524910"
---
# <a name="determine-allow-deny-list-and-application-inventory-for-software-restriction-policies"></a>Determinación de la lista de permisos y denegaciones y del inventario de aplicaciones para directivas de restricción de software

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema para profesionales de TI se ofrece orientación sobre cómo crear una lista de permitidos y denegados para que las aplicaciones las administren las directivas de restricción de software (SRP) a partir de Windows Server 2008 y Windows Vista.

## <a name="introduction"></a>Introducción
Las directivas de restricción de software (SRP) son una característica basada en directivas de grupo que identifica los programas de software que se ejecutan en los equipos de un dominio y controla la capacidad de ejecución de dichos programas. Puede usar las directivas de restricción de software para crear una configuración muy restringida para los equipos, en los que solamente pueden ejecutarse aquellas aplicaciones específicamente identificadas. Se integran con Microsoft Active Directory Domain Services y directiva de grupo, pero también se pueden configurar en equipos independientes. Para obtener un punto de partida de SRP, consulte las [directivas de restricción de software](software-restriction-policies.md).

A partir de Windows Server 2008 R2 y Windows 7, se puede usar Windows AppLocker en lugar de o junto con SRP para una parte de la estrategia de control de la aplicación.

Para obtener información acerca de cómo realizar tareas específicas con SRP, consulte lo siguiente:

-   [Trabajo con reglas de directivas de restricción de software](work-with-software-restriction-policies-rules.md)

-   [Usar directivas de restricción de software para ayudar a proteger el equipo contra un virus de correo electrónico](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

### <a name="what-default-rule-to-choose-allow-or-deny"></a>Qué regla predeterminada elegir: permitir o denegar
Las directivas de restricción de software se pueden implementar en uno de los dos modos que son la base de la regla predeterminada: lista de permitidos o lista de denegación. Puede crear una directiva que identifique todas las aplicaciones que pueden ejecutarse en su entorno; la regla predeterminada dentro de la Directiva está restringida y bloqueará todas las aplicaciones que no permita que se ejecuten explícitamente. O bien, puede crear una directiva que identifique todas las aplicaciones que no se pueden ejecutar; la regla predeterminada es Unrestricted y solo restringe las aplicaciones que ha enumerado explícitamente.

> [!IMPORTANT]
> El modo de lista de denegación puede ser una estrategia de mantenimiento elevado para su organización con respecto al control de la aplicación. Crear y mantener una lista en evolución que prohibe todo el malware y otras aplicaciones problemáticas llevaría mucho tiempo y es susceptible a errores.

### <a name="create-an-inventory-of-your-applications-for-the-allow-list"></a>Crear un inventario de las aplicaciones para la lista de permitidos
Para usar de forma eficaz la regla predeterminada permitir, debe determinar exactamente qué aplicaciones son necesarias en su organización. Hay herramientas diseñadas para generar un inventario de aplicaciones, como el recopilador de inventario en el kit de herramientas de compatibilidad de aplicaciones de Microsoft. Pero SRP tiene una característica de registro avanzado para ayudarle a entender exactamente qué aplicaciones se están ejecutando en su entorno.

##### <a name="to-discover-which-applications-to-allow"></a>Para detectar las aplicaciones que se deben permitir

1.  En un entorno de prueba, implemente la Directiva de restricción de software con la regla predeterminada establecida en irrestricto y quite las reglas adicionales. Si habilita SRP sin forzarlo a restringir las aplicaciones, SPR podrá supervisar qué aplicaciones se están ejecutando.

2.  Cree el siguiente valor del registro para habilitar la característica de registro avanzado y establezca la ruta de acceso en la que se debe escribir el archivo de registro.

    **"HKEY_LOCAL_MACHINE \SOFTWARE\Policies\Microsoft\Windows\Safer\CodeIdentifiers"**

    Valor de cadena: *nombreDeArchivoDeRegistro ruta de acceso a nombreDeArchivoDeRegistro*

    Dado que SRP está evaluando todas las aplicaciones cuando se ejecutan, se escribe una entrada en el archivo de registro *NameLogFile* cada vez que se ejecuta la aplicación.

3.  Evaluación del archivo de registro

    Cada entrada de registro indica:

    -   el autor de la llamada de la Directiva de restricción de software y el ID. de proceso (PID) del proceso que realiza la llamada.

    -   destino que se está evaluando.

    -   la regla de SRP que se encontró cuando se ejecutó la aplicación

    -   identificador de la regla de SRP.

    Un ejemplo de la salida escrita en un archivo de registro:

**Explorer. exe (PID = 4728) identifiedC: \ Windows\system32\onenote.exe as Unrestricted usingpath Rule, GUID = {320bd852-aa7c-4674-82c5-9a80321670a3}**    Todas las aplicaciones y el código asociado que comprueba y establece en Block se anotarán en el archivo de registro, que puede usar para determinar qué ejecutables se deben tener en cuenta para la lista de permitidos.

