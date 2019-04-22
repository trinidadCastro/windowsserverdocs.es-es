---
title: Configuraciones admitidas de seguridad de Windows 10 para VDI de servicios de escritorio remoto
description: Proporciona información sobre las configuraciones admitidas para Windows 10 VDI con RDS en Windows Server 2016.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 10/27/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8f164f5d-a498-4f91-a12f-3e01d554f810
author: lizap
manager: dongill
ms.openlocfilehash: ff890150dcea30c425267dcaae9b1bdbc6d78b8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820086"
---
# <a name="supported-windows-10-security-configurations-for-remote-desktop-services-vdi"></a>Configuraciones admitidas de seguridad de Windows 10 para VDI de servicios de escritorio remoto

> Se aplica a: Windows Server 2016

Windows 10 y Windows Server 2016 tienen nuevos niveles de protección integrada en el sistema operativo para protegerse frente a infracciones de seguridad, ayudan a bloquear los ataques malintencionados y mejore la seguridad de máquinas virtuales, aplicaciones y datos.

> [!NOTE]
> No olvide revisar la [información de configuración admitidos para servicios de escritorio remoto](rds-supported-config.md).

La siguiente tabla se describen las características nuevas se admite en una implementación de VDI con RDS.

|  Tipo de colección de VDI               |  Managed agrupados |  Administradas personal |  No administrada de grupo                                     |  Personal no administrada                                    |
|-------------------------------------|------------------|--------------------|--------------------------------------------------------|--------------------------------------------------------|
| [Credential Guard](https://technet.microsoft.com/itpro/windows/keep-secure/credential-guard)                    | Sí              | Sí                | Sí                                                    | Sí                                                    |
| [Device Guard](https://technet.microsoft.com/itpro/windows/keep-secure/device-guard-deployment-guide)                        | Sí              | Sí                | Sí                                                    | Sí                                                    |
| [Credential Guard remoto](https://technet.microsoft.com/itpro/windows/keep-secure/remote-credential-guard)             | No               | No                 | No                                                     | No                                                     |
| [Blindada & admite el cifrado de las máquinas virtuales](../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms.md) | No               | No                 | Cifrado de máquinas virtuales compatibles con una configuración adicional | Cifrado de máquinas virtuales compatibles con una configuración adicional |

## <a name="remote-credential-guard"></a>Credential Guard remoto:

De Credential Guard remoto solo se admite para las conexiones directas a las máquinas de destino y no para las reuniones a través de agente de conexión a Escritorio remoto y puerta de enlace de escritorio remoto.
> [!NOTE]
> Si tiene un agente de conexión en un entorno de instancia única y el nombre DNS coincide con el nombre del equipo, puede usar a Credential Guard remoto, aunque esto no se admite.

## <a name="shielded-vms-and-encryption-supported-vms"></a>Cifrado y las máquinas virtuales blindadas admiten máquinas virtuales: 

- No se admiten las máquinas virtuales blindadas en VDI de servicios de escritorio remoto 

Para aprovechar las máquinas virtuales de cifrado compatibles con:
- Usar una colección no administrada y una tecnología de aprovisionamiento fuera del proceso de creación de colección de servicios de escritorio remoto para aprovisionar las máquinas virtuales. 
- No se admiten discos de perfil de usuario ya que se basan en los discos diferenciales 

