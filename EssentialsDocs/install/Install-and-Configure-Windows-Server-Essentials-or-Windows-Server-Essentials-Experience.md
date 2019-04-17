---
title: Instalar y configurar Windows Server Essentials o Windows Server Essentials Experience
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 48ea6cd4-3955-4aaf-9236-2515a6c3e730
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 8a2310b178663c6ca32a4e07d11656f1aaf2a11b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="install-and-configure-windows-server-essentials-or-windows-server-essentials-experience"></a>Instalar y configurar Windows Server Essentials o Windows Server Essentials Experience

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Windows Server Essentials es un servidor primera ideal para pequeñas empresas con hasta 25 usuarios y dispositivos de 50. Para las organizaciones con hasta 100 usuarios y dispositivos de 200, ahora puedes usar Windows Server 2012 R2 con el rol de Windows Server Essentials Experience instalado. Este tema trata ambos casos.  
  
Windows Server Essentials Experience es un rol de Windows Server 2016 que te permite sacar partido de todas las características (por ejemplo, copia de seguridad de acceso Web remoto y el PC) que están disponibles en Windows Server Essentials sin los bloqueos y los límites que se aplican en Windows Server Essentials. Este rol de servidor también está disponible en Windows Server Essentials y está habilitada de manera predeterminada.
  
Antes de instalar Windows Server Essentials o el rol de experiencia de Essentials, ten en cuenta las siguientes limitaciones.  
  
|Experiencia de Windows Server Essentials en Windows Server Essentials|Experiencia de Windows Server Essentials en Windows Server 2016
|----|----|
|-Debe ser el controlador de dominio en la raíz del bosque y dominio y debe contener todas las funciones FSMO.<br /><br /> -No puede instalarse en un entorno con un dominio de Active Directory existente (sin embargo, hay un período de gracia de 21 días para llevar a cabo migraciones).|-No tiene que ser un controlador de dominio si está instalado en un entorno con un dominio de Active Directory existente.<br /><br /> -Si no existe un dominio de Active Directory, instalar el rol, se creará un dominio de Active Directory, y el servidor se convertirá en el controlador de dominio en la raíz del bosque y dominio, manteniendo las funciones FSMO.  
|Solo se puede implementar en un solo dominio.|Solo se puede implementar en un solo dominio.  
|Un controlador de dominio de solo lectura no puede existir en el dominio.|Un controlador de dominio de solo lectura no puede existir en el dominio.

> [!NOTE]
>  Para descargar versiones de evaluación de los sistemas operativos, visita el centro de evaluación de TechNet:  
>   
>  [Descargar Windows Server 2016](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016)  
>   
>  [Descargar Windows Server Essentials](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016-essentials)
  
## <a name="installation-options"></a>Opciones de instalación  
 Este documento proporciona instrucciones paso a paso para instalar y configurar Windows Server Essentials. Según el entorno de red, tienes las siguientes opciones de instalación disponibles:  
  
-    Windows Server Essentials (con el rol de Windows Server Essentials Experience habilitado de manera predeterminada)  
  
-    Windows Server 2016 con el rol de Windows Server Essentials Experience instalado  
 
|Entorno de implementación|Descripción|Sección relacionada|  
|----------------------------|-----------------|---------------------|  
|Nuevo entorno de Active Directory|Puedes instalar Windows Server Essentials para crear un nuevo entorno de Active Directory.|[Implementar Windows Server Essentials para configurar un entorno de Active Directory](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_NewAD)|  
|Entorno de Active Directory|Puedes instalar Windows Server Essentials en un entorno de Active Directory existente.|[Implementar Windows Server Essentials en un entorno de Active Directory existente](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_ExistingAD)|  
|Entorno virtual|Puedes implementar Windows Server Essentials como máquina virtual.|[Virtualizar el entorno](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_VirtualWSE)|  
|Implementación automatizada|Puede automatizar la implementación de Windows Server Essentials mediante Windows PowerShell.|[Instalar y configurar Windows Server Essentials mediante Windows PowerShell](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_PowerShell)|  
  
## <a name="before-you-begin"></a>Antes de comenzar  
 Antes de comenzar la instalación, revisa la siguiente documentación:  
  
-   [Información general de producto de Windows Server Essentials](https://www.microsoft.com/server-cloud/windows-server-essentials/windows-server-2012-r2-essentials.aspx)  
  

-   [Requisitos del sistema de Windows Server Essentials](../get-started/system-requirements.md)   

  
##  <a name="BKMK_NewAD"></a>Implementar Windows Server Essentials para configurar un entorno de Active Directory  
 Windows Server Essentials ofrece una forma para que puedas configurar rápidamente un entorno de Active Directory y características de servidor relacionado.  
  
###  <a name="BKMK_WSEDeploy"></a>Implementar Windows Server Essentials  
 Si estás usando Windows Server Essentials, Windows Server Essentials Experience ya está habilitado. Sin embargo, debes completar algunos pasos para configurar el servidor.  
  
##### <a name="to-configure-windows-server-essentials-on-a-physical-server"></a>Para configurar Windows Server Essentials en un servidor físico  
  
1.  Después de las ventanas **bienvenida** página, la **configurar Windows Server Essentials Wizard** es visible en el escritorio.  
  
2.  Sigue las instrucciones para completar al Asistente para la siguiente manera:  
  
    1.  En la **configurar Windows Server Essentials** página, haz clic en **siguiente**.  
  
    2.  En **configuración de tiempo**, asegúrate de que la fecha, hora y zona horaria sean correctas y, a continuación, haz clic en **siguiente**.  
  
    3.  En **información de la empresa**, escribe el nombre de tu empresa, como **Contoso, Ltd.**y, a continuación, haz clic en **siguiente**. Opcionalmente, puedes cambiar el nombre de dominio interno y el nombre del servidor.  
  
    4.  En **crear administrador de red**, escribe un nuevo nombre de cuenta de administrador y una contraseña.  
  
        > [!NOTE]
        >  No uses el valor predeterminado **administrador** nombre de cuenta y contraseña.  
  
    5.  Haz clic en **configurar**.  
  
3.  El servidor se reiniciará varias veces durante el proceso de configuración y los inicios de sesión será automática hasta que finalice la configuración. Este proceso tarda unos 20 minutos.  
  
4.  En el escritorio, haz clic en el icono de escritorio para iniciar el panel del servidor. En la **Home** página, completa la **Introducción** tareas que se enumeran en la **instalación** pestaña.  
  
 Después de completar la configuración del servidor, el servidor que ejecuta Windows Server Essentials se configurará como un controlador de dominio.  
  
###  <a name="BKMK_DeployWSERole"></a>Implementar el rol de experiencia con Windows Server Essentials en Windows Server 2012 R2 Standard y Datacenter  
 Puedes usar el administrador del servidor para habilitar y configurar el rol de experiencia con Windows Server Essentials en Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter con el siguiente procedimiento.  
  
##### <a name="to-deploy-the-windows-server-essentials-experience-role-in-windows-server-2012-r2"></a>Para implementar el rol de experiencia con Windows Server Essentials en Windows Server 2012 R2  
  
1.  Inicia sesión en el servidor como un administrador local.  
  
2.  Abre **administrador del servidor**y, a continuación, haz clic en **agregar Roles y características**.  
  
3.  En **seleccionar roles de servidor**, selecciona el **Windows Server Essentials Experience** rol. En el cuadro de diálogo, haz clic en **agregar características**y, a continuación, haz clic en **siguiente**.  
  
4.  En **características**, haz clic en **siguiente**.  
  
5.  Revisa el **Windows Server Essentials Experience** descripción de la función y, a continuación, haz clic en **siguiente**.  
  
6.  En las páginas que sigue, haz clic en **siguiente**y, a continuación, en la página de confirmación, haz clic en **instalar**.  
  
7.  Una vez completada la instalación, Windows Server Essentials Experience debe aparecer como un rol de servidor en el administrador del servidor.  
  
8.  En el área de notificación de marca en el administrador del servidor, haz clic en el marcador y, a continuación, haz clic en **configurar Windows Server Essentials**.  
  
9. (Opcional) Si es necesario, cambiar el nombre del servidor.  
  
    > [!IMPORTANT]
    >  No puedes cambiar el nombre del servidor después de haber configurado Windows Server Essentials.  
  

10. Sigue el Asistente para configurar Windows Server Essentials, como se describió en el [implementar Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_WSEDeploy) sección.  

10. Sigue el Asistente para configurar Windows Server Essentials, como se describió en el [implementar Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_WSEDeploy) sección.  

  
##  <a name="BKMK_ExistingAD"></a>Implementar Windows Server Essentials en un entorno de Active Directory existente  
 También puedes implementar Windows Server Essentials si la organización ya tiene un entorno de Active Directory existente. Además, puedes elegir si quieres implementar Windows Server Essentials como un controlador de dominio.  
  
> [!IMPORTANT]
>  Esta opción solo está disponible si implementas el rol de experiencia con Windows Server Essentials en Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter.  
  
#### <a name="to-deploy-windows-server-essentials-in-an-existing-active-directory-environment"></a>Para implementar Windows Server Essentials en un entorno de Active Directory existente  
  
1.  (Opcional) Si es necesario, cambiar el nombre del servidor.  
  
    > [!IMPORTANT]
    >  No puedes cambiar el nombre del servidor después de haber configurado Windows Server Essentials.  
  
2.  Unir el servidor que ejecuta Windows Server Essentials a un dominio existente de la siguiente manera:  
  
    1.  Si quieres que este servidor sea un controlador de dominio, configurar el servidor como un controlador de dominio de réplica.  
  
    2.  Si no desea este servidor sea un controlador de dominio, unirte a este servidor a tu dominio con las herramientas de Windows nativas.  
  
3.  Reiniciar el servidor e iniciar sesión en el servidor como un administrador de dominio.  
  
4.  Abre el administrador del servidor y, a continuación, haz clic en **agregar Roles y características**.  
  
5.  En las páginas que sigue, haz clic en **siguiente**.  
  
6.  En **seleccionar Roles de servidor**, selecciona **Windows Server Essentials Experience**. En el cuadro de diálogo, haz clic en **agregar características**y, a continuación, haz clic en **siguiente**.  
  
7.  En **características**, haz clic en **siguiente**.  
  
8.  Revisa el **Windows Server Essentials Experience** descripción y, a continuación, haz clic en **siguiente**.  
  
9. En las páginas que sigue, haz clic en **siguiente**y, a continuación, en la página de confirmación, haz clic en **instalar**.  
  
10. Una vez completada la instalación, Windows Server Essentials Experience se mostrará como un rol de servidor en el administrador del servidor.  
  
11. En el área de notificación de marca en **administrador del servidor**, haz clic en el marcador y, a continuación, haz clic en **configurar Windows Server Essentials**.  
  
12. Sigue el Asistente para configurar Windows Server Essentials. Según la configuración de Active Directory, se le informará si va a configurar Windows Server Essentials en un controlador de dominio o como un miembro de dominio. Haz clic en **configurar** para comenzar la configuración. El proceso de configuración tarda aproximadamente 10 minutos en completarse.  
  
##  <a name="BKMK_VirtualWSE"></a>Virtualizar el entorno  
  Windows Server Essentials, Windows Server 2012 R2 Standard y Windows Server 2012 R2 Datacenter se pueden ejecutar como máquinas virtuales. Ejecutar máquinas virtuales con las herramientas de administración de Hyper-V en un servidor que ejecuta Hyper-V. Desde una perspectiva de licencias, Windows Server Essentials te permite configurar el rol de Hyper-V y virtualizar su entorno. La licencia le permite configurar otro sistema operativo invitado que ejecuta Windows Server Essentials. Según el proveedor del sistema "configuración de s ¢, Windows Server Essentials te permite configurar un entorno virtualizado sin problemas.  
  
#### <a name="to-deploy-windows-server-essentials-as-a-virtual-machine"></a>Para implementar Windows Server Essentials como máquina virtual  
  
1.  Después de la página de bienvenida de Windows (en función de tu proveedor "¢ s configuración del sistema), el **antes de comenzar** página proporciona una opción para configurar Windows Server Essentials como una instancia virtual o en hardware físico. La disponibilidad de estas opciones se predefinidos por el proveedor del sistema y no siempre esté disponibles ambas opciones. Para instalar Windows Server Essentials como máquina virtual, en **instalar Windows Server Essentials**, selecciona **instalar como instancia virtual**y, a continuación, haz clic en **configurar**.  
  
2.  El asistente proporcionará una máquina virtual que se tarda unos cinco minutos.  
  

3.  A continuación, configura Windows Server Essentials, como se describió en el [implementar Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_WSEDeploy) sección.  

3.  A continuación, configura Windows Server Essentials, como se describió en el [implementar Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_WSEDeploy) sección.  

  
##  <a name="BKMK_PowerShell"></a>Instalar y configurar Windows Server Essentials mediante Windows PowerShell  
 Puede automatizar la instalación de Windows Server Essentials mediante los cmdlets de Windows PowerShell.  
  
#### <a name="to-install-windows-server-essentials-by-using-windows-powershell"></a>Para instalar Windows Server Essentials mediante Windows PowerShell  
  
1.  Abre la consola de Windows PowerShell desde un símbolo del sistema con privilegios elevados.  
  
2.  Instalar el rol de Windows Server Essentials Experience mediante el siguiente comando:  
  
    ```  
    Add-WindowsFeature ServerEssentialsRole  
    ```  
  
3.  Ejecutar `Get-Help Start-WssConfigurationService`.  
  
    1.  Ejecuta el siguiente comando para iniciar la configuración para configurar Windows Server Essentials como un controlador de dominio:  
  
        ```  
        Start-WssConfigurationService -CompanyName "ContosoTest" -DNSName "ContosoTest.com" -NetBiosName "ContosoTest" -ComputerName "YourServerName  œNewAdminCredential $cred  
        ```  
  
    2.  Ejecuta el siguiente comando para iniciar la configuración para configurar Windows Server Essentials como un miembro de dominio existente. Debe ser miembro del grupo de administradores de empresa y grupo de administradores de dominio de Active Directory para realizar esta tarea:  
  
        ```  
        Start-WssConfigurationService  œCredential <Your Credential>  
  
        ```  
  
4.  Supervisar el progreso de la instalación mediante los siguientes comandos:  
  
    -   Para que el estado de la instalación en la barra de progreso, ejecute `Get-WssConfigurationStatus  œShowProgress`.  
  
    -   Para obtener el progreso inmediato sin la barra de progreso, ejecuta `Get-WssConfigurationStatus`.  
  
## <a name="see-also"></a>Consulta también  
  
-   [Novedades en Windows Server Essentials](../get-started/what-s-new.md)  
  
-   [Instalar Windows Server Essentials](Install-Windows-Server-Essentials.md)  
  
-   [Introducción a Windows Server Essentials](../get-started/get-started.md)
