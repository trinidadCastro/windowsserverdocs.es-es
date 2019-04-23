---
title: Servicio de hospedaje de escritorio
description: Describe los componentes de un servicio de hospedaje de escritorio.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 07/06/2018
ms.tgt_pltfrm: na
ms.topic: article
author: heidilohr
manager: dougkim
ms.openlocfilehash: adbb9fd69bc61d2e877cadb0484a4e42093f262a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840476"
---
# <a name="desktop-hosting-service"></a>Servicio de hospedaje de escritorio

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Este artículo explica más información acerca de lo componentes del servicio de hospedaje de escritorio.

## <a name="tenant-environment"></a>Entorno de inquilinos

Como se describe en [roles de servicios de escritorio remoto](rds-roles.md), cada función que desempeña una parte distinta en el entorno de inquilino.

Servicio del proveedor de hospedaje de escritorio se implementa como un conjunto de entornos de inquilinos aislados. Entorno de cada inquilino consta de un contenedor de almacenamiento, un conjunto de máquinas virtuales y una combinación de servicios de Azure, toda comunicación a través de una red virtual aislada. Cada máquina virtual contiene uno o varios de los componentes que constituyen el entorno de escritorio hospedado del inquilino. Las siguientes subsecciones describen los componentes que constituyen el entorno de escritorio hospedado de cada inquilino.

## <a name="active-directory-domain-services"></a>Active Directory Domain Services

Los servicios de dominio de Active Directory (AD DS) proporciona la información de dominio y bosque, tal que los usuarios del inquilino pueden iniciar sesión en los equipos de escritorio y aplicaciones para llevar a cabo sus cargas de trabajo. Esto también le permite configurar o conectarse a recursos compartidos de archivos necesarios y las bases de datos que pueden ser necesarios para las aplicaciones de Windows.

Bosque del inquilino no requiere ninguna relación de confianza con el bosque de administración del proveedor. Una cuenta de administrador de dominio puede configurarse en el dominio del inquilino para permitir que el personal técnico del proveedor para realizar tareas administrativas en el entorno del inquilino (por ejemplo, supervisar el estado del sistema y aplicar las actualizaciones de software) y ayudar a con solución de problemas y la configuración.

Hay varias maneras de implementar AD DS:

1. Habilitar Azure Active Directory Domain Services en el entorno de red virtual del inquilino. Esto creará una instancia administrada de AD DS para el inquilino en función de los usuarios y grupos que existen en Azure AD.
2. Configurar un servidor de AD DS independiente en el entorno de red virtual del inquilino. Esto le ofrece todo el control completo de la instancia de AD DS que se ejecutan en máquinas virtuales.
3. Crear una conexión VPN de sitio a sitio a un servidor de AD DS que se encuentra en las instalaciones del inquilino. Esto permite que el inquilino para conectarse a su instancia de AD DS existente y reducir la duplicación de los usuarios, grupos, unidades organizativas y así sucesivamente.

Para obtener más información, consulta los artículos siguientes:

* [Documentación de Azure Active Directory Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/)
* [Guía de arquitectura de referencia para Windows Server 2012 R2 de hospedaje de escritorio](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)
* [Crear una conexión de sitio a sitio en el portal de Azure](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)

## <a name="sql-database"></a>Base de datos SQL

Una base de datos de alta disponibilidad SQL está usando el agente de conexión de escritorio remoto para almacenar información de implementación, como la asignación de conexiones de los usuarios actuales a los servidores host.

Hay varias maneras de implementar una base de datos SQL:

1. Crear una base de datos de SQL Azure en el entorno del inquilino. Esto le brinda la funcionalidad de una base de datos con redundancia de SQL sin tener que administrar los servidores. Esto también permite que se pague por lo que consume en lugar de invertir en infraestructura.
2. Crear un clúster de SQL Server AlwaysOn. Esto le permite aprovechar la infraestructura existente de SQL Server y le ofrece control completo sobre las instancias de SQL Server.

Para obtener más información acerca de cómo configurar una infraestructura altamente disponible de base de datos SQL, consulte los artículos siguientes:

* [¿Qué es el servicio de base de datos de SQL Azure?](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview)
* [Creación y configuración de grupos de disponibilidad (SQL Server)](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server?view=sql-server-2017).
* [Agregar el servidor de agente de conexión a Escritorio remoto a la implementación y configuración de alta disponibilidad](rds-connection-broker-cluster.md).

## <a name="file-server"></a>Servidor de archivos

El servidor de archivos usa el protocolo de bloque de mensajes del servidor (SMB) 3.0 para proporcionar las carpetas compartidas. Estas carpetas compartidas se usan para crear y almacenar archivos de disco de perfil de usuario (.vhdx) para realizar una copia de seguridad de los datos y permitir que los usuarios compartir datos entre sí del inquilino servicio en nube.

La máquina virtual que implementa el servidor de archivos debe tener un disco de datos de Azure conectada y configurada con carpetas compartidas. Los discos de datos de Azure usar caché de escritura simultánea, lo que garantiza que no se borrarán escrituras en el disco cada vez que se reinicia la máquina virtual.

Los inquilinos pequeños pueden reducir los costos mediante la combinación en el servidor de archivos y [rol licencias de escritorio remoto](rds-roles.md#remote-desktop-licensing) en una sola máquina virtual en el entorno del inquilino.

Para obtener más información, consulta los artículos siguientes:

* [Almacenamiento en Windows Server](../../storage/storage.md)
* [Cómo conectar un disco de datos administrado a una máquina virtual de Windows en Azure portal](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Fclassic%2Ftoc.json)

### <a name="user-profile-disks"></a>Discos de perfil de usuario

Discos de perfil de usuario permiten a los usuarios guardar archivos y configuraciones personales al iniciar sesión una sesión en un servidor Host de sesión de escritorio remoto en una colección y, después, tener acceso a las mismas configuraciones y archivos al iniciar sesión en otro [RD Session Host](rds-roles.md#remote-desktop-session-host) servidor de la colección. Cuando el usuario primero inicia sesión, servidor de archivos del inquilino crea un disco de perfil de usuario que se monta en el servidor Host de sesión de escritorio remoto que está conectado actualmente al usuario. En cada posteriores inicios de sesión, se monta el disco de perfil de usuario en el servidor host de sesión de escritorio remoto apropiado y está desmontado con cada cierre de sesión. Solo el usuario puede tener acceso a contenido del disco de perfil.