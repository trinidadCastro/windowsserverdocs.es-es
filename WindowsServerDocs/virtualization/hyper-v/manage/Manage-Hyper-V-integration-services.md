---
title: Administrar los servicios de integración de Hyper-V
description: Describe cómo activar y desactivar la servicios de integración e instalarlas si es necesario
ms.technology: compute-hyper-v
author: KBDAzure
ms.author: kathydav
manager: dongill
ms.date: 12/20/2016
ms.topic: article
ms.prod: windows-server-threshold
ms.service: na
ms.assetid: 9cafd6cb-dbbe-4b91-b26c-dee1c18fd8c2
ms.openlocfilehash: e2c14e471abb9af7a9182100969a8dd94a17205a
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812195"
---
>Se aplica a: Windows 10, Windows Server 2016, Windows Server de 2019

# <a name="manage-hyper-v-integration-services"></a>Administrar los servicios de integración de Hyper-V

Servicios de integración de Hyper-V mejorar el rendimiento de la máquina virtual y proporcionar características útiles mediante el aprovechamiento de comunicación bidireccional con el host de Hyper-V. Muchos de estos servicios son comodidades, por ejemplo, la copia de archivos de invitado, mientras que otros son importantes a la funcionalidad de la máquina virtual, como controladores de dispositivo sintético. Este conjunto de servicios y los controladores se conocen a veces como "componentes de integración". Puede controlar o no funcionan los servicios individuales de comodidad para cualquier máquina virtual dada. Los componentes del controlador no pretenden ser atendidas manualmente.

Para obtener más información acerca de cada servicio de integración, consulte [servicios de integración de Hyper-V](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/integration-services).

> [!IMPORTANT]
> Cada servicio que desee usar debe habilitarse en el host y el invitado para poder funcionar. Todos los servicios de integración excepto "Interfaz de servicio de invitado de Hyper-V" están activadas de forma predeterminada en los sistemas operativos invitados de Windows. Los servicios se pueden activar y desactivar individualmente. Las secciones siguientes muestran cómo.

## <a name="turn-an-integration-service-on-or-off-using-hyper-v-manager"></a>Activar un servicio de integración o desactivar mediante el Administrador de Hyper-V

1. Desde el panel central, haga clic en la máquina virtual y haga clic en **configuración**.
  
2. En el panel izquierdo de la **configuración** ventana, en **administración**, haga clic en **Integration Services**.
  
El panel de Integration Services enumera todos los servicios de integración disponibles en el host de Hyper-V, y si el host ha habilitado la máquina virtual para usarlos.

### <a name="turn-an-integration-service-on-or-off-using-powershell"></a>Activar un servicio de integración o desactivar mediante PowerShell

Para hacer esto en PowerShell, use [Enable-VMIntegrationService](https://technet.microsoft.com/library/hh848500.aspx) y [Disable-VMIntegrationService](https://technet.microsoft.com/library/hh848488.aspx).

Los ejemplos siguientes muestran la activación del invitado archivo copia servicio de integración y desactivar para una máquina virtual denominada "demovm".

1. Obtener una lista de servicios de integración en ejecución:
  
    ``` PowerShell
    Get-VMIntegrationService -VMName "DemoVM"
    ```

1. La salida debe parecerse a la siguiente:

    ``` PowerShell
   VMName      Name                    Enabled PrimaryStatusDescription SecondaryStatusDescription
   ------      ----                    ------- ------------------------ --------------------------
   DemoVM      Guest Service Interface False   OK
   DemoVM      Heartbeat               True    OK                       OK
   DemoVM      Key-Value Pair Exchange True    OK
   DemoVM      Shutdown                True    OK
   DemoVM      Time Synchronization    True    OK
   DemoVM      VSS                     True    OK
   ```

1. Activar la interfaz de servicio de invitado:

   ``` PowerShell
   Enable-VMIntegrationService -VMName "DemoVM" -Name "Guest Service Interface"
   ```

1. Compruebe que la interfaz de servicio de invitado está habilitada:

   ```
   Get-VMIntegrationService -VMName "DemoVM" 
   ``` 

1. Desactivar la interfaz de servicio de invitado:

    ```
    Disable-VMIntegrationService -VMName "DemoVM" -Name "Guest Service Interface"
    ```
   
## <a name="checking-the-guests-integration-services-version"></a>Comprobando la versión de servicios de integración de invitado
Algunas características no funcionen correctamente o en absoluto si Servicios de integración de invitado no están actualizados. Para obtener la información de versión de un Windows, inicie sesión en el sistema operativo invitado, abra un símbolo del sistema y ejecute este comando:

```
REG QUERY "HKLM\Software\Microsoft\Virtual Machine\Auto" /v IntegrationServicesVersion
```

Sistemas operativos anteriores no tendrá todos los servicios disponibles. Por ejemplo, los invitados de Windows Server 2008 R2 no pueden tener la "interfaz de servicio de invitado Hyper-V".

## <a name="start-and-stop-an-integration-service-from-a-windows-guest"></a>Iniciar y detener un servicio de integración de un invitado de Windows
Para un servicio de integración en estar completamente funcional, su servicio correspondiente debe estar ejecutando dentro del invitado además está habilitado en el host. En los invitados de Windows, cada servicio de integración aparece como un servicio de Windows estándar. Puede usar el applet Servicios en el Panel de Control o PowerShell para detener e iniciar estos servicios.

> [!IMPORTANT]
> Detener un servicio de integración puede afectar gravemente a la capacidad del host para administrar la máquina virtual. Para que funcione correctamente, cada servicio de integración que desee usar debe habilitarse en el host e invitado.
> Como práctica recomendada, solo debe controlar servicios de integración de Hyper-V mediante las instrucciones anteriores. El servicio de búsqueda de coincidencias en el sistema operativo invitado se detendrá o empezará automáticamente cuando se cambia su estado en Hyper-V.
> Si se inicia un servicio en el sistema operativo invitado, pero está deshabilitado en Hyper-V, se detendrá el servicio. Si se detiene un servicio en el sistema operativo invitado que está habilitado en Hyper-V, Hyper-V finalmente iniciará lo nuevo. Si deshabilita el servicio en el invitado, Hyper-V no se puede iniciar.

### <a name="use-windows-services-to-start-or-stop-an-integration-service-within-a-windows-guest"></a>Usar servicios de Windows para iniciar o detener un servicio de integración dentro de un invitado de Windows

1. Abra el Administrador de servicios mediante la ejecución de ```services.msc``` como administrador o haciendo doble clic en el icono Servicios del Panel de Control.

    ![Captura de pantalla que muestra el panel de servicios de Windows](media/HVServices.png) 

1. Busque los servicios que empiezan por "Hyper-V". 

1. Haga clic en el servicio que desee iniciar o detener. Haga clic en la acción deseada.

### <a name="use-windows-powershell-to-start-or-stop-an-integration-service-within-a-windows-guest"></a>Usar Windows PowerShell para iniciar o detener un servicio de integración dentro de un invitado de Windows

1. Para obtener una lista de servicios de integración, ejecute:

    ```
    Get-Service -Name vm*
    ```

1.  La salida debe ser similar al siguiente:

    ```PowerShell
    Status   Name               DisplayName
    ------   ----               -----------
    Running  vmicguestinterface Hyper-V Guest Service Interface
    Running  vmicheartbeat      Hyper-V Heartbeat Service
    Running  vmickvpexchange    Hyper-V Data Exchange Service
    Running  vmicrdv            Hyper-V Remote Desktop Virtualizati...
    Running  vmicshutdown       Hyper-V Guest Shutdown Service
    Running  vmictimesync       Hyper-V Time Synchronization Service
    Stopped  vmicvmsession      Hyper-V VM Session Service
    Running  vmicvss            Hyper-V Volume Shadow Copy Requestor
    ```

1. Ejecute cualquiera [Start-Service](https://technet.microsoft.com/library/hh849825.aspx) o [Stop-Service](https://technet.microsoft.com/library/hh849790.aspx). Por ejemplo, para desactivar directa de Windows PowerShell, ejecute:

    ```
    Stop-Service -Name vmicvmsession
    ```

## <a name="start-and-stop-an-integration-service-from-a-linux-guest"></a>Iniciar y detener un servicio de integración de un invitado de Linux 

Los servicios de integración de Linux normalmente se ofrecen a través del kernel de Linux. El controlador de servicios de integración de Linux se denomina **hv_utils**.

1. Para averiguar si **hv_utils** se carga, use este comando:

   ``` BASH
   lsmod | grep hv_utils
   ``` 
  
2. La salida debe ser similar al siguiente:  
  
    ``` BASH
    Module                  Size   Used by
    hv_utils               20480   0
    hv_vmbus               61440   8 hv_balloon,hyperv_keyboard,hv_netvsc,hid_hyperv,hv_utils,hyperv_fb,hv_storvsc
    ```

3. Para averiguar si se están ejecutando los demonios necesarios, use este comando.
  
    ``` BASH
    ps -ef | grep hv
    ```
  
4. La salida debe ser similar al siguiente: 
  
    ```BASH
    root       236     2  0 Jul11 ?        00:00:00 [hv_vmbus_con]
    root       237     2  0 Jul11 ?        00:00:00 [hv_vmbus_ctl]
    ...
    root       252     2  0 Jul11 ?        00:00:00 [hv_vmbus_ctl]
    root      1286     1  0 Jul11 ?        00:01:11 /usr/lib/linux-tools/3.13.0-32-generic/hv_kvp_daemon
    root      9333     1  0 Oct12 ?        00:00:00 /usr/lib/linux-tools/3.13.0-32-generic/hv_kvp_daemon
    root      9365     1  0 Oct12 ?        00:00:00 /usr/lib/linux-tools/3.13.0-32-generic/hv_vss_daemon
    scooley  43774 43755  0 21:20 pts/0    00:00:00 grep --color=auto hv          
    ```

5. Para ver los demonios que están disponibles, ejecute:

    ``` BASH
    compgen -c hv_
    ```
  
6. La salida debe ser similar al siguiente:
  
    ``` BASH
    hv_vss_daemon
    hv_get_dhcp_info
    hv_get_dns_info
    hv_set_ifconfig
    hv_kvp_daemon
    hv_fcopy_daemon     
    ```
  
   Demonios de servicio de integración que debe aparecer incluyen lo siguiente. Si falta alguno, que no se admiten en el sistema o no se pueden instalar. Para obtener detalles, vea [Linux y FreeBSD máquinas virtuales de Hyper-V en Windows](https://technet.microsoft.com/library/dn531030.aspx).  
   - **hv_vss_daemon**: Este demonio es necesario para crear las copias de seguridad de máquina virtual en directo de Linux.
   - **hv_kvp_daemon**: Este demonio permite establecer y consultar pares clave-valor intrínsecos y extrínsecos.
   - **hv_fcopy_daemon**: Este demonio implementa un servicio entre el host e invitadas de copia de archivos.  

### <a name="examples"></a>Ejemplos

Estos ejemplos muestra cómo detener e iniciar el demonio KVP, denominado `hv_kvp_daemon`.

1. Use el identificador de proceso \(PID\) para detener el proceso del demonio. Para buscar el PID, examine la segunda columna de la salida o use `pidof`. Los demonios de Hyper-V ejecutan como raíz, por lo que necesitará permisos de raíz.

    ``` BASH
    sudo kill -15 `pidof hv_kvp_daemon`
    ```

1. Para comprobar que todos los `hv_kvp_daemon` proceso han desaparecido, ejecute:

    ```
    ps -ef | hv
    ```

1. Para iniciar el demonio de nuevo, ejecute el demonio como raíz:

    ``` BASH
    sudo hv_kvp_daemon
    ``` 

1. Para comprobar que el `hv_kvp_daemon` proceso aparece con un nuevo Id. de proceso, ejecute:

    ```
    ps -ef | hv
    ```

## <a name="keep-integration-services-up-to-date"></a>Manténgase al día los servicios de integración

Se recomienda que mantenga los servicios de integración actualizado para obtener el mejor rendimiento y características más recientes para las máquinas virtuales. Esto ocurre para la mayoría de los invitados de Windows de forma predeterminada si se configuran para obtener las actualizaciones importantes de Windows Update. Invitados de Linux con kernels actuales recibirán los componentes de integración más recientes cuando actualice el kernel.

**Para máquinas virtuales que se ejecutan en hosts de Windows 10:**

> [!NOTE]
> El archivo de imagen vmguest.iso no se incluye con Hyper-V en Windows 10 porque ya no se necesita.

| Invitado  | Mecanismo de actualización | Notas |
|:---------|:---------|:---------|
| Windows 10 | Windows Update | |
| Windows 8.1 | Windows Update | |
| Windows 8 | Windows Update | Requiere el Servicio de integración de intercambio de datos.* |
| Windows 7 | Windows Update | Requiere el Servicio de integración de intercambio de datos.* |
| Windows Vista (SP 2) | Windows Update | Requiere el Servicio de integración de intercambio de datos.* |
| - | | |
| Windows Server 2016 | Windows Update | |
| Windows Server, canal semianual | Windows Update | |
| Windows Server 2012 R2 | Windows Update | |
| Windows Server 2012 | Windows Update | Requiere el Servicio de integración de intercambio de datos.* |
| Windows Server 2008 R2 (SP 1) | Windows Update | Requiere el Servicio de integración de intercambio de datos.* |
| Windows Server 2008 (SP 2) | Windows Update | Soporte extendido solo en Windows Server 2016 ([más](https://support.microsoft.com/lifecycle?p1=12925)). |
| Windows Home Server 2011 | Windows Update | No se admitirán en Windows Server 2016 ([más](https://support.microsoft.com/lifecycle?p1=15820)). |
| Windows Small Business Server 2011 | Windows Update | No se admite con el soporte estándar ([más información](https://support.microsoft.com/lifecycle?p1=15817)). |
| - | | |
| Invitados Linux | administrador de paquetes | Servicios de integración para Linux están integrados en la distribución, pero puede haber actualizaciones opcionales disponibles. ******** |

\* Si no se puede habilitar el servicio de integración de intercambio de datos, los servicios de integración de estos invitados están disponibles en el [centro de descarga de](https://support.microsoft.com/kb/3071740) como un archivo contenedor (cab). Las instrucciones para aplicar un archivo .cab están disponibles en este [entrada de blog](https://blogs.technet.com/b/virtualization/archive/2015/07/24/integration-components-available-for-virtual-machines-not-connected-to-windows-update.aspx).

**Para las máquinas virtuales que se ejecutan en hosts de Windows 8.1:**

| Invitado  | Mecanismo de actualización | Notas |
|:---------|:---------|:---------|
| Windows 10 | Windows Update | |
| Windows 8.1 | Windows Update | |
| Windows 8 | Disco de servicios de integración | Consulte [instrucciones](#install-or-update-integration-services), a continuación. |
| Windows 7 | Disco de servicios de integración | Consulte [instrucciones](#install-or-update-integration-services), a continuación. |
| Windows Vista (SP 2) | Disco de servicios de integración | Consulte [instrucciones](#install-or-update-integration-services), a continuación. |
| Windows XP (SP 2, SP 3) | Disco de servicios de integración | Consulte [instrucciones](#install-or-update-integration-services), a continuación. |
| - | | |
| Windows Server 2016 | Windows Update | |
| Windows Server, canal semianual | Windows Update | |
| Windows Server 2012 R2 | Windows Update | |
| Windows Server 2012 | Disco de servicios de integración | Consulte [instrucciones](#install-or-update-integration-services), a continuación. |
| Windows Server 2008 R2 | Disco de servicios de integración | Consulte [instrucciones](#install-or-update-integration-services), a continuación. |
| Windows Server 2008 (SP 2) | Disco de servicios de integración | Consulte [instrucciones](#install-or-update-integration-services), a continuación. |
| Windows Home Server 2011 | Disco de servicios de integración | Consulte [instrucciones](#install-or-update-integration-services), a continuación. |
| Windows Small Business Server 2011 | Disco de servicios de integración | Consulte [instrucciones](#install-or-update-integration-services), a continuación. |
| Windows Server 2003 R2 (SP 2) | Disco de servicios de integración | Consulte [instrucciones](#install-or-update-integration-services), a continuación. |
| Windows Server 2003 (SP 2) | Disco de servicios de integración | Consulte [instrucciones](#install-or-update-integration-services), a continuación. |
| - | | |
| Invitados Linux | administrador de paquetes | Servicios de integración para Linux están integrados en la distribución, pero puede haber actualizaciones opcionales disponibles. ** |


**Para las máquinas virtuales que se ejecutan en hosts de Windows 8:**

| Invitado  | Mecanismo de actualización | Notas |
|:---------|:---------|:---------|
| Windows 8.1 | Windows Update | |
| Windows 8 | Disco de servicios de integración | Consulte [instrucciones](#install-or-update-integration-services), a continuación. |
| Windows 7 | Disco de servicios de integración | Consulte [instrucciones](#install-or-update-integration-services), a continuación. |
| Windows Vista (SP 2) | Disco de servicios de integración | Consulte [instrucciones](#install-or-update-integration-services), a continuación. |
| Windows XP (SP 2, SP 3) | Disco de servicios de integración | Consulte [instrucciones](#install-or-update-integration-services), a continuación. |
| - | | |
| Windows Server 2012 R2 | Windows Update | |
| Windows Server 2012 | Disco de servicios de integración | Consulte [instrucciones](#install-or-update-integration-services), a continuación. |
| Windows Server 2008 R2 | Disco de servicios de integración | Consulte [instrucciones](#install-or-update-integration-services), a continuación.|
| Windows Server 2008 (SP 2) | Disco de servicios de integración | Consulte [instrucciones](#install-or-update-integration-services), a continuación. |
| Windows Home Server 2011 | Disco de servicios de integración | Consulte [instrucciones](#install-or-update-integration-services), a continuación. |
| Windows Small Business Server 2011 | Disco de servicios de integración | Consulte [instrucciones](#install-or-update-integration-services), a continuación. |
| Windows Server 2003 R2 (SP 2) | Disco de servicios de integración | Consulte [instrucciones](#install-or-update-integration-services), a continuación. |
| Windows Server 2003 (SP 2) | Disco de servicios de integración | Consulte [instrucciones](#install-or-update-integration-services), a continuación. |
| - | | |
| Invitados Linux | administrador de paquetes | Servicios de integración para Linux están integrados en la distribución, pero puede haber actualizaciones opcionales disponibles. ** |

Para obtener más detalles sobre los invitados de Linux, consulte [Linux y FreeBSD máquinas virtuales de Hyper-V en Windows](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows).

## <a name="install-or-update-integration-services"></a>Instalar o actualizar servicios de integración

Para los hosts anteriores a Windows Server 2016 y Windows 10, deberá instalar manualmente o actualizar los servicios de integración en los sistemas operativos invitados. 
  
1.  Abre el Administrador Hyper-V. En el menú Herramientas del administrador del servidor, haga clic en **Administrador de Hyper-V**.  
  
2.  Conéctese a la máquina virtual. Haga clic en la máquina virtual y haga clic en **Connect**.  
  
3.  En el menú Acción de Conexión a máquina virtual, haga clic en **Insertar disco de instalación de servicios de integración**. Esta acción carga el disco de instalación en la unidad de DVD virtual. Según el sistema operativo invitado, es posible que deba iniciar la instalación manualmente.  
  
4.  Al finalizar la instalación, todos los servicios de integración estarán disponibles para su uso.

Estos pasos no se pueden automatizar o realizar dentro de una sesión de Windows PowerShell para máquinas virtuales en línea. Se puede aplicar a las imágenes VHDX sin conexión; [consulte este blog](https://blogs.technet.microsoft.com/virtualization/2013/04/18/how-to-install-integration-services-when-the-virtual-machine-is-not-running/).
