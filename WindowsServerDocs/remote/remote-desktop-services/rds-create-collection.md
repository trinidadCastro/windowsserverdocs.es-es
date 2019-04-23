---
title: Crear una colección de servicios de escritorio remoto
description: Obtenga información sobre cómo agregar y programas RDSH y RemoteApp para la implementación de RDS.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 11/08/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ae9767e3-864a-4eb2-96c0-626759ce6d60
author: lizap
manager: dongill
ms.openlocfilehash: 52806e28c4ef87453995728623efe2954a76dfd9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839246"
---
# <a name="create-a-remote-desktop-services-collection-for-desktops-and-apps-to-run"></a>Crear una colección de servicios de escritorio remoto para escritorios y aplicaciones se ejecuten

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Utilice los pasos siguientes para crear una colección de sesiones de servicios de escritorio remoto. Una colección de sesiones contiene las aplicaciones y escritorios que desea que estén disponibles para los usuarios. Después de crear la colección, publicarla para que los usuarios pueden acceder a él.

Antes de crear una colección, debe decidir qué tipo de colección necesita: agrupadas sesiones de escritorio o sesiones de escritorio personales. 

- **Usar sesiones de escritorio agrupadas para la virtualización basada en sesión**: Aproveche la potencia de proceso de Windows Server para proporcionar un entorno multisesión rentable para cargas de trabajo de usuarios diario de unidad
- **Usar sesiones de escritorio personales para crear una infraestructura de escritorio virtual (VDI)**: Aproveche el cliente de Windows para proporcionar el alto rendimiento, la compatibilidad de aplicaciones y la familiaridad que los usuarios han llegado a esperar de su experiencia de escritorio de Windows.
 
Con una sesión agrupada, varios usuarios tener acceso a un grupo compartido de recursos, mientras que con una sesión de escritorio personal, los usuarios se asignan su propio escritorio desde dentro del grupo. La sesión agrupada proporciona menor costo general, mientras que las sesiones de personales permiten a los usuarios personalizar su experiencia de escritorio.

Si necesita compartir hospedan aplicaciones que son de gráficos, puede combinar escritorios personales sesión con la vGPU de RemoteFX configurado para la aceleración de gráficos. Como alternativa, puede combinar los escritorios personales sesión con la nueva funcionalidad de asignación de dispositivo discretos (DDA) también proporcionan compatibilidad para las aplicaciones hospedadas que requieren gráficos acelerados. Desproteger [qué tecnología de virtualización de gráficos es adecuada para usted](rds-graphics-virtualization.md) para obtener más información.


Independientemente del tipo de colección que elija, deberá rellenar esas colecciones con RemoteApps - programas y recursos que los usuarios pueden acceder desde cualquier dispositivo compatible y trabaja como si el programa se ejecuta localmente.

## <a name="create-a-pooled-desktop-session-collection"></a>Crear una colección de sesiones de escritorio agrupados

1.  En el administrador del servidor, haga clic en **Remote Desktop Services > colecciones > tareas > crear colecciones de sesiones**.  
2.  Escriba un nombre para la colección, por ejemplo **ContosoAps**.  
3.  Seleccione el servidor Host de sesión de escritorio remoto que ha creado (por ejemplo, Contoso Shr1).  
4.  Acepte el valor predeterminado **grupos de usuarios**.  
5.  Escriba la ubicación del recurso compartido de archivos que creó para los discos de perfil de usuario para esta colección (por ejemplo, **\Contoso-Cb1\UserDisksr**).   
6.  Haga clic en **Crear**. Cuando se crea la colección, haga clic en **cerrar**.  


## <a name="create-a-personal-desktop-session-collection"></a>Crear una colección de sesiones de escritorio personal

Use el cmdlet New-RDSessionCollection para crear una colección de escritorios personales de sesión. Los siguientes tres parámetros proporcionan la información de configuración necesaria para los escritorios personales sesión:

- **-PersonalUnmanaged** : especifica el tipo de colección de sesiones que le permite asignar usuarios a un servidor de host de sesión personales. Si no especifica este parámetro, se crea la colección como una colección de Host de sesión de escritorio remoto tradicional, donde los usuarios están asignados a la siguiente host de sesión disponibles cuando inician sesión.
- **-GrantAdministrativePrivilege** : si usa **- PersonalUnmanaged**, especifica que el usuario asignado al host de sesión concederá privilegios administrativos. Si no usa este parámetro, los usuarios se conceden privilegios de usuario estándar sólo.
- **-AutoAssignUser** : si usa **- PersonalUnmanaged**, especifica que los nuevos usuarios que se conectan a través del agente de conexión a Escritorio remoto se asignan automáticamente a un host de sesión sin asignar. Si no hay ningún host de sesión sin asignar en la colección, el usuario verá un mensaje de error. Si no usa este parámetro, deberá [manualmente asignar usuarios a un host de sesión](rds-manage-personal-collection.md#manually-assign-a-user-to-a-personal-session-host) antes de iniciar sesión.

Puede usar los cmdlets de PowerShell para administrar las colecciones de sesiones de escritorio personal. Consulte [administrar las colecciones de sesiones de escritorio personal](rds-manage-personal-collection.md) para obtener más información.

## <a name="publish-remoteapp-programs"></a>Publicar programas RemoteApp
Siga estos pasos para publicar las aplicaciones y los recursos de la colección:

1.  En el administrador del servidor, seleccione la nueva colección (**ContosoApps**).  
2.  En programas de RemoteApp, haga clic en **programas RemoteApp publicar**.  
3. Seleccione los programas que desea publicar y, a continuación, haga clic en **publicar**.  
