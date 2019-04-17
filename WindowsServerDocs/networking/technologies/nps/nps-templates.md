---
title: Plantillas NPS
description: Este tema proporciona una visión general de las plantillas de servidor de directivas de red en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: fdfc0df1-21c7-492c-9fad-38fe9c7d935a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2835959b8c076ef7b6aeb1fca31a62717ef95037
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="nps-templates"></a>Plantillas NPS

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Plantillas \(NPS\) el servidor de directivas de red te permiten crear elementos de configuración, como los clientes del servicio autenticación remota telefónica de usuario \(RADIUS\) o secretos compartidos, que se pueden reutilizar en el servidor NPS y la exportación para su uso en otros servidores NPS local.

Plantillas NPS están diseñadas para reducir la cantidad de tiempo y el costo que se tarda en configuración de NPS en uno o varios servidores. Los siguientes tipos de plantilla NPS están disponibles para la configuración de administración de plantillas:

- Secretos compartidos
- Clientes RADIUS
- Servidores RADIUS remotos
- Filtros IP
- Grupos de servidores

Configuración de una plantilla es diferente a la configuración del servidor NPS directamente. Creación de una plantilla no afecta a la funcionalidad del servidor NPS. Es solo cuando seleccionas la plantilla en la ubicación adecuada en la consola NPS que la plantilla afecta a la funcionalidad del servidor NPS. 

Por ejemplo, si configuras a un cliente RADIUS en la consola NPS en clientes y servidores RADIUS, ha modificado la configuración del servidor NPS y toma un solo paso en la configuración de NPS para comunicarse con uno de tu red acceso servidores \(NAS's\). \ (Sería el siguiente paso configurar el NAS se comuniquen NPS. \) sin embargo, si configuras una nueva plantilla de clientes de RADIUS en la consola NPS en **plantillas administración** en lugar de crear un nuevo cliente RADIUS en **clientes y servidores RADIUS**, has creado una plantilla, pero no ha modificado la funcionalidad del servidor NPS aún. Para modificar la funcionalidad del servidor NPS, debe seleccionar la plantilla de la ubicación correcta en la consola NPS.

## <a name="creating-templates"></a>Creación de plantillas

Para crear una plantilla, abre la consola NPS, haz clic en un tipo de plantilla, como **filtros IP**y, a continuación, haz clic en **nueva**. Se abre un nuevo cuadro de diálogo de propiedades de plantilla que te permite configurar tu plantilla.

## <a name="using-templates-locally"></a>Uso de plantillas localmente

Puedes usar una plantilla que hayas creado en **plantillas administración** navegando a una ubicación en la consola NPS que se aplicará la plantilla. Por ejemplo, si creas una nueva plantilla secretos compartidos que quieres aplicar a una configuración de cliente RADIUS, en **clientes y servidores RADIUS** y **clientes RADIUS**, abre las propiedades de cliente RADIUS. En **seleccionar una plantilla existente de secretos compartidos**, selecciona la plantilla que creaste anteriormente en la lista de plantillas disponibles.

Para obtener más información acerca de NPS, consulta [servidor de directivas de redes (NPS)](nps-top.md).
