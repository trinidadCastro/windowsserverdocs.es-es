---
title: Administrar la infraestructura Hiperconvergida con Windows Admin Center
description: Administrar la infraestructura Hiperconvergida con Windows Admin Center (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 03/01/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 9ce4381735746ace6358aa2cb30f8b341c576054
ms.sourcegitcommit: e558dda2774345e9ad17ff04b759f68c59d88139
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2019
ms.locfileid: "9262848"
---
# Administrar la infraestructura Hiperconvergida con Windows Admin Center

>Se aplica a: Windows Admin Center, Versión preliminar de Windows Admin Center

## ¿Qué es la infraestructura hiperconvergida

Infraestructura Hiperconvergida consolida definido por software cálculo, almacenamiento y redes en un clúster para ofrecer alto rendimiento, rentable y la virtualización fácilmente escalable. Esta funcionalidad se introdujo en Windows Server 2016 con [Espacios de almacenamiento directo](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview), [Redes definidas por Software](https://docs.microsoft.com/en-us/windows-server/networking/sdn/software-defined-networking) y [Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-on-windows-server).

> [!Tip]
> ¿Buscas para adquirir infraestructura hiperconvergida? Microsoft recomienda estas soluciones [Definidas por Software de Windows Server](https://microsoft.com/wssd) de nuestros socios. Son diseñados, ensamblados y validan con nuestra arquitectura de referencia para garantizar la compatibilidad y confiabilidad, por lo que obtienes y ejecutar rápidamente.

> [!IMPORTANT]
> Algunas de las características descritas en este artículo solo están disponibles en el centro de administración de Windows. [¿Cómo se puede obtener esta versión?](http://aka.ms/windowsadmincenter)

## Qué es Windows Admin Center

[Windows Admin Center](../understand/windows-admin-center.md) es la herramienta de administración de próxima generación para Windows Server, el sucesor de las herramientas de "en el equipo" tradicionales, como el administrador del servidor. Es gratuito y se pueden instalar y utilizar sin una conexión a Internet. Puedes usar Windows Admin Center para administrar y supervisar la infraestructura hiperconvergida ejecuta Windows Server 2016 o Windows Server 2019.

![Panel de clústeres hiperconvergidos](../media/manage-hyper-converged/hci-dashboard-v1809.png)

## Características clave

Aspectos destacados del centro de administración de Windows de la infraestructura hiperconvergida incluyen:

- **Unificada solo-de consola para cálculo, almacenamiento y redes pronto.** Permite ver las máquinas virtuales, los servidores host, volúmenes, unidades y más dentro de una experiencia diseñado específicamente, coherente e interconectada.
- **Crear y administrar las máquinas virtuales de Hyper-V y espacios de almacenamiento.** Flujos de trabajo radicalmente simples para crear, abrir, cambiar el tamaño y eliminar volúmenes; crear, iniciar, conectarse a y mover las máquinas virtuales; y mucho más.
- **Supervisión de todo el clúster eficaz.** El panel de gráficos de memoria y el uso de CPU, capacidad de almacenamiento, e/s por segundo, rendimiento y latencia en tiempo real, a través de todos los servidores del clúster, con alertas claras cuando algo no es correcto.
- **Compatibilidad con redes definidas por (SDN) de software.** Administrar y supervisar redes virtuales, subredes, conectar máquinas virtuales a redes virtuales y supervisar la infraestructura SDN.

Windows Admin Center para infraestructura hiperconvergida se está desarrollando activamente por Microsoft. Recibe actualizaciones frecuentes que mejoran las características existentes y agregan nuevas características.

## Antes de empezar

Para administrar el clúster como infraestructura hiperconvergida en Windows Admin Center, debe estar ejecutando Windows Server 2016 o Windows Server 2019 y han habilitado Hyper-V y espacios de almacenamiento directo. Opcionalmente, también puede tener redes definidas por Software habilitado y administra a través de Windows Admin Center.

> [!Tip]
> Windows Admin Center también ofrece una administración de propósito general de la experiencia para los clústeres con compatibilidad con cualquier carga de trabajo, disponible para Windows Server 2012 y versiones posteriores. Si parece que se ajusten mejor, al agregar el clúster a Windows Admin Center, selecciona el [**Clúster de conmutación por error**](manage-failover-clusters.md) en lugar de **Clúster hiperconvergido**.

### Preparar el clúster de Windows Server 2016 para Windows Admin Center

Windows Admin Center para infraestructura hiperconvergida depende de la administración de que API agregadas después del lanzamiento de Windows Server 2016. Antes de poder administrar el clúster de Windows Server 2016 con Windows Admin Center, tendrás que realizar estos dos pasos:

1. Comprobar que todos los servidores del clúster han instalado la [actualización acumulativa para Windows Server 2016 (KB4103723) de 2018-05](https://support.microsoft.com/help/4103723/windows-10-update-kb4103723) o posterior. Para descargar e instalar esta actualización, ve a **configuración** > **Actualizar & seguridad** > selecciona **comprobación en línea para las actualizaciones de Microsoft Update**y**Windows Update** .
2. Ejecuta el siguiente cmdlet de PowerShell como administrador en el clúster:

```powershell
    Add-ClusterResourceType -Name "SDDC Management" -dll "$env:SystemRoot\Cluster\sddcres.dll" -DisplayName "SDDC Management"
```

> [!Tip]
> Solo debes ejecutar el cmdlet una vez, en cualquier servidor del clúster. Puedes ejecutarlo localmente en Windows PowerShell o usar el proveedor de servicio de seguridad de credenciales (CredSSP) para ejecutar de forma remota. Según la configuración, es posible que no podrás ejecutar este cmdlet desde dentro de Windows Admin Center.

### Preparar el clúster de Windows Server 2019 para Windows Admin Center

Si el clúster ejecuta Windows Server 2019, los pasos anteriores no son necesarios. Agregar solo el clúster a Windows Admin Center como se describe en la siguiente sección y estás listo!

### Configurar definido por Software de redes (opcional) ###

Puedes configurar la infraestructura hiperconvergida ejecuta Windows Server 2016 o 2019 usar redes definidas por Software (SDN) con los siguientes pasos:

1. Preparar el disco duro virtual del sistema operativo que es el mismo sistema operativo ha instalado en los hosts de infraestructura hiperconvergida. Este VHD se usará para todas las máquinas virtuales de CN, SLB/GW.
2. Descargar la carpeta y los archivos de SDN Express desde [https://github.com/Microsoft/SDN/tree/master/SDNExpress](https://github.com/Microsoft/SDN/tree/master/SDNExpress).
3. Preparar una máquina virtual diferentes mediante la consola de implementación. Esta máquina virtual debe ser capaz de obtener acceso a los hosts SDN. Además, la máquina virtual debe tener instalada la herramienta de Hyper-V de RSAT.
4. Copia todo el contenido descargado para SDN Express a la consola de deployment máquina virtual. Y compartir esta carpeta **SDNExpress** . Asegúrese de que todos los hosts pueden acceder a la carpeta compartida **SDNExpress** , como se define en la línea del archivo de configuración 8:
```
    \\$env:Computername\SDNExpress
```
5. Copie el VHD del sistema operativo en la carpeta **imágenes** en la carpeta **SDNExpress** en la consola de deployment máquina virtual.
6. Modificar la configuración rápida de SDN con la configuración del entorno. Finalizar los dos pasos siguientes después de modificar la configuración rápida de SDN en función de la información de su entorno.
7. Ejecuta PowerShell con privilegios de administrador para implementar SDN:

```powershell
    .\SDNExpress.ps1 -ConfigurationDataFile .\your_fabricconfig.PSD1 -verbose 
```

La implementación te llevará unos 30 y 45 minutos.

## Comenzar

Una vez que se implementa la infraestructura hiperconvergida, puedes administrar con Windows Admin Center.

### Instalar Windows Admin Center

Si no lo has hecho ya, descargar e instalar Windows Admin Center. Lo más rápido manera de obtener y ejecución es instalarlo en el equipo de Windows 10 y administrar los servidores de forma remota. Lleva menos de cinco minutos. [Descargar ahora](https://aka.ms/windowsadmincenter) u [obtener más información acerca de otras opciones de instalación](../deploy/install.md).

### Agregar clúster Hiperconvergido

Para agregar el clúster a Windows Admin Center:

1. Haga clic en **+ Agregar** todas las conexiones.
2. Elegir esta opción para agregar una **Conexión de clúster hiperconvergida**.
3. Escribe el nombre del clúster y, si se te solicite, las credenciales que se usan.
4. Haz clic en **Agregar** para finalizar.

El clúster se agregará a tu lista de conexiones. Haz clic en él para iniciar el panel.

![Agregar conexión de clústeres](../media/manage-hyper-converged/add-hyper-converged-cluster-connection.gif)

### Agregar habilitado SDN clústeres (versión preliminar de Windows Admin Center)

La versión preliminar más reciente de Windows Admin Center es compatible con administración de redes definidas por Software para infraestructura hiperconvergida. Al agregar un URI de REST de controlador de red para la conexión de clústeres Hiperconvergidos, puedes usar el Administrador de clústeres hiperconvergidos para administrar los recursos SDN y supervisar la infraestructura SDN.

1. Haga clic en **+ Agregar** todas las conexiones.
2. Elegir esta opción para agregar una **Conexión de clúster hiperconvergida**.
3. Escribe el nombre del clúster y, si se te solicite, las credenciales que se usan.
4. Comprueba **la controladora de red de configurar** para continuar.
5. Escribe el **URI de controlador de red** y haga clic en **Validar**.
6. Haz clic en **Agregar** para finalizar.

El clúster se agregará a tu lista de conexiones. Haz clic en él para iniciar el panel.

![Agregar conexión habilitada SDN clústeres](../media/manage-hyper-converged/add-snd-enabled-hci-connection.png)

> [!Important]
> Entornos de SDN con la autenticación Kerberos para la comunicación Northbound no se admiten actualmente.

## Preguntas frecuentes

### ¿Existen diferencias entre la administración de Windows Server 2016 y Windows Server 2019?

Sí. Windows Admin Center para infraestructura hiperconvergida recibe actualizaciones frecuentes que mejoran la experiencia de Windows Server 2016 y Windows Server 2019. Sin embargo, algunas características nuevas solo están disponibles para Windows Server 2019: por ejemplo, el modificador para alternar para desduplicación y compresión.

### ¿Puedo usar Windows Admin Center para administrar espacios de almacenamiento directo para otros casos de uso (no hiperconvergida), por ejemplo, el servidor de archivos de escalabilidad horizontal (SoFS) o Microsoft SQL Server?

Windows Admin Center para infraestructura hiperconvergida no proporciona administración o las opciones de supervisión específicamente para otros casos de uso de espacios de almacenamiento directo: por ejemplo, no puede crear recursos compartidos de archivos. Sin embargo, las características de panel y core, como crear volúmenes o reemplazar las unidades, funcionan para los clústeres con espacios de almacenamiento directo.

### ¿Qué es la diferencia entre un clúster de conmutación por error y un clúster hiperconvergido?

En general, el término "hiperconvergida" se refiere a ejecutar Hyper-V y espacios de almacenamiento directo en el mismo clúster de servidores para virtualizar recursos de almacenamiento y cálculo. En el contexto de Windows Admin Center, al hacer clic en **+ Agregar** en la lista de conexiones, puedes elegir entre la adición de una **conexión de clúster de conmutación por error** o una **conexión de clúster hiperconvergido**:

- La **conexión de clúster de conmutación por error** es el sucesor de la aplicación de escritorio de administrador de clústeres de conmutación por error. Proporciona una experiencia de administración de propósito general, familiar para los clústeres con compatibilidad con cualquier carga de trabajo, incluyendo Microsoft SQL Server. Es disponibles para Windows Server 2012 y versiones posteriores.

- La **conexión de clúster hiperconvergido** es una experiencia completamente nueva adaptada para espacios de almacenamiento directo y Hyper-V. Presenta el Panel y enfatiza gráficos y alertas para la supervisión. Está disponible para Windows Server 2016 y Windows Server 2019.

### ¿Por qué se necesita la última actualización acumulativa para Windows Server 2016?

Windows Admin Center para infraestructura hiperconvergida depende de la administración de que API desarrolladas desde el lanzamiento de Windows Server 2016. Estas API se agregan en la [actualización acumulativa para Windows Server 2016 (KB4103723) de 2018-05](https://support.microsoft.com/help/4103723/windows-10-update-kb4103723), disponible a partir del 8 de mayo de 2018.

### ¿Cuánto cuesta usar Windows Admin Center?

Windows Admin Center no tiene ningún coste adicional más allá de Windows.

Puedes usar Windows Admin Center (disponible como una descarga independiente) con licencias válidas de Windows Server o Windows 10 sin ningún coste adicional: se cede bajo licencia en un EULA complementario de Windows.

### ¿Necesita Windows Admin Center System Center?

No.

### ¿Requiere una conexión a Internet?

No.

Aunque Windows Admin Center ofrece eficaces y la integración cómoda con la nube de Microsoft Azure, la administración de core y la experiencia de supervisión de una infraestructura hiperconvergida es completamente local. Se puede instalar y utiliza sin una conexión a Internet.

## Cosas que debe probar

Si estás acaba de empezar, estos son algunos tutoriales rápidos que te ayudarán a obtener información sobre cómo Windows Admin Center para infraestructura hiperconvergida se organiza y funciona. Por favor, juzgar buena y Ten cuidado con los entornos de producción. Estos vídeos se grabaron con Windows Admin Center, versión 1804 y una compilación de Insider Preview de Windows Server 2019.

### Administrar volúmenes espacios de almacenamiento directo

<ul>
               <li>(0:37) <a href="https://youtu.be/o66etKq70N8">cómo crear un volumen de reflejo triple</a></li>
               <li>(1:17) <a href="https://youtu.be/R72QHudqWpE">cómo crear un volumen de paridad acelerada por reflejos</a></li>
               <li>(1:02) <a href="https://youtu.be/j59z7ulohs4">cómo abrir un volumen y agregar archivos</a></li>
               <li>(0:51) <a href="https://youtu.be/PRibTacyKko">cómo activar desduplicación y compresión</a></li>
               <li>(0:47) <a href="https://youtu.be/hqyBzipBoTI">cómo ampliar un volumen</a></li>
               <li>(0:26) <a href="https://youtu.be/DbjF8r2F6Jo">cómo eliminar un volumen</a></li>
</ul>

<table>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Crear el volumen de reflejo triple</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/o66etKq70N8" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>Crear el volumen de paridad acelerada por reflejos</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/R72QHudqWpE" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Abre el volumen y agregar archivos</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/j59z7ulohs4" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>Activar la desduplicación y compresión</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/PRibTacyKko" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Ampliar volúmenes</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/hqyBzipBoTI" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>Eliminar volumen</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/DbjF8r2F6Jo" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
</table>

### Crear una nueva máquina virtual

1. Haz clic en la herramienta de **máquinas virtuales** desde el panel de navegación izquierda.
2. En la parte superior de la herramienta de máquinas virtuales, elige la pestaña de **inventario** y después haz clic en **nuevo** para crear una nueva máquina virtual.
3. Escribe el nombre de máquina virtual y elegir entre máquinas virtuales de generación 1 y 2.
4. Comentada, a continuación, puede elegir qué host para crear la máquina virtual en inicialmente o usar la host recomendado.
5. Elegir una ruta de acceso para los archivos de la máquina virtual. Elegir un volumen en la lista desplegable o haga clic en **Examinar** para seleccionar una carpeta con el selector de carpetas. Los archivos de configuración de máquina virtual y el archivo de disco duro virtual se guardará en una sola carpeta en el `\Hyper-V\[virtual machine name]` ruta de acceso de la ruta de acceso o el volumen seleccionado.
6. Elegir el número de procesadores virtuales, si quieres que la virtualización anidada habilitada, configurar la configuración de la memoria, adaptadores de red, los discos duros virtuales y elegir si quieres instalar un sistema operativo desde un archivo de imagen .iso o desde la red.
7. Haz clic en **crear** para crear la máquina virtual.
8. Una vez que la máquina virtual se crea y aparece en la lista de máquinas virtuales, puede iniciar la máquina virtual.
9. Una vez que se inicia la máquina virtual, puede conectarse a la consola de la máquina virtual mediante VMConnect para instalar el sistema operativo. Seleccione la máquina virtual en la lista, haz clic en **más** > **Connect** para descargar el archivo RDP. Abre el archivo RDP en la aplicación de la conexión a Escritorio remoto. Dado que esto se conecta a la consola de la máquina virtual, tendrás que escribir las credenciales de administrador del host de Hyper-V.

[Obtenga más información sobre la administración de máquinas virtuales con Windows Admin Center](manage-virtual-machines.md).

### Pausar y reiniciar un servidor de forma segura

1. En el **panel**, selecciona **los servidores** en la barra de navegación en el lado izquierdo o haciendo clic en el vínculo **Ver servidores >** en el icono en la esquina inferior derecha del panel.
2. En la parte superior, cambiar de **Resumen** a la pestaña de **inventario** .
3. Seleccionar un servidor haciendo clic en su nombre para abrir la página de detalles del **servidor** .
4. Haz clic en el **servidor de pausa para el mantenimiento**. Si es seguro continuar, así moverá las máquinas virtuales a otros servidores del clúster. El servidor tendrá estado agoten mientras esto sucede. Si quieres, puedes ver las máquinas virtuales mover en la página de **máquinas virtuales > inventario** , donde se muestra claramente su servidor host en la cuadrícula. Cuando se han movido todas las máquinas virtuales, el estado del servidor estará **en pausa**.
5. Haz clic en **el servidor de administración** para tener acceso a todas las herramientas de administración de cada servidor en Windows Admin Center.
6. Haz clic en **reiniciar**, a continuación, el **Sí**. Tendrás inició volver a la lista de conexiones.
7. Vuelve a activar el **panel**, el servidor está en rojo mientras esté hacia abajo.
8. Una vez que se copia de seguridad, vuelva a explorar la página de **servidor** y haz clic en **servidor de reanudación de mantenimiento** para establecer el estado del servidor para su simplemente arriba. En el tiempo, las máquinas virtuales se moverán atrás: se requiere ninguna acción del usuario.

### Reemplazar una unidad con errores

1. Cuando se produce un error, aparecerá una alerta en el área de **alertas** superior izquierda del **panel**.
2. También puedes seleccionar **unidades** en la barra de navegación en el lado izquierdo o haz clic en el vínculo **> unidades de vista** en el icono en la esquina inferior derecha para examinar unidades y ver su estado por sí mismo. En la pestaña de **inventario** , la cuadrícula admite ordenación, agrupación y búsqueda de palabra clave.
3. En el **panel**, haz clic en la alerta para ver detalles, como la ubicación física de la unidad.
4. Para obtener más información, haz clic en el acceso directo de **Ir a la unidad** a la página de detalles de la **unidad** .
5. Si el hardware lo admite, puedes hacer clic en **activar la luz activar/desactivar** para controlar la luz del indicador de la unidad.
6. Espacios de almacenamiento directo automáticamente retira y saca las unidades con error. Cuando esto ha ocurrido, se retirará el estado de la unidad y su barra de la capacidad de almacenamiento está vacía.
7. Quitar la unidad con error e insertar su reemplazo.
8. En **las unidades > inventario**, aparecerá la nueva unidad. En el tiempo, se borrará la alerta, volúmenes reparará vuelve al estado correcto y se equilibrar el almacenamiento en la nueva unidad: se requiere ninguna acción del usuario.

### Administración de redes virtuales (clústeres HCI SDN habilitado con la versión preliminar de Windows Admin Center)

1. Selecciona **Las redes virtuales** de la navegación en el lado izquierdo.
2. Haz clic en **nuevo** para crear una nueva red virtual y subredes, o elige una red virtual existente y haz clic en la **configuración** para modificar su configuración.
3. Haz clic en una red virtual existente para ver las conexiones de máquina virtual a las subredes de red virtual y aplicadas a subredes de red virtual de listas de control de acceso.

![Administración de redes virtuales](../media/manage-hyper-converged/manage-virtual-networks.png)

### Conectar una máquina virtual a una red virtual (clústeres HCI SDN habilitado con la versión preliminar de Windows Admin Center)

1. Selecciona **las máquinas virtuales** de la navegación en el lado izquierdo.
2. Elige un > de máquina virtual existente haga clic en **configuración** > abre la pestaña de **redes** en la **configuración**.
3. Configurar los campos de **Red Virtual** y **Subred Virtual** para conectar la máquina virtual a una red virtual.

También puedes configurar la red virtual al crear una máquina virtual.

![Conectar una máquina virtual a una red virtual](../media/manage-hyper-converged/connect-vm-to-virtual-network.png)

### Infraestructura de redes definidas por Software de monitor (clústeres HCI SDN habilitado con la versión preliminar de Windows Admin Center)

1. En la barra de navegación en el lado izquierdo, selecciona la **Supervisión de SDN** .
2. Permite ver información detallada sobre el estado de la controladora de red, equilibrador de carga de Software, la puerta de enlace Virtual y supervisar el uso de Virtual puerta de enlace de grupo, pública y privada IP y el estado del host SDN.

![Infraestructura SDN de Monitor](../media/manage-hyper-converged/sdn-monitoring.png)

## Comentarios

¡Se trata de todos tus comentarios! La ventaja más importante de actualizaciones frecuentes es escuchar lo que funciona y lo que se debe mejorarse. Estas son algunas maneras para informarnos de lo que está pensando:

- [Enviar y votar para las solicitudes de características en UserVoice](https://windowsserver.uservoice.com/forums/295071/category/319162?query=%5Bhci%5D)
- [Unir el foro de Windows Admin Center en la comunidad tecnológica Microsoft](https://techcommunity.microsoft.com/t5/Windows-Server-Management/bd-p/WindowsServerManagement)
- Tweet a `@servermgmt`

### Consulta también

- [Windows Admin Center](../understand/windows-admin-center.md)
- [Espacios de almacenamiento directo](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview)
- [Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-on-windows-server)
- [Redes definidas por software](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking)
