---
title: Operaciones de actualizaciones
description: Tema de Windows Server Update Service (WSUS) - administración de actualizaciones, incluido el proceso de aprobación
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
ms.openlocfilehash: 4d99e006a03e12d7201390748aec8671236cf297
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822556"
---
# <a name="updates-operations"></a>Operaciones de actualizaciones

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Después de que las actualizaciones se han sincronizado con su servidor WSUS, se examinarán de automáticamente por importancia para los equipos cliente del servidor. Sin embargo, debe aprobar las actualizaciones antes de implementarlas en los equipos de la red. Cuando aprueba una actualización, básicamente está indicando a WSUS qué hacer con él (las opciones son **instalar** o **rechazar** para una nueva actualización). Puede aprobar las actualizaciones para el **todos los equipos** o grupo para subgrupos. Si no aprueba una actualización, su estado de aprobación permanece **no aprobado**, y el servidor WSUS permite a los clientes a evaluar si necesitan la actualización.

Si su servidor WSUS se está ejecutando en modo de réplica, no podrá aprobar las actualizaciones en el servidor WSUS. Para obtener más información acerca del modo de réplica, vea [el modo de réplica de WSUS ejecutando](running-wsus-replica-mode.md).

## <a name="approving-updates"></a>Aprobación de actualizaciones
Puede aprobar la instalación de actualizaciones para todos los equipos de la red WSUS o grupos de equipos diferentes. Después de aprobar una actualización, puede hacer una (o más) de las siguientes acciones:

-   Esta aprobación se aplican a grupos secundarios, si existe.

-   Establecer una fecha límite para la instalación automática. Cuando se selecciona esta opción, se establecen horas específicas y fechas para instalar actualizaciones, invalidar cualquier configuración en los equipos cliente. Además, puede especificar una fecha pasada para la fecha límite si desea aprobar una actualización inmediatamente (para instalar la próxima vez que los equipos cliente, póngase en contacto con el servidor WSUS).

-   Quitar una actualización instalada si esa actualización admite la eliminación.

Hay dos consideraciones importantes que debe tener en cuenta:

-   En primer lugar, no se puede establecer una fecha límite para la instalación automática de una actualización si es necesario (por ejemplo, si se especifica un valor importante a la actualización) proporcionados por el usuario. Para determinar si una actualización requerirá intervención del usuario, examine la **puede solicitar la intervención del usuario** campo en las propiedades de actualización de una actualización que se muestra en el **actualiza** página. También buscará un mensaje en el **aprobar actualizaciones** cuadro que dice, "**la actualización seleccionada requiere la intervención del usuario y no es compatible con una fecha límite de instalación**."

-   Si hay actualizaciones para el componente de servidor WSUS, no se puede aprobar otras actualizaciones a los sistemas cliente hasta que se aprueba la actualización WSUS. Verá este mensaje de advertencia en el cuadro de diálogo Aprobar actualizaciones: "Hay actualizaciones de WSUS que no se han aprobado. Debe aprobar las actualizaciones WSUS antes de aprobar esta actualización." En este caso, debe haga clic en el nodo de actualizaciones de WSUS y asegúrese de que todas las actualizaciones en esa vista se ha aprobado antes de volver a las actualizaciones generales.

#### <a name="to-approve-updates"></a>Para aprobar actualizaciones

1.  En la consola de administración de WSUS, haga clic en **actualizaciones** y, a continuación, haga clic en **todas las actualizaciones**.

2.  En la lista de actualizaciones, seleccione la actualización que desea aprobar y haga clic en (o vaya al panel de acciones), y en el cuadro de diálogo Aprobar actualizaciones, seleccione el grupo de equipos para el que desea aprobar la actualización y haga clic en la flecha situada junto a ella.

3.  Seleccione **aprobada para su instalación**y, a continuación, haga clic en **aprobar**.

4.  El **progreso de la aprobación** ventana mostrará el progreso para completar la aprobación. Cuando el proceso haya finalizado, el **cerrar** aparece el botón. Haga clic en **Cerrar**.

5.  Puede seleccionar una fecha límite haciendo clic en la actualización, seleccionando el grupo de equipos adecuado, al hacer clic en la flecha situada junto a ella y, a continuación, haga clic en **fecha límite**.

    -   Puede seleccionar uno de los plazos estándares (una semana, dos semanas, un mes), o puede hacer clic **personalizado** para especificar una fecha y hora.

    -   Si desea que una actualización se instale tan pronto como el contacto de los equipos cliente del servidor, haga clic en **personalizado**y, a continuación, establezca una fecha y hora a la fecha y hora actuales o a uno en el pasado.

#### <a name="to-approve-multiple-updates"></a>Para aprobar varias actualizaciones

1.  En la consola de administración de WSUS, haga clic en **actualizaciones** y, a continuación, haga clic en **todas las actualizaciones**.

2.  Para seleccionar varias actualizaciones contiguas, presione **MAYÚS** mientras selecciona las actualizaciones. Para seleccionar varias actualizaciones no contiguas, presione y mantenga presionada la tecla **CTRL** mientras selecciona las actualizaciones.

3.  Haga clic en la selección y haga clic en **aprobar**. El **aprobar actualizaciones** abre el cuadro de diálogo con el **estado de aprobación** establecido en **mantener aprobaciones existentes** y **Aceptar** botón deshabilitado.

4.  Puede cambiar las aprobaciones para grupos individuales, pero al hacerlo así no tendrá ningún efecto aprobaciones secundarios. Seleccione el grupo para el que desea cambiar la aprobación y haga clic en la flecha situada a su izquierda. En el menú contextual, haga clic en **aprobada para su instalación**.

5.  La aprobación para el grupo seleccionado cambia a **instalar**. Si existen todos los grupos secundarios, sigue siendo su aprobación **mantener aprobación existente**. Para cambiar la aprobación de los grupos secundarios, haga clic en el grupo y haga clic en la flecha situada a su izquierda. En el menú contextual, haga clic en **aplicar a los elementos secundarios**.

6.  Para establecer un objeto secundario concreto para heredar todos su aprobación del principal, haga clic en el elemento secundario y haga clic en la flecha situada a su izquierda. En el menú contextual, haga clic en **igual al elemento primario**. Si establece un elemento secundario que heredan las aprobaciones, pero no cambian las aprobaciones primario, secundario hereda las aprobaciones existentes del elemento primario.

7.  Si desea que el comportamiento de aprobación para cambiar todos los elementos secundarios, aprobar **todos los equipos**y, a continuación, elija **aplicar a los elementos secundarios**.

8.  Haga clic en **Aceptar** después de establecer todas las aprobaciones. El **progreso de la aprobación** ventana mostrará el progreso para completar la aprobación. Cuando el proceso haya finalizado, el **cerrar** botón estará disponible. Haga clic en **Cerrar**.

## <a name="declining-updates"></a>Rechazar actualizaciones
Si selecciona esta opción, la actualización se quita de la lista predeterminada de las actualizaciones disponibles y el servidor WSUS no ofrecerá la actualización a los clientes, ya sea por evaluación o la instalación. Puede llegar a esta opción seleccionando una actualización o el grupo de actualizaciones y con el botón secundario o vaya al panel de acciones. Las actualizaciones rechazadas aparecerán en la lista de actualizaciones sólo si selecciona **rechazada** en la lista de aprobación al especificar el filtro de la lista de actualizaciones en **vista**.

#### <a name="to-decline-updates"></a>Para rechazar actualizaciones

1.  En la consola de administración de WSUS, haga clic en **actualizaciones**y, a continuación, haga clic en **todas las actualizaciones**.

2.  En la lista de actualizaciones, seleccione una o varias actualizaciones que desea rechazar.

3.  Seleccione **rechazar**y, a continuación, haga clic en **Sí** en el mensaje de confirmación.

## <a name="cleaning-up-declined-updates"></a>limpieza de las actualizaciones rechazadas
Las actualizaciones rechazadas siguen consumiendo algunos recursos de servidor WSUS. Debe ejecutar la limpieza del servidor el Asistente para quitar las actualizaciones rechazadas de la base de datos WSUS. Vea: [El Asistente para la limpieza del servidor](the-server-cleanup-wizard.md), para obtener más detalles.

## <a name="reinstating-declined-updates"></a>Restablecimiento de las actualizaciones rechazadas
Después de que se ha rechazado una actualización, todavía puede restablecerla.

#### <a name="to-reinstate-declined-updates"></a>Para restablecer las actualizaciones rechazadas

1.  En la consola de administración de WSUS, haga clic en **actualizaciones** y, a continuación, haga clic en **todas las actualizaciones**.

2.  cambiar **aprobación** a **rechazada** y haga clic en **actualizar**. Carga la lista de las actualizaciones rechazadas.

3.  En la lista de actualizaciones, seleccione una o varias actualizaciones rechazadas que desea restablecer.

4.  Para restablecer una actualización concreta, haga clic en la actualización y seleccione **aprobar**. En el **aprobar actualizaciones** cuadro de diálogo, haga clic en **Aceptar** para volver a aplicar el estado de aprobación predeterminada "No aprobado". La actualización se mostrará en la lista como **no aprobado** en lugar de rechazada.

Después de una actualización rechazada se limpia mediante el uso de la limpieza del servidor de WSUS de asistente, se eliminarán del servidor WSUS y ya no aparecerá en la vista de todas las actualizaciones. Puede volver a importar rechazada, limpiado actualizaciones desde el catálogo de Microsoft Update. Para obtener más información, consulte [WSUS y el sitio del catálogo](wsus-and-the-catalog-site.md).

## <a name="change-an-approved-update-to-not-approved"></a>cambiar una actualización aprobada a no aprobado
Si se ha aprobado una actualización y decide no instalar en este momento y en su lugar desea guardarlo para una fecha futura, puede cambiar la actualización en un estado no aprobado. Esto significa que la actualización permanecerá en la lista predeterminada de las actualizaciones disponibles y notificará la conformidad de clientes, pero no se instalará en los clientes.

#### <a name="to-change-an-update-from-approved-to-not-approved"></a>Para cambiar una actualización de aprobado a no aprobado

1.  En la consola de administración de WSUS, haga clic en **actualizaciones**y, a continuación, haga clic en **todas las actualizaciones**.

2.  En la lista de actualizaciones, seleccione una o varias actualizaciones aprobadas que desea cambiar a no aprobado.

3.  En el menú contextual o **acciones** panel, seleccione **no aprobado**y, a continuación, haga clic en **Sí** en el mensaje de confirmación.

## <a name="approving-updates-for-removal"></a>Aprobación de actualizaciones para su eliminación
Puede aprobar una actualización para su eliminación (es decir, para desinstalar una actualización ya instalado). Esta opción sólo está disponible si la actualización ya está instalada y admite la eliminación. Puede especificar una fecha límite para que se puede desinstalar la actualización, o especificar una fecha pasada para la fecha límite si desea quitar la actualización inmediatamente (la próxima vez que los equipos cliente, póngase en contacto con el servidor WSUS).

Es importante mencionar que no todas las actualizaciones admiten la eliminación. Puede ver si una actualización admite la eliminación, seleccione una actualización individual y examinando el **detalles** panel. En **detalles adicionales**, verá el **extraíble** categoría. Si no se puede quitar la actualización a través de WSUS, en algunos casos se puede eliminar con **agregar o quitar programas** desde **Panel de Control**.

#### <a name="to-approve-updates-for-removal"></a>Para aprobar las actualizaciones para su eliminación

1.  En la consola de administración de WSUS, haga clic en **actualizaciones** y, a continuación, haga clic en **todas las actualizaciones**.

2.  En la lista de actualizaciones, seleccione una o varias actualizaciones que desea aprobar para su eliminación y haga clic en ellos (o vaya a la **acciones** panel).

3.  En el **aprobar actualizaciones** cuadro de diálogo, seleccione el grupo de equipos del que desea quitar la actualización y haga clic en la flecha situada junto a ella.

4.  Seleccione **aprobado para su eliminación**y, a continuación, haga clic en el **quitar** botón.

5.  Una vez finalizada la aprobación de eliminación, puede seleccionar una fecha límite haciendo clic en la actualización una vez más, seleccionando el grupo de equipos adecuado y, a continuación, haga clic en la flecha situada junto a ella. A continuación, seleccione **fecha límite**. Puede seleccionar uno de los plazos estándares (una semana, dos semanas, un mes), o puede hacer clic **personalizado** para seleccionar una fecha y hora específicas.

6.  Si desea que una actualización que se va a quitar tan pronto como el contacto de los equipos cliente del servidor, haga clic en **personalizado**y establecer una fecha en el pasado.

## <a name="approving-updates-automatically"></a>Aprobar actualizaciones automáticamente
Puede configurar su servidor WSUS para aprobación automática de ciertas actualizaciones. También puede especificar la aprobación automática de revisiones de actualizaciones conforme estén disponibles. Esta opción está seleccionada de forma predeterminada. Una revisión es una versión de una actualización que ha sufrido cambios realizados en él (por ejemplo, puede que haya caducado, o podrían haber cambiado sus reglas de aplicabilidad). Si elige no aprobar automáticamente la versión revisada de una actualización, WSUS usará la versión más antigua y debe aprobar manualmente la revisión de la actualización.

Puede crear reglas que se aplicará automáticamente el servidor WSUS durante la sincronización. Especifique qué actualizaciones desea aprobar automáticamente para la instalación, por clasificación de actualizaciones por producto y por grupo de equipos. Esto se aplica solo a las nuevas actualizaciones, en lugar de actualizaciones revisadas. También puede especificar una fecha de límite de aprobación de actualización, que establece un número de días y una hora específica de la oferta antes de que la actualización aprobada se instala a la fecha límite. Estas opciones están disponibles en el **opciones** panel, en **aprobaciones automáticas**.

#### <a name="to-automatically-approve-updates"></a>Para aprobar automáticamente las actualizaciones

1.  En la consola de administración de WSUS, haga clic en **opciones**y, a continuación, haga clic en **aprobaciones automáticas**.

2.  En **Reglas de actualización**, haga clic en **Nueva regla**.

3.  En el **Agregar regla** cuadro de diálogo, en **paso 1: seleccionar propiedades**, seleccione si desea usar **cuando una actualización está en una clasificación específica** o **cuando una actualización está en un producto específico** (o ambos) como criterio. Si lo desea, seleccione si desea **establecer una fecha límite** para la aprobación.

4.  En **paso 2: editar las propiedades** haga clic en las propiedades del subrayado para seleccionar las clasificaciones, los productos y los grupos de equipos que se desean aprobaciones automáticas, según corresponda. Si lo desea, elija la aprobación de actualización de la fecha límite día y hora.

5.  En **paso 3: Especifique un cuadro de nombre**, escriba un nombre único para la regla.

6.  Haga clic en **Aceptar**.

Reglas de aprobación automática no se aplicarán a las actualizaciones que requieren un contrato de licencia de usuario final (CLUF) que aún no ha sido aceptado en el servidor. Si encuentra que aplicar una regla de aprobación automática no hace que todas las actualizaciones pertinentes que se apruebe, debe aprobar estas actualizaciones manualmente.

## <a name="automatically-approving-revisions-to-updates-and-declining-expired-updates"></a>Aprobar automáticamente las revisiones de actualizaciones y rechazar actualizaciones caducadas
La sección de aprobaciones automáticas del panel de opciones contiene una opción predeterminada para aprobar automáticamente las revisiones de actualizaciones aprobadas. También puede establecer el servidor WSUS para rechazar automáticamente las actualizaciones desfasadas. Si elige no aprobar automáticamente la versión revisada de una actualización, el servidor WSUS utilizará la revisión más antigua y debe aprobar manualmente la revisión de la actualización.

> [!NOTE]
> Una revisión es una versión de una actualización que ha cambiado (por ejemplo, puede que haya caducado o haya actualizado las reglas de aplicabilidad).

#### <a name="to-automatically-approve-revisions-to-updates-and-decline-expired-updates"></a>Para aprobar las revisiones de las actualizaciones automáticamente y rechazar actualizaciones caducadas

1.  En la consola de administración de WSUS, haga clic en **opciones**y, a continuación, haga clic en **aprobaciones automáticas**.

2.  En el **avanzadas** pestaña, asegúrese de que ambos **aprobar automáticamente las nuevas revisiones de actualizaciones aprobadas** y **rechaza automáticamente actualizaciones cuando da lugar a una nueva revisión que expiren** están seleccionadas.

3.  Haga clic en Aceptar.

    > [!NOTE]
    > Mantener los valores predeterminados de estas opciones permite que mantener un buen rendimiento en la red WSUS. Si no desea que las actualizaciones desfasadas se rechaza automáticamente, debe Asegúrese rechazarlas manualmente de forma periódica.

## <a name="automatically-declining-superseded-updates"></a>Rechazar automáticamente las actualizaciones sustituidas
Cuando aprueba una nueva actualización que reemplaza a una actualización existente que se aprueba automáticamente, la actualización reemplazada se convierte en "No aplicable" en un equipo o dispositivo una vez que se ha instalado la actualización más reciente. En la consola de WSUS, puede comprobar que una actualización no es aplicable para todos los equipos. Una vez que el caso, la actualización puede ser rechazada de forma segura. Además, al ejecutar la limpieza del servidor de WSUS asistente es posible que rechace de automáticamente la actualización.

Para buscar actualizaciones reemplazadas, puede seleccionar la columna de marca "Reemplazado" en la vista de todas las actualizaciones y la ordenación en esa columna. Habrá cuatro grupos:

-   Las actualizaciones que nunca han sido reemplazadas (un icono en blanco).

-   Las actualizaciones que han sido reemplazadas, pero nunca han habían reemplazado a otra actualización (un icono con un cuadrado azul en la parte inferior).

-   Actualizaciones que han sido sustituidas y han sustituido otra actualización (un icono con un cuadrado azul en la parte central).

-   Actualizaciones que se reemplazó otra actualización (un icono con un cuadrado azul en la parte superior).

No hay ninguna característica en Windows Server Update Services que rechazos sustituir automáticamente las actualizaciones tras la aprobación de una actualización más reciente. Se recomienda establecer primero la aprobación a "No aprobado" y, a continuación, use el Asistente para la limpieza del servidor para rechaza automáticamente la actualización se han cumplido todas las condiciones pertinentes. Para obtener más información, vea: [El Asistente para la limpieza del servidor](the-server-cleanup-wizard.md).

## <a name="approving-superseding-or-superseded-updates"></a>Aprobación de actualizaciones reemplaza o reemplazadas
Normalmente, una actualización que reemplaza otras actualizaciones realiza una o varias de las siguientes acciones:

-   Mejora, mejora o agrega a la reparación proporcionada por una o varias de las actualizaciones publicadas anteriormente.

-   Mejora la eficacia de su paquete de archivos de actualización que se instala en los equipos cliente si se aprueba la actualización para la instalación. Por ejemplo, la actualización reemplazada puede contener archivos que ya no son relevantes para la revisión o para los sistemas operativos admitidos ahora la nueva actualización, por lo que esos archivos no se incluyen en el paquete de archivos de actualización de reemplazo.

-   Actualiza las versiones más recientes de los sistemas operativos. También es importante tener en cuenta que la actualización de reemplazo no podría admitir versiones anteriores de sistemas operativos.

Por el contrario, una actualización que es reemplazada por otra actualización realiza lo siguiente:

-   Corrige un problema similar de la actualización que la sustituye. Sin embargo, la actualización que la sustituye puede mejorar la corrección que proporciona la actualización reemplazada.

-   Actualiza las versiones anteriores de sistemas operativos. En algunos casos, estas versiones de sistemas operativos ya no son actualizadas por la actualización de reemplazo.

En el panel de detalles de una actualización individual, un icono de información y un mensaje en la parte superior indica reemplaza o se ha reemplazado por otra actualización. Además, puede determinar qué actualizaciones reemplazan ni sustituye la actualización examinando el **actualizaciones que reemplazan esta actualización** y **actualizaciones reemplazadas por esta actualización** entradas en el **detalles adicionales** sección de la **propiedades**. Panel de detalles de la actualización se muestra debajo de la lista de actualizaciones.

Automáticamente, WSUS no rechaza las actualizaciones reemplazadas y se recomienda que no asuma que las actualizaciones reemplazadas se deben rechazar en favor de la nueva actualización. Antes de rechazar una actualización reemplazada, asegúrese de que ya no se necesita alguno de los equipos cliente. Los siguientes son ejemplos de escenarios en los que es posible que deba instalar una actualización reemplazada:

-   Si una actualización de reemplazo es compatible con versiones más recientes solo de un sistema operativo y algunos de los equipos cliente ejecutan versiones anteriores del sistema operativo.

-   Si una actualización de reemplazo se aplicabilidad más restringida que la actualización que reemplaza, lo que la haría inapropiada para algunos equipos cliente.

-   Si una actualización ya no reemplaza una actualización lanzada anteriormente debido a los cambios nuevo. Es posible que a través de los cambios en cada versión, una actualización ya no Reemplace otra actualización que antes reemplazó en una versión anterior. En este escenario, aún verá un mensaje sobre la actualización reemplazada, aunque la actualización que reemplaza a se ha reemplazado por una actualización que no lo hace.


