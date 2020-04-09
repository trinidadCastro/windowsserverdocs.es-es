---
ms.assetid: d555041a-709e-42c7-920a-8dfd2c7e0488
title: Comprobar que un servidor proxy de federación está operativo
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 3e74af4a63476040ca44522ceb7c0ae22e914fec
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855858"
---
# <a name="verify-that-a-federation-server-proxy-is-operational"></a>Comprobar que un servidor proxy de federación está operativo


Puede usar el procedimiento siguiente para comprobar que el servidor proxy de Federación puede comunicarse con el Servicio de federación en Servicios de federación de Active Directory (AD FS) \(AD FS\). Ejecute este procedimiento después de ejecutar el Asistente para configuración de servidor **proxy de Federación de AD FS** para configurar el equipo para que se ejecute en el rol de servidor proxy de Federación. Para obtener más información sobre cómo ejecutar este asistente, vea [configurar un equipo para el rol de servidor proxy de Federación](Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md).  
  
> [!IMPORTANT]  
> Como resultado de esta prueba, se genera correctamente un evento específico en el Visor de eventos del equipo del servidor proxy de federación.  
  
La pertenencia al grupo **Administradores** o equivalente en el equipo local es el requisito mínimo necesario para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-verify-that-a-federation-server-proxy-is-operational"></a>Para comprobar que un servidor proxy de Federación está operativo  
  
1.  Inicie sesión en el servidor proxy de Federación como administrador.  
  
2.  En la pantalla **Inicio** , escriba**visor de eventos**y, a continuación, presione Entrar.  
  
3.  En el panel de detalles, haga doble\-haga clic en **registros de aplicaciones y servicios**, haga doble\-haga clic en **AD FS eventos**y, a continuación, haga clic en **Administración**.  
  
4.  En la columna **Id. del evento**, busca el id. de evento 198.  
  
    Si el servidor proxy de Federación está configurado correctamente, verá un nuevo evento en el registro de aplicaciones de Visor de eventos, con el ID. de evento 198. Este evento comprueba que el servicio de servidor proxy de Federación se inició correctamente y ahora está en línea.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: configuración de un servidor proxy de Federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

