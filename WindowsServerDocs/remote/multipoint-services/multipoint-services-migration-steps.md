---
title: Pasos para migrar Multipoint Services
description: Le guía por los pasos necesarios para migrar a multipoint Services en Windows Server 2016
ms.date: 07/29/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 3ee77efa-7cc5-4ddf-aaff-b5634a717014
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: f2e293fafb8d6f5d84e9ea5a4ad8ef3b7fe2ba7d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858698"
---
# <a name="migrate-to--multipoint-services-in-windows-server-2016"></a>Migrar a multipoint Services en Windows Server 2016

>Se aplica a: Windows Server 2016

Use los pasos siguientes junto con la información que recopiló en la hoja de cálculo de planeamiento de la migración para migrar a multipoint Services en Windows Server 2016.

## <a name="transfer-server-settings"></a>Transferir la configuración del servidor
En el servidor de destino, abra Multipoint Manager. Haga clic en **Editar configuración del servidor**. Aplique la configuración de acuerdo con la hoja de cálculo de planeamiento de la migración.

> [!NOTE]
> Si necesita habilitar la protección de disco en el servidor de destino, espere hasta que configure Multipoint Services.

## <a name="transfer-station-settings"></a>Configuración de la estación de transferencia
Asegúrese de que las estaciones estén conectadas al servidor de destino y todas se hayan asignado antes de aplicar la configuración de la estación. Las estaciones se detectarán automáticamente. Siga las instrucciones de cada pantalla de la estación para definir la asignación de servidor de las estaciones de usuario y los dispositivos USB conectados. Aplique la configuración de la estación que prefiera como se indica en la hoja de cálculo de planeamiento de la migración.

## <a name="migrate-the-vdi-template"></a>Migración de la plantilla de VDI

Antes de poder importar la plantilla VDI desde el servidor de origen, habilite los escritorios virtuales en el servidor de destino mediante Multipoint Manager:

1. Vaya a la pestaña **escritorios virtuales** de Multipoint Manager.
2. Haga clic en **Habilitar escritorios virtuales**. El servidor instalará el rol de Hyper-V y, a continuación, se reiniciará.
3. Abra Multipoint Manager y navegue de nuevo a **escritorios virtuales**.
4. Haga clic en **Importar plantilla de escritorio virtual**. Siga las instrucciones para importar la plantilla desde el servidor de origen.

> [!NOTE]
> Al importar una plantilla de escritorio virtual, se restablecerá cualquier personalización aplicada a la plantilla. 

## <a name="next-step"></a>Paso siguiente
[Valide la nueva implementación de Multipoint Services.](multipoint-services-post-migration-steps.md)