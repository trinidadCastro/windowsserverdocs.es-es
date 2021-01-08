---
title: Solucionar problemas relacionados con la activación de OTP
description: Obtenga información acerca de cómo solucionar problemas relacionados con la habilitación de la autenticación de OTP de DirectAccess mediante el cmdlet Enable-DAOtpAuthentication PowerShell o la consola de administración de acceso remoto.
manager: brianlic
ms.topic: article
ms.assetid: b58252ca-4c1d-4664-a3c4-7301e2121517
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: a7189238da0c318b89876955913c2cf687c7a5f5
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2021
ms.locfileid: "98039675"
---
# <a name="troubleshooting-enabling-otp"></a>Solucionar problemas relacionados con la activación de OTP

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Este tema contiene información para la solución de problemas relacionados con la habilitación de la autenticación OTP de DirectAccess mediante el cmdlet **de PowerShell enable-DAOtpAuthentication** o la consola de administración de acceso remoto.

## <a name="failed-to-enroll-the-otp-signing-certificate"></a>No se pudo inscribir el certificado de firma de OTP
**Error recibido** (registro de eventos de servidor). No se puede inscribir un certificado de firma de OTP con la plantilla de certificado <OTP_signing_template_name>

**Causa**

Hay tres causas posibles de este error:

-   La plantilla no existe.

-   Los permisos establecidos en la plantilla no permiten inscribir el servidor de DirectAccess.

-   No hay conectividad de red a la entidad de certificación (CA) emisora.

**Solución**

1.  Asegúrese de que la plantilla de certificado de firma de OTP con el nombre especificado:

    1.  Existe y tiene los permisos adecuados.

    2.  Está configurado para ser emitido por al menos una entidad de certificación que pueda emitir certificados para el servidor de DirectAccess.

2.  Si la plantilla no existe, créela como se describe en 3,3 planear el certificado de la autoridad de registro, o bien, si existe otra plantilla que coincida, vuelva a configurar OTP de DirectAccess con el nuevo nombre de plantilla.

## <a name="failed-to-enable-directaccess-otp-when-webdav-is-installed"></a>No se pudo habilitar OTP de DirectAccess al instalar WebDAV
**Escenario**. Al intentar aplicar la configuración de OTP de DirectAccess en la consola de administración de acceso remoto o mediante el `Enable-DAOtpAuthentication` cmdlet de PowerShell, se produce un error en la operación.

**Error recibido** (registro de eventos de servidor). No se puede aplicar la configuración de OTP de DirectAccess porque la extensión de IIS WebDAV se está ejecutando en el servidor. Quite WebDAV y vuelva a aplicar la configuración.

**Causa**

El servicio OTP de DirectAccess no es compatible con la característica de publicación de WebDAV y no se puede habilitar mientras WebDAV está instalado.

**Solución**

Desinstale el rol de WebDAV:

1.  En la consola de Administrador del servidor, en el panel izquierdo, haga clic en **IIS**.

2.  En el panel principal, desplácese a **roles y características**.

3.  Haga clic con el botón secundario en **publicación WebDAV** y, a continuación, haga clic en **quitar rol o característica**.

4.  Complete el Asistente para quitar roles y características.

5.  Vuelva a aplicar la configuración de OTP de DirectAccess.

## <a name="no-templates-available-in-the-remote-access-management-console"></a>No hay plantillas disponibles en la consola de administración de acceso remoto
**Escenario**. Al configurar las plantillas de certificado de OTP o de autoridad de registro mediante la consola de administración de acceso remoto, algunas o todas las plantillas no se encuentran en las ventanas de selección.

**Causa**

Hay dos causas posibles para este error:

-   La plantilla no se configura según los requisitos de OTP de DirectAccess y, por tanto, no se puede seleccionar.

-   Las CA seleccionadas en **servidores de CA de OTP** no están configuradas para emitir las plantillas necesarias.

**Solución**

1.  Asegúrese de que la plantilla de inicio de sesión de OTP y la plantilla de certificado de firma de OTP están configuradas correctamente, tal y como se describe en 3,2 planeamiento de la plantilla de certificado OTP y 3,3 planear el certificado de la entidad de registro.

2.  Asegúrese de que las CA configuradas en la lista **servidores de CA de OTP** estén configuradas para emitir las plantillas relevantes:

    1.  En el servidor de CA, abra la consola entidad de certificación.

    2.  En el panel izquierdo, expanda el servidor de CA elegido.

    3.  Haga clic en **plantillas de certificado** y asegúrese de que las plantillas necesarias estén habilitadas. Si no es así, haga clic con el botón secundario en **plantillas de certificado**, haga clic en **nuevo**, haga clic en **plantilla de certificado para emitir** y, a continuación, seleccione las plantillas que desea habilitar.

## <a name="cannot-set-renewal-period-of-otp-template-to-1-hour"></a>No se puede establecer el período de renovación de la plantilla OTP en 1 hora
**Escenario**. Al configurar la plantilla de inicio de sesión OTP de DirectAccess con la CA de Windows 2003, no es posible establecer el período de renovación de la plantilla en 1 hora.

**Causa**

El complemento MMC plantillas de certificado de Windows Server 2003 no permite establecer el período de renovación de una plantilla en 1 hora.

**Solución**

Instalar el complemento plantillas de certificado en un servidor posterior a Windows Server 2003 y usarlo para configurar la plantilla de inicio de sesión de OTP, vea [instalar el complemento plantillas de certificado](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732445(v=ws.11)).

