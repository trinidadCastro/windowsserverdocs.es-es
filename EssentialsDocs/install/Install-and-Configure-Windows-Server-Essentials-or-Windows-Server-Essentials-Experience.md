---
title: Instalación y configuración de Windows Server Essentials o Experiencia con Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 48ea6cd4-3955-4aaf-9236-2515a6c3e730
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: b0323c8ce2ac69ade9adeca7e948c728e4791f09
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80311659"
---
# <a name="install-and-configure-windows-server-essentials-or-windows-server-essentials-experience"></a>Instalación y configuración de Windows Server Essentials o Experiencia con Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Windows Server Essentials es un primer servidor ideal para pequeñas empresas con hasta 25 usuarios y 50 dispositivos. En el caso de las organizaciones con hasta 100 usuarios y 200 dispositivos, ahora puede usar Windows Server 2012 R2 con el rol de experiencia con Windows Server Essentials instalado. En este tema se describen dos escenarios.  
  
La experiencia con Windows Server Essentials es un rol de Windows Server 2016 que le permite sacar partido de todas las características (como el acceso Web remoto y la copia de seguridad del equipo) que están disponibles en Windows Server Essentials sin los bloqueos y límites que se aplican en  Windows Server Essentials. Este rol de servidor también está disponible en Windows Server Essentials y está habilitado de forma predeterminada.
  
Antes de instalar Windows Server Essentials o el rol de experiencia de Essentials, tenga en cuenta las siguientes limitaciones.  
  
|Experiencia con Windows Server Essentials en Windows Server Essentials|Experiencia con Windows Server Essentials en Windows Server 2016
|----|----|
|-Debe ser el controlador de dominio en la raíz del bosque y el dominio, y debe contener todos los roles FSMO.<br /><br /> -No se puede instalar en un entorno con un dominio de Active Directory existente (sin embargo, hay un período de gracia de 21 días para realizar migraciones).|-No tiene que ser un controlador de dominio si está instalado en un entorno con un dominio de Active Directory existente.<br /><br /> -Si no existe un dominio de Active Directory, al instalar el rol se creará un dominio Active Directory y el servidor se convertirá en el controlador de dominio en la raíz del bosque y el dominio, y contendrá todos los roles FSMO.  
|Solo se puede implementar en un único dominio.|Solo se puede implementar en un único dominio.  
|No puede existir un controlador de dominio de solo lectura en el dominio.|No puede existir un controlador de dominio de solo lectura en el dominio.

> [!NOTE]
>  Para descargar versiones de evaluación de los sistemas operativos, visite el centro de evaluación de TechNet:  
>   
>  [Descargar Windows Server 2016](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016)  
>   
>  [Descargar Windows Server Essentials](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016-essentials)
  
## <a name="installation-options"></a>Opciones de instalación  
 Este documento proporciona instrucciones paso a paso para instalar y configurar Windows Server Essentials. En función de su entorno de red, dispone de las siguientes opciones de instalación:  
  
-    Windows Server Essentials (con el rol experiencia con Windows Server Essentials habilitado de forma predeterminada)  
  
-    Windows Server 2016 con el rol de experiencia con Windows Server Essentials instalado  
 
|Entorno de implementación|Descripción|Sección relacionada|  
|----------------------------|-----------------|---------------------|  
|Nuevo entorno de Active Directory|Puede instalar Windows Server Essentials para crear un entorno de Active Directory.|[Implementar Windows Server Essentials para configurar un nuevo entorno de Active Directory](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_NewAD)|  
|Entorno de Active Directory existente|Puede instalar Windows Server Essentials en un entorno de Active Directory existente.|[Implementar Windows Server Essentials en un entorno de Active Directory existente](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_ExistingAD)|  
|Entorno virtual|Puede implementar Windows Server Essentials como una máquina virtual.|[Virtualización del entorno](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_VirtualWSE)|  
|Implementación automatizada|Puede automatizar la implementación de Windows Server Essentials mediante Windows PowerShell.|[Instalar y configurar Windows Server Essentials mediante Windows PowerShell](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_PowerShell)|  
  
## <a name="before-you-begin"></a>Antes de comenzar  
 Antes de comenzar la instalación, revise la siguiente documentación:  
  
-   [Información general del producto de Windows Server Essentials](https://www.microsoft.com/server-cloud/windows-server-essentials/windows-server-2012-r2-essentials.aspx)  
  

-   [Requisitos del sistema para Windows Server Essentials](../get-started/system-requirements.md)   

  
##  <a name="deploy-windows-server-essentials-to-set-up-a-new-active-directory-environment"></a><a name="BKMK_NewAD"></a>Implementar Windows Server Essentials para configurar un nuevo entorno de Active Directory  
 Windows Server Essentials permite configurar rápidamente un entorno de Active Directory y las características de servidor relacionadas.  
  
###  <a name="deploying-windows-server-essentials"></a><a name="BKMK_WSEDeploy"></a>Implementar Windows Server Essentials  
 Si usa Windows Server Essentials, la experiencia con Windows Server Essentials ya está habilitada. Sin embargo, debe completar algunos pasos para configurar el servidor.  
  
##### <a name="to-configure-windows-server-essentials-on-a-physical-server"></a>Para configurar Windows Server Essentials en un servidor físico  
  
1. Después de la página de **bienvenida** de Windows, el **asistente para configurar Windows Server Essentials** está visible en el escritorio.  
  
2. Siga las instrucciones siguientes para completar el asistente:  
  
   1.  En la página **Configurar Windows Server Essentials**, haga clic en **Siguiente**.  
  
   2.  En **Configuración de tiempo**, asegúrese de que la fecha, hora y zona horaria sean correctas y, a continuación, haga clic en **Siguiente**.  
  
   3.  En **Información de la compañía**, escriba el nombre de su compañía, por ejemplo, **Contoso,Ltd.** y, a continuación, haga clic en **Siguiente**. Si quiere, puede cambiar el nombre de dominio interno y el nombre del servidor.  
  
   4.  En **Crear cuenta de administrador de red**, escriba un nuevo nombre de cuenta de administrador y una contraseña.  
  
       > [!NOTE]
       >  No use el nombre predeterminado **Administrador** como nombre de cuenta y contraseña.  
  
   5.  Haga clic en **Configurar**.  
  
3. El servidor se reiniciará varias veces durante la configuración, y los inicios de sesión serán automáticos hasta que finalice la configuración. Este proceso tarda aproximadamente 20 minutos.  
  
4. En el escritorio, haga clic en el icono de escritorio para iniciar el panel del servidor. En la página **Inicio**, complete las tareas de **Introducción** en la pestaña **Instalación**.  
  
   Cuando se haya configurado el servidor, el servidor que ejecuta Windows Server Essentials se establecerá como un controlador de dominio.  
  
###  <a name="deploying-the-windows-server-essentials-experience-role-in-windows-server-2012-r2-standard-and-datacenter"></a><a name="BKMK_DeployWSERole"></a>Implementar el rol de experiencia con Windows Server Essentials en Windows Server 2012 R2 Standard y Datacenter  
 Puede usar Administrador del servidor para habilitar y configurar el rol de experiencia con Windows Server Essentials en Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter mediante el procedimiento siguiente.  
  
##### <a name="to-deploy-the-windows-server-essentials-experience-role-in-windows-server-2012-r2"></a>Para implementar el rol de Experiencia con Windows Server Essentials en Windows Server 2012 R2  
  
1.  Inicie sesión como administrador local en el servidor.  
  
2.  Abra el **Administrador del servidor** y, a continuación, haga clic en **Agregar roles y características**.  
  
3.  En **Seleccionar roles de servidor**, seleccione el rol **Experiencia con Windows Server Essentials**. En el cuadro de diálogo, haga clic en **Agregar características** y, a continuación, haga clic en **Siguiente**.  
  
4.  En **Características**, haga clic en **Siguiente**.  
  
5.  Revise la descripción del rol de **Experiencia con Windows Server Essentials** y, después, haga clic en **Siguiente**.  
  
6.  En las páginas siguientes, haga clic en **Siguiente** y, a continuación, en la página de confirmación, haga clic en **Instalar**.  
  
7.  Una vez completada la instalación, la experiencia con Windows Server Essentials debe aparecer como un rol de servidor en Administrador del servidor.  
  
8.  En el área de notificación de marca del Administrador del servidor, haga clic en la marca y, a continuación, en **Configurar Windows Server Essentials**.  
  
9. (Opcional) Cambie el nombre del servidor, si fuera necesario.  
  
    > [!IMPORTANT]
    >  No puede cambiar el nombre del servidor después de configurar Windows Server Essentials.  
  

10. Siga el Asistente para configurar Windows Server Essentials como se describió anteriormente en la sección [implementación de Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_WSEDeploy) .  

10. Siga el Asistente para configurar Windows Server Essentials como se describió anteriormente en la sección [implementación de Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_WSEDeploy) .  

  
##  <a name="deploy-windows-server-essentials-in-an-existing-active-directory-environment"></a><a name="BKMK_ExistingAD"></a>Implementar Windows Server Essentials en un entorno de Active Directory existente  
 También puede implementar Windows Server Essentials si su organización ya tiene un entorno de Active Directory existente. Además, puede implementar Windows Server Essentials como un controlador de dominio.  
  
> [!IMPORTANT]
>  Esta opción solo está disponible si implementa el rol de experiencia con Windows Server Essentials en Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter.  
  
#### <a name="to-deploy-windows-server-essentials-in-an-existing-active-directory-environment"></a>Para implementar Windows Server Essentials en un entorno de Active Directory existente  
  
1.  (Opcional) Cambie el nombre del servidor, si fuera necesario.  
  
    > [!IMPORTANT]
    >  No puede cambiar el nombre del servidor después de configurar Windows Server Essentials.  
  
2.  Unir el servidor que ejecuta Windows Server Essentials a un dominio existente de la siguiente manera:  
  
    1.  Si desea que este servidor sea un controlador de dominio, configure el servidor como una réplica de controlador de dominio.  
  
    2.  Si no desea que este servidor sea un controlador de dominio, debe unir este servidor al dominio mediante las herramientas nativas de Windows.  
  
3.  Reinicie el servidor e inicie sesión en el servidor como administrador del dominio.  
  
4.  Abra el Administrador del servidor y, a continuación, haga clic en **Agregar roles y características**.  
  
5.  En las páginas siguientes, haga clic en **Siguiente**.  
  
6.  En **Seleccionar roles de servidor**, seleccione **Experiencia con Windows Server Essentials**. En el cuadro de diálogo, haga clic en **Agregar características** y, a continuación, haga clic en **Siguiente**.  
  
7.  En **Características**, haga clic en **Siguiente**.  
  
8.  Revise la descripción de **Experiencia con Windows Server Essentials** y, después, haga clic en **Siguiente**.  
  
9. En las páginas siguientes, haga clic en **Siguiente** y, a continuación, en la página de confirmación, haga clic en **Instalar**.  
  
10. Una vez completada la instalación, la experiencia con Windows Server Essentials se mostrará como un rol de servidor en Administrador del servidor.  
  
11. En el área de notificación de marca del **Administrador del servidor**, haga clic en la marca y, a continuación, en **Configurar Windows Server Essentials**.  
  
12. Siga el asistente para configurar Windows Server Essentials. En función de la configuración de Active Directory que tenga, se le informará si va a configurar Windows Server Essentials en un controlador de dominio o como miembro del dominio. Haga clic en **Configurar** para empezar la configuración. El proceso de configuración tarda aproximadamente 10 minutos en completarse.  
  
##  <a name="virtualize-your-environment"></a><a name="BKMK_VirtualWSE"></a>Virtualización del entorno  
  Windows Server Essentials, Windows Server 2012 R2 Standard y Windows Server 2012 R2 Datacenter se pueden ejecutar como máquinas virtuales. Las máquinas virtuales se ejecutan mediante herramientas de administración de Hyper-V en un servidor que ejecuta Hyper-V. Desde el punto de vista de las licencias, Windows Server Essentials le permite configurar el rol de Hyper-V y virtualizar su entorno. La licencia le permite configurar otro sistema operativo invitado que ejecuta Windows Server Essentials. En función de la configuración de ¢ s del proveedor del sistema, Windows Server Essentials le permite configurar un entorno virtualizado sin problemas.  
  
#### <a name="to-deploy-windows-server-essentials-as-a-virtual-machine"></a>Para implementar Windows Server Essentials como una máquina virtual  
  
1.  Después de la página de bienvenida de Windows (según el proveedor del sistema "configuración de ¢ s"), la página **antes de comenzar** ofrece la opción de configurar Windows Server Essentials como una instancia virtual o en hardware físico. La disponibilidad de estas opciones depende del proveedor del sistema y no siempre están disponibles. Para instalar Windows Server Essentials como una máquina virtual, en **instalar Windows Server Essentials**, seleccione **instalar como instancia virtual**y, a continuación, haga clic en **configurar**.  
  
2.  El asistente ofrece una máquina virtual que tarda aproximadamente cinco minutos.  
  

3.  Después, configure Windows Server Essentials como se describió anteriormente en la sección [implementación de Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_WSEDeploy) .  

3.  Después, configure Windows Server Essentials como se describió anteriormente en la sección [implementación de Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_WSEDeploy) .  

  
##  <a name="install-and-configure-windows-server-essentials-by-using-windows-powershell"></a><a name="BKMK_PowerShell"></a>Instalar y configurar Windows Server Essentials mediante Windows PowerShell  
 Puede automatizar la instalación de Windows Server Essentials mediante cmdlets de Windows PowerShell.  
  
#### <a name="to-install-windows-server-essentials-by-using-windows-powershell"></a>Para instalar Windows Server Essentials mediante Windows PowerShell  
  
1.  Abra la consola de Windows PowerShell desde un símbolo del sistema con privilegios elevados.  
  
2.  Instale el rol experiencia con Windows Server Essentials mediante el comando siguiente:  
  
    ```  
    Add-WindowsFeature ServerEssentialsRole  
    ```  
  
3.  Ejecute `Get-Help Start-WssConfigurationService`.  
  
    1.  Ejecute el siguiente comando para iniciar la configuración para configurar Windows Server Essentials como un controlador de dominio:  
  
        ```  
        Start-WssConfigurationService -CompanyName "ContosoTest" -DNSName "ContosoTest.com" -NetBiosName "ContosoTest" -ComputerName "YourServerName  œNewAdminCredential $cred  
        ```  
  
    2.  Ejecute el siguiente comando para iniciar la configuración para configurar Windows Server Essentials como un miembro de dominio existente. Debe ser miembro del grupo de administradores de la empresa y del grupo de administradores de dominio de Active Directory para realizar esta tarea:  
  
        ```  
        Start-WssConfigurationService  œCredential <Your Credential>  
  
        ```  
  
4.  Supervise el progreso de la instalación mediante los siguientes comandos:  
  
    -   Para que el estado de la instalación aparezca en la barra de progreso, ejecute `Get-WssConfigurationStatus  œShowProgress`.  
  
    -   Para obtener un progreso inmediato sin la barra de progreso, ejecute `Get-WssConfigurationStatus`.  
  
## <a name="see-also"></a>Vea también  
  
-   [Novedades de Windows Server Essentials](../get-started/what-s-new.md)  
  
-   [Instalación de Windows Server 2012 Essentials](Install-Windows-Server-Essentials.md)  
  
-   [Introducción a Windows Server Essentials](../get-started/get-started.md)
