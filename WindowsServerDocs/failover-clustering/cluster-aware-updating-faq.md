---
ms.assetid: 6416d125-bcaf-433d-971a-2f0283bca2c2
title: 'Actualización compatible con clústeres: preguntas más frecuentes'
ms.topic: article
ms.prod: windows-server
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 04/28/2017
description: Respuestas a las preguntas más frecuentes sobre la actualización compatible con clústeres en Windows Server.
ms.openlocfilehash: a08366c7e64d9612d63e348d4cecdb4b2389737a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361350"
---
# <a name="cluster-aware-updating-frequently-asked-questions"></a>Actualización compatible con clústeres: Preguntas más frecuentes

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La [actualización compatible con clústeres](cluster-aware-updating.md) \(CAU @ no__t-2 es una característica que coordina las actualizaciones de software en todos los servidores de un clúster de conmutación por error de forma que no afecte a la disponibilidad del servicio más que una conmutación por error planeada de un nodo de clúster. En algunas aplicaciones con características de disponibilidad continua \(such como Hyper @ no__t-1V con migración en vivo, o un servidor de archivos SMB 3. x con conmutación por error transparente de SMB @ no__t-2, la CAU puede coordinar la actualización de clústeres automatizada sin impacto en el servicio disponibilidad.

## <a name="does-cau-support-updating-storage-spaces-direct-clusters"></a>¿Admite la CAU la actualización de clústeres de Espacios de almacenamiento directo?  
Sí. La CAU admite la actualización de clústeres de [espacios de almacenamiento directo](../storage/storage-spaces/storage-spaces-direct-overview.md) independientemente del tipo de implementación: hiperconvergido o convergente. En concreto, la orquestación de CAU garantiza que la suspensión de cada nodo del clúster espera a que el espacio de almacenamiento en clúster subyacente sea correcto.

## <a name="does-cau-work-with-windowsserver-2008r2-or-windows7"></a>¿Funciona la CAU con Windows Server 2008 R2 o Windows 7?  
No. La CAU coordina la operación de actualización de clústeres solo desde equipos que ejecutan Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10, Windows 8.1 o Windows 8. El clúster de conmutación por error que se está actualizando debe ejecutar Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.
  
## <a name="is-cau-limited-to-specific-clustered-applications"></a>¿La CAU está limitada a aplicaciones en clúster específicas?  
No. La CAU es válida para distintos tipos de aplicaciones en clúster. La CAU es una solución de clúster externo @ no__t-0updating que se superpone a las API de agrupación en clústeres y a los cmdlets de PowerShell. Como tal, la CAU puede coordinar la actualización para cualquier aplicación en clúster que esté configurada en un clúster de conmutación por error de Windows Server.  
  
> [!NOTE]  
> Actualmente, se prueban y certifican las siguientes cargas de trabajo en clúster para la CAU: SMB, Hyper @ no__t-0V, Replicación DFS, espacios de nombres DFS, iSCSI y NFS.  
  
## <a name="does-cau-support-updates-from-microsoft-update-and-windows-update"></a>¿Admite la CAU actualizaciones de Microsoft Update y Windows Update?  
Sí. De forma predeterminada, la CAU se configura con un plug @ no__t-0in que usa las API de la utilidad Windows Update Agent \(WUA @ no__t-2 en los nodos del clúster. La infraestructura de WUA puede configurarse para que apunte a Microsoft Update y Windows Update o a Windows Server Update Services \(WSUS @ no__t-1 como su origen de actualizaciones.  
  
## <a name="does-cau-support-wsus-updates"></a>¿Admite la CAU actualizaciones de WSUS?  
Sí. De forma predeterminada, la CAU se configura con un plug @ no__t-0in que usa las API de la utilidad Windows Update Agent \(WUA @ no__t-2 en los nodos del clúster. La infraestructura de WUA puede configurarse para que apunte a Microsoft Update y Windows Update o a un servidor local Windows Server Update Services \(WSUS @ no__t-1 como su origen de actualizaciones.  
  
## <a name="can-cau-apply-limited-distribution-release-updates"></a>¿Puede la CAU aplicar actualizaciones de versiones de distribución limitada?  
Sí. La versión de distribución limitada @no__t las actualizaciones de 0LDR @ no__t-1, también denominadas revisiones, no se publican a través de Microsoft Update o Windows Update, por lo que el agente de Windows Update \(WUA @ no__t-3 Plug @ no__t-4in que usa la CAU de forma predeterminada .  
  
Sin embargo, la CAU incluye un segundo Plug @ no__t-0in que puede seleccionar para aplicar actualizaciones de revisión. Este paquete de revisión @ no__t-0in también se puede personalizar para aplicar actualizaciones de controladores, firmware y BIOS que no sean @ no__t-1Microsoft.  
  
## <a name="can-i-use-cau-to-apply-cumulative-updates"></a>¿Puedo usar la CAU para aplicar actualizaciones acumulativas?  
Sí. Si las actualizaciones acumulativas son actualizaciones de versiones de distribución general o LDR, la CAU las puede aplicar.  
  
## <a name="can-i-schedule-updates"></a>¿Puedo programar actualizaciones?  
Sí. La CAU admite los siguientes modos de actualización (ambos permiten programar las actualizaciones):  
  
**Self @ no__t-1updating** Permite que el clúster se actualice automáticamente en función de un perfil definido y una programación regular, como durante una ventana de mantenimiento mensual. También puede iniciar una ejecución de @ no__t-0Updating a petición en cualquier momento. Para habilitar el modo Self @ no__t-0updating, debe agregar el rol en clúster de la CAU al clúster. La característica de autono__t-0updating de la CAU se comporta como cualquier otra carga de trabajo en clúster y puede funcionar perfectamente con las conmutaciones por error planeadas y no planeadas de un equipo coordinador de actualizaciones.  
  
**Remoto @ no__t-1updating** Permite iniciar una ejecución de actualización en cualquier momento desde un equipo que ejecuta Windows o Windows Server. Puede iniciar una ejecución de actualización a través de la ventana de actualización compatible con clústeres o mediante el cmdlet **Invoke @ no__t-1CauRun de** PowerShell. Remote @ no__t-0updating es el modo de actualización predeterminado de la CAU. Puede usar el Programador de tareas para ejecutar el cmdlet [Invoke-CauRun](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-caurun) en un programa deseado desde un equipo remoto que no sea uno de los nodos del clúster.  
  
## <a name="can-i-schedule-updates-to-apply-during-a-backup"></a>¿Se pueden programar actualizaciones para que se apliquen durante una copia de seguridad?  
Sí. La CAU no impone ninguna restricción en este sentido. Sin embargo, realizar actualizaciones de software en un servidor @no__t 0with los posibles reinicios asociados a @ no__t-1 mientras se está realizando una copia de seguridad del servidor no es un procedimiento recomendado. Tenga en cuenta que la CAU depende solo de las API en clúster para determinar conmutaciones por error y conmutación por recuperación de recursos; de este modo, la CAU ignora el estado de la copia de seguridad del servidor.  
  
## <a name="can-cau-work-with-system-center-configuration-manager"></a>¿Puede la CAU trabajar con System Center Configuration Manager?  
La CAU es una herramienta que coordina las actualizaciones de software en un nodo de clúster y Configuration Manager también realiza actualizaciones de software de servidor. Es importante configurar estas herramientas para que no tengan una cobertura superpuesta de los mismos servidores en cualquier implementación del centro de recursos, incluido el uso de diferentes servidores de Windows Server Update Services. Esto garantiza que el objetivo detrás del uso de la CAU no se derrota accidentalmente, porque Configuration Manager actualización de @ no__t-0driven no incorpora el conocimiento del clúster.  
  
## <a name="do-i-need-administrative-credentials-to-run-cau"></a>¿Necesito credenciales administrativas para ejecutar la CAU?  
Sí. Para ejecutar las herramientas de la CAU, esta necesita credenciales administrativas en el servidor local o necesita el derecho de usuario de **Suplantar a un cliente tras la autenticación** en el servidor local o en el equipo cliente en el que se está ejecutando. Sin embargo, para coordinar actualizaciones de software en los nodos del clúster, la CAU requiere credenciales administrativas del clúster en todos los nodos. Aunque la interfaz de usuario de CAU puede iniciarse sin las credenciales, pide las credenciales administrativas del clúster cuando se conecta a una instancia de clúster para obtener una vista previa o aplicar actualizaciones.  
  
## <a name="can-i-script-cau"></a>¿Se puede incluir en el script CAU?  
Sí. La CAU viene con cmdlets de PowerShell que ofrecen un amplio conjunto de opciones de scripting. Son los mismos cmdlets que la interfaz de usuario de la CAU llama para realizar acciones de la CAU.  

## <a name="what-happens-to-active-clustered-roles"></a>¿Qué ocurre con los roles en clúster activos?

Los roles en clúster \(formerly denominados aplicaciones y servicios @ no__t-1 que están activos en un nodo, conmutan por error a otros nodos antes de que pueda comenzar la actualización de software. La CAU orquesta estas conmutaciones por error mediante el modo de mantenimiento, que pone en pausa y purga el nodo de todos los roles en clúster activos. Cuando finalizan las actualizaciones de software, la CAU reanuda el nodo y los roles en clúster vuelven a conmutar al nodo actualizado. Esto garantiza que la distribución de roles en clúster relacionados con nodos se mantenga igual durante las ejecuciones de actualización de la CAU de un clúster.  
  
## <a name="how-does-cau-select-target-nodes-for-clustered-roles"></a>¿Cómo selecciona la CAU los nodos de destino para los roles en clúster?

La CAU depende de las API en clúster para coordinar las conmutaciones por error. La implementación de la API de agrupación en clústeres selecciona los nodos de destino confiando en métricas internas y heurística de ubicación inteligente \(such como niveles de carga de trabajo @ no__t-1 en los nodos de destino.  
  
## <a name="does-cau-load-balance-the-clustered-roles"></a>¿Equilibra la carga de CAU los roles en clúster?

La CAU no equilibra la carga de los nodos en clúster, pero intenta preservar la distribución de los roles en clúster. Cuando la CAU finaliza la actualización de un nodo de clúster, intenta volver a conmutar a ese nodo los roles en clúster hospedados anteriormente. La CAU depende de API en clúster para volver a conmutar los recursos al principio del proceso de pausa. Por lo tanto, en ausencia de conmutaciones por error y de la configuración preferida del propietario, la distribución de roles en clúster debe permanecer sin cambios.  
  
## <a name="how-does-cau-select-the-order-of-nodes-to-update"></a>¿Cómo hace la CAU para seleccionar el orden de los nodos para actualizar?  
De manera predeterminada, la CAU selecciona el orden de los nodos para actualizar en función del nivel de actividad. Los nodos que hospedan la menor cantidad de roles en clúster se actualizan primero. Sin embargo, un administrador puede especificar un orden determinado para actualizar los nodos mediante la especificación de un parámetro para la ejecución de actualización en la interfaz de usuario de CAU o mediante los cmdlets de PowerShell.  
  
## <a name="what-happens-if-a-cluster-node-is-offline"></a>¿Qué ocurre si un nodo de clúster está sin conexión?

El administrador que inicializa una ejecución de actualización puede especificar el umbral aceptable para el número de nodos que pueden estar sin conexión. Por lo tanto, una ejecución de actualización puede continuar en un clúster aunque ninguno de los nodos de clúster esté en línea.  
  
## <a name="can-i-use-cau-to-update-only-a-single-node"></a>¿Puedo usar la CAU para actualizar solo un nodo?  
No. La CAU es una herramienta de actualización de clúster @ no__t-0scoped, por lo que solo permite seleccionar clústeres para actualizar. Si desea actualizar un solo nodo, puede usar las herramientas de actualización existentes del servidor, independientemente de la CAU.  
  
## <a name="can-cau-report-updates-that-are-initiated-from-outside-cau"></a>¿Las actualizaciones de informes de CAU se pueden iniciar desde fuera de la CAU?  
No. La CAU solo puede informar de ejecuciones de actualización que se inicializan desde la CAU. Sin embargo, cuando se inicia una ejecución de actualización de CAU subsiguiente, las actualizaciones que se instalaron a través de métodos que no son de no__t-0CAU se consideran adecuadas para determinar las actualizaciones adicionales que pueden ser aplicables a cada nodo de clúster.  
  
## <a name="can-cau-support-my-unique-it-process-needs"></a>¿Puede la CAU admitir mis necesidades de procesos de ti únicos?  
Sí. La CAU ofrece las siguientes dimensiones de flexibilidad para ajustarse a las necesidades únicas de procesos de TI de los clientes empresariales:  
  
**Scripts** de Una ejecución de actualización puede especificar un script de PowerShell pre @ no__t-1update y un script de PowerShell post @ no__t-2update. El script pre-no__t-0update se ejecuta en cada nodo de clúster antes de que el nodo esté en pausa. El script post @ no__t-0update se ejecuta en cada nodo del clúster después de instalar las actualizaciones del nodo.  
  
> [!NOTE]  
> .NET Framework 4,6 o 4,5 y PowerShell deben estar instalados en cada nodo del clúster en el que desea ejecutar los scripts pre @ no__t-0update y post @ no__t-1update. También debe habilitar la comunicación remota de PowerShell en los nodos del clúster. Para obtener información detallada sobre los requisitos del sistema, consulte [requisitos y procedimientos recomendados para la actualización compatible con clústeres](cluster-aware-updating-requirements.md).  
  
**Opciones de ejecución de actualización avanzadas** Además, el administrador puede especificar desde un gran conjunto de opciones de ejecución de actualización avanzadas como el número máximo de veces que se vuelve a intentar el proceso de actualización en cada nodo. Estas opciones pueden especificarse mediante la interfaz de usuario de CAU o los cmdlets de PowerShell de CAU. Esta configuración personalizada se puede guardar en un perfil de ejecución de actualización y se puede volver a usar más adelante para las Ejecuciones de actualización.  
  
**Arquitectura pública de plug-no__t-1in** La CAU incluye características para registrar, anular el registro y seleccionar Plug @ no__t-2ins. La CAU se suministra con dos valores predeterminados de plug @ no__t-0ins: uno coordina las API del agente Windows Update \(WUA @ no__t-2 en cada nodo del clúster. el segundo aplica revisiones que se copian manualmente en un recurso compartido de archivos al que pueden tener acceso los nodos del clúster. Si una empresa tiene necesidades únicas que no se pueden cumplir con estos dos plug-no__t-0ins, la empresa puede compilar una nueva versión de CAU Plug-no__t-1in según la especificación de API pública. Para obtener más información, consulte [cluster @ no__t-1Aware updatinging Plug @ no__t-2in Reference](https://msdn.microsoft.com/library/hh418084(VS.85).aspx).  
  
Para obtener información acerca de la configuración y personalización de la característica de CAU Plug-no__t-0ins para admitir diferentes escenarios de actualización, consulte [how up @ no__t-2Ins Work](assetId:///847b571b-12b3-473c-953f-75a5a1f51333).  
  
## <a name="how-can-i-export-the-cau-preview-and-update-results"></a>¿Cómo puedo exportar la vista previa de la CAU y actualizar los resultados?  
La CAU ofrece opciones de exportación a través de la interfaz Command @ no__t-0line y a través de la interfaz de usuario.  
  
**Comando @ no__t-opciones de la interfaz 1line:**  
  
-   Obtenga una vista previa de los resultados mediante el cmdlet de PowerShell **Invoke @ no__t-1CauScan | ConvertTo @ no__t-2Xml**. Salida: XML  
  
-   Informe de los resultados mediante el cmdlet de PowerShell **Invoke @ no__t-1CauRun | ConvertTo @ no__t-2Xml**. Salida: XML  
  
-   Informe de los resultados mediante el cmdlet de PowerShell **Get @ no__t-1CauReport | Export @ no__t-2CauReport**. Salida: HTML, CSV  
  
**Opciones de la interfaz de usuario:**  
  
-   Copie los resultados del informe desde la pantalla **Vista previa de actualizaciones**. Salida: CSV  
  
-   Copie los resultados del informe desde la pantalla **Generar informe** . Salida: CSV  
  
-   Exporte los resultados del informe desde la pantalla **Generar informe** . Salida: HTML  
  
## <a name="how-do-i-install-cau"></a>¿Cómo se instala la CAU?  
Una instalación de CAU se integra perfectamente en la característica de Clúster de conmutación por error. La CAU se instala de la siguiente manera:  
  
-   Cuando el clúster de conmutación por error está instalado en un nodo de clúster, se instala automáticamente el proveedor de CAU Instrumental de administración de Windows \(WMI @ no__t-1.  
  
-   Cuando se instala la característica herramientas de clúster de conmutación por error en un servidor o equipo cliente, la interfaz de usuario de actualización compatible con clústeres y los cmdlets de PowerShell se instalan automáticamente.
  
## <a name="does-cau-need-components-running-on-the-cluster-nodes-that-are-being-updated"></a>¿Necesita la CAU componentes que se ejecutan en los nodos de clúster que se están actualizando?  
La CAU no necesita un servicio que se ejecute en los nodos del clúster. Sin embargo, la CAU necesita un componente de software @no__t proveedor de WMI de 0The-no__t-1 instalado en los nodos del clúster. Este componente se instala con la característica Clúster de conmutación por error.  
  
Para habilitar el modo Self @ no__t-0updating, el rol en clúster de CAU también debe agregarse al clúster.  
  
## <a name="what-is-the-difference-between-using-cau-and-vmm"></a>¿Cuál es la diferencia entre el uso de CAU y VMM?  
  
-   System Center Virtual Machine Manager \(VMM @ no__t-1 solo se centra en la actualización de clústeres de Hyper @ no__t-2V, mientras que la CAU puede actualizar cualquier tipo de clúster de conmutación por error compatible, incluidos los clústeres de Hyper @ no__t-1A.  
  
-   VMM requiere licencias adicionales, mientras que la CAU tiene licencia para todos los servidores de Windows. Las características, las herramientas y la interfaz de usuario de la CAU se instalan con los componentes de Clúster de conmutación por error.  
  
-   Si ya tiene una licencia de System Center, puede seguir usando VMM para actualizar clústeres de Hyper @ no__t-0V porque ofrece una experiencia de actualización de software y administración integrada.  
  
-   La CAU solo se admite en clústeres que ejecutan Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012. VMM también admite clústeres de Hyper @ no__t-0V en equipos que ejecutan Windows Server 2008 R2 y Windows Server 2008.  
  
## <a name="can-i-use-remote-updating-on-a-cluster-that-is-configured-for-self-updating"></a>¿Puedo usar Remote @ no__t-0updating en un clúster configurado para self @ no__t-1updating?  
Sí. Un clúster de conmutación por error en una configuración de @ no__t-0updating se puede actualizar mediante el servicio remoto @ no__t-1updating en @ no__t-2demand, de la misma forma que se puede forzar un examen de Windows Update en cualquier momento del equipo, incluso si Windows Update está configurado para instalar actualizaciones manera. Sin embargo, debe asegurarse de que no haya una ejecución de actualización en curso.  
  
## <a name="can-i-reuse-my-cluster-update-settings-across-clusters"></a>¿Puedo volver a usar la configuración de actualización de clúster en otros clústeres?  
Sí. La CAU admite una cantidad de opciones de ejecución de actualización que determinan cómo se comporta la ejecución de actualización cuando actualiza el clúster. Estas opciones se pueden guardar como un perfil de ejecución de actualización y se pueden volver a usar en cualquier clúster. Se recomienda guardar y volver a usar la configuración en los clústeres de conmutación por error que tengan necesidades de actualización similares. Por ejemplo, puede crear un perfil de ejecución de actualización de clúster de "Business @ no__t-0Critical SQL Server" para todos los clústeres de Microsoft SQL Server que admiten los servicios Business @ no__t-1critical.  
  
## <a name="where-is-the-cau-plug-in-specification"></a>¿Dónde está la especificación de no__t-0in de la CAU?  
  
-   [Cluster @ no__t-1Aware actualización de plug @ no__t-2in](https://msdn.microsoft.com/library/hh418084(VS.85).aspx)  
  
-   [Ejemplo de actualización compatible con clústeres de no__t-1in](https://code.msdn.microsoft.com/windowsdesktop/Cluster-Aware-Updating-6a8854c9)  
  
## <a name="see-also"></a>Vea también  
  
-   [Cluster @ no__t-1Aware actualización de información general](cluster-aware-updating.md)  
  
