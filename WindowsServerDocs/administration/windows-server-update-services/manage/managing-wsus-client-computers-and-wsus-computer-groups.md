---
title: Administración de equipos cliente de WSUS y grupos de equipos de WSUS
description: 'Tema de Windows Server Update Service (WSUS): Cómo administrar equipos cliente y grupos'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 454fa385dc9fb91218ad836d4ad34e92e9644dac
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361627"
---
# <a name="managing-wsus-client-computers-and-wsus-computer-groups"></a>Administración de equipos cliente de WSUS y grupos de equipos de WSUS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El nodo equipos es punto de acceso central en la consola de administración de WSUS para administrar los dispositivos y equipos cliente de WSUS. En este nodo puede encontrar los distintos grupos que ha configurado (más el grupo predeterminado, los equipos sin asignar).

## <a name="managing-client-computers"></a>Administración de equipos cliente
Al seleccionar uno de los grupos de equipos del nodo **equipos** en **Opciones** , los equipos de ese grupo se muestran en el panel de detalles. Si un equipo se asigna a varios grupos, aparecerá en las listas de ambos grupos. Si selecciona un equipo en la lista, puede ver sus propiedades, que incluyen detalles generales sobre el equipo y el estado de las actualizaciones para el mismo, como el estado de instalación o detección de una actualización para un equipo determinado. Puede filtrar la lista de equipos de un grupo de equipos determinado por estado. El valor predeterminado muestra solo los equipos para los que se necesitan actualizaciones o que han tenido errores de instalación; sin embargo, puede filtrar la presentación por cualquier estado. Haga clic en **Actualizar** después de cambiar el filtro de estado.

También puede administrar grupos de equipos en la página equipos, que incluye la creación de grupos y la asignación de equipos a ellos. Para obtener más información acerca de la administración de grupos de equipos, consulte Administración de grupos de equipos en la sección siguiente de esta guía y la sección [1,5. Planear grupos de equipos WSUS @ no__t-0 en el paso 1: Preparar la implementación de WSUS de la guía de implementación de WSUS.

> [!NOTE]
> En primer lugar, debe configurar los equipos cliente para que se pongan en contacto con el servidor WSUS antes de poder administrarlos desde ese servidor. Hasta que realice esta tarea, el servidor WSUS no reconocerá los equipos cliente y no aparecerán en la lista de la página equipos. Para obtener más información acerca de cómo configurar los equipos cliente, consulte [1,5. Planear grupos de equipos WSUS @ no__t-0 del paso 1: Preparar la implementación de WSUS y el paso 3: Configure WSUS en la guía de implementación de WSUS.

## <a name="controlling-when-wsus-client-computers-install-updates"></a>Controlar cuándo los equipos cliente de WSUS instalan actualizaciones
Hay dos métodos para controlar el momento en que los equipos cliente de WSUS instalan actualizaciones:

-   Aprobación con fechas límite: Las fechas límite se aplican estrictamente cuando se instala una actualización

-   Directivas de grupo de WSUS: Control de directivas de grupo cuando el agente de Windows Update examina e instala actualizaciones

    Para obtener más información, vea: [Paso 5: Configure directiva de grupo valores para Actualizaciones automáticas @ no__t-0, en la guía de implementación de WSUS.

## <a name="managing-computer-groups"></a>Administración de grupos de equipos
WSUS le permite dirigir actualizaciones a grupos de equipos cliente para asegurarse de que determinados equipos reciban siempre las actualizaciones correspondientes en los momentos más oportunos. Por ejemplo, si todos los equipos de un departamento (como el equipo de Contabilidad) tienen una configuración específica, puede configurar un grupo para ese equipo, decidir las actualizaciones que se instalarán y después usar informes de WSUS para evaluar las actualizaciones correspondientes al equipo.

Los equipos siempre se asignan al grupo **todos los equipos** y permanecen asignados al grupo **equipos sin asignar** hasta que los asigne a otro grupo. Los equipos pueden pertenecer a más de un grupo.

Los grupos de equipos pueden configurarse en jerarquías (por ejemplo, el grupo Nómina y el grupo Cuentas por pagar debajo del grupo Contabilidad). Las actualizaciones aprobadas para un grupo superior se implementarán automáticamente en los grupos inferiores, así como en el grupo más alto. Por lo tanto, si aprueba update1 para el grupo de contabilidad, la actualización se implementará en todos los equipos del grupo de contabilidad, en todos los equipos del grupo de nóminas y en todos los equipos del grupo cuentas por pagar.

Dado que los equipos pueden asignarse a varios grupos, es posible que una actualización se apruebe más de una vez para el mismo equipo. No obstante, la actualización se implementará una sola vez y cualquier conflicto se resolverá en el servidor WSUS. Para continuar con el ejemplo anterior, si se asigna el EquipoA a los grupos nómina y cuentas por pagar, y update1 se aprueba para ambos grupos, se implementará una sola vez.

Puede asignar equipos a grupos de equipos por medio de dos métodos: asignación del lado servidor o asignación del lado cliente. Con los destinatarios del lado servidor, mueva manualmente uno o más equipos cliente a un grupo de equipos a la vez. Con la asignación dirigida al cliente, se usa la directiva de grupo o se modifica la configuración del Registro en los equipos cliente para habilitar esos equipos de manera que se agreguen automáticamente a los grupos de equipos creados anteriormente. Este proceso puede incluirse en el script e implementarse en muchos equipos a la vez. Debe especificar el método de destinatarios que usará en el servidor WSUS seleccionando una de las dos opciones en la sección **equipos** de la página **Opciones** .

> [!NOTE]
> Si un servidor WSUS se ejecuta en el modo de réplica, no se pueden crear grupos en ese servidor. Todos los grupos de equipos necesarios para los clientes del servidor de réplicas deben crearse en el servidor WSUS que sea la raíz de la jerarquía de servidores WSUS. Para obtener más información acerca del modo de réplica, vea [ejecutar el modo de réplica de WSUS](running-wsus-replica-mode.md) y para obtener más información sobre la compatibilidad del lado servidor y el cliente, vea la sección [1,5. Planear grupos de equipos WSUS @ no__t-0 del paso 1: Preparar la implementación de WSUS en la guía de implementación de WSUS.


