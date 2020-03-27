---
title: Control de acceso basado en roles
description: Este tema forma parte de la guía de administración de la administración de direcciones IP (IPAM) en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ecdfc589-fa14-4bb3-ab7e-456ebc719385
ms.author: lizross
author: eross-msft
ms.openlocfilehash: dc4dc26a5079a34cdaa3d455e59f6bfb4d1f74c1
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309519"
---
# <a name="role-based-access-control"></a>Control de acceso basado en roles

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se proporciona información sobre el uso del control de acceso basado en roles en IPAM.  
  
> [!NOTE]  
> Además de este tema, está disponible la siguiente documentación sobre el control de acceso de IPAM en esta sección.  
>   
> -   [Administrar Access Control basado en roles con Administrador del servidor](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Server-Manager.md)  
> -   [Administrar Access Control basado en roles con Windows PowerShell](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md)  
  
El control de acceso basado en roles permite especificar privilegios de acceso en distintos niveles, incluidos el servidor DNS, la zona DNS y los niveles de registro de recursos DNS.  
Mediante el control de acceso basado en roles, puede especificar quién tiene un control granular sobre las operaciones para crear, editar y eliminar diferentes tipos de registros de recursos DNS.  
  
Puede configurar el control de acceso para que los usuarios estén restringidos a los siguientes permisos.  
  
-   Los usuarios solo pueden modificar determinados registros de recursos DNS.  
  
-   Los usuarios pueden editar los registros de recursos DNS de un tipo específico, como PTR o MX  
  
-   Los usuarios pueden editar los registros de recursos DNS para zonas específicas  
  
## <a name="see-also"></a>Consulta también  
[Administrar Access Control basado en roles con Administrador del servidor](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Server-Manager.md)  
[Administrar Access Control basado en roles con Windows PowerShell](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md)  
[Administrar IPAM](Manage-IPAM.md)  
  


