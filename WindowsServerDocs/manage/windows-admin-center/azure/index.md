---
title: Conexión de Windows Server a servicios de implementación híbrida de Azure
description: Puedes ampliar las implementaciones locales de Windows Server a la nube mediante el uso de servicios de implementación híbrida de Azure.
ms.technology: manage
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 04/12/2019
ms.openlocfilehash: 625bd4b79d277dfaa81767cd781c2ba1316d637e
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297106"
---
# Conexión de Windows Server a servicios de implementación híbrida de Azure

>Se aplica a: Windows Server 2019, Windows Server 2016

Puedes ampliar las implementaciones locales de Windows Server a la nube mediante el uso de servicios de implementación híbrida de Azure. Estos servicios en la nube proporcionan una matriz de funciones útiles, incluido lo siguiente:

- Proteger las máquinas virtuales y usar en la nube copia de seguridad y recuperación ante desastres (alta disponibilidad y recuperación ante desastres) con Azure Site Recovery. 
- Realizar un seguimiento de lo que sucede en las aplicaciones, la red y la infraestructura con la Ayuda de análisis avanzado y aprendizaje en el Monitor de Azure. 
- Simplificar la conectividad de red a Azure con adaptador de red de Azure.
- Mantener al día con la administración de actualizaciones de Azure máquinas virtuales.

Servicios de implementación híbrida de Azure funcionan con servidores de Windows en las siguientes configuraciones:

- Independientes servidores físicos y máquinas virtuales (VM)
- Clústeres, incluidos los clústeres hiperconvergidos certificados por el [HCI de pila de Azure](../../../azure-stack-hci/index.md)y los programas de [Centro (WSSD)](https://www.microsoft.com/en-us/cloud-platform/software-defined-datacenter)

Si bien puedes configurar los servicios de implementación híbrida de Azure más mediante el portal de Azure y una descarga o dos, muchos están integrados directamente en Windows Admin Center para proporcionar una experiencia de instalación simplificada y una vista centrado en el servidor de los servicios.

## Herramienta de servicios de implementación híbrida de Azure

La herramienta de servicios de implementación híbrida de Azure en [Windows Admin Center](../understand/windows-admin-center.md) consolida todos los servicios de Azure integrados en un concentrador centralizado donde puede descubrir fácilmente todos los servicios de Azure disponibles que aportan valor a tu locales o híbridas entorno. 

![Captura de pantalla de Windows Admin Center que muestra la herramienta de servicios de implementación híbrida de Azure](../media/azure-services/ahs-discover.png)

Si se conecta a un servidor con los servicios de Azure ya está habilitados, la herramienta de servicios de implementación híbrida de Azure actúa como un panel único para ver todos los servicios habilitados en ese servidor. Fácilmente puede acceder a la herramienta pertinente en Windows Admin Center, iniciar un vistazo en el portal de Azure para la administración más profundo de los servicios de Azure, o lectura más con documentación a su disposición. 

![Captura de pantalla de Windows Admin Center que muestra los servicios de Azure que ya están instalados en el servidor](../media/azure-services/ahs-dayN.png)

La herramienta de servicios de implementación híbrida de Azure, puedes:
- Copia de seguridad de Windows Server desde Windows Admin Center con [Azure Backup](azure-backup.md)
- Proteger las máquinas virtuales de Hyper-V de Windows Admin Center con [Azure Site Recovery](azure-site-recovery.md)
- Sincronizar el servidor de archivos con la nube, mediante la [Sincronización de archivos de Azure](azure-file-sync.md)
- Administrar las actualizaciones de sistema operativo para todos los servidores de Windows, tanto en local o en la nube, con la [Administración de actualizaciones de Azure](azure-update-management.md)
- Supervisar los servidores, tanto en local o en la nube y configurar alertas con el [Monitor de Azure](azure-monitor.md)
- Conectar los servidores locales a una red Virtual de Azure con [Adaptador de red de Azure](https://aka.ms/WACNetworkAdapter)

## Servicios para servidores independientes y las máquinas virtuales

Se trata de la lista completa de los servicios de Azure que proporcionan funcionalidad para servidores independientes y las máquinas virtuales:

- **(Nuevo) Sincronizar el servidor de archivos con la nube mediante el uso de [Sincronización de archivos de Azure](https://aka.ms/afs)**  
Sincronizar archivos en este servidor con recursos compartidos de archivos de Azure. Mantener todos los archivos locales o usar solo en la nube en niveles y la memoria caché de los archivos en el servidor, niveles de datos "fríos" en la nube usados con frecuencia. Los datos en la nube pueden ser copias de seguridad, lo que elimina la necesidad de preocuparse de copia de seguridad de servidor local. Además, multi-la sincronización de sitio puede mantener un conjunto de archivos sincronizados en varios servidores.

- **Agregar una capa de seguridad al centro de administración de Windows mediante la adición de autenticación de [Azure Active Directory (AD Azure)](https://azure.microsoft.com/services/active-directory/)**  
Puedes agregar una capa de seguridad adicional para Windows Admin Center por requerir que los usuarios autentican con las identidades de Azure Active Directory (AD Azure) para tener acceso a la puerta de enlace. Autenticación de Azure AD también te permite aprovechar las características de seguridad de Azure AD, como el acceso condicional y la autenticación multifactor.  
Para obtener más información, consulta [Configurar Azure AD la autenticación para Windows Admin Center.](../configure/user-access-control.md#azure-active-directory)  

- **Proteger las máquinas virtuales de Hyper-V con [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)**  
Puede replicar las cargas de trabajo que se ejecutan en máquinas virtuales para que su infraestructura empresarial crítica esté protegido en caso de desastre. Windows Admin Center simplifica el programa de instalación y el proceso de replicación de las máquinas virtuales en los servidores de Hyper-V o clústeres, lo que facilita a reforzar la resistencia de su entorno con el servicio de recuperación ante desastres de Azure Site Recovery.  
Para obtener más información, consulta [proteger las máquinas virtuales con Azure Site Recovery y Windows Admin Center](azure-site-recovery.md).

- **La copia de seguridad de los servidores de Windows con [Azure Backup](https://docs.microsoft.com/azure/backup/backup-overview)**  
Puede crear una copia de los servidores de Windows Azure, para ayudar a protegerte contra ransomware, daños y eliminaciones o de forma deliberadas.  
Para obtener más información, consulta [la copia de seguridad de los servidores con copia de seguridad de Azure](azure-backup.md).

- **Supervisar y obtener alertas de correo electrónico para todos los servidores en su entorno con el [Monitor de Azure para máquinas virtuales](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview)**  
Monitor de Azure, también conocida como información de máquinas virtuales, puedes usar para supervisar el estado del servidor y los eventos, crear alertas de correo electrónico, obtener una vista consolidada de rendimiento del servidor a través de tu entorno y visualizar las aplicaciones, los sistemas, y los servicios conectados a una determinada servidor.  
Para obtener más información, consulta [conectar los servidores al Monitor de Azure y configurar notificaciones por correo electrónico](azure-monitor.md).

- **Administrar las actualizaciones del sistema operativo de forma centralizada para todos los servidores de Windows con [Administración de actualizaciones de Azure](https://docs.microsoft.com/azure/automation/automation-update-management)**  
Puedes administrar las actualizaciones y revisiones de seguridad para varios servidores y máquinas virtuales desde un único lugar, en lugar de en cada servidor. Con la administración de actualizaciones de Azure, rápidamente puede evaluar el estado de las actualizaciones disponibles, programar la instalación de las actualizaciones necesarias y revisar los resultados de la implementación para comprobar las actualizaciones que correspondan correctamente. Es posible si los servidores son las máquinas virtuales de Azure, hospedada por otros proveedores de nube o local.  
Para obtener más información, consulta [Configurar servidores para la administración de actualizaciones de Azure](azure-update-management.md).

- **Conectar los servidores locales a una red Virtual de Azure con un [Adaptador de red de Azure](https://aka.ms/WACNetworkAdapter)**  
Puedes agregar un adaptador de red de Azure a los servidores locales que te ayudarán a conectar el servidor de forma segura a una red Virtual de Azure.  
Para obtener más información, consulta [configurar un punto a sitio la conexión VPN entre un servidor de Windows de forma local y una red Virtual de Azure](https://aka.ms/WACNetworkAdapter).

- **Administrar máquinas virtuales de IaaS de Azure con [Windows Admin Center](manage-azure-vms.md)**  
Puedes usar Windows Admin Center para administrar tus máquinas virtuales de Azure, así como los equipos locales. Mediante la configuración de la puerta de enlace de Windows Admin Center para conectarse a la VNet de Azure, puedes administrar máquinas virtuales de Azure con las herramientas coherentes y simplificadas que proporciona Windows Admin Center. Para obtener más información, consulta [Configurar Windows Admin Center para administrar máquinas virtuales en Azure](manage-azure-vms.md).

## Servicios de clústeres

Estos son los servicios de Azure que proporcionan funcionalidad a los clústeres como un todo (estos no son todos integrados en Windows Admin Center todavía):

- [Supervisar un clúster hiperconvergido con el Monitor de Azure](../../../storage/storage-spaces/configure-azure-monitor.md)
- [Proteger las máquinas virtuales con Azure Site Recovery](azure-site-recovery.md)
- [Implementación de un testigo de clúster en la nube](../../../failover-clustering/deploy-cloud-witness.md)

## Consulta también

- [Conectar Windows Admin Center a Azure](azure-integration.md)
- [Implementar Windows Admin Center en Azure](deploy-wac-in-azure.md)