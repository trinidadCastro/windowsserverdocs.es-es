---
ms.assetid: 638c89bd-87e6-484b-9d2e-8ae2a74227e5
title: Establece un certificado de comunicaciones de servicio
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 0d630372d82cf1519da82397a9c90d6a28903ed0
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="set-a-service-communications-certificate"></a>Establece un certificado de comunicaciones de servicio

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Servidores de federación de los servicios de federación de Active Directory \(AD FS\) usan el certificado de comunicaciones de servicio para proteger el tráfico de servicios Web para la comunicación de capa de Sockets seguros \(SSL\) con los clientes Web o con servidores proxy de servidor de federación. Este es el mismo certificado de un servidor de federación se usa como el certificado SSL en Internet Information Services \(IIS\).  
  
Puedes usar el siguiente procedimiento para cambiar el certificado de comunicaciones de servicio con la administración de AD FS en snap\.  
  
> [!NOTE]  
> La administración de AD FS en snap\ hace referencia a los certificados de autenticación de servidor para servidores de federación como certificados de comunicación de servicio.  
  
Pertenencia a **administradores**, o equivalente, en el equipo local es lo mínimo necesario para completar este procedimiento.  ¿Revisar detalles sobre el uso de las cuentas adecuadas y agrupar pertenencias a [Local y dominio predeterminada grupos](https://go.microsoft.com/fwlink/?LinkId=83477) \ (http:///\/ go.microsoft.com\/fwlink\ /? LinkId\ = 83477\).   
  
### <a name="to-set-a-service-communications-certificate"></a>Para establecer un certificado de comunicaciones de servicio  
  
1.  En la **inicio**, escriba**AD FS administración**, y, a continuación, presione ENTRAR.  
  
2.  En el árbol de consola, double\ clic **servicio**y, a continuación, haz clic en **certificados**.  
  
3.  En la **acciones** panel, haz clic en el **establecer un certificado de comunicaciones de servicio** vínculo.  
  
4.  En la **seleccionar un certificado de comunicaciones de servicio** diálogo cuadro, vaya al archivo de certificado que quieras establecer como el certificado de comunicaciones de servicio, selecciona el archivo de certificado y, a continuación, haz clic en **abrir**.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: Configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Requisitos de certificado para los servidores de federación](https://technet.microsoft.com/library/dd807040.aspx)  
  

