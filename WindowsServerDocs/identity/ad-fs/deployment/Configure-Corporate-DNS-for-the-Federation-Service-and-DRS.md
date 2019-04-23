---
ms.assetid: aca4a4fa-b12c-4eed-a499-f9aedb7d2fd6
title: Configurar el DNS corporativo para los servicios de federación y DRS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 9b66bed99cbc2ac2cdf116579adaea282c45fabe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876396"
---
# <a name="configure-corporate-dns-for-the-federation-service-and-drs"></a>Configurar el DNS corporativo para los servicios de federación y DRS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2
  
## <a name="step-6-add-a-host-a-and-alias-cname-resource-record-to-corporate-dns-for-the-federation-service-and-drs"></a>Paso 6: Agregar un Host \(A\) y Alias \(CNAME\) registro de recurso al DNS corporativo para el servicio de federación y DRS  
Debe agregar los siguientes registros de recursos para el sistema de nombres de dominio corporativo \(DNS\) para su servicio de federación y el servicio de registro de dispositivo que configuró en los pasos anteriores.  
  
|Entrada|Tipo|Dirección|  
|---------|--------|-----------|  
|federation\_service\_name|Host \(A\)|Dirección IP del servidor de AD FS o la dirección IP del equilibrador de carga que se configura delante de la granja de servidores de AD FS|  
|enterpriseregistration|Alias \(CNAME\)|federation\_server\_name.contoso.com|  
  
Puede usar el siguiente procedimiento para agregar un host \(A\) y alias \(CNAME\) los registros de recursos DNS corporativo para el servidor de federación y el servicio de registro de dispositivos.  
  
Pertenencia a **administradores**, o equivalente, es el requisito mínimo para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas adecuadas y pertenencia a grupos en [dominio grupos predeterminados locales y](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-and-alias-cname-resource-records-to-dns-for-your-federation-server"></a>Para agregar un host \(A\) y alias \(CNAME\) los registros de recursos DNS para el servidor de federación  
  
1.  En, controlador de dominio en el administrador del servidor, en el **herramientas** menú, haga clic en **DNS** para abrir el complemento DNS\-en.  
  
2.  En el árbol de consola, expanda el **dominio\_controlador\_nombre** nodo, expanda **zonas de búsqueda directa**, ¿no\-haga clic en **dominio\_nombre**y, a continuación, haga clic en **nuevo Host \(A o AAAA\)**.  
  
3.  En el **nombre** , escriba el nombre que se usará para la granja de AD FS.  
  
4.  En el cuadro **Dirección IP**, escribe la dirección IP del servidor de federación. Haga clic en **Agregar host**.  
  
5.  Derecha\-haga clic en el **dominio\_nombre** nodo y, a continuación, haga clic en **nuevo Alias \(CNAME\)**.  
  
6.  En el cuadro de diálogo **Nuevo registro de recursos**, escribe **enterpriseregistration** en el cuadro **Nombre de alias**.  
  
7.  En el nombre de dominio completo \(FQDN\) del cuadro del host de destino, escriba **federación\_servicio\_granja\_name.domain\_name.com**y, a continuación, Haga clic en **Aceptar**.  
  
    > [!IMPORTANT]  
    > En una implementación del mundo real, si su empresa tiene varios nombre Principal de usuario \(UPN\) sufijos, debe crear varios registros CNAME para cada uno de los sufijos UPN en DNS.  
  
## <a name="see-also"></a>Vea también 

[Implementación de AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guía de implementación de Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Implementar una granja de servidores de federación](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

