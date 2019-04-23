---
title: Administrar una colección de sesiones de escritorio personal de RDS
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
author: lizap
manager: dongill
ms.openlocfilehash: 286c7ba4bd4428669d135c35c825033d22b8f40e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865726"
---
## <a name="manage-your-personal-desktop-session-collections"></a>Administrar las colecciones de sesiones de escritorio personal

Utilice la siguiente información para administrar una colección de sesiones de escritorio personal en servicios de escritorio remoto.

### <a name="manually-assign-a-user-to-a-personal-session-host"></a>Asignar manualmente un usuario a un host de sesión personal
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
 
### <a name="removing-a-user-assignment-from-a-personal-session-host"></a>Quitar una asignación de usuario de un Host de sesión Personal
Use la **Remove-RDPersonalSessionDesktopAssignment** cmdlet para quitar la asociación entre un equipo de escritorio de la sesión personal y el usuario. El cmdlet admite los siguientes parámetros:

-CollectionName \<string\>

-ConnectionBroker \<cadena\>

-Force

-Nombre \<cadena\>

-Usuario \<cadena\>

**– Forzar** obliga al comando se ejecute sin pedir confirmación del usuario.

### <a name="query-user-assignments"></a>Asignaciones de usuario de consulta
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
