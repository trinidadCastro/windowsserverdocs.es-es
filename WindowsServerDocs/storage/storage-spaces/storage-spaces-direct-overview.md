---
title: Información general de Espacios de almacenamiento directos
ms.prod: windows-server-threshold
ms.author: cosdar
ms.manager: dongill
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 03/26/2019
ms.assetid: 8bd0d09a-0421-40a4-b752-40ecb5350ffd
description: Una descripción general de espacios de almacenamiento directo, una característica de Windows Server que permite a los servidores del clúster con almacenamiento interno en una solución de almacenamiento definido por software.
ms.localizationpriority: medium
ms.openlocfilehash: d71e4fc404f760102a2b22f71bbeec680868e9b8
ms.sourcegitcommit: e558dda2774345e9ad17ff04b759f68c59d88139
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2019
ms.locfileid: "9262788"
---
# Información general de Espacios de almacenamiento directos

>Se aplica a: Windows Server 2019, Windows Server 2016

Espacios de almacenamiento directo usa servidores estándar del sector con unidades conectadas localmente para crear almacenamiento definido por software de alta disponibilidad y escalabilidad por un porcentaje mínimo del costo de las matrices de SAN o NAS tradicionales. Su arquitectura convergida o hiperconvergida simplifica considerablemente la adquisición y la implementación, mientras que las características, como el almacenamiento en caché, las capas de almacenamiento y codificación de borrado, junto con las últimas innovaciones de hardware tales como redes de RDMA y unidades de NVMe, ofrecer inigualable eficacia y el rendimiento.

Espacios de almacenamiento directo se incluye en Windows Server 2019 Datacenter, Windows Server 2016 Datacenter y [Compilaciones de Windows Server Insider Preview](https://insider.windows.com/for-business-getting-started-server/). 

Para otras aplicaciones de espacios de almacenamiento, como los clústeres de SAS compartidos y servidores independientes, consulta la [información general de espacios de almacenamiento](overview.md). Si buscas información sobre el uso de espacios de almacenamiento en un equipo Windows 10, consulta [Los espacios de almacenamiento en Windows 10](https://support.microsoft.com/help/12438/windows-10-storage-spaces).

<table>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Comprender</a></strong>
            <ul>
              <li>Información general (estás aquí)</li>
              <li><a href="understand-the-cache.md">Descripción de la memoria caché</a></li>
              <li><a href="storage-spaces-fault-tolerance.md">Tolerancia a errores y eficiencia del almacenamiento</a></li>
              <li><a href="drive-symmetry-considerations.md">Consideraciones de simetría de unidad</a></li>
              <li><a href="understand-storage-resync.md">Comprender y controlar la resincronización de almacenamiento</a></li>
              <li><a href="understand-quorum.md">Cuórum de clúster y grupo de descripción</a></li>
              <li><a href="cluster-sets.md">Conjuntos de clústeres</a></li>
            </ul>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>Planificar</a></strong>
            <ul>
              <li><a href="storage-spaces-direct-hardware-requirements.md">Requisitos de hardware</a></li>
              <li><a href="csv-cache.md">Usar la caché de lectura en memoria CSV</li>
              <li><a href="choosing-drives.md">Elegir las unidades</a></li>
              <li><a href="plan-volumes.md">Planificar los volúmenes</a></li>
              <li><a href="storage-spaces-direct-in-vm.md">Usar clústeres de VM invitados</a></li>
              <li><a href="storage-spaces-direct-disaster-recovery.md">Recuperación ante desastres</a></li>
            </ul>
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Implementar</a></strong>
            <ul>
                    <li><a href="deploy-storage-spaces-direct.md">Implementar espacios de almacenamiento directo</a></li>
                    <li><a href="create-volumes.md">Crear volúmenes</a></li>
              <li><a href="nested-resiliency.md">Resistencia anidada</a></li>
              <li><a href="../../failover-clustering/manage-cluster-quorum.md">Configurar cuórum</a></li>
              <li><a href="upgrade-storage-spaces-direct-to-windows-server-2019.md">Actualizar un clúster de Espacios de almacenamiento directo a Windows Server 2019</a></li>
            </ul>
        </td>        
        <td style="padding: 5px; border: 0;">
            <strong>Administrar</a></strong>
            <ul>
              <li><a href="../../manage/windows-admin-center/use/manage-hyper-converged.md">Administrar con Windows Admin Center</a></li>
              <li><a href="add-nodes.md">Agregar servidores o unidades</a></li>
              <li><a href="maintain-servers.md">Desconectar un servidor para realizar labores de mantenimiento</li>
              <li><a href="remove-servers.md">Quitar servidores</a></li>
              <li><a href="resize-volumes.md">Ampliar volúmenes</a></li>
              <li><a href="../update-firmware.md">Actualizar el firmware de la unidad</a></li>
              <li><a href="performance-history.md">Historial de rendimiento</a></li>
              <li><a href="delimit-volume-allocation.md">Delimitar la asignación de volúmenes</a></li>
              <li><a href="configure-azure-monitor.md">Utilizar el Monitor de Azure en un clúster hiperconvergido</a></li>
            </ul>
        </td>
    </tr>
    <tr style="border: 0;">
         <td style="padding: 5px; border: 0;">
            <strong>Solución de problemas</a></strong>
            <ul>
              <li><a href="storage-spaces-states.md">Solucionar problemas de mantenimiento y estados operativos</a></li>
              <li><a href="data-collection.md">Recopilar datos de diagnóstico con espacios de almacenamiento directo</a></li>
            </ul>
         <td style="padding: 5px; border: 0;">
            <strong>Entradas de blog recientes</a></strong>
            <ul>
              <li><a href="https://blogs.technet.microsoft.com/filecab/2018/10/30/windows-server-2019-and-intel-optane-dc-persistent-memory/">13.7 millones de IOPS con espacios de almacenamiento directo: el nuevo registro de la industria de infraestructura hiperconvergida</a></li>
              <li><a href="https://blogs.technet.microsoft.com/filecab/2018/10/02/hci-the-countdown-clock-starts-now/">Infraestructura hiperconvergida de Windows Server 2019 - el reloj de cuenta atrás empieza ya!</a></li>
              <li><a href="https://blogs.technet.microsoft.com/filecab/2018/06/27/windows-server-summit-recap/">Cinco grandes anuncios desde la conferencia de servidor de Windows</a></li>
              <li><a href="https://blogs.technet.microsoft.com/filecab/2018/03/27/storage-spaces-direct-momentum/">Clústeres de espacios de almacenamiento directo 10.000 y recuento …</a></li>
            </ul>
</table>

## Vídeos

**Vídeo de introducción rápida (5 minutos)**

<iframe src="https://www.youtube-nocookie.com/embed/raeUiNtMk0E" width="560" height="315" allowfullscreen></iframe>

**Espacios de almacenamiento directo en Microsoft Ignite 2018 (1 hora)**

[Ver en YouTube](https://www.youtube.com/watch?v=5kaUiW3qo30)

**Espacios de almacenamiento directo en Microsoft Ignite 2017 (1 hora)**

[Ver en YouTube](https://www.youtube.com/watch?v=YDr2sqNB-3c)

**Iniciar el evento de Microsoft Ignite 2016 (1 hora)**

[Ver en YouTube](https://www.youtube.com/watch?v=-LK2ViRGbWs)

## Ventajas clave

<table>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:100px">
            <img src="media/storage-spaces-direct-in-windows-server-2016/simplicity-icon.png" width="100" alt="">
        </td>
        <td style="padding: 10px; border: 0;">
            <b>Simplicidad.</b> Pasa de servidores estándar del sector que ejecutan Windows Server 2016 a tu primer clúster de Espacios de almacenamiento directo en menos de 15 minutos. En el caso de los usuarios de System Center, la implementación consiste en una simple casilla.
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
            <b>Tolerancia a errores. </b> La resistencia integrada controla los errores de unidades, servidores o componentes con una disponibilidad continua. También es posible configurar implementaciones más grandes para la <a href="../../failover-clustering/fault-domains.md">tolerancia a errores de chasis y bastidor</a>. Cuando se produce un error de hardware, cámbielo. El software se repara automáticamente, sin pasos de administración complicados.
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:100px">
            <img src="media/storage-spaces-direct-in-windows-server-2016/efficiency-icon.png" width="100" alt="">
        </td>
        <td style="padding: 10px; border: 0;">
            <b>Eficiencia de los recursos.</b> La codificación de borrado ofrece una eficiencia de almacenamiento hasta 2,4 veces mayor, con innovaciones exclusivas, como códigos de reconstrucción local y capas ReFS en tiempo real, para ampliar estas mejoras a las unidades de disco duro y las cargas de trabajo mixtas en frío y en caliente, al mismo tiempo que reduce al mínimo el consumo de CPU para asignar los recursos donde se necesitan más: las máquinas virtuales.
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:100px">
            <img src="media/storage-spaces-direct-in-windows-server-2016/manageability-icon.png" width="100" alt="">
        </td>
        <td style="padding: 10px; border: 0;">
            <b>Capacidad de administración.</b> Use los <a href="../storage-qos/storage-qos-overview.md">controles de QoS de almacenamiento</a> para comprobar las máquinas virtuales demasiado ocupadas con límites mínimos y máximos de E/S por segundo por máquina virtual. El <a href="../../failover-clustering/health-service-overview.md">Servicio de mantenimiento</a> proporciona continuamente alertas y supervisión integrada, mientras que las nuevas API facilitan la recopilación de métricas enriquecidas de rendimiento y capacidad de todo el clúster.
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:100px">
            <img src="media/storage-spaces-direct-in-windows-server-2016/scalability-icon.png" width="100" alt="">
        </td>
        <td style="padding: 10px; border: 0;">
            <b>Escalabilidad.</b> Usa hasta 16 servidores y más de 400 unidades para obtener varios hasta 1petabyte (1000terabytes) de almacenamiento por clúster. Para escalar horizontalmente, basta con agregar unidades o más servidores. Espacios de almacenamiento directo incorporará automáticamente las unidades nuevas y empezará a usarlas. La eficiencia de almacenamiento y el rendimiento mejoran de forma predecible a escala.
        </td>
    </tr>
</table>

## Opciones de implementación

Espacios de almacenamiento directo está diseñado para dos opciones de implementación distintas:

### Convergido

**Almacenamiento y cálculo en clústeres independientes.** La opción de implementación convergida, también denominada "desagregada", separa en capas un Servidor de archivos de escalabilidad horizontal (SoFS) encima de Espacios de almacenamiento directo para proporcionar almacenamiento conectado a la red a través de recursos compartidos de archivos SMB3. Esto permite escalar el proceso y la carga de trabajo independientemente del clúster de almacenamiento, lo que es esencial para implementaciones a gran escala, como IaaS (infraestructura como servicio) de Hyper-V para empresas y proveedores de servicios.

![Espacios de almacenamiento directo proporciona el almacenamiento mediante la característica Servidor de archivos de escalabilidad horizontal para las máquinas virtuales de Hyper-V que estén en otro servidor o clúster](media/storage-spaces-direct-in-windows-server-2016/converged-minimal.png)

### Hiperconvergido

**Un clúster para cálculo y almacenamiento.** La opción de implementación hiperconvergida ejecuta máquinas virtuales de Hyper-V o bases de datos de SQL Server directamente en los servidores que proporcionan el almacenamiento y almacena sus archivos en los volúmenes locales. Esto elimina la necesidad de configurar los permisos y el acceso al servidor de archivos, y además reduce los costos de hardware para pequeñas y medianas empresas o implementaciones remotas de oficinas y sucursales. Consulta [implementar espacios de almacenamiento directo](deploy-storage-spaces-direct.md).

![Espacios de almacenamiento directo proporciona el almacenamiento para las máquinas virtuales de Hyper-V que estén en el mismo clúster](media/storage-spaces-direct-in-windows-server-2016/hyper-converged-minimal.png)

## Funcionamiento

Espacios de almacenamiento directo es la evolución de Espacios de almacenamiento, introducido por primera vez en Windows Server 2012. Aprovecha muchas de las características que se conocen hoy en día en Windows Server, como los clústeres de conmutación por error, el sistema de archivos del Volumen compartido de clúster (CSV), el Bloque de mensajes del servidor (SMB) 3 y, por supuesto, Espacios de almacenamiento. También introduce tecnologías nuevas, en especial el bus de almacenamiento de software.

A continuación presentamos una visión general de la pila de Espacios de almacenamiento directo:

![Pila de Espacios de almacenamiento directo](media/storage-spaces-direct-in-windows-server-2016/converged-full-stack.png)

**Hardware de red.** Espacios de almacenamiento directo usa SMB3 (incluido SMB directo y SMB multicanal) sobre Ethernet para la comunicación entre los servidores. Le recomendamos encarecidamente que disponga de más de 10 GbE con acceso directo a memoria remota (RDMA), ya sea iWARP o RoCE.

**Hardware de almacenamiento.** De 2 a 16 servidores con unidades de SATA, SAS o NVMe conectadas localmente. Cada servidor debe tener al menos dos unidades de estado sólido y al menos cuatro unidades adicionales. Los dispositivos SATA y SAS deben estar detrás de un adaptador de bus host (HBA) y un ampliador SAS. Le recomendamos encarecidamente las plataformas meticulosamente diseñadas y ampliamente validadas de nuestros socios (próximamente).

**Clústeres de conmutación por error.** La característica integrada de agrupación en clústeres de Windows Server se usa para conectar los servidores.

**Bus de almacenamiento de software.** El bus de almacenamiento de software es una novedad de Espacios de almacenamiento directo. Abarca el clúster y establece un tejido de almacenamiento definido por software gracias al cual todos los servidores pueden ver todas las unidades locales de los demás servidores. Puede considerarse una manera de reemplazar el cableado costoso y restrictivo del canal de fibra o el SAS compartido.

**Caché de capa de bus de almacenamiento.** El bus de almacenamiento de software enlaza dinámicamente las unidades más rápidas presentes (por ejemplo, SSD) a unidades más lentas (por ejemplo, HDD) para proporcionar un almacenamiento en caché de lectura y escritura del lado servidor que acelera la E/S y aumenta el rendimiento.

**Grupo de almacenamiento.** La colección de unidades que forman la base de Espacios de almacenamiento se denomina grupo de almacenamiento. Se crea automáticamente, y todas las unidades válidas se detectan y se agregan automáticamente. Te recomendamos encarecidamente que uses un grupo por cada clúster, con la configuración predeterminada. Lee nuestro blog sobre [profundización en el grupo de almacenamiento](https://blogs.technet.microsoft.com/filecab/2016/11/21/deep-dive-pool-in-spaces-direct/) para obtener más información.

**Espacios de almacenamiento.** Espacios de almacenamiento proporciona tolerancia a errores a "discos" virtuales mediante [la creación de reflejo, la codificación de borrado o ambas](storage-spaces-fault-tolerance.md). Considéralo un volumen RAID distribuido definido por software que usa las unidades del grupo. En Espacios de almacenamiento directo, estos discos virtuales suelen tener resistencia a dos errores de unidad o de servidor simultáneos (por ejemplo, creación de reflejo triple, con cada copia de datos en un servidor diferente) aunque la tolerancia a errores de chasis y bastidor también esté disponible.

**Sistema de archivos resistente (ReFS).** ReFS es un sistema de archivos Premier diseñado específicamente para la virtualización. Incluye aceleraciones considerables para operaciones de archivos .vhdx, como la creación, la expansión y la combinación de puntos de control, así como sumas de comprobación integradas para detectar y corregir errores de bit. También introduce niveles en tiempo real que hacen rotar los datos en tiempo real entre niveles de almacenamiento llamados "en frío" y "en caliente" en función del uso.

**Volúmenes compartidos de clúster.** El sistema de archivos CSV unifica todos los volúmenes de ReFS en un único espacio de nombres accesible a través de cualquier servidor, por lo que para cada servidor, cada volumen parece montado localmente y actúa como tal.

**Servidor de archivos de escalabilidad horizontal.** Esta capa final solo es necesaria para las implementaciones convergidas. Proporciona acceso a archivos remotos con el protocolo de acceso SMB3 a clientes (como otro clúster que ejecuta Hyper-V) a través de la red, con lo que efectivamente convierte Espacios de almacenamiento directo en un almacenamiento conectado a la red (NAS).

## Historias de clientes

Hay [más de 10.000 clústeres](https://blogs.technet.microsoft.com/filecab/2018/03/27/storage-spaces-direct-momentum/) en todo el mundo ejecución espacios de almacenamiento directo. Las organizaciones de todos los tamaños, desde pequeñas empresas implementar tan solo dos nodos, en grandes empresas y gobiernos implementar cientos de nodos y dependen de espacios de almacenamiento directo para sus aplicaciones críticas y la infraestructura.

Visita [Microsoft.com/HCI](https://www.microsoft.com/hci) para leer sus casos:

[![GRID de logotipos de cliente](media/storage-spaces-direct-in-windows-server-2016/customer-stories.png)](https://www.microsoft.com/hci)

## Herramientas de administración

Las siguientes herramientas pueden usarse para administrar y supervisar espacios de almacenamiento directo:

| Nombre | ¿Gráfica o línea de comandos? | ¿De pago o incluyen? |
|-----------------|----------------------------|-------------------|
| [Windows Admin Center](../../manage/windows-admin-center/overview.md)     | Gráfica    | Incluido |
| Administrador de clústeres de conmutación por error & del administrador del servidor                                 | Gráfica    | Incluido |
| Windows PowerShell                                                        | Línea de comandos | Incluido |
| [System Center Virtual Machine Manager (SCVMM)](https://technet.microsoft.com/system-center-docs/vmm/manage/manage-storage-spaces-direct-vmm) & [Operations Manager (SCOM)](https://www.microsoft.com/download/details.aspx?id=54700) | Gráfica    | De pago     |

## Introducción

Prueba Espacios de almacenamiento directo [en Microsoft Azure](https://blogs.technet.microsoft.com/filecab/2016/05/05/s2dazuretp5/), o descarga una copia de evaluación con licencia para 180 días de Windows Server desde [Evaluaciones de Windows Server](https://go.microsoft.com/fwlink/?linkid=842602).

## Consulta también

-   [Tolerancia a errores y eficiencia del almacenamiento](storage-spaces-fault-tolerance.md)
-   [Réplica de almacenamiento](../storage-replica/storage-replica-overview.md)
-   [Almacenamiento en el blog de Microsoft](https://blogs.technet.microsoft.com/filecab/)
-   [Storage Spaces Direct throughput with iWARP](https://blogs.technet.microsoft.com/filecab/2017/03/13/storage-spaces-direct-throughput-with-iwarp) (Rendimiento de Espacios de almacenamiento directo con iWARP) (blog de TechNet)
-   [Novedades de los clústeres de conmutación por error en Windows Server](../../failover-clustering/whats-new-in-failover-clustering.md)  
-   [Calidad de servicio de almacenamiento](../storage-qos/storage-qos-overview.md)
-   [Soporte técnico de Windows IT Pro](https://www.microsoft.com/itpro/windows/support)
