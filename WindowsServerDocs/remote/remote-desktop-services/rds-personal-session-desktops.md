---
title: Uso de escritorios personales sesión con servicios de escritorio remoto
description: Aprenda a compartir personalizadas, asigna escritorios a través de RDS.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 09/16/2016
manager: dongill
ms.openlocfilehash: 41f6a28c1b754a5277a0966a87dae08a6aa34e08
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875956"
---
# <a name="use-personal-session-desktops-with-remote-desktop-services"></a>Uso de escritorios personales sesión con servicios de escritorio remoto

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede implementar escritorios personales basada en servidor en un entorno de informática en nube mediante el uso de escritorios personales de sesión.  (Un entorno de informática en nube tiene una separación entre los servidores de tejido de Hyper-V y las máquinas virtuales invitadas, por ejemplo, en la nube de Microsoft Azure o la plataforma de nube de Microsoft). La funcionalidad de escritorio de la sesión personal amplía el escenario de implementación de escritorio basados en sesión en servicios de escritorio remoto para crear un nuevo tipo de colección de sesiones que cada usuario está asignado a su propio host de sesión personales con derechos administrativos. 

Utilice la siguiente información para crear y administrar una colección de escritorios personales de sesión.

## <a name="create-a-personal-session-desktop-collection"></a>Crear una colección de escritorios personales sesión

Use el cmdlet New-RDSessionCollection para crear una colección de escritorios personales de sesión. Los siguientes tres parámetros proporcionan la información de configuración necesaria para los escritorios personales sesión:

- **-PersonalUnmanaged** : especifica el tipo de colección de sesiones que le permite asignar usuarios a un servidor de host de sesión personales. Si no especifica este parámetro, se crea la colección como una colección de Host de sesión de escritorio remoto tradicional, donde los usuarios están asignados a la siguiente host de sesión disponibles cuando inician sesión.
- **-GrantAdministrativePrivilege** : si usa **- PersonalUnmanaged**, especifica que el usuario asignado al host de sesión concederá privilegios administrativos. Si no usa este parámetro, los usuarios se conceden privilegios de usuario estándar sólo.
- **-AutoAssignUser** : si usa **- PersonalUnmanaged**, especifica que los nuevos usuarios que se conectan a través del agente de conexión a Escritorio remoto se asignan automáticamente a un host de sesión sin asignar. Si no hay ningún host de sesión sin asignar en la colección, el usuario verá un mensaje de error. Si no usa este parámetro, deberá [manualmente asignar usuarios a un host de sesión](#manually-assign-a-user-to-a-personal-session-host) antes de iniciar sesión.

## <a name="manually-assign-a-user-to-a-personal-session-host"></a>Asignar manualmente un usuario a un host de sesión personal
Use la **conjunto RDPersonalSessionDesktopAssignment** cmdlet para asignar manualmente un usuario a un servidor de host de sesión personales en la colección. El cmdlet admite los siguientes parámetros:

-CollectionName \<string\>

-ConnectionBroker \<cadena\> 

-Usuario \<cadena\>

-Nombre \<cadena\>

- **– CollectionName** : especifica el nombre de la colección de escritorios personales de sesión. Este parámetro es obligatorio.
- **– ConnectionBroker** -especifica el servidor de agente de conexión de escritorio remoto (RD Connection Broker) para la implementación de escritorio remoto. Si no proporciona un valor, el cmdlet usa el nombre de dominio completo (FQDN) del equipo local.
- **: Usuario** -especifica la cuenta de usuario para asociar con el escritorio de la sesión personal, en formato dominio\usuario. Este parámetro es obligatorio.
- **: Nombre** : especifica el nombre del servidor host de sesión. Este parámetro es obligatorio. El host de sesión identificado aquí debe ser un miembro de la colección que la **- CollectionName** especifica el parámetro.

El **importación RDPersonalSessionDesktopAssignment** cmdlet importa las asociaciones entre las cuentas de usuario y los escritorios personales sesión desde un archivo de texto. El cmdlet admite los siguientes parámetros:

-CollectionName \<string\>

-ConnectionBroker \<cadena\>

-Ruta de acceso \<cadena >

**: Ruta de acceso** especifica la ruta de acceso y el nombre de un archivo para importar.
 
## <a name="removing-a-user-assignment-from-a-personal-session-host"></a>Quitar una asignación de usuario de un Host de sesión Personal
Use la **Remove-RDPersonalSessionDesktopAssignment** cmdlet para quitar la asociación entre un equipo de escritorio de la sesión personal y el usuario. El cmdlet admite los siguientes parámetros:

-CollectionName \<string\>

-ConnectionBroker \<cadena\>

-Force

-Nombre \<cadena\>

-Usuario \<cadena\>

**– Forzar** obliga al comando se ejecute sin pedir confirmación del usuario.

## <a name="query-user-assignments"></a>Asignaciones de usuario de consulta
Use la **Get RDPersonalSessionDesktopAssignment** para obtener una lista de los escritorios personales de sesión y cuentas de usuario asociadas. El cmdlet admite los siguientes parámetros:

-CollectionName \<string\>

-ConnectionBroker \<cadena\>

-Usuario \<cadena\>

-Nombre \<cadena\>

Puede ejecutar el cmdlet para consultar por nombre de la colección, nombre de usuario, o por nombre de sesión del escritorio. Si especifica solo el **: CollectionName** , el cmdlet devuelve una lista de hosts de sesión y usuarios asociados. Si se especifica también la **– usuario** parámetro, se devuelve el host de sesión asociado a ese usuario. Si proporciona el **– nombre** parámetro, se devuelve el usuario asociado con ese host de sesión. 


El **exportación RDPersonalPersonalDesktopAssignment** cmdlet exporta las asociaciones entre usuarios y equipos de escritorio virtuales personales actuales a un archivo de texto. El cmdlet admite los siguientes parámetros:

-CollectionName \<string\>

-ConnectionBroker \<cadena\>

-Ruta de acceso \<cadena\>


Todos los nuevos cmdlets admiten los parámetros comunes:-Verbose,-Debug, - ErrorAction, - ErrorVariable,-OutBuffer y - OutVariable. Para obtener más información, consulte [about_CommonParameters](https://go.microsoft.com/fwlink/p/?LinkID=113216).

## <a name="hardware-accelerated-graphics"></a>Gráficos acelerados por hardware
Windows Server 2016 extiende la tecnología (vGPU) del adaptador de gráficos 3D de RemoteFX para admitir OpenGL y es compatible con máquinas virtuales invitadas de Windows Server 2016 de usuario único. Puede combinar los escritorios personales sesión con las nuevas capacidades de vGPU para proporcionar compatibilidad con las aplicaciones hospedadas que requieren gráficos acelerados. Como alternativa, puede combinar los escritorios personales sesión con la nueva funcionalidad de asignación de dispositivo discretos (DDA) también proporcionan compatibilidad para las aplicaciones hospedadas que requieren gráficos acelerados.
