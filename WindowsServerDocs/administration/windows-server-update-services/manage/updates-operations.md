---
title: Operaciones de actualizaciones
description: 'Tema de Windows Server Update Service (WSUS): Cómo administrar actualizaciones, incluido el proceso de aprobación'
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4cb7ff54-3014-4e91-842a-a7b831ea59ff
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2618586fdc38588bb58e122116345eb88a680a3f
ms.sourcegitcommit: 6423dfa9cecb3b06bdd563cae113c3e80a4ec330
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2019
ms.locfileid: "71105060"
---
# <a name="updates-operations"></a>Operaciones de actualizaciones

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Una vez que las actualizaciones se hayan sincronizado con el servidor WSUS, se examinarán automáticamente en busca de relevancia en los equipos cliente del servidor. Sin embargo, debe aprobar las actualizaciones antes de implementarlas en los equipos de la red. Cuando aprueba una actualización, básicamente está indicando a WSUS qué hacer con ella (las opciones son **instalar** o **rechazar** para una nueva actualización). Puede aprobar las actualizaciones para el grupo **todos los equipos** o para los subgrupos. Si no aprueba una actualización, su estado de aprobación sigue siendo **no aprobado**y el servidor WSUS permite a los clientes evaluar si necesitan o no la actualización.

Si el servidor WSUS se ejecuta en modo de réplica, no podrá aprobar actualizaciones en el servidor WSUS. Para obtener más información acerca del modo de réplica, vea [ejecutar el modo de réplica de WSUS](running-wsus-replica-mode.md).

## <a name="approving-updates"></a>Aprobación de actualizaciones
Puede aprobar la instalación de actualizaciones para todos los equipos de la red WSUS o para distintos grupos de equipos. Después de aprobar una actualización, puede realizar una o más de las siguientes acciones:

-   Aplique esta aprobación a los grupos secundarios, si los hay.

-   Establezca una fecha límite para la instalación automática. Cuando se selecciona esta opción, se establecen horas y fechas específicas para instalar actualizaciones, invalidando cualquier configuración en los equipos cliente. Además, puede especificar una fecha pasada para la fecha límite Si desea aprobar una actualización inmediatamente (para instalarla la próxima vez que los equipos cliente se pongan en contacto con el servidor WSUS).

-   Quitar una actualización instalada si la actualización admite la eliminación.

Hay dos consideraciones importantes que debe tener en cuenta:

-   En primer lugar, no se puede establecer una fecha límite para la instalación automática de una actualización si se requiere la intervención del usuario (por ejemplo, especificando una configuración relevante para la actualización). Para determinar si una actualización requerirá la intervención del usuario, consulte el campo de **entrada de usuario puede solicitar** en las propiedades de actualización para una actualización que se muestra en la página **actualizaciones** . Compruebe también si hay un mensaje en el cuadro **Aprobar actualizaciones** que indica: "**la actualización seleccionada requiere la intervención del usuario y no admite una fecha límite de instalación**".

-   Si hay actualizaciones para el componente de servidor de WSUS, no puede aprobar otras actualizaciones en los sistemas cliente hasta que se apruebe la actualización de WSUS. Verá este mensaje de advertencia en el cuadro de diálogo aprobar actualizaciones: "Hay actualizaciones de WSUS que no se han aprobado. Debe aprobar las actualizaciones de WSUS antes de aprobar esta actualización ". En este caso, debe hacer clic en el nodo actualizaciones de WSUS y asegurarse de que todas las actualizaciones de esa vista se han aprobado antes de volver a las actualizaciones generales.

#### <a name="to-approve-updates"></a>Para aprobar actualizaciones

1.  En la consola de administración de WSUS, haga clic en **actualizaciones** y, a continuación, haga clic en **todas las actualizaciones**.

2.  En la lista de actualizaciones, seleccione la actualización que desea aprobar y haga clic con el botón secundario (o vaya al panel acciones) y, en el cuadro de diálogo aprobar actualizaciones, seleccione el grupo de equipos para el que desea aprobar la actualización y haga clic en la flecha situada junto a ella.

3.  Seleccione **aprobado para la instalación**y, a continuación, haga clic en **aprobar**.

4.  La ventana progreso de la **aprobación** mostrará el progreso hasta completar la aprobación. Cuando se complete el proceso, aparecerá el botón **cerrar** . Haga clic en **Cerrar**.

5.  Para seleccionar una fecha límite, haga clic con el botón secundario en la actualización, seleccione el grupo de equipos adecuado, haga clic en la flecha situada junto a ella y, a continuación, haga clic en **fecha límite**.

    -   Puede seleccionar una de las fechas límite estándar (una semana, dos semanas, un mes) o puede hacer clic en **personalizado** para especificar una fecha y una hora.

    -   Si desea que se instale una actualización tan pronto como los equipos cliente se pongan en contacto con el servidor, haga clic en **personalizada**y, a continuación, establezca una fecha y hora en la fecha y hora actuales o en una en el pasado.

#### <a name="to-approve-multiple-updates"></a>Para aprobar varias actualizaciones

1.  En la consola de administración de WSUS, haga clic en **actualizaciones** y, a continuación, haga clic en **todas las actualizaciones**.

2.  Para seleccionar varias actualizaciones contiguas, presione **MAYÚS** mientras selecciona las actualizaciones. Para seleccionar varias actualizaciones no contiguas, mantenga presionada la **tecla Ctrl** mientras selecciona las actualizaciones.

3.  Haga clic con el botón secundario en la selección y haga clic en **aprobar**. Se abre el cuadro de diálogo **Aprobar actualizaciones** con el **Estado de aprobación** establecido para **mantener las aprobaciones existentes** y el botón **Aceptar** deshabilitado.

4.  Puede cambiar las aprobaciones de los grupos individuales, pero si lo hace, no afectará a las aprobaciones secundarias. Seleccione el grupo para el que desea cambiar la aprobación y haga clic en la flecha situada a su izquierda. En el menú contextual, haga clic en **aprobado para su instalación**.

5.  La aprobación del grupo seleccionado cambia para **instalarse**. Si hay grupos secundarios, su aprobación permanece manteniendo la **aprobación existente**. Para cambiar la aprobación de los grupos secundarios, haga clic en el grupo y, a continuación, haga clic en la flecha situada a su izquierda. En el menú contextual, haga clic en **aplicar a elementos secundarios**.

6.  Para establecer un elemento secundario específico para que herede toda su aprobación del elemento primario, haga clic en el elemento secundario y, a continuación, haga clic en la flecha situada a su izquierda. En el menú contextual, haga clic en **igual que el elemento primario**. Si establece un elemento secundario para que herede las aprobaciones, pero no cambia las aprobaciones principales, el secundario heredará las aprobaciones existentes del primario.

7.  Si desea que el comportamiento de aprobación cambie para todos los elementos secundarios, apruebe **todos los equipos**y, a continuación, elija **aplicar a elementos secundarios**.

8.  Haga clic en **Aceptar** después de establecer todas las aprobaciones. La ventana progreso de la **aprobación** mostrará el progreso hasta completar la aprobación. Una vez completado el proceso, el botón **cerrar** estará disponible. Haga clic en **Cerrar**.

## <a name="declining-updates"></a>Rechazar actualizaciones
Si selecciona esta opción, la actualización se quita de la lista predeterminada de actualizaciones disponibles y el servidor WSUS no ofrecerá la actualización a los clientes, ya sea para la evaluación o la instalación. Puede llegar a esta opción si selecciona una actualización o un grupo de actualizaciones y hace clic con el botón derecho en el panel acciones. Las actualizaciones rechazadas solo aparecerán en la lista de actualizaciones si selecciona **rechazada** en la lista de aprobación al especificar el filtro para la lista de actualizaciones en **Ver**.

#### <a name="to-decline-updates"></a>Para rechazar las actualizaciones

1.  En la consola de administración de WSUS, haga clic en **actualizaciones**y, a continuación, haga clic en **todas las actualizaciones**.

2.  En la lista de actualizaciones, seleccione una o varias actualizaciones que desee rechazar.

3.  Seleccione **rechazar**y, a continuación, haga clic en **sí** en el mensaje de confirmación.

## <a name="cleaning-up-declined-updates"></a>Limpieza de las actualizaciones rechazadas
Las actualizaciones rechazadas siguen consumiendo algunos recursos del servidor WSUS. Debe ejecutar el Asistente para la limpieza del servidor para quitar las actualizaciones rechazadas de la base de datos de WSUS. Vea: [El Asistente para la limpieza del servidor](the-server-cleanup-wizard.md), para obtener más detalles.

## <a name="reinstating-declined-updates"></a>Restableciendo actualizaciones rechazadas
Una vez que se ha rechazado una actualización, todavía puede restablecerla.

#### <a name="to-reinstate-declined-updates"></a>Para restablecer las actualizaciones rechazadas

1.  En la consola de administración de WSUS, haga clic en **actualizaciones** y, a continuación, haga clic en **todas las actualizaciones**.

2.  cambie la **aprobación** a **rechazada** y haga clic en **Actualizar**. La lista de cargas de actualizaciones rechazadas.

3.  En la lista de actualizaciones, seleccione una o varias actualizaciones rechazadas que desee restablecer.

4.  Para restablecer una actualización determinada, haga clic con el botón derecho en la actualización y seleccione **aprobar**. En el cuadro de diálogo **Aprobar actualizaciones** , haga clic en **Aceptar** para volver a aplicar el estado de aprobación "no aprobado" predeterminado. La actualización se mostrará en la lista como **no aprobada** en lugar de rechazarse.

Después de limpiar una actualización rechazada mediante el Asistente para la limpieza del servidor WSUS, se eliminará del servidor WSUS y ya no aparecerá en la vista todas las actualizaciones. Puede volver a importar las actualizaciones rechazadas y limpiadas del catálogo de Microsoft Update. Para obtener más información, vea [WSUS y el sitio del catálogo](wsus-and-the-catalog-site.md).

## <a name="change-an-approved-update-to-not-approved"></a>Cambiar una actualización aprobada a no aprobada
Si se ha aprobado una actualización y decide no instalarla en este momento y, en su lugar, desea guardarla para una hora futura, puede cambiar la actualización a un estado de no aprobado. Esto significa que la actualización permanecerá en la lista predeterminada de actualizaciones disponibles y notificará la conformidad del cliente, pero no se instalará en los clientes.

#### <a name="to-change-an-update-from-approved-to-not-approved"></a>Para cambiar una actualización de aprobado a no aprobado

1.  En la consola de administración de WSUS, haga clic en **actualizaciones**y, a continuación, haga clic en **todas las actualizaciones**.

2.  En la lista de actualizaciones, seleccione una o varias actualizaciones aprobadas que desee cambiar a no aprobadas.

3.  En el menú contextual o en el panel **acciones** , seleccione **no aprobado**y, a continuación, haga clic en **sí** en el mensaje de confirmación.

## <a name="approving-updates-for-removal"></a>Aprobación de actualizaciones para su eliminación
Puede aprobar una actualización para su eliminación (es decir, desinstalar una actualización ya instalada). Esta opción solo está disponible si la actualización ya está instalada y admite la eliminación. Puede especificar una fecha límite para la desinstalación de la actualización, o bien especificar una fecha pasada para la fecha límite Si desea quitar inmediatamente la actualización (la próxima vez que los equipos cliente se pongan en contacto con el servidor WSUS).

Es importante mencionar que no todas las actualizaciones admiten la eliminación. Puede ver si una actualización admite la eliminación seleccionando una actualización individual y examinando el panel de **detalles** . En **detalles adicionales**, verá la categoría **extraíble** . Si la actualización no se puede quitar a través de WSUS, en algunos casos se puede quitar con **Agregar o quitar programas** del **Panel de control**.

#### <a name="to-approve-updates-for-removal"></a>Para aprobar las actualizaciones para su eliminación

1.  En la consola de administración de WSUS, haga clic en **actualizaciones** y, a continuación, haga clic en **todas las actualizaciones**.

2.  En la lista de actualizaciones, seleccione una o varias actualizaciones que desee aprobar para su eliminación y haga clic con el botón derecho en ellas (o vaya al panel **acciones** ).

3.  En el cuadro de diálogo **Aprobar actualizaciones** , seleccione el grupo de equipos del que desea quitar la actualización y haga clic en la flecha situada junto a ella.

4.  Seleccione **aprobado para eliminación**y, a continuación, haga clic en el botón **quitar** .

5.  Una vez finalizada la aprobación de eliminación, puede seleccionar una fecha límite haciendo clic con el botón secundario en la actualización, seleccionando el grupo de equipos adecuado y haciendo clic en la flecha situada junto a ella. A continuación, seleccione **fecha límite**. Puede seleccionar una de las fechas límite estándar (una semana, dos semanas, un mes) o puede hacer clic en **personalizado** para seleccionar una fecha y hora específicas.

6.  Si desea quitar una actualización tan pronto como los equipos cliente se pongan en contacto con el servidor, haga clic en **personalizada**y establezca una fecha en el pasado.

## <a name="approving-updates-automatically"></a>Aprobar actualizaciones automáticamente
Puede configurar el servidor WSUS para la aprobación automática de determinadas actualizaciones. También puede especificar la aprobación automática de las revisiones de las actualizaciones existentes a medida que estén disponibles. Esta opción está seleccionada de forma predeterminada. Una revisión es una versión de una actualización en la que se han realizado cambios (por ejemplo, podría haber expirado o haber cambiado sus reglas de aplicabilidad). Si no decide aprobar la versión revisada de una actualización automáticamente, WSUS usará la versión anterior y debe aprobar manualmente la revisión de la actualización.

Puede crear reglas que el servidor WSUS aplicará automáticamente durante la sincronización. Especifique qué actualizaciones desea aprobar automáticamente para la instalación, por clasificación de actualizaciones, por producto y por grupo de equipos. Esto solo se aplica a las nuevas actualizaciones, en lugar de a las actualizaciones revisadas. También puede especificar una fecha límite de aprobación de la actualización, que establece un número de días y una hora específica de la oferta antes de que la actualización aprobada tenga la fecha límite: instalada. Estas opciones están disponibles en el panel **Opciones** , en **aprobaciones automáticas**.

#### <a name="to-automatically-approve-updates"></a>Para aprobar automáticamente las actualizaciones

1.  En la consola de administración de WSUS, haga clic en **Opciones**y, a continuación, en **aprobaciones automáticas**.

2.  En **Reglas de actualización**, haga clic en **Nueva regla**.

3.  En el cuadro de diálogo **Agregar regla** , en **paso 1: seleccionar propiedades**, seleccione si desea usar **cuando una actualización está en una clasificación específica** o **si una actualización está en un producto específico** (o ambos) como criterio. Opcionalmente, seleccione si desea **establecer una fecha límite** para la aprobación.

4.  En **paso 2: editar las propiedades** , haga clic en las propiedades subrayadas para seleccionar las clasificaciones, los productos y los grupos de equipos para los que desea aprobaciones automáticas, según corresponda. Opcionalmente, elija la fecha límite de aprobación de la actualización día y hora.

5.  En **el paso 3: Especifique un nombre de**cuadro, escriba un nombre único para la regla.

6.  Haga clic en **OK**.

Las reglas de aprobación automática no se aplicarán a las actualizaciones que requieran un contrato de licencia para el usuario final (CLUF) que todavía no se haya aceptado en el servidor. Si observa que la aplicación de una regla de aprobación automática no hace que se aprueben todas las actualizaciones relevantes, debe aprobar estas actualizaciones manualmente.

## <a name="automatically-approving-revisions-to-updates-and-declining-expired-updates"></a>Aprobar automáticamente las revisiones para las actualizaciones y rechazar las actualizaciones expiradas
La sección Aprobaciones automáticas del panel Opciones contiene una opción predeterminada para aprobar automáticamente las revisiones de las actualizaciones aprobadas. También puede configurar el servidor WSUS para que rechace automáticamente las actualizaciones expiradas. Si decide no aprobar la versión revisada de una actualización automáticamente, el servidor WSUS usará la revisión anterior y debe aprobar manualmente la revisión de la actualización.

> [!NOTE]
> Una revisión es una versión de una actualización que ha cambiado (por ejemplo, puede haber expirado o haber actualizado reglas de aplicabilidad).

#### <a name="to-automatically-approve-revisions-to-updates-and-decline-expired-updates"></a>Para aprobar automáticamente las revisiones para las actualizaciones y rechazar las actualizaciones expiradas

1.  En la consola de administración de WSUS, haga clic en **Opciones**y, a continuación, en **aprobaciones automáticas**.

2.  En la pestaña **Opciones avanzadas** , asegúrese de que **aprobar automáticamente las nuevas revisiones de las actualizaciones aprobadas** y **rechazar automáticamente las actualizaciones cuando se selecciona una nueva revisión hace que expiren** .

3.  Haga clic en Aceptar.

    > [!NOTE]
    > Mantener los valores predeterminados para estas opciones permite mantener un buen rendimiento en la red WSUS. Si no quiere que las actualizaciones expiradas se rechacen automáticamente, debe asegurarse de rechazarlas de forma periódica.

## <a name="automatically-declining-superseded-updates"></a>Rechazar actualizaciones reemplazadas automáticamente
Cuando aprueba una actualización nueva que reemplaza a una actualización existente que se aprueba automáticamente, la actualización reemplazada se convierte en "no aplicable" en un equipo o dispositivo una vez que se ha instalado la actualización más reciente. Puede comprobar en la consola de WSUS que una actualización no es aplicable a todos los equipos. En ese caso, la actualización se puede rechazar de forma segura. Además, la actualización puede rechazarse automáticamente al ejecutar el Asistente para la limpieza del servidor WSUS.

Para buscar actualizaciones reemplazadas, puede seleccionar la columna de marca "reemplazada" en la vista todas las actualizaciones y ordenar por esa columna. Habrá cuatro grupos:

-   Actualizaciones que nunca se han reemplazado (un icono en blanco).

-   Actualizaciones que se han reemplazado, pero que nunca han reemplazado otra actualización (un icono con un cuadrado azul en la parte inferior).

-   Actualizaciones que se han reemplazado y que han reemplazado otra actualización (un icono con un cuadrado azul en el centro).

-   Actualizaciones que han reemplazado otra actualización (un icono con un cuadrado azul en la parte superior).

No hay ninguna característica en Windows Server Update Services que rechace automáticamente las actualizaciones reemplazadas tras la aprobación de una actualización más reciente. Se recomienda establecer primero la aprobación en "no aprobado" y, a continuación, usar el Asistente para la limpieza del servidor para rechazar automáticamente la actualización cuando se cumplan todas las condiciones pertinentes. Para más información, consulte: [Asistente para la limpieza del servidor](the-server-cleanup-wizard.md).

## <a name="approving-superseding-or-superseded-updates"></a>Aprobación de actualizaciones reemplazadas o reemplazadas
Normalmente, una actualización que reemplaza a otras actualizaciones realiza una o varias de las siguientes acciones:

-   Mejora o agrega la revisión proporcionada por una o varias actualizaciones publicadas anteriormente.

-   Mejora la eficacia del paquete de archivos de actualización, que se instala en los equipos cliente si se aprueba la actualización para la instalación. Por ejemplo, la actualización reemplazada puede contener archivos que ya no son relevantes para la corrección o para los sistemas operativos que ahora son compatibles con la nueva actualización, de modo que esos archivos no se incluyen en el paquete de archivos de la actualización de reemplazo.

-   Actualiza las versiones más recientes de los sistemas operativos. También es importante tener en cuenta que la actualización de reemplazo podría no ser compatible con versiones anteriores de sistemas operativos.

Por el contrario, una actualización que es reemplazada por otra actualización realiza lo siguiente:

-   Corrige un problema similar al de la actualización que la sustituye. Sin embargo, la actualización que la sustituye podría mejorar la solución que proporciona la actualización reemplazada.

-   Actualiza versiones anteriores de sistemas operativos. En algunos casos, la actualización de reemplazo ya no actualiza estas versiones de los sistemas operativos.

En el panel de detalles de una actualización individual, un icono informativo y un mensaje en la parte superior indican que se reemplaza o se reemplaza por otra actualización. Además, puede determinar qué actualizaciones sustituyen o si se sustituyen por la actualización examinando las actualizaciones que **sustituyen a esta actualización** y **las actualizaciones reemplazadas por estas** entradas de actualización en la sección **detalles adicionales** de la  **Propiedades**. El panel de detalles de una actualización se muestra debajo de la lista de actualizaciones.

WSUS no rechaza automáticamente las actualizaciones reemplazadas y se recomienda que no asuma que las actualizaciones reemplazadas se deben rechazar en favor de la nueva actualización de reemplazo. Antes de rechazar una actualización reemplazada, asegúrese de que ya no la necesita ninguno de los equipos cliente. A continuación se muestran ejemplos de escenarios en los que es posible que deba instalar una actualización reemplazada:

-   Si una actualización de reemplazo solo es compatible con las versiones más recientes de un sistema operativo y algunos de los equipos cliente ejecutan versiones anteriores del sistema operativo.

-   Si una actualización de sustitución tiene una aplicabilidad más restringida que la actualización que reemplaza, lo que haría inadecuada para algunos equipos cliente.

-   Si una actualización ya no sustituye a una actualización publicada anteriormente debido a cambios nuevos. Es posible que, a través de los cambios en cada versión, una actualización ya no reemplace a una actualización que se haya reemplazado anteriormente en una versión anterior. En este escenario, seguirá viendo un mensaje acerca de la actualización reemplazada, aunque la actualización que la sustituye se haya reemplazado por una actualización que no la tenga.


