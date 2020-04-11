---
title: Habilitar la recuperación ante desastres de RDS mediante Azure Site Recovery
description: Aprende a habilitar la recuperación ante desastres de RDS mediante Azure Site Recovery.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 05/05/2017
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 0c7af18be4aa767009f1dd0b82f145ffe6874768
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861408"
---
# <a name="enable-disaster-recovery-of-rds-using-azure-site-recovery"></a>Habilitar la recuperación ante desastres de RDS mediante Azure Site Recovery

>Se aplica a: Windows Server (Canal semianual), Windows Server 2019 y Windows Server 2016

Para asegurarse de que la implementación de RDS está configurada correctamente para la recuperación ante desastres, es preciso proteger todos los componentes que la constituyen:

- Active Directory
- Nivel de SQL Server
- Componentes de RDS
- Componentes de red

## <a name="configure-active-directory-and-dns-replication"></a>Configuración de la replicación de Active Directory y DNS

Necesitas Active Directory en el sitio de recuperación ante desastres para que la implementación de RDS funcione. Tienes dos opciones según la complejidad de la implementación de RDS:

- Opción 1: si tienes un número pequeño de aplicaciones y un único controlador de dominio para todo el sitio local y vas a realizar una conmutación por error de todo el sitio, usa ASR-Replication para replicar el controlador de dominio en el sitio secundario (lo cual es cierto en los escenarios de sitio a sitio y de sitio a Azure).
- Opción 2: si tienes un gran número de aplicaciones y ejecutas un bosque de Active Directory y vas a realizar una conmutación por error de pocas aplicaciones a la vez, configura un controlador de dominio adicional en el sitio de recuperación ante desastres (ya sea un sitio secundario o en Azure).

Consulta [Protección de Active Directory y DNS con Azure Site Recovery](/azure/site-recovery/site-recovery-active-directory) para más información acerca de cómo hacer que un controlador de dominio esté disponible en el sitio de recuperación ante desastres. Para el resto de esta guía, se supone que has seguido estos pasos y que el controlador de dominio está disponible.

## <a name="set-up-sql-server-replication"></a>Configuración de la replicación de SQL Server

Consulta [Protección de SQL Server con la recuperación ante desastres de SQL Server y Azure Site Recovery](/azure/site-recovery/site-recovery-sql) para conocer los pasos de configuración de la replicación de SQL Server.

## <a name="enable-protection-for-the-rds-application-components"></a>Habilitar la protección de los componentes de la aplicación de RDS

Según el tipo de implementación de RDS puedes habilitar la protección de las máquinas virtuales de diferentes componentes (como se muestra en la tabla siguiente) en Azure Site Recovery. Configura los elementos correspondientes de Azure Site Recovery en función de si las máquinas virtuales están implementadas en Hyper-V o VMWare.


|               Tipo de implementación                |                                                                                                     Pasos de protección                                                                                                     |
|----------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     Escritorio virtual personal (no administrado)     | 1. Asegúrate de que todos los hosts de virtualización están preparados con el rol RDVH instalado.    </br>2. Agente de conexión.  </br>3. Escritorios personales. </br>4. Máquina virtual de plantilla Gold. </br>5. Acceso web, servidor de licencias y servidor de puerta de enlace. |
| Escritorio virtual agrupado (administrado, sin UPD) |                    1. Todos los hosts de virtualización están preparados con el rol RDVH instalado.  </br>2. Agente de conexión.  </br>3. Máquina virtual de plantilla Gold. </br>4. Acceso web, servidor de licencias y servidor de puerta de enlace.                    |
|   Sesiones de RemoteApps y escritorio (sin UPD)   |                                                          1. Hosts de sesión.  </br>2. Agente de conexión. </br>3. Acceso web, servidor de licencias y servidor de puerta de enlace.                                                           |

