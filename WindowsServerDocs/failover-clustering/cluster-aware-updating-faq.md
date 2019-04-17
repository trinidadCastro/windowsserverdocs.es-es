---
ms.assetid: 6416d125-bcaf-433d-971a-2f0283bca2c2
title: "Actualizar clústeres - preguntas más frecuentes"
ms.topic: article
ms.prod: storage-failover-clustering
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 4/28/2017
description: "Obtén respuestas a las preguntas más frecuentes sobre la actualización de clústeres en Windows Server."
ms.openlocfilehash: 8417ea8b6b76e16c3f4db3bac5062d90a8da3ff2
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="cluster-aware-updating-frequently-asked-questions"></a>Actualizar clústeres: Preguntas más frecuentes

> Se aplica a: Windows Server (punto y anual canal), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

[Actualizar clústeres](cluster-aware-updating.md) \(CAU\) es una característica que coordine las actualizaciones de software en todos los servidores en un clúster de conmutación por error de manera que no afecta a la disponibilidad de servicio más de una conmutación por error planeada de un nodo del clúster. Para algunas aplicaciones con características de disponibilidad continua \ como (Hyper\-V con la migración en vivo) o un servidor de archivos SMB 3.x con Failover\ transparente de SMB, puede coordinar PRU clúster automatizada actualizar con ningún impacto en la disponibilidad de servicio.

## <a name="does-cau-support-updating-storage-spaces-direct-clusters"></a>¿PRU compatible con clústeres directa de los espacios de almacenamiento actualización?  
Sí. Admite la actualización PRU [directa de los espacios de almacenamiento](../storage/storage-spaces/storage-spaces-direct-overview.md) clústeres independientemente del tipo de implementación: hyper convergido o convergido. Específicamente, organización PRU garantiza que la suspensión de cada nodo del clúster espera a que el espacio de almacenamiento clúster subyacente estar en buen estado.

## <a name="does-cau-work-with-windows-server-2008-r2-or-windows-7"></a>¿PRU funciona con Windows Server 2008 R2 o Windows 7?  
No. PRU coordenadas del clúster actualización solo de equipos que ejecuten Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10, Windows 8.1 o Windows 8. Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012, debes ejecutar el clúster de conmutación por error está actualizando.
  
## <a name="is-cau-limited-to-specific-clustered-applications"></a>¿PRU está limitado a aplicaciones específicas de clústeres?  
No. PRU es válido para el tipo de la aplicación clúster. PRU es una solución de actualización cluster\ externa que superpuesta clústeres cmdlets de PowerShell y API. Como tal, PRU puede coordinar actualizar para cualquier aplicación clúster que se configura en un clúster de conmutación por error de Windows Server.  
  
> [!NOTE]  
> Actualmente, las cargas de trabajo clústeres siguientes son comprobados o certificados para PRU: SMB, Hyper\-V, la replicación de DFS, espacios de nombres DFS, iSCSI y NFS.  
  
## <a name="does-cau-support-updates-from-microsoft-update-and-windows-update"></a>¿PRU compatible con las actualizaciones de Microsoft Update y Windows Update?  
Sí. De manera predeterminada, PRU está configurado con un plug\ de que usa la API de utilidad de agente de Windows Update \(WUA\) en los nodos del clúster. La infraestructura WUA puede configurarse para que apunte a Microsoft Update y Windows Update o Windows Server Update Services \(WSUS\) como origen de las actualizaciones.  
  
## <a name="does-cau-support-wsus-updates"></a>¿PRU compatible con las actualizaciones WSUS?  
Sí. De manera predeterminada, PRU está configurado con un plug\ de que usa la API de utilidad de agente de Windows Update \(WUA\) en los nodos del clúster. La infraestructura WUA puede configurarse para apuntar a Microsoft Update y Windows Update o a un servidor de Windows Server Update Services \(WSUS\) local como origen de las actualizaciones.  
  
## <a name="can-cau-apply-limited-distribution-release-updates"></a>¿PRU aplicar las actualizaciones de versión de distribución limitada?  
Sí. Actualizaciones \(LDR\) de versión de distribución limitada, también se denomina revisiones, no se publican a través de Microsoft Update o Windows Update, por lo que no se pueden descargar mediante el agente de Windows Update de \(WUA\) plug\-que PRU utiliza de manera predeterminada.  
  
Sin embargo, PRU incluye un segundo plug\ que puede seleccionar para aplicar actualizaciones de revisión. También puede personalizarse esta revisión en plug\ para aplicar actualizaciones de BIOS, el firmware y el controlador non\ de Microsoft.  
  
## <a name="can-i-use-cau-to-apply-cumulative-updates"></a>¿Puedo usar PRU para aplicar actualizaciones acumulativas?  
Sí. Si hay actualizaciones de versión de distribución general o LDR las actualizaciones acumulativas, PRU puede aplicarlas.  
  
## <a name="can-i-schedule-updates"></a>¿Puedo programar actualizaciones?  
Sí. PRU admite los siguientes modos de actualización, que permiten actualizar programarse:  
  
**Actualizar Self\** permite al clúster actualizarse según una programación regular y de un perfil definido como durante un período de mantenimiento mensual. También puede iniciar una actualización Self\ ejecutarse a petición en cualquier momento. Para habilitar el modo de actualización self\, debes agregar el rol clúster PRU al clúster. La característica de actualización self\ PRU realiza como cualquier otra carga de trabajo clúster, y puede funcionar sin problemas de la conmutación por error planificado y de un equipo del Coordinador de actualización.  
  
**Actualizar Remote\** te permite iniciar una actualización ejecutar en cualquier momento desde un equipo que ejecute Windows o Windows Server. Puede iniciar una actualización ejecutar a través de la ventana actualizar clústeres o mediante la **Invoke\ CauRun** cmdlet de PowerShell. Actualizar Remote\ predeterminado es el modo de actualización para PRU. Puedes usar el programador de tareas para ejecutar el [Invoke CauRun](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-caurun) cmdlet en una programación deseada desde un equipo remoto que no es uno de los nodos del clúster.  
  
## <a name="can-i-schedule-updates-to-apply-during-a-backup"></a>¿Puedo programar actualizaciones para aplicar durante una copia de seguridad?  
Sí. PRU no impone restricciones a este respecto. Sin embargo, realizar actualizaciones de software en un servidor \ (con la restarts\ posibles asociada) mientras una copia de seguridad del servidor está en progreso no es un procedimiento recomendado de TI. Ten en cuenta que PRU depende solo clústeres de API para determinar el recurso migraciones tras error y las; por lo tanto, PRU no conoce el estado de copia de seguridad del servidor.  
  
## <a name="can-cau-work-with-system-center-configuration-manager"></a>¿Puede PRU funcionan con System Center Configuration Manager?  
PRU es una herramienta que coordine las actualizaciones de software en un nodo del clúster y Configuration Manager también lleva a cabo las actualizaciones de software de servidor. Es importante configurar estas herramientas para que no tienen superpuestas cobertura de los mismos servidores en cualquier implementación datacenter, incluido el uso de distintos servidores de Windows Server Update Services. Esto garantiza que el objetivo detrás de PRU no se accidentalmente rechaza, porque controlada por el Administrador de configuración archivos\ actualizar no incorpora el reconocimiento de clúster.  
  
## <a name="do-i-need-administrative-credentials-to-run-cau"></a>¿Necesito credenciales administrativas para ejecutar PRU?  
Sí. Para ejecutar las herramientas de PRU, PRU necesite credenciales administrativas en el servidor local, o el **suplantar a un cliente tras la autenticación** derecho de usuario en el servidor local o el equipo cliente en el que se está ejecutando. Sin embargo, para coordinar las actualizaciones de software en los nodos del clúster, PRU requiere credenciales administrativas del clúster en todos los nodos. Aunque se pueden iniciar la UI CAU sin las credenciales, pedirá las credenciales administrativas del clúster cuando se conecta a una instancia de clúster para obtener una vista previa o aplicar las actualizaciones.  
  
## <a name="can-i-script-cau"></a>¿Script PRU?  
Sí. PRU incluye cmdlets de PowerShell que ofrecen un amplio conjunto de opciones de scripting. Estos son los cmdlets mismos que la UI CAU llama para realizar acciones PRU.  

## <a name="what-happens-to-active-clustered-roles"></a>¿Qué ocurre con los roles clústeres activos?

Roles de clúster \ (anteriormente denominada aplicaciones y services\) que están activos en un nodo, conmutación por error a otros nodos antes de comenzar la actualización de software. PRU organiza estas migraciones tras error mediante el modo de mantenimiento, lo que pausa y agotaría el nodo de todos los roles clústeres activos. Cuando las actualizaciones de software se completan, PRU reanuda el nodo y las funciones clústeres producirá un error en el nodo actualizado. Esto garantiza que la distribución de clústeres roles con respecto a nodos permanece igual a través de las ejecuciones de actualización de PRU de un clúster.  
  
## <a name="how-does-cau-select-target-nodes-for-clustered-roles"></a>¿Cómo selecciona PRU nodos de destino para las funciones clústeres?

PRU se basa en clústeres API para coordinar las migraciones tras error. La implementación de la API de clúster selecciona los nodos de destino al confiar en las métricas internas y medidas heurísticas ubicación inteligente \ (por ejemplo, la carga de trabajo levels\) a través de los nodos de destino.  
  
## <a name="does-cau-load-balance-the-clustered-roles"></a>¿Carga PRU equilibra las funciones clústeres?

PRU no carga saldo los nodos del clústeres, pero intenta conservar la distribución de las funciones clústeres. Cuando PRU finaliza su actualización un nodo del clúster, trata de un error atrás anteriormente hospedan agrupados roles a ese nodo. PRU se basa en clústeres API para conmutar los recursos al principio del proceso de pausa. Por lo tanto, en ausencia de imprevisto migraciones tras error y la configuración de propietario preferido, la distribución de clústeres roles debe no se modifica.  
  
## <a name="how-does-cau-select-the-order-of-nodes-to-update"></a>¿Cómo PRU selecciona el orden de los nodos de actualizar?  
De manera predeterminada, PRU selecciona el orden de los nodos para actualizar según el nivel de actividad. Los nodos que hospedan los roles clústeres menor se actualizan en primer lugar. Sin embargo, un administrador puede especificar un determinado orden para actualizar los nodos especificando un parámetro para la actualización ejecutar en la UI CAU o mediante los cmdlets de PowerShell.  
  
## <a name="what-happens-if-a-cluster-node-is-offline"></a>¿Qué sucede si no está conectado a un nodo del clúster?

El administrador que inicia una actualización ejecutar puede especificar el umbral aceptable para el número de los nodos que puede ser sin conexión. Por lo tanto, ejecutar una actualización puede continuar en un clúster incluso si los nodos del clúster no están en línea.  
  
## <a name="can-i-use-cau-to-update-only-a-single-node"></a>¿Puedo usar PRU para actualizar un solo nodo?  
No. PRU es una herramienta de actualización cluster\ ámbito, por lo que solo permite seleccionar clústeres para actualizar. Si quieres actualizar un nodo único, puedes usar herramientas PRU independientemente de la actualización de servidor existente.  
  
## <a name="can-cau-report-updates-that-are-initiated-from-outside-cau"></a>¿Puede PRU notificar las actualizaciones que se inician desde PRU exterior?  
No. PRU puede solo actualizar se ejecute un informe que se inician desde dentro de PRU. Sin embargo, cuando se inicia un posteriores PRU actualizar ejecutar, las actualizaciones que se instalaron mediante métodos non\ PRU se consideran correctamente para determinar las actualizaciones adicionales que podrían ser aplicables a cada nodo del clúster.  
  
## <a name="can-cau-support-my-unique-it-process-needs"></a>¿Puede soporte PRU necesita el proceso de TI único?  
Sí. PRU ofrece las siguientes dimensiones de flexibilidad para adaptarlo a necesidades de proceso único de TI de los clientes de empresa:  
  
**Scripts** actualizar ejecutar una puede especificar un script de PowerShell de actualización de pre\ y un script de PowerShell post\ actualización. El script de actualización de pre\ se ejecuta en cada nodo del clúster antes de que el nodo está en pausa. El script de actualización de post\ se ejecuta en cada nodo del clúster después de instalaron las actualizaciones del nodo.  
  
> [!NOTE]  
> .NET framework 4.6 o 4.5 y PowerShell deben instalarse en cada nodo del clúster en el que quieres ejecutar los scripts de actualización de pre\ y post\ actualización. También debes habilitar PowerShell remoto en los nodos del clúster. Para los requisitos de sistema detallada, consulta [requisitos y los procedimientos recomendados para actualizar clústeres](cluster-aware-updating-requirements.md).  
  
**Actualizar ejecutar opciones avanzadas** el administrador puede especificar además de un gran conjunto de opciones avanzadas de actualizar a ejecutar, como el número máximo de veces que se vuelve a intentar el proceso de actualización en cada nodo. Estas opciones pueden especificarse mediante la UI CAU o los cmdlets de PowerShell PRU. Estas opciones de configuración personalizados se pueda guardar en un perfil de la ejecución de actualización y reutilizar las ejecuciones posteriores actualizar.  
  
**Arquitectura de plug\ pública** PRU incluye características para anular el registro, el registro, y plug\ complementos. selecciona PRU se entrega con dos predeterminado plug\-ins: uno coordenadas de la API de Windows Update Agent \(WUA\) en cada nodo del clúster; la segunda aplica las revisiones que se copian manualmente a un recurso compartido de archivos que se puede acceder a los nodos del clúster. Si una empresa tiene requisitos únicos que no se pueden cumplir con estos dos plug\ complementos, la empresa puede generar un nuevos PRU plug\ de acuerdo con la especificación de API pública. Para obtener más información, consulta [Cluster\ cuenta actualizar Plug\ en referencia](http://msdn.microsoft.com/library/hh418084(VS.85).aspx).  
  
Para obtener información sobre la configuración y personalización PRU plug\ complementos para admitir distintas situaciones de actualización, consulta el tema [cómo funcionan los complementos Plug\](assetId:///847b571b-12b3-473c-953f-75a5a1f51333).  
  
## <a name="how-can-i-export-the-cau-preview-and-update-results"></a>¿Cómo se puede exportar la vista previa de PRU y actualizar los resultados?  
PRU ofrece opciones de exportación a través de la interfaz de línea de Web\ y la interfaz de usuario.  
  
**Opciones de la interfaz de línea de Web\:**  
  
-   Obtener una vista previa de resultados mediante el cmdlet de PowerShell **Invoke\ CauScan | ConvertTo\ Xml**. Salida: XML  
  
-   Notificar los resultados mediante el cmdlet de PowerShell **Invoke\ CauRun | ConvertTo\ Xml**. Salida: XML  
  
-   Notificar los resultados mediante el cmdlet de PowerShell **Get\ CauReport | Export\ CauReport**. Salida: HTML, CSV  
  
**Opciones de la interfaz de usuario:**  
  
-   Copia los resultados del informe de la **obtener una vista previa de las actualizaciones** pantalla. Salida: CSV  
  
-   Copia los resultados del informe de la **generar informe** pantalla. Salida: CSV  
  
-   Exportar los resultados del informe de la **generar informe** pantalla. Salida: HTML  
  
## <a name="how-do-i-install-cau"></a>¿Cómo instalo PRU?  
Una instalación PRU se integra perfectamente en la característica clúster de conmutación por error. PRU se instala como sigue:  
  
-   Cuando se instala la clústeres de conmutación por error en un nodo del clúster, el proveedor \(WMI\) PRU Instrumental de administración de Windows se instala automáticamente.  
  
-   Cuando se instala la característica de herramientas de clúster de conmutación por error en un equipo cliente o servidor, los cmdlets de PowerShell y la interfaz de usuario actualizando clústeres se instalan automáticamente.
  
## <a name="does-cau-need-components-running-on-the-cluster-nodes-that-are-being-updated"></a>¿Es necesario PRU componentes que se ejecutan en los nodos del clúster que se van a actualizar?  
PRU no necesita un servicio que se ejecuta en los nodos del clúster. Sin embargo, PRU necesita un componente de software \ (la provider\ WMI) instalados en los nodos del clúster. Este componente se instala con la característica clúster de conmutación por error.  
  
Para habilitar el modo de actualización self\, el rol clúster PRU también debe agregarse al clúster.  
  
## <a name="what-is-the-difference-between-using-cau-and-vmm"></a>¿Qué es la diferencia entre usar PRU y VMM?  
  
-   System Center Virtual Machine Manager \(VMM\) se centra en la actualización solo clústeres Hyper\-V, mientras que PRU puede actualizar cualquier tipo de clúster de conmutación admitido, incluidos los clústeres Hyper\-V.  
  
-   VMM requiere una licencia adicional, mientras que PRU tiene licencia para todos los Windows Server. Las características PRU, herramientas y la interfaz de usuario se instalan con componentes de clústeres de conmutación por error.  
  
-   Si ya tienes una licencia de System Center, puedes seguir usar VMM para actualizar clústeres Hyper\-V, ya que ofrece una administración integrada y la experiencia de actualización de software.  
  
-   PRU solo es compatible con clústeres que ejecutan Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012. VMM también es compatible con clústeres Hyper\-V en equipos que ejecutan Windows Server 2008 R2 y Windows Server 2008.  
  
## <a name="can-i-use-remote-updating-on-a-cluster-that-is-configured-for-self-updating"></a>¿Puedo usar actualizar remote\ en un clúster que está configurado para actualizar self\?  
Sí. Un clúster de conmutación por error en una configuración de actualización self\ puede actualizarse a través de la actualización remote\ on\ demanda, al igual que puedes forzar a un examen de Windows Update en cualquier momento en el equipo, incluso si Windows Update está configurado para instalar actualizaciones automáticamente. Sin embargo, debes para asegurarse de que una actualización ejecutar no está en curso.  
  
## <a name="can-i-reuse-my-cluster-update-settings-across-clusters"></a>¿Se puede reutilizar mi configuración de actualización de clúster entre clústeres?  
Sí. PRU admite una serie de opciones de actualización ejecutar que determinan cómo ejecutar la actualización se comporta cuando se actualiza el clúster. Estas opciones se pueden guardar como un perfil de la ejecución de actualización y puede reutilizar en cualquier clúster. Te recomendamos que guardar y reutilizar la configuración en los clústeres de conmutación por error que tienen las mismas necesidades de actualización. Por ejemplo, podrías crear un "Business\ críticos SQL Server clúster Actualizar perfil de ejecución" para todos los clústeres de Microsoft SQL Server que admiten los servicios críticos business\.  
  
## <a name="where-is-the-cau-plug-in-specification"></a>¿Dónde está la especificación de plug\ de PRU?  
  
-   [Reconocimiento de Cluster\ actualizar Plug\ de referencia](http://msdn.microsoft.com/library/hh418084(VS.85).aspx)  
  
-   [Actualizar cuenta plug\ en la muestra de clúster](http://code.msdn.microsoft.com/windowsdesktop/Cluster-Aware-Updating-6a8854c9)  
  
## <a name="see-also"></a>Consulta también  
  
-   [Información general de actualización Cluster\ cuenta](cluster-aware-updating.md)  
  
