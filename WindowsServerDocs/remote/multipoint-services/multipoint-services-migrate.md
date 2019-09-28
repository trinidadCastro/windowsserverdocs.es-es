---
title: Migrar a multipoint Services en Windows Server 2016
description: Obtenga información sobre cómo migrar desde una versión anterior de Multipoint Services
ms.custom: na
ms.date: 07/29/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 16c217ad-700a-48a3-8398-4a7f7e9edb52
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 297de9ee2450856e24b9196a8bfb312991657e6d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389042"
---
# <a name="multipoint-services-migration-in-windows-server-2016"></a>Migración de Multipoint Services en Windows Server 2016
>Se aplica a: Windows Server 2016

Puede migrar desde una versión anterior de Windows Server 2016 Multipoint Services a la versión RTM de Multipoint Services. La siguiente información proporciona información de preparación y pasos de migración y comprobación.

La documentación y las herramientas de migración facilitan la migración de la configuración del rol de servidor y los datos de un servidor existente a un servidor de destino que ejecuta Windows Server 2016. Mediante el proceso que se describe en esta guía puede simplificar el proceso de migración, reducir el tiempo de migración, aumentar la precisión del proceso de migración y contribuir a eliminar los posibles conflictos que, de otro modo, podrían producirse durante dicho proceso. 

## <a name="what-to-know-before-you-begin"></a>Qué debe saber antes de empezar
Antes de comenzar el proceso de migración, tenga en cuenta lo siguiente:

- El proceso de migración no recopila ni registra automáticamente la configuración de las aplicaciones en el rol Multipoint Services. Debe crear un plan de migración personalizado para las aplicaciones que desee migrar. Esto también es así cuando se usa la característica escritorios virtuales de Multipoint Services.
- En esta guía no se proporcionan instrucciones para mover datos guardados en carpetas de usuario o compartidas en MultiPoint Server. Esto se aplica a las estaciones normales y a las estaciones de escritorios virtuales.
- En esta guía no se incluyen instrucciones sobre cómo migrar cuando el servidor de origen ejecuta varios roles. Si el servidor ejecuta varios roles, debe diseñar un procedimiento de migración personalizado específico para el entorno del servidor, en función de la información proporcionada en las guías de migración de roles.
- Esta guía no contiene información para migrar Servicios de Escritorio remoto cal. Para obtener esta información, vea [migrar servicios de escritorio remoto licencias de acceso de cliente (cal de RDS)](https://technet.microsoft.com/library/dd851844.aspx).

## <a name="supported-migration-scenarios-for-multipoint-services-in-windows-server-2016"></a>Escenarios de migración admitidos para Multipoint Services en Windows Server 2016
Los servicios de rol de Multipoint Service están disponibles en Windows Server 2016 Standard y Datacenter. En esta guía de migración se describe cómo migrar los servicios de rol de Multipoint Services desde un servidor de origen que ejecuta Windows Server 2016 a un servidor de destino que ejecuta la misma versión.

## <a name="scenarios-that-are-not-supported"></a>Escenarios no admitidos

No se admiten los siguientes escenarios de migración:

- Migración o actualización desde Windows MultiPoint Server 2012 y 2011.
- Migración de un servidor de origen a un servidor de destino que se ejecuta en el sistema operativo con un idioma de interfaz de usuario del sistema diferente instalado.
- Migración del servicio de rol Multipoint Services de servidores físicos a máquinas virtuales.
- Migración de aplicaciones o configuraciones de aplicaciones de MultiPoint Server.

## <a name="the-impact-of-migration-on-multipoint-services"></a>Impacto de la migración en Multipoint Services
Tenga en cuenta que el rol Multipoint Services no estará disponible durante la migración. Para minimizar el tiempo de inactividad y el impacto en los usuarios, planee llevar a cabo la migración de los datos durante las horas de menos actividad. Notifique a los usuarios que los recursos no estarán disponibles durante ese tiempo.

## <a name="migration-information-and-steps"></a>Información y pasos de la migración
Use la siguiente información para planear y llevar a cabo la migración de Multipoint Services:

- [Recopile la información que necesita para la migración.](multipoint-services-migration-preparation.md)
- [Migre el servicio de rol Multipoint Services.](multipoint-services-migration-steps.md)
- [Validar la migración y realizar las tareas de limpieza posteriores a la migración](multipoint-services-post-migration-steps.md)