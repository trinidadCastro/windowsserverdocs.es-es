---
title: Configurar directivas de grupo para la implementación de un dominio
description: Obtenga información sobre cómo configurar directivas de grupo en Multipoint Services
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
ms.openlocfilehash: 5c9d8efc1ed4a2f498ffce6c69d443ae819dced9
ms.sourcegitcommit: 1bc3c229e9688ac741838005ec4b88e8f9533e8a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2019
ms.locfileid: "68314319"
---
# <a name="configure-group-policies-for-a-domain-deployment"></a>Configurar directivas de grupo para la implementación de un dominio
Para asegurarse de que la implementación de su dominio de Multipoint Services funciona correctamente, aplique la siguiente configuración de directiva de grupo a la cuenta de usuario WMSshell en un sistema Multipoint Services.  
  
> [!IMPORTANT]  
> Algunos valores de configuración de directiva de grupo pueden impedir que se apliquen las opciones de configuración necesarias a multipoint Services. Asegúrese de que comprende y define la configuración de directiva de grupo para que funcione correctamente en Multipoint Services. Por ejemplo, un valor directiva de grupo que impide que el inicio de sesión automático presente problemas con el comportamiento de inicio de sesión de Multipoint Services.  
  
## <a name="update-group-policies-for-the-wmsshell-user-account"></a>Actualizar directivas de grupo para la cuenta de usuario de WMSshell 
La cuenta de usuario de WMSshell es una cuenta del sistema que Multipoint Services usa para iniciar sesión en la consola, donde se crean las estaciones reales. Esta cuenta no está pensada para ser administrada por Multipoint Manager.
  
> [!NOTE]  
> Para obtener información sobre cómo actualizar las directivas de grupo, consulte [Editor de directivas de grupo local](https://technet.microsoft.com/library/dn265982.aspx).  
  
**DIRECTIVAS** Configuración de usuario > Plantillas administrativas > panel de  control > Personalización  
  
Asigne los valores siguientes:  
  
|Parámetro|Valores|  
|-----------|----------|  
|Habilitar protector de pantalla|Disabled|  
|Tiempo de espera del protector de pantalla|Disabled<br /><br />Segundos: XXX|  
|Proteger el protector de pantalla mediante contraseña|Disabled|  
  
**DIRECTIVAS** Configuración del equipo > configuración de Windows > configuración de seguridad > directivas locales > asignación **de derechos de usuario > permitir el inicio de sesión local**  
  
|Parámetro|Valores|  
|-----------|----------|  
|Permitir el inicio de sesión local|Asegúrese de que la lista de cuentas incluye la cuenta WMSshell.<br /><br />**Nota:** De forma predeterminada, la cuenta WMSshell es un miembro del grupo usuarios. Si el grupo de usuarios está en la lista y WMSshell es un miembro del grupo de usuarios, no es necesario que agregue la cuenta de WMSshell a la lista.|  
  
> [!IMPORTANT]  
> Cuando establezca directivas de grupo, asegúrese de que las directivas no interfieren con las actualizaciones automáticas y los informes de errores de Windows en el servidor multipoint. Se establecen mediante las opciones **instalar actualizaciones automáticamente** y **automática informe de errores de Windows** que se seleccionaron durante la instalación de Windows MultiPoint Server, configuradas en Multipoint Manager mediante **Editar configuración del servidor**, o bien configurado en actualizaciones programadas para protección de disco.  
  
## <a name="update-the-registry"></a>Actualizar el registro  
Para una implementación de dominio de Multipoint Services, debe actualizar las siguientes subclaves del registro.  
  
> [!IMPORTANT]  
> La edición incorrecta del Registro puede dañar gravemente el sistema. Antes de realizar cambios en el Registro, debe hacer una copia de seguridad de los datos de valor guardados en el equipo.  
  
#### <a name="to-update-registry-subkeys-for-a-domain-deployment-of-multipoint-services"></a>Para actualizar las subclaves del registro para una implementación de dominio de Multipoint Services  
  
1.  Abra el editor del registro. (En un símbolo del sistema, escriba **regedit. exe**y presione Entrar).  
  
2.  En el panel izquierdo, busque la siguiente subclave del registro y selecciónela:  
  
    HKEY_USERS\<SIDofWMSshell > \Software\Policies\Microsoft\Windows\Control Panel\Desktop  
  
    donde "<SIDofWMSshell>" es el identificador de seguridad (SID) de la cuenta WMSshell. Para averiguar cómo identificar el SID, consulte [cómo asociar un nombre de usuario a un identificador de seguridad (SID)](https://support.microsoft.com/kb/154599).  
  
3.  En la lista de la derecha, actualice las siguientes subclaves.  
  
    |Subclave|Nombre de valor|Datos de valor|  
    |----------|--------------|--------------|  
    |ScreenSaveActive|REG_SZ|0 (cero)|  
    |ScreenSaveTimeout|REG_SZ|120|  
    |ScreenSaverIsSecure|REG_SZ|0 (cero)|  
  
    Para actualizar una subclave del registro:  
  
    1.  Con la clave del registro seleccionada en el panel izquierdo, haga clic con el botón secundario en la subclave del panel derecho y, a continuación, haga clic en **modificar**.  
  
    2.  En el cuadro de diálogo Editar cadena, escriba un nuevo valor en **datos del valor**y, a continuación, haga clic en **Aceptar**.  
  
4.  Cuando termine de actualizar las subclaves del registro, reinicie el equipo para activar los cambios. 
