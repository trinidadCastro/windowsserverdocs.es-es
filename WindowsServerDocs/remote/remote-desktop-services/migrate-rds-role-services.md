---
title: Migración de la implementación de Servicios de Escritorio remoto en Windows Server 2016
description: En este artículo se describe cómo migrar la implementación de Servicios de Escritorio remoto a los nuevos servidores de Windows Server 2016.
ms.author: chrimo
ms.date: 11/01/2016
ms.topic: article
ms.assetid: 9b1fa833-4325-48a8-bf34-46265f40c001
author: christianmontoya
manager: scottman
ms.openlocfilehash: 62c2cc99277b3cf74f6bde5be59b69569c27a31b
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87961819"
---
# <a name="migrate-your-remote-desktop-services-deployment-to-windows-server-2016"></a>Migración de la implementación de Servicios de Escritorio remoto en Windows Server 2016

Si actualmente ejecutas Servicios de Escritorio remoto en Windows Server 2012 R2, puedes moverte a Windows Server 2016 para aprovechar las nuevas características, como la compatibilidad con Azure SQL y Espacios de almacenamiento directo.

Se admite la migración de una implementación de Servicios de Escritorio remoto desde servidores de origen que ejecutan Windows Server 2016 a servidores de destino que ejecutan Windows Server 2016. En otras palabras, no hay ninguna migración local directa de RDS en Windows Server 2012 R2 a Windows Server 2016. En su lugar, para la mayoría de los componentes de RDS, primero se actualiza a Windows Server 2016 y, a continuación, se migran los datos y las licencias. Los únicos componentes con una migración directa son web de Escritorio remoto, puerta de enlace de Escritorio remoto y el servidor de licencias.

Para obtener más información sobre los requisitos y el proceso de actualización, consulta [Actualizar las implementaciones de Servicios de Escritorio remoto a Windows Server 2016](./upgrade-to-rds.md).

Lleva a cabo los siguientes pasos para migrar las implementaciones de Servicios de Escritorio remoto:

- [Migrar los servidores de agente de conexión a Escritorio remoto](#migrate-rdconnection-broker-servers)

- [Migrar las colecciones de sesiones](#migrate-session-collections)

- [Migrar las colecciones de escritorios virtuales](#migrate-virtual-desktop-collections)

- [Migrar los servidores de Acceso web de Escritorio remoto](#migrate-rdweb-access-servers)

- [Migrar los servidores de Puerta de enlace de Escritorio remoto](#migrate-rdgateway-servers)

- [Migrar los servidores de Administración de licencias de Escritorio remoto](#migrate-rdgateway-servers)

- [Migrar los certificados](#migrate-certificates)

## <a name="migrate-rdconnection-broker-servers"></a>Migrar los servidores de agente de conexión a Escritorio remoto

Este es el primer y más importante paso para la migración: migrar los agentes de conexión a Escritorio remoto a los servidores de destino que ejecutan Windows Server 2016.

> [!IMPORTANT]
> Para que se pueda admitir la migración, los servidores de origen de Agente de conexión a Escritorio remoto deben configurarse para alta disponibilidad. Para obtener más información, consulta [Implementación de un clúster de Agente de conexión a Escritorio remoto](./rds-connection-broker-cluster.md).

1. Si tiene más de un servidor de Agente de conexión a Escritorio remoto en la configuración de alta disponibilidad, quite todos los servidores de Agente de conexión a Escritorio remoto excepto el que está actualmente activo.

2. [Actualiza](./upgrade-to-rds.md) el servidor de Agente de conexión a Escritorio remoto que queda en la implementación a Windows Server 2016.

3. Agrega servidores de Agente de conexión a Escritorio remoto de Windows Server 2016 en la implementación de alta disponibilidad.

> [!NOTE]
> No se admiten configuraciones mixtas de alta disponibilidad con Windows Server 2016 y Windows Server 2012 R2 para los servidores de Agente de conexión a Escritorio remoto.
> Un Agente de conexión a Escritorio remoto que ejecuta Windows Server 2016 puede servir colecciones de sesiones con servidores de host de sesión de Escritorio remoto que ejecutan Windows Server 2012 R2 y puede servir colecciones de escritorios virtuales con servidores de host de virtualización de Escritorio remoto que ejecutan Windows Server 2012 R2.

## <a name="migrate-session-collections"></a>Migrar las colecciones de sesiones

Sigue estos pasos para migrar una colección de sesiones en Windows Server 2012 R2 a una colección de sesiones en Windows Server 2016.

> [!IMPORTANT]
> Migra las colecciones de sesiones únicamente después de haber completado correctamente el paso anterior, [Migrar los servidores de Agente de conexión a Escritorio remoto](#migrate-rdconnection-broker-servers).

1. [Actualiza la colección de sesiones](./upgrade-to-rdsh.md) de Windows Server 2012 R2 a Windows Server 2016.

2. Agrega el nuevo servidor de host de sesión de Escritorio remoto que ejecuta Windows Server 2016 a la colección de sesiones.

3. Cierre todas las sesiones de los servidores de Host de sesión de Escritorio remoto y quite de la colección de sesiones los servidores que requieren migración.

   > [!NOTE]
   > Si la plantilla de UVHD (UVHD-template.vhdx) está habilitada en la colección de sesiones y se ha migrado el servidor de archivos a un nuevo servidor, actualice la propiedad de la colección Discos de perfil de usuario: Propiedad de colección de ubicaciones con la nueva ruta de acceso. Los Discos de perfil de usuario deben estar disponibles en la nueva ubicación en la misma ruta de acceso relativa que tenían en el servidor de origen.
   >
   > No se admite una colección de sesiones de servidores de host de sesión de Escritorio remoto con una combinación de servidores que ejecutan Windows Server 2012 R2 y Windows Server 2016.

## <a name="migrate-virtual-desktop-collections"></a>Migrar las colecciones de escritorios virtuales

Sigue estos pasos para migrar una colección de escritorios virtuales desde un servidor de origen que ejecuta Windows Server 2012 R2 a un servidor de destino que ejecuta Windows Server 2016.

> [!IMPORTANT]
> Migra las colecciones de escritorios virtuales después de completar correctamente el paso anterior, [Migrar los servidores de agente de conexión a Escritorio remoto](#migrate-rdconnection-broker-servers).

1. [Actualiza la colección de escritorios virtuales](./upgrade-to-rdvh.md) desde el servidor que ejecuta Windows Server 2012 R2 a Windows Server 2016.

2. Agrega los nuevos servidores de host de virtualización de Escritorio remoto de Windows Server 2016 a la colección de escritorios virtuales.

3. Migre todas las máquinas virtuales de la colección de escritorios virtuales que se están ejecutando en servidores de Host de virtualización de Escritorio remoto a los nuevos servidores.

4. Quite todos los servidores de Host de virtualización de Escritorio remoto que tengan que migrarse de la colección de escritorios virtuales del servidor de origen.

> [!NOTE]
> Si la plantilla de UVHD (UVHD-template.vhdx) está habilitada en la colección de sesiones y se ha migrado el servidor de archivos a un nuevo servidor, actualice la propiedad de la colección Discos de perfil de usuario: Propiedad de colección de ubicaciones con la nueva ruta de acceso. Los Discos de perfil de usuario deben estar disponibles en la nueva ubicación en la misma ruta de acceso relativa que tenían en el servidor de origen.
>
> No se admite una colección de escritorios virtuales de servidores de host de sesión de virtualización de Escritorio remoto con una combinación de servidores que ejecutan Windows Server 2012 R2 y Windows Server 2016.

## <a name="migrate-rdweb-access-servers"></a>Migrar los servidores de Acceso web de Escritorio remoto

Sigue estos pasos para migrar servidores de acceso web de Escritorio remoto:

- Une los servidores de destino que ejecutan Windows Server 2016 a la implementación de Servicios de Escritorio remoto e instala el rol web de Escritorio remoto.

- Usa la [herramienta de implementación web de IIS](https://www.iis.net/) para migrar la configuración del sitio web de web de Escritorio remoto de los servidores de acceso web de Escritorio remoto actuales a los servidores de destino que ejecutan Windows Server 2016.

- [Migra los certificados](#migrate-certificates) a los servidores de destino que ejecutan Windows Server 2016.

- Quita los servidores de origen de la implementación de Servicios de Escritorio remoto.

## <a name="migrate-rdgateway-servers"></a>Migrar los servidores de Puerta de enlace de Escritorio remoto

Sigue estos pasos para migrar servidores de puerta de enlace de Escritorio remoto:

- Une los servidores de destino que ejecutan Windows Server 2016 a la implementación de Servicios de Escritorio remoto e instala el rol de puerta de enlace de Escritorio remoto.

- Usa la [herramienta de implementación web de IIS](https://www.iis.net/) para migrar la configuración del punto de conexión de la puerta de enlace de Escritorio remoto de los servidores de puerta de enlace de Escritorio remoto actuales a los servidores de destino que ejecutan Windows Server 2016.

- [Migra los certificados](#migrate-certificates) a los servidores de destino que ejecutan Windows Server 2016.

- Quita los servidores de origen de la implementación de Servicios de Escritorio remoto.

## <a name="migrate-rdlicensing-servers"></a>Migrar los servidores de Administración de licencias de Escritorio remoto

Sigue estos pasos para migrar un servidor de licencias de Escritorio remoto desde un servidor de origen que ejecuta Windows Server 2012 o Windows Server 2012 R2 a un servidor de destino que ejecuta Windows Server 2016.

1. [Migra las licencias de acceso de cliente de Servicios de Escritorio remoto (CAL de RDS)](migrate-rds-cals.md) del servidor de origen al servidor de destino.

2. Edita las **propiedades de implementación** en **Administrador de servidores** en el servidor de administración de Escritorio remoto (que normalmente se ejecuta en el primer servidor de agente de conexión a Escritorio remoto) para incluir solo los nuevos servidores de licencias de Escritorio remoto que ejecutan Windows Server 2016.

3. Desactiva el servidor de licencias de Escritorio remoto de origen: En **Administrador de licencias de Escritorio remoto**, haz clic en el servidor adecuado, mantén el mouse sobre la opción **Avanzado** para seleccionar **Desactivar servidor** y, a continuación, sigue los pasos del asistente.

4. Quita los servidores de licencias de Escritorio remoto de origen de la implementación en **Administrador de servidores** en el servidor de administración de Escritorio remoto.

## <a name="migrate-certificates"></a>Migrar los certificados

Una migración de certificados correcta requiere tanto el proceso real de migración de certificados como la actualización de la información de certificado en las propiedades de implementación de Servicios de Escritorio remoto.

La migración típica de certificados incluye los siguientes pasos:

- Exportar el certificado a un archivo PFX con la clave privada.

- Importar el certificado desde un archivo PFX.

Después de migrar los certificados apropiados, actualiza los siguientes certificados necesarios para la implementación de Servicios de Escritorio remoto en el administrador de servidores o PowerShell:

- Agente de conexión a Escritorio remoto: inicio de sesión único

- Agente de conexión a Escritorio remoto: publicación de archivos RDP

- Puerta de enlace de Escritorio remoto: conexión HTTPS

- Acceso web de Escritorio remoto: suscripción de conexión de RemoteApp y escritorio y conexión HTTPS
