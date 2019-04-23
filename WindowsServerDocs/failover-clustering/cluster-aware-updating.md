---
title: Información general de la Actualización compatible con clústeres
ms.topic: article
ms.prod: windows-server-threshold
ms.manager: dongill
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 08/06/2018
description: Actualización compatible con clústeres (CAU) automatiza la instalación de actualización de software en los clústeres que ejecutan Windows Server.
ms.assetid: 3c2993b4-aa81-452b-a5c3-3724ad95d892
ms.openlocfilehash: 77ccfe7dc9a769d602ff069d5f1d8e77522aa001
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827426"
---
# <a name="cluster-aware-updating-overview"></a>Información general de la Actualización compatible con clústeres

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema proporciona información general sobre clúster\-actualización compatible con \(CAU\), una característica que automatiza el proceso de actualización de software de servidores de clúster manteniendo la disponibilidad.

> [!NOTE]
> Al actualizar [espacios de almacenamiento directo](../storage/storage-spaces/storage-spaces-direct-overview.md) clústeres, se recomienda usar la actualización compatible con clústeres.
  
## <a name="BKMK_OVER"></a>Descripción de la característica  
Actualización de clústeres es una característica automatizada que permite actualizar servidores en un [clúster de conmutación por error](failover-clustering-overview.md) con poca o ninguna pérdida de disponibilidad durante el proceso de actualización. Durante la ejecución de actualización, actualización compatible con clúster de forma transparente realiza las siguientes tareas:  

1. Cada nodo del clúster se coloca en modo de mantenimiento de nodo.
2. Mueve los roles en clúster fuera del nodo.
3. Instala las actualizaciones y otras actualizaciones dependientes.
4. Realiza un reinicio si es necesario.
5. Saca el nodo del modo de mantenimiento.
6. Restaura los roles en el nodo de clúster.
7. Se desplaza al actualizar el siguiente nodo.

En muchos roles en clúster de un clúster, el proceso de actualización automático desencadena una conmutación por error planeada, lo que puede provocar una interrupción transitoria del servicio para los clientes conectados. Sin embargo, en el caso de cargas de trabajo disponibles continuamente, como Hyper\-V con el servidor de archivo o la migración en vivo con SMB conmutación por error transparente, la actualización compatible con clúster puede coordinar las actualizaciones de clúster sin que ello afecte a la disponibilidad del servicio.    
  
## <a name="practical-applications"></a>Aplicaciones prácticas  
  
-   CAU reduce las interrupciones de servicio en servicios en clúster, reduce la necesidad de soluciones de actualización manual y hace que el extremo\-a\-más confiable del proceso de actualización para el Administrador de clúster de fondo. Cuando esta característica se usa junto con cargas de trabajo de clúster disponibles continuamente, como servidores de archivos disponibles continuamente \(carga de trabajo de servidor de archivos con conmutación por error transparente de SMB\) o Hyper\-V, el clúster actualizaciones se pueden realizar sin impacto alguno en disponibilidad del servicio para los clientes.  
  
-   La CAU facilita la adopción de procesos de TI coherentes en toda la empresa. Actualizar perfiles de ejecución se puede crear para distintas clases de clústeres de conmutación por error y, a continuación, se administran centralmente en un recurso compartido de archivos para asegurarse de que las implementaciones de CAU en toda la organización de TI apliquen actualizaciones de forma coherente, incluso si los clústeres se administran por diferentes líneas \-de\-empresariales o administradores.  
  
-   La CAU puede programar las ejecuciones de actualización en intervalos diarios, semanales o mensuales regulares para ayudar a coordinar las actualizaciones de clústeres con otros procesos de administración de TI.  
  
-   CAU proporciona una arquitectura extensible para actualizar el inventario de software de clúster en un clúster\-manera compatible con clústeres. Esto puede usarse por los publicadores para coordinar la instalación de actualizaciones de software que no están publicadas en Windows Update o Microsoft Update o que no están disponibles en Microsoft, por ejemplo, las actualizaciones para que no sean\-controladores de dispositivos de Microsoft.  
  
-   CAU self\-modo de actualización permite que un dispositivo de "clúster en una" \(un conjunto de equipos físicos en clúster, que suelen ir empaquetados en un chasis\) actualizarse automáticamente. Generalmente, estas aplicaciones se implementan en sucursales con soporte técnico de TI local mínimo para administrar los clústeres. Self\-modo de actualización resulta muy valioso en estos escenarios de implementación.  
  
## <a name="important-functionality"></a>Funcionalidad importante  
La siguiente es una descripción de funcionalidad importante actualización de clústeres:  
  
-   Una interfaz de usuario \(UI\) : la ventana de actualización compatible con clústeres - y un conjunto de cmdlets que puede usar para obtener una vista previa, aplicarlas, supervisarlas e informar sobre las actualizaciones  
  
-   Un extremo de\-a\-terminar la automatización del clúster\-operación de actualización \(una ejecución de actualización\), organizada por uno o varios equipos coordinadores de actualizaciones  
  
-   Un enchufe predeterminada\-en el que se integra con el agente de actualización de Windows existente \(WUA\) y Windows Server Update Services \(WSUS\) infraestructura en Windows Server para aplicar importante Microsoft actualizaciones  
  
-   Un segundo conector\-en el que puede utilizarse para aplicar revisiones de Microsoft y que se pueden personalizar para aplicar no\-las actualizaciones de Microsoft  
  
-   Perfiles de ejecución de actualización que configura con ajustes de opciones de ejecución de actualización, como el número máximo de veces que la actualización se reintentará por nodo. Los perfiles de ejecución de actualización permiten reutilizar rápidamente la misma configuración entre ejecuciones de actualización y compartir fácilmente la configuración de actualización con otros clústeres de conmutación por error.  
  
-   Una arquitectura extensible que admite nuevas plug\-en el desarrollo para coordinar el otro nodo\-actualizando herramientas en el clúster, como instaladores de software personalizados, herramientas y el adaptador de red o el adaptador de bus host laactualizacióndelBIOS\( HBA\) herramientas de actualización.  
  
Actualización de clústeres puede coordinar toda la operación en dos modos de actualización del clúster:  
  
-   **Self\-modo de actualización** para este modo, el rol en clúster de CAU se configura como una carga de trabajo en el clúster de conmutación por error que debe actualizarse y se define una programación de actualización correspondiente. El clúster se actualiza automáticamente a horas programadas mediante un perfil de ejecución de actualización personalizado o predeterminado. Durante la ejecución de actualización, el proceso de coordinador de actualizaciones de la CAU se inicia en el nodo que actualmente es propietario del rol en clúster de la CAU y el proceso realiza actualizaciones secuencialmente en cada nodo de clúster. Para actualizar el nodo de clúster actual, el rol en clúster de la CAU realiza una conmutación por error a otro nodo de clúster y un nuevo proceso de coordinador de actualizaciones en dicho nodo asume el control de la ejecución de actualización. En el propio\-modo de actualización, CAU puede actualizar el clúster de conmutación por error mediante un completamente automatizada, terminar\-a\-final al proceso de actualización. Un administrador también puede desencadenar actualizaciones en\-exigen en este modo, o simplemente utilizar el control remoto\-actualizar enfoque si lo desea. En el propio\-modo de actualización, un administrador puede obtener información de resumen sobre una ejecución de actualización en curso al conectarse al clúster y ejecutar el **obtener\-CauRun** cmdlet de Windows PowerShell.  
  
-   **Remoto\-modo de actualización** para este modo, se configura un equipo remoto, que se llama a un coordinador de actualizaciones, con las herramientas de CAU. El coordinador de actualizaciones no es miembro del clúster que se actualiza durante la ejecución de actualización. Desde el equipo remoto, el administrador desencadena una en\-exigen la ejecución de actualización mediante un perfil de ejecución de actualización personalizado o predeterminado. Remoto\-modo de actualización es útil para supervisar real\-progreso del tiempo durante la ejecución de actualización y para los clústeres que se ejecutan en instalaciones Server Core.  
  
## <a name="hardware-and-software-requirements"></a>Requisitos de hardware y software  

CAU se puede usar en todas las ediciones de Windows Server, incluidas las instalaciones Server Core. Para obtener información detallada de los requisitos, consulte [Cluster-Aware Updating requisitos y procedimientos recomendados](cluster-aware-updating-requirements.md).

### <a name="installing-cluster-aware-updating"></a>Instalar la actualización de clústeres
Para usar la CAU, instale la característica clúster de conmutación por error en Windows Server y crear un clúster de conmutación por error. Los componentes que admiten funciones de la CAU se instalan automáticamente en cada nodo de clúster.

Para instalar una característica Clúster de conmutación por error, puede usar las siguientes herramientas:
- Asistente para agregar roles y características en el Administrador del servidor
- [Install-WindowsFeature](https://docs.microsoft.com/powershell/module/servermanager/Install-WindowsFeature?view=winserver2012r2-ps&viewFallbackFrom=win10-ps) cmdlet de Windows PowerShell
- Herramienta de línea de comandos Administración y mantenimiento de imágenes de implementación (DISM)

Para obtener más información, consulte [instalar la característica clúster de conmutación por error](create-failover-cluster.md#install-the-failover-clustering-feature).

También debe instalar las herramientas de agrupación en clústeres de conmutación por error, que forman parte de herramientas de administración remota del servidor y se instalan de forma predeterminada al instalar la característica clúster de conmutación por error en el administrador del servidor. Las herramientas de agrupación en clústeres de conmutación por error incluyen la interfaz de usuario de actualización de clústeres y los cmdlets de PowerShell. 

Debe instalar las Herramientas de clústeres de conmutación por error como se indica a continuación para admitir los diferentes modos de actualización de CAU:

- Para usar CAU en self\-actualizando, modo de instalar las herramientas de clústeres de conmutación por error en cada nodo del clúster.   
  
- Para habilitar remoto\-actualizando, modo de instalar las herramientas de clústeres de conmutación por error en un equipo que tenga conectividad de red con el clúster de conmutación por error.  
  
> [!NOTE]  
> -   No se puede usar las herramientas de clústeres de conmutación por error en Windows Server 2012 para administrar la actualización de clústeres en una versión más reciente de Windows Server. 
> -   Para usar la CAU solo en remoto\-modo de actualización, instalación de las herramientas de agrupación en clústeres de conmutación por error en los nodos del clúster no es necesario. si bien algunas características de CAU no estarán disponibles. Para obtener más información, consulte [requisitos y procedimientos recomendados para el clúster\-actualización compatible con](cluster-aware-updating-requirements.md).  
> -   A menos que esté usando la CAU solo en self\-modo de actualización, el equipo en el que se instalan las herramientas de CAU y que coordina las actualizaciones no puede ser un miembro del clúster de conmutación por error.  
  
### <a name="enabling-self-updating-mode"></a>Habilitar el modo de auto-actualización
Para habilitar el modo de auto-actualización, debe agregar el rol en clúster Cluster-Aware Updating al clúster de conmutación por error. Para ello, use uno de los métodos siguientes:
- En el administrador del servidor, seleccione **herramientas** > **Cluster-Aware Updating**, a continuación, en la ventana de actualización de clústeres, seleccione **clúster configurardelasopcionesdeauto-actualización**. 
- En una sesión de PowerShell, ejecute el [Add-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Add-CauClusterRole?view=win10-ps) cmdlet.  
  
Para desinstalar la CAU, desinstale la característica clúster de conmutación por error o herramientas de agrupación en clústeres de conmutación por error mediante el administrador del servidor, el [Uninstall-WindowsFeature](https://docs.microsoft.com/powershell/module/servermanager/Uninstall-WindowsFeature?view=win10-ps) cmdlet, o el comando DISM\-herramientas de línea.  
  
### <a name="additional-requirements-and-best-practices"></a>Otros requisitos y procedimientos recomendados  

Puede ejecutar el Analizador de procedimientos recomendados de la CAU para asegurarse de que la CAU puede actualizar correctamente los nodos de clúster, así como para obtener más instrucciones sobre cómo configurar el entorno de clúster de conmutación por error para que use la CAU.  
  
Para conocer los requisitos detallados y procedimientos recomendados para usar CAU así como información sobre cómo ejecutar la herramienta Best Practices Analyzer de CAU, consulte [requisitos y procedimientos recomendados para el clúster\-actualización compatible con](cluster-aware-updating-requirements.md).  
  
### <a name="starting-cluster-aware-updating"></a>Iniciando la actualización compatible con clústeres  
  
##### <a name="to-start-cluster-aware-updating-from-server-manager"></a>Para iniciar la actualización compatible con clústeres de administrador del servidor  
  
1.  Inicie el Administrador del servidor.  
  
2.  Realiza una de las siguientes acciones:  
  
    -   En el **herramientas** menú, haga clic en **clúster\-actualización compatible con**.  
  
    -   Si uno o más nodos del clúster o el clúster, se agrega al administrador del servidor, en el **todos los servidores** página derecha\-haga clic en el nombre de un nodo \(o el nombre del clúster\)y, a continuación, haga clic en  **Actualizar clúster**.  
  
## <a name="see-also"></a>Vea también  
Los siguientes vínculos proporcionan más información sobre el uso de actualización de clústeres.  
  
-   [Requisitos y procedimientos recomendados para el clúster\-actualización compatible con](cluster-aware-updating.md)  
  
-   [Clúster\-actualización compatible con: Preguntas más frecuentes](cluster-aware-updating-faq.md)  
  
-   [Opciones avanzadas y perfiles de ejecución actualización para CAU](cluster-aware-updating-options.md)  
  
-   [Cómo conectar CAU\-trabajo ins](cluster-aware-updating-plug-ins.md)  
  
-   [Clúster\-Cmdlets de Windows PowerShell de actualización compatible con](https://docs.microsoft.com/powershell/module/clusterawareupdating/?view=win10-ps&viewFallbackFrom=winserverr2-ps)  
  
-   [Clúster\-Plug la actualización compatible con\-de referencia](https://docs.microsoft.com/previous-versions/windows/desktop/mscs/cluster-aware-update-plug-in-interfaces-and-classes)  
  

