---
ms.assetid: d555041a-709e-42c7-920a-8dfd2c7e0488
title: "Comprobar que un Proxy de servidor de federación está operativo"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 4900d8621b94a514a07bba55b2f7f3df5dd36353
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="verify-that-a-federation-server-proxy-is-operational"></a>Comprobar que un Proxy de servidor de federación está operativo

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puedes usar el siguiente procedimiento para comprobar que el proxy de servidor de federación pueda comunicarse con los servicios de federación de los servicios de federación de Active Directory \(AD FS\). Ejecutar este procedimiento después de ejecutar el **Asistente de configuración de Proxy de servidor de AD FS federación** para configurar el equipo para ejecutar la federación rol de servidor proxy. Para obtener más información sobre cómo ejecutar este asistente, consulta [configurar un equipo para el rol de Proxy del servidor de federación](Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md).  
  
> [!IMPORTANT]  
> El resultado de esta prueba es la generación de un evento específico en el Visor de eventos en el equipo de proxy del servidor de federación correcta.  
  
Pertenencia a **administradores**, o equivalente, en el equipo local es lo mínimo necesario para completar este procedimiento.  Revisar detalles sobre el uso de las cuentas adecuadas y agrupar pertenencias a [Local y dominio predeterminada grupos](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-verify-that-a-federation-server-proxy-is-operational"></a>Para comprobar que un proxy de servidor de federación está operativo  
  
1.  Iniciar sesión en el proxy de servidor de federación como administrador.  
  
2.  En la **inicio** , escriba**Visor de eventos**, y, a continuación, presione ENTRAR.  
  
3.  En el panel de detalles, double\ clic **registros de aplicaciones y servicios**, double\ clic **AD FS eventos**y, a continuación, haz clic en **administrador**.  
  
4.  En la **identificador de evento** columna, busca 198 de identificador de evento.  
  
    Si el proxy de servidor de federación está configurado correctamente, verás un nuevo evento en el registro de aplicación de Visor de eventos con el evento 198 ID. Este evento se comprueba que el servicio de proxy del servidor de federación se inició correctamente y que ahora está en línea.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: Configurar un servidor Proxy Server federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

