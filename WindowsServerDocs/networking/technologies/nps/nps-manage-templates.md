---
title: Administrar plantillas NPS
description: En este tema se proporcionan instrucciones sobre cómo crear, aplicar, exportar e importar plantillas de NPS para el servidor de directivas de redes en Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: 989b00c5-4767-4081-ace5-6321f8b2c55e
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 9e86c545fb467e7e6e381f0be5e0d12e43ce775c
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949351"
---
# <a name="manage-nps-templates"></a>Administrar plantillas NPS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar \( \) las plantillas de NPS del servidor de directivas de redes para crear elementos de configuración, como servicio de autenticación remota telefónica de usuario \( \) clientes RADIUS o secretos compartidos, que puede reutilizar en el NPS local y exportar para su uso en otros NPSs.

La administración de plantillas proporciona un nodo en la consola de NPS donde puede crear, modificar, eliminar, duplicar y ver el uso de las plantillas de NPS. Las plantillas de NPS están diseñadas para reducir la cantidad de tiempo y el costo que se tarda en configurar NPS en uno o varios servidores.

Los siguientes tipos de plantilla de NPS están disponibles para la configuración en la administración de plantillas.

- **Secretos compartidos**. Este tipo de plantilla permite especificar un secreto compartido que se puede reutilizar (si se selecciona la plantilla en la ubicación adecuada en la consola NPS) al configurar clientes y servidores RADIUS.

- **Clientes RADIUS**. Este tipo de plantilla permite configurar las opciones de cliente RADIUS que se pueden reutilizar seleccionando la plantilla en la ubicación adecuada en la consola NPS.

- **Servidores RADIUS remotos**. Esta plantilla permite configurar las opciones del servidor RADIUS remoto que puede reutilizar seleccionando la plantilla en la ubicación adecuada en la consola NPS.

- **Filtros IP**. Esta plantilla permite crear filtros IPv6 del Protocolo de Internet versión 4 (IPv4) y del Protocolo de Internet versión \( 6 \) , que puede reutilizar si \( selecciona la plantilla en la ubicación adecuada en la consola de NPS \) al configurar las directivas de red.

## <a name="create-an-nps-template"></a>Creación de una plantilla de NPS

La configuración de una plantilla es diferente de la configuración de NPS directamente. La creación de una plantilla no afecta a la funcionalidad del NPS. Solo cuando se selecciona la plantilla en la ubicación adecuada en la consola de NPS y se aplica la plantilla que la plantilla afecta a la funcionalidad de NPS.

Por ejemplo, si configura un cliente RADIUS en la consola de NPS en **clientes y servidores RADIUS**, modifica la configuración de NPS y realiza un paso en la configuración de NPS para comunicarse con uno de los servidores de acceso a la red. \(El siguiente paso consiste en configurar el servidor de acceso a la red \( NAS \) para comunicarse con NPS.\)

Sin embargo, si configura una nueva plantilla de **clientes RADIUS** en la consola de NPS en **Administración de plantillas** en lugar de crear un nuevo cliente RADIUS en **clientes y servidores RADIUS**, ha creado una plantilla, pero aún no ha modificado la funcionalidad de NPS. Para modificar la funcionalidad de NPS, debe aplicar la plantilla desde la ubicación correcta en la consola de NPS.

En el procedimiento siguiente se proporcionan instrucciones sobre cómo crear una nueva plantilla.

La pertenencia al grupo **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.

### <a name="to-create-an-nps-template"></a>Para crear una plantilla de NPS


1. En el NPS, en Administrador del servidor, haga clic en **herramientas** y, a continuación, haga clic en **servidor de directivas de redes**. Se abre la consola NPS.

2. En la consola de NPS, expanda **Administración de plantillas**, haga clic con el botón secundario en un tipo de plantilla, como **clientes RADIUS** y, a continuación, haga clic en **nuevo**.

3. Se abre un cuadro de diálogo nuevas propiedades de plantilla que puede usar para configurar la plantilla.

## <a name="apply-an-nps-template"></a>Aplicación de una plantilla de NPS

Puede usar una plantilla que haya creado en la **Administración de plantillas** Si navega a una ubicación en la consola de NPS en la que puede aplicar la plantilla. Por ejemplo, si desea aplicar una plantilla de secretos compartidos a una configuración de cliente RADIUS, puede usar el procedimiento siguiente.

La pertenencia al grupo **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.

### <a name="to-apply-an-nps-template"></a>Para aplicar una plantilla de NPS

1. En el NPS, en Administrador del servidor, haga clic en **herramientas** y, a continuación, haga clic en **servidor de directivas de redes**. Se abre la consola NPS.

2. En la consola NPS, expanda **clientes y servidores RADIUS** y, a continuación, expanda **clientes RADIUS**.

3.In **clientes RADIUS**, en el panel de detalles, haga clic con el botón secundario en el cliente RADIUS al que desea aplicar la plantilla de NPS y, a continuación, haga clic en **propiedades**.

4. En el cuadro de diálogo Propiedades del cliente RADIUS, en **Seleccione una plantilla de secretos compartidos existente**, seleccione la plantilla que desea aplicar en la lista de plantillas.

## <a name="export-or-import-nps-templates"></a>Exportar o importar plantillas de NPS

Puede exportar plantillas para usarlas en otros NPSs, o puede importar plantillas en la **Administración de plantillas** para usarlas en el equipo local.

La pertenencia al grupo **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.

### <a name="to-export-or-import-nps-templates"></a>Para exportar o importar plantillas de NPS

1. Para exportar plantillas NPS, en la consola NPS, haga clic con el botón derecho en **Administración de plantillas** y, a continuación, haga clic en **exportar plantillas a un archivo**.

2. Para importar plantillas NPS, en la consola NPS, haga clic con el botón derecho en **Administración de plantillas** y, a continuación, haga clic en **importar plantillas de un equipo** o **importar plantillas desde un archivo**.


