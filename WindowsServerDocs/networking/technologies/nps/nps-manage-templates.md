---
title: Administrar las plantillas de NPS
description: Este tema proporciona instrucciones sobre cómo crear, aplicar, exportar e importar plantillas NPS para el servidor de directivas de red en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 989b00c5-4767-4081-ace5-6321f8b2c55e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0a7f4c50d87c155c1adcd445eae8df23aab7730b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="manage-nps-templates"></a>Administrar las plantillas de NPS

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar el servidor de directivas de red \(NPS\) plantillas para crear elementos de configuración, como los clientes del servicio autenticación remota telefónica de usuario \(RADIUS\) o secretos compartidos, que se pueden reutilizar en el servidor NPS y la exportación para su uso en otros servidores NPS local. 

Administración de plantillas proporciona un nodo en la consola NPS donde puedes crear, modificar, eliminar, duplicar y ver el uso de plantillas NPS. Plantillas NPS están diseñadas para reducir la cantidad de tiempo y el costo que se tarda en configuración de NPS en uno o varios servidores.

Los siguientes tipos de plantilla NPS están disponibles para la configuración de administración de plantillas.

- **Secretos compartidos**. Este tipo de plantilla permite especificar un secreto compartido que puedes volver a usar (seleccionando la plantilla en la ubicación adecuada en la consola NPS) cuando se configuran de clientes y servidores RADIUS. 

- **Los clientes RADIUS**. Este tipo de plantilla permite establecer la configuración de cliente RADIUS que puedes volver a usar, selecciona la plantilla en la ubicación adecuada en la consola NPS.

- **Los servidores RADIUS remotos**. Esta plantilla permite configurar la configuración del servidor RADIUS remoto que se puede reutilizar seleccionando la plantilla en la ubicación adecuada en la consola NPS. 

- **Filtros IP**. Esta plantilla permite crear protocolo de Internet versión 4 (IPv4) y protocolo de Internet versión 6 filtros \(IPv6\) que se pueden reutilizar \ (seleccionando la plantilla en la ubicación adecuada en el console\ NPS) al configurar las directivas de red.

## <a name="create-an-nps-template"></a>Crear una plantilla NPS

Configuración de una plantilla es diferente a la configuración del servidor NPS directamente. Creación de una plantilla no afecta a la funcionalidad del servidor NPS. Es solo cuando selecciona la plantilla en la ubicación adecuada en la consola NPS y aplica la plantilla que la plantilla afecta a la funcionalidad del servidor NPS. 

Por ejemplo, si se configura un cliente RADIUS en la consola NPS en **clientes y servidores RADIUS**, puedes modificar la configuración del servidor NPS y realizar un paso en la configuración de NPS para comunicarse con uno de los servidores de acceso de red. \ (El siguiente paso es configurar el servidor de acceso de red \(NAS\) se comuniquen NPS. \) 

Sin embargo, si configuras una nueva **clientes RADIUS** plantilla en la consola NPS en **plantillas administración** en lugar de crear un nuevo cliente RADIUS en **clientes y servidores RADIUS**, has creado una plantilla, pero no ha modificado la funcionalidad del servidor NPS aún. Para modificar la funcionalidad del servidor NPS, debes aplicar la plantilla de la ubicación correcta en la consola NPS.

El siguiente procedimiento proporciona instrucciones sobre cómo crear una nueva plantilla.

Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.

### <a name="to-create-an-nps-template"></a>Para crear una plantilla NPS


1. En el servidor NPS, en el administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **el servidor de directivas de red**. Abre la consola NPS. 

2. En la consola NPS, expanda **plantillas administración**, haz clic en un tipo de plantilla, como **clientes RADIUS**y, a continuación, haz clic en **nueva**.

3. Un nuevo cuadro de diálogo de propiedades de plantilla abre que puedes usar para configurar tu plantilla.

## <a name="apply-an-nps-template"></a>Aplicar una plantilla NPS

Puedes usar una plantilla que se han creado en **plantillas administración** navegando a una ubicación en la consola NPS donde puede aplicar la plantilla. Por ejemplo, si quieres aplicar una plantilla de secretos compartidos en una configuración del cliente RADIUS, puedes usar el siguiente procedimiento.

Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.

### <a name="to-apply-an-nps-template"></a>Para aplicar una plantilla NPS

1. En el servidor NPS, en el administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **el servidor de directivas de red**. Abre la consola NPS.

2. En la consola NPS, expanda **clientes y servidores RADIUS**y después expande **clientes RADIUS**.

3. En **clientes RADIUS**, en el panel de detalles, haz clic en el cliente RADIUS al que quieres aplicar la plantilla NPS y, a continuación, haz clic en **propiedades**.

4. En el cuadro de diálogo Propiedades cuadro para el cliente RADIUS, en **seleccionar una plantilla existente de secretos compartidos**, selecciona la plantilla que va a aplicar en la lista de plantillas.

## <a name="export-or-import-nps-templates"></a>Exportar o importar plantillas NPS

Puedes exportar plantillas para su uso en otros servidores NPS o puede importar plantillas en **plantillas administración** para su uso en el equipo local. 

Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.

### <a name="to-export-or-import-nps-templates"></a>Para exportar o importar plantillas NPS

1. Para exportar plantillas NPS, en la consola NPS, haz clic en **plantillas administración**y, a continuación, haz clic en **exportar plantillas en un archivo**.

2. Para importar plantillas NPS, en la consola NPS, haz clic en **plantillas administración**y, a continuación, haz clic en **importar plantillas desde un equipo** o **importar plantillas de un archivo**.


