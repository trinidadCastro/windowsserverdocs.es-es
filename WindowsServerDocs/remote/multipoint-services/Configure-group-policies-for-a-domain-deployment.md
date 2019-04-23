---
title: Configurar directivas de grupo para la implementación de un dominio
description: Obtenga información sobre cómo configurar las directivas de grupo en MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 13e5fa90-d330-4155-a6b8-78eb650cbbfa
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: f661fbdc40fd7dd2562d51756bc7642c8e9a4a82
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888046"
---
# <a name="configure-group-policies-for-a-domain-deployment"></a>Configurar directivas de grupo para la implementación de un dominio
Para asegurarse de que la implementación de dominio de MultiPoint Services funciona correctamente, se aplican las siguientes opciones de directiva de grupo para la cuenta de usuario WMSshell en un sistema MultiPoint Services.  
  
> [!IMPORTANT]  
> Algunas configuraciones de directiva de grupo pueden evitar que los valores de configuración necesarios que se apliquen a MultiPoint Services. Asegúrese de que entiende y definir la configuración de directiva de grupo para que funcionen correctamente en MultiPoint Services. Por ejemplo, una configuración de directiva de grupo que impide el inicio de sesión automático podría presentar problemas con el comportamiento de inicio de sesión de MultiPoint Services.  
  
## <a name="update-group-policies-for-the-wmsshell-user-account"></a>Actualizar las directivas de grupo para la cuenta de usuario WMSshell 
La cuenta de usuario WMSshell es una cuenta de sistema que MultiPoint services usa para iniciar sesión en la consola donde se crean las estaciones actuall. Esta cuenta no es entario administrarse mediante MultiPoint Manager.
  
> [!NOTE]  
> Para averiguar cómo actualizar las directivas de grupo, consulte [Editor de directivas de grupo Local](https://technet.microsoft.com/library/dn265982.aspx).  
  
**DIRECTIVA:** Configuración de usuario > plantillas administrativas > Panel de Control > **personalización**  
  
Asignar los valores siguientes:  
  
|Parámetro|Valores|  
|-----------|----------|  
|Habilitar protector de pantalla|Deshabilitada|  
|Tiempo de espera del protector de pantalla|Deshabilitada<br /><br />Segundos: xxx|  
|Proteger el protector de pantalla mediante contraseña|Deshabilitada|  
  
**DIRECTIVA:** Configuración del equipo > configuración de Windows > configuración de seguridad > directivas locales > asignación de derechos de usuario > **Permitir inicio de sesión localmente**  
  
|Parámetro|Valores|  
|-----------|----------|  
|Permitir el inicio de sesión local|Asegúrese de que la lista de cuentas incluye la cuenta WMSshell.<br /><br />**Nota:** De forma predeterminada, la cuenta de WMSshell es un miembro del grupo de usuarios. Si el grupo de usuarios está en la lista y WMSshell es un miembro del grupo de usuarios, no es necesario agregar la cuenta de WMSshell a la lista.|  
  
> [!IMPORTANT]  
> Cuando se establece ninguna directiva de grupo, asegúrese de que las directivas no interfieren con las actualizaciones automáticas y error de Windows error reporting en el servidor MultiPoint. Estos se establecen mediante el **instalar actualizaciones automáticamente** y **automática Windows Error Reporting** configuración que se seleccionó durante la instalación de Windows MultiPoint Server, configurado en MultiPoint Mediante el administrador **editar la configuración del servidor**, o configuradas en las actualizaciones programadas para la protección de disco.  
  
## <a name="update-the-registry"></a>Actualizar el registro  
Para una implementación de dominio de MultiPoint Services, debe actualizar las siguientes subclaves del registro.  
  
> [!IMPORTANT]  
> La edición incorrecta del Registro puede dañar gravemente el sistema. Antes de realizar cambios en el Registro, debe hacer una copia de seguridad de los datos de valor guardados en el equipo.  
  
#### <a name="to-update-registry-subkeys-for-a-domain-deployment-of-multipoint-services"></a>Para actualizar las subclaves del registro para la implementación de un dominio de MultiPoint Services  
  
1.  Abra el editor del registro. (En un símbolo del sistema, escriba **regedit.exe**, y presione ENTRAR.)  
  
2.  En el panel izquierdo, busque y, a continuación, seleccione la siguiente subclave del registro:  
  
    HKEY_USERS\<SIDofWMSshell>\Software\Policies\Microsoft\Windows\Control Panel\Desktop  
  
    donde '<SIDofWMSshell>' es el identificador de seguridad (SID) para la cuenta WMSshell. Para averiguar cómo identificar el SID, consulte [cómo asociar un nombre de usuario con un identificador de seguridad (SID)](https://support.microsoft.com/kb/154599).  
  
3.  En la lista de la derecha, actualice las siguientes subclaves.  
  
    |Subclave|Nombre de valor|Datos de valor|  
    |----------|--------------|--------------|  
    |ScreenSaveActive|REG_SZ|0 (cero)|  
    |ScreenSaveTimeout|REG_SZ|120|  
    |ScreenSaverIsSecure|REG_SZ|0 (cero)|  
  
    Para actualizar una subclave del registro:  
  
    1.  Con la clave del registro seleccionada en el panel izquierdo, haga clic en la subclave en el panel derecho y, a continuación, haga clic en **modificar**.  
  
    2.  En el cuadro de diálogo Editar cadena, escriba un nuevo valor en **datos del valor**y, a continuación, haga clic en **Aceptar**.  
  
4.  Cuando termine de actualizar las subclaves del registro, reinicie el equipo para activar los cambios. 