---
title: Crear una colección de Servicios de Escritorio remoto
description: Obtén información sobre cómo agregar programas RDSH y RemoteApp a tu implementación de RDS.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 10/22/2019
ms.topic: article
ms.assetid: ae9767e3-864a-4eb2-96c0-626759ce6d60
author: lizap
manager: dongill
ms.openlocfilehash: 6a842c7984dc63fe40c05300f6cfbb6718846525
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852958"
---
# <a name="create-a-remote-desktop-services-collection-for-desktops-and-apps-to-run"></a>Creación de una colección de servicios de Escritorio remoto para que se ejecuten escritorios y aplicaciones

>Se aplica a: Windows Server (Canal semianual), Windows Server 2019 y Windows Server 2016

Sigue los pasos a continuación para crear una colección de sesiones de Servicios de Escritorio remoto. Una colección de sesiones contiene las aplicaciones y escritorios que quieres que estén disponibles para los usuarios. Después de crear la colección, publícala para que los usuarios puedan acceder a ella.

Antes de crear una colección, debes decidir qué tipo de colección necesitas: sesiones de escritorio agrupadas o sesiones de escritorio personales. 

- **Usa sesiones de escritorio agrupadas para virtualización basada en sesiones**: Aprovecha la eficacia de proceso de Windows Server para proporcionar un entorno multisesión rentable que impulse las cargas de trabajo diarias de los usuarios.
- **Usa sesiones de escritorio personales para crear una infraestructura de escritorio virtual (VDI)** : Aprovecha el cliente de Windows para proporcionar el alto rendimiento, la compatibilidad de aplicaciones y la familiaridad que los usuarios esperan de su experiencia de escritorio de Windows.
 
Con una sesión agrupada, varios usuarios acceden a un grupo compartido de recursos, mientras que con una sesión de escritorio personal, se asigna a cada usuario su propio escritorio dentro del grupo. La sesión agrupada proporciona un menor costo general, mientras que las sesiones personales permiten a los usuarios personalizar su experiencia de escritorio.

Si necesitas compartir aplicaciones hospedadas con gran cantidad de gráficos, puedes combinar escritorios de sesión personal con la nueva funcionalidad Discrete Device Assignment (DDA, asignación discreta de dispositivos) para proporcionar también compatibilidad con aplicaciones hospedadas que requieran gráficos acelerados. Para más información, consulta [¿La tecnología de virtualización de gráficos es adecuada para usted?](rds-graphics-virtualization.md)


Independientemente del tipo de colección que elijas, deberás rellenar esas colecciones con RemoteApps: programas y recursos a que los usuarios pueden acceder desde cualquier dispositivo compatible y trabajar como si el programa se ejecutase localmente.

## <a name="create-a-pooled-desktop-session-collection"></a>Crear una colección de sesiones de escritorio agrupadas

1.  En el Administrador de servidores, haz clic en **Servicios de Escritorio remoto > Colecciones > Tareas > Crear colecciones de sesiones**.  
2.  Escribe un nombre de colección, por ejemplo, **ContosoAps**.  
3.  Selecciona el servidor host de sesión de Escritorio remoto que has creado (por ejemplo, Contoso-Shr1).  
4.  Acepta los **Grupos de usuarios** predeterminados.  
5.  Escribe la ubicación del recurso compartido de archivos que creaste para los discos de perfil de usuario para esta colección (por ejemplo, **\Contoso-Cb1\UserDisksr**).   
6.  Haga clic en **Crear**. Cuando se haya creado la colección, haz clic en **Cerrar**.  


## <a name="create-a-personal-desktop-session-collection"></a>Crear una colección de sesiones de escritorio personales

Usa el cmdlet New-RDSessionCollection para crear una colección de escritorios de sesión personal. Los siguientes tres parámetros proporcionan la información de configuración necesaria para los escritorios de sesión personal:

- **-PersonalUnmanaged**: Especifica el tipo de colección de sesiones que te permite asignar usuarios a un servidor host de sesión personal. Si no especificas este parámetro, la colección se crea como una colección de host de sesión de Escritorio remoto tradicional, donde los usuarios se asignan al siguiente host de sesión disponible cuando inician sesión.
- **-GrantAdministrativePrivilege**: Si usas **-PersonalUnmanaged**, especifica que al usuario asignado al host de sesión se le concedan privilegios administrativos. Si no usas este parámetro, los usuarios recibirán privilegios de usuario estándar únicamente.
- **-AutoAssignUser**: Si usas **-PersonalUnmanaged**, especifica que los nuevos usuarios que se conecten a través del Agente de conexión a Escritorio remoto se asignen automáticamente a un host de sesión sin asignar. Si no hay ningún host de sesión sin asignar en la colección, el usuario verá un mensaje de error. Si no usas este parámetro, tendrás que [asignar manualmente los usuarios a un host de sesión](rds-manage-personal-collection.md#manually-assign-a-user-to-a-personal-session-host) antes de que inicien sesión.

Puedes usar los cmdlets de PowerShell para administrar las colecciones de sesiones de escritorio personales. Para más información, consulta [Administrar tus colecciones de sesiones de escritorios personales](rds-manage-personal-collection.md).

## <a name="publish-remoteapp-programs"></a>Publicar programas RemoteApp
Sigue estos pasos para publicar las aplicaciones y los recursos en la colección:

1.  En el Administrador de servidores, selecciona la nueva colección (**ContosoApps**).  
2.  En Programas de RemoteApp, haz clic en **Publicar programas RemoteApp**.  
3. Selecciona los programas que quieres publicar y, luego, haz clic en **Publicar**.  
