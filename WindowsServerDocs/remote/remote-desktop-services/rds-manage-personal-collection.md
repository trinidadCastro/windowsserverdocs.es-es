---
title: Administrar una colección de sesiones de escritorios personales en RDS
description: Obtén información sobre cómo agregar programas RDSH y RemoteApp a tu implementación de RDS.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 11/08/2016
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 7be6b2bfe1105357811927f5da7092e8c16c3446
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2020
ms.locfileid: "75949846"
---
# <a name="manage-your-personal-desktop-session-collections"></a>Administrar tus colecciones de sesiones de escritorios personales

Usa la siguiente información para administrar una colección de sesiones de escritorios personales en los Servicios de Escritorio remoto.

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


Todos los nuevos cmdlets admiten los parámetros comunes: -Verbose, -Debug, -ErrorAction, -ErrorVariable, -OutBuffer y -OutVariable. Para obtener más información, consulta [about_CommonParameters](https://go.microsoft.com/fwlink/p/?LinkID=113216).
