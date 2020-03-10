---
title: Servicio de hospedaje de escritorio
description: Describe los componentes de un servicio de hospedaje de escritorio.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 07/06/2018
ms.tgt_pltfrm: na
ms.topic: article
author: heidilohr
manager: lizross
ms.openlocfilehash: cf189b15ca15fb556424b5e4931f19d4be356d4d
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2020
ms.locfileid: "78370779"
---
# <a name="desktop-hosting-service"></a>Servicio de hospedaje de escritorio

>Se aplica a: Windows Server (Canal semianual), Windows Server 2019 y Windows Server 2016

Este artículo te proporciona más información acerca de los componentes del servicio de hospedaje de escritorio.

## <a name="tenant-environment"></a>Entorno de inquilino

Como se indica en [Roles de servicio de Escritorio remoto](rds-roles.md), cada rol juega un papel diferente en el entorno del inquilino.

El servicio de hospedaje de escritorio del proveedor se implementa como un conjunto de entornos de inquilino aislados. El entorno de cada inquilino consta de un contenedor de almacenamiento, un conjunto de máquinas virtuales y una combinación de servicios de Azure, donde todos se comunican a través de una red virtual aislada. Cada máquina virtual contiene uno o varios de los componentes que constituyen el entorno de escritorio hospedado del inquilino. En los siguientes apartados se describen los componentes que constituyen el entorno de escritorio hospedado de cada inquilino.

## <a name="active-directory-domain-services"></a>Servicios de dominio de Active Directory

Active Directory Domain Services (AD DS) proporciona información sobre el dominio y el bosque de tal forma que los usuarios del inquilino pueden iniciar sesión en los escritorios y aplicaciones para realizar sus cargas de trabajo. Esto también te permite configurar los recursos compartidos de archivos y bases de datos (o conectarte a ellos) que necesitan las aplicaciones de Windows.

El bosque del inquilino no requiere ninguna relación de confianza con el bosque de administración del proveedor. Puede configurarse una cuenta de administrador de dominio en el dominio del inquilino para permitir que el personal técnico del proveedor realice tareas administrativas en el entorno del inquilino (por ejemplo, supervisar el estado del sistema y aplicar las actualizaciones de software) y ayude con la solución de problemas y la configuración.

Hay varias maneras de implementar AD DS:

1. Habilita Azure Active Directory Domain Services en el entorno de red virtual del inquilino. Esto creará una instancia administrada de AD DS para el inquilino basada en los usuarios y grupos que existen en Azure AD.
2. Configura un servidor de AD DS independiente en el entorno de red virtual del inquilino. Esto te proporciona el control completo de la instancia de AD DS en ejecución en las máquinas virtuales.
3. Crea una conexión VPN de sitio a sitio a un servidor de AD DS que se encuentre en las instalaciones del inquilino. Esto permite que el inquilino se conecte a su instancia de AD DS existente y esto reduzca la duplicación de usuarios, grupos, unidades organizativas, etc.

Para obtener más información, consulta los artículos siguientes:

* [Documentación de Azure Active Directory Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/)
* [Guía de arquitectura de referencia de hospedaje de escritorio para Windows Server 2012 R2](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)
* [Creación de una conexión de sitio a sitio en Azure Portal](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)

## <a name="sql-database"></a>Base de datos SQL

El Agente de conexión a Escritorio remoto usa una base de datos SQL altamente disponible para almacenar información de implementación, como la asignación de conexiones de los usuarios actuales con los servidores de host.

Hay varias maneras de implementar una base de datos SQL:

1. Crea una base de datos de Azure SQL en el entorno del inquilino. Esto te brinda la funcionalidad de una base de datos SQL con redundancia sin tener que administrar los servidores en sí mismos. Esto también te permite pagar por lo que consumes en lugar de tener que invertir en infraestructura.
2. Crea un clúster de AlwaysOn para SQL Server. Esto te permite aprovechar la infraestructura existente de SQL Server y te ofrece un control completo sobre las instancias de SQL Server.

Para más información acerca de cómo configurar una infraestructura de base datos SQL con alta disponibilidad, consulta los siguientes artículos:

* [¿Qué es el servicio Azure SQL Database?](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview)
* [Creación y configuración de grupos de disponibilidad (SQL Server)](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server?view=sql-server-2017).
* [Adición del servidor de Agente de conexión a Escritorio remoto para la implementación y la configuración de alta disponibilidad](rds-connection-broker-cluster.md).

## <a name="file-server"></a>Servidor de archivos

El servidor de archivos proporciona las carpetas compartidas mediante el protocolo Bloque de mensajes del servidor (SMB) 3.0. Las carpetas compartidas se usan para crear y almacenar archivos de disco de perfiles de usuario (.vhdx), para hacer copias de seguridad de los datos y para permitir a los usuarios compartir datos entre sí dentro del servicio en la nube del inquilino.

La máquina virtual que implementa el servidor de archivos debe tener un disco de datos de Azure conectado y configurado con carpetas compartidas. Los discos de datos de Azure usan caché de escritura simultánea lo que garantiza que las escrituras en el disco no se eliminarán cuando la máquina virtual se reinicie.

Los inquilinos pequeños pueden reducir los costos combinando el servidor de archivos y el [rol de administración de licencias de Escritorio remoto](rds-roles.md#remote-desktop-licensing) en una única máquina virtual del entorno del inquilino.

Para obtener más información, consulta los artículos siguientes:

* [Almacenamiento en Windows Server](../../storage/storage.md)
* [Conexión de un disco de datos administrado a una máquina virtual Windows en Azure Portal](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Fclassic%2Ftoc.json)

### <a name="user-profile-disks"></a>Discos de perfiles de usuario

Los discos de perfiles de usuario permiten a los usuarios guardar archivos y configuraciones personales cuando inician una sesión en un servidor host de sesión de Escritorio remoto en una colección para que, luego, puedan acceder a la misma configuración y archivos al iniciar sesión en otro [servidor host de sesión de Escritorio remoto](rds-roles.md#remote-desktop-session-host) de la colección. Cuando el usuario inicia sesión por primera vez, el servidor de archivos del inquilino crea un disco de perfiles de usuario que se monta en el servidor host de sesión de Escritorio remoto al que está conectado actualmente el usuario. En cada inicio de sesión posterior, se monta el disco de perfil de usuario en el servidor host de sesión de Escritorio remoto adecuado y, con cada cierre de sesión, se desmonta. Solo el usuario puede acceder al contenido del disco de perfiles.