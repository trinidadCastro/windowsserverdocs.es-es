---
ms.assetid: ad61c586-ba8a-4534-8824-b45994d60c6b
title: "Comprobar que un servidor de federación está operativo"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 2034b4c35061879a64004486395d0887c59087b2
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="verify-that-a-federation-server-is-operational"></a>Comprobar que un servidor de federación está operativo

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puedes usar los siguientes procedimientos para comprobar que un servidor de federación está operativo; es decir, que cualquier cliente en la misma red puede llegar a un servidor de federación de nuevo.  
  
Pertenencia a **usuarios**, **operadores de copia de seguridad**, **usuarios avanzados**, **administradores** o equivalente, en el equipo local es lo mínimo necesario para completar este procedimiento.  Revisar detalles sobre el uso de las cuentas adecuadas y agrupar pertenencias a [Local y dominio predeterminada grupos](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="procedure-1-to-verify-that-a-federation-server-is-operational"></a>Procedimiento 1: Comprobar que un servidor de federación está operativo  
  
1.  Para comprobar que Internet Information Services \(IIS\) está correctamente configurado en el servidor de federación, inicie sesión en un equipo cliente que se encuentra en el mismo bosque que el servidor de federación.  
  
2.  Abre una ventana del explorador, en el tipo de la barra de direcciones la federación de nombre de host DNS del servidor y luego anexa /adfs/fs/federationserverservice.asmx a ella para el servidor de federación de nuevo, por ejemplo:  
  
    **https://FS1.fabrikam.com/adfs/fs/federationserverservice.asmx**  
  
3.  Presione ENTRAR y, a continuación, se complete el procedimiento siguiente en el equipo de servidor de federación. Si aparece el mensaje **hay un problema con el certificado de seguridad del sitio Web**, haz clic en **continuar a este sitio Web**.  
  
    El resultado esperado es una visualización de XML con el documento de descripción de servicio. Si aparece esta página, IIS en el servidor de federación está operativas y preparar para páginas correctamente.  
  
Pertenencia a **administradores**, o equivalente, en el equipo local es lo mínimo necesario para completar este procedimiento.  Revisar detalles sobre el uso de las cuentas adecuadas y agrupar pertenencias a [Local y dominio predeterminada grupos](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="procedure-2-to-verify-that-a-federation-server-is-operational"></a>Procedimiento 2: Para comprobar que un servidor de federación está operativo  
  
1.  Iniciar sesión como administrador el servidor de federación de nuevo.  
  
2.  En la **inicio** , escriba **Visor de eventos**, y, a continuación, presione ENTRAR.  
  
3.  En el panel de detalles, double\ clic **registros de aplicaciones y servicios**, double\ clic **AD FS eventos**y, a continuación, haz clic en **administrador**.  
  
4.  En la **identificador de evento** columna, busque el identificador de 100. Si el servidor de federación está configurado correctamente, verás un nuevo evento, en el registro de aplicación de Visor de eventos, con el 100 de identificador de evento. Este evento se comprueba que el servidor de federación pudo comunicarse correctamente con el servicio de federación.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: Configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md)  
  

