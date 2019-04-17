---
ms.assetid: 1115d276-00f6-4c23-9278-eedcc31295d8
title: "Comprueba que el servidor de federación de Windows Server 2012 R2 Operational"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2df8a00a953196d7ca19ea0d164abbbf6eefd829
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="verify-your-windows-server-2012-r2-federation-server-is-operational"></a>Comprueba que el servidor de federación de Windows Server 2012 R2 Operational

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

Puedes usar los siguientes procedimientos para comprobar que un servidor de federación está operativo; es decir, que cualquier cliente en la misma red puede llegar a un servidor de federación de nuevo.  
  
Pertenencia a **usuarios**, **operadores de copia de seguridad**, **usuarios avanzados**, **administradores** o equivalente, en el equipo local es lo mínimo necesario para completar este procedimiento.  Revisar detalles sobre el uso de las cuentas adecuadas y agrupar pertenencias a [Local y dominio predeterminada grupos](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="procedure-1-to-verify-that-a-federation-server-is-operational"></a>Procedimiento 1: Comprobar que un servidor de federación está operativo  
  
1.  Para comprobar que Internet Information Services \(IIS\) está correctamente configurado en el servidor de federación, inicie sesión en un equipo cliente que se encuentra en el mismo bosque que el servidor de federación.  
  
2.  Abre una ventana del explorador, en el tipo de la barra de direcciones la federación de nombre de host DNS del servidor y luego anexa \/adfs\/fs\/federationserverservice.asmx a ella para el servidor de federación de nuevo, por ejemplo:  
  
    **https:\/\/FS1.fabrikam.com\/adfs\/fs\/federationserverservice.asmx**  
  
3.  Presione ENTRAR y, a continuación, se complete el procedimiento siguiente en el equipo de servidor de federación. Si aparece el mensaje **hay un problema con el certificado de seguridad del sitio Web**, haz clic en **continuar a este sitio Web**.  
  
    El resultado esperado es una visualización de XML con el documento de descripción de servicio. Si aparece esta página, IIS en el servidor de federación está operativas y preparar para páginas correctamente.  
  
Pertenencia a **administradores**, o equivalente, en el equipo local es lo mínimo necesario para completar este procedimiento.  Revisar detalles sobre el uso de las cuentas adecuadas y agrupar pertenencias a [Local y dominio predeterminada grupos](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="procedure-2-to-verify-that-a-federation-server-is-operational"></a>Procedimiento 2: Para comprobar que un servidor de federación está operativo  
  
1.  Iniciar sesión como administrador el servidor de federación de nuevo.  
  
2.  En la **inicio** , escriba**Visor de eventos**, y, a continuación, presione ENTRAR.  
  
3.  En el panel de detalles, double\ clic **registros de aplicaciones y servicios**, double\ clic **AD FS eventos**y, a continuación, haz clic en **administrador**.  
  
4.  En la **identificador de evento** columna, busque el identificador de 100. Si el servidor de federación está configurado correctamente, verás un nuevo evento, en el registro de aplicación de Visor de eventos, con el 100 de identificador de evento. Este evento se comprueba que el servidor de federación pudo comunicarse correctamente con el servicio de federación.  
  
## <a name="see-also"></a>Consulta también 

[Implementación de AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guía de implementación de Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Implementar un conjunto de servidor de federación](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
   
  

