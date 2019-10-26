---
title: Uso de un entorno de escritorios de sesión personal con Servicios de Escritorio remoto
description: Aprende a compartir escritorios asignados personalizados a través de RDS.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 10/22/2019
manager: dongill
ms.openlocfilehash: 9386733911ca81ad60d038854bd68e5603aae4cf
ms.sourcegitcommit: 3262c5c7cece9f2adf2b56f06b7ead38754a451c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812286"
---
# <a name="use-personal-session-desktops-with-remote-desktop-services"></a>Uso de un entorno de escritorios de sesión personal con Servicios de Escritorio remoto

>Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016

Puedes implementar escritorios personales basados en servidor en un entorno de informática en la nube mediante escritorios de sesión personal.  (Un entorno de informática en la nube tiene una separación entre los servidores de tejido de Hyper-V y las máquinas virtuales de invitado, por ejemplo, Microsoft Azure Cloud o la plataforma de Microsoft Cloud). La funcionalidad de escritorios de sesión personal amplía el escenario de implementación de escritorios basados en sesión en los Servicios de Escritorio remoto, para crear un nuevo tipo de colección de sesiones donde a cada usuario se asigna su propio host de sesión personal con derechos administrativos. 

Usa la siguiente información para crear y administrar una colección de escritorios de sesión personal.

## <a name="create-a-personal-session-desktop-collection"></a>Crear una colección de escritorios de sesión personal

Usa el cmdlet New-RDSessionCollection para crear una colección de escritorios de sesión personal. Los siguientes tres parámetros proporcionan la información de configuración necesaria para los escritorios de sesión personal:

- **-PersonalUnmanaged**: Especifica el tipo de colección de sesiones que te permite asignar usuarios a un servidor host de sesión personal. Si no especificas este parámetro, la colección se crea como una colección de host de sesión de Escritorio remoto tradicional, donde los usuarios se asignan al siguiente host de sesión disponible cuando inician sesión.
- **-GrantAdministrativePrivilege**: Si usas **-PersonalUnmanaged**, especifica que al usuario asignado al host de sesión se le concedan privilegios administrativos. Si no usas este parámetro, los usuarios recibirán privilegios de usuario estándar únicamente.
- **-AutoAssignUser**: Si usas **-PersonalUnmanaged**, especifica que los nuevos usuarios que se conecten a través del Agente de conexión a Escritorio remoto se asignen automáticamente a un host de sesión sin asignar. Si no hay ningún host de sesión sin asignar en la colección, el usuario verá un mensaje de error. Si no usas este parámetro, tendrás que [asignar manualmente los usuarios a un host de sesión](#manually-assign-a-user-to-a-personal-session-host) antes de que inicien sesión.

## <a name="manually-assign-a-user-to-a-personal-session-host"></a>Asignar manualmente un usuario a un host de sesión personal
Usa el cmdlet **Set-RDPersonalSessionDesktopAssignment** para asignar manualmente un usuario a un servidor host de sesión personal en la colección. El cmdlet admite los parámetros siguientes:

-CollectionName \<cadena\>

-ConnectionBroker \<cadena\> 

-User \<cadena\>

-Name \<cadena\>

- **–CollectionName**: Especifica el nombre de la colección de escritorios de sesión personal. Este parámetro es obligatorio.
- **–ConnectionBroker**: Especifica el servidor del Agente de conexión a Escritorio remoto para la implementación de Escritorio remoto. Si no proporcionas un valor, el cmdlet usa el nombre de dominio completo (FQDN) del equipo local.
- **–User**: Especifica la cuenta de usuario que se va a asociar con el escritorio de sesión personal, en formato DOMINIO\Usuario. Este parámetro es obligatorio.
- **–Name**: Especifica el nombre del servidor host de sesión. Este parámetro es obligatorio. El host de sesión identificado aquí debe ser miembro de la colección que especifica el parámetro **-CollectionName**.

El cmdlet **Import-RDPersonalSessionDesktopAssignment** importa las asociaciones entre las cuentas de usuario y los escritorios de sesión personal desde un archivo de texto. El cmdlet admite los parámetros siguientes:

-CollectionName \<cadena\>

-ConnectionBroker \<cadena\>

-Path \<cadena>

**–Path**: Especifica la ruta de acceso y el nombre del archivo que se va a importar.
 
## <a name="removing-a-user-assignment-from-a-personal-session-host"></a>Quitar una asignación de usuario de un host de sesión personal
Usa el cmdlet **Remove-RDPersonalSessionDesktopAssignment** para quitar la asociación entre un escritorio de sesión personal y un usuario. El cmdlet admite los parámetros siguientes:

-CollectionName \<cadena\>

-ConnectionBroker \<cadena\>

-Force

-Name \<cadena\>

-User \<cadena\>

**–Force**: Obliga al comando a ejecutarse sin solicitar la confirmación del usuario.

## <a name="query-user-assignments"></a>Consultar las asignaciones de usuario
Usa el cmdlet **Get-RDPersonalSessionDesktopAssignment** para obtener una lista de los escritorios de sesión personal y las cuentas de usuario asociadas. El cmdlet admite los parámetros siguientes:

-CollectionName \<cadena\>

-ConnectionBroker \<cadena\>

-User \<cadena\>

-Name \<cadena\>

Puedes ejecutar el cmdlet para consultar por nombre de colección, nombre de usuario, o nombre de escritorio de sesión. Si especificas solo el parámetro **–CollectionName**, el cmdlet devuelve una lista de hosts de sesión y usuarios asociados. Si especificas también el parámetro **–User**, se devuelve el host de sesión asociado a ese usuario. Si proporcionas el parámetro **–Name**, se devuelve el usuario asociado con ese host de sesión. 


El cmdlet **Export-RDPersonalPersonalDesktopAssignment** exporta las asociaciones actuales entre los usuarios y los escritorios virtuales personales a un archivo de texto. El cmdlet admite los parámetros siguientes:

-CollectionName \<cadena\>

-ConnectionBroker \<cadena\>

-Path \<cadena\>


Todos los nuevos cmdlets admiten los parámetros comunes: -Verbose, -Debug, -ErrorAction, -ErrorVariable, -OutBuffer y -OutVariable. Para obtener más información, consulte [about_CommonParameters](https://go.microsoft.com/fwlink/p/?LinkID=113216).
