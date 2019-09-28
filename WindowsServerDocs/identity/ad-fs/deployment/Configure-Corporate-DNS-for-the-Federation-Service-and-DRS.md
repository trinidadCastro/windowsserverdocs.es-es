---
ms.assetid: aca4a4fa-b12c-4eed-a499-f9aedb7d2fd6
title: Configurar el DNS corporativo para los servicios de federación y DRS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 9f0b04f9dc050117fdefc630759c86d2b1bb1ecc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408447"
---
# <a name="configure-corporate-dns-for-the-federation-service-and-drs"></a>Configurar el DNS corporativo para los servicios de federación y DRS
  
## <a name="step-6-add-a-host-a-and-alias-cname-resource-record-to-corporate-dns-for-the-federation-service-and-drs"></a>Paso 6: Agregue un host \(A @ no__t-1 y un registro de recursos de alias \(CNAME @ no__t-3 al DNS corporativo para el Servicio de federación y el DRS  
Debe agregar los siguientes registros de recursos al sistema de nombres de dominio corporativo \(DNS @ no__t-1 para el servicio de Federación y el servicio de registro de dispositivos que configuró en los pasos anteriores.  
  
|Entrada|Tipo|Dirección|  
|---------|--------|-----------|  
|Federation @ no__t-0service @ no__t-1name|Host \(A @ no__t-1|Dirección IP del servidor de AD FS o la dirección IP del equilibrador de carga que se configura delante de la granja de servidores de AD FS|  
|enterpriseregistration|Alias \(CNAME @ no__t-1|Federación @ no__t-0server\_name.contoso.com|  
  
Puede usar el siguiente procedimiento para agregar un host \(A @ no__t-1 y alias \(CNAME @ no__t-3 registros de recursos al DNS corporativo para el servidor de Federación y el servicio de registro de dispositivos.  
  
La pertenencia al grupo **administradores**, o equivalente, es el requisito mínimo para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-and-alias-cname-resource-records-to-dns-for-your-federation-server"></a>Para agregar un host \(A @ no__t-1 y los registros de recursos de alias \(CNAME @ no__t-3 a DNS para el servidor de Federación  
  
1.  En el controlador de dominio, en Administrador del servidor, en el menú **herramientas** , haga clic en **DNS** para abrir el complemento DNS @ no__t-2in.  
  
2.  En el árbol de consola, expanda el nodo **dominio @ no__t-1controller @ no__t-2name** , expanda **zonas de búsqueda directa**, haga clic en @ no__t-4click **dominio @ no__t-6name**y, a continuación, haga clic en **nuevo host \(A o AAAA @ no__t-9**.  
  
3.  En el cuadro **nombre** , escriba el nombre que se usará para la granja de AD FS.  
  
4.  En el cuadro **Dirección IP**, escribe la dirección IP del servidor de federación. Haga clic en **Agregar host**.  
  
5.  Pulse @ no__t-0click en el nodo **dominio @ no__t-2name** y, a continuación, haga clic en **nuevo alias \(CNAME @ no__t-5**.  
  
6.  En el cuadro de diálogo **Nuevo registro de recursos**, escribe **enterpriseregistration** en el cuadro **Nombre de alias**.  
  
7.  En el nombre de dominio completo \(FQDN @ no__t-1 del cuadro host de destino, escriba **Federation @ no__t-3service @ no__t-4farm @ no__t-5name. Domain @ no__t-6name. com**y, a continuación, haga clic en **Aceptar**.  
  
    > [!IMPORTANT]  
    > En una implementación real, si su empresa tiene varios sufijos de nombre principal de usuario \(UPN @ no__t-1, debe crear varios registros CNAME para cada uno de esos sufijos UPN en DNS.  
  
## <a name="see-also"></a>Vea también 

[Implementación de AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guía de implementación de AD FS de Windows Server 2012 R2](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Implementar una granja de servidores de federación](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

