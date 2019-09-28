---
title: Administrar licencias de acceso de cliente
description: Aprenda a trabajar con cal en Multipoint Services
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 0ca951c5e4c4fcdba06d0b475a7d7536a9c7f91f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395455"
---
# <a name="manage-client-access-licenses"></a>Administrar licencias de acceso de cliente
Cada estación que se conecta a un sistema Multipoint Services, incluido el equipo que ejecuta Multipoint Services que se usa como estación, debe tener una licencia de *acceso de cliente (cal)* de escritorio remoto por usuario válida.

Si usa escritorios virtuales de estación en lugar de estaciones físicas, debe instalar una CAL para cada escritorio virtual de estación.  
  
1.  Compre una licencia de cliente para cada estación que esté conectada al equipo o servidor Multipoint Services. Para obtener más información acerca de la compra de cal, visite la documentación de Escritorio remoto licencias. 

2.  En la pantalla **Inicio** , Abra **Multipoint Manager**.  
  
3.  Haga clic en la pestaña **Inicio** y, a continuación, haga clic en **Agregar licencias de acceso de cliente**.  Se abrirá la herramienta de administración de licencias de CAL.

# <a name="set-the-licensing-mode-manually"></a>Establecer el modo de licencia manualmente
Si no se ha configurado correctamente, la configuración de Multipoint Services le pedirá una notificación sobre el período de gracia que ha expirado. Siga estos pasos para establecer el modo de licencia:

1. Inicie **Editor de directivas de grupo local** (gpedit. msc).

2. En el panel izquierdo, vaya a **Directiva de equipo local-> configuración de equipo-> Plantillas administrativas-> componentes de Windows-> servicios de escritorio remoto-> Escritorio remoto host de sesión-> licencias**.

3. En el panel derecho, haga clic con el botón derecho en **usar el escritorio remoto servidores de licencias especificados** y seleccione **Editar**:
   - En el cuadro de diálogo Editor de directivas de grupo, seleccione **habilitado** .
   - Escriba el nombre del equipo local en el campo **servidores de licencias que se usarán** .
   - Seleccione **Aceptar** .
  
4. En el panel derecho, haga clic con el botón derecho en **establecer el modo de licencia de escritorio remoto** y seleccione **Editar** .
   - En el cuadro de diálogo Editor de directivas de grupo, seleccione **habilitado** .
   - Establecer el **modo de licencia** en por dispositivo/por usuario
   - Seleccione **Aceptar** . 

  
## <a name="see-also"></a>Vea también  
[Administrar tareas del sistema mediante MultiPoint Manager](Manage-System-Tasks-Using-MultiPoint-Manager.md)
