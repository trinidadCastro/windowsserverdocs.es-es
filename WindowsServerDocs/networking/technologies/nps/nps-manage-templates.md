---
title: Administrar plantillas NPS
description: Este tema proporciona instrucciones sobre cómo crear, aplicar, exportar e importar plantillas de NPS para el servidor de directivas de redes en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 989b00c5-4767-4081-ace5-6321f8b2c55e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 170a0e86f7dcca77c6efe841318b522554f8e78e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845136"
---
# <a name="manage-nps-templates"></a>Administrar plantillas NPS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar el servidor de directivas de red \(NPS\) plantillas para crear elementos de configuración, como Remote Authentication Dial-in User Service \(RADIUS\) clientes o los secretos compartidos, que puede volver a usar en el equipo local NPS y exportación para su uso en otros NPSs. 

Administración de plantillas proporciona un nodo en la consola NPS, donde puede crear, modificar, eliminar, duplicar y ver el uso de plantillas de NPS. Plantillas de NPS están diseñadas para reducir la cantidad de tiempo y el costo que se tarda en configuración de NPS en uno o varios servidores.

Los siguientes tipos de plantilla NPS están disponibles para la configuración de administración de plantillas.

- **Secretos compartidos**. Este tipo de plantilla permite especificar un secreto compartido que puede volver a usar (seleccionando la plantilla en la ubicación adecuada en la consola de NPS) al configurar clientes y servidores RADIUS. 

- **Los clientes RADIUS**. Este tipo de plantilla permite configurar opciones de cliente RADIUS que se pueden volver a utilizar seleccionando la plantilla en la ubicación adecuada en la consola de NPS.

- **Servidores remotos RADIUS**. Esta plantilla permite configurar los valores de servidor RADIUS remotos que se pueden volver a utilizar seleccionando la plantilla en la ubicación adecuada en la consola de NPS. 

- **Filtros IP**. Esta plantilla permite crear el protocolo de Internet versión 4 (IPv4) y protocolo de Internet versión 6 \(IPv6\) filtros que puede reutilizar \(seleccionando la plantilla en la ubicación adecuada en NPS consola\) al configurar las directivas de red.

## <a name="create-an-nps-template"></a>Crear una plantilla de NPS

Configuración de una plantilla es diferente a la configuración de NPS directamente. Creación de una plantilla no afecta a la funcionalidad de NPS. Es sólo al seleccionar la plantilla en la ubicación adecuada en la consola de NPS y aplicar la plantilla que la plantilla afecta a la funcionalidad NPS. 

Por ejemplo, si configura un cliente RADIUS en la consola NPS en **clientes y servidores RADIUS**, modificar la configuración de NPS y realizar un paso en la configuración de NPS para comunicarse con uno de los servidores de acceso de red. \(El siguiente paso es configurar el servidor de acceso de red \(NAS\) para comunicarse con NPS.\) 

Sin embargo, si configura un nuevo **clientes RADIUS** plantilla en la consola NPS en **administración de plantillas** en lugar de crear un nuevo cliente RADIUS en **clientes RADIUS y servidores**, ha creado una plantilla, pero no se ha alterado la funcionalidad NPS todavía. Para modificar la funcionalidad NPS, debe aplicar la plantilla desde la ubicación correcta en la consola de NPS.

El siguiente procedimiento proporciona instrucciones sobre cómo crear una nueva plantilla.

Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.

### <a name="to-create-an-nps-template"></a>Para crear una plantilla NPS


1. En el NPS, en el administrador del servidor, haga clic en **herramientas**y, a continuación, haga clic en **servidor de directivas de red**. Se abre la consola de NPS. 

2. En la consola NPS, expanda **administración de plantillas**, haga clic en un tipo de plantilla, como **clientes RADIUS**y, a continuación, haga clic en **New**.

3. Un nuevo cuadro de diálogo de propiedades de plantilla se abre que puede usar para configurar la plantilla.

## <a name="apply-an-nps-template"></a>Aplicar una plantilla de NPS

Puede usar una plantilla que ha creado en **administración de plantillas** navegando a una ubicación en la consola NPS, donde puede aplicar la plantilla. Por ejemplo, si desea aplicar una plantilla de secretos compartidos a una configuración de cliente RADIUS, puede usar el procedimiento siguiente.

Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.

### <a name="to-apply-an-nps-template"></a>Para aplicar una plantilla NPS

1. En el NPS, en el administrador del servidor, haga clic en **herramientas**y, a continuación, haga clic en **servidor de directivas de red**. Se abre la consola de NPS.

2. En la consola NPS, expanda **clientes y servidores RADIUS**y, a continuación, expanda **clientes RADIUS**.

3. en **clientes RADIUS**, en el panel de detalles, haga clic en el cliente RADIUS a la que desea aplicar la plantilla NPS y, a continuación, haga clic en **propiedades**.

4. En el cuadro de diálogo de propiedades cuadro para el cliente RADIUS, en **seleccionar una plantilla existente de secretos compartidos**, seleccione la plantilla que desea aplicar en la lista de plantillas.

## <a name="export-or-import-nps-templates"></a>Exportar o importar plantillas de NPS

Puede exportar plantillas para su uso en otros NPSs, o bien puede importar plantillas en **administración de plantillas** para su uso en el equipo local. 

Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.

### <a name="to-export-or-import-nps-templates"></a>Para exportar o importar plantillas de NPS

1. Para exportar las plantillas NPS, en la consola NPS, haga clic en **administración de plantillas**y, a continuación, haga clic en **exportar plantillas a un archivo**.

2. Para importar las plantillas NPS, en la consola NPS, haga clic en **administración de plantillas**y, a continuación, haga clic en **importar plantillas desde un equipo** o **importar plantillas desde un archivo**.


