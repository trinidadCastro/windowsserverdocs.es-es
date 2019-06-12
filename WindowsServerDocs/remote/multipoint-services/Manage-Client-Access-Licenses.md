---
title: Administrar licencias de acceso de cliente
description: Obtenga información sobre cómo trabajar con CAL en MultiPoint Services
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 675e089e-d841-401e-bba7-69f3929ef609
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: f5f78d3d2387d3b95177a6a8a40fb9b16d8ed8e2
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446123"
---
# <a name="manage-client-access-licenses"></a>Administrar licencias de acceso de cliente
Todas las estaciones que se conecta a un sistema MultiPoint Services, incluido el equipo que ejecuta MultiPoint Services que se usa como estación, deben tener un escritorio remoto válida por usuario *licencia de acceso de cliente (CAL)* .

Si usa en lugar de físicas estaciones de escritorios virtuales de estación, debe instalar una CAL para cada escritorio virtual de la estación.  
  
1.  Adquirir una licencia de cliente para cada estación que esté conectado a su equipo de MultiPoint Services o el servidor. Para obtener más información acerca de cómo comprar CAL, consulte la documentación de licencias de escritorio remoto. <!--@Liza: add link to RDS licensing here-->

2.  Desde el **iniciar** pantalla, abra **MultiPoint Manager**.  
  
3.  Haga clic en el **inicio** pestaña y, a continuación, haga clic en **agregar licencias de acceso de cliente**.  Esto abrirá la herramienta de administración de licencia CAL.

# <a name="set-the-licensing-mode-manually"></a>Establecer el modo de licencia manualmente
Si no configurado correctamente la configuración de MultiPoint Services le solicitará una notificación sobre el que se ha expirado el período de gracia. Siga estos pasos para establecer el modo de licencia:

1. Iniciar **Editor de directivas de grupo Local** (gpedit.msc).

2. En el panel izquierdo, vaya a **directiva de equipo Local -> configuración del equipo - > plantillas administrativas -> Windows -> componentes de servicios de escritorio remoto - > Host de sesión de escritorio remoto -> licencias**.

3. Haga clic en el panel derecho, **usar los servidores de licencias de escritorio remoto especificados** y seleccione **editar**:
   - En el cuadro de diálogo editor de directiva de grupo, seleccione **habilitado**
   - Escriba el nombre del equipo local en el **licencia servidores para usar** campo.
   - Seleccione **Aceptar**
  
4. Haga clic en el panel derecho, **establecer el modo de licencia de escritorio remoto** y seleccione **editar**
   - En el cuadro de diálogo editor de directiva de grupo, seleccione **habilitado**
   - Establecer el **modo de licencia** para cada dispositivo / por usuario
   - Seleccione **Aceptar** 

  
## <a name="see-also"></a>Vea también  
[Administrar tareas del sistema mediante MultiPoint Manager](Manage-System-Tasks-Using-MultiPoint-Manager.md)
