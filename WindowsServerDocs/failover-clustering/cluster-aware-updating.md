---
title: Información general de la Actualización compatible con clústeres
description: La actualización compatible con clústeres (CAU) automatiza la instalación de actualizaciones de software en clústeres que ejecutan Windows Server.
ms.topic: article
manager: lizross
author: JasonGerend
ms.author: jgerend
ms.date: 08/06/2018
ms.assetid: 3c2993b4-aa81-452b-a5c3-3724ad95d892
ms.openlocfilehash: defec4b34dfd0e2efa5724e499330c79b993bb60
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87990810"
---
# <a name="cluster-aware-updating-overview"></a>Información general de la Actualización compatible con clústeres

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se proporciona información general sobre \- la Cau de actualización compatible con clústeres \( \) , una característica que automatiza el proceso de actualización de software en servidores en clúster manteniendo la disponibilidad.

> [!NOTE]
> Al actualizar clústeres de [espacios de almacenamiento directo](../storage/storage-spaces/storage-spaces-direct-overview.md) , se recomienda usar la actualización compatible con clústeres.

## <a name="feature-description"></a><a name="BKMK_OVER"></a>Descripción de la característica
La actualización compatible con clústeres es una característica automatizada que permite actualizar servidores en un [clúster de conmutación por error](failover-clustering-overview.md) con poca o ninguna pérdida de disponibilidad durante el proceso de actualización. Durante una ejecución de actualización, la actualización compatible con clústeres realiza de forma transparente las siguientes tareas:

1. Coloca cada nodo del clúster en modo de mantenimiento de nodo.
2. Mueve los roles en clúster fuera del nodo.
3. Instala las actualizaciones y las actualizaciones dependientes.
4. Realiza un reinicio si es necesario.
5. Saca el nodo del modo de mantenimiento.
6. Restaura los roles en clúster en el nodo.
7. Se desplaza para actualizar el siguiente nodo.

En muchos roles en clúster de un clúster, el proceso de actualización automático desencadena una conmutación por error planeada, lo que puede provocar una interrupción transitoria del servicio para los clientes conectados. Sin embargo, en el caso de las cargas de trabajo disponibles continuamente, como Hyper \- V con migración en vivo o servidor de archivos con conmutación por error transparente de SMB, la actualización compatible con clústeres puede coordinar las actualizaciones de clúster sin afectar a la disponibilidad del servicio.

## <a name="practical-applications"></a>Aplicaciones prácticas

-   La CAU reduce las interrupciones del servicio en los servicios en clúster, reduce la necesidad de soluciones de actualización manual y hace que el \- \- proceso de actualización de clústeres de un extremo a otro sea más confiable para el administrador. Cuando la característica CAU se usa junto con cargas de trabajo de clúster disponibles continuamente, como la carga de trabajo del servidor de archivos de los servidores de archivos disponibles continuamente \( con conmutación por error transparente \) de SMB o Hyper \- V, las actualizaciones de clúster se pueden realizar sin impacto alguno en la disponibilidad del servicio para los clientes.

-   La CAU facilita la adopción de procesos de TI coherentes en toda la empresa. Los perfiles de ejecución de actualización se pueden crear para diferentes clases de clústeres de conmutación por error y, a continuación, administrarse de forma centralizada en un recurso compartido de archivos para garantizar que las implementaciones de la CAU en toda la organización de ti apliquen actualizaciones de manera coherente, incluso si los clústeres están administrados por diferentes líneas \- de \- negocio o administradores.

-   La CAU puede programar las ejecuciones de actualización en intervalos diarios, semanales o mensuales regulares para ayudar a coordinar las actualizaciones de clústeres con otros procesos de administración de TI.

-   La CAU proporciona una arquitectura extensible para actualizar el inventario de software de clústeres en un modo compatible con clústeres \- . Los publicadores pueden usar esto para coordinar la instalación de actualizaciones de software que no están publicadas en Windows Update o Microsoft Update o que no están disponibles en Microsoft, por ejemplo, actualizaciones de controladores de dispositivos que no son de \- Microsoft.

-   El \- modo de actualización automática de la Cau habilita un \( conjunto de equipos físicos en clúster, normalmente empaquetados en un chasis \) para actualizarse. Generalmente, estas aplicaciones se implementan en sucursales con soporte técnico de TI local mínimo para administrar los clústeres. El \- modo de auto-actualización ofrece un gran valor en estos escenarios de implementación.

## <a name="important-functionality"></a>Funcionalidad importante
A continuación se describe una funcionalidad importante de actualización compatible con clústeres:

-   Una interfaz de usuario de interfaz \( \) de usuario: la ventana de actualización compatible con clústeres: y un conjunto de cmdlets que puede usar para obtener una vista previa, aplicar, supervisar e informar sobre las actualizaciones.

-   Una \- automatización de un extremo a \- otro de la operación de actualización del clúster de \- \( una ejecución de actualización \) , orquestada por uno o varios equipos coordinadores de actualizaciones

-   Un complemento predeterminado \- que se integra con la infraestructura de servidor de Windows Update \( WUA \) y Windows Server Update Services \( WSUS existente \) en Windows Server para aplicar las actualizaciones importantes de Microsoft.

-   Un segundo complemento \- que se puede usar para aplicar revisiones de Microsoft y que se puede personalizar para aplicar actualizaciones que no sean de \- Microsoft.

-   Perfiles de ejecución de actualización que configura con ajustes de opciones de ejecución de actualización, como el número máximo de veces que la actualización se reintentará por nodo. Los perfiles de ejecución de actualización permiten reutilizar rápidamente la misma configuración entre ejecuciones de actualización y compartir fácilmente la configuración de actualización con otros clústeres de conmutación por error.

-   Una arquitectura extensible que admite nuevos complementos \- de desarrollo para coordinar otras herramientas de \- actualización de nodos en el clúster, como instaladores de software personalizados, herramientas de actualización de BIOS y adaptador de red o herramientas de actualización de HBA del adaptador de bus host \( \) .

La Actualización compatible con clústeres puede coordinar toda la operación de actualización de clústeres de dos modos:

-   **Modo de auto- \- actualización** : en este modo, el rol en clúster de la Cau se configura como una carga de trabajo en el clúster de conmutación por error que se va a actualizar y se define una programación de actualización asociada. El clúster se actualiza automáticamente a horas programadas mediante un perfil de ejecución de actualización personalizado o predeterminado. Durante la ejecución de actualización, el proceso de coordinador de actualizaciones de la CAU se inicia en el nodo que actualmente es propietario del rol en clúster de la CAU y el proceso realiza actualizaciones secuencialmente en cada nodo de clúster. Para actualizar el nodo de clúster actual, el rol en clúster de la CAU realiza una conmutación por error a otro nodo de clúster y un nuevo proceso de coordinador de actualizaciones en dicho nodo asume el control de la ejecución de actualización. En \- el modo de auto-actualización, la Cau puede actualizar el clúster de conmutación por error mediante un proceso de actualización de un extremo a otro totalmente automatizado \- \- . Un administrador también puede desencadenar actualizaciones a \- petición en este modo, o simplemente usar el \- enfoque de actualización remota si lo desea. En el modo de auto- \- actualización, un administrador puede obtener información de resumen sobre una ejecución de actualización en curso mediante la conexión al clúster y la ejecución del cmdlet **Get \- CauRun** de Windows PowerShell.

-   **El \- modo de actualización remota** para este modo, un equipo remoto, llamado Coordinador de actualizaciones, se configura con las herramientas de la Cau. El coordinador de actualizaciones no es miembro del clúster que se actualiza durante la ejecución de actualización. Desde el equipo remoto, el administrador desencadena una ejecución de \- actualización a petición mediante un perfil de ejecución de actualización personalizado o predeterminado. \-El modo de actualización remota es útil para supervisar \- el progreso en tiempo real durante la ejecución de actualización y para los clústeres que se ejecutan en instalaciones Server Core.

## <a name="hardware-and-software-requirements"></a>Requisitos de hardware y software

La CAU se puede usar en todas las ediciones de Windows Server, incluidas las instalaciones Server Core. Para obtener información detallada sobre los requisitos, consulte [requisitos y procedimientos recomendados para la actualización compatible con clústeres](cluster-aware-updating-requirements.md).

### <a name="installing-cluster-aware-updating"></a>Instalación de la actualización compatible con clústeres
Para usar la CAU, instale la característica de clústeres de conmutación por error en Windows Server y cree un clúster de conmutación por error. Los componentes que admiten funciones de la CAU se instalan automáticamente en cada nodo de clúster.

Para instalar una característica Clúster de conmutación por error, puede usar las siguientes herramientas:
- Asistente para agregar roles y características en el Administrador del servidor
- [Install-WindowsFeature](/powershell/module/servermanager/Install-WindowsFeature?view=winserver2012r2-ps&viewFallbackFrom=win10-ps)   Cmdlet de Windows PowerShell
- Herramienta de línea de comandos Administración y mantenimiento de imágenes de implementación (DISM)

Para obtener más información, vea [instalar la característica de clústeres de conmutación por error](create-failover-cluster.md#install-the-failover-clustering-feature).

También debe instalar las herramientas de clúster de conmutación por error, que forman parte del Herramientas de administración remota del servidor y se instalan de forma predeterminada al instalar la característica de clústeres de conmutación por error en Administrador del servidor. Las herramientas de clúster de conmutación por error incluyen la interfaz de usuario de actualización compatible con clústeres y los cmdlets de PowerShell.

Debe instalar las Herramientas de clústeres de conmutación por error como se indica a continuación para admitir los diferentes modos de actualización de CAU:

- Para usar la CAU en modo de auto- \- actualización, instale las herramientas de clúster de conmutación por error en cada nodo del clúster.

- Para habilitar \- el modo de actualización remota, instale las herramientas de clúster de conmutación por error en un equipo que tenga conectividad de red con el clúster de conmutación por error.

> [!NOTE]
> -   No puede usar las herramientas de clúster de conmutación por error en Windows Server 2012 para administrar la actualización compatible con clústeres en una versión más reciente de Windows Server.
> -   Para usar la CAU solo en \- el modo de actualización remota, no es necesario instalar las herramientas de clúster de conmutación por error en los nodos del clúster. si bien algunas características de CAU no estarán disponibles. Para obtener más información, consulte [requisitos y procedimientos recomendados para la \- actualización compatible con clústeres](cluster-aware-updating-requirements.md).
> -   A menos que esté usando la CAU solo en modo de auto- \- actualización, el equipo en el que están instaladas las herramientas de la Cau y que coordina las actualizaciones no puede ser miembro del clúster de conmutación por error.

### <a name="enabling-self-updating-mode"></a>Habilitación del modo de auto-actualización
Para habilitar el modo de auto-actualización, debe agregar el rol en clúster de actualización compatible con clústeres al clúster de conmutación por error. Para ello, use uno de los métodos siguientes:
- En Administrador del servidor, seleccione **herramientas**  >  **actualización compatible con clústeres**y, a continuación, en la ventana actualización compatible con clústeres, seleccione **configurar opciones de auto-actualización de clústeres**.
- En una sesión de PowerShell, ejecute el cmdlet [Add-CauClusterRole](/powershell/module/clusterawareupdating/Add-CauClusterRole?view=win10-ps) .

Para desinstalar la CAU, desinstale la característica de clúster de conmutación por error o las herramientas de clúster de conmutación por error mediante Administrador del servidor, el cmdlet [Uninstall-WindowsFeature](/powershell/module/servermanager/Uninstall-WindowsFeature?view=win10-ps) o las herramientas de línea de comandos de DISM \- .

### <a name="additional-requirements-and-best-practices"></a>Otros requisitos y procedimientos recomendados

Puede ejecutar el Analizador de procedimientos recomendados de la CAU para asegurarse de que la CAU puede actualizar correctamente los nodos de clúster, así como para obtener más instrucciones sobre cómo configurar el entorno de clúster de conmutación por error para que use la CAU.

Para conocer los requisitos detallados y los procedimientos recomendados para usar la CAU, así como información sobre cómo ejecutar la Analizador de procedimientos recomendados de CAU, consulte [requisitos y procedimientos recomendados para la \- actualización compatible con clústeres](cluster-aware-updating-requirements.md).

### <a name="starting-cluster-aware-updating"></a>Iniciar la actualización compatible con clústeres

##### <a name="to-start-cluster-aware-updating-from-server-manager"></a>Para iniciar la actualización compatible con clústeres desde Administrador del servidor

1.  Inicie el Administrador del servidor.

2.  Lleve a cabo una de las siguientes acciones:

    -   En el menú **herramientas** , haga clic en ** \- actualización compatible con clústeres**.

    -   Si uno o más nodos de clúster, o el clúster, se agregan a Administrador del servidor, en la página **todos los servidores** , haga clic con el botón secundario \- en el nombre de un nodo \( o en el nombre del clúster \) y, a continuación, haga clic en **Actualizar clúster**.

## <a name="additional-references"></a>Referencias adicionales
Los vínculos siguientes proporcionan más información acerca del uso de la actualización compatible con clústeres.

-   [Requisitos y procedimientos recomendados para la \- actualización compatible con clústeres](cluster-aware-updating.md)

-   [\-Actualización compatible con clústeres: preguntas más frecuentes](cluster-aware-updating-faq.md)

-   [Opciones avanzadas y perfiles de ejecución de actualización para CAU](cluster-aware-updating-options.md)

-   [Cómo funcionan los complementos de CAU \-](cluster-aware-updating-plug-ins.md)

-   [\-Cmdlets de actualización compatible con clústeres en Windows PowerShell](/powershell/module/clusterawareupdating/?view=win10-ps&viewFallbackFrom=winserverr2-ps)

-   [\-Referencia del complemento de actualización compatible con clústeres \-](/previous-versions/windows/desktop/mscs/cluster-aware-update-plug-in-interfaces-and-classes)