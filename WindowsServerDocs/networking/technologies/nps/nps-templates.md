---
title: Plantillas NPS
description: En este tema se proporciona información general de plantillas de servidor de directivas de red en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: fdfc0df1-21c7-492c-9fad-38fe9c7d935a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0647dbf0f99a01e32ba68475b439501e2dbeebfe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823316"
---
# <a name="nps-templates"></a>Plantillas NPS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Servidor de directivas de red \(NPS\) plantillas le permiten crear elementos de configuración, como Remote Authentication Dial-in User Service \(RADIUS\) clientes o los secretos compartidos, que puede volver a usar en el equipo local NPS y exportación para su uso en otros NPSs.

Plantillas de NPS están diseñadas para reducir la cantidad de tiempo y el costo que se tarda en configuración de NPS en uno o varios servidores. Los siguientes tipos de plantilla NPS están disponibles para la configuración de administración de plantillas:

- Secretos compartidos
- Clientes RADIUS
- Servidores remotos RADIUS
- Filtros IP
- Los grupos de servidores

Configuración de una plantilla es diferente a la configuración de NPS directamente. Creación de una plantilla no afecta a la funcionalidad de NPS. Es sólo cuando selecciona la plantilla en la ubicación adecuada en la consola NPS que la plantilla afecta a la funcionalidad NPS. 

Por ejemplo, si configura un cliente RADIUS en la consola NPS en los clientes y servidores RADIUS, tiene modifica la configuración de NPS y realizar un paso para configurar NPS para comunicarse con uno de los servidores de acceso de red \(de NAS\) . \(El siguiente paso sería configurar NAS para comunicarse con NPS.\) Sin embargo, si configura una nueva plantilla de clientes RADIUS en la consola NPS en **administración de plantillas** en lugar de crear un nuevo cliente RADIUS en **clientes y servidores RADIUS**, ha creado un plantilla, pero no modificado la funcionalidad NPS todavía. Para modificar la funcionalidad NPS, debe seleccionar la plantilla desde la ubicación correcta en la consola de NPS.

## <a name="creating-templates"></a>Creación de plantillas

Para crear una plantilla, abra la consola NPS, haga clic en un tipo de plantilla, como **filtros IP**y, a continuación, haga clic en **New**. Se abre un cuadro de diálogo de propiedades de plantilla nueva que le permite configurar la plantilla.

## <a name="using-templates-locally"></a>Mediante las plantillas de forma local

Puede usar una plantilla que ha creado en **administración de plantillas** navegando a una ubicación en la consola de NPS donde se puede aplicar la plantilla. Por ejemplo, si crea una nueva plantilla de secretos compartidos que desea aplicar a una configuración de cliente RADIUS, en **clientes y servidores RADIUS** y **clientes RADIUS**, abra las propiedades del cliente RADIUS. En **seleccionar una plantilla existente de secretos compartidos**, seleccione la plantilla que creó anteriormente en la lista de plantillas disponibles.

Para obtener más información acerca de NPS, consulte [servidor de directivas de redes (NPS)](nps-top.md).
