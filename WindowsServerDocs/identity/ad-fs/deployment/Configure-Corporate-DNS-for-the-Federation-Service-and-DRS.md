---
ms.assetid: aca4a4fa-b12c-4eed-a499-f9aedb7d2fd6
title: "Configurar DNS corporativa para los servicios de federación y DRS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 9b66bed99cbc2ac2cdf116579adaea282c45fabe
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="configure-corporate-dns-for-the-federation-service-and-drs"></a>Configurar DNS corporativa para los servicios de federación y DRS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2
  
## <a name="step-6-add-a-host-a-and-alias-cname-resource-record-to-corporate-dns-for-the-federation-service-and-drs"></a>Paso 6: Agregar un Host \(A\) y Alias \(CNAME\) registro de recursos corporativos DNS para los servicios de federación y DRS  
Debes agregar los siguientes registros de recursos corporativos \(DNS\) sistema de nombres de dominio para el servicio de federación y servicio de registro de dispositivos que configuraste en los pasos anteriores.  
  
|Entrada|Tipo|Dirección|  
|---------|--------|-----------|  
|federation\_service\_name|Hospedar \(A\)|Dirección IP del servidor de AD FS o la dirección IP de la que está configurado delante de la granja de servidores de AD FS equilibrado de carga|  
|enterpriseregistration|Alias \(CNAME\)|federation\_server\_name.contoso.com|  
  
Puedes usar el siguiente procedimiento para agregar host \(A\) y alias \(CNAME\) registros de recursos corporativos DNS para el servidor de federación y el servicio de registro del dispositivo.  
  
Pertenencia a **administradores**, o equivalente, es el requisito mínimo para completar este procedimiento.  Revisar detalles sobre el uso de las cuentas adecuadas y agrupar pertenencias a [Local y dominio predeterminada grupos](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-and-alias-cname-resource-records-to-dns-for-your-federation-server"></a>Para agregar host \(A\) y alias \(CNAME\) registros de recursos a DNS para el servidor de federación  
  
1.  En el controlador de dominio, en el administrador del servidor, en la **herramientas** menú, haz clic en **DNS** para abrir el DNS en snap\.  
  
2.  En el árbol de consola, expande el **domain\_controller\_name** nodo, expanda **zonas de búsqueda directa**, right\ clic **domain\_name**y, a continuación, haz clic en **nuevo Host \(A or AAAA\)**.  
  
3.  En la **nombre** , escriba el nombre que se usará para el conjunto de AD FS.  
  
4.  En la **dirección IP** , escriba la dirección IP del servidor de federación. Haz clic en **agregar Host**.  
  
5.  Right\ y haga clic en el **domain\_name** nodo y, a continuación, haz clic en **nuevo Alias \(CNAME\)**.  
  
6.  En la **nuevo registro de recursos** cuadro de diálogo, escribe **enterpriseregistration** en la **nombre Alias** cuadro.  
  
7.  En el nombre de dominio completo nombre \(FQDN\) del cuadro de host de destino, tipo **federation\_service\_farm\_name.domain\_name.com**y, a continuación, haz clic en **Aceptar**.  
  
    > [!IMPORTANT]  
    > En una implementación del mundo real, si tu empresa tiene varios sufijos \(UPN\) nombre Principal de usuario, debes crear varios registros CNAME para cada uno de esos sufijos en DNS.  
  
## <a name="see-also"></a>Consulta también 

[Implementación de AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guía de implementación de Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Implementar un conjunto de servidor de federación](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

