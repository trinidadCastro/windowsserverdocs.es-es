---
title: Control de acceso basado en rol
description: Aprenda a usar el control de acceso basado en roles en IPAM.
manager: brianlic
ms.topic: article
ms.assetid: ecdfc589-fa14-4bb3-ab7e-456ebc719385
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: e2a22019ee68d98f8122c79ebe8146c898164f7f
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2021
ms.locfileid: "98039365"
---
# <a name="role-based-access-control"></a>Control de acceso basado en rol

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se proporciona información sobre el uso del control de acceso basado en roles en IPAM.

> [!NOTE]
> Además de este tema, está disponible la siguiente documentación sobre el control de acceso de IPAM en esta sección.
>
> -   [Administración del control de acceso basado en roles con el Administrador del servidor](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Server-Manager.md)
> -   [Administración del control de acceso basado en roles con Windows PowerShell](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md)

El control de acceso basado en roles permite especificar privilegios de acceso en distintos niveles, incluidos el servidor DNS, la zona DNS y los niveles de registro de recursos DNS.
Mediante el control de acceso basado en roles, puede especificar quién tiene un control granular sobre las operaciones para crear, editar y eliminar diferentes tipos de registros de recursos DNS.

Puede configurar el control de acceso para que los usuarios estén restringidos a los siguientes permisos.

-   Los usuarios solo pueden modificar determinados registros de recursos DNS.

-   Los usuarios pueden editar los registros de recursos DNS de un tipo específico, como PTR o MX

-   Los usuarios pueden editar los registros de recursos DNS para zonas específicas

## <a name="see-also"></a>Consulte también
[Administrar Access Control basado en roles con administrador del servidor](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Server-Manager.md) 
 [Administrar Access Control basado en roles con Windows PowerShell](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md) 
 [Administrar IPAM](Manage-IPAM.md)



