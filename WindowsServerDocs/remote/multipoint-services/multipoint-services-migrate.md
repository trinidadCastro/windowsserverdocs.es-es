---
title: Migrar a MultiPoint Services en Windows Server 2016
description: Aprenda a migrar desde una versión anterior de MultiPoint Services
ms.custom: na
ms.date: 07/29/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 16c217ad-700a-48a3-8398-4a7f7e9edb52
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 24c35c31bf920c41bafa16901ee30a023565dad8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861206"
---
# <a name="multipoint-services-migration-in-windows-server-2016"></a>Migración de multiPoint Services en Windows Server 2016
>Se aplica a: Windows Server 2016

Puede migrar desde una versión anterior de Windows Server 2016 MultiPoint Services a la versión RTM de MultiPoint Services. La siguiente información proporciona preparación de la información de migración y verificación de los pasos.

Herramientas y documentación de migración simplifican la migración de la configuración del rol de servidor y los datos de un servidor existente a un servidor de destino que ejecuta Windows Server 2016. Mediante el proceso que se describe en esta guía puede simplificar el proceso de migración, reducir el tiempo de migración, aumentar la precisión del proceso de migración y contribuir a eliminar los posibles conflictos que, de otro modo, podrían producirse durante dicho proceso. 

## <a name="what-to-know-before-you-begin"></a>Lo que necesita saber antes de comenzar
Antes de comenzar el proceso de migración, tenga en cuenta lo siguiente:

- El proceso de migración no recopilar automáticamente ni registrar la configuración para las aplicaciones en el rol Servicios MultiPoint. Debe crear un plan de migración personalizado para las aplicaciones que se van a migrar. Esto también es true cuando se usa la característica de escritorios virtuales en MultiPoint Services.
- Esta guía no proporciona instrucciones para mover los datos guardan en el usuario o las carpetas comparten en el servidor MultiPoint. Esto se aplica a regulares estaciones y escritorios virtuales estaciones.
- Esta guía no incluye instrucciones sobre cómo migrar cuando el servidor de origen ejecuta varios roles. Si el servidor ejecuta varios roles, debe diseñar un procedimiento de migración personalizado específico para su entorno de servidor, según la información proporcionada en las guías de migración del rol.
- Esta guía no contiene información para migrar servicios de escritorio remoto CAL. Para obtener esta información, consulte [migrar escritorio servicios cliente licencias de acceso remoto (CAL de RDS)](https://technet.microsoft.com/library/dd851844.aspx).

## <a name="supported-migration-scenarios-for-multipoint-services-in-windows-server-2016"></a>Escenarios de migración admitidos para los servicios MultiPoint en Windows Server 2016
Los servicios de rol Servicios MultiPoint está disponible en Windows Server 2016 Standard y Datacenter. Esta guía de migración describe cómo migrar los servicios de rol Servicios Multipoint desde un servidor de origen que ejecuta Windows Server 2016 a un servidor de destino que ejecuta la misma versión.

## <a name="scenarios-that-are-not-supported"></a>Escenarios no admitidos

No se admiten los siguientes escenarios de migración:

- Migración o actualización desde Windows MultiPoint Server 2012 y 2011.
- Migración desde un servidor de origen a un servidor de destino que se está ejecutando en el sistema operativo con un sistema diferente instalado el idioma de interfaz de usuario.
- Migrar el servicio de rol Servicios MultiPoint de los servidores físicos a máquinas virtuales.
- Migración de aplicaciones o configuración de la aplicación desde el servidor MultiPoint.

## <a name="the-impact-of-migration-on-multipoint-services"></a>El impacto de la migración en MultiPoint Services
Tenga en cuenta que el rol Servicios MultiPoint no estará disponible durante la migración. Para minimizar el tiempo de inactividad y el impacto en los usuarios, planee llevar a cabo la migración de los datos durante las horas de menos actividad. Notifique a los usuarios que los recursos no estarán disponibles durante ese tiempo.

## <a name="migration-information-and-steps"></a>Pasos e información de migración
Use la siguiente información para planear y llevar a cabo la migración de MultiPoint Services:

- [Recopile la información que necesita para la migración.](multipoint-services-migration-preparation.md)
- [Migrar el servicio de rol de MultiPoint Services.](multipoint-services-migration-steps.md)
- [Validación de la migración y realice las tareas de limpieza posterior a la migración](multipoint-services-post-migration-steps.md)