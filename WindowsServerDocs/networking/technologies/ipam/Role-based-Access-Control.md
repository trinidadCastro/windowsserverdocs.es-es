---
title: Control de acceso basado en roles
description: Este tema forma parte de la Guía de administración de administración de direcciones IP (IPAM) en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ecdfc589-fa14-4bb3-ab7e-456ebc719385
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bb4c1a6a5f2ea708863be3cbf0abe436e12608de
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282140"
---
# <a name="role-based-access-control"></a>Control de acceso basado en roles

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se proporciona información sobre cómo usar el control de acceso basado en roles en IPAM.  
  
> [!NOTE]  
> Además de este tema, la siguiente documentación de control de acceso IPAM está disponible en esta sección.  
>   
> -   [Administración basada en roles con el administrador del servidor de Control de acceso](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Server-Manager.md)  
> -   [Administración basada en roles Control de acceso con Windows PowerShell](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md)  
  
Control de acceso basado en roles permite especificar los privilegios de acceso en varios niveles, incluido el servidor DNS, la zona DNS y niveles de registro de recursos DNS.  
Mediante el uso de control de acceso basado en roles, puede especificar quién tiene control granular sobre las operaciones para crear, editar y eliminar diferentes tipos de registros de recursos DNS.  
  
Puede configurar el control de acceso para que los usuarios están restringidos a los siguientes permisos.  
  
-   Los usuarios pueden editar solo registros de recursos DNS específicos  
  
-   Los usuarios pueden editar los registros de recursos DNS de un tipo específico, como PTR o MX  
  
-   Los usuarios pueden editar los registros de recursos DNS para zonas específicas  
  
## <a name="see-also"></a>Vea también  
[Administración basada en roles con el administrador del servidor de Control de acceso](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Server-Manager.md)  
[Administración basada en roles Control de acceso con Windows PowerShell](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md)  
[Administrar IPAM](Manage-IPAM.md)  
  


