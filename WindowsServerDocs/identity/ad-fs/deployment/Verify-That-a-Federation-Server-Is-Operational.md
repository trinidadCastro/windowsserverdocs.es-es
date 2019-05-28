---
ms.assetid: ad61c586-ba8a-4534-8824-b45994d60c6b
title: Comprobar que un servidor de federación está operativo
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: b9498451e8f6d7701e9ed4b3ac7d61f19d2dcdb4
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191895"
---
# <a name="verify-that-a-federation-server-is-operational"></a>Comprobar que un servidor de federación está operativo


Puede realizar los siguientes procedimientos para comprobar que un servidor de federación está operativo; es decir, que todos los clientes de la misma red pueden acceder a un nuevo servidor de federación.  
  
Para completar este procedimiento, se requiere como mínimo la pertenencia a **Usuarios**, **Operadores de copia de seguridad**, **Usuarios avanzados**, **Administradores** o equivalente.  Revise los detalles sobre el uso de las cuentas adecuadas y pertenencia a grupos en [dominio grupos predeterminados locales y](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="procedure-1-to-verify-that-a-federation-server-is-operational"></a>Procedimiento 1: Para comprobar que un servidor de federación sea operativo  
  
1.  Para comprobar que Internet Information Services \(IIS\) está configurado correctamente en el servidor de federación, inicie sesión en un equipo cliente que se encuentra en el mismo bosque que el servidor de federación.  
  
2.  Abra una ventana del explorador, en el tipo de barra de dirección la federación de nombre de host DNS del servidor y luego anéxele /adfs/fs/federationserverservice.asmx para el nuevo servidor de federación, por ejemplo:  
  
    **https://fs1.fabrikam.com/adfs/fs/federationserverservice.asmx**  
  
3.  Presione ENTRAR y complete el siguiente procedimiento en el equipo de servidor de federación. Si aparece el mensaje **Existe un problema con el certificado de seguridad de este sitio web**, haga clic en **Pasar a este sitio web**.  
  
    Se espera que aparezca un documento XML con la descripción del servicio. Si esta página aparece, significa que IIS del servidor de federación está operativo y que está sirviendo correctamente a las páginas.  
  
El requisito mínimo para realizar este procedimiento es pertenecer al grupo **Administradores** o un grupo equivalente en el equipo local.  Revise los detalles sobre el uso de las cuentas adecuadas y pertenencia a grupos en [dominio grupos predeterminados locales y](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="procedure-2-to-verify-that-a-federation-server-is-operational"></a>Procedimiento 2: Para comprobar que un servidor de federación sea operativo  
  
1.  Inicie sesión en el nuevo servidor de federación como administrador.  
  
2.  En el **iniciar** , escriba **Visor de eventos**, y, a continuación, presione ENTRAR.  
  
3.  En el panel de detalles, haga doble\-haga clic en **registros de aplicaciones y servicios**, double\-haga clic en **AD FS Eventing**y, a continuación, haga clic en **Admin**.  
  
4.  En el **Id. de evento** columna, busque el identificador de evento 100. Si el servidor de federación está configurado correctamente, verá un nuevo evento, en el registro de aplicación del Visor de eventos, con el identificador de evento 100. Este evento comprueba que el servidor de federación fue capaz de comunicarse correctamente con el servicio de federación.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md)  
  

