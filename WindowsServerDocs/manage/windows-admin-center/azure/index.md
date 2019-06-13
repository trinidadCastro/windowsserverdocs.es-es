---
title: Conectar el servidor de Windows a servicios híbridos de Azure
description: Puede extender las implementaciones locales de Windows Server a la nube mediante el uso de servicios híbridos de Azure.
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
# <a name="connecting-windows-server-to-azure-hybrid-services"></a>Conectar el servidor de Windows a servicios híbridos de Azure

>Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server (canal semianual)

Puede extender las implementaciones locales de Windows Server a la nube mediante el uso de servicios híbridos de Azure. Estos servicios en la nube proporcionan una matriz de funciones útiles, incluidas las siguientes:

- Proteger las máquinas virtuales y usar en la nube copia de seguridad y recuperación ante desastres (HA/DR) con Azure Site Recovery. 
- Realizar un seguimiento de qué está sucediendo en sus aplicaciones, la red y la infraestructura con la Ayuda de análisis avanzado y aprendizaje automático en Azure Monitor. 
- Simplificar la conectividad de red en Azure con el adaptador de red de Azure.
- Mantener las máquinas virtuales al día con la administración de actualizaciones de Azure.

Servicios híbridos de Azure funcionan con servidores de Windows en las siguientes configuraciones:

- Servidores físicos independientes y máquinas virtuales (VM)
- Clústeres, incluidos los clústeres hiperconvergidos certificados por la [Azure Stack HCI](https://docs.microsoft.com/azure-stack/operator/azure-stack-hci-overview), y [Windows Server Software-Defined (WSSD)](https://www.microsoft.com/en-us/cloud-platform/software-defined-datacenter) programas

Aunque puede configurar los servicios híbridos de Azure más mediante el portal de Azure y una descarga o dos, muchos están integradas directamente en Windows Admin Center para proporcionar una experiencia de instalación simplificada y una vista centrada en el servidor de los servicios.

## <a name="azure-hybrid-services-tool"></a>Herramienta de servicios de híbrida de Azure

Herramienta de los servicios de Azure híbrido en [Windows Admin Center](../understand/windows-admin-center.md) consolida todos los servicios de Azure integrados en un concentrador centralizado donde podrá detectar fácilmente todos los servicios de Azure disponibles que aportan valor a sus instalaciones o entorno híbrido. 

![Captura de pantalla de Windows Admin Center que muestra la herramienta servicios híbridos de Azure](../media/azure-services/ahs-discover.png)

Si se conecta a un servidor con servicios de Azure ya está habilitados, la herramienta de servicios de híbrida de Azure actúa como un único panel de vidrio para ver todos los servicios habilitados en ese servidor. Fácilmente puede llegar a la herramienta pertinente dentro de Windows Admin Center, inicie en el portal de Azure para la administración más profunda de los servicios de Azure o lea más documentación a su alcance. 

![Captura de pantalla de Windows Admin Center que muestra los servicios de Azure que ya están instalados en el servidor](../media/azure-services/ahs-dayN.png)

Desde la herramienta Servicios híbrido de Azure, hacer lo siguiente:
- Copia de seguridad de Windows Server desde Windows Admin Center con [copia de seguridad de Azure](azure-backup.md)
- Proteger las máquinas virtuales de Hyper-V de Windows Admin Center con [Azure Site Recovery](azure-site-recovery.md)
- Sincronizar el servidor de archivos con la nube, mediante [Azure File Sync](azure-file-sync.md)
- Administrar las actualizaciones de sistema operativo de todos los servidores de Windows, tanto en el entorno local o en la nube, con [Update Management de Azure](azure-update-management.md)
- Supervisar los servidores, tanto en el entorno local o en la nube y configurar alertas con [Azure Monitor](azure-monitor.md)
- Conectar los servidores de un entorno local a Azure Virtual Network con [adaptador de red de Azure](https://aka.ms/WACNetworkAdapter)

## <a name="services-for-stand-alone-servers-and-vms"></a>Servicios para máquinas virtuales y servidores independientes

Se trata de una lista completa de los servicios de Azure que proporcionan la funcionalidad en servidores independientes y máquinas virtuales:

- **(Nuevo) Sincronizar el servidor de archivos con la nube mediante [Azure File Sync](https://aka.ms/afs)**  
Sincronizar archivos en este servidor con recursos compartidos de archivos de Azure. Mantener todos los archivos locales o uso en la nube los niveles y la memoria caché solo los archivos en el servidor, los niveles de datos inactivos en la nube usados con frecuencia. Datos en la nube pueden ser una copia de seguridad, lo que elimina la necesidad de preocuparse de copia de seguridad de servidor local. Además, sincronización de varios sitios puede mantener un conjunto de archivos sincronizados entre varios servidores.

- **Agregar una capa de seguridad a Windows Admin Center agregando [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/) autenticación**  
Puede agregar una capa adicional de seguridad para Windows Admin Center al requerir que los usuarios se autentiquen mediante identidades de Azure Active Directory (Azure AD) para tener acceso a la puerta de enlace. Autenticación de Azure AD también le permite aprovechar las características de seguridad de Azure AD como el acceso condicional y la autenticación multifactor.  
Para obtener más información, consulte [autenticación configurar Azure AD para Windows Admin Center.](../configure/user-access-control.md#azure-active-directory)  

- **Proteger las máquinas virtuales de Hyper-V con [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)**  
Puede replicar las cargas de trabajo que se ejecutan en máquinas virtuales para que su infraestructura crítico para la empresa estén protegido en caso de desastre. Windows Admin Center simplifica el programa de instalación y el proceso de replicar las máquinas virtuales en los servidores de Hyper-V o clústeres, lo que facilita que refuerzan la resistencia de su entorno con el servicio de recuperación ante desastres de Azure Site Recovery.  
Para obtener más información, consulte [protege las máquinas virtuales con Azure Site Recovery y Windows Admin Center](azure-site-recovery.md).

- **Copia de seguridad de los servidores de Windows con [Azure Backup](https://docs.microsoft.com/azure/backup/backup-overview)**  
Puede realizar copias de seguridad de los servidores de Windows en Azure, lo que ayuda a protegerse del ransomware, daños y eliminaciones accidentales o malintencionadas.  
Para obtener más información, consulte [copia de seguridad de sus servidores con Azure Backup](azure-backup.md).

- **Supervise y obtenga alertas por correo electrónico para todos los servidores en su entorno con [Azure Monitor para las máquinas virtuales](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview)**  
Puede usar Azure Monitor, también conocido como información de las máquinas virtuales, para supervisar el estado del servidor y eventos, crear alertas de correo electrónico, obtener una vista consolidada del rendimiento del servidor en un entorno y visualizar las aplicaciones, sistemas, y servicios conectados a un determinado servidor.  
Para obtener más información, consulte [conectar los servidores a Azure Monitor y configurar notificaciones por correo electrónico](azure-monitor.md).

- **Administrar centralmente las actualizaciones del sistema operativo para todos los servidores de Windows con [Update Management de Azure](https://docs.microsoft.com/azure/automation/automation-update-management)**  
Puede administrar las actualizaciones y revisiones para varios servidores y máquinas virtuales desde un único lugar, en lugar de en cada servidor. Con Update Management de Azure, rápidamente puede evaluar el estado de las actualizaciones disponibles, programar la instalación de las actualizaciones necesarias y revisar los resultados de implementación para comprobar si las actualizaciones que se aplican correctamente. Es posible si los servidores son máquinas virtuales de Azure, hospedado por otros proveedores de nube o local.  
Para obtener más información, consulte [configurar servidores para la administración de actualizaciones de Azure](azure-update-management.md).

- **Conectar los servidores de un entorno local a Azure Virtual Network con un [adaptador de red de Azure](https://aka.ms/WACNetworkAdapter)**  
Puede agregar un adaptador de red de Azure a los servidores locales que le ayudarán a conectar el servidor de forma segura a una red Virtual de Azure.  
Para obtener más información, consulte [configurar una conexión VPN de punto a sitio entre un servidor de Windows local y Azure Virtual Network](https://aka.ms/WACNetworkAdapter).

- **Administrar máquinas virtuales de IaaS de Azure con [Windows Admin Center](manage-azure-vms.md)**  
Puede usar Windows Admin Center para administrar sus máquinas virtuales de Azure, así como en las máquinas locales. Al configurar la puerta de enlace de Windows Admin Center para conectarse a la red virtual de Azure, puede administrar máquinas virtuales en Azure con las herramientas coherentes y simplificadas que proporciona Windows Admin Center. Para obtener más información, consulte [configurar Windows Admin Center para administrar máquinas virtuales en Azure](manage-azure-vms.md).

## <a name="services-for-clusters"></a>Servicios para los clústeres

Estos son los servicios de Azure que proporcionan funcionalidad a los clústeres como un todo (estos no son todas integrados en Windows Admin Center todavía):

- [Supervisar un clúster hiperconvergido con Azure Monitor](../../../storage/storage-spaces/configure-azure-monitor.md)
- [Proteger las máquinas virtuales con Azure Site Recovery](azure-site-recovery.md)
- [Implementación de un testigo de clúster en la nube](../../../failover-clustering/deploy-cloud-witness.md)

## <a name="see-also"></a>Consulte también

- [Conectar Windows Admin Center en Azure](azure-integration.md)
- [Implementar Windows Admin Center en Azure](deploy-wac-in-azure.md)