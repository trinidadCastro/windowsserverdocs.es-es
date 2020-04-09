---
ms.assetid: aca4a4fa-b12c-4eed-a499-f9aedb7d2fd6
title: Configurar el DNS corporativo para los servicios de federación y DRS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 3e3f2b36b7949e4bbde78942006e985f41abf9df
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80814269"
---
# <a name="configure-corporate-dns-for-the-federation-service-and-drs"></a>Configurar el DNS corporativo para los servicios de federación y DRS
  
## <a name="step-6-add-a-host-a-and-alias-cname-resource-record-to-corporate-dns-for-the-federation-service-and-drs"></a>Paso 6: agregar un host \(un\) y alias \(registro de recursos de\) CNAME al DNS corporativo para el Servicio de federación y el DRS  
Debe agregar los siguientes registros de recursos al sistema de nombres de dominio corporativo \(\) DNS para el servicio de Federación y el servicio de registro de dispositivos que configuró en los pasos anteriores.  
  
|Entrada|Tipo|Dirección|  
|---------|--------|-----------|  
|nombre\_servicio de\_de Federación|Host \(un\)|Dirección IP del servidor de AD FS o la dirección IP del equilibrador de carga que se configura delante de la granja de servidores de AD FS|  
|enterpriseregistration|Alias \(CNAME\)|servidor de\_de Federación\_name.contoso.com|  
  
Puede usar el siguiente procedimiento para agregar un host \(un\) y alias \(registros de recursos de\) CNAME al DNS corporativo para el servidor de Federación y el servicio de registro de dispositivos.  
  
La pertenencia al grupo **administradores**, o equivalente, es el requisito mínimo para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-and-alias-cname-resource-records-to-dns-for-your-federation-server"></a>Para agregar un host \(un\) y alias \(registros de recursos de\) CNAME a DNS para el servidor de Federación  
  
1.  En el controlador de dominio, en Administrador del servidor, en el menú **herramientas** , haga clic en **DNS** para abrir el\-de complemento DNS en.  
  
2.  En el árbol de consola, expanda el nodo **controlador de dominio\_\_** , expanda **zonas de búsqueda directa**, haga clic con el botón derecho\-en **dominio\_nombre**y, a continuación, haga clic en **nuevo host \(a o AAAA\)** .  
  
3.  En el cuadro **nombre** , escriba el nombre que se usará para la granja de AD FS.  
  
4.  En el cuadro **Dirección IP**, escribe la dirección IP del servidor de federación. Haga clic en **Agregar host**.  
  
5.  \-haga clic en el nodo **dominio\_nombre** y, a continuación, haga clic en **nuevo Alias \(CNAME\)** .  
  
6.  En el cuadro de diálogo **Nuevo registro de recursos**, escribe **enterpriseregistration** en el cuadro **Nombre de alias**.  
  
7.  En el nombre de dominio completo \(FQDN\) del cuadro host de destino, escriba **federación\_servicio\_granja\_nombre. dominio\_Name.com**y, a continuación, haga clic en **Aceptar**.  
  
    > [!IMPORTANT]  
    > En una implementación real, si su empresa tiene varios nombres principales de usuario \(sufijos de UPN\), debe crear varios registros CNAME para cada uno de esos sufijos UPN en DNS.  
  
## <a name="see-also"></a>Consulta también 

[Implementación de AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guía de implementación de AD FS de Windows Server 2012 R2](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Implementar una granja de servidores de federación](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

