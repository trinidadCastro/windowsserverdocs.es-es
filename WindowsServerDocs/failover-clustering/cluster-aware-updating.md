---
title: "Introducción a actualizar clústeres"
ms.topic: article
ms.prod: windows-server-threshold
ms.manager: dongill
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 3/23/2017
description: "Actualizar clústeres (PRU) automatiza la instalación de actualización de software en los clústeres que ejecutan Windows Server."
ms.assetid: 3c2993b4-aa81-452b-a5c3-3724ad95d892
ms.openlocfilehash: 68336741accabd6e4cb0da5dbd708be707f68df6
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="cluster-aware-updating-overview"></a>Introducción a actualizar clústeres

> Se aplica a: Windows Server (punto y anual canal), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tema proporciona una visión general de \(CAU\) actualizar Cluster\ cuenta, una característica que automatiza el software de actualización proceso en los servidores agrupados manteniendo la disponibilidad.

> [!NOTE]
> Al actualizar [directa de los espacios de almacenamiento](../storage/storage-spaces/storage-spaces-direct-overview.md) clústeres, te recomendamos que uses una actualización compatible con clústeres.
  
## <a name="BKMK_OVER"></a>Descripción de la característica  
Actualizar clústeres es una característica automatizada que le permite actualizar servidores en una [clúster de conmutación por error](failover-clustering-overview.md) con poca o ninguna pérdida de disponibilidad durante el proceso de actualización. Durante una ejecutar actualizar, actualizar clústeres transparente realiza las siguientes tareas:  

1. Cada nodo del clúster se pone en modo de mantenimiento del nodo.
2. Mueve las funciones clústeres del nodo.
3. Instala las actualizaciones y las actualizaciones dependientes.
4. Realiza un reinicio si es necesario.
5. Lleva el nodo del modo de mantenimiento.
6. Restaura las funciones en el nodo del clústeres.
7. Se mueve actualizar el nodo siguiente.

Para muchas funciones clústeres del clúster, el proceso de actualización automática desencadena una conmutación por error planeada. Esto puede provocar una interrupción del servicio transitorios para los clientes conectados. Sin embargo, en el caso de las cargas de trabajo continuamente disponibles, como Hyper\-V con la migración en vivo o el servidor de archivos con SMB de conmutación por error transparente, la actualización de clústeres pueden coordinar las actualizaciones de clúster sin que afecte a la disponibilidad de servicio.    
  
## <a name="practical-applications"></a>Aplicaciones prácticas  
  
-   PRU reduce las interrupciones del servicio en los servicios de clústeres, reduce la necesidad de soluciones alternativas de actualización manual y hace que el clúster end\-to\-end más confiable de proceso de actualización para el administrador. Cuando se usa la característica PRU junto con cargas de trabajo de clúster continuamente disponibles, como los servidores de archivo disponibles continuamente \ (archivo carga de trabajo con SMB transparente Failover\) o Hyper\-V, el clúster que se pueden realizar actualizaciones sin afectar a la disponibilidad de servicio para los clientes.  
  
-   PRU facilita la adopción de procesos de TI coherentes en toda la empresa. Actualizar los perfiles de ejecución puede crearse en las distintas clases de clústeres de conmutación por error y, a continuación, administra de forma centralizada en un recurso compartido de archivos para asegurarte de que las implementaciones de PRU en toda la organización de TI aplican actualizaciones de forma coherente, incluso si se administran los clústeres diferentes lines\-descripción\-business o los administradores.  
  
-   PRU puede programar la actualización se ejecuta en intervalos regulares de mensuales, diarias o semanales ayudar a coordinar las actualizaciones del clúster con otros procesos de administración de TI.  
  
-   PRU proporciona una arquitectura extensible para actualizar el inventario de software de clúster de forma cluster\ cuenta. Esto puede usarse por los editores para coordinar la instalación de actualizaciones de software que no se publican en Windows Update o Microsoft Update o que no están disponibles de Microsoft, por ejemplo, las actualizaciones de controladores de dispositivo non\ de Microsoft.  
  
-   El modo de actualización self\ PRU permite a un dispositivo "clúster en un cuadro de" \ (un conjunto de clústeres equipos físicos, normalmente empaquetado en una chassis\) actualizarse. Por lo general, estos dispositivos se implementan en las sucursales con soporte mínimo de TI local para administrar los clústeres. Modo de actualización Self\ ofrece un gran valor en estos escenarios de implementación.  
  
## <a name="important-functionality"></a>Funcionalidad importante  
La siguiente es una descripción de la funcionalidad de clústeres de actualización importante:  
  
-   Una interfaz de usuario \(UI\) - la ventana de la actualización con reconocimiento de clúster - y un conjunto de cmdlets que se puede usar para obtener una vista previa, aplicar, supervisar y notificar sobre las actualizaciones  
  
-   Una automatización end\-to\-end de la operación de actualización cluster\ \ (una actualización Run\), organizados por uno o más equipos del Coordinador de actualización  
  
-   Un valor predeterminado plug\ en que se integra con la infraestructura de Windows Server Update Services \(WSUS\) en Windows Server y existente \(WUA\) agente de Windows Update para aplicar las actualizaciones importantes de Microsoft  
  
-   Una segunda plug\ en que puede usarse para aplicar revisiones de Microsoft, y que se puede personalizar para aplicar actualizaciones non\ de Microsoft  
  
-   Actualizar perfiles de ejecutar que configures con la configuración de opciones de actualización a ejecutar, como el número máximo de veces que se volverá a la actualización de cada nodo. Actualizar ejecutar perfiles te permiten rápidamente volver a usar la misma configuración durante las ejecuciones de actualización y compartir fácilmente la configuración de actualización con otros clústeres de conmutación por error.  
  
-   Arquitectura de lenguaje que admita el nuevo plug\ de desarrollo para coordinar otras herramientas de actualización node\ en el clúster, como los instaladores de software personalizado, herramientas, la actualización de BIOS y adaptador o host de bus adaptador \(HBA\) herramientas de actualización de red.  
  
Actualizar clústeres pueden coordinar el clúster completado la operación de dos modos de actualización:  
  
-   **Modo de actualización Self\** para este modo, se configura el rol clúster PRU como una carga de trabajo en el clúster de conmutación por error que tiene que actualizarse, y se define un programa de asociados de actualización. El clúster se actualiza a horas programadas mediante el uso de un perfil personalizado actualizar ejecutar o predeterminado. Durante la actualización de ejecución, inicia el proceso de PRU Coordinador de actualización en el nodo que posee actualmente el rol clúster PRU y el proceso de forma secuencial realiza actualizaciones en cada nodo del clúster. Para actualizar el nodo del clúster actual, el rol clúster PRU conmuta por error a otro nodo del clúster, y un nuevo proceso de actualización coordinador en ese nodo asume el control de la actualización de ejecución. En el modo de actualización self\ PRU puede actualizar el clúster de conmutación por error mediante un proceso totalmente automatizado de actualización end\-to\-end. Un administrador puede también desencadenador actualiza on\ demanda en este modo, o simplemente utilizar la actualización remote\ abordar si lo deseas. En el modo de actualización self\, un administrador puede obtener información resumen sobre ejecutar una actualización en curso conecten al clúster y ejecutando la **Get\ CauRun** cmdlet de Windows PowerShell.  
  
-   **Modo de actualización Remote\** de este modo, se configura un equipo remoto, que se llama a un coordinador de actualización, con las herramientas de PRU. El Coordinador de actualización no es un miembro del clúster que se actualiza durante la ejecución de la actualización. Desde el equipo remoto, el administrador desencadena una petición de on\ actualizar ejecutar usando un perfil personalizado actualizar ejecutar o predeterminado. Modo de actualización Remote\ es útil para supervisar el progreso de tiempo real\ durante la ejecución de la actualización y para clústeres que se ejecutan en las instalaciones de Server Core.  
  
## <a name="hardware-and-software-requirements"></a>Requisitos de hardware y software  

PRU puede usarse en todas las ediciones de Windows Server, incluidas las instalaciones de Server Core. Para obtener información de requisitos detallados, consulta [actualizar clústeres requisitos y recomendaciones](cluster-aware-updating-requirements.md).

### <a name="installing-cluster-aware-updating"></a>Instalar una actualización compatible con clústeres
Para usar PRU, instala la característica clúster de conmutación por error en Windows Server y crear un clúster de conmutación por error. Los componentes que admiten funcionalidad PRU se instalan automáticamente en cada nodo del clúster.

Para instalar la característica clúster de conmutación por error, puedes usar las siguientes herramientas:
- Agregar Roles and Features Wizard en el administrador del servidor
- [Install-WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/server-manager/install-windowsfeature) cmdlet de Windows PowerShell
- Herramienta de línea de comandos de mantenimiento de imágenes de implementación y administración (DISM)

Para obtener más información, consulta [instalar o desinstalar Roles, los servicios de rol o características](https://technet.microsoft.com/library/hh831809(v=ws.11).aspx).

También debes instalar las herramientas de clústeres de conmutación por error, que forman parte de las herramientas de administración de servidor remoto y se instalan de forma predeterminada cuando se instala la característica clúster de conmutación por error en el administrador del servidor. Las herramientas de clústeres de conmutación por error incluyen la interfaz de usuario de una actualización compatible con clústeres y los cmdlets de PowerShell. 

Debes instalar las herramientas de clústeres de conmutación por error siguiente para admitir los diferentes modos de actualización de PRU:

- Para usar PRU en modo de actualización self\, instala las herramientas de clústeres de conmutación por error en cada nodo del clúster.   
  
- Para habilitar el modo de actualización remote\, instala las herramientas de clústeres de conmutación por error en un equipo que tiene conectividad de red al clúster de conmutación por error.  
  
> [!NOTE]  
> -   No puedes usar las herramientas de clústeres de conmutación por error en Windows Server 2012 para administrar una actualización compatible con clústeres en una versión más reciente de Windows Server. 
> -   Para usar PRU solo en modo de actualización remote\, no es necesario instalar las herramientas de clústeres de conmutación por error en los nodos del clúster. Sin embargo, algunas características PRU no estará disponibles. Para obtener más información, consulta [requisitos y los procedimientos recomendados para actualizar el reconocimiento de Cluster\](cluster-aware-updating-requirements.md).  
> -   A menos que estás usando PRU solo en modo de actualización self\, el equipo en el que se instalan las herramientas de PRU y que coordina las actualizaciones no puede ser un miembro del clúster de conmutación por error.  
  
### <a name="enabling-self-updating-mode"></a>Habilitar el modo de actualización automática
Para habilitar el modo de actualización automática, debes agregar el rol clúster clústeres actualizar al clúster de conmutación por error. Para ello, usa uno de los siguientes métodos:
- En el administrador del servidor, selecciona **herramientas** > **actualizar clústeres**, a continuación, en la ventana actualizar clústeres, seleccione **clúster configurar opciones de actualización automática**. 
- En una sesión de PowerShell, ejecuta el [agregar CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/add-cauclusterrole) cmdlet.  
  
Desinstalar PRU, desinstalar la característica de clústeres de conmutación por error o herramientas de clúster de conmutación por error mediante el administrador del servidor, el [desinstalar-WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/server-manager/uninstall-windowsfeature) cmdlet, o las herramientas de línea de Web\ DISM.  
  
### <a name="additional-requirements-and-best-practices"></a>Requisitos adicionales y procedimientos recomendados  

Para garantizar que PRU puede actualizar los nodos del clúster correctamente, y para obtener instrucciones adicionales para configurar el entorno de clúster de conmutación por error para usar PRU, puedes ejecutar el analizador de procedimientos recomendados de PRU.  
  
Para requisitos detallados y procedimientos recomendados para usar PRU e información acerca de cómo ejecutar el analizador de procedimientos recomendados de PRU, consulta [requisitos y los procedimientos recomendados para actualizar el reconocimiento de Cluster\](cluster-aware-updating-requirements.md).  
  
### <a name="starting-cluster-aware-updating"></a>A partir de una actualización compatible con clústeres  
  
##### <a name="to-start-cluster-aware-updating-from-server-manager"></a>Para comenzar a actualizar clústeres desde el administrador del servidor  
  
1.  Inicia el administrador del servidor.  
  
2.  Realiza una de las siguientes acciones:  
  
    -   En la **herramientas** menú, haz clic en **actualizar Cluster\ cuenta**.  
  
    -   Si uno o varios nodos del clúster o el clúster, se agrega al administrador del servidor, en la **todos los servidores** página right\ y haga clic en el nombre de un nodo \ (o el nombre de la cluster\) y, a continuación, haz clic en **actualización clúster**.  
  
## <a name="see-also"></a>Consulta también  
Los siguientes vínculos proporcionan más información sobre el uso de clústeres de actualización.  
  
-   [Requisitos y los procedimientos recomendados para actualizar Cluster\ cuenta](cluster-aware-updating.md)  
  
-   [Actualizar con reconocimiento de Cluster\: Preguntas más frecuentes](cluster-aware-updating-faq.md)  
  
-   [Opciones avanzadas y actualización de perfiles de ejecución para PRU](cluster-aware-updating-options.md)  
  
-   [Cómo funcionan los complementos Plug\ PRU](cluster-aware-updating-plug-ins.md)  
  
-   [Cmdlets de actualización Cluster\ compatibles en Windows PowerShell](https://go.microsoft.com/fwlink/p/?LinkId=237675)  
  
-   [Reconocimiento de Cluster\ actualizar Plug\ de referencia](https://msdn.microsoft.com/library/hh418084.aspx)  
  

