---
title: Control de acceso basado en rol
description: Este tema forma parte de la guía de administración de la administración de direcciones IP (IPAM) en Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: ecdfc589-fa14-4bb3-ab7e-456ebc719385
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 41e9e92bf2a6a1536d355590010589d2da4e5c0f
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97948001"
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



