---
title: 'Proteger la implementación de RDS: Recuperación ante desastres'
description: Obtén información sobre las opciones de recuperación ante desastres para Servicios de Escritorio remoto
ms.topic: article
ms.assetid: 9ff6a3b0-ea14-424e-9524-209252e9f1a8
author: lizap
ms.author: elizapo
ms.date: 06/12/2017
ms.openlocfilehash: e9270c6d1609e67d4875afb2a9ba971de14c1c7c
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87936899"
---
# <a name="configure-disaster-recovery-for-remote-desktop-services"></a>Configurar la recuperación ante desastres para Servicios de Escritorio remoto

Al implementar Servicios de Escritorio remoto en tu entorno, se convierte en parte fundamental de la infraestructura, en especial las aplicaciones y los recursos que compartes con los usuarios. Si la implementación de RDS deja de funcionar por cualquier motivo, desde un error de red hasta un desastre natural, los usuarios no podrán acceder a esas aplicaciones y recursos, y tu negocio se verá afectado negativamente. Para evitar esto, puedes configurar una solución de recuperación ante desastres que te permita la conmutación por error de tu implementación: si la implementación de RDS no está disponible, por cualquier motivo, hay una copia de seguridad disponible que tomará el control automáticamente.

Para mantener tu implementación de RDS en ejecución en caso de que un solo componente o equipo deje de funcionar, se recomienda configurar la implementación de RDS para alta disponibilidad. Para ello, puedes configurar una [granja de RDSH](rds-scale-rdsh-farm.md) y asegurarte de que los [Agentes de conexión se agrupan en clústeres para alta disponibilidad](rds-connection-broker-cluster.md).

Las soluciones de recuperación ante desastres que se recomiendan aquí son para proteger una implementación ante un desastre grave, algo que afecte a toda tu implementación de RDS (incluidos los roles redundantes configurados para alta disponibilidad). Si se produce un desastre tal, tener una solución de recuperación ante desastres integrada en la implementación te permitirá la conmutación por error de toda la implementación, y hará que las aplicaciones y los recursos vuelvan a estar en funcionamiento de manera rápida para los usuarios.

Usa la siguiente información para implementar soluciones de recuperación ante desastres en RDS:

- [Aprovechar varios centros de datos de Azure para garantizar que los usuarios puedan acceder a tu implementación de RDS, incluso si un centro de datos de Azure deja de funcionar (redundancia geográfica)](rds-multi-datacenter-deployment.md)
- [Implementar Azure Site Recovery para proporcionar conmutación por error para los componentes de RDS en conmutaciones por error de sitio a sitio o de sitio a Azure](rds-disaster-recovery-with-azure.md)


