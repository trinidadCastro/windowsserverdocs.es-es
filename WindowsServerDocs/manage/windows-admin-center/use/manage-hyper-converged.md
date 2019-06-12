---
title: Administrar infraestructuras Hiperconvergidas con Windows Admin Center
description: Administrar infraestructuras Hiperconvergidas con Windows Admin Center (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 03/01/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: fe00072932d9c7f283ebd887a5292ac9a9d0e37f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446032"
---
# <a name="manage-hyper-converged-infrastructure-with-windows-admin-center"></a>Administrar infraestructuras Hiperconvergidas con Windows Admin Center

>Se aplica a: Windows Admin Center, vista previa de Windows Admin Center

## <a name="what-is-hyper-converged-infrastructure"></a>¿Qué es la infraestructura de Hyper-Converged

Infraestructura Hiperconvergida consolida red en un clúster para proporcionar alto rendimiento, rentable y la virtualización fácilmente escalable, almacenamiento y proceso definidas por software. Esta funcionalidad se introdujo en Windows Server 2016 con [espacios de almacenamiento directo](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview), [redes definidas por Software](https://docs.microsoft.com/en-us/windows-server/networking/sdn/software-defined-networking) y [Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-on-windows-server).

> [!Tip]
> ¿Desea para adquirir Hyper-Converged infraestructura? Microsoft recomienda estos [definido por el Software de Windows Server](https://microsoft.com/wssd) soluciones de nuestros asociados. Están diseñadas, ensambla y validadas con respecto a nuestra arquitectura de referencia para garantizar la compatibilidad y confiabilidad, por lo que ponerse en marcha rápidamente.

> [!IMPORTANT]
> Algunas de las características descritas en este artículo solo están disponibles en versión preliminar de Windows Admin Center. [¿Cómo se puede obtener esta versión?](http://aka.ms/windowsadmincenter)

## <a name="what-is-windows-admin-center"></a>Qué es Windows Admin Center

[Windows Admin Center](../understand/windows-admin-center.md) es la herramienta de administración de próxima generación para Windows Server, el sucesor de las herramientas tradicionales de "en el cuadro" como administrador del servidor. Es gratuito y se puede instalar y usar sin una conexión a Internet. Puede usar Windows Admin Center para administrar y supervisar la infraestructura de Hyper-Converged que ejecutan Windows Server 2016 o Windows Server 2019.

![Panel de clúster hiperconvergido](../media/manage-hyper-converged/hci-dashboard-v1809.png)

## <a name="key-features"></a>Principales características

Aspectos destacados de Windows Admin Center Hyper-Converged infraestructura incluyen:

- **Unificada único-de-consola de proceso, almacenamiento y redes pronto.** Ver las máquinas virtuales, servidores de host, volúmenes, unidades etc. dentro de una experiencia diseñados específicamente, coherente y conectadas entre sí.
- **Crear y administrar máquinas virtuales de Hyper-V y espacios de almacenamiento.** Los flujos de trabajo más sencillas para crear, abrir, cambiar el tamaño y eliminar volúmenes; crear, iniciar, conectarse a y mover las máquinas virtuales; y mucho más.
- **Supervisión eficaz de todo el clúster.** El panel de gráficos de memoria y uso de CPU, capacidad de almacenamiento, IOPS, rendimiento y latencia en tiempo real, en todos los servidores del clúster, desactive alertas cuando algo no es correcto.
- **Soporte técnico de software Defined Networking (SDN).** Administrar y supervisar redes virtuales, subredes, conectar máquinas virtuales a las redes virtuales y supervisar la infraestructura de SDN.

Windows Admin Center Hyper-Converged infraestructura está siendo desarrollado activamente por Microsoft. Recibe actualizaciones frecuentes que mejoran las características existentes y agregan nuevas características.

## <a name="before-you-start"></a>Antes de empezar

Para administrar el clúster como infraestructura Hyper-Converged en Windows Admin Center, debe estar ejecutando Windows Server 2016 o Windows Server 2019 y han habilitado Hyper-V y espacios de almacenamiento directo. Si lo desea, también puede tener redes definidas por Software puede habilitar y administrar a través de Windows Admin Center.

> [!Tip]
> Windows Admin Center también ofrece la experiencia de administración de uso general para los clústeres que admiten cualquier carga de trabajo disponible para Windows Server 2012 y versiones posteriores. Si esto suena como una solución mejor, cuando se agrega el clúster a Windows Admin Center, seleccione [ **clúster de conmutación por error** ](manage-failover-clusters.md) en lugar de **Hyper-Converged clúster**.

### <a name="prepare-your-windows-server-2016-cluster-for-windows-admin-center"></a>Preparar el clúster de Windows Server 2016 para Windows Admin Center

Windows Admin Center para infraestructura Hyper-Converged depende después del lanzamiento de Windows Server 2016 se ha agregado las API de administración. Antes de poder administrar el clúster de Windows Server 2016 con Windows Admin Center, deberá realizar estos dos pasos:

1. Compruebe que todos los servidores del clúster se ha instalado el [2018-05 de la actualización acumulativa para Windows Server 2016 (KB4103723)](https://support.microsoft.com/help/4103723/windows-10-update-kb4103723) o una versión posterior. Para descargar e instalar esta actualización, vaya a **configuración** > **actualización y seguridad** > **actualizar Windows** y seleccione  **Buscar actualizaciones desde Microsoft Update en línea**.
2. Ejecute el siguiente cmdlet de PowerShell como administrador en el clúster:

```powershell
    Add-ClusterResourceType -Name "SDDC Management" -dll "$env:SystemRoot\Cluster\sddcres.dll" -DisplayName "SDDC Management"
```

> [!Tip]
> Solo deberá ejecutar el cmdlet una vez, en cualquier servidor en el clúster. Puede ejecutarla localmente en Windows PowerShell o use el proveedor de servicio de seguridad de credenciales (CredSSP) para ejecutar de forma remota. Según la configuración, puede no ser capaz de ejecutar este cmdlet desde dentro de Windows Admin Center.

### <a name="prepare-your-windows-server-2019-cluster-for-windows-admin-center"></a>Preparar el clúster de Windows Server 2019 para Windows Admin Center

Si el clúster ejecuta Windows Server 2019, los pasos anteriores no son necesarios. Solo tiene que agregar el clúster a Windows Admin Center como se describe en la sección siguiente y estará listo para continuar.

### <a name="configure-software-defined-networking-optional"></a>Configurar definidas por Software de red (opcional) ###

Puede configurar la infraestructura de Hyper-Converged ejecuta Windows Server 2016 o 2019 para usar redes definidas por Software (SDN) con los pasos siguientes:

1. Prepare el VHD del SO que es el mismo sistema operativo ha instalado en los hosts de infraestructuras hiperconvergidas. Este disco duro virtual se usará para todas las máquinas virtuales de NC, SLB/GW.
2. Descargar todas las carpetas y archivos en SDN Express desde [ https://github.com/Microsoft/SDN/tree/master/SDNExpress ](https://github.com/Microsoft/SDN/tree/master/SDNExpress).
3. Preparar una máquina virtual diferente mediante la consola de implementación. Esta máquina virtual debería poder tener acceso a los hosts SDN. Además, la máquina virtual debe tener instalada la herramienta de Hyper-V de RSAT.
4. Copie todo el contenido descargado para SDN Express en la consola de implementación de máquina virtual. Compartir **SDNExpress** carpeta. Asegúrese de que todos los hosts pueden tener acceso a la **SDNExpress** carpeta compartida, como se define en la línea 8 del archivo de configuración:
   ```
    \\$env:Computername\SDNExpress
   ```
5. Copiar el VHD del SO para la **imágenes** carpeta bajo la **SDNExpress** carpeta en la consola de implementación de máquina virtual.
6. Modificar la configuración de SDN Express con la configuración del entorno. Finalizar los dos pasos siguientes después de modificar la configuración de SDN Express basada en información de su entorno.
7. Ejecute PowerShell con privilegios de administrador para la implementación de SDN:

```powershell
    .\SDNExpress.ps1 -ConfigurationDataFile .\your_fabricconfig.PSD1 -verbose 
```

La implementación tardará aproximadamente 30 y 45 minutos.

## <a name="get-started"></a>Comenzar

Una vez implementada la infraestructura de Hyper-Converged, se puede administrar con Windows Admin Center.

### <a name="install-windows-admin-center"></a>Instalar Windows Admin Center

Si no lo ha hecho ya, descargue e instale Windows Admin Center. Lo más rápido hasta ponerse en marcha y ejecución es instalarlo en el equipo de Windows 10 y administrar los servidores de forma remota. Esto tarda menos de cinco minutos. [Descargar ahora](https://aka.ms/windowsadmincenter) o [más información sobre otras opciones de instalación](../deploy/install.md).

### <a name="add-hyper-converged-cluster"></a>Agregar clúster Hiperconvergido

Para agregar el clúster a Windows Admin Center:

1. Haga clic en **+ agregar** en todas las conexiones.
2. Optar por agregar una **conexión del clúster Hyper-Converged**.
3. Escriba el nombre del clúster y, si se le pide las credenciales para usar.
4. Haga clic en **agregar** para finalizar.

El clúster se agregarán a la lista de conexiones. Haga clic en él para iniciar el panel.

![Agregar conexión de clúster hiperconvergido](../media/manage-hyper-converged/add-hyper-converged-cluster-connection.gif)

### <a name="add-sdn-enabled-hyper-converged-cluster-windows-admin-center-preview"></a>Agregar clúster Hiperconvergido habilitado SDN (versión preliminar de Windows Admin Center)

La última versión preliminar de Windows Admin Center admite la administración de redes definidas por Software de infraestructura Hyper-Converged. Mediante la adición de un URI de REST de controladora de red para la conexión de clúster Hiperconvergido, puede usar el Administrador de clústeres hiperconvergidos para administrar los recursos de SDN y supervisar la infraestructura de SDN.

1. Haga clic en **+ agregar** en todas las conexiones.
2. Optar por agregar una **conexión del clúster Hyper-Converged**.
3. Escriba el nombre del clúster y, si se le pide las credenciales para usar.
4. Comprobar **configurar la controladora de red** para continuar.
5. Escriba el **URI del controlador de red** y haga clic en **validar**.
6. Haga clic en **agregar** para finalizar.

El clúster se agregarán a la lista de conexiones. Haga clic en él para iniciar el panel.

![Agregar conexión habilitada para SDN clúster hiperconvergido](../media/manage-hyper-converged/add-snd-enabled-hci-connection.png)

> [!Important]
> Actualmente no se admiten entornos de SDN con la autenticación Kerberos para la comunicación de Northbound.

## <a name="frequently-asked-questions"></a>Preguntas frecuentes

### <a name="are-there-differences-between-managing-windows-server-2016-and-windows-server-2019"></a>¿Hay diferencias entre la administración de Windows Server 2016 y Windows Server 2019?

Sí. Windows Admin Center Hyper-Converged infraestructura recibe actualizaciones frecuentes que mejoran la experiencia de Windows Server 2016 y Windows Server 2019. Sin embargo, algunas características nuevas solo están disponibles para Windows Server 2019: por ejemplo, el modificador para alternar de desduplicación y compresión.

### <a name="can-i-use-windows-admin-center-to-manage-storage-spaces-direct-for-other-use-cases-not-hyper-converged-such-as-converged-scale-out-file-server-sofs-or-microsoft-sql-server"></a>¿Puedo usar Windows Admin Center para administrar espacios de almacenamiento directo para otros casos de uso (no hiperconvergido), como servidor de archivos de escalabilidad horizontal (SoFS) o Microsoft SQL Server?

Windows Admin Center para infraestructura Hyper-Converged no proporciona administración o las opciones de supervisión específicamente para otros casos de uso de espacios de almacenamiento directo: por ejemplo, no puede crear recursos compartidos de archivos. Sin embargo, las características de panel y core, como crear volúmenes o reemplazando unidades, funcionan para cualquier clúster de espacios de almacenamiento directo.

### <a name="whats-the-difference-between-a-failover-cluster-and-a-hyper-converged-cluster"></a>¿Qué es la diferencia entre un clúster de conmutación por error y Hyper-Converged?

En general, el término "hiperconvergido" se refiere a la ejecución de Hyper-V y espacios de almacenamiento directo en el mismo en el clúster de servidores para virtualizar los recursos de proceso y almacenamiento. En el contexto de Windows Admin Center, al hacer clic en **+ agregar** en la lista de conexiones, puede elegir entre agregar un **conexión del clúster de conmutación por error** o un **Hyper-Converged clúster conexión**:

- El **conexión del clúster de conmutación por error** es el sucesor de la aplicación de escritorio del Administrador de clústeres de conmutación por error. Proporciona una experiencia de administración familiares y de uso general para los clústeres que admiten cualquier carga de trabajo, incluido Microsoft SQL Server. Está disponible para Windows Server 2012 y posterior.

- El **conexión del clúster Hyper-Converged** se adapta una experiencia completamente nueva para Hyper-V y espacios de almacenamiento directo. Presenta el Panel y enfatiza gráficos y alertas para la supervisión. Está disponible para Windows Server 2016 y Windows Server 2019.

### <a name="why-do-i-need-the-latest-cumulative-update-for-windows-server-2016"></a>¿Por qué necesito la actualización acumulativa más reciente para Windows Server 2016?

Windows Admin Center Hyper-Converged infraestructura depende de la administración de que API desarrolladas desde el lanzamiento de Windows Server 2016. Estas API se agregan en el [2018-05 de la actualización acumulativa para Windows Server 2016 (KB4103723)](https://support.microsoft.com/help/4103723/windows-10-update-kb4103723), disponible a partir del 8 de mayo de 2018.

### <a name="how-much-does-it-cost-to-use-windows-admin-center"></a>¿Cuánto cuesta usar Windows Admin Center?

Windows Admin Center no tiene ningún coste adicional más allá de Windows.

Puede usar Windows Admin Center (disponible como descarga independiente) con licencias válidas de Windows Server o Windows 10 sin ningún costo adicional: arreglo un CLUF adicional de Windows.

### <a name="does-windows-admin-center-require-system-center"></a>¿Necesita Windows Admin Center System Center?

No.

### <a name="does-it-require-an-internet-connection"></a>¿Requiere una conexión a Internet?

No.

Aunque Windows Admin Center ofrece eficaces y de integración conveniente con la nube de Microsoft Azure, la administración central y la experiencia de supervisión de infraestructura Hyper-Converged está completamente en el entorno local. Se puede instalar y usar sin una conexión a Internet.

## <a name="things-to-try"></a>Opciones que puede probar

Si acaba de empezar, aquí tiene algunos tutoriales rápidos para ayudarle a aprender cómo Windows Admin Center Hyper-Converged infraestructura está organizada y funciona. Por favor, ejecute buen juicio y debe tener cuidado con los entornos de producción. Estos vídeos se grabaron con Windows Admin Center versión 1804 y una compilación de Insider Preview de Windows Server 2019.

### <a name="manage-storage-spaces-direct-volumes"></a>Administrar volúmenes de espacios de almacenamiento directo

<ul>
               <li>(0:37) <a href="https://youtu.be/o66etKq70N8">cómo crear un volumen triple</a></li>
               <li>(1:17) <a href="https://youtu.be/R72QHudqWpE">cómo crear un volumen reflejado acelerada paridad</a></li>
               <li>(1:02) <a href="https://youtu.be/j59z7ulohs4">cómo abrir un volumen y agregar archivos</a></li>
               <li>(0:51) <a href="https://youtu.be/PRibTacyKko">cómo activar la compresión y desduplicación</a></li>
               <li>(0:47) <a href="https://youtu.be/hqyBzipBoTI">cómo expandir un volumen</a></li>
               <li>(0:26) <a href="https://youtu.be/DbjF8r2F6Jo">cómo eliminar un volumen</a></li>
</ul>

<table>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Crear volumen, triple</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/o66etKq70N8" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>Crear volumen reflejado acelerada paridad</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/R72QHudqWpE" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Abra el volumen y agregar archivos</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/j59z7ulohs4" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>Activar la compresión y desduplicación</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/PRibTacyKko" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Expanda el volumen</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/hqyBzipBoTI" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>Eliminar volumen</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/DbjF8r2F6Jo" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
</table>

### <a name="create-a-new-virtual-machine"></a>Crear una máquina virtual nueva

1. Haga clic en el **máquinas virtuales** herramienta desde el panel de navegación del lado izquierdo.
2. En la parte superior de la herramienta de máquinas virtuales, elija el **inventario** y, después, haga clic en **New** para crear una nueva máquina virtual.
3. Escriba el nombre de la máquina virtual y elija entre las máquinas virtuales de generación 1 y 2.
4. Con qué facilidad puede, a continuación, puede elegir qué host para crear la máquina virtual en inicialmente o utilizar el host recomendado.
5. Elija una ruta de acceso para los archivos de máquina virtual. Elija un volumen en la lista desplegable o haga clic en **examinar** para elegir una carpeta con el selector de carpeta. Los archivos de configuración de máquina virtual y el archivo de disco duro virtual se guardarán en una sola carpeta en el `\Hyper-V\[virtual machine name]` ruta de acceso de la ruta de acceso o el volumen seleccionado.
6. Elija el número de procesadores virtuales, si desea habilitar la virtualización anidada, configurar opciones de memoria, adaptadores de red, discos duros virtuales y elija si desea instalar un sistema operativo desde un archivo de imagen .iso o desde la red.
7. Haga clic en **Crear** para crear la máquina virtual.
8. Una vez que la máquina virtual se crea y aparece en la lista de máquinas virtuales, puede iniciar la máquina virtual.
9. Una vez que se inicia la máquina virtual, puede conectarse a la consola de la máquina virtual mediante VMConnect para instalar el sistema operativo. Seleccione la máquina virtual en la lista, haga clic en **más** > **Connect** para descargar el archivo RDP. Abra el archivo .rdp en la aplicación de conexión a Escritorio remoto. Puesto que este se conecta a la consola de la máquina virtual, deberá especificar las credenciales de administrador del host de Hyper-V.

[Más información sobre la administración de máquinas virtuales con Windows Admin Center](manage-virtual-machines.md).

### <a name="pause-and-safely-restart-a-server"></a>Pausar y reiniciar un servidor de forma segura

1. Desde el **panel**, seleccione **servidores** desde el panel de navegación a la izquierda o haga clic en el **ver servidores >** vínculo en el icono en la esquina inferior derecha del panel .
2. En la parte superior, cambiar de **resumen** a la **inventario** ficha.
3. Seleccione un servidor, haga clic en su nombre para abrir el **Server** página de detalles.
4. Haga clic en **pausar el servidor para el mantenimiento**. Si es seguro continuar, esto moverá las máquinas virtuales a otros servidores del clúster. El servidor tendrá estado Purgando mientras se realiza. Si lo desea, puede ver las máquinas virtuales mover el **máquinas virtuales > inventario** página, donde se muestra claramente su servidor de host en la cuadrícula. Cuando se han movido todas las máquinas virtuales, el estado del servidor será **en pausa**.
5. Haga clic en **Administrar servidor** para tener acceso a todas las herramientas de administración de cada servidor en Windows Admin Center.
6. Haga clic en **reiniciar**, a continuación, **Sí**. Le expulsado volver a la lista de conexiones.
7. En el **panel**, el servidor está de color rojo mientras está inactivo.
8. Una vez que se copia de seguridad, vaya de nuevo el **Server** página y haga clic en **server reanudación de mantenimiento** para establecer el estado del servidor a simplemente arriba. En el tiempo, las máquinas virtuales se moverá: se requiere ninguna acción del usuario.

### <a name="replace-a-failed-drive"></a>Reemplazar una unidad con errores

1. Cuando falla una unidad, aparece una alerta en la parte superior izquierda **alertas** área de la **panel**.
2. También puede seleccionar **unidades** desde el panel de navegación del lado izquierdo o haga clic en el **unidades de vista >** vínculo en el icono en la esquina inferior derecha para examinar las unidades y ver su estado por sí mismo. En el **inventario** ficha, la cuadrícula admite la ordenación, agrupación y búsqueda de palabra clave.
3. Desde el **panel**, haga clic en la alerta para ver los detalles, como la ubicación física de la unidad.
4. Para obtener más información, haga clic en el **ir a la unidad** acceso directo a la **unidad** página de detalles.
5. Si el hardware lo admite, haga clic en **activar luz activado/desactivado** para controlar la luz del indicador de la unidad.
6. Espacios de almacenamiento directo automáticamente retira y evacua unidades con errores. Cuando esto sucede, se retira el estado de la unidad y la barra de capacidad de almacenamiento está vacía.
7. Quite la unidad con errores e inserte su reemplazo.
8. En **unidades > inventario**, aparecerá la nueva unidad. En el tiempo, se borrará la alerta, reparación volúmenes al estado correcto y reequilibra el almacenamiento en la nueva unidad: se requiere ninguna acción del usuario.

### <a name="manage-virtual-networks-sdn-enabled-hci-clusters-using-windows-admin-center-preview"></a>Administración de redes virtuales (clústeres HCI basadas en SDN con Windows Admin Center Preview)

1. Seleccione **redes virtuales** desde el panel de navegación del lado izquierdo.
2. Haga clic en **New** para crear una nueva red virtual y subredes, o elegir una red virtual existente y haga clic en **configuración** para modificar su configuración.
3. Haga clic en una red virtual existente para ver las conexiones de la máquina virtual a las subredes de red virtual y tener acceso a listas de control que se aplica a las subredes de red virtual.

![Administración de redes virtuales](../media/manage-hyper-converged/manage-virtual-networks.png)

### <a name="connect-a-virtual-machine-to-a-virtual-network-sdn-enabled-hci-clusters-using-windows-admin-center-preview"></a>Conectar una máquina virtual a una red virtual (clústeres HCI basadas en SDN con Windows Admin Center Preview)

1. Seleccione **máquinas virtuales** desde el panel de navegación del lado izquierdo.
2. Elija una máquina virtual existente > haga clic en **configuración** > Abra el **redes** pestaña **configuración**.
3. Configurar la **red Virtual** y **subred Virtual** campos para conectar la máquina virtual a una red virtual.

También puede configurar la red virtual al crear una máquina virtual.

![Conectar una máquina virtual a una red virtual](../media/manage-hyper-converged/connect-vm-to-virtual-network.png)

### <a name="monitor-software-defined-networking-infrastructure-sdn-enabled-hci-clusters-using-windows-admin-center-preview"></a>Supervisar la infraestructura de redes definidas por Software (clústeres HCI basadas en SDN con Windows Admin Center Preview)

1. Seleccione **SDN supervisión** desde el panel de navegación del lado izquierdo.
2. Ver información detallada sobre el estado de la puerta de enlace Virtual de controladora de red, equilibrador de carga de Software y supervisar el uso de grupo de servidores de puerta de enlace Virtual, público y grupo de direcciones IP privadas y el estado del host SDN.

![Supervisar la infraestructura de SDN](../media/manage-hyper-converged/sdn-monitoring.png)

## <a name="feedback"></a>Comentarios

¡Es toda la información sobre sus comentarios! La ventaja más importante de las actualizaciones frecuentes es saber qué funciona y qué hay que mejorar. Estas son algunas formas para hacernos saber lo que está pensando:

- [Envíe y vote para solicitudes de características en UserVoice](https://windowsserver.uservoice.com/forums/295071/category/319162?query=%5Bhci%5D)
- [Únase al foro de Windows Admin Center en la comunidad tecnológica de Microsoft](https://techcommunity.microsoft.com/t5/Windows-Server-Management/bd-p/WindowsServerManagement)
- Para un tweet `@servermgmt`

### <a name="see-also"></a>Vea también

- [Windows Admin Center](../understand/windows-admin-center.md)
- [Espacios de almacenamiento directo](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview)
- [Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-on-windows-server)
- [Redes definidas por software](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking)
