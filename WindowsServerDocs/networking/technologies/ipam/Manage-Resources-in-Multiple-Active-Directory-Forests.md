---
title: Administrar recursos en varios bosques de Active Directory
description: Este tema es parte de la Guía de administración de administración de direcciones IP (IPAM) en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 82f8f382-246e-4164-8306-437f7a019e0f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b01680d4b35461ff85965781ebc60e7a613d1cb8
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="manage-resources-in-multiple-active-directory-forests"></a>Administrar recursos en varios bosques de Active Directory

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para obtener información sobre cómo usar IPAM para administrar controladores de dominio, servidores DHCP y servidores DNS en varios bosques de Active Directory.  
  
Para usar IPAM para administrar recursos remotos bosques de Active Directory, cada bosque que quieres administrar debe tener dos forma de confianza con el bosque donde está instalado IPAM.  
  
Para iniciar el proceso de detección de bosques de Active Directory diferentes, abra el administrador del servidor y pulse IPAM. En la consola del cliente IPAM, haz clic en **configurar la detección de servidor**y, a continuación, haz clic en **obtener bosques**. Esta opción inicia una tarea en segundo plano que haya detectado sus dominios y bosques de confianza. Una vez completado el proceso de detección, haz clic en **configurar la detección de servidor**, que abre el cuadro de diálogo siguiente.  
  
![Configurar la detección de servidor](../../media/Manage-Resources-in-Multiple-Active-Directory-Forests/ipam_serverdiscovery.jpg)  

>[!NOTE]
>Para el aprovisionamiento de basados en grupo contraseña\ para un escenario de bosque de Active Directory entre, asegúrate de que ejecutes el siguiente cmdlet de Windows PowerShell en el servidor IPAM y no en lo controladores de dominio del dominio que confía. Por ejemplo, si el servidor IPAM es parte del bosque corp.contoso.com y el bosque de confianza es fabrikam.com, puedes ejecutar el siguiente cmdlet de Windows PowerShell en el servidor IPAM corp.contoso.com basados en grupo contraseña\ aprovisionamiento en el bosque fabrikam.com. Para ejecutar este cmdlet, debe ser miembro del grupo Administradores de dominio del bosque fabrikam.com.

    
    Invoke-IpamGpoProvisioning -Domain fabrikam.COM -GpoPrefixName IPAMSERVER -IpamServerFqdn IPAM.CORP.CONTOSO.COM
    

En la **configurar la detección de servidor** cuadro de diálogo, haz clic en **selecciona del bosque**y, a continuación, elige el bosque que desee administrar con IPAM. También seleccionar los dominios que quieras administrar y, a continuación, haz clic en **agregar**.

En **selecciona los roles de servidor para descubrir**, para cada dominio que quieras administrar, especificar el tipo de servidores para detectar. Las opciones son **controlador de dominio**, **servidor DHCP**, y **servidor DNS**.

De manera predeterminada, se detectan los controladores de dominio, servidores DHCP y servidores DNS, por lo tanto, si no quieres descubrir uno de estos tipos de servidores, asegúrate de que se anule la selección de la casilla de verificación para esta opción.

En la ilustración de ejemplo anterior, el servidor IPAM está instalado en el bosque contoso.com y se agrega el dominio raíz del bosque fabrikam.com para la administración de IPAM. Los roles de servidor seleccionado permiten IPAM detectar y administrar controladores de dominio, servidores DHCP y servidores DNS en el dominio raíz del fabrikam.com y el dominio de raíz contoso.com.

Después de haber especificado roles de servidor, dominios y bosques, haz clic en **Aceptar**. IPAM realiza la detección y, cuando se complete la detección, puede administrar recursos en el bosque local y remoto.
