---
title: Administración de recursos en varios bosques de Active Directory
description: Este tema forma parte de la Guía de administración de administración de direcciones IP (IPAM) en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 82f8f382-246e-4164-8306-437f7a019e0f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2bbd303df635af314cee2126a75f0569ede2f5de
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282188"
---
# <a name="manage-resources-in-multiple-active-directory-forests"></a>Administración de recursos en varios bosques de Active Directory

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este tema para aprender a usar IPAM para administrar controladores de dominio, servidores DHCP y servidores DNS en varios bosques de Active Directory.  
  
Para usar IPAM para administrar recursos de bosques de Active Directory remotos, cada bosque que desea administrar debe tener dos de confianza bidireccional con el bosque donde se instala IPAM.  
  
Para iniciar el proceso de detección de bosques de Active Directory diferentes, abra Administrador del servidor y haga clic en IPAM. En la consola de cliente IPAM, haga clic en **Configurar detección de servidor**y, a continuación, haga clic en **obtener bosques**. Esto inicia una tarea en segundo plano que detecta sus dominios y bosques de confianza. Al finalizar el proceso de detección, haga clic en **Configurar detección de servidor**, que abre el cuadro de diálogo siguiente.  
  
![Configurar la detección de servidores](../../media/Manage-Resources-in-Multiple-Active-Directory-Forests/ipam_serverdiscovery.jpg)  

>[!NOTE]
>La directiva de grupo\-en función de aprovisionamiento para un escenario entre bosques de Active Directory, asegúrese de ejecutar el siguiente cmdlet de Windows PowerShell en el servidor IPAM y no en lo controladores de dominio del dominio que confía. Por ejemplo, si el servidor IPAM está unido al bosque de Corp.contoso.com y el bosque que confía es fabrikam.com, puede ejecutar el siguiente cmdlet de Windows PowerShell en el servidor IPAM en corp.contoso.com la directiva de grupo\-de aprovisionamiento en la bosque de Fabrikam.com. Para ejecutar este cmdlet, debe ser miembro del grupo Admins. del dominio del bosque de fabrikam.com.

    
    Invoke-IpamGpoProvisioning -Domain fabrikam.COM -GpoPrefixName IPAMSERVER -IpamServerFqdn IPAM.CORP.CONTOSO.COM
    

En el **Configurar detección de servidor** cuadro de diálogo, haga clic en **seleccione el bosque**y, a continuación, elija el bosque que desea administrar con IPAM. Seleccione también los dominios que desea administrar y, a continuación, haga clic en **agregar**.

En **seleccionar los roles de servidor para detectar**, para cada dominio que desea administrar, especifique el tipo de servidores que quieres detectar. Las opciones son **controlador de dominio**, **servidor DHCP**, y **servidor DNS**.

De forma predeterminada, se detectan controladores de dominio, servidores DHCP y servidores DNS: por lo que si no desea detectar uno de estos tipos de servidores, asegúrese de que anule la selección de la casilla de verificación para esa opción.

En la ilustración del ejemplo anterior, el servidor IPAM se instala en el bosque contoso.com y se agrega el dominio raíz del bosque fabrikam.com para la administración de IPAM. Los roles de servidor seleccionado que IPAM pueda detectar y administrar controladores de dominio, servidores DHCP y servidores DNS en el dominio raíz de fabrikam.com y el dominio raíz de contoso.com.

Después de haber especificado los bosques, dominios y roles de servidor, haga clic en **Aceptar**. IPAM realiza la detección y, cuando se completa la detección, puede administrar los recursos del bosque local y remota.
