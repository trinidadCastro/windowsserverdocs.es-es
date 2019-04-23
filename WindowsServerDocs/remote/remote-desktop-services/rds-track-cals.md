---
title: Realizar un seguimiento de las licencias de acceso de cliente de servicios de escritorio remoto (CAL de RDS)
description: Aprenda a realizar un seguimiento de CAL a través de la implementación de RDS.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 80d82d30-3ad0-4a8c-9a9b-2773c47eee19
author: lizap
ms.author: elizapo
ms.date: 05/11/2017
manager: dongill
ms.openlocfilehash: ed7490b2b61eb516d43b280b67fcefaeedb3e225
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890626"
---
# <a name="track-your-remote-desktop-services-client-access-licenses-rds-cals"></a>Realizar un seguimiento de las licencias de acceso de cliente de servicios de escritorio remoto (CAL de RDS)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar la herramienta Administrador de licencias de escritorio remoto para crear informes para realizar un seguimiento de las RDS CAL por usuario que ha emitido un servidor de licencias de escritorio remoto.

> [!NOTE]
>  Si usa Azure AD Domain Services en su entorno, la herramienta Administrador de licencias de escritorio remoto no funcionará para obtener las CAL por usuario. En su lugar, deberá realizar un seguimiento de licencias manualmente, ya sea a través de los eventos de inicio de sesión, el sondeo conexiones activas de escritorio remoto mediante el agente de conexión, u otro mecanismo que funcione para usted. 

Siga estos pasos para generar un informe de CAL de usuario por:

1. En el Administrador de licencias de escritorio remoto, haga clic en el servidor de licencias, haga clic en **crear informe**y, a continuación, haga clic en **uso de CAL por usuario**.
2. Establecer el ámbito para el informe: seleccione uno de los siguientes:
   - Todo el dominio: el dominio en el que el servidor de licencias es miembro.
   - Unidades organizativas: cualquier unidad organizativa del dominio en el que el servidor de licencias es miembro.
   - Todo el dominio y los dominios de confianza - puede incluir dominios de otros bosques. Si selecciona esta opción puede aumentar el tiempo que se tarda en crear el informe.

   La selección que realice Determina qué cuentas de usuario en AD DS se buscan información de CAL de RDS por usuario generar el informe.
3. Haga clic en **crear informe**. El informe se crea y aparece un mensaje para confirmar que el informe se creó correctamente. Haga clic en **Aceptar** para cerrar el mensaje.

El informe que ha creado aparece en la sección de informes en el nodo del servidor de licencias. El informe proporciona la siguiente información:

- Fecha y hora en que se creó el informe
- El ámbito del informe (p. ej., dominio, OU = Ventas o los dominios de confianza)
- El número de RDS CAL por usuario que están instalados en el servidor de licencias
- El número de RDS CAL por usuario que ha emitido el servidor de licencias específico para el ámbito del informe

También puede guardar el informe como un archivo CSV en una ubicación de carpeta en el equipo. Para guardar el informe, haga clic en el informe que desea guardar, haga clic en Guardar como y, a continuación, especifique el nombre de archivo y la ubicación para guardar el informe.

Los informes creados se muestran en el nodo de informes en el nodo del servidor de licencias en el Administrador de licencias de escritorio remoto. Si ya no necesita un informe, puede eliminarlo.
