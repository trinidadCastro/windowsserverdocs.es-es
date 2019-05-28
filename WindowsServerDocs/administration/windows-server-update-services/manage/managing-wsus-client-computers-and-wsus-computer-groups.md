---
title: Administración de equipos cliente de WSUS y grupos de equipos de WSUS
description: Tema de Windows Server Update Service (WSUS) - administración de equipos cliente y grupos
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4b1ea915-0f9f-4f0e-8913-a1dd460d07ab
author: coreyp-at-msft
ms.author: coreyp
manager: lizapo
ms.date: 10/16/2017
ms.openlocfilehash: 4ede63ab08d204c29555b28ae3a73795291c321c
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222484"
---
# <a name="managing-wsus-client-computers-and-wsus-computer-groups"></a>Administración de equipos cliente de WSUS y grupos de equipos de WSUS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El nodo de equipos es el punto de acceso central en la consola de administración de WSUS para administrar dispositivos y equipos cliente de WSUS. En este nodo puede encontrar los diferentes grupos que ha configurado (más en el grupo de forma predeterminada, los equipos sin asignar).

## <a name="managing-client-computers"></a>Administración de equipos cliente
Selección de uno de los grupos de equipos en los **equipos** nodo bajo **opciones** hace que los equipos de ese grupo que se mostrará en el panel de detalles. Si un equipo se asigna a varios grupos, aparecerá en las listas de ambos grupos. Si selecciona un equipo en la lista, puede ver sus propiedades, que incluyen información general sobre el equipo y el estado de las actualizaciones, como la instalación o el estado de detección de una actualización de un equipo determinado. Puede filtrar la lista de equipos sometidos a un grupo de equipos especificado por estado. El valor predeterminado muestra solo los equipos para las actualizaciones que son necesarios o que han tenido errores de instalación; Sin embargo, puede filtrar la visualización por cualquier estado. Haga clic en **actualizar** después de cambiar el filtro de estado.

También puede administrar grupos de equipos en la página de los equipos, que incluye la creación de los grupos y asignar equipos a ellos. Para obtener más información acerca de cómo administrar grupos de equipos, consulte Administración de grupos de equipos en la siguiente sección de esta guía y [1.5. Planear grupos de equipos WSUS](../plan/plan-your-wsus-deployment.md#15-plan-wsus-computer-groups) en el paso 1: Preparar la implementación de WSUS de la Guía de implementación de WSUS.

> [!NOTE]
> En primer lugar, debe configurar los equipos cliente para ponerse en contacto con el servidor WSUS antes de poder administrar desde ese servidor. Hasta que realice esta tarea, el servidor WSUS no reconocerá los equipos cliente y no se mostrará en la lista en la página de los equipos. Para obtener más información acerca de cómo configurar los equipos cliente, consulte [1.5. Planear grupos de equipos WSUS](../plan/plan-your-wsus-deployment.md#15-plan-wsus-computer-groups) del paso 1: Preparar la implementación de WSUS y el paso 3: Configurar WSUS, en la Guía de implementación de WSUS.

## <a name="controlling-when-wsus-client-computers-install-updates"></a>Controlar cuando los equipos cliente WSUS instalan las actualizaciones
Hay dos métodos para controlar cuando los equipos cliente WSUS instalan actualizaciones:

-   Aprobación con fechas límite: Plazos estrictamente cuando se instala una actualización

-   Directivas de grupo de WSUS: Control de directivas de grupo cuando el agente de actualización de Windows analiza e instala las actualizaciones

    Para obtener más información, consulte: [Paso 5: Configuración de directivas de grupo para actualizaciones automáticas](../deploy/4-configure-group-policy-settings-for-automatic-updates.md), en la Guía de implementación de WSUS.

## <a name="managing-computer-groups"></a>Administrar grupos de equipos
WSUS le permite dirigir actualizaciones a grupos de equipos cliente para asegurarse de que determinados equipos reciban siempre las actualizaciones correspondientes en los momentos más oportunos. Por ejemplo, si todos los equipos de un departamento (como el equipo de Contabilidad) tienen una configuración específica, puede configurar un grupo para ese equipo, decidir las actualizaciones que se instalarán y después usar informes de WSUS para evaluar las actualizaciones correspondientes al equipo.

Los equipos siempre se asignan a la **todos los equipos** agrupar y seguirán asignados al **equipos sin asignar** grupo hasta que los asigne a otro grupo. Los equipos pueden pertenecer a más de un grupo.

Los grupos de equipos pueden configurarse en jerarquías (por ejemplo, el grupo Nómina y el grupo Cuentas por pagar debajo del grupo Contabilidad). Las actualizaciones aprobadas para un grupo superior se implementarán automáticamente en los grupos inferiores, así como para el propio grupo superior. Por lo tanto, si aprueba Update1 para el grupo de administración de cuentas, la actualización se implementará en todos los equipos del grupo Contabilidad, todos los equipos del grupo nómina y todos los equipos del grupo cuentas por pagar.

Dado que los equipos pueden asignarse a varios grupos, es posible que una actualización se apruebe más de una vez para el mismo equipo. No obstante, la actualización se implementará una sola vez y cualquier conflicto se resolverá en el servidor WSUS. Para continuar con el ejemplo anterior, si el EquipoA se asigna a las nóminas y los grupos de cuentas por pagar y Update1 se aprueba para ambos grupos, se implementará una sola vez.

Puede asignar equipos a grupos de equipos por medio de dos métodos: asignación del lado servidor o asignación del lado cliente. Con el lado servidor, mueve manualmente uno o más equipos cliente a un grupo de equipos a la vez. Con la asignación dirigida al cliente, se usa la directiva de grupo o se modifica la configuración del Registro en los equipos cliente para habilitar esos equipos de manera que se agreguen automáticamente a los grupos de equipos creados anteriormente. Este proceso puede incluir en el script e implementarse en muchos equipos a la vez. Debe especificar el método destinatarios que usará en el servidor WSUS, seleccione una de las dos opciones en el **equipos** sección de la **opciones** página.

> [!NOTE]
> Si un servidor WSUS se ejecuta en el modo de réplica, no se pueden crear grupos en ese servidor. Todos los grupos de equipos necesarios para los clientes del servidor réplica deben crearse en el servidor WSUS que es la raíz de la jerarquía de servidores WSUS. Para obtener más información acerca del modo de réplica, vea [el modo de réplica de WSUS ejecutando](running-wsus-replica-mode.md) y para obtener más información sobre cómo destinar de cliente y servidor, consulte la sección [1.5. Planear grupos de equipos WSUS](../plan/plan-your-wsus-deployment.md#15-plan-wsus-computer-groups) del paso 1: Preparar la implementación de WSUS en la Guía de implementación de WSUS.


