---
title: Administrar la infraestructura hiperconvergida con el centro de administración de Windows
description: Administrar la infraestructura hiperconvergida con el centro de administración de Windows (proyecto Honolulu)
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 03/01/2019
ms.localizationpriority: medium
ms.openlocfilehash: 56d953c721fff2218b256fa99d83078485438c0f
ms.sourcegitcommit: 5344adcf9c0462561a4f9d47d80afc1d095a5b13
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2020
ms.locfileid: "90765976"
---
# <a name="manage-hyper-converged-infrastructure-with-windows-admin-center"></a>Administrar la infraestructura hiperconvergida con el centro de administración de Windows

>Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

## <a name="what-is-hyper-converged-infrastructure"></a>Qué es la infraestructura hiperconvergida

La infraestructura hiperconvergida consolida el proceso, el almacenamiento y la red definidos por software en un clúster para proporcionar una virtualización de alto rendimiento, rentable y fácilmente escalable. Esta funcionalidad se presentó en Windows Server 2016 con [espacios de almacenamiento directo](../../../storage/storage-spaces/storage-spaces-direct-overview.md), las [redes definidas por software](../../../networking/sdn/software-defined-networking.md) y [Hyper-V](../../../virtualization/hyper-v/hyper-v-on-windows-server.md).

> [!Tip]
> ¿Desea adquirir una infraestructura hiperconvergida? Microsoft recomienda estas soluciones [definidas por software de Windows Server](https://microsoft.com/wssd) de nuestros asociados. Se diseñan, ensamblan y validan con nuestra arquitectura de referencia para garantizar la compatibilidad y la confiabilidad, de modo que pueda ponerse en marcha rápidamente.

> [!IMPORTANT]
> Algunas de las características que se describen en este artículo solo están disponibles en la versión preliminar del centro de administración de Windows. [¿Cómo obtener esta versión?](../overview.md)

## <a name="what-is-windows-admin-center"></a>¿Qué es Windows Admin Center?

El [centro de administración de Windows](../overview.md) es la herramienta de administración de la próxima generación para Windows Server, la sucesora de herramientas "integradas" tradicionales como administrador del servidor. Es gratuito y se puede instalar y usar sin conexión a Internet. Puede usar el centro de administración de Windows para administrar y supervisar la infraestructura hiperconvergida que ejecuta Windows Server 2016 o Windows Server 2019.

![Panel de clúster hiperconvergido](../media/manage-hyper-converged/hci-dashboard-v1809.png)

## <a name="key-features"></a>Principales características

Los aspectos destacados del centro de administración de Windows para la infraestructura hiperconvergida incluyen:

- **Una sola sección de vidrio unificada para proceso, almacenamiento y redes en breve.** Vea las máquinas virtuales, los servidores host, los volúmenes, las unidades y mucho más dentro de una experiencia de uso interconectada, coherente y compilada.
- **Crear y administrar espacios de almacenamiento y máquinas virtuales de Hyper-V.** Flujos de trabajo radicalmente simples para crear, abrir, cambiar de tamaño y eliminar volúmenes; y crear, iniciar, conectar y trasladar máquinas virtuales. y mucho más.
- **Supervisión eficaz en todo el clúster.** En el panel se representan el uso de la CPU y la memoria, la capacidad de almacenamiento, la IOPS, el rendimiento y la latencia en tiempo real, en todos los servidores del clúster, con alertas claras cuando algo no es correcto.
- **Compatibilidad con redes definidas por software (SDN).** Administrar y supervisar redes virtuales, subredes, conectar máquinas virtuales a redes virtuales y supervisar la infraestructura de SDN.

Microsoft está desarrollando activamente el centro de administración de Windows para la infraestructura hiperconvergida. Recibe actualizaciones frecuentes que mejoran las características existentes y agregan nuevas características.

## <a name="before-you-start"></a>Antes de empezar

Para administrar el clúster como una infraestructura hiperconvergida en el centro de administración de Windows, debe ejecutar Windows Server 2016 o Windows Server 2019 y tener habilitado Hyper-V y Espacios de almacenamiento directo. Opcionalmente, también puede tener habilitadas las redes definidas por software y administradas a través del centro de administración de Windows.

> [!Tip]
> El centro de administración de Windows también ofrece una experiencia de administración de uso general para cualquier clúster que admita cualquier carga de trabajo, disponible para Windows Server 2012 y versiones posteriores. Si esto suena mejor, al agregar el clúster al centro de administración de Windows, seleccione clúster de [**conmutación por error**](manage-failover-clusters.md) en lugar de **clúster hiperconvergido**.

### <a name="prepare-your-windows-server-2016-cluster-for-windows-admin-center"></a>Preparar el clúster de Windows Server 2016 para el centro de administración de Windows

El centro de administración de Windows para la infraestructura hiperconvergida depende de las API de administración agregadas después de que se publicara Windows Server 2016. Antes de poder administrar el clúster de Windows Server 2016 con el centro de administración de Windows, deberá realizar estos dos pasos:

1. Compruebe que todos los servidores del clúster hayan instalado la [actualización acumulativa 2018-05 para Windows server 2016 (KB4103723)](https://support.microsoft.com/help/4103723/windows-10-update-kb4103723) o posterior. Para descargar e instalar esta actualización, vaya a **configuración**  >  **Actualizar & seguridad**  >  **Windows Update** y seleccione **Buscar actualizaciones en línea desde Microsoft Update**.
2. Ejecute el siguiente cmdlet de PowerShell como administrador en el clúster:

```powershell
    Add-ClusterResourceType -Name "SDDC Management" -dll "$env:SystemRoot\Cluster\sddcres.dll" -DisplayName "SDDC Management"
```

> [!Tip]
> Solo tiene que ejecutar el cmdlet una vez, en cualquier servidor del clúster. Puede ejecutarlo localmente en Windows PowerShell o usar el proveedor de servicios de seguridad de credenciales (CredSSP) para ejecutarlo de forma remota. En función de la configuración, es posible que no pueda ejecutar este cmdlet desde el centro de administración de Windows.

### <a name="prepare-your-windows-server-2019-cluster-for-windows-admin-center"></a>Preparar el clúster de Windows Server 2019 para el centro de administración de Windows

Si el clúster ejecuta Windows Server 2019, no es necesario realizar los pasos anteriores. Solo tiene que agregar el clúster al centro de administración de Windows como se describe en la sección siguiente y está listo.

### <a name="configure-software-defined-networking-optional"></a>Configurar redes definidas por software (opcional) ###

Puede configurar la infraestructura hiperconvergida que ejecuta Windows Server 2016 o 2019 para usar redes definidas por software (SDN) con los siguientes pasos:

1. Prepare el VHD del sistema operativo que es el mismo sistema operativo que instaló en los hosts de la infraestructura hiperconvergida. Este VHD se usará para todas las máquinas virtuales de NC/SLB/GW.
2. Descargue todos los archivos y carpetas de SDN Express desde [https://github.com/Microsoft/SDN/tree/master/SDNExpress](https://github.com/Microsoft/SDN/tree/master/SDNExpress) .
3. Preparar una máquina virtual diferente mediante la consola de implementación. Esta máquina virtual debe poder tener acceso a los hosts de SDN. Además, la máquina virtual debe tener instalada la herramienta de Hyper-V de RSAT.
4. Copie todo lo que descargó para SDN Express en la máquina virtual de la consola de implementación. Y comparta esta carpeta **SDNExpress** . Asegúrese de que todos los hosts puedan tener acceso a la carpeta compartida **SDNExpress** , tal como se define en la línea 8 del archivo de configuración:
   ```
    \\$env:Computername\SDNExpress
   ```
5. Copie el VHD del sistema operativo en la carpeta **images** en la carpeta **SDNExpress** de la máquina virtual de la consola de implementación.
6. Modifique la configuración de SDN Express con la configuración de su entorno. Complete los dos pasos siguientes después de modificar la configuración de SDN Express según la información del entorno.
7. Ejecute PowerShell con privilegios de administrador para implementar SDN:

```powershell
    .\SDNExpress.ps1 -ConfigurationDataFile .\your_fabricconfig.PSD1 -verbose
```

La implementación tardará aproximadamente entre 30 y 45 minutos.

## <a name="get-started"></a>Introducción

Una vez implementada la infraestructura hiperconvergida, puede administrarla mediante el centro de administración de Windows.

### <a name="install-windows-admin-center"></a>Instalación de Windows Admin Center

Si todavía no lo ha hecho, descargue e instale el centro de administración de Windows. La forma más rápida de empezar a trabajar es instalarla en el equipo con Windows 10 y administrar los servidores de forma remota. Esto tarda menos de cinco minutos. [Descargue ahora](../overview.md) u [Obtenga más información sobre otras opciones de instalación](../deploy/install.md).

### <a name="add-hyper-converged-cluster"></a>Agregar clúster hiperconvergido

Para agregar el clúster al centro de administración de Windows:

1. Haga clic en **+ Agregar** en todas las conexiones.
2. Elija Agregar una **conexión de clúster hiperconvergida**.
3. Escriba el nombre del clúster y, si se le solicita, las credenciales que se usarán.
4. Haga clic en **Agregar** para finalizar.

El clúster se agregará a la lista de conexiones. Haga clic en él para iniciar el panel.

![Agregar una conexión de clúster hiperconvergida](../media/manage-hyper-converged/add-hyper-converged-cluster-connection.gif)

### <a name="add-sdn-enabled-hyper-converged-cluster-windows-admin-center-preview"></a>Agregar clúster hiperconvergido con SDN habilitado (versión preliminar del centro de administración de Windows)

La versión más reciente del centro de administración de Windows es compatible con la administración de redes definidas por software para la infraestructura hiperconvergida. Al agregar un URI de REST de la controladora de red a la conexión de clúster hiperconvergida, puede usar el administrador de clústeres hiperconvergido para administrar los recursos de SDN y supervisar la infraestructura de SDN.

1. Haga clic en **+ Agregar** en todas las conexiones.
2. Elija Agregar una **conexión de clúster hiperconvergida**.
3. Escriba el nombre del clúster y, si se le solicita, las credenciales que se usarán.
4. Active **la casilla configurar la controladora de red** para continuar.
5. Escriba el **URI de la controladora de red** y haga clic en **validar**.
6. Haga clic en **Agregar** para finalizar.

El clúster se agregará a la lista de conexiones. Haga clic en él para iniciar el panel.

![Agregar una conexión de clúster hiperconvergida habilitada para SDN](../media/manage-hyper-converged/add-snd-enabled-hci-connection.png)

> [!Important]
> Actualmente no se admiten los entornos de SDN con autenticación Kerberos para la comunicación de Northbound.

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

### <a name="are-there-differences-between-managing-windows-server-2016-and-windows-server-2019"></a>¿Existen diferencias entre la administración de Windows Server 2016 y Windows Server 2019?

Sí. El centro de administración de Windows para la infraestructura hiperconvergida recibe actualizaciones frecuentes que mejoran la experiencia de Windows Server 2016 y Windows Server 2019. Sin embargo, algunas características nuevas solo están disponibles para Windows Server 2019, por ejemplo, el modificador de alternancia para la desduplicación y la compresión.

### <a name="can-i-use-windows-admin-center-to-manage-storage-spaces-direct-for-other-use-cases-not-hyper-converged-such-as-converged-scale-out-file-server-sofs-or-microsoft-sql-server"></a>¿Puedo usar el centro de administración de Windows para administrar Espacios de almacenamiento directo para otros casos de uso (no hiperconvergidos), como Servidor de archivos de escalabilidad horizontal convergente (SoFS) o Microsoft SQL Server?

El centro de administración de Windows para la infraestructura hiperconvergida no proporciona opciones de administración o supervisión específicamente para otros casos de uso de Espacios de almacenamiento directo; por ejemplo, no puede crear recursos compartidos de archivos. Sin embargo, el panel y las características principales, como la creación de volúmenes o la sustitución de unidades, funcionan para cualquier clúster de Espacios de almacenamiento directo.

### <a name="whats-the-difference-between-a-failover-cluster-and-a-hyper-converged-cluster"></a>¿Cuál es la diferencia entre un clúster de conmutación por error y un clúster hiperconvergido?

En general, el término "hiperconvergido" hace referencia a la ejecución de Hyper-V y Espacios de almacenamiento directo en los mismos servidores en clúster para virtualizar los recursos de proceso y almacenamiento. En el contexto del centro de administración de Windows, al hacer clic en **+ Agregar** en la lista de conexiones, puede elegir entre agregar una **conexión de clúster de conmutación por error** o una **conexión de clúster hiperconvergida**:

- La **conexión de clúster de conmutación por error** es la sucesora de la aplicación de escritorio administrador de clústeres de conmutación por error. Proporciona una experiencia de administración familiar y de uso general para cualquier clúster que admita cualquier carga de trabajo, incluido Microsoft SQL Server. Está disponible para Windows Server 2012 y versiones posteriores.

- La **conexión de clúster hiperconvergida** es una experiencia de todo-nuevo adaptada a espacios de almacenamiento directo e Hyper-V. Presenta el Panel y enfatiza gráficos y alertas para la supervisión. Está disponible para Windows Server 2016 y Windows Server 2019.

### <a name="why-do-i-need-the-latest-cumulative-update-for-windows-server-2016"></a>¿Por qué se necesita la actualización acumulativa más reciente para Windows Server 2016?

El centro de administración de Windows para la infraestructura hiperconvergida depende de las API de administración desarrolladas desde que se lanzó Windows Server 2016. Estas API se agregan en la [actualización acumulativa 2018-05 para Windows Server 2016 (KB4103723)](https://support.microsoft.com/help/4103723/windows-10-update-kb4103723), disponibles a partir del 8 de mayo de 2018.

### <a name="how-much-does-it-cost-to-use-windows-admin-center"></a>¿Cuánto cuesta usar Windows Admin Center?

Windows Admin Center no tiene ningún coste adicional más allá de Windows.

Puedes usar Windows Admin Center (disponible como una descarga independiente) con licencias válidas de Windows Server o Windows 10 sin coste adicional; se concede bajo licencia en un EULA complementario de Windows.

### <a name="does-windows-admin-center-require-system-center"></a>¿Necesita Windows Admin Center System Center?

No.

### <a name="does-it-require-an-internet-connection"></a>¿Requiere una conexión a Internet?

No.

Aunque el centro de administración de Windows ofrece una integración eficaz y cómoda con la nube de Microsoft Azure, la experiencia de administración y supervisión básica para la infraestructura hiperconvergida es totalmente local. Se puede instalar y usar sin conexión a Internet.

## <a name="things-to-try"></a>Opciones que puede probar

Si acaba de empezar, estos son algunos tutoriales rápidos que le ayudarán a saber cómo se organiza y funciona el centro de administración de Windows para la infraestructura hiperconvergida. Utilice buenos juicios y tenga cuidado con los entornos de producción. Estos vídeos se grabaron con la versión 1804 del centro de administración de Windows y una compilación de Insider Preview de Windows Server 2019.

### <a name="manage-storage-spaces-direct-volumes"></a>Administración de volúmenes de Espacios de almacenamiento directo

<ul>
               <li>(0:37) <a href="https://youtu.be/o66etKq70N8">creación de un volumen de reflejo triple</a></li>
               <li>(1:17) <a href="https://youtu.be/R72QHudqWpE">creación de un volumen de paridad acelerado para reflejo</a></li>
               <li>(1:02) <a href="https://youtu.be/j59z7ulohs4">Cómo abrir un volumen y agregar archivos</a></li>
               <li>(0:51) <a href="https://youtu.be/PRibTacyKko">Cómo activar la desduplicación y la compresión</a></li>
               <li>(0:47) <a href="https://youtu.be/hqyBzipBoTI">Cómo expandir un volumen</a></li>
               <li>(0:26) <a href="https://youtu.be/DbjF8r2F6Jo">eliminación de un volumen</a></li>
</ul>

<table>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Crear volumen, reflejo triple</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/o66etKq70N8" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>Crear volumen, paridad de reflejo acelerado</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/R72QHudqWpE" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Abrir volumen y agregar archivos</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/j59z7ulohs4" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>Activar la desduplicación y la compresión</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/PRibTacyKko" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Expandir volumen</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/hqyBzipBoTI" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>Eliminar volumen</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/DbjF8r2F6Jo" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
</table>

### <a name="create-a-new-virtual-machine"></a>Creación de una máquina virtual

1. Haga clic en la herramienta **virtual machines** del panel de navegación del lado izquierdo.
2. En la parte superior de la herramienta Virtual Machines, elija la pestaña **inventario** y, a continuación, haga clic en **nuevo** para crear una nueva máquina virtual.
3. Escriba el nombre de la máquina virtual y elija entre las máquinas virtuales de generación 1 y 2.
4. A continuación, uou puede elegir en qué host se debe crear inicialmente la máquina virtual o usar el host recomendado.
5. Elija una ruta de acceso para los archivos de la máquina virtual. Elija un volumen en la lista desplegable o haga clic en **examinar** para elegir una carpeta con el selector de carpetas. Los archivos de configuración de la máquina virtual y el archivo de disco duro virtual se guardarán en una sola carpeta en la `\Hyper-V\[virtual machine name]` ruta de acceso del volumen o la ruta de acceso seleccionados.
6. Elija el número de procesadores virtuales, si desea que la virtualización anidada esté habilitada, configure las opciones de memoria, los adaptadores de red, los discos duros virtuales y elija si desea instalar un sistema operativo desde un archivo de imagen. ISO o desde la red.
7. Haga clic en **Crear** para crear la máquina virtual.
8. Una vez creada la máquina virtual y aparece en la lista de máquinas virtuales, puede iniciar la máquina virtual.
9. Una vez que se inicia la máquina virtual, puede conectarse a la consola de la máquina virtual a través de VMConnect para instalar el sistema operativo. Seleccione la máquina virtual en la lista, haga clic en **más**  >  **conectar** para descargar el archivo. RDP. Abra el archivo. RDP en la aplicación Conexión a Escritorio remoto. Puesto que se está conectando a la consola de la máquina virtual, deberá escribir las credenciales de administrador del host de Hyper-V.

[Más información acerca de la administración de máquinas virtuales con el centro de administración de Windows](manage-virtual-machines.md).

### <a name="pause-and-safely-restart-a-server"></a>Pausar y reiniciar un servidor de forma segura

1. En el **Panel**, seleccione **servidores** en la navegación en el lado izquierdo o haga clic en el vínculo **Ver servidores >**  en el icono de la esquina inferior derecha del panel.
2. En la parte superior, cambie de **Resumen** a la pestaña **inventario** .
3. Seleccione un servidor haciendo clic en su nombre para abrir la página de detalles del **servidor** .
4. Haga clic en **pausar servidor para mantenimiento**. Si es seguro continuar, se moverán las máquinas virtuales a otros servidores del clúster. El servidor tendrá un agotamiento del estado mientras esto sucede. Si lo desea, puede ver las máquinas virtuales que se mueven en la página **máquinas virtuales > inventario** , donde su servidor host se muestra claramente en la cuadrícula. Cuando todas las máquinas virtuales se hayan cambiado, el estado del servidor será en **pausa**.
5. Haga clic en **administrar servidor** para tener acceso a todas las herramientas de administración por servidor en el centro de administración de Windows.
6. Haga clic en **reiniciar**y, a continuación, en **sí**. Se volverá a poner en la lista de conexiones.
7. De nuevo en el **Panel**, el servidor está coloreado en rojo mientras está inactivo.
8. Una vez que haya realizado una copia de seguridad, vaya de nuevo a la página del **servidor** y haga clic en **reanudar servidor desde mantenimiento** para volver a establecer el estado del servidor en simple. En el tiempo, las máquinas virtuales se moverán: no se requiere ninguna acción por parte del usuario.

### <a name="replace-a-failed-drive"></a>Reemplazar una unidad con errores

1. Cuando se produce un error en una unidad, aparece una alerta en el área de **alertas** superior izquierda del **Panel**.
2. También puede seleccionar **Drives** (Unidades) en el panel de navegación del lado izquierdo, o hacer clic en el vínculo **VIEW DRIVES >** (VER UNIDADES) en el icono de la esquina inferior derecha para examinar las unidades y ver su estado. En la pestaña **inventario** , la cuadrícula admite la ordenación, la agrupación y la búsqueda de palabras clave.
3. En el **Panel**, haga clic en la alerta para ver los detalles, como la ubicación física de la unidad.
4. Para más información, haga clic en el acceso directo **Go to drive** (vaya a la unidad) para ir a la página de detalles de la unidad **Drive**.
5. Si el hardware lo admite, puede hacer clic en **Turn light on/off** (Activar o desactivar la luz) para controlar la luz del indicador de la unidad.
6. Espacios de almacenamiento directo retira y evacua automáticamente las unidades con errores. Cuando esto sucede, el estado de la unidad es Retired (Retirada) y su barra de capacidad de almacenamiento está vacía.
7. Quite la unidad con errores e inserte la unidad de reemplazo.
8. En **Drives > Inventory** (Unidades > Inventario) aparecerá la nueva unidad. Con el tiempo, la alerta se borrará, los volúmenes volverán a un estado correcto y el almacenamiento se reequilibrará en la nueva unidad. No se requiere ninguna acción del usuario.

### <a name="manage-virtual-networks-sdn-enabled-hci-clusters-using-windows-admin-center-preview"></a>Administrar redes virtuales (clústeres HCI habilitados para SDN mediante la versión preliminar del centro de administración de Windows)

1. Seleccione **redes virtuales** en la navegación del lado izquierdo.
2. Haga clic en **nuevo** para crear una nueva red virtual y subredes, o elija una red virtual existente y haga clic en **configuración** para modificar su configuración.
3. Haga clic en una red virtual existente para ver las conexiones de máquinas virtuales a las subredes de la red virtual y a las listas de control de acceso aplicadas a las subredes de la red virtual.

![Administración de redes virtuales](../media/manage-hyper-converged/manage-virtual-networks.png)

### <a name="connect-a-virtual-machine-to-a-virtual-network-sdn-enabled-hci-clusters-using-windows-admin-center-preview"></a>Conectar una máquina virtual a una red virtual (clústeres HCI habilitados para SDN mediante la versión preliminar del centro de administración de Windows)

1. Seleccione **virtual machines** en la navegación del lado izquierdo.
2. Elija una máquina virtual existente > haga clic en **configuración** > Abra la pestaña **redes** en **configuración**.
3. Configure los campos **Virtual Network** y **subred virtual** para conectar la máquina virtual a una red virtual.

También puede configurar la red virtual al crear una máquina virtual.

![Conexión de una máquina virtual a una red virtual](../media/manage-hyper-converged/connect-vm-to-virtual-network.png)

### <a name="monitor-software-defined-networking-infrastructure-sdn-enabled-hci-clusters-using-windows-admin-center-preview"></a>Supervisar la infraestructura de red definida por software (clústeres HCI habilitados para SDN mediante la versión preliminar del centro de administración de Windows)

1. Seleccione **supervisión de Sdn** en la navegación del lado izquierdo.
2. Vea información detallada sobre el estado de la controladora de red, el Load Balancer de software, la puerta de enlace virtual y supervise el grupo de puerta de enlace virtual, el uso del grupo de direcciones IP públicas y privadas y el estado del host de SDN.

![Supervisión de la infraestructura de SDN](../media/manage-hyper-converged/sdn-monitoring.png)

## <a name="give-us-feedback"></a>Envíenos sus comentarios.

¡ Es todo lo que tiene que hacer! La ventaja más importante de las actualizaciones frecuentes es oír lo que está trabajando y lo que se debe mejorar. Estas son algunas formas de hacernos saber lo que está pensando:

- [Envíe y vote solicitudes de características en UserVoice](https://windowsserver.uservoice.com/forums/295071/category/319162?query=%5Bhci%5D)
- [Únase al foro del centro de administración de Windows en Microsoft Tech Community](https://techcommunity.microsoft.com/t5/Windows-Server-Management/bd-p/WindowsServerManagement)
- Tweet hasta `@servermgmt`

### <a name="additional-references"></a>Referencias adicionales

- [Windows Admin Center](../overview.md)
- [Espacios de almacenamiento directo](../../../storage/storage-spaces/storage-spaces-direct-overview.md)
- [Hyper-V](../../../virtualization/hyper-v/hyper-v-on-windows-server.md)
- [Redes definidas por software](../../../networking/sdn/software-defined-networking.md)