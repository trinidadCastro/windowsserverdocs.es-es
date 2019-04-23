---
title: Instalar el rol de Hyper-V en Windows Server
description: Proporciona instrucciones para instalar Hyper-V mediante el administrador del servidor o Windows PowerShell
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 8e871317-09d2-4314-a6ec-ced12b7aee89
author: KBDAzure
ms.author: kathydav
ms.date: 12/02/2016
ms.openlocfilehash: a4167065c11cf87ba761ed65884b88c5413dfdf8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870436"
---
# <a name="install-the-hyper-v-role-on-windows-server"></a>Instalar el rol de Hyper-V en Windows Server

>Se aplica a: Windows Server 2016, Windows Server 2019
  
Para crear y ejecutar máquinas virtuales, instalar el rol de Hyper-V en Windows Server con el administrador del servidor o el **Install-WindowsFeature** cmdlet de Windows PowerShell. Para Windows 10, consulte [instalar Hyper-V en Windows 10](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v).

Para obtener más información acerca de Hyper-V, vea el [Introducción a la tecnología Hyper-V](..\Hyper-V-Technology-Overview.md). Para probar Windows Server 2019, puede descargar e instalar una copia de evaluación. Consulte la [centro de evaluación](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019).

Antes de instalar a Windows Server o agregar el rol de Hyper-V, asegúrese de que:
- El hardware del equipo es compatible. Para obtener más información, consulte [requisitos del sistema para Windows Server](../../../get-started/System-Requirements.md) y [requisitos del sistema para Hyper-V en Windows Server](../System-requirements-for-Hyper-V-on-Windows.md).
- No va a usar las aplicaciones de virtualización de terceros que se basan en las mismas características de procesador que Hyper-V requiere. Algunos ejemplos son la estación de trabajo de VMWare y VirtualBox. Puede instalar Hyper-V sin desinstalar estas otras aplicaciones. Sin embargo, si intenta usarlas para administrar las máquinas virtuales cuando se ejecuta el hipervisor de Hyper-V, las máquinas virtuales podría no iniciarse o podría ejecutarse no confiable. Para obtener información detallada e instrucciones para desactivar el hipervisor de Hyper-V si necesita usar una de estas aplicaciones, consulte [no funcionan las aplicaciones de virtualización con Hyper-V, Device Guard y Credential Guard](https://support.microsoft.com/help/3204980/virtualization-applications-do-not-work-together-with-hyper-v-device-g).

Si desea instalar sólo las herramientas de administración, como el Administrador de Hyper-V, consulte [administrar hosts de Hyper-V con el Administrador de Hyper-V de forma remota](..\Manage\Remotely-manage-Hyper-V-hosts.md).
  
## <a name="BKMK_SERV"></a>Instalar Hyper-V mediante el administrador del servidor  
  
1. En el menú **Administrador del servidor** del menú **Administrar**, haz clic en **Agregar roles y características**.  
  
2. En la página **Antes de comenzar**, compruebe que el servidor de destino y el entorno de red estén preparados para el rol y la característica que desea instalar. Haz clic en **Siguiente**.  
  
3. En la página **Seleccionar tipo de instalación**, seleccione **Instalación basada en características o en roles** y, a continuación, en **Siguiente**.  
  
4. En la página **Seleccionar servidor de destino**, seleccione un servidor del grupo de servidores y haga clic en **Siguiente**.  
  
5. En la pantalla **Seleccionar roles de servidor**, seleccione **Hyper-V**.  
  
6. Para agregar las herramientas que usa para crear y administrar máquinas virtuales, haga clic en **Agregar características**. En la página Características, haga clic en **Siguiente**.  
  
7. En las páginas **Crear conmutadores virtuales**, **Migración de máquinas virtuales** y **Almacenes predeterminados**, seleccione las opciones apropiadas.  
  
8. En la página **Confirmar selecciones de instalación**, seleccione **Reiniciar automáticamente el servidor de destino en caso necesario** y haga clic en **Instalar**.  
  
9. Cuando finalice la instalación, compruebe que Hyper-V se ha instalado correctamente. Abra el **todos los servidores** página en el administrador del servidor y seleccione un servidor en el que haya instalado Hyper-V. Compruebe el **Roles y características** icono en la página del servidor seleccionado.  
  
## <a name="BKMK_PWRSH"></a>Instalar Hyper-V mediante el cmdlet Install-WindowsFeature  
  
1. En el Escritorio de Windows, haga clic en el botón Inicio y escriba cualquier parte del nombre **Windows PowerShell**.  
  
2. Haga clic en Windows PowerShell y seleccione **ejecutar como administrador**.  
  
3. Para instalar Hyper-V en un servidor que está conectado de forma remota a, ejecute el siguiente comando y reemplace `<computer_name>` con el nombre del servidor.  
  
    ```powershell
    Install-WindowsFeature -Name Hyper-V -ComputerName <computer_name> -IncludeManagementTools -Restart  
    ```  
  
    Si está conectado localmente en el servidor, ejecute el comando sin `-ComputerName <computer_name>`.  
  
4. Una vez reiniciado el servidor, puede ver que el rol Hyper-V está instalado y vea Cuáles son los otros roles y características instalados, ejecute el comando siguiente:  
  
    ```powershell
    Get-WindowsFeature -ComputerName <computer_name>  
    ```  
  
    Si está conectado localmente en el servidor, ejecute el comando sin `-ComputerName <computer_name>`.  
  
> [!NOTE]  
> Si instala este rol en un servidor que ejecuta la opción de instalación Server Core de Windows Server 2016 y use el parámetro `-IncludeManagementTools`, solo el Hyper-V módulo para Windows PowerShell está instalado. Puede usar la herramienta de administración de GUI, el Administrador de Hyper-V, en otro equipo para administrar de forma remota un host de Hyper-V que se ejecuta en una instalación Server Core. Para obtener instrucciones sobre cómo conectarse de forma remota, consulte [administrar hosts de Hyper-V con el Administrador de Hyper-V de forma remota](..\Manage\Remotely-manage-Hyper-V-hosts.md).  
  
## <a name="see-also"></a>Vea también  
  
- [Install-WindowsFeature](https://docs.microsoft.com/powershell/module/Microsoft.Windows.ServerManager.Migration/Install-WindowsFeature)  
