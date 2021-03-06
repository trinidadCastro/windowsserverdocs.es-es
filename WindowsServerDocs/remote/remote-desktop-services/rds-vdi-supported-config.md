---
title: Configuraciones de seguridad admitidas de Windows 10 para VDI de Servicios de Escritorio remoto
description: Proporciona información sobre las configuraciones admitidas para VDI de Windows 10 con RDS en Windows Server 2016.
ms.author: elizapo
ms.date: 10/27/2016
ms.topic: article
ms.assetid: 8f164f5d-a498-4f91-a12f-3e01d554f810
author: lizap
manager: dongill
ms.openlocfilehash: 7fd8de56d02dfe83add67b740405265a232747d9
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87946348"
---
# <a name="supported-windows-10-security-configurations-for-remote-desktop-services-vdi"></a>Configuraciones de seguridad admitidas de Windows 10 para VDI de Servicios de Escritorio remoto

> Se aplica a: Windows Server 2016

Windows 10 y Windows Server 2016 tienen nuevas capas de protección integradas en el sistema operativo para protección adicional contra las vulneraciones de seguridad, ayudan a bloquear ataques malintencionados y mejoran la seguridad de máquinas virtuales, aplicaciones y datos.

> [!NOTE]
> No olvides revisar la [información de configuración admitida para Servicios de Escritorio remoto](rds-supported-config.md).

En la siguiente tabla se describen las características nuevas que se admite en una implementación de VDI con RDS.

|  Tipo de colección de VDI               |  Administrada, agrupada |  Administrada, personal |  No administrada, agrupada                                     |  No administrada, personal                                    |
|-------------------------------------|------------------|--------------------|--------------------------------------------------------|--------------------------------------------------------|
| [Credential Guard](/windows/security/identity-protection/credential-guard/credential-guard)                    | Sí              | Sí                | Sí                                                    | Sí                                                    |
| [Device Guard](/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control-deployment-guide)                        | Sí              | Sí                | Sí                                                    | Sí                                                    |
| [Credential Guard remoto](/windows/security/identity-protection/remote-credential-guard)             | No               | No                 | No                                                     | No                                                     |
| [VM blindadas y compatibles con cifrado](../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms.md) | No               | No                 | VM compatibles con cifrado, con configuración adicional | VM compatibles con cifrado, con configuración adicional |

## <a name="remote-credential-guard"></a>Credential Guard remoto:

Credential Guard remoto solo se admite para las conexiones directas a las máquinas de destino y no para aquellas a través del Agente de conexión a Escritorio remoto y Puerta de enlace de Escritorio remoto.
> [!NOTE]
> Si tienes un Agente de conexión en un entorno de instancia única, y el nombre DNS coincide con el nombre del equipo, puedes usar Credential Guard remoto, aunque no se admite.

## <a name="shielded-vms-and-encryption-supported-vms"></a>VM blindadas y VM compatibles con cifrado:

- No se admiten VM blindadas en VDI de Servicios de Escritorio remoto.

Para aprovechar VM compatibles con cifrado:
- Usa una colección no administrada y una tecnología de aprovisionamiento fuera del proceso de creación de colecciones de Servicios de Escritorio remoto para aprovisionar las máquinas virtuales.
- No se admiten discos de perfil de usuario, ya que se basan en discos diferenciales.
