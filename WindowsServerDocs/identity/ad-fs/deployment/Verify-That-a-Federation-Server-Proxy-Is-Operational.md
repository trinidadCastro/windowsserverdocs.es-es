---
ms.assetid: d555041a-709e-42c7-920a-8dfd2c7e0488
title: Comprobar que un servidor proxy de federación está operativo
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 4900d8621b94a514a07bba55b2f7f3df5dd36353
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814626"
---
# <a name="verify-that-a-federation-server-proxy-is-operational"></a>Comprobar que un servidor proxy de federación está operativo

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puede usar el procedimiento siguiente para comprobar que el servidor proxy de federación puede comunicarse con el servicio de federación de Active Directory Federation Services \(AD FS\). Ejecute este procedimiento después de ejecutar el **Asistente para la configuración de AD FS Federation Server Proxy** para configurar el equipo para ejecutar en el rol de servidor proxy de federación. Para obtener más información sobre cómo ejecutar este asistente, consulte [configurar un equipo para el rol de servidor Proxy de federación](Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md).  
  
> [!IMPORTANT]  
> Como resultado de esta prueba, se genera correctamente un evento específico en el Visor de eventos del equipo del servidor proxy de federación.  
  
El requisito mínimo para realizar este procedimiento es pertenecer al grupo **Administradores** o un grupo equivalente en el equipo local.  Revise los detalles sobre el uso de las cuentas adecuadas y pertenencia a grupos en [dominio grupos predeterminados locales y](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-verify-that-a-federation-server-proxy-is-operational"></a>Para comprobar que un servidor proxy de federación está operativo  
  
1.  Inicie sesión en el servidor proxy de federación como administrador.  
  
2.  En el **iniciar** , escriba**Visor de eventos**, y, a continuación, presione ENTRAR.  
  
3.  En el panel de detalles, haga doble\-haga clic en **registros de aplicaciones y servicios**, double\-haga clic en **AD FS Eventing**y, a continuación, haga clic en **Admin**.  
  
4.  En la columna **Id. del evento**, busque el id. de evento 198.  
  
    Si el servidor proxy de federación está configurado correctamente, verá un nuevo evento en el registro de aplicación del Visor de eventos, con el evento ID 198. Este evento comprueba que el servicio de proxy de servidor de federación se inició correctamente y ahora está en línea.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: Configurar un servidor Proxy de federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
