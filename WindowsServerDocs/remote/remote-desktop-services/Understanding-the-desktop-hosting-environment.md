---
title: Introducción al entorno de hospedaje de escritorio
description: Información general de una implementación de RDS con IaaS de Azure.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 08/01/2016
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 8fdebcad1370e06c19752944e85363c714f1fbcd
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2020
ms.locfileid: "80854698"
---
# <a name="understanding-the-desktop-hosting-environment"></a>Introducción al entorno de hospedaje de escritorio

>Se aplica a: Windows Server (Canal semianual), Windows Server 2019 y Windows Server 2016

La siguiente información describe los componentes del servicio de hospedaje de escritorio.  
  
## <a name="tenant-environment"></a>Entorno de inquilino  
El servicio de hospedaje de escritorio del proveedor se implementa como un conjunto de entornos de inquilino aislados. El entorno de cada inquilino consta de un contenedor de almacenamiento, un conjunto de máquinas virtuales y una combinación de servicios de Azure, donde todos se comunican a través de una red virtual aislada. Cada máquina virtual contiene uno o varios de los componentes que constituyen el entorno de escritorio hospedado del inquilino. En los siguientes apartados se describen los componentes que constituyen el entorno de escritorio hospedado de cada inquilino.

## <a name="remote-desktop-services"></a>Servicios de Escritorio remoto
En un entorno de hospedaje de escritorio, se instalan los siguientes roles de Servicios de Escritorio remoto entre varias máquinas virtuales:

  - Agente de conexión a Escritorio remoto
  - Puerta de enlace de Escritorio remoto
  - Administración de licencias de Escritorio remoto
  - Host de sesión de escritorio remoto
  - Acceso web de Escritorio remoto

Para obtener una descripción completa de cada uno de estos roles y cómo interactúan entre sí, revisa el documento [Roles de RDS](Understanding-RDS-roles.md).
  
##  <a name="azure-active-directory-domain-services"></a>(Azure) Active Directory Domain Services  
Hay varias maneras de conectarse y administrar los Active Directory Domain Services (AD DS) para un entorno de hospedaje de escritorio en Azure:

1. Crea una máquina virtual en el entorno del inquilino que ejecuta el rol de AD DS.
2. Crea una conexión VPN de sitio a sitio con el entorno local del inquilino para usar una instancia existente de AD DS.
3. Usa el rol de PaaS de Azure AD Domain Services, que crea un dominio en la red virtual del inquilino según la instancia de Azure Active Directory del inquilino.

Con Servicios de Escritorio remoto, el inquilino debe tener un Active Directory para administrar el acceso al entorno, almacenamiento de perfiles de usuario y supervisión dentro de la implementación. Cuando se usa AD DS estándar (no de Azure), el bosque del inquilino no requiere ninguna relación de confianza con el bosque de administración del proveedor. Puede configurarse una cuenta de administrador de dominio en el dominio del inquilino para permitir que el personal técnico del proveedor realice tareas administrativas en el entorno del inquilino (por ejemplo, supervisar el estado del sistema y aplicar las actualizaciones de software) y ayude con la solución de problemas y la configuración.  
    
Información adicional:  
[Documentación de Active Directory Domain Services en Azure](https://azure.microsoft.com/documentation/services/active-directory-ds/)  
[Instalar un nuevo bosque de Active Directory en una red virtual de Azure](https://azure.microsoft.com/documentation/articles/active-directory-new-forest-virtual-machine/)  
[Creación de una red virtual de Resource Manager con conexión VPN de sitio a sitio mediante Azure Portal](https://azure.microsoft.com/documentation/articles/vpn-gateway-howto-site-to-site-resource-manager-portal/)  
  
## <a name="azure-sql-database"></a>Base de datos SQL de Azure  
Azure SQL Database permite a los proveedores de servicios de hosting ampliar su implementación de Servicios de Escritorio remoto sin necesidad de implementar y mantener un clúster de SQL Server Always-On completo. El Agente de conexión a Escritorio remoto usa Azure SQL Database para almacenar información de implementación, como la asignación de conexiones de los usuarios actuales con los servidores de host final. Al igual que otros servicios de Azure, Azure SQL DB sigue un modelo de consumo, ideal para implementaciones de cualquier tamaño.   
  
Información adicional:  
[¿Qué es SQL Database?](https://azure.microsoft.com/documentation/articles/sql-database-technical-overview/)  
  
## <a name="azure-active-directory-application-proxy"></a>Azure Active Directory Application Proxy  
Azure Active Directory Application Proxy es un servicio proporcionado en SKU de pago de Azure Active Directory, que permite a los usuarios conectarse a aplicaciones internas a través del servicio propio de proxy inverso de Azure. Esto permite que los puntos de conexión web de Escritorio remoto y de Puerta de enlace de Escritorio remoto estén ocultos dentro de la red virtual, lo que elimina la necesidad de exponerlos a Internet a través de una dirección IP pública. Además, esto permite a los proveedores de servicios de hosting condensar el número de máquinas virtuales en el entorno del inquilino, a la vez que mantienen una implementación completa.
  
Información adicional:  
[Habilitación de Azure AD Application Proxy](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-enable/)  
    
## <a name="file-server"></a>Servidor de archivos  
El servidor de archivos proporciona las carpetas compartidas mediante el protocolo Bloque de mensajes del servidor (SMB) 3.0. Las carpetas compartidas se usan para crear y almacenar archivos de disco de perfiles de usuario (.vhdx), para hacer copias de seguridad de los datos y para brindar a los usuarios un lugar para compartir datos con otros usuarios de la red virtual del inquilino.
  
La VM que se usa para implementar el servidor de archivos debe tener un disco de datos de Azure conectado y configurado con carpetas compartidas. Los discos de datos de Azure usan caché de escritura simultánea que garantiza que la escritura en el disco persista entre reinicios de la VM.  
  
Para inquilinos pequeños, el costo se puede reducir al combinar el servidor de archivos con la máquina virtual que se ejecuta los roles de Agente de conexión a Escritorio remoto y de licencias de Escritorio remoto en una sola máquina virtual en el entorno del inquilino.  
  
Información adicional  
[Introducción a los servicios de archivos y almacenamiento](https://technet.microsoft.com/library/hh831487.aspx)  
[Conexión de un disco de datos a una máquina virtual](http://www.windowsazure.com/manage/windows/how-to-guides/attach-a-disk/)  
  
### <a name="user-profile-disks"></a>Discos de perfiles de usuario  
Los discos de perfil de usuario permiten a los usuarios guardar archivos y configuraciones personales cuando inician una sesión en un servidor host de sesión de Escritorio remoto en una colección para que, luego, tengan acceso a la misma configuración y archivos al iniciar sesión en otro servidor host de sesión de Escritorio remoto de la colección. Cuando el usuario inicia sesión por primera vez, se crea un disco de perfil de usuario en el servidor de archivos del inquilino, y ese disco se monta en el servidor host de sesión de Escritorio remoto al que se conecta el usuario. En cada inicio de sesión posterior, se monta el disco de perfil de usuario en el servidor host de sesión de Escritorio remoto adecuado y, con cada cierre de sesión, se desmonta. Solo ese usuario puede acceder al contenido del disco de perfil.  
  


