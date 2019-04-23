---
title: Información general de Espacios de almacenamiento directos
ms.prod: windows-server-threshold
ms.author: cosdar
ms.manager: dongill
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 03/06/2019
ms.assetid: 8bd0d09a-0421-40a4-b752-40ecb5350ffd
description: Información general de espacios de almacenamiento directo, una característica de Windows Server que permite los servidores en clúster con almacenamiento interno en una solución de almacenamiento definidas mediante software.
ms.localizationpriority: medium
ms.openlocfilehash: 25de20b398f780f5da07b6b6cf4d396a7d12204a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823896"
---
# <a name="storage-spaces-direct-overview"></a>Información general de Espacios de almacenamiento directos

>Se aplica a: Windows Server 2019, Windows Server 2016

Espacios de almacenamiento directo usa servidores estándar del sector con unidades conectadas localmente para crear almacenamiento definido por software de alta disponibilidad y escalabilidad por un porcentaje mínimo del costo de las matrices de SAN o NAS tradicionales. Su arquitectura convergente o hiperconvergida simplifica radicalmente la adquisición y la implementación, aunque características como almacenamiento en caché, los niveles de almacenamiento y codificación de borrado, junto con las últimas innovaciones de hardware, como las redes RDMA y unidades de NVMe, entregar sin parangón de eficiencia y el rendimiento.

Espacios de almacenamiento directo se incluye en Windows Server 2019 Datacenter, Windows Server 2016 Datacenter, y [Windows Server Insider Preview Builds](https://insider.windows.com/for-business-getting-started-server/). 

Para otras aplicaciones de espacios de almacenamiento, como clústeres de SAS compartidos y servidores independientes, consulte [Introducción a los espacios de almacenamiento](overview.md). Si está buscando información sobre uso de espacios de almacenamiento en un equipo con Windows 10, consulte [espacios de almacenamiento en Windows 10](https://support.microsoft.com/help/12438/windows-10-storage-spaces).

<table>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Comprender</a></strong>
            <ul>
              <li>Información general (estás aquí)</li>
              <li><a href="understand-the-cache.md">Comprender la memoria caché</a></li>
              <li><a href="storage-spaces-fault-tolerance.md">Eficacia de almacenamiento y la tolerancia a errores</a></li>
              <li><a href="drive-symmetry-considerations.md">Consideraciones de simetría de unidad</a></li>
              <li><a href="understand-storage-resync.md">Entender y supervisar la resincronización de almacenamiento</a></li>
              <li><a href="understand-quorum.md">Quórum de clúster y grupo de descripción</a></li>
              <li><a href="cluster-sets.md">Conjuntos de clústeres</a></li>
            </ul>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>Plan</a></strong>
            <ul>
              <li><a href="storage-spaces-direct-hardware-requirements.md">Requisitos de hardware</a></li>
              <li><a href="csv-cache.md">Uso de los CSV en memoria memoria caché de lectura</li>
              <li><a href="choosing-drives.md">Elija las unidades</a></li>
              <li><a href="plan-volumes.md">Plan de volúmenes</a></li>
              <li><a href="storage-spaces-direct-in-vm.md">Usar clústeres invitados de máquina virtual</a></li>
              <li><a href="storage-spaces-direct-disaster-recovery.md">Recuperación ante desastres</a></li>
            </ul>
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Implementar</a></strong>
            <ul>
                    <li><a href="deploy-storage-spaces-direct.md">Implementar espacios de almacenamiento directo</a></li>
                    <li><a href="create-volumes.md">Creación de volúmenes</a></li>
              <li><a href="nested-resiliency.md">Resistencia anidado</a></li>
              <li><a href="../../failover-clustering/manage-cluster-quorum.md">Configuración de quórum</a></li>
              <li><a href="upgrade-storage-spaces-direct-to-windows-server-2019.md">Actualizar un clúster de espacios de almacenamiento directo en Windows Server 2019</a></li>
            </ul>
        </td>        
        <td style="padding: 5px; border: 0;">
            <strong>administrar</a></strong>
            <ul>
              <li><a href="../../manage/windows-admin-center/use/manage-hyper-converged.md">Administrar con Windows Admin Center</a></li>
              <li><a href="add-nodes.md">Agregar servidores o unidades de disco</a></li>
              <li><a href="maintain-servers.md">Poner un servidor sin conexión para el mantenimiento</li>
              <li><a href="remove-servers.md">Quitar servidores</a></li>
              <li><a href="resize-volumes.md">Extender volúmenes</a></li>
              <li><a href="../update-firmware.md">Actualizar el firmware de unidad</a></li>
              <li><a href="performance-history.md">Historial de rendimiento</a></li>
              <li><a href="delimit-volume-allocation.md">Delimitar la asignación de volúmenes</a></li>
            </ul>
        </td>
    </tr>
    <tr style="border: 0;">
         <td style="padding: 5px; border: 0;">
            <strong>Solución de problemas</a></strong>
            <ul>
              <li><a href="storage-spaces-states.md">Solución de problemas de salud y estados operativos</a></li>
              <li><a href="data-collection.md">Recopilar datos de diagnóstico con espacios de almacenamiento directo</a></li>
            </ul>
         <td style="padding: 5px; border: 0;">
            <strong>Publicaciones recientes en blogs</a></strong>
            <ul>
              <li><a href="https://blogs.technet.microsoft.com/filecab/2018/10/30/windows-server-2019-and-intel-optane-dc-persistent-memory/">13.7 millones de IOPS con espacios de almacenamiento directo: el nuevo registro de la industria para infraestructuras hiperconvergidas</a></li>
              <li><a href="https://blogs.technet.microsoft.com/filecab/2018/10/02/hci-the-countdown-clock-starts-now/">Infraestructura hiperconvergida en Windows Server 2019 - el reloj de cuenta regresiva empieza ahora!</a></li>
              <li><a href="https://blogs.technet.microsoft.com/filecab/2018/06/27/windows-server-summit-recap/">Cinco big anuncios de Windows Server Summit</a></li>
              <li><a href="https://blogs.technet.microsoft.com/filecab/2018/03/27/storage-spaces-direct-momentum/">10 000 clústeres de espacios de almacenamiento directo y el recuento...</a></li>
            </ul>
</table>

## <a name="videos"></a>Vídeos

**Introducción rápida de vídeo (5 minutos)**

<iframe src="https://www.youtube-nocookie.com/embed/raeUiNtMk0E" width="560" height="315" allowfullscreen></iframe>

**Espacios de almacenamiento directo en Microsoft Ignite 2018 (1 hora)**

[Ver en YouTube](https://www.youtube.com/watch?v=5kaUiW3qo30)

**Espacios de almacenamiento directo en Microsoft Ignite 2017 (1 hora)**

[Ver en YouTube](https://www.youtube.com/watch?v=YDr2sqNB-3c)

**Iniciar el evento en Microsoft Ignite 2016 (1 hora)**

[Ver en YouTube](https://www.youtube.com/watch?v=-LK2ViRGbWs)

## <a name="key-benefits"></a>Principales ventajas

<table>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:100px">
            <img src="media/storage-spaces-direct-in-windows-server-2016/simplicity-icon.png" width="100" alt="">
        </td>
        <td style="padding: 10px; border: 0;">
            <b>Simplicidad.</b> Pase de servidores estándar del sector que ejecutan Windows Server 2016 a su primer clúster de Espacios de almacenamiento directo en menos de 15 minutos. En el caso de los usuarios de System Center, la implementación consiste en una simple casilla.
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:100px">
            <img src="media/storage-spaces-direct-in-windows-server-2016/performance-icon.png" width="100" alt="">
        </td>
        <td style="padding: 10px; border: 0;">
            <b>Rendimiento inigualable.</b> Tanto si son de memoria flash como híbridas, Espacios de almacenamiento directo supera fácilmente las <a href="https://blogs.technet.microsoft.com/filecab/2016/07/26/storage-iops-update-with-storage-spaces-direct/">150 000 E/S por segundo aleatorias de 4K mixtas por servidor</a> con una latencia baja constante gracias a su arquitectura incrustada de hipervisor, su caché de lectura y escritura integrada y su compatibilidad con las unidades de NVMe de vanguardia montadas directamente en el bus PCIe.
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:100px">
            <img src="media/storage-spaces-direct-in-windows-server-2016/fault-tolerance-icon.png" width="100" alt="">
        </td>
        <td style="padding: 10px; border: 0;">
            <b>Tolerancia a errores. </b> Resistencia integrada controla los errores de componentes, servidor o unidad con disponibilidad continua. También es posible configurar implementaciones más grandes para la <a href="../../failover-clustering/fault-domains.md">tolerancia a errores de chasis y bastidor</a>. Cuando se produce un error de hardware, cámbielo. El software se repara automáticamente, sin pasos de administración complicados.
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:100px">
            <img src="media/storage-spaces-direct-in-windows-server-2016/efficiency-icon.png" width="100" alt="">
        </td>
        <td style="padding: 10px; border: 0;">
            <b>Eficacia de los recursos.</b> La codificación de borrado ofrece una eficiencia de almacenamiento hasta 2,4 veces mayor, con innovaciones exclusivas, como códigos de reconstrucción local y capas ReFS en tiempo real, para ampliar estas mejoras a las unidades de disco duro y las cargas de trabajo mixtas en frío y en caliente, al mismo tiempo que reduce al mínimo el consumo de CPU para asignar los recursos donde se necesitan más: las máquinas virtuales.
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:100px">
            <img src="media/storage-spaces-direct-in-windows-server-2016/manageability-icon.png" width="100" alt="">
        </td>
        <td style="padding: 10px; border: 0;">
            <b>Facilidad de uso.</b> Use los <a href="../storage-qos/storage-qos-overview.md">controles de QoS de almacenamiento</a> para comprobar las máquinas virtuales demasiado ocupadas con límites mínimos y máximos de E/S por segundo por máquina virtual. El <a href="../../failover-clustering/health-service-overview.md">Servicio de mantenimiento</a> proporciona continuamente alertas y supervisión integrada, mientras que las nuevas API facilitan la recopilación de métricas enriquecidas de rendimiento y capacidad de todo el clúster.
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:100px">
            <img src="media/storage-spaces-direct-in-windows-server-2016/scalability-icon.png" width="100" alt="">
        </td>
        <td style="padding: 10px; border: 0;">
            <b>Escalabilidad.</b> Usa hasta 16 servidores y más de 400 unidades para obtener varios hasta 1 petabyte (1000 terabytes) de almacenamiento por clúster. Para escalar horizontalmente, basta con agregar unidades o más servidores. Espacios de almacenamiento directo incorporará automáticamente las unidades nuevas y empezará a usarlas. La eficiencia de almacenamiento y el rendimiento mejoran de forma predecible a escala.
        </td>
    </tr>
</table>

## <a name="deployment-options"></a>Opciones de implementación

Espacios de almacenamiento directo está diseñado para dos opciones de implementación distintas:

### <a name="converged"></a>Convergido

**Almacenamiento y proceso en clústeres independientes.** La opción de implementación convergida, también denominada "desagregada", separa en capas un Servidor de archivos de escalabilidad horizontal (SoFS) encima de Espacios de almacenamiento directo para proporcionar almacenamiento conectado a la red a través de recursos compartidos de archivos SMB3. Esto permite escalar el proceso y la carga de trabajo independientemente del clúster de almacenamiento, lo que es esencial para implementaciones a gran escala, como IaaS (infraestructura como servicio) de Hyper-V para empresas y proveedores de servicios.

![Espacios de almacenamiento directo proporciona el almacenamiento mediante la característica Servidor de archivos de escalabilidad horizontal para las máquinas virtuales de Hyper-V que estén en otro servidor o clúster](media/storage-spaces-direct-in-windows-server-2016/converged-minimal.png)

### <a name="hyper-converged"></a>Hiperconvergido

**Un clúster de proceso y almacenamiento.** La opción de implementación hiperconvergida ejecuta máquinas virtuales de Hyper-V o bases de datos de SQL Server directamente en los servidores que proporcionan el almacenamiento y almacena sus archivos en los volúmenes locales. Esto elimina la necesidad de configurar los permisos y el acceso al servidor de archivos, y además reduce los costos de hardware para pequeñas y medianas empresas o implementaciones remotas de oficinas y sucursales. Consulte [implementar espacios de almacenamiento directo](deploy-storage-spaces-direct.md).

![Espacios de almacenamiento directo proporciona el almacenamiento para las máquinas virtuales de Hyper-V que estén en el mismo clúster](media/storage-spaces-direct-in-windows-server-2016/hyper-converged-minimal.png)

## <a name="how-it-works"></a>Cómo funciona

Espacios de almacenamiento directo es la evolución de Espacios de almacenamiento, introducido por primera vez en Windows Server 2012. Aprovecha muchas de las características que se conocen hoy en día en Windows Server, como los clústeres de conmutación por error, el sistema de archivos del Volumen compartido de clúster (CSV), el Bloque de mensajes del servidor (SMB) 3 y, por supuesto, Espacios de almacenamiento. También introduce tecnologías nuevas, en especial el bus de almacenamiento de software.

A continuación presentamos una visión general de la pila de Espacios de almacenamiento directo:

![Pila de Espacios de almacenamiento directo](media/storage-spaces-direct-in-windows-server-2016/converged-full-stack.png)

**Hardware de red.** Espacios de almacenamiento directo usa SMB3 (incluido SMB directo y SMB multicanal) sobre Ethernet para la comunicación entre los servidores. Le recomendamos encarecidamente que disponga de más de 10 GbE con acceso directo a memoria remota (RDMA), ya sea iWARP o RoCE.

**Hardware de almacenamiento.** De 2 a 16 servidores con unidades de SATA, SAS o NVMe conectadas localmente. Cada servidor debe tener al menos dos unidades de estado sólido y al menos cuatro unidades adicionales. Los dispositivos SATA y SAS deben estar detrás de un adaptador de bus host (HBA) y un ampliador SAS. Le recomendamos encarecidamente las plataformas meticulosamente diseñadas y ampliamente validadas de nuestros socios (próximamente).

**Agrupación en clústeres de conmutación por error.** La característica integrada de agrupación en clústeres de Windows Server se usa para conectar los servidores.

**Bus de almacenamiento de software.** El bus de almacenamiento de software es una novedad de Espacios de almacenamiento directo. Abarca el clúster y establece un tejido de almacenamiento definido por software gracias al cual todos los servidores pueden ver todas las unidades locales de los demás servidores. Considérelo una manera de reemplazar el cableado costoso y restrictivo de Canal de fibra o SAS compartido.

**Caché de capa de Bus de almacenamiento.** El Bus de almacenamiento de Software enlaza dinámicamente las unidades más rápidas presentes (por ejemplo, SSD) con unidades más lentas (por ejemplo, HDD) para proporcionar la caché de lectura/escritura del lado servidor que acelera la E/S y aumenta el rendimiento.

**Grupo de almacenamiento.** La colección de unidades que forman la base de Espacios de almacenamiento se denomina grupo de almacenamiento. Se crea automáticamente, y todas las unidades válidas se detectan y se agregan automáticamente. Te recomendamos encarecidamente que uses un grupo por cada clúster, con la configuración predeterminada. Lee nuestro blog sobre [profundización en el grupo de almacenamiento](https://blogs.technet.microsoft.com/filecab/2016/11/21/deep-dive-pool-in-spaces-direct/) para obtener más información.

**Espacios de almacenamiento.** Espacios de almacenamiento proporciona tolerancia a errores a "discos" virtuales mediante [la creación de reflejo, la codificación de borrado o ambas](storage-spaces-fault-tolerance.md). Considéralo un volumen RAID distribuido definido por software que usa las unidades del grupo. En Espacios de almacenamiento directo, estos discos virtuales suelen tener resistencia a dos errores de unidad o de servidor simultáneos (por ejemplo, creación de reflejo triple, con cada copia de datos en un servidor diferente) aunque la tolerancia a errores de chasis y bastidor también esté disponible.

**Sistema de archivos resistente (ReFS).** ReFS es un sistema de archivos Premier diseñado específicamente para la virtualización. Incluye aceleraciones considerables para operaciones de archivos .vhdx, como la creación, la expansión y la combinación de puntos de control, así como sumas de comprobación integradas para detectar y corregir errores de bit. También introduce niveles en tiempo real que hacen rotar los datos en tiempo real entre niveles de almacenamiento llamados "en frío" y "en caliente" en función del uso.

**Volúmenes compartidos de clúster.** El sistema de archivos CSV unifica todos los volúmenes de ReFS en un único espacio de nombres accesible a través de cualquier servidor, por lo que para cada servidor, cada volumen parece montado localmente y actúa como tal.

**Servidor de archivos de escalabilidad horizontal.** Esta capa final solo es necesaria para las implementaciones convergidas. Proporciona acceso a archivos remotos con el protocolo de acceso SMB3 a clientes (como otro clúster que ejecuta Hyper-V) a través de la red, con lo que efectivamente convierte Espacios de almacenamiento directo en un almacenamiento conectado a la red (NAS).

## <a name="customer-stories"></a>Testimonios de clientes

Hay [más de 10.000 clústeres](https://blogs.technet.microsoft.com/filecab/2018/03/27/storage-spaces-direct-momentum/) en todo el mundo que ejecuta espacios de almacenamiento directo. Las organizaciones de todos los tamaños, desde pequeñas empresas implementar solo dos nodos, en grandes empresas y gobiernos implementar cientos de nodos, dependen de espacios de almacenamiento directo de su infraestructura y aplicaciones críticas.

Visite [Microsoft.com/HCI](https://www.microsoft.com/hci) lea sus historias de:

[![Cuadrícula de logotipos de clientes](media/storage-spaces-direct-in-windows-server-2016/customer-stories.png)](https://www.microsoft.com/hci)

## <a name="management-tools"></a>Herramientas de administración

Las siguientes herramientas pueden usarse para administrar y supervisar espacios de almacenamiento directo:

| Name | ¿Gráficos o de línea de comandos? | ¿Incluida o de pago? |
|-----------------|----------------------------|-------------------|
| [Windows Admin Center](../../manage/windows-admin-center/overview.md)     | Gráfico    | Incluido |
| El administrador del servidor y Administrador de clústeres de conmutación por error                                 | Gráfico    | Incluido |
| Windows PowerShell                                                        | Línea de comandos | Incluido |
| [System Center Virtual Machine Manager (SCVMM)](https://technet.microsoft.com/system-center-docs/vmm/manage/manage-storage-spaces-direct-vmm) & [Operations Manager (SCOM)](https://www.microsoft.com/download/details.aspx?id=54700) | Gráfico    | De pago     |

## <a name="get-started"></a>Comenzar

Prueba Espacios de almacenamiento directo [en Microsoft Azure](https://blogs.technet.microsoft.com/filecab/2016/05/05/s2dazuretp5/), o descarga una copia de evaluación con licencia para 180 días de Windows Server desde [Evaluaciones de Windows Server](https://go.microsoft.com/fwlink/?linkid=842602).

## <a name="see-also"></a>Vea también

-   [Eficacia de almacenamiento y la tolerancia a errores](storage-spaces-fault-tolerance.md)
-   [Réplica de almacenamiento](../storage-replica/storage-replica-overview.md)
-   [Almacenamiento en el blog de Microsoft](https://blogs.technet.microsoft.com/filecab/)
-   [Storage Spaces Direct throughput with iWARP](https://blogs.technet.microsoft.com/filecab/2017/03/13/storage-spaces-direct-throughput-with-iwarp) (Rendimiento de Espacios de almacenamiento directo con iWARP) (blog de TechNet)
-   [Novedades de la conmutación por error en Windows Server](../../failover-clustering/whats-new-in-failover-clustering.md)  
-   [Calidad de servicio de almacenamiento](../storage-qos/storage-qos-overview.md)
-   [Windows TI soporte técnico profesional](https://www.microsoft.com/itpro/windows/support)