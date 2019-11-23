---
title: Plantillas NPS
description: En este tema se proporciona información general sobre las plantillas de servidor de directivas de redes en Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: fdfc0df1-21c7-492c-9fad-38fe9c7d935a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bafff7a6a15312ab1bdca2e7b98307bc94731d3a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405315"
---
# <a name="nps-templates"></a>Plantillas NPS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Servidor de directivas de redes \(las plantillas de\) de NPS permiten crear elementos de configuración, como Servicio de autenticación remota telefónica de usuario \(RADIUS\) clientes o secretos compartidos, que puede reutilizar en el NPS local y exportar para su uso en otros NPSs.

Las plantillas de NPS están diseñadas para reducir la cantidad de tiempo y el costo que se tarda en configurar NPS en uno o varios servidores. Los siguientes tipos de plantilla de NPS están disponibles para la configuración en la administración de plantillas:

- Secretos compartidos
- Clientes RADIUS
- Servidores RADIUS remotos
- Filtros IP
- Grupos de servidores de actualizaciones

La configuración de una plantilla es diferente de la configuración de NPS directamente. La creación de una plantilla no afecta a la funcionalidad del NPS. Solo cuando se selecciona la plantilla en la ubicación adecuada en la consola de NPS, la plantilla afecta a la funcionalidad de NPS. 

Por ejemplo, si configura un cliente RADIUS en la consola de NPS en clientes y servidores RADIUS, ha modificado la configuración de NPS y ha realizado un paso en la configuración de NPS para comunicarse con uno de los servidores de acceso a la red \(\)de NAS. \(el siguiente paso sería configurar el NAS para comunicarse con NPS. Sin embargo,\), si configura una nueva plantilla de clientes RADIUS en la consola de NPS en **Administración de plantillas** en lugar de crear un nuevo cliente RADIUS en **clientes y servidores RADIUS**, ha creado una plantilla, pero aún no ha modificado la funcionalidad de NPS. Para modificar la funcionalidad de NPS, debe seleccionar la plantilla en la ubicación correcta en la consola de NPS.

## <a name="creating-templates"></a>Crear plantillas

Para crear una plantilla, abra la consola de NPS, haga clic con el botón secundario en un tipo de plantilla, como **filtros IP**y, a continuación, haga clic en **nuevo**. Se abre un cuadro de diálogo nuevas propiedades de plantilla que le permite configurar la plantilla.

## <a name="using-templates-locally"></a>Uso de plantillas localmente

Puede usar una plantilla que haya creado en la **Administración de plantillas** Si navega a una ubicación en la consola de NPS en la que se puede aplicar la plantilla. Por ejemplo, si crea una nueva plantilla de secretos compartidos que desea aplicar a una configuración de cliente RADIUS, en **clientes y servidores RADIUS** y **clientes RADIUS**, abra las propiedades del cliente RADIUS. En **seleccionar una plantilla de secretos compartidos existente**, seleccione la plantilla que creó anteriormente en la lista de plantillas disponibles.

Para obtener más información acerca de NPS, consulte [servidor de directivas de redes (NPS)](nps-top.md).
