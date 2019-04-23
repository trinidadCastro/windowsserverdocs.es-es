---
title: Comprender el entorno de hospedaje de escritorio
description: Información general de un deployhment RDS con IaaS de Azure.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 08/01/2016
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 880fc8f9fa2db5ec56d2117e02c069650c61584a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877926"
---
# <a name="understanding-the-desktop-hosting-environment"></a>Comprender el entorno de hospedaje de escritorio

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

La siguiente información describe los componentes del servicio de hospedaje de escritorio.  
  
## <a name="tenant-environment"></a>Entorno de inquilinos  
Servicio del proveedor de hospedaje de escritorio se implementa como un conjunto de entornos de inquilinos aislados. Entorno de cada inquilino consta de un contenedor de almacenamiento, un conjunto de máquinas virtuales y una combinación de servicios de Azure, toda comunicación a través de una red virtual aislada. Cada máquina virtual contiene uno o varios de los componentes que constituyen el entorno de escritorio hospedado del inquilino. Las siguientes subsecciones describen los componentes que constituyen el entorno de escritorio hospedado de cada inquilino.

## <a name="remote-desktop-services"></a>Servicios de Escritorio remoto
En un entorno de hospedaje de escritorio, se instalan los siguientes roles de servicios de escritorio remoto entre varias máquinas virtuales:

  - Agente de conexión a Escritorio remoto
  - Puerta de enlace de Escritorio remoto
  - Administración de licencias de Escritorio remoto
  - Host de sesión de Escritorio remoto
  - Acceso Web a Escritorio remoto

Para obtener una descripción completa de cada uno de estos roles y cómo interactúan entre sí, revise el [roles descripción RDS](Understanding-RDS-roles.md) documento.
  
##  <a name="azure-active-directory-domain-services"></a>(Azure) Servicios de dominio de Active Directory  
Hay varias maneras de conectarse y administrar los servicios de dominio de Active Directory (AD DS) para un entorno de hospedaje de escritorio en Azure:

1. Crear una máquina virtual en el entorno del inquilino que ejecuta el rol de AD DS
2. Crear una conexión VPN de sitio a sitio con el entorno local del inquilino para usar una instancia existente AD DS
3. Usar el rol de PaaS de Azure AD Domain Services, que crea un dominio de red virtual del inquilino según de Azure Active Directory del inquilino

Con servicios de escritorio remoto, el inquilino debe tener un Active Directory para administrar el acceso en el entorno de almacenamiento de información de perfil de usuario y la supervisión en la implementación. Cuando se usa estándar (que no son de Azure) AD DS, bosque del inquilino no requiere ninguna relación de confianza con el bosque de administración del proveedor. Una cuenta de administrador de dominio puede configurarse en el dominio del inquilino para permitir que el personal técnico del proveedor para realizar tareas administrativas en el entorno del inquilino (por ejemplo, supervisar el estado del sistema y aplicar las actualizaciones de software) y ayudar a con solución de problemas y la configuración.  
    
Información adicional:  
[Documentación de servicios de dominio de Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory-ds/)  
[Instalar un nuevo bosque de Active Directory en una red virtual de Azure](https://azure.microsoft.com/documentation/articles/active-directory-new-forest-virtual-machine/)  
[Crear un recurso en el Administrador de red virtual con una conexión VPN de sitio a sitio mediante el Portal de Azure](https://azure.microsoft.com/documentation/articles/vpn-gateway-howto-site-to-site-resource-manager-portal/)  
  
## <a name="azure-sql-database"></a>Se aplica a: Base de datos SQL de Azure  
Azure SQL Database permite a los proveedores de hospedaje ampliar su implementación de servicios de escritorio remoto sin necesidad de implementar y mantener un clúster de SQL Server Always-On completo. La base de datos de SQL Azure usando el agente de conexión de escritorio remoto para almacenar información de implementación, como la asignación de conexiones de los usuarios actuales con servidores de host del extremo. Al igual que otros servicios de Azure, Azure SQL DB sigue un modelo de consumo, ideal para la implementación de cualquier tamaño.   
  
Información adicional:  
[¿Qué es la base de datos SQL?](https://azure.microsoft.com/documentation/articles/sql-database-technical-overview/)  
  
## <a name="azure-active-directory-application-proxy"></a>Proxy de aplicación de Azure Active Directory  
Azure Active Directory Application Proxy es un servicio proporcionado en el pago de SKU de Azure Active Directory que permiten a los usuarios conectarse a las aplicaciones internas a través del servicio de proxy inverso de Azure. Esto permite que los puntos de conexión Web a Escritorio remoto y puerta de enlace de escritorio remoto esté oculto dentro de la red virtual, lo que elimina la necesidad de estar expuestos a internet a través de una dirección IP pública. Además, esto permite a los proveedores de hospedaje condensar el número de máquinas virtuales en el entorno del inquilino manteniendo una implementación completa.
  
Información adicional:  
[Habilitación del Proxy de aplicación de Azure AD](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-enable/)  
    
## <a name="file-server"></a>Servidor de archivos  
El servidor de archivos proporciona las carpetas compartidas mediante el protocolo bloque de mensajes del servidor (SMB) 3.0. Las carpetas compartidas se usan para crear y almacenar archivos de disco de perfil de usuario (.vhdx), a los datos de copia de seguridad y para permitir que a los usuarios un lugar para compartir datos con otros usuarios de red virtual del inquilino.
  
La máquina virtual que se usa para implementar el servidor de archivos debe tener un disco de datos de Azure conectada y configurada con carpetas compartidas. Discos de datos de Azure usan caché de escritura simultánea que garantiza que se escribe en el disco se conservan entre reinicios de la máquina virtual.  
  
Para inquilinos pequeños, se puede reducir el costo mediante la combinación del servidor de archivos con la máquina virtual que se ejecutan los roles de agente de conexión a Escritorio remoto y licencias de escritorio remoto en una sola máquina virtual en el entorno del inquilino.  
  
Información adicional  
[Información general de servicios de almacenamiento y de archivo](https://technet.microsoft.com/library/hh831487.aspx)  
[Cómo conectar un disco de datos a una máquina Virtual](http://www.windowsazure.com/manage/windows/how-to-guides/attach-a-disk/)  
  
### <a name="user-profile-disks"></a>Discos de perfil de usuario  
Discos de perfil de usuario permiten a los usuarios guardar archivos y configuraciones personales al iniciar sesión una sesión en un servidor Host de sesión de escritorio remoto en una colección y, a continuación, tengan acceso a la misma configuración y los archivos al iniciar sesión en otro servidor Host de sesión de escritorio remoto de la colección. Cuando el usuario primero inicia sesión, se crea un disco de perfil de usuario en el servidor de archivos del inquilino y que el disco montado en el servidor Host de sesión de escritorio remoto al que está conectado el usuario. En cada posteriores inicios de sesión, se monta el disco de perfil de usuario en el servidor host de sesión de escritorio remoto adecuado, y con cada cierre de sesión, es no montada. El contenido del disco de perfil solo se puede acceder ese usuario.  
  


