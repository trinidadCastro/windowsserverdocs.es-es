---
title: Habilitar la recuperación ante desastres de RDS con Azure Site Recovery
description: Obtenga información sobre cómo habilitar la recuperación ante desastres de RDS con Azure Site Recovery.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 05/05/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: e3f9db4afb37452b4fd5d0229b385492b915fe45
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859016"
---
# <a name="enable-disaster-recovery-of-rds-using-azure-site-recovery"></a>Habilitar la recuperación ante desastres de RDS con Azure Site Recovery

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Para asegurarse de que la implementación de RDS está configurada correctamente para la recuperación ante desastres, es preciso proteger todos los componentes que constituyen la implementación de RDS:

- Active Directory
- Nivel de SQL Server
- Componentes de RDS
- Componentes de red
 
## <a name="configure-active-directory-and-dns-replication"></a>Configurar la replicación de Active Directory y DNS

Necesita Active Directory en el sitio de recuperación ante desastres para la implementación de RDS para que funcione. Tiene dos opciones, según la complejidad de la implementación de RDS es:

- Opción 1: si tiene un número pequeño de aplicaciones y un controlador de dominio para todo el sitio local y se conmuta de todo el sitio, use replicación de ASR para replicar el controlador de dominio en el sitio secundario (true para ambos escenarios sitio a sitio y sitio a Azure).
- Opción 2: si tiene un gran número de aplicaciones y ejecuta un bosque de Active Directory y verá conmutación por error pocas aplicaciones a la vez, configurar un controlador de dominio adicional en el sitio de recuperación ante desastres (ya sea un sitio secundario o en Azure).

Consulte [protección de Active Directory y DNS con Azure Site Recovery](/azure/site-recovery/site-recovery-active-directory) para obtener más información acerca de cómo realizar un controlador de dominio disponible en el sitio de recuperación ante desastres. El resto de esta guía, se supone que ha seguido estos pasos y que tiene el controlador de dominio disponible.

## <a name="set-up-sql-server-replication"></a>Configurar la replicación de SQL Server

Consulte [proteger SQL Server con la recuperación ante desastres de SQL Server y Azure Site Recovery](/azure/site-recovery/site-recovery-sql) para conocer los pasos configurar la replicación de SQL Server.

## <a name="enable-protection-for-the-rds-application-components"></a>Habilitar la protección de los componentes de aplicación de RDS

Según el tipo de implementación de RDS puede habilitar la protección de máquinas virtuales de diferentes componentes (como se muestra en la tabla siguiente) en Azure Site Recovery. Configure los elementos correspondientes de Azure Site Recovery en función de si están implementadas las máquinas virtuales en Hyper-V o VMWare.

| Tipo de implementación                              | Pasos de protección                                                                                                                                                                                      |
|----------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Escritorio virtual personal (no administrado)         |  1. Asegúrese de que todos los hosts de virtualización están preparados con la función RDVH instalada.    </br>2. Agente de conexión.  </br>3. Escritorios personales. </br>4. Gold plantilla de máquina virtual. </br>5. Web Access, servidor de licencias y el servidor de puerta de enlace |
| Escritorio virtual agrupado (administrado con sin UPD) |  1. Todos los hosts de virtualización están preparados con la función RDVH instalada.  </br>2. Agente de conexión.  </br>3. Gold plantilla de máquina virtual. </br>4. Web Access, servidor de licencias y el servidor de puerta de enlace.                                  |
| RemoteApps y sesiones de escritorio (sin UPD)     |  1. Hosts de sesión.  </br>2. Agente de conexión. </br>3. Web Access, servidor de licencias y el servidor de puerta de enlace.                                                                                                          |                                                                                                                                      |

