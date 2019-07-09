---
title: 'RDS: Creación e implementación'
description: Pasos para crear una implementación de Escritorio remoto
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
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/17/2019
ms.locfileid: "66453072"
---
# <a name="build-and-deploy-your-remote-desktop-services-deployment"></a>Crea e implementa la implementación de Servicios de Escritorio remoto.

Una implementación de Servicios de Escritorio remoto es la infraestructura utilizada para compartir aplicaciones y recursos con los usuarios. Dependiendo de la experiencia que quieras proporcionar, puedes hacerla tan pequeña o compleja como necesites. Las implementaciones de Escritorio remoto se escalan fácilmente. Puedes aumentar y disminuir a voluntad los servidores de Acceso web a Escritorio remoto, de Puerta de enlace, de Agente de conexión y de Host de sesión. Puedes utilizar el Agente de conexión a Escritorio remoto para distribuir las cargas de trabajo. La autenticación basada en Active Directory proporciona un entorno altamente seguro. 

Los [clientes de Escritorio remoto](clients/remote-desktop-clients.md) habilitan el acceso desde cualquier tableta, teléfono o equipo Windows, Apple o Android.

Consulta [Arquitectura de Servicios de Escritorio remoto](desktop-hosting-logical-architecture.md) para obtener un análisis detallado de las diferentes piezas que funcionan en conjunto para conformar la implementación de los Servicios de Escritorio remoto.

¿Tienes una implementación de Escritorio remoto existente basada en una versión anterior de Windows Server? Comprueba tus opciones para migrar a Windows Server 2016, donde puedes aprovechar las nuevas y mejores funcionalidades en cuanto a rendimiento y escalabilidad:

- [Migración de la implementación de RDS en Windows Server 2016](migrate-rds-role-services.md)
- [Actualización de la implementación de RDS en Windows Server 2016](upgrade-to-rds-2016.md)

¿Quieres crear una nueva implementación de Escritorio remoto? Utiliza la siguiente información para implementar un Escritorio remoto en Windows Server 2016:

- [Implementación de una infraestructura de Servicios de Escritorio remoto](rds-deploy-infrastructure.md)
- [Creación de una colección de sesiones para guardar las aplicaciones y los recursos que quieres compartir](rds-create-collection.md)
- [Licencia de tu implementación de RDS](rds-client-access-license.md)
- Pide a tus usuarios que instalen un [cliente de Escritorio remoto](clients/remote-desktop-clients.md) para que puedan acceder a las aplicaciones y los recursos. 
- Habilita la alta disponibilidad mediante la adición de Hosts de sesión y Agentes de conexión adicionales:
   - [Escalar horizontalmente una colección de RDS existente con una granja de servidores de host de sesión de Escritorio remoto](rds-scale-rdsh-farm.md)
   - [Agregar alta disponibilidad a la infraestructura del agente de conexión a Escritorio remoto](rds-connection-broker-cluster.md)
   - [Agregar alta disponibilidad al front-end web de la puerta de enlace de Escritorio remoto y web de RD](rds-rdweb-gateway-ha.md)
   - [Implementación de un sistema de archivos de dos nodos de espacios de almacenamiento directo para el almacenamiento de UPD](rds-storage-spaces-direct-deployment.md)


Si eres un socio de hospedaje interesado en usar Escritorio remoto para proporcionar aplicaciones y recursos a los clientes o eres un cliente que busca a alguien para hospedar sus aplicaciones, consulta [Asociados de hospedaje de Servicios de Escritorio remoto](rds-hosting-partners.md) para obtener información sobre una evaluación que puedes realizar sobre el uso de RDS en Azure como entorno de hospedaje, así como una lista de asociados que lo han pasado.
