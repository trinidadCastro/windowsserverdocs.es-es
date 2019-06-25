---
title: 'RDS: compilar e implementar'
description: Pasos para crear una implementación de escritorio remoto
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 04/18/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 176ae424-96e9-4c78-88f5-da418e76c3d7
author: lizap
manager: dongill
ms.openlocfilehash: 309ea068488d005eabfe22f8ea055f85dd098452
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66453072"
---
# <a name="build-and-deploy-your-remote-desktop-services-deployment"></a>Compilar e implementar la implementación de servicios de escritorio remoto

Una implementación de servicios de escritorio remoto es la infraestructura usada para compartir aplicaciones y recursos con los usuarios. Dependiendo de la experiencia que desea proporcionar, puede hacerlo tan pequeñas o complejo según sea necesario. Escalan fácilmente las implementaciones de escritorio remotas. Puede aumentar y reducir el acceso Web a Escritorio remoto, se realizará en los servidores de puerta de enlace, el agente de conexión y el Host de sesión. Puede utilizar el agente de conexión a Escritorio remoto para distribuir las cargas de trabajo. Autenticación de Active Directory en función proporciona un entorno altamente seguro. 

[Clientes de escritorio remoto](clients/remote-desktop-clients.md) habilitar el acceso desde cualquier tableta Windows, Apple o Android equipo, o por teléfono.

Consulte [arquitectura de servicios de escritorio remoto](desktop-hosting-logical-architecture.md) para obtener una explicación detallada de los distintos componentes que funcionan conjuntamente para realizar la implementación de servicios de escritorio remoto.

¿Tiene una implementación de escritorio remoto existente creada en una versión anterior de Windows Server? Revise las opciones para migrar a WIndows Server 2016, donde puede sacar partido de funciones nuevas y mejores acerca del rendimiento y escalabilidad:

- [Migrar la implementación de RDS en Windows Server 2016](migrate-rds-role-services.md)
- [Actualizar la implementación de RDS a Windows Server 2016](upgrade-to-rds-2016.md)

¿Desea crear una nueva implementación de escritorio remoto? Use la siguiente información para implementar un escritorio remoto en Windows Server 2016:

- [Implementar la infraestructura de servicios de escritorio remoto](rds-deploy-infrastructure.md)
- [Crear una colección de sesiones para mantener las aplicaciones y los recursos que desea compartir](rds-create-collection.md)
- [Licencia de su implementación de RDS](rds-client-access-license.md)
- Que los usuarios instalen un [cliente de escritorio remoto](clients/remote-desktop-clients.md) para que pueden acceder a las aplicaciones y los recursos. 
- Habilitar la alta disponibilidad mediante la adición de Hosts de sesión y agentes de conexión adicionales:
   - [Escalar horizontalmente una colección de RDS existente con una granja de servidores de host de sesión de Escritorio remoto](rds-scale-rdsh-farm.md)
   - [Agregar alta disponibilidad a la infraestructura del agente de conexión a Escritorio remoto](rds-connection-broker-cluster.md)
   - [Agregar alta disponibilidad al front-end web de la puerta de enlace de Escritorio remoto y web de RD](rds-rdweb-gateway-ha.md)
   - [Implementación de un sistema de archivos de dos nodos de espacios de almacenamiento directo para el almacenamiento de UPD](rds-storage-spaces-direct-deployment.md)


Si está interesado en usar Escritorio remoto para proporcionar aplicaciones y recursos a los clientes o un cliente busca a alguna persona hospedar las aplicaciones un socio de hospedaje, consulte [socios de hospedaje de servicios de escritorio remoto](rds-hosting-partners.md) para obtener información acerca de un evaluación puede tener sobre el uso de RDS en Azure como un entorno de hospedaje, así como una lista de socios que se ha pasado.