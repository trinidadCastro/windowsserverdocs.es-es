---
title: Conexión de Windows Server con los servicios híbridos de Azure
description: Para ampliar las implementaciones locales de Windows Server a la nube se puede usar los servicios híbridos de Azure.
ms.technology: manage
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 05/31/2019
ms.openlocfilehash: 460399a57bc229b44d37a9fdd1e4938bf9e7d6ac
ms.sourcegitcommit: fe621b72d45d0259bac1d5b9031deed3dcbed29d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2019
ms.locfileid: "66455363"
---
# <a name="connecting-windows-server-to-azure-hybrid-services"></a>Conexión de Windows Server con los servicios híbridos de Azure

>Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server (canal semianual)

Para ampliar las implementaciones locales de Windows Server a la nube se puede usar los servicios híbridos de Azure. Dichos servicios en la nube proporcionan una matriz de funciones útiles, entre las que se incluyen las siguientes:

- Proteger las máquinas virtuales y usar la copia de seguridad y recuperación ante desastres (HA/DR) en la nube con Azure Site Recovery. 
- Realizar el seguimiento de lo que sucede en las aplicaciones, la red y la infraestructura con la ayuda de análisis y aprendizaje automático avanzados en Azure Monitor. 
- Simplificar la conectividad de la red con Azure con el adaptador de red de Azure.
- Mantener actualizadas las máquinas virtuales con Azure Update Management.

Los servicios híbridos de Azure funcionan con los servidores de Windows en las siguientes configuraciones:

- Servidores físicos independientes y máquinas virtuales (VM)
- Clústeres, incluidos los clústeres hiperconvergidos certificados por los programas [Azure Stack HCI](https://docs.microsoft.com/azure-stack/operator/azure-stack-hci-overview) y [Windows Server Software-Defined (WSSD)](https://www.microsoft.com/en-us/cloud-platform/software-defined-datacenter)

Aunque la mayor parte de los servicios híbridos de Azure se pueden configurar mediante Azure Portal y una descarga o dos, muchos están integrados directamente en Windows Admin Center, con el fin de simplificar su instalación y ofrecer una vista de los servicios centrada en el servidor.

## <a name="azure-hybrid-services-tool"></a>Herramienta Azure Hybrid Services

La herramienta Azure Hybrid Services de [Windows Admin Center](../understand/windows-admin-center.md) consolida todos los servicios de Azure integrados en un concentrador centralizado en el que se pueden detectar fácilmente todos los servicios de Azure disponibles que aportan valor a su entorno local o híbrido. 

![Captura de pantalla de Windows Admin Center en la que se muestra la herramienta Azure Hybrid Services](../media/azure-services/ahs-discover.png)

Si se conecta a un servidor con los servicios de Azure ya habilitados, la herramienta Azure Hybrid Services actúa como un panel único que permite ver todos los servicios habilitados de dicho servidor. Resulta muy fácil acceder a la herramienta pertinente en Windows Admin Center, iniciarla en Azure Portal para administrar de forma más dichos servicios de Azure o leer más documentación, ya que está al alcance de la mano. 

![Captura de pantalla de Windows Admin Center en la que se muestran los servicios de Azure que están ya instalados en el servidor](../media/azure-services/ahs-dayN.png)

Desde la herramienta Azure Hybrid Services se pueden realizar las siguientes operaciones:
- Realizar una copia de seguridad de Windows Server desde Windows Admin Center con [Azure Backup](azure-backup.md)
- Proteger las máquinas virtuales de Hyper-V desde Windows Admin Center con [Azure Site Recovery](azure-site-recovery.md)
- Sincronizar un servidor de archivos con la nube mediante [Azure File Sync](azure-file-sync.md)
- Administrar las actualizaciones del sistema operativo de todos los servidores de Windows, tanto locales como en la nube, con [Azure Update Management](azure-update-management.md)
- Supervisar los servidores, tanto locales como en la nube, y configurar alertas con [Azure Monitor](azure-monitor.md)
- Conectar los servidores de locales a Azure Virtual Network con el [adaptador de red de Azure](https://aka.ms/WACNetworkAdapter)

## <a name="services-for-stand-alone-servers-and-vms"></a>Servicios para máquinas virtuales y servidores independientes

Esta es la lista completa de servicios de Azure que proporcionan funcionalidad a servidores independientes y máquinas virtuales:

- **(Novedad) Sincronice su servidor de archivos con la nube mediante [Azure File Sync](https://aka.ms/afs)**  
Sincronice los archivos de este servidor con recursos compartidos de archivos de Azure. Mantenga todos los archivos en un entorno local o use la nube por niveles, almacene en la memoria caché solo los archivos que se usen con más frecuencia y deje en la nube los datos que se usan de forma esporádica. Se puede hacer una copia de seguridad de datos en la nube, lo que elimina la necesidad de preocuparse por la realización de copias de seguridad de los servidores locales. Además, la sincronización entre varios sitios puede mantener un conjunto de archivos sincronizados entre varios servidores.

- **Agregue una capa de seguridad a Windows Admin Center mediante la autenticación de [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/)**  
Puede agregar una capa adicional de seguridad a Windows Admin Center solicitando a los usuarios se autentiquen mediante identidades de Azure Active Directory (Azure AD) para acceder a la puerta de enlace. La autenticación de Azure AD también le permite aprovechar las características de seguridad de Azure AD como el acceso condicional y la autenticación multifactor.  
Para más información, consulte [Configuración de la autenticación de Azure AD para Windows Admin Center](../configure/user-access-control.md#azure-active-directory).  

- **Proteja las máquinas virtuales de Hyper-V con [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)**  
Puede replicar las cargas de trabajo que se ejecutan en las máquinas virtuales para que su infraestructura empresarial crítica esté protegido en caso de desastre. Windows Admin Center simplifica la instalación y el proceso de replicación de las máquinas virtuales en los servidores o clústeres de Hyper-V, lo que facilita el refuerzo de la resistencia de su entorno con el servicio de recuperación ante desastres de Azure Site Recovery.  
Para más información, consulte [Protección de máquinas virtuales con Azure Site Recovery y Windows Admin Center](azure-site-recovery.md).

- **Realice copias de seguridad de los servidores de Windows con [Azure Backup](https://docs.microsoft.com/azure/backup/backup-overview)**  
Puede realizar copias de seguridad de los servidores de Windows en Azure, lo que le ayuda a protegerse del eliminaciones accidentales o malintencionadas, datos dañados y ransomware.  
Para más información, consulte [Copia de seguridad de servidores con Azure Backup](azure-backup.md).

- **Supervise y obtenga alertas por correo electrónico de todos los servidores de su entorno con [Azure Monitor para VM ](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview)**  
Azure Monitor, también conocido como Virtual Machines Insights, se puede usar para supervisar el mantenimiento y los eventos de los servidores, crear alertas de correo electrónico, obtener una vista consolidada del rendimiento del servidor en su entorno y visualizar las aplicaciones, los sistemas y los servicios conectados a un servidor dado.  
Para más información, consulte [Conexión se servidores a Azure Monitor y configuración de notificaciones por correo electrónico](azure-monitor.md).

- **Administre de forma centralizada las actualizaciones del sistema operativo de todos los servidores de Windows con [Azure Update Management](https://docs.microsoft.com/azure/automation/automation-update-management)**  
Puede administrar las actualizaciones y revisiones de varios servidores y máquinas virtuales desde un único lugar, en lugar de hacerlo servidor a servidor. Con Azure Update Management, puede evaluar rápidamente el estado de las actualizaciones disponibles, programar la instalación de las actualizaciones necesarias y revisar los resultados de las implementaciones, con el fin de comprobar si las actualizaciones se aplican correctamente. Esto es posible si los servidores son máquinas virtuales de Azure, hospedadas por otros proveedores de nube o en un entorno local.  
Para más información, consulte [Configuración de servidores para Azure Update Management](azure-update-management.md).

- **Conecte los servidores de locales a una red virtual de Azure con un [adaptador de red de Azure](https://aka.ms/WACNetworkAdapter)**  
Puede agregar un adaptador de red de Azure a los servidores locales, ya que ello le ayudará a conectar de forma segura el servidor de forma segura a una red virtual de Azure.  
Para más información, consulte [Configuración de una conexión VPN de punto a sitio entre un servidor de Windows local y una red virtual de Azure](https://aka.ms/WACNetworkAdapter).

- **Administre máquinas virtuales de IaaS de Azure con [Windows Admin Center](manage-azure-vms.md)**  
Puede usar Windows Admin Center para administrar tanto máquinas virtuales de Azure como máquinas locales. Al configurar la puerta de enlace de Windows Admin Center para conectarse a una red virtual de Azure, puede administrar máquinas virtuales en Azure mediante las consistentes y simplificadas herramientas que proporciona Windows Admin Center. Para más información, consulte [Configuración de Windows Admin Center para administrar máquinas virtuales en Azure](manage-azure-vms.md).

## <a name="services-for-clusters"></a>Servicios para clústeres

Estos son los servicios de Azure que proporcionan funcionalidad a los clústeres en conjunto (aún no están todos integrados en Windows Admin Center):

- [Supervisión de un clúster hiperconvergido con Azure Monitor](../../../storage/storage-spaces/configure-azure-monitor.md)
- [Protección de máquinas virtuales con Azure Site Recovery](azure-site-recovery.md)
- [Implementación de un testigo en la nube para un clúster](../../../failover-clustering/deploy-cloud-witness.md)

## <a name="see-also"></a>Consulte también

- [Conexión de Windows Admin Center con Azure](azure-integration.md)
- [Implementación de Windows Admin Center en Azure](deploy-wac-in-azure.md)