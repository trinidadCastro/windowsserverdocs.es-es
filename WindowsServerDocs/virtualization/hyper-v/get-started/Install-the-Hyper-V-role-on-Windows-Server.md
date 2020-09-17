---
title: Instalación del rol de Hyper-V en Windows Server
description: Proporciona instrucciones para la instalación de Hyper-V con Administrador del servidor o Windows PowerShell.
ms.topic: get-started-article
ms.assetid: 8e871317-09d2-4314-a6ec-ced12b7aee89
ms.author: benarm
author: BenjaminArmstrong
ms.date: 12/02/2016
ms.openlocfilehash: e8b95d744954d477e33322cad8d20ff3ff8d7258
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746010"
---
# <a name="install-the-hyper-v-role-on-windows-server"></a>Instalación del rol de Hyper-V en Windows Server

>Se aplica a: Windows Server 2016 y Windows Server 2019

Para crear y ejecutar máquinas virtuales, instale el rol de Hyper-V en Windows Server mediante Administrador del servidor o el cmdlet **install-WindowsFeature** en Windows PowerShell.
Para Windows 10, consulte [instalación de Hyper-V en Windows 10](/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v).

Para obtener más información acerca de Hyper-V, consulte la [información general sobre la tecnología de Hyper-v](../Hyper-V-Technology-Overview.md). Para probar Windows Server 2019, puede descargar e instalar una copia de evaluación. Consulte el [centro de evaluación](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019).

Antes de instalar Windows Server o de agregar el rol de Hyper-V, asegúrese de que:
- El hardware del equipo es compatible. Para obtener más información, consulte [requisitos del sistema para Windows Server](../../../get-started/System-Requirements.md) y [requisitos del sistema para Hyper-V en Windows Server](../System-requirements-for-Hyper-V-on-Windows.md).
- No tiene previsto usar aplicaciones de virtualización de terceros que se basan en las mismas características de procesador que requiere Hyper-V. Entre los ejemplos se incluyen VMWare Workstation y VirtualBox. Puede instalar Hyper-V sin desinstalar estas otras aplicaciones. Sin embargo, si intenta usarlas para administrar máquinas virtuales cuando el hipervisor de Hyper-V se está ejecutando, es posible que las máquinas virtuales no se inicien o se ejecuten de forma no confiable. Para obtener más información e instrucciones para desactivar el hipervisor de Hyper-V si necesita usar una de estas aplicaciones, consulte [las aplicaciones de virtualización no funcionan en conjunto con Hyper-V, Device Guard y Credential Guard](https://support.microsoft.com/help/3204980/virtualization-applications-do-not-work-together-with-hyper-v-device-g).

Si desea instalar solo las herramientas de administración, como el administrador de Hyper-V, consulte [administración remota de hosts de Hyper-v con el administrador de Hyper-v](../Manage/Remotely-manage-Hyper-V-hosts.md).

## <a name="install-hyper-v-by-using-server-manager"></a>Instale Hyper-V mediante Administrador del servidor

1. En el menú **Administrador del servidor** del menú **Administrar**, haz clic en **Agregar roles y características**.

2. En la página **Antes de comenzar**, compruebe que el servidor de destino y el entorno de red estén preparados para el rol y la característica que desea instalar. Haga clic en **Siguiente**.

3. En la página **Seleccionar tipo de instalación**, seleccione **Instalación basada en características o en roles** y, a continuación, en **Siguiente**.

4. En la página **Seleccionar servidor de destino**, seleccione un servidor del grupo de servidores y haga clic en **Siguiente**.

5. En la pantalla **Seleccionar roles de servidor**, seleccione **Hyper-V**.

6. Para agregar las herramientas que usa para crear y administrar máquinas virtuales, haga clic en **Agregar características**. En la página Características, haga clic en **Siguiente**.

7. En las páginas **Crear conmutadores virtuales**, **Migración de máquinas virtuales** y **Almacenes predeterminados**, seleccione las opciones apropiadas.

8. En la página **Confirmar selecciones de instalación**, seleccione **Reiniciar automáticamente el servidor de destino en caso necesario** y haga clic en **Instalar**.

9. Una vez finalizada la instalación, compruebe que Hyper-V se ha instalado correctamente. Abra la página **todos los servidores** en Administrador del servidor y seleccione un servidor en el que haya instalado Hyper-V. Compruebe el icono **roles y características** en la página del servidor seleccionado.

## <a name="install-hyper-v-by-using-the-install-windowsfeature-cmdlet"></a>Instalación de Hyper-V mediante el cmdlet install-WindowsFeature

1. En el Escritorio de Windows, haga clic en el botón Inicio y escriba cualquier parte del nombre **Windows PowerShell**.

2. Haga clic con el botón derecho en Windows PowerShell y seleccione **Ejecutar como administrador**.

3. Para instalar Hyper-V en un servidor al que esté conectado de forma remota, ejecute el siguiente comando y reemplácelo `<computer_name>` por el nombre del servidor.

    ```powershell
    Install-WindowsFeature -Name Hyper-V -ComputerName <computer_name> -IncludeManagementTools -Restart
    ```

    Si está conectado localmente al servidor, ejecute el comando sin `-ComputerName <computer_name>` .

4. Una vez reiniciado el servidor, puede ver que el rol de Hyper-V está instalado y ver qué otros roles y características se instalan mediante la ejecución del siguiente comando:

    ```powershell
    Get-WindowsFeature -ComputerName <computer_name>
    ```

    Si está conectado localmente al servidor, ejecute el comando sin `-ComputerName <computer_name>` .

> [!NOTE]
> Si instala este rol en un servidor que ejecuta la opción de instalación Server Core de Windows Server 2016 y usa el parámetro `-IncludeManagementTools` , solo se instala el módulo de Hyper-V para Windows PowerShell. Puede usar la herramienta de administración de GUI, administrador de Hyper-V, en otro equipo para administrar de forma remota un host de Hyper-V que se ejecuta en una instalación Server Core. Para obtener instrucciones sobre cómo conectarse de forma remota, consulte [administración remota de hosts de Hyper-v con el administrador de Hyper-v](../Manage/Remotely-manage-Hyper-V-hosts.md).

## <a name="additional-references"></a>Referencias adicionales

- [Install-WindowsFeature](/powershell/module/Microsoft.Windows.ServerManager.Migration/Install-WindowsFeature)