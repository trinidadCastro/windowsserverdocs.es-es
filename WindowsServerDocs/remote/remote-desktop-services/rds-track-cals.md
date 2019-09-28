---
title: Seguimiento de las licencias de acceso de cliente para Servicios de Escritorio remoto (CAL de RDS)
description: Aprende a realizar un seguimiento de las licencias de acceso de cliente (CAL) en la implementación de RDS.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 278aa7a2d35aeacfaee8deddcd64e668a320853f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387122"
---
# <a name="track-your-remote-desktop-services-client-access-licenses-rds-cals"></a>Seguimiento de las licencias de acceso de cliente para Servicios de Escritorio remoto (CAL de RDS)

>Se aplica a: Windows Server (Canal semianual), Windows Server 2019 y Windows Server 2016

Puedes usar la herramienta Administrador de licencias de Escritorio remoto para crear informes de seguimiento de las licencias de acceso de cliente por usuario para Servicios de Escritorio remoto que un servidor de licencias de Escritorio remoto haya emitido.

> [!NOTE]
>  Si usas Azure AD Domain Services en el entorno, la herramienta Administrador de licencias de Escritorio remoto no funcionará para obtener las CAL por usuario. En vez de eso, deberás realizar un seguimiento de licencias manualmente, ya sea a través de los eventos de inicio de sesión, el sondeo de conexiones activas a Escritorio remoto mediante el agente de conexión o cualquier otro mecanismo que te funcione. 

Usa estos pasos para generar un informe de CAL por usuario:

1. En el Administrador de licencias de Escritorio remoto, haz clic con el botón derecho en el servidor de licencias, haz clic en **Crear informe** y, finalmente, haz clic en **Uso de CAL por usuario**.
2. Establece el ámbito del informe. Para ello, selecciona una de las opciones siguientes:
   - Dominio completo: el dominio en el que el servidor de licencias es miembro.
   - Unidad organizativa: cualquier unidad organizativa del dominio en el que el servidor de licencias es miembro.
   - Todo el dominio y los dominios de confianza: puede incluir dominios de otros bosques. Si seleccionas esta opción puede aumentar el tiempo que se tarda en crear el informe.

   La selección que realices determinará en qué cuentas de usuario de AD DS se buscará la información de CAL por usuario de RDS para generar el informe.
3. Haz clic en **Crear informe**. El informe se crea y aparece un mensaje para confirmar que se ha creado correctamente. Haga clic en **Aceptar** para cerrar el mensaje.

El informe que has creado aparece en la sección de informes en el nodo del servidor de licencias. El informe proporciona la siguiente información:

- Fecha y hora en que se creó el informe
- El ámbito del informe (por ejemplo, dominio, unidad organizativa = Ventas, o todos los dominios de confianza)
- El número de CAL por usuario de RDS que están instalados en el servidor de licencias
- El número de CAL por usuario de RDS que ha emitido el servidor de licencias específico para el ámbito del informe

También puedes guardar el informe como un archivo CSV en una ubicación de carpeta en el equipo. Para guardar el informe, haz clic con el botón derecho en el informe que deseas guardar, haz clic en Guardar como y, a continuación, especifica el nombre del archivo y la ubicación para guardar el informe.

Los informes creados se muestran en el nodo de informes del nodo del servidor de licencias en el Administrador de licencias de Escritorio remoto. Si ya no necesitas un informe, puedes eliminarlo.
