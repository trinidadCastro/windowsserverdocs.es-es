---
title: Conexión de Windows Server con los servicios híbridos de Azure
description: Para ampliar las implementaciones locales de Windows Server a la nube se puede usar los servicios híbridos de Azure.
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: medium
ms.date: 05/31/2019
ms.openlocfilehash: eb53a01d4b82bd800b6cc87d2f5d3c36bfa7ff85
ms.sourcegitcommit: 01b3140f79f5614ce566e8036474feefafbeddc3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2020
ms.locfileid: "94581439"
---
# <a name="connecting-windows-server-to-azure-hybrid-services"></a>Conexión de Windows Server con los servicios híbridos de Azure

Para ampliar las implementaciones locales de Windows Server a la nube se puede usar los servicios híbridos de Azure. Estos servicios en la nube proporcionan una matriz de funciones útiles, tanto para ampliar el entorno local a Azure como para llevar a cabo una administración centralizada desde Azure.

![Diagrama que muestra una flecha desde el entorno local a la nube para ampliar el entorno local a Azure, y una flecha desde la nube al entorno local para llevar a cabo una administración centralizada con Azure](../media/azure-services/hybrid-framing.png)

Con los servicios híbridos de Azure en Windows Admin Center, puedes:

- [Proteger las máquinas virtuales y usar la copia de seguridad y la recuperación ante desastres (alta disponibilidad/DR) en la nube.](#back-up-and-protect-your-on-premises-servers-and-vms)
- [Ampliar la capacidad local mediante el almacenamiento y el proceso de Azure, y simplificar la conectividad de red con Azure](#extend-on-premises-capacity-with-azure).
- [Centralizar la supervisión, la gobernanza, la configuración y la seguridad en las aplicaciones, la red y la infraestructura con la ayuda de los servicios de administración de Azure inteligentes de la nube](#centrally-manage-your-hybrid-environment-from-azure).

Aunque la mayor parte de los servicios híbridos de Azure se pueden configurar mediante la descarga de una aplicación y la configuración manual de algunos parámetros, muchos están integrados directamente en Windows Admin Center, con el fin de simplificar la experiencia de configuración y ofrecer una vista de los servicios centrada en el servidor. Windows Admin Center también proporciona hipervínculos inteligentes útiles en Azure Portal para ver los recursos de Azure conectados, así como para obtener una vista centralizada del entorno híbrido.

## <a name="discover-integrated-services-in-the-azure-hybrid-services-tool"></a>Detección de servicios integrados en la herramienta de servicios híbridos de Azure

La herramienta Azure Hybrid Services de [Windows Admin Center](../overview.md) consolida todos los servicios de Azure integrados en un concentrador centralizado en el que se pueden detectar fácilmente todos los servicios de Azure disponibles que aportan valor a su entorno local o híbrido.

![Captura de pantalla de Windows Admin Center en la que se muestra la herramienta Azure Hybrid Services](../media/azure-services/Azure-Hybrid-Services-Discover.png)

Si se conecta a un servidor con los servicios de Azure ya habilitados, la herramienta Azure Hybrid Services actúa como un panel único que permite ver todos los servicios habilitados de dicho servidor. Resulta muy fácil acceder a la herramienta pertinente en Windows Admin Center, iniciarla en Azure Portal para administrar de forma más dichos servicios de Azure o leer más documentación, ya que está al alcance de la mano.

![Captura de pantalla de Windows Admin Center en la que se muestran los servicios de Azure que están ya instalados en el servidor](../media/azure-services/Azure-Hybrid-Services-DayN.png)

Desde la herramienta Azure Hybrid Services se pueden realizar las siguientes operaciones:

- Realizar una copia de seguridad de Windows Server desde Windows Admin Center con [Azure Backup](azure-backup.md)
- Proteger las máquinas virtuales de Hyper-V desde Windows Admin Center con [Azure Site Recovery](azure-site-recovery.md)
- Sincronizar un servidor de archivos con la nube mediante [Azure File Sync](azure-file-sync.md)
- Administrar las actualizaciones del sistema operativo de todos los servidores de Windows, tanto locales como en la nube, con [Azure Update Management](azure-update-management.md)
- Supervisar los servidores, tanto locales como en la nube, y configurar alertas con [Azure Monitor](azure-monitor.md)
- Aplicar directivas de gobernanza a los servidores locales a través de Azure Policy con [Azure Arc para servidores](/azure/azure-arc/servers/overview)
- Proteger tus servidores y obtener protección contra amenazas avanzada con [Azure Security Center](/azure/security-center/windows-admin-center-integration)
- Conectar los servidores de locales a Azure Virtual Network con el [adaptador de red de Azure](https://aka.ms/WACNetworkAdapter)
- Hacer que el aspecto de las máquinas virtuales de Azure sea similar al de la red local con la [red extendida de Azure](https://docs.microsoft.com/azure/virtual-network/subnet-extension#extend-your-subnet-to-azure)

## <a name="back-up-and-protect-your-on-premises-servers-and-vms"></a>Copia de seguridad y protección de las máquinas virtuales y los servidores locales

- **Copia de seguridad de servidores de Windows con [Azure Backup](/azure/backup/backup-overview)** Puede realizar copias de seguridad de los servidores de Windows en Azure, lo que le ayuda a protegerse del eliminaciones accidentales o malintencionadas, datos dañados y ransomware.
Para más información, consulte [Copia de seguridad de servidores con Azure Backup](azure-backup.md).

- **Protección de las máquinas virtuales de Hyper-V con [Azure Site Recovery](/azure/site-recovery/site-recovery-overview)** Puede replicar las cargas de trabajo que se ejecutan en las máquinas virtuales para que su infraestructura empresarial crítica esté protegido en caso de desastre. Windows Admin Center simplifica la instalación y el proceso de replicación de las máquinas virtuales en los servidores o clústeres de Hyper-V, lo que facilita el refuerzo de la resistencia de su entorno con el servicio de recuperación ante desastres de Azure Site Recovery.
Para más información, consulte [Protección de máquinas virtuales con Azure Site Recovery y Windows Admin Center](azure-site-recovery.md).

- **Use la replicación sincrónica o asincrónica basada en bloques en una VM de Azure mediante [Réplica de almacenamiento](../../../storage/storage-replica/storage-replica-overview.md)** Puede configurar la replicación basada en bloque o en el volumen en un nivel de servidor a servidor mediante Réplica de almacenamiento en una VM o un servidor secundario. Windows Admin Center te permite crear una máquina virtual de Azure específica para el destino de replicación, de forma que puedes ajustar el tamaño adecuado del almacenamiento, así como configurarlo correctamente, en una nueva máquina virtual de Azure.
Para obtener más información, consulta [Replicación de servidor a servidor con la réplica de almacenamiento](../../../storage/storage-replica/server-to-server-storage-replication.md).

## <a name="extend-on-premises-capacity-with-azure"></a>Ampliación de la capacidad local con Azure

### <a name="extend-storage-capacity"></a>Ampliación de la capacidad de almacenamiento

- **Sincronización del servidor de archivos con la nube mediante [Azure File Sync](/azure/storage/files/storage-sync-files-planning)** Sincronice archivos en este servidor con recursos compartidos de archivos de Azure. Mantén todos los archivos en un entorno local o usa la nube por niveles para liberar espacio. Así mismo, almacena en la memoria caché solo los archivos que se usen con más frecuencia en el servidor y deja en la nube los datos que se usan de forma esporádica. Se puede hacer una copia de seguridad de datos en la nube, lo que elimina la necesidad de preocuparse por la realización de copias de seguridad de los servidores locales. Además, la sincronización entre varios sitios puede mantener un conjunto de archivos sincronizados entre varios servidores.
Para obtener más información, consulta [Sincronizar el servidor de archivos con la nube mediante Azure File Sync](azure-file-sync.md).

- **Migración de almacenamiento a una VM en Azure mediante [Servicio de migración de almacenamiento](../../../storage/storage-migration-service/overview.md)** Use la herramienta paso a paso para realizar un inventario de los datos en servidores Windows y Linux y, luego, transferir los datos a una nueva VM de Azure. Windows Admin Center puede crear una nueva máquina virtual de Azure para el trabajo que tenga un tamaño adecuado y una configuración correcta para recibir los datos del servidor de origen.
Para obtener más información, consulta [Usar el servicio de migración de almacenamiento para migrar un servidor](../../../storage/storage-migration-service/migrate-data.md).

### <a name="extend-compute-capacity"></a>Ampliación de la capacidad de proceso

- **Creación de una nueva máquina virtual de Azure sin salir del Centro de administración de Windows** Desde la página *Todas las conexiones* del Centro de administración de Windows, vaya a **Agregar** y seleccione **Crear nuevo** en **VM de Azure**. Incluso puedes unirte a un dominio de la máquina virtual de Azure y configurar el almacenamiento desde esta herramienta de creación paso a paso.

- **Uso de Azure para lograr el cuórum en el clúster de conmutación por error con [Testigo de la nube](../../../failover-clustering/deploy-cloud-witness.md)** En lugar de invertir en hardware adicional para lograr el cuórum en un clúster de dos nodos, puedes usar una cuenta de Azure Storage como testigo del clúster para el clúster de Azure Stack HCI u otro de conmutación por error.
Para más información, vea [Deploy a Cloud Witness for a Failover Cluster](../../../failover-clustering/deploy-cloud-witness.md) (Implementación de un testigo en la nube para un clúster de conmutación por error).

### <a name="simplify-network-connectivity-between-your-on-premises-and-azure-networks"></a>Simplificación de la conectividad de red entre las redes locales y de Azure

- **Conexión de los servidores locales a una instancia de Azure Virtual Network con [Adaptador de red de Azure](https://aka.ms/WACNetworkAdapter)** Deje que el Centro de administración de Windows simplifique la configuración de una VPN de punto a sitio desde un servidor local en una red virtual de Azure.

- **Haga que las VM de Azure se parezcan a su red local con la [red extendida de Azure](https://docs.microsoft.com/azure/virtual-network/subnet-extension#extend-your-subnet-to-azure)** El Centro de administración de Windows puede configurar una VPN de sitio a sitio y ampliar las direcciones IP locales a la red virtual de Azure para que pueda migrar las cargas de trabajo a Azure con más facilidad sin interrumpir las dependencias de las direcciones IP.

## <a name="centrally-manage-your-hybrid-environment-from-azure"></a>Administración centralizada de tu entorno híbrido desde Azure

- **Supervisión y obtención de alertas de correo electrónico de todos los servidores de su entorno con [Azure Monitor para máquinas virtuales](/azure/azure-monitor/insights/vminsights-overview)** Azure Monitor, también conocido como Virtual Machines Insights, se puede usar para supervisar el mantenimiento y los eventos de los servidores, crear alertas de correo electrónico, obtener una vista consolidada del rendimiento del servidor en su entorno y visualizar las aplicaciones, los sistemas y los servicios conectados a un servidor dado. Windows Admin Center también puede configurar alertas por correo electrónico predeterminadas para los eventos de estado del clúster y el rendimiento del estado del servidor.
Para más información, consulte [Conexión se servidores a Azure Monitor y configuración de notificaciones por correo electrónico](azure-monitor.md).

- **Administración centralizada de las actualizaciones del sistema operativo para todos los servidores de Windows con [Azure Update Management](/azure/automation/automation-update-management)** Puede administrar las actualizaciones y las revisiones de varios servidores y varias VM desde un único lugar, en lugar de hacerlo por servidor. Con Azure Update Management, puede evaluar rápidamente el estado de las actualizaciones disponibles, programar la instalación de las actualizaciones necesarias y revisar los resultados de las implementaciones, con el fin de comprobar si las actualizaciones se aplican correctamente. Esto es posible si los servidores son máquinas virtuales de Azure, hospedadas por otros proveedores de nube o en un entorno local.
Para más información, consulte [Configuración de servidores para Azure Update Management](azure-update-management.md).

- **Mejora de la postura de seguridad y obtención de protección contra amenazas avanzada con [Azure Security Center](/azure/security-center/security-center-intro)** Azure Security Center es un sistema de administración unificado de la seguridad de infraestructura que fortalece el nivel de seguridad de tus centros de datos y ofrece protección contra amenazas avanzada en las cargas de trabajo híbridas de la nube, tanto si están en Azure o no, así como si se encuentran en el entorno local. Con Windows Admin Center, puedes configurar y conectar fácilmente tus servidores a Azure Security Center.
Para obtener más información, consulta [Integración de Azure Security Center con Windows Admin Center (versión preliminar)](/azure/security-center/windows-admin-center-integration).

- **Aplicación de directivas y garantía del cumplimiento en el entorno híbrido con [Azure Arc para servidores](/azure/azure-arc/servers/overview) y [Azure Policy](/azure/governance/policy/overview)** Realice un inventario de los servidores locales, organícelos y adminístrelos desde Azure. Puedes controlar los servidores mediante Azure Policy y el acceso mediante RBAC, así como habilitar servicios de administración adicionales desde Azure.

## <a name="clusters-versus-stand-alone-servers-and-vms"></a>Clústeres frente a máquinas virtuales y servidores independientes

Los servicios híbridos de Azure funcionan con los servidores de Windows en las siguientes configuraciones:

- Servidores físicos independientes y máquinas virtuales (VM)
- Clústeres, incluidos los clústeres hiperconvergidos certificados por los programas [Azure Stack HCI](/azure-stack/hci/overview?view=azs-2002) y [Windows Server Software-Defined (WSSD)](https://www.microsoft.com/cloud-platform/software-defined-datacenter)

### <a name="services-for-stand-alone-servers-and-vms"></a>Servicios para máquinas virtuales y servidores independientes

Esta es la lista completa de servicios de Azure que proporcionan funcionalidad a servidores independientes y máquinas virtuales:

- Realizar una copia de seguridad de Windows Server desde Windows Admin Center con [Azure Backup](azure-backup.md)
- Proteger las máquinas virtuales de Hyper-V desde Windows Admin Center con [Azure Site Recovery](azure-site-recovery.md)
- Sincronizar un servidor de archivos con la nube mediante [Azure File Sync](azure-file-sync.md)
- Administrar las actualizaciones del sistema operativo de todos los servidores de Windows, tanto locales como en la nube, con [Azure Update Management](azure-update-management.md)
- Supervisar los servidores, tanto locales como en la nube, y configurar alertas con [Azure Monitor](azure-monitor.md)
- Aplicar directivas de gobernanza a los servidores locales a través de Azure Policy con [Azure Arc para servidores](/azure/azure-arc/servers/overview)
- Proteger tus servidores y obtener protección contra amenazas avanzada con [Azure Security Center](/azure/security-center/windows-admin-center-integration)
- Conectar los servidores de locales a Azure Virtual Network con el [adaptador de red de Azure](https://aka.ms/WACNetworkAdapter)
- Hacer que el aspecto de las máquinas virtuales de Azure sea similar al de la red local con la [red extendida de Azure](https://docs.microsoft.com/azure/virtual-network/subnet-extension#extend-your-subnet-to-azure)

### <a name="services-for-clusters"></a>Servicios para clústeres

Estos son los servicios de Azure que proporcionan funcionalidad a los clústeres en conjunto:

- [Supervisión de un clúster hiperconvergido con Azure Monitor](../../../storage/storage-spaces/configure-azure-monitor.md)
- [Protección de máquinas virtuales con Azure Site Recovery](azure-site-recovery.md)
- [Implementación de un testigo en la nube para un clúster](../../../failover-clustering/deploy-cloud-witness.md)

## <a name="other-azure-integrated-abilities-of-windows-admin-center"></a>Otras capacidades integradas en Azure de Windows Admin Center

- **[Adición de conexiones de VM de Azure](manage-azure-vms.md) en el Centro de administración de Windows** Puede usar el Centro de administración de Windows para administrar sus VM de Azure, así como sus máquinas locales. Al configurar la puerta de enlace de Windows Admin Center para conectarse a una red virtual de Azure, puede administrar máquinas virtuales en Azure mediante las consistentes y simplificadas herramientas que proporciona Windows Admin Center.
Para más información, consulte [Configuración de Windows Admin Center para administrar máquinas virtuales en Azure](manage-azure-vms.md).

- **Adición de una capa de seguridad al Centro de administración de Windows mediante la incorporación de la autenticación de [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/)** Puede agregar una capa adicional de seguridad a Windows Admin Center solicitando a los usuarios se autentiquen mediante identidades de Azure Active Directory (Azure AD) para acceder a la puerta de enlace. La autenticación de Azure AD también le permite aprovechar las características de seguridad de Azure AD como el acceso condicional y la autenticación multifactor.
Para obtener más información, consulta [Configuración de la autenticación de Azure AD para Windows Admin Center](../configure/user-access-control.md#azure-active-directory).

- **Administración de recursos de Azure directamente a través de [Azure Cloud Shell](/azure/cloud-shell/overview) insertado en el Centro de administración de Windows** Aproveche Azure Cloud Shell para obtener una experiencia de Bash o PowerShell en Windows Admin Center que le proporcionará acceso simplificado a las tareas de administración de Azure.
Para obtener más información, consulta [Introducción a Azure Cloud Shell](/azure/cloud-shell/overview).


## <a name="additional-references"></a>Referencias adicionales

- [Conexión de Windows Admin Center con Azure](azure-integration.md)
- [Implementación de Windows Admin Center en Azure](deploy-wac-in-azure.md)
