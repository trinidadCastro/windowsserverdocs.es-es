---
title: Pasos para migrar servicios de MultiPoint
description: Le guía por los pasos para migrar a MultiPoint Services en Windows Server 2016
ms.custom: na
ms.date: 07/29/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3ee77efa-7cc5-4ddf-aaff-b5634a717014
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: b63ef4bf63ce990aa0b0ba7624905ba8f14dde98
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854816"
---
# <a name="migrate-to--multipoint-services-in-windows-server-2016"></a>Migrar a MultiPoint Services en Windows Server 2016

>Se aplica a: Windows Server 2016

Utilice los siguientes pasos más la información que recopiló en la hoja de cálculo de planeación de migración para migrar a MultiPoint Services en Windows Server 2016.

## <a name="transfer-server-settings"></a>Transferir la configuración de servidor
En el servidor de destino, abra MultiPoint Manager. Haga clic en **editar la configuración de servidor**. Aplicar la configuración de acuerdo con la hoja de cálculo de planeación de migración.

> [!NOTE]
> Si tiene que habilitar la protección de disco en el servidor de destino, espere hasta después de configurar MultiPoint Services.

## <a name="transfer-station-settings"></a>Transferir la configuración de la estación
Asegúrese de que las estaciones están conectadas al servidor de destino y a todas asignado antes de aplicar la configuración de la estación. Se detectarán automáticamente las estaciones. Siga las instrucciones en cada pantalla de la estación para definir la asignación de servidor de las estaciones de usuario y los dispositivos USB conectados. Aplicar la configuración de la estación preferido como se describe en la hoja de cálculo de planeación de migración.

## <a name="migrate-the-vdi-template"></a>Migrar la plantilla VDI

Para poder importar la plantilla VDI desde el servidor de origen, habilita escritorios virtuales del servidor de destino mediante MultiPoint Manager:

1. Vaya a la **escritorios virtuales** ficha en MultiPoint Manager.
2. Haga clic en **habilitado escritorios virtuales**. El servidor se instale el rol Hyper-V y, a continuación, reinicie.
3. Abra MultiPoint Manager y vuelva a **escritorios virtuales**.
4. Haga clic en **Importar plantilla de escritorio virtual**. Siga las instrucciones para importar la plantilla desde el servidor de origen.

> [!NOTE]
> Al importar una plantilla de escritorio virtual, se restablecerá cualquier personalización que se aplica a la plantilla. 

## <a name="next-step"></a>Paso siguiente
[Validar la nueva implementación de MultiPoint Services.](multipoint-services-post-migration-steps.md)