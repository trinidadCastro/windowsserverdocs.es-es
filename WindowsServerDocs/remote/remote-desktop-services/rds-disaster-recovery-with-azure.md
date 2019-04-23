---
title: Configuración de recuperación ante desastres para RDS mediante la recuperación ante desastres de Azure
description: Aprenda a usar la recuperación ante desastres de Azure para recuperación ante desastres para una implementación de RDS
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 06/12/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 561a515e23d12cc3397c40fd885550e735ed4d27
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878176"
---
# <a name="set-up-disaster-recovery-for-rds-using-azure-site-recovery"></a>Configuración de recuperación ante desastres para RDS mediante Azure Site Recovery

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar Azure Site Recovery para crear una solución de recuperación ante desastres para la implementación de servicios de escritorio remoto. 

[Azure Site Recovery](/azure/site-recovery/site-recovery-overview) es un servicio basado en Azure que proporciona funcionalidades de recuperación de desastres mediante la coordinación de la replicación, conmutación por error y recuperación de máquinas virtuales. Azure Site Recovery admite un número de tecnologías de replicación para replicar, proteger, de forma coherente y sin problemas máquinas virtuales de conmutación por error y aplicaciones públicas o privadas o nubes del proveedor de hospedaje. 

Utilice la siguiente información para crear y validar la solución de recuperación ante desastres.

## <a name="disaster-recovery-deployment-options"></a>Opciones de implementación de recuperación ante desastres

Puede implementar RDS en servidores físicos o máquinas virtuales que ejecutan Hyper-V o VMWare. Azure Site Recovery puede proteger de forma local y las implementaciones de virtuales a un sitio secundario o en Azure. La siguiente tabla muestra que los diferentes admiten las implementaciones de RDS en escenarios de recvoery ante desastres de sitio a sitio y sitio a Azure.

| Tipo de implementación                          | Sitio a sitio de Hyper-V | Hyper-V de sitio en Azure | Sitio a Azure en VMWare | Física de sitio en Azure |
|------------------------------------------|----------------------|-----------------------|---------------------|----------------------|-----------------------|------------------------|
| Escritorio virtual agrupado (no administrado)       |Sí|No|No|No |
| Escritorio virtual agrupado (no administrado, UPD) | Sí|No|No|No|
| Sesiones de RemoteApp y escritorio (sin UPD) | Sí|Sí|Sí|Sí  |

## <a name="prerequisites"></a>Requisitos previos

Para poder configurar Azure Site Recovery para la implementación, asegúrese de que cumplir los requisitos siguientes:

- Crear un [implementación de RDS local](rds-deploy-infrastructure.md).
- Agregar [del almacén de servicios de Azure Site Recovery](/azure/site-recovery/site-recovery-vmm-to-azure#create-a-recovery-services-vault) a su suscripción de Microsoft Azure.
- Si va a usar Azure como sitio de recuperación, ejecute el [herramienta de evaluación de preparación de máquinas virtuales de Azure](https://azure.microsoft.com/downloads/vm-readiness-assessment/) en las máquinas virtuales para garantizar que son compatibles con máquinas virtuales de Azure y Azure Site Recovery Services.
 
## <a name="implementation-checklist"></a>Lista de comprobación de implementación

Hablaremos sobre los distintos pasos para habilitar los servicios de Azure Site Recovery para la implementación de RDS con más detalle, pero estos son los pasos de implementación de alto nivel.

| **Paso 1: configurar las máquinas virtuales para la recuperación ante desastres**                                                                                                                                                                                               |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Hyper-V - descarga el proveedor de Microsoft Azure Site Recovery. Instálelo en su servidor VMM o el host de Hyper-V. Consulte [requisitos previos para la replicación en Azure mediante Azure Site Recovery](/azure/site-recovery/site-recovery-prereq) para obtener información.                                                                                                                             |
| VMWare: configurar el servidor de protección, el servidor de configuración y los servidores de destino maestro                                                                                                                                                      |
| **Paso 2: preparar los recursos**                                                                                                                                                                                                           |
| Agregar un [cuenta de Azure Storage](/azure/storage/storage-create-storage-account).                                                                                                                                                                                                              |
| Hyper-V - descargar el agente de Microsoft Azure Recovery Services e instálelo en los servidores host de Hyper-V.                                                                                                                                     |
| VMWare: asegúrese de que el servicio de movilidad se instala en todas las máquinas virtuales.                                                                                                                                                                           |
| [Habilitar la protección de máquinas virtuales en la nube de VMM, sitios de Hyper-V o VMWare sitios](rds-enable-dr-with-asr.md).                                                                                                                                                                    |
| **Paso 3: diseñar el plan de recuperación.**                                                                                                                                                                                                        |
| Asignar los recursos: asignación de redes locales a redes virtuales de Azure.                                                                                                                                                                              |
| [Crear el plan de recuperación](rds-disaster-recovery-plan.md). |
| Probar el plan de recuperación mediante la creación de una conmutación por error de prueba. Asegúrese de que todas las máquinas virtuales pueden tener acceso a los recursos necesarios, como Active Directory. Asegúrese de red se configuran las redirecciones y trabajar para RDS. Para obtener instrucciones detalladas sobre cómo probar el plan de recuperación, consulte [ejecutar una conmutación por error de prueba](/azure/site-recovery/site-recovery-test-failover-to-azure)|
| **Paso 4: ejecutar una exploración de recuperación ante desastres.**                                                                                                                                                                                                     |
| Ejecute un simulacro de recuperación ante desastres con conmutaciones por error planeadas y no planeadas. Asegúrese de que todas las máquinas virtuales tengan acceso a los recursos necesarios, como Active Directory. Asegúrese de que todas las máquinas virtuales tengan acceso a los recursos necesarios, como Active Directory. Para obtener instrucciones detalladas sobre las conmutaciones por error y cómo ejecutar maniobras de, consulte [conmutación por error en Site Recovery](/azure/site-recovery/site-recovery-failover).|


