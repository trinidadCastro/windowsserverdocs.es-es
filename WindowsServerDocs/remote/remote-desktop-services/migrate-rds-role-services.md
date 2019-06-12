---
title: Migrar la implementación de servicios de escritorio remoto en Windows Server 2016
description: En este artículo se describe cómo migrar la implementación de servicios de escritorio remoto a los nuevos servidores de Windows Server 2016.
ms.custom: na
ms.prod: windows-server-threshold
msreviewer: ''
nams.suite: ''
nams.technology: remote-desktop-services
ms.author: chrimo
ms.date: 11/01/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9b1fa833-4325-48a8-bf34-46265f40c001
author: christianmontoya
manager: scottman
ms.openlocfilehash: 0e4736f753fc0ad2ece6135de84d481eecb8b7a1
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812586"
---
# <a name="migrate-your-remote-desktop-services-deployment-to-windows-server-2016"></a>Migrar la implementación de servicios de escritorio remoto en Windows Server 2016

Si actualmente está ejecutando servicios de escritorio remoto en Windows Server 2012 R2, puede mover a Windows Server 2016 para aprovechar las nuevas características como la compatibilidad con SQL Azure y espacios de almacenamiento directo.

Se admite la migración de una implementación de servicios de escritorio remoto desde servidores de origen que ejecuta Windows Server 2016 a los servidores de destino que ejecuta Windows Server 2016. En otras palabras, no hay ninguna migración directa de en lugar de RDS en Windows Server 2012 R2 a Windows Server 2016. En su lugar, para la mayoría de los componentes RDS, actualizar a Windows Server 2016 y, a continuación, migrar datos y las licencias. Los únicos componentes con una migración directa son Web de escritorio remoto, puerta de enlace de escritorio remoto y el servidor de licencias.

Para obtener más información sobre los requisitos y el proceso de actualización, vea [actualizar las implementaciones de servicios de escritorio remoto a Windows Server 2016](upgrade-to-rds-2016.md).

Siga estos pasos para migrar la implementación de servicios de escritorio remoto:

- [Migrar los servidores de agente de conexión a Escritorio remoto](#migrate-rdconnection-broker-servers)

- [Migrar las colecciones de sesiones](#migrate-session-collections)

- [Migre las colecciones de escritorios virtuales](#migrate-virtual-desktop-collections)

- [Migrar servidores de acceso Web de escritorio remoto](#migrate-rdweb-access-servers)

- [Migrar los servidores de puerta de enlace de escritorio remoto](#migrate-rdgateway-servers)

- [Migrar los servidores de licencias de escritorio remoto](#migrate-rdgateway-servers)

- [Migrar certificados](#migrate-certificates)

## <a name="migrate-rdconnection-broker-servers"></a>Migrar los servidores de agente de conexión a Escritorio remoto

Este es el primero y más importante paso para la migración: migrar los agentes de conexión a Escritorio remoto a los servidores de destino que ejecuta Windows Server 2016.

> [!IMPORTANT]
> Los servidores de origen de agente de conexión de escritorio remoto (RD Connection Broker) deben configurarse para que alta disponibilidad admitir la migración. Para obtener más información, consulte [implementar un clúster de agente de conexión a Escritorio remoto](Deploy-a-Remote-Desktop-Connection-Broker-cluster.md).

1. Si tiene más de un servidor de Agente de conexión a Escritorio remoto en la configuración de alta disponibilidad, quite todos los servidores de Agente de conexión a Escritorio remoto excepto el que está actualmente activo.

2. [Actualizar](upgrade-to-rds-2016.md) el servidor de agente de conexión a Escritorio remoto restante en la implementación de Windows Server 2016.

3. Agregar servidores de agente de conexión a Escritorio remoto de Windows Server 2016 en la implementación de alta disponibilidad.

> [!NOTE] 
> No se admite una configuración mixtas de alta disponibilidad con Windows Server 2016 y Windows Server 2012 R2 para los servidores de agente de conexión a Escritorio remoto. Un agente de conexión a Escritorio remoto que ejecuta Windows Server 2016 puede servir colecciones de sesiones con servidores de Host de sesión de escritorio remoto que ejecutan Windows Server 2012 R2, y puede servir colecciones de escritorios virtuales con servidores de Host de virtualización de escritorio remoto que ejecutan Windows Server 2012 R2.

## <a name="migrate-session-collections"></a>Migrar las colecciones de sesiones

Siga estos pasos para migrar una colección de sesiones en Windows Server 2012 R2 a una colección de sesiones de Windows Server 2016.

> [!IMPORTANT]
> Migre las colecciones de sesiones únicamente después de completar correctamente el paso anterior, [servidores migrar RD Connection Broker](#migrate-rdconnection-broker-servers).

1. [Actualizar la colección de sesiones](Upgrade-to-RDSH-2016.md) desde Windows Server 2012 R2 a Windows Server 2016.

2. Agregue un nuevo servidor Host de sesión de escritorio remoto que ejecuta Windows Server 2016 a la colección de sesiones.

3. Cierre todas las sesiones de los servidores de Host de sesión de Escritorio remoto y quite de la colección de sesiones los servidores que requieren migración.

   > [!NOTE]
   > Si la plantilla de UVHD (UVHD-template.vhdx) está habilitada en la colección de sesiones y se ha migrado el servidor de archivos a un servidor nuevo, actualice los discos de perfil de usuario: Propiedad de colección de ubicación con la nueva ruta de acceso. Los Discos de perfil de usuario deben estar disponibles en la nueva ubicación en la misma ruta de acceso relativa que tenían en el servidor de origen.
   >
   > No se admite una colección de sesiones de servidores Host de sesión de escritorio remoto con una combinación de los servidores que ejecutan Windows Server 2012 R2 y Windows Server 2016.

## <a name="migrate-virtual-desktop-collections"></a>Migrar las colecciones de escritorios virtuales

Siga estos pasos para migrar una colección de escritorios virtuales desde un servidor de origen ejecuta Windows Server 2012 R2 a un servidor de destino que ejecuta Windows Server 2016.

> [!IMPORTANT]
> Migre las colecciones de escritorios virtuales después de completar correctamente el paso anterior, [servidores migrar RD Connection Broker](#migrate-rdconnection-broker-servers).

1. [Actualizar la colección de escritorios virtuales](Upgrade-to-RDVH-2016.md) desde el servidor que ejecute Windows Server 2012 R2 a Windows Server 2016.

2. Agregar los nuevos servidores de Host de virtualización de escritorio remoto de Windows Server 2016 a la colección de escritorios virtuales.

3. Migre todas las máquinas virtuales de la colección de escritorios virtuales que se están ejecutando en servidores de Host de virtualización de Escritorio remoto a los nuevos servidores.

4. Quite todos los servidores de Host de virtualización de Escritorio remoto que tengan que migrarse de la colección de escritorios virtuales del servidor de origen.

> [!NOTE]
> Si la plantilla de UVHD (UVHD-template.vhdx) está habilitada en la colección de sesiones y se ha migrado el servidor de archivos a un servidor nuevo, actualice los discos de perfil de usuario: Propiedad de colección de ubicación con la nueva ruta de acceso. Los Discos de perfil de usuario deben estar disponibles en la nueva ubicación en la misma ruta de acceso relativa que tenían en el servidor de origen.
>
> No se admite una colección de escritorios virtuales de servidores Host de virtualización de escritorio remoto con una combinación de los servidores que ejecutan Windows Server 2012 R2 y Windows Server 2016.

## <a name="migrate-rdweb-access-servers"></a>Migrar los servidores de Acceso web de Escritorio remoto

Siga estos pasos para migrar servidores de acceso Web de escritorio remoto:

- Únase a los servidores de destino que ejecuta Windows Server 2016 a la implementación de servicios de escritorio remoto e instale el rol Web de escritorio remoto

- Use [herramienta de implementación Web de IIS](https://www.iis.net/) para migrar la configuración del sitio Web de Web de escritorio remoto de los servidores de acceso Web de RD actual a los servidores de destino que ejecutan Windows Server 2016.

- [Migrar certificados](#migrate-certificates) a los servidores de destino que ejecuta Windows Server 2016.

- Quite los servidores de origen de la implementación de servicios de escritorio remoto.

## <a name="migrate-rdgateway-servers"></a>Migrar los servidores de Puerta de enlace de Escritorio remoto

Siga estos pasos para migrar servidores de puerta de enlace de escritorio remoto:

- Únase a los servidores de destino que ejecuta Windows Server 2016 a la implementación de servicios de escritorio remoto e instale el rol de puerta de enlace de escritorio remoto

- Use [herramienta de implementación Web de IIS](https://www.iis.net/) para migrar la configuración de punto de conexión de puerta de enlace de escritorio remoto de los servidores de puerta de enlace de escritorio remoto actual a los servidores de destino que ejecutan Windows Server 2016.

- [Migrar certificados](#migrate-certificates) a los servidores de destino que ejecuta Windows Server 2016.

- Quite los servidores de origen de la implementación de servicios de escritorio remoto.

## <a name="migrate-rdlicensing-servers"></a>Migrar los servidores de Administración de licencias de Escritorio remoto

Siga estos pasos para migrar un servidor de licencias de escritorio remoto desde un servidor de origen que ejecuta Windows Server 2012 o Windows Server 2012 R2 a un servidor de destino que ejecuta Windows Server 2016.

1. [Migrar las licencias de acceso de cliente de servicios de escritorio remoto (CAL de RDS)](migrate-rds-cals.md) desde el servidor de origen al servidor de destino.

2. Editar el **las propiedades de implementación** en **administrador del servidor** en el servidor de administración de escritorio remoto (que normalmente se ejecuta en el primer servidor de agente de conexión a Escritorio remoto) para incluir solo las nuevas licencias de escritorio remoto servidores que ejecutan Windows Server 2016.

3. Desactivar el servidor de licencias de escritorio remoto de origen: En **el Administrador de licencias de escritorio remoto**, haga clic en el servidor adecuado, mantenga el mouse sobre **avanzadas** seleccionar **desactivar servidor**y, a continuación, siga los pasos del Asistente .

4. Quite los servidores de licencias de escritorio remoto de origen de la implementación en **administrador del servidor** en el servidor de administración de escritorio remoto.

## <a name="migrate-certificates"></a>Migrar los certificados

Migración de certificados correcta, requiere tanto el proceso real de migración de certificados y actualizar la información de certificado en las propiedades de implementación de servicios de escritorio remoto.

Migración de certificados típico incluye los siguientes pasos:

- Exporte el certificado a un archivo PFX con la clave privada.

- Importe el certificado desde un archivo PFX.

Después de migrar los certificados apropiados, actualice los siguientes certificados necesarios para la implementación de servicios de escritorio remoto con el administrador del servidor o de PowerShell:

- Agente de conexión a Escritorio remoto: inicio de sesión único

- Agente de conexión a Escritorio remoto: publicación del archivo RDP

- Puerta de enlace de escritorio remoto: conexión HTTPS

- Acceso Web de RD - conexión HTTPS y la suscripción de conexión de RemoteApp y escritorio
