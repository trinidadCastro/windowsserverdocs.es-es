---
title: Solucionar problemas relacionados con la activación de OTP
description: En este tema forma parte de la Guía de implementación de acceso remoto con autenticación OTP en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b58252ca-4c1d-4664-a3c4-7301e2121517
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 55314f3bd5e3500847beed256580b1924521abc9
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280797"
---
# <a name="troubleshooting-enabling-otp"></a>Solucionar problemas relacionados con la activación de OTP

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Este tema contiene información para solucionar problemas relacionados con la habilitación de la autenticación de OTP de DirectAccess mediante el **Enable DAOtpAuthentication** cmdlet de PowerShell o la consola de administración de acceso remoto.
  
## <a name="failed-to-enroll-the-otp-signing-certificate"></a>No se pudo inscribir el certificado de firma de OTP  
**Recibidas el error** (registro de eventos de servidor). No se puede inscribir un certificado de firma de OTP con plantilla de certificado < OTP_signing_template_name >  
  
**Causa**  
  
Existen tres posibles causas de este error:  
  
-   La plantilla no existe.  
  
-   Los permisos establecidos en la plantilla no permitir que el servidor de DirectAccess a inscribir.  
  
-   No hay ninguna conectividad de red a la entidad de certificación emisora (CA).  
  
**Solución**  
  
1.  Asegúrese de que la firma de OTP plantilla de certificado con el nombre especificado:  
  
    1.  Existe y tiene los permisos adecuados.  
  
    2.  Se establece para ser emitidos por al menos una entidad emisora de certificados que puede emitir certificados para el servidor de DirectAccess.  
  
2.  Si la plantilla no existe, crearlo tal como se describe en el Plan 3.3 el certificado de autoridad de registro o si existe otra plantilla coincidente, volver a configurar OTP de DirectAccess con el nuevo nombre de plantilla.  
  
## <a name="failed-to-enable-directaccess-otp-when-webdav-is-installed"></a>No se pudo habilitar OTP de DirectAccess cuando se instala WebDAV  
**Escenario**. Al intentar aplicar la configuración de OTP de DirectAccess en la consola de administración de acceso remoto o mediante el `Enable-DAOtpAuthentication` cmdlet de PowerShell, la operación se produce un error.  
  
**Recibidas el error** (registro de eventos de servidor). No se puede aplicar la configuración de OTP de DirectAccess porque la extensión WebDAV IIS se está ejecutando en el servidor. Quitar WebDAV y vuelva a aplicar la configuración.  
  
**Causa**  
  
El servicio de OTP de DirectAccess no es compatible con la característica de publicación en WebDAV y no se puede habilitar si WebDAV está instalado.  
  
**Solución**  
  
Desinstalar el rol de WebDAV:  
  
1.  En la consola de administrador del servidor, en el panel izquierdo, haga clic en **IIS**.  
  
2.  En el panel principal, desplácese a **ROLES y características**.  
  
3.  Haga clic en **publicación en WebDAV**y, a continuación, haga clic en **quitar rol o característica**.  
  
4.  Complete el quitar Roles y características Asistente.  
  
5.  Vuelva a aplicar la configuración de OTP de DirectAccess.  
  
## <a name="no-templates-available-in-the-remote-access-management-console"></a>No hay plantillas disponibles en la consola de administración de acceso remoto  
**Escenario**. Al configurar OTP o el registro de plantillas de certificado de entidad mediante la consola de administración de acceso remoto, algunas o todas las plantillas no están en las ventanas de selección.  
  
**Causa**  
  
Hay dos causas posibles para este error:  
  
-   La plantilla no está configurada según los requisitos de OTP de DirectAccess y, por lo que no se pueden seleccionar.  
  
-   Las CA seleccionadas en **servidores de CA OTP** no están configuradas para emitir las plantillas necesarias.  
  
**Solución**  
  
1.  Asegúrese de que la plantilla de inicio de sesión OTP y la plantilla de certificado de firma de OTP están configurados correctamente, como se describe en el Plan 3.2 de la plantilla de certificado OTP y 3.3 planear el certificado de autoridad de registro.  
  
2.  Asegúrese de que las CA configuradas en el **servidores de CA OTP** lista son configurado para problemas de las plantillas pertinentes:  
  
    1.  En el servidor de CA, abra la consola de entidad de certificación.  
  
    2.  En el panel izquierdo, expanda el servidor de CA elegido.  
  
    3.  Haga clic en **plantillas de certificado** y asegúrese de que están habilitadas las plantillas necesarias. Si no, haga clic en **plantillas de certificado**, haga clic en **New**, haga clic en **plantilla de certificado para emitir**y, a continuación, seleccione las plantillas que desea habilitar.  
  
## <a name="cannot-set-renewal-period-of-otp-template-to-1-hour"></a>No se puede establecer el período de renovación de la plantilla OTP a 1 hora  
**Escenario**. Al configurar la plantilla de inicio de sesión de OTP de DirectAccess mediante Windows 2003 CA, no es posible establecer el período de renovación de la plantilla a 1 hora.  
  
**Causa**  
  
El complemento MMC de plantillas de certificado en Windows Server 2003 no permite establecer el período de renovación de una plantilla a 1 hora.  
  
**Solución**  
  
Instalar complemento de plantillas de certificado en un servidor de posteriores a Windows Server 2003 y usarlo para configurar la plantilla de inicio de sesión OTP, consulte [instalar el complemento Plantillas de certificado](https://technet.microsoft.com/library/cc732445.aspx).  
  


