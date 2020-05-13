---
title: Administrar Integration Services de Hyper-V
description: Describe cómo activar y desactivar los servicios de integración e instalarlos si es necesario.
ms.technology: compute-hyper-v
author: kbdazure
ms.author: kathydav
manager: dongill
ms.date: 12/20/2016
ms.topic: article
ms.prod: windows-server
ms.assetid: 9cafd6cb-dbbe-4b91-b26c-dee1c18fd8c2
ms.openlocfilehash: 4237da8ee393953a8eb2a2b577c2df201f96a7be
ms.sourcegitcommit: aed942d11f1a361fc1d17553a4cf190a864d1268
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/12/2020
ms.locfileid: "83235059"
---
# <a name="manage-hyper-v-integration-services"></a>Administrar Integration Services de Hyper-V

> Se aplica a: Windows 10, Windows Server 2012, Windows Server 2012R2, Windows Server 2016 y Windows Server 2019

Hyper-V Integration Services mejorar el rendimiento de las máquinas virtuales y proporcionar características útiles al aprovechar la comunicación bidireccional con el host de Hyper-V. Muchos de estos servicios son cómodos, como la copia de archivos de invitado, mientras que otros son importantes para la funcionalidad de la máquina virtual, como los controladores de dispositivos sintéticos. Este conjunto de servicios y controladores a veces se denominan "componentes de integración". Puede controlar si los servicios de comodidad individuales funcionan o no para una máquina virtual determinada. Los componentes del controlador no están pensados para que se puedan atender manualmente.

Para obtener más información sobre cada servicio de integración, consulte [Integration Services de Hyper-V](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/integration-services).

> [!IMPORTANT]
> Cada servicio que desee usar debe estar habilitado en el host y en el invitado para poder funcionar. Todos los servicios de integración excepto "Interfaz de servicio de invitado de Hyper-V" están activados de forma predeterminada en los sistemas operativos invitados de Windows. Los servicios se pueden activar y desactivar individualmente. En las secciones siguientes se muestra cómo hacerlo.

## <a name="turn-an-integration-service-on-or-off-using-hyper-v-manager"></a>Activar o desactivar un servicio de integración con el administrador de Hyper-V

1. En el panel central, haga clic con el botón secundario en la máquina virtual y haga clic en **configuración**.

2. En el panel izquierdo de la ventana de **configuración** , en **Administración**, haga clic en **Integration Services**.

En el panel Integration Services se enumeran todos los servicios de integración disponibles en el host de Hyper-V y si el host ha habilitado la máquina virtual para usarlos.

### <a name="turn-an-integration-service-on-or-off-using-powershell"></a>Activar o desactivar un servicio de integración mediante PowerShell

Para hacer esto en PowerShell, use [enable-VMIntegrationService](https://technet.microsoft.com/library/hh848500.aspx) y [Disable-VMIntegrationService](https://technet.microsoft.com/library/hh848488.aspx).

En los siguientes ejemplos se muestra cómo activar y desactivar el servicio de integración de copia de archivos invitados para una máquina virtual denominada "demovm".

1. Obtiene una lista de los servicios de integración en ejecución:

    ``` PowerShell
    Get-VMIntegrationService -VMName "DemoVM"
    ```

1. El resultado debe ser similar al siguiente:

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

1. Active la interfaz del servicio de invitado:

   ``` PowerShell
   Enable-VMIntegrationService -VMName "DemoVM" -Name "Guest Service Interface"
   ```

1. Compruebe que la interfaz de servicio de invitado está habilitada:

   ```
   Get-VMIntegrationService -VMName "DemoVM"
   ```

1. Desactivar la interfaz del servicio de invitado:

    ```
    Disable-VMIntegrationService -VMName "DemoVM" -Name "Guest Service Interface"
    ```

## <a name="checking-the-guests-integration-services-version"></a>Comprobando la versión de Integration Services del invitado
Es posible que algunas características no funcionen correctamente o en absoluto si los servicios de integración del invitado no están actualizados. Para obtener la información de versión de una ventana de, inicie sesión en el sistema operativo invitado, abra un símbolo del sistema y ejecute este comando:

```
REG QUERY "HKLM\Software\Microsoft\Virtual Machine\Auto" /v IntegrationServicesVersion
```

Los sistemas operativos invitados anteriores no tendrán todos los servicios disponibles. Por ejemplo, los invitados de Windows Server 2008 R2 no pueden tener el "Interfaz de servicio de invitado de Hyper-V".

## <a name="start-and-stop-an-integration-service-from-a-windows-guest"></a>Iniciar y detener un servicio de integración desde un invitado de Windows
Para que un servicio de integración sea totalmente funcional, el servicio correspondiente debe estar en ejecución en el invitado, además de habilitarse en el host. En los invitados de Windows, cada servicio de integración aparece como un servicio de Windows estándar. Puede usar el applet servicios del panel de control o PowerShell para detener e iniciar estos servicios.

> [!IMPORTANT]
> Detener un servicio de integración puede afectar gravemente a la capacidad del host de administrar la máquina virtual. Para que funcione correctamente, todos los servicios de integración que desee usar deben estar habilitados tanto en el host como en el invitado.
> Como práctica recomendada, solo debe controlar Integration Services desde Hyper-V con las instrucciones anteriores. El servicio de búsqueda de coincidencias en el sistema operativo invitado se detendrá o se iniciará automáticamente al cambiar su estado en Hyper-V.
> Si inicia un servicio en el sistema operativo invitado, pero está deshabilitado en Hyper-V, el servicio se detendrá. Si detiene un servicio en el sistema operativo invitado que está habilitado en Hyper-v, Hyper-V lo iniciará en ese momento. Si deshabilita el servicio en el invitado, Hyper-V no podrá iniciarlo.

### <a name="use-windows-services-to-start-or-stop-an-integration-service-within-a-windows-guest"></a>Usar los servicios de Windows para iniciar o detener un servicio de integración en un invitado de Windows

1. Abra el administrador de servicios ejecutando ```services.msc``` como administrador o haciendo doble clic en el icono servicios del panel de control.

    ![Captura de pantalla que muestra el panel servicios de Windows](media/HVServices.png)

1. Busque los servicios que empiezan por "Hyper-V".

1. Haga clic con el botón secundario en el servicio que desee iniciar o detener. Haga clic en la acción deseada.

### <a name="use-windows-powershell-to-start-or-stop-an-integration-service-within-a-windows-guest"></a>Usar Windows PowerShell para iniciar o detener un servicio de integración en un invitado de Windows

1. Para obtener una lista de Integration Services, ejecute:

    ```
    Get-Service -Name vm*
    ```

1.  La salida debe tener un aspecto similar a este:

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

1. Ejecute [Start-Service](https://technet.microsoft.com/library/hh849825.aspx) o [Stop-Service](https://technet.microsoft.com/library/hh849790.aspx). Por ejemplo, para desactivar Windows PowerShell Direct, ejecute:

    ```
    Stop-Service -Name vmicvmsession
    ```

## <a name="start-and-stop-an-integration-service-from-a-linux-guest"></a>Inicio y detención de un servicio de integración desde un invitado de Linux

Los servicios de integración de Linux normalmente se ofrecen a través del kernel de Linux. El controlador de servicios de integración de Linux se denomina **hv_utils**.

1. Para averiguar si se carga **hv_utils** , use este comando:

   ``` BASH
   lsmod | grep hv_utils
   ```

2. La salida debe tener un aspecto similar a este:

    ``` BASH
    Module                  Size   Used by
    hv_utils               20480   0
    hv_vmbus               61440   8 hv_balloon,hyperv_keyboard,hv_netvsc,hid_hyperv,hv_utils,hyperv_fb,hv_storvsc
    ```

3. Use este comando para averiguar si se están ejecutando los demonios necesarios.

    ``` BASH
    ps -ef | grep hv
    ```

4. La salida debe tener un aspecto similar a este:

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

6. La salida debe tener un aspecto similar a este:

    ``` BASH
    hv_vss_daemon
    hv_get_dhcp_info
    hv_get_dns_info
    hv_set_ifconfig
    hv_kvp_daemon
    hv_fcopy_daemon
    ```

   Entre los demonios de servicio de integración que se pueden mostrar se incluyen los siguientes. Si falta alguno, es posible que no se admita en el sistema o que no esté instalado. Para obtener más información, consulte [máquinas virtuales Linux y FreeBSD compatibles con Hyper-V en Windows](https://technet.microsoft.com/library/dn531030.aspx).
   - **hv_vss_daemon**: este demonio es necesario para crear copias de seguridad de máquinas virtuales de Linux en vivo.
   - **hv_kvp_daemon**: este demonio permite establecer y consultar los pares de valores de clave intrínsecos y extrínsecos.
   - **hv_fcopy_daemon**: este demonio implementa un servicio de copia de archivos entre el host y el invitado.

### <a name="examples"></a>Ejemplos

En estos ejemplos se muestra cómo detener e iniciar el demonio KVP, denominado `hv_kvp_daemon` .

1. Use el PID de ID. de proceso \( \) para detener el proceso del demonio. Para buscar el PID, examine la segunda columna de la salida, o use `pidof` . Los demonios de Hyper-V se ejecutan como raíz, por lo que necesitará permisos raíz.

    ``` BASH
    sudo kill -15 `pidof hv_kvp_daemon`
    ```

1. Para comprobar que todo el `hv_kvp_daemon` proceso ha desaparecido, ejecute:

    ```
    ps -ef | hv
    ```

1. Para volver a iniciar el demonio, ejecute el demonio como raíz:

    ``` BASH
    sudo hv_kvp_daemon
    ```

1. Para comprobar que el `hv_kvp_daemon` proceso aparece con un nuevo identificador de proceso, ejecute:

    ```
    ps -ef | hv
    ```

## <a name="keep-integration-services-up-to-date"></a>Mantener los servicios de integración actualizados

Se recomienda mantener actualizados los servicios de integración para obtener el mejor rendimiento y las características más recientes de las máquinas virtuales. Esto sucede en la mayoría de los invitados de Windows de forma predeterminada si están configurados para obtener actualizaciones importantes de Windows Update. Los invitados de Linux que usan kernels actuales recibirán los componentes de integración más recientes al actualizar el kernel.

**En el caso de las máquinas virtuales que se ejecutan en hosts de Windows 10/Windows Server 2016/2019:**

> [!NOTE]
> El archivo de imagen vmguest. ISO no se incluye con Hyper-V en Windows 10/Windows Server 2016/2019 porque ya no es necesario.

| Invitado  | Mecanismo de actualización | Notas |
|:---------|:---------|:---------|
| Windows 10 | Windows Update | |
| Windows 8.1 | Windows Update | |
| Windows 8 | Windows Update | Requiere el Servicio de integración de intercambio de datos.* |
| Windows 7 | Windows Update | Requiere el Servicio de integración de intercambio de datos.* |
| Windows Vista (SP 2) | Windows Update | Requiere el Servicio de integración de intercambio de datos.* |
| - | | |
| Windows Server 2016 | Windows Update | |
| Windows Server, Canal semianual | Windows Update | |
| Windows Server 2012 R2 | Windows Update | |
| Windows Server 2012 | Windows Update | Requiere el Servicio de integración de intercambio de datos.* |
| Windows Server 2008 R2 (SP 1) | Windows Update | Requiere el Servicio de integración de intercambio de datos.* |
| Windows Server 2008 (SP 2) | Windows Update | Soporte extendido solo en Windows Server 2016 ([más información](https://support.microsoft.com/lifecycle?p1=12925)). |
| Windows Home Server 2011 | Windows Update | No se admitirá en Windows Server 2016 ([Lea más](https://support.microsoft.com/lifecycle?p1=15820)). |
| Windows Small Business Server 2011 | Windows Update | No se admite con el soporte estándar ([más información](https://support.microsoft.com/lifecycle?p1=15817)). |
| - | | |
| Invitados Linux | administrador de paquetes | Integration Services para Linux está integrado en el distribución, pero puede que haya actualizaciones opcionales disponibles. ******** |

\*Si no se puede habilitar el servicio de integración de intercambio de datos, los servicios de integración de estos invitados están disponibles en el [centro de descarga](https://support.microsoft.com/kb/3071740) como un archivo contenedor (CAB). Las instrucciones para aplicar un archivo. cab están disponibles en esta [entrada de blog](https://techcommunity.microsoft.com/t5/virtualization/integration-components-available-for-virtual-machines-not/ba-p/382247).

**En el caso de las máquinas virtuales que se ejecutan en hosts de Windows 8.1/Windows Server 2012R2:**

| Invitado  | Mecanismo de actualización | Notas |
|:---------|:---------|:---------|
| Windows 10 | Windows Update | |
| Windows 8.1 | Disco de servicios de integración | Vea las [instrucciones](#install-or-update-integration-services)siguientes. |
| Windows 8 | Disco de servicios de integración | Vea las [instrucciones](#install-or-update-integration-services)siguientes. |
| Windows 7 | Disco de servicios de integración | Vea las [instrucciones](#install-or-update-integration-services)siguientes. |
| Windows Vista (SP 2) | Disco de servicios de integración | Vea las [instrucciones](#install-or-update-integration-services)siguientes. |
| Windows XP (SP 2, SP 3) | Disco de servicios de integración | Vea las [instrucciones](#install-or-update-integration-services)siguientes. |
| - | | |
| Windows Server 2016 | Windows Update | |
| Windows Server, Canal semianual | Windows Update | |
| Windows Server 2012 R2 | Disco de servicios de integración | Vea las [instrucciones](#install-or-update-integration-services)siguientes. |
| Windows Server 2012 | Disco de servicios de integración | Vea las [instrucciones](#install-or-update-integration-services)siguientes. |
| Windows Server 2008 R2 | Disco de servicios de integración | Vea las [instrucciones](#install-or-update-integration-services)siguientes. |
| Windows Server 2008 (SP 2) | Disco de servicios de integración | Vea las [instrucciones](#install-or-update-integration-services)siguientes. |
| Windows Home Server 2011 | Disco de servicios de integración | Vea las [instrucciones](#install-or-update-integration-services)siguientes. |
| Windows Small Business Server 2011 | Disco de servicios de integración | Vea las [instrucciones](#install-or-update-integration-services)siguientes. |
| Windows Server 2003 R2 (SP 2) | Disco de servicios de integración | Vea las [instrucciones](#install-or-update-integration-services)siguientes. |
| Windows Server 2003 (SP 2) | Disco de servicios de integración | Vea las [instrucciones](#install-or-update-integration-services)siguientes. |
| - | | |
| Invitados Linux | administrador de paquetes | Integration Services para Linux está integrado en el distribución, pero puede que haya actualizaciones opcionales disponibles. ** |


**En el caso de las máquinas virtuales que se ejecutan en hosts de Windows 8/Windows Server 2012:**

| Invitado  | Mecanismo de actualización | Notas |
|:---------|:---------|:---------|
| Windows 8.1 | Disco de servicios de integración | Vea las [instrucciones](#install-or-update-integration-services)siguientes. |
| Windows 8 | Disco de servicios de integración | Vea las [instrucciones](#install-or-update-integration-services)siguientes. |
| Windows 7 | Disco de servicios de integración | Vea las [instrucciones](#install-or-update-integration-services)siguientes. |
| Windows Vista (SP 2) | Disco de servicios de integración | Vea las [instrucciones](#install-or-update-integration-services)siguientes. |
| Windows XP (SP 2, SP 3) | Disco de servicios de integración | Vea las [instrucciones](#install-or-update-integration-services)siguientes. |
| - | | |
| Windows Server 2012 R2 | Disco de servicios de integración | Vea las [instrucciones](#install-or-update-integration-services)siguientes. |
| Windows Server 2012 | Disco de servicios de integración | Vea las [instrucciones](#install-or-update-integration-services)siguientes. |
| Windows Server 2008 R2 | Disco de servicios de integración | Vea las [instrucciones](#install-or-update-integration-services)siguientes.|
| Windows Server 2008 (SP 2) | Disco de servicios de integración | Vea las [instrucciones](#install-or-update-integration-services)siguientes. |
| Windows Home Server 2011 | Disco de servicios de integración | Vea las [instrucciones](#install-or-update-integration-services)siguientes. |
| Windows Small Business Server 2011 | Disco de servicios de integración | Vea las [instrucciones](#install-or-update-integration-services)siguientes. |
| Windows Server 2003 R2 (SP 2) | Disco de servicios de integración | Vea las [instrucciones](#install-or-update-integration-services)siguientes. |
| Windows Server 2003 (SP 2) | Disco de servicios de integración | Vea las [instrucciones](#install-or-update-integration-services)siguientes. |
| - | | |
| Invitados Linux | administrador de paquetes | Integration Services para Linux está integrado en el distribución, pero puede que haya actualizaciones opcionales disponibles. ** |

Para obtener más información sobre los invitados de Linux, consulte [máquinas virtuales Linux y FreeBSD compatibles con Hyper-V en Windows](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows).

## <a name="install-or-update-integration-services"></a>Instalar o actualizar los servicios de integración

> [!NOTE]
> En el caso de los hosts anteriores a Windows Server 2016 y Windows 10, deberá **instalar o actualizar manualmente** los servicios de integración en los sistemas operativos invitados.

Procedimiento para instalar o actualizar manualmente Integration Services:

1.  Abra el administrador de Hyper-V. En el menú herramientas de Administrador del servidor, haga clic en **Administrador de Hyper-V**.

2.  Conexión a una máquina virtual. Haga clic con el botón secundario en la máquina virtual y haga clic en **conectar**.

3.  En el menú Acción de Conexión a máquina virtual, haga clic en **Insertar disco de instalación de servicios de integración**. Esta acción carga el disco de instalación en la unidad de DVD virtual. En función del sistema operativo invitado, es posible que deba iniciar la instalación manualmente.

4.  Al finalizar la instalación, todos los servicios de integración estarán disponibles para su uso.

> [!NOTE]
> Estos pasos **no se pueden automatizar** ni realizar en una sesión de Windows PowerShell para las máquinas virtuales **en línea** .
> Puede aplicarlos a imágenes VHDX **sin conexión** ; vea [Cómo instalar Integration Services cuando la máquina virtual no se está ejecutando](https://docs.microsoft.com/virtualization/community/team-blog/2013/20130418-how-to-install-integration-services-when-the-virtual-machine-is-not-running).
> También puede automatizar la implementación de los servicios de integración a través de **Configuration Manager** con las máquinas virtuales **en línea**, pero debe reiniciar las máquinas virtuales al final de la instalación. consulte [implementación de Integration Services de Hyper-V en máquinas virtuales con el administrador de configuración y DISM](https://docs.microsoft.com/archive/blogs/manageabilityguys/deploying-hyper-v-integration-services-to-vms-using-config-manager-and-dism)
