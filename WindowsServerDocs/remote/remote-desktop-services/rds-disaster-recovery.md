---
title: Proteger la implementación de RDS - recuperación ante desastres
description: Obtenga información sobre las opciones de recuperación ante desastres para servicios de escritorio remoto
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9ff6a3b0-ea14-424e-9524-209252e9f1a8
author: lizap
ms.author: elizapo
ms.date: 06/12/2017
ms.openlocfilehash: a6eac3a50999633d15b1b6dc28608f60f6fef6c7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834086"
---
# <a name="configure-disaster-recovery-for-remote-desktop-services"></a>Configurar la recuperación ante desastres para servicios de escritorio remoto

Al implementar servicios de escritorio remoto en su entorno, se convierte en una parte fundamental de la infraestructura, especialmente las aplicaciones y los recursos que puede compartir con los usuarios. Si la implementación de RDS deja de funcionar debido a cualquier cosa de un error de red a un desastre natural, los usuarios no pueden acceder a las aplicaciones y los recursos y su negocio se ve afectado negativamente. Para evitar esto, puede configurar una solución de recuperación ante desastres que permite la conmutación por error de la implementación - si la implementación de RDS está disponible, por cualquier motivo, hay una copia de seguridad disponible para tomar el control automáticamente.

Para mantener su implementación de RDS ejecutándose en el caso de un solo componente o el equipo deje de funcionar, se recomienda configurar la implementación de RDS de alta disponibilidad. Puede hacerlo mediante la configuración de un [granja RDSH](rds-scale-rdsh-farm.md) y garantizar su [agentes de conexión se agrupan en clústeres de alta disponibilidad](rds-connection-broker-cluster.md). 

Las soluciones de recuperación ante desastres, que se recomienda aquí son proteger una implementación de catástrofe - algo que daña toda la implementación de RDS (incluido con redundancia de roles configurados para alta disponibilidad). Si se produce un desastre de tal, tener una solución de recuperación ante desastres integrada en su implementación Permitir conmutación por error toda la implementación y obtener rápidamente aplicaciones y recursos de seguridad y en ejecución para los usuarios.

Use la siguiente información para implementar soluciones de recuperación ante desastres en RDS:

- [Aprovechar varios centros de datos de Azure para garantizar que los usuarios pueden acceder a la implementación de RDS, incluso si un centro de datos de Azure deja de funcionar (redundancia geográfica)](rds-multi-datacenter-deployment.md)
- [Implementar Azure Site Recovery para proporcionar conmutación por error para los componentes RDS en conmutaciones por error de sitio a sitio o sitio en Azure](rds-disaster-recovery-with-azure.md)


