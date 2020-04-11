---
title: Configuración de la recuperación ante desastres para RDS mediante la recuperación ante desastres de Azure
description: Aprende a usar la recuperación ante desastres de Azure en una implementación de RDS
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 06/12/2017
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 68fa7026a3198b7800c4855f8472f4a0bec62009
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858998"
---
# <a name="set-up-disaster-recovery-for-rds-using-azure-site-recovery"></a>Configuración de la recuperación ante desastres para RDS mediante Azure Site Recovery

>Se aplica a: Windows Server (Canal semianual), Windows Server 2019, Windows Server 2016

Puedes usar Azure Site Recovery para crear una solución de recuperación ante desastres para la implementación de Servicios de Escritorio remoto. 

[Azure Site Recovery](/azure/site-recovery/site-recovery-overview) es un servicio basado en Azure que ofrece funcionalidades de recuperación ante desastres mediante la coordinación de la replicación, conmutación por error y recuperación de máquinas virtuales. Azure Site Recovery admite varias tecnologías de replicación para replicar, proteger y conmutar por error de forma coherente y sin problemas máquinas virtuales y aplicaciones en nubes públicas o privadas o en las del proveedor de hospedaje. 

Utiliza la siguiente información para crear y validar la solución de recuperación ante desastres.

## <a name="disaster-recovery-deployment-options"></a>Opciones de implementación de la recuperación ante desastres

Puedes implementar RDS en servidores físicos o en máquinas virtuales que ejecutan Hyper-V o VMWare. Azure Site Recovery puede proteger las implementaciones tanto virtuales como locales en un sitio secundario o en Azure. En la tabla siguiente se muestran las diferentes implementaciones que admite RDS en escenarios de recuperación ante desastres de sitio a sitio y de sitio a Azure.

| Tipo de implementación                          | De sitio a sitio en Hyper-V | De sitio a Azure en Hyper-V | De sitio a Azure en VMWare | De sitio a Azure física |
|------------------------------------------|----------------------|-----------------------|---------------------|----------------------|-----------------------|------------------------|
| Escritorio virtual agrupado (no administrado)       |Sí|No|No|No |
| Escritorio virtual agrupado (administrado, sin UPD) | Sí|No|No|No|
| Sesiones de RemoteApp y escritorio (sin UPD) | Sí|Sí|Sí|Sí  |

## <a name="prerequisites"></a>Requisitos previos

Para configurar Azure Site Recovery para la implementación, asegúrate de cumplir los requisitos siguientes:

- Crear una [implementación de RDS local](rds-deploy-infrastructure.md).
- Agregar el [almacén de servicios de Azure Site Recovery](/azure/site-recovery/site-recovery-vmm-to-azure#create-a-recovery-services-vault) a tu suscripción a Microsoft Azure.
- Si vas a usar Azure como sitio de recuperación, ejecuta la [herramienta de evaluación de preparación de máquinas virtuales de Azure](https://azure.microsoft.com/downloads/vm-readiness-assessment/) en las máquinas virtuales para garantizar que son compatibles con las máquinas virtuales de Azure y con los servicios de Azure Site Recovery.
 
## <a name="implementation-checklist"></a>Lista de comprobación de la implementación

Trataremos los distintos pasos para habilitar los servicios de Azure Site Recovery para la implementación de RDS con más detalle, pero aquí están los pasos de la implementación de alto nivel.

| **Paso 1: Configuración de las máquinas virtuales para la recuperación ante desastres**                                                                                                                                                                                               |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Hyper-V: descarga el proveedor de Microsoft Azure Site Recovery. Instálalo en tu servidor VMM o en el host de Hyper-V. Consulta [Requisitos previos para la replicación en Azure mediante Azure Site Recovery](/azure/site-recovery/site-recovery-prereq) para más información.                                                                                                                             |
| VMWare: configura el servidor de protección, el servidor de configuración y los servidores de destino principales                                                                                                                                                      |
| **Paso 2: Preparación de los recursos**                                                                                                                                                                                                           |
| Agrega una [cuenta de almacenamiento de Azure](/azure/storage/storage-create-storage-account).                                                                                                                                                                                                              |
| Hyper-V: descarga el agente de Microsoft Azure Recovery Services e instálalo en los servidores host de Hyper-V.                                                                                                                                     |
| VMWare: asegúrate de que el servicio de movilidad se instala en todas las máquinas virtuales.                                                                                                                                                                           |
| [Habilita la protección de las máquinas virtuales en la nube de VMM, en los sitios de Hyper-V o en los sitios de VMWare](rds-enable-dr-with-asr.md).                                                                                                                                                                    |
| **Paso 3: Diseño del plan de recuperación.**                                                                                                                                                                                                        |
| Asigna tus recursos: asigna las redes locales a las redes virtuales de Azure.                                                                                                                                                                              |
| [Crea el plan de recuperación](rds-disaster-recovery-plan.md). |
| Prueba el plan de recuperación mediante la creación de una conmutación por error de prueba. Asegúrate de que todas las máquinas virtuales pueden acceder a los recursos necesarios, como Active Directory. Asegúrate de que los redireccionamientos de red están configurados y funcionan para RDS. Para obtener instrucciones detalladas sobre cómo probar el plan de recuperación, consulta [Ejecución de una conmutación por error de prueba](/azure/site-recovery/site-recovery-test-failover-to-azure).|
| **Paso 4: Ejecución de una exploración de recuperación ante desastres.**                                                                                                                                                                                                     |
| Ejecuta una exploración de recuperación ante desastres con conmutaciones por error planeadas y no planeadas. Asegúrate de que todas las máquinas virtuales tengan acceso a los recursos necesarios, como Active Directory. Asegúrate de que todas las máquinas virtuales tengan acceso a los recursos necesarios, como Active Directory. Para obtener instrucciones detalladas sobre las conmutaciones por error y cómo ejecutar exploraciones, consulta [Conmutación por error en Site Recovery](/azure/site-recovery/site-recovery-failover).|


