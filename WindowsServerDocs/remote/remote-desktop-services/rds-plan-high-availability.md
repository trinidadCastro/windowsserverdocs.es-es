---
title: 'Servicios de escritorio remoto: alta disponibilidad'
description: Planear la información sobre cómo configurar una implementación de RDS de alta disponibilidad.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ec630ea0-ab80-4dfe-a25f-f4f601651f72
author: lizap
ms.author: elizapo
ms.date: 09/07/2016
manager: dongill
ms.openlocfilehash: b5a2bd38c8831063d6fd2ba525b71a10403b8fc2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839266"
---
# <a name="remote-desktop-services---high-availability"></a>Servicios de escritorio remoto: alta disponibilidad

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Limitación y errores son inevitables en sistemas a gran escala. Es fácil de configurar los roles de infraestructura de escritorio remoto para admitir la alta disponibilidad y permitir que los usuarios finales para conectarse a la perfección, cada vez.

En servicios de escritorio remoto, los siguientes elementos representan los roles de infraestructura de escritorio remoto, con sus correspondientes instrucciones para establecer la alta disponibilidad:
- [Agente de conexión de escritorio remoto](Deploy-a-Remote-Desktop-Connection-Broker-cluster.md)
- [Puerta de enlace de escritorio remoto](Deploy-a-RD-Web-Access-and-Gateway-farm.md)
- Administración de licencias de Escritorio remoto
- [Acceso Web a Escritorio remoto](Deploy-a-RD-Web-Access-and-Gateway-farm.md)

Alta disponibilidad se establece mediante la duplicación de cada uno de los servicios de roles en un segundo máquinas. En Azure, puede recibir un tiempo de actividad garantizado al colocar el conjunto de las dos máquinas virtuales (el mismo rol que hospeda) en una disponibilidad establece.

Junto con los conjuntos de disponibilidad, ahora puede aprovechar la eficacia de Azure SQL Database y su SLA con respaldo de Azure para asegurarse de que siempre tiene la información de conexión y puede redirigir a los usuarios a sus escritorios y aplicaciones.

Para los procedimientos recomendados sobre la creación de su entorno de RDS, vea el [arquitectura de hospedaje de escritorio](desktop-hosting-reference-architecture.md).