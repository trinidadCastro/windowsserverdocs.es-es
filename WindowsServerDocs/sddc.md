---
title: Centro de datos definido por software de Windows Server
Description: Introducción a SDDC de Windows Server
ms.prod: windows-server
ms.technology: SDDC
ms.topic: get-started article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 06/04/2019
ms.localizationpriority: medium
ms.openlocfilehash: 5a90ad13a51a540c6c76adacb7df0225d642573a
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2020
ms.locfileid: "80857198"
---
# <a name="windows-server-software-defined-datacenter"></a>Centro de datos definido por software de Windows Server

>Se aplica a: Windows Server 2019 y Windows Server 2016

![](media/sddc/heading.png)

## <a name="what-is-windows-server-software-defined-datacenter"></a>¿Qué es el centro de datos definido por software de Windows Server?

El centro de datos definido por software (SDDC) es un término común del sector que suele hacer referencia a un centro de datos donde toda la infraestructura está virtualizada. La virtualización es la clave, y esto significa que el hardware y software del centro de datos se expanden más allá de una relación uno a uno tradicional. Con un hardware de emulación de hipervisor de software, los sistemas operativos y las aplicaciones pueden abstraerse de un hardware físico y multiplicarse para crear grupos de recursos elásticos de procesadores, memoria, E/S y redes.
 
La implementación de Microsoft del SDDC consta de las tecnologías de Windows Server resaltadas en este artículo. Se inicia con el hipervisor Hyper-V que proporciona la plataforma de virtualización en la que se crean el almacenamiento y las redes. Las tecnologías de seguridad, desarrolladas para los desafíos exclusivos de las infraestructuras virtualizadas, mitigan las amenazas internas y externas. Con PowerShell integrado en Windows Server y la incorporación de [System Center](https://docs.microsoft.com/system-center/) y [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview), puedes programar y automatizar el aprovisionamiento, la implementación, la configuración y la administración.

Las tecnologías integradas en Windows Server y System Center son los bloques de creación principales de la experiencia de SDDC de Windows Server. Pero, aunque sea una plataforma virtualizada, aún es necesario tener el hardware adecuado. Los partners de Microsoft que participan en los programas de **Soluciones de Windows Server definido por software (WSSD)** y **Soluciones de Azure Stack HCI** pueden ayudar a tu empresa a adquirir el hardware adecuado y empezar a usarlo desde el día cero.

![](media/sddc/video.png) **[Ver un vídeo para obtener más información sobre SDDC de Microsoft](https://mva.microsoft.com/training-courses/whats-new-in-windows-server-2016-16457?l=YcsJR6sXC_1006218965)**

![](media/sddc/poster-ico.png) **[Descargar un archivo .pdf de tamaño póster de esta página](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/WindowsServerDocs/media/sddc/sddc_poster_0801417_ANSI-E.pdf)**

![](media/sddc/spacer1.png)<a href="https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/WindowsServerDocs//media/sddc/sddc_poster_0801417_ANSI-E.pdf"><img src="media/sddc/poster.png"></a>

## <a name="azure-stack-hci-solutions"></a>Soluciones de Azure Stack HCI

Crear tu centro de datos definido por software de Windows Server en la infraestructura de hardware adecuada es un primer paso crucial para el éxito. Este es el motivo por el que nos hemos asociado con 15 partners para crear diseños de SDDC validados por Microsoft y los procedimientos recomendados para la implementación.

Los partners de Microsoft ofrecen una matriz de soluciones que funcionan con Windows Server 2019 mediante el programa de Azure Stack HCI y con Windows Server 2016 mediante el programa de Windows Server definido por software (WSSD) para ofrecer una infraestructura de redes y almacenamiento, hiperconvergida y de alto rendimiento. Las soluciones hiperconvergidas reúnen cálculo, almacenamiento y redes en servidores y componentes estándar del sector para mejorar el control y la inteligencia de los centros de datos.

![](media/sddc/learn.png) **[Más información sobre las soluciones de Azure Stack HCI](https://azure.microsoft.com/overview/azure-stack/hci)**

![](media/sddc/learn.png) **[Más información sobre las soluciones de WSSD](https://www.microsoft.com/cloud-platform/software-defined-datacenter)**

## <a name="windows-server-virtualized-technologies"></a>Tecnologías virtualizadas de Windows Server ##

En el resto de este tema se explican las tecnologías SDDC de Windows Server y se proporcionan vínculos a la documentación de cada una. Las tecnologías se muestran en la tabla siguiente:

![](media/sddc/table.png)

![](media/sddc/virtualize.png)

### <a name="windows-server-hyper-converged"></a>Windows Server, soluciones hiperconvergidas

Las tecnologías de virtualización de Windows Server incluyen actualizaciones de Hyper-V, el conmutador virtual de Hyper-V, las máquinas virtuales (VM) blindadas y el tejido protegido que mejoran la seguridad, la escalabilidad y la confiabilidad. Las actualizaciones de clústeres de conmutación por error, redes y almacenamiento facilitan aún más la implementación y administración de estas tecnologías cuando se usan con Hyper-V.

![](media/sddc/spacer1.png)![](media/sddc/hyper-converged.png)

![](media/sddc/learn.png) **[Más información sobre Windows Server, soluciones hiperconvergidas](https://docs.microsoft.com/windows-server/get-started/what-s-new-in-windows-server-2016#computevirtualizationvirtualizationmd)**

### <a name="hyper-v-hypervisor"></a>Hipervisor de Hyper-V

Hyper-V es una tecnología de virtualización basada en el hipervisor para Windows. El hipervisor es fundamental para la virtualización. Se trata de la plataforma de virtualización específica de procesador que permite que varios sistemas operativos aislados compartan una misma plataforma de hardware.

![](media/sddc/spacer1.png)![](media/sddc/hypervisor.png)

![](media/sddc/learn.png) **[Más información sobre el hipervisor de Hyper-V](https://www.microsoft.com/cloud-platform/server-virtualization)**

### <a name="guest-clustering-with-shared-vhdx"></a>Agrupación en clústeres de invitados con VHDX compartido

![](media/sddc/virtualize-line.png)

Flexible, seguro y no vinculado a la topología de almacenamiento subyacente, VHDX compartido elimina la necesidad de presentar el almacenamiento físico subyacente a un sistema operativo invitado. El nuevo VHDX compartido admite el cambio de tamaño en línea.

![](media/sddc/spacer1.png)![](media/sddc/cluster.png)

- VHDX compartido puede residir en un volumen compartido de clúster (CSV) de un almacenamiento de bloque o en un almacenamiento basado en archivos SMB.
- Protegido: VHDX compartido admite la réplica de Hyper-V y la copia de seguridad de nivel de host.

![](media/sddc/learn.png) **[Más información sobre la agrupación en clústeres de invitados con VHDX compartido](https://technet.microsoft.com/library/dn281956(v=ws.11).aspx)**

### <a name="hyper-v-replica"></a>Réplica de Hyper-V

![](media/sddc/virtualize-line.png)

Replicación integrada de VM basada en software en la red con certificados. No se vincula al hardware de almacenamiento, red o servidor de ningún sitio.

![](media/sddc/spacer1.png)![](media/sddc/replica.png)

No necesita otras tecnologías de replicación de máquinas virtuales, lo que reduce los costes.
- Controla la migración en vivo automáticamente.
- Administración y configuración sencillas, ya sea a través del Administrador de Hyper-V, PowerShell o con Azure Site Recovery.

![](media/sddc/learn.png) **[Más información sobre la réplica de Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/set-up-hyper-v-replica)**

![](media/sddc/networking.png)

### <a name="network-controller"></a>Controladora de red

![](media/sddc/networking-line.png)

Un punto de automatización programable y centralizado para administrar, configurar y supervisar la infraestructura de red virtual y física de tu centro de datos, así como para resolver problemas en la misma.

![](media/sddc/spacer1.png)![](media/sddc/netcontroller.png)

Los administradores usan una herramienta de administración que interactúa directamente con la controladora de red. La controladora de red proporciona información sobre la infraestructura de red, incluida la infraestructura física y virtual, a la herramienta de administración.

![](media/sddc/learn.png) **[Más información sobre la controladora de red](https://docs.microsoft.com/windows-server/networking/sdn/technologies/network-controller/network-controller)**

### <a name="datacenter-firewall"></a>Firewall de centro de datos

![](media/sddc/networking-line.png)

Cuando se implementa y ofrece como servicio, los administradores de inquilinos pueden instalar y configurar directivas de firewall para ayudar a proteger las redes virtuales de tráfico no deseado de Internet y redes intranet.

![](media/sddc/spacer1.png)![](media/sddc/firewall.png)

El administrador de proveedores de servicios o el administrador de inquilinos puede administrar las directivas de firewall de centro de datos mediante la controladora de red.

![](media/sddc/learn.png) **[Más información sobre el firewall de centro de datos](https://docs.microsoft.com/windows-server/networking/sdn/technologies/network-function-virtualization/datacenter-firewall-overview)**

### <a name="switch-embedded-teaming"></a>Switch Embedded Teaming

![](media/sddc/networking-line.png)

SET es una solución alternativa de NIC Teaming que puedes usar en entornos que incluyen Hyper-V y la pila [Redes definidas por software (SDN)](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking).

![](media/sddc/spacer1.png)![](media/sddc/teaming.png)

![](media/sddc/learn.png) **[Más información sobre Switch Embedded Teaming](https://docs.microsoft.com/windows-server/networking/sdn/technologies/set-for-sdn)**

### <a name="software-load-balancing"></a>Equilibrio de carga de software

![](media/sddc/networking-line.png)

SLB permite que varios servidores hospeden la misma carga de trabajo, lo que proporciona alta disponibilidad y escalabilidad. Escala horizontalmente las funcionalidades de equilibrio de carga mediante máquinas virtuales SLB de los mismos servidores de Hyper-V que usas para las otras cargas de trabajo de máquina virtual. SLB admite la creación y eliminación rápidas de puntos de conexión de equilibrio de carga para operaciones del proveedor de servicios en la nube. SLB admite decenas de gigabytes por clúster, proporciona un modelo de aprovisionamiento sencillo y es fácil de escalar de forma horizontal y vertical.

![](media/sddc/spacer1.png)![](media/sddc/balancer.png)

![](media/sddc/learn.png) **[Más información sobre el equilibrio de carga de software](https://docs.microsoft.com/windows-server/networking/sdn/technologies/network-function-virtualization/software-load-balancing-for-sdn)**


![](media/sddc/storage.png)

### <a name="storage-spaces-direct"></a>Espacios de almacenamiento directos

![](media/sddc/storage-line.png)

Los espacios de almacenamiento directo usan servidores estándar del sector con unidades conectadas localmente para crear almacenamiento definido por software de alta disponibilidad y escalabilidad por un porcentaje mínimo del coste de las matrices de SAN o NAS tradicionales. Su arquitectura simplifica considerablemente la adquisición y la implementación.

![En cada nodo hay unidades conectadas localmente, que se agrupan en el nivel de clúster mediante espacios de almacenamiento directo, a las que tienen acceso las máquinas virtuales (VM) a través de CSV](media/sddc/spacer1.png)![](media/sddc/ssd.png)

El espacio de almacenamiento directo presenta el nuevo Bus de almacenamiento de software y aprovecha muchas de las características que se conocen hoy en día en Windows Server, como los clústeres de conmutación por error, los volúmenes compartidos de clúster (CSV), el bloque de mensajes del servidor (SMB) 3 y los espacios de almacenamiento.

![](media/sddc/learn.png) **[Más información sobre los espacios de almacenamiento directo](storage/storage-spaces/storage-spaces-direct-overview.md)**
### <a name="storage-quality-of-service"></a>Calidad de servicio de almacenamiento ###

![](media/sddc/storage-line.png)

Supervisa y administra centralmente el rendimiento del almacenamiento para máquinas virtuales con Hyper-V y los roles de servidor de archivos de escalabilidad horizontal, mejorando la equidad de recursos de almacenamiento entre varias máquinas virtuales.

![](media/sddc/spacer1.png)![](media/sddc/qos.png)

La calidad de servicio de almacenamiento está integrada en la solución de almacenamiento definida por software de Microsoft que proporcionan el servidor de archivos de escalabilidad horizontal e Hyper-V con el protocolo SMB3. Un nuevo administrador de directivas proporciona supervisión de rendimiento para el almacenamiento central.

![](media/sddc/learn.png) **[Más información sobre la calidad de servicio de almacenamiento](https://docs.microsoft.com/windows-server/storage/storage-qos/storage-qos-overview)**

### <a name="storage-replica"></a>Réplica de almacenamiento


![](media/sddc/storage-line.png)

La preparación y recuperación ante desastres hacen que no se pierda ningún dato, con la capacidad de proteger datos de forma sincrónica en diferentes bastidores, plantas, edificios, campus, ciudades y países con un uso más eficaz de varios centros de datos.

![](media/sddc/spacer1.png)
![](media/sddc/storage-replica.png)

Replicación sincrónica

1. La aplicación escribe los datos.
2. Se escriben los datos de registro y estos se replican en el sitio remoto.
3. Se escriben los datos de registro en el sitio remoto.
4. Confirmación del sitio remoto.
5. Confirmación de escritura en la aplicación.

t y t1: Datos vaciados en el volumen, los registros siempre se escriben

![](media/sddc/learn.png) **[Más información sobre la réplica de almacenamiento](https://docs.microsoft.com/windows-server/storage/storage-replica/storage-replica-overview)**

![](media/sddc/security.png)

### <a name="guarded-fabric"></a>Tejido protegido

![](media/sddc/security-line.png)

Como administrador de empresa de nube privada y proveedor de servicio en la nube, puedes usar un tejido protegido para proporcionar un entorno más seguro para las máquinas virtuales. Un tejido protegido consta de un servicio de protección de host (HGS), por lo general, un clúster de tres nodos, además de uno o varios hosts protegidos y un conjunto de máquinas virtuales blindadas (VM).

![](media/sddc/spacer1.png)![](media/sddc/guarded-fabric.png)

![](media/sddc/learn.png) **[Más información sobre el tejido protegido](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms)**

### <a name="shielded-vms"></a>VM blindadas

![](media/sddc/security-line.png)

Los datos y el estado de una máquina virtual blindada están protegidos frente a inspección, robos y alteraciones de los administradores tanto de malware como de los centros de datos.

![](media/sddc/spacer1.png)![](media/sddc/shielded.png)

- Las máquinas virtuales blindadas solo se ejecutarán en tejidos designados como propietarios de la máquina virtual.
- Las máquinas virtuales blindadas se cifran mediante BitLocker, u otros medios, para que solo los propietarios designados puedan ejecutarlas.
- La ejecución de máquinas virtuales puede convertirse en blindada.

![](media/sddc/learn.png) **[Más información sobre las máquinas virtuales blindadas](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms)**

### <a name="host-guardian-service"></a>Servicio de protección de host

![](media/sddc/security-line.png)

El servicio de protección de host cuenta con claves para tejidos legítimos, así como para máquinas virtuales cifradas.

![](media/sddc/spacer1.png)![](media/sddc/guardian.png)

![](media/sddc/learn.png) **[Más información sobre el servicio de protección de host](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-manage-hgs)**

### <a name="device-health-attestation"></a>Atestación de estado de dispositivo

![](media/sddc/security-line.png)

La atestación permite a las empresas aumentar el nivel de seguridad de la organización a la seguridad supervisada y atestiguada de hardware, con mínimo o ningún impacto en los costes de operación.


![](media/sddc/spacer1.png)![](media/sddc/attestation.png)


El modo de confianza de hardware, mostrada arriba, proporciona el máximo nivel de seguridad, con confianza de raíz de hardware TPM v2.0 y cumplimiento con la directiva de integridad de código para liberar las claves.


![](media/sddc/learn.png) **[Más información sobre la atestación de estado de dispositivo](https://docs.microsoft.com/windows-server/security/device-health-attestation)**

![](media/sddc/management.png)

### <a name="powershell-desired-state-configuration"></a>Desired State Configuration de PowerShell

![](media/sddc/management-line.png)

Desired State Configuration de Windows PowerShell ofrece una plataforma de administración de configuración integrada en Windows que se basa en estándares abiertos. DSC es lo suficientemente flexible como para funcionar de forma confiable y coherente en cada una de las etapas del ciclo de vida de implementación (desarrollo, prueba, preproducción, producción), así como durante el escalado horizontal.

![](media/sddc/spacer1.png)![](media/sddc/dsc.png)

DSC admite implementaciones continuas para poder implementar configuraciones una y otra vez sin interrumpir nada.

-  Las configuraciones de DSC solo aplican los ajustes que han cambiado con respecto a la versión original para que implementaciones sean más rápidas.
-  DSC puede usarse en un entorno local, público o de nube privada.
-  Puedes integrar DSC con cualquier solución de Microsoft o de terceros siempre y cuando puedas ejecutar un script de PowerShell en el sistema de destino.

![](media/sddc/learn.png) **[Más información sobre PowerShell DSC](https://docs.microsoft.com/powershell/dsc/overview)**


### <a name="system-center-vmm"></a>System Center VMM

![](media/sddc/management-line.png)

Virtual Machine Manager forma parte del conjunto System Center, que se usa para configurar, administrar y transformar centros de datos tradicionales para proporcionar una experiencia de administración unificada a través del entorno local, el proveedor de servicios y la nube de Azure.

![](media/sddc/spacer1.png)![](media/sddc/vmm.png)

- Centro de datos: configura y administra los componentes del centro de datos como un solo tejido en VMM. 
- Hosts de virtualización: VMM puede añadir, aprovisionar y administrar clústeres y hosts de virtualización de Hyper-V y VMware.
- Redes: VMM proporciona virtualización de red, incluida la compatibilidad para crear y administrar redes virtuales y puertas de enlace de red. 
- Almacenamiento: VMM puede descubrir, clasificar, aprovisionar, designar y asignar almacenamiento local y remoto.

![](media/sddc/learn.png) **[Más información sobre System Center VMM](https://docs.microsoft.com/system-center/vmm/)**

### <a name="windows-admin-center"></a>Windows Admin Center

![](media/sddc/management-line.png)

Windows Admin Center es un conjunto de herramientas de administración, basadas en explorador e implementadas localmente, que permite la administración local de los servidores de Windows sin dependencia alguna de Azure o la nube. Windows Admin Center proporciona a los administradores de TI un control total sobre todos los aspectos de la infraestructura del servidor y es especialmente útil para la administración de las redes privadas que no están conectados a Internet.

![](media/sddc/spacer1.png)![](media/sddc/architecture.png)

La publicación del servidor web en DNS y la configuración del firewall corporativo pueden permitir el acceso a Windows Admin Center desde la Internet pública, lo que permite conectarse y administrar los servidores desde cualquier lugar con Microsoft Edge o Google Chrome.

![](media/sddc/learn.png) **[Más información sobre Windows Admin Center](manage/windows-admin-center/overview.md)**
