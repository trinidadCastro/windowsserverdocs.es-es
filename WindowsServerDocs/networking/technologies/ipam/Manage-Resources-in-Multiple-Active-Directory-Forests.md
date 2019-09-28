---
title: Administración de recursos en varios bosques de Active Directory
description: Este tema forma parte de la guía de administración de la administración de direcciones IP (IPAM) en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 82f8f382-246e-4164-8306-437f7a019e0f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5fad1062b65b4784a8a5ddfde927951230cb6ab8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355232"
---
# <a name="manage-resources-in-multiple-active-directory-forests"></a>Administración de recursos en varios bosques de Active Directory

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información sobre cómo usar IPAM para administrar controladores de dominio, servidores DHCP y servidores DNS en varios bosques de Active Directory.  
  
Para usar IPAM para administrar recursos en bosques de Active Directory remotos, cada bosque que desee administrar debe tener una relación de confianza bidireccional con el bosque en el que está instalado IPAM.  
  
Para iniciar el proceso de detección de distintos bosques de Active Directory, abra Administrador del servidor y haga clic en IPAM. En la consola de cliente de IPAM, haga clic en **configurar detección de servidores**y, a continuación, en **obtener bosques**. Esto inicia una tarea en segundo plano que detecta bosques de confianza y sus dominios. Una vez completado el proceso de detección, haga clic en **configurar detección de servidores**, que abre el siguiente cuadro de diálogo.  
  
![Configurar la detección de servidores](../../media/Manage-Resources-in-Multiple-Active-Directory-Forests/ipam_serverdiscovery.jpg)  

>[!NOTE]
>Por directiva de grupo el aprovisionamiento de @ no__t-0based para un escenario de Active Directory entre bosques, asegúrese de ejecutar el siguiente cmdlet de Windows PowerShell en el servidor IPAM y no en los controladores de dominio de confianza. Por ejemplo, si el servidor IPAM está unido al bosque corp.contoso.com y el bosque que confía es fabrikam.com, puede ejecutar el siguiente cmdlet de Windows PowerShell en el servidor IPAM en corp.contoso.com para el aprovisionamiento de directiva de grupo @ no__t-0based en el bosque fabrikam.com. Para ejecutar este cmdlet, debe ser miembro del grupo Admins. del dominio en el bosque fabrikam.com.

    
    Invoke-IpamGpoProvisioning -Domain fabrikam.COM -GpoPrefixName IPAMSERVER -IpamServerFqdn IPAM.CORP.CONTOSO.COM
    

En el cuadro de diálogo **configurar detección de servidores** , haga clic en **seleccionar el bosque**y, a continuación, elija el bosque que desea administrar con IPAM. Seleccione también los dominios que desea administrar y, a continuación, haga clic en **Agregar**.

En **Seleccione los roles de servidor que se van a detectar**, para cada dominio que desee administrar, especifique el tipo de servidores que desea detectar. Las opciones son **controlador de dominio**, **servidor DHCP**y **servidor DNS**.

De forma predeterminada, se detectan los controladores de dominio, los servidores DHCP y los servidores DNS, por lo que si no desea detectar uno de estos tipos de servidores, asegúrese de anular la selección de la casilla correspondiente a esa opción.

En la ilustración anterior del ejemplo, el servidor IPAM se instala en el bosque contoso.com y se agrega el dominio raíz del bosque fabrikam.com para la administración de IPAM. Los roles de servidor seleccionados permiten a IPAM detectar y administrar controladores de dominio, servidores DHCP y servidores DNS en el dominio raíz fabrikam.com y el dominio raíz contoso.com.

Después de haber especificado los bosques, dominios y roles de servidor, haga clic en **Aceptar**. IPAM realiza la detección y, cuando se completa la detección, puede administrar los recursos en el bosque local y remoto.
