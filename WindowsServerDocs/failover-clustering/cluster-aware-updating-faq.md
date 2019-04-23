---
ms.assetid: 6416d125-bcaf-433d-971a-2f0283bca2c2
title: 'Actualización de clústeres: preguntas más frecuentes'
ms.topic: article
ms.prod: windows-server-threshold
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 04/28/2017
description: Respuestas a las preguntas más frecuentes acerca de la actualización compatible con clústeres en Windows Server.
ms.openlocfilehash: f9009811093823554f16295cc1205f1b99ead93f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882526"
---
# <a name="cluster-aware-updating-frequently-asked-questions"></a>Actualización de clústeres: Preguntas más frecuentes

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

[Actualización de clústeres](cluster-aware-updating.md) \(CAU\) es una característica que coordina el software actualiza cualquier más de una conmutación por error planeada de un nodo de clúster en todos los servidores en un clúster de conmutación por error de forma que no afecta a la disponibilidad del servicio. Algunas aplicaciones con características de disponibilidad continua \(como Hyper\-V con migración en vivo o servidor de archivos SMB 3.x con conmutación por error transparente de SMB\), CAU puede coordinar la actualización de clústeres automatizada sin tener impacto en la disponibilidad del servicio.

## <a name="does-cau-support-updating-storage-spaces-direct-clusters"></a>¿Admite la actualización de clústeres de espacios de almacenamiento directo CAU?  
Sí. Admite la actualización de CAU [espacios de almacenamiento directo](../storage/storage-spaces/storage-spaces-direct-overview.md) clústeres independientemente del tipo de implementación: hiperconvergido o convergente. En concreto, orquestación de CAU garantiza que cada nodo del clúster de la suspensión espera para que el espacio de almacenamiento en clúster subyacente sea correcto.

## <a name="does-cau-work-with-windowsserver-2008r2-or-windows7"></a>¿Funciona la CAU con Windows Server 2008 R2 o Windows 7?  
No. CAU coordina la operación solo desde equipos que ejecutan Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10, Windows 8.1 o Windows 8 de actualización de clústeres. Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012, debe ejecutar el clúster de conmutación por error que se está actualizando.
  
## <a name="is-cau-limited-to-specific-clustered-applications"></a>¿Se CAU se limita a aplicaciones en clúster específicas?  
No. La CAU es válida para distintos tipos de aplicaciones en clúster. La CAU es un clúster externo\-actualizando la solución que se superpone a las API y cmdlets de PowerShell de clústeres. Por lo tanto, CAU puede coordinar la actualización para cualquier aplicación en clúster que está configurado en un clúster de conmutación por error de Windows Server.  
  
> [!NOTE]  
> Actualmente, las siguientes cargas de trabajo en clúster se prueban y certifican para CAU: SMB, Hyper\-V, replicación DFS, espacios de nombres DFS, iSCSI y NFS.  
  
## <a name="does-cau-support-updates-from-microsoft-update-and-windows-update"></a>¿Admite la CAU actualizaciones de Microsoft Update y Windows Update?  
Sí. De forma predeterminada, la CAU está configurada con un enchufe\-en que utiliza el agente de actualización de Windows \(WUA\) API de la utilidad en los nodos del clúster. La infraestructura de WUA puede configurarse para que apunte a Microsoft Update y Windows Update o Windows Server Update Services \(WSUS\) como su origen de actualizaciones.  
  
## <a name="does-cau-support-wsus-updates"></a>¿Admite la CAU actualizaciones de WSUS?  
Sí. De forma predeterminada, la CAU está configurada con un enchufe\-en que utiliza el agente de actualización de Windows \(WUA\) API de la utilidad en los nodos del clúster. La infraestructura de WUA puede configurarse para que apunte a Microsoft Update y Windows Update o a un servidor local de Windows Server Update Services \(WSUS\) server como origen de actualizaciones.  
  
## <a name="can-cau-apply-limited-distribution-release-updates"></a>¿Puede la CAU aplicar actualizaciones de versiones de distribución limitada?  
Sí. Limita la versión de distribución \(LDR\) actualizaciones, también se denominan revisiones, no se publican a través de Microsoft Update o Windows Update, por lo que no se puede descargar el agente de Windows Update \(WUA\) enchufe\-que CAU usa de forma predeterminada.  
  
Sin embargo, la CAU incluye un segundo conector\-en que puede seleccionar para aplicar actualizaciones de revisión. Esta revisión de plug\-en también se pueden personalizar para aplicar no\-las actualizaciones de BIOS, firmware y controlador de Microsoft.  
  
## <a name="can-i-use-cau-to-apply-cumulative-updates"></a>¿Puedo usar la CAU para aplicar actualizaciones acumulativas?  
Sí. Si las actualizaciones acumulativas son actualizaciones de versiones de distribución general o LDR, la CAU las puede aplicar.  
  
## <a name="can-i-schedule-updates"></a>¿Puedo programar las actualizaciones?  
Sí. La CAU admite los siguientes modos de actualización (ambos permiten programar las actualizaciones):  
  
**Self\-actualizar** permite el clúster se actualice automáticamente en función de un perfil definido y una programación regular, como durante una ventana de mantenimiento mensual. También puede iniciar un autoservicio\-ejecución de actualización a petición en cualquier momento. Para habilitar el autoservicio\-modo de actualización, debe agregar el rol en clúster de CAU al clúster. La CAU self\-función de actualización se comporta como cualquier otra carga de trabajo en clúster y puede funcionar perfectamente con las conmutaciones por error planeada y no planeada de un equipo Coordinador de actualizaciones.  
  
**Remoto\-actualizar** le permite iniciar una ejecución de actualización en cualquier momento desde un equipo que ejecuta Windows o Windows Server. Puede iniciar una ejecución de actualización a través de la ventana de actualización de clústeres o mediante el uso de la **Invoke\-CauRun** cmdlet de PowerShell. Remoto\-la actualización es la predeterminada en modo de actualización para CAU. Puede usar el Programador de tareas para ejecutar el cmdlet [Invoke-CauRun](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-caurun) en un programa deseado desde un equipo remoto que no sea uno de los nodos del clúster.  
  
## <a name="can-i-schedule-updates-to-apply-during-a-backup"></a>¿Puedo programar la actualización que aplicar durante una copia de seguridad?  
Sí. CAU no impone ninguna restricción en este sentido. Sin embargo, realizar actualizaciones de software en un servidor \(con el potencial asociado se reinicia\) mientras una copia de seguridad está en progreso no es una práctica recomendada de TI. Tenga en cuenta que la CAU depende solo de las API en clúster para determinar conmutaciones por error y conmutación por recuperación de recursos; de este modo, la CAU ignora el estado de la copia de seguridad del servidor.  
  
## <a name="can-cau-work-with-system-center-configuration-manager"></a>¿Puede la CAU funcionar con System Center Configuration Manager?  
CAU es una herramienta que coordina las actualizaciones de software en un nodo de clúster y Configuration Manager también realiza actualizaciones de software de servidor. Es importante configurar estas herramientas para que no tengan la superposición de cobertura de los mismos servidores en cualquier implementación de centro de datos, incluido el uso de diferentes servidores de Windows Server Update Services. Esto garantiza que el objetivo de uso de CAU no fracase, dado que Configuration Manager\-controlado por la actualización no incorpora compatibilidad con clústeres.  
  
## <a name="do-i-need-administrative-credentials-to-run-cau"></a>¿Necesito credenciales administrativas para ejecutar la CAU?  
Sí. Para ejecutar las herramientas de la CAU, esta necesita credenciales administrativas en el servidor local o necesita el derecho de usuario de **Suplantar a un cliente tras la autenticación** en el servidor local o en el equipo cliente en el que se está ejecutando. Sin embargo, para coordinar actualizaciones de software en los nodos del clúster, la CAU requiere credenciales administrativas del clúster en todos los nodos. Aunque puede iniciar la UI de CAU sin las credenciales, pedirá las credenciales administrativas del clúster cuando se conecta a una instancia de clúster para obtener una vista previa o aplicar actualizaciones.  
  
## <a name="can-i-script-cau"></a>¿Puedo generar script para la CAU?  
Sí. La CAU viene con cmdlets de PowerShell que ofrecen un amplio conjunto de opciones de scripting. Son los mismos cmdlets que la interfaz de usuario de la CAU llama para realizar acciones de la CAU.  

## <a name="what-happens-to-active-clustered-roles"></a>¿Qué ocurre con los roles en clúster activos?

Roles en clúster \(anteriormente denominados aplicaciones y servicios\) que están activos en un nodo, la conmutación por error a otros nodos antes de que pueda comenzar la actualización de software. La CAU orquesta estas conmutaciones por error mediante el modo de mantenimiento, que pone en pausa y purga el nodo de todos los roles en clúster activos. Cuando finalizan las actualizaciones de software, la CAU reanuda el nodo y los roles en clúster vuelven a conmutar al nodo actualizado. Esto garantiza que la distribución de roles en clúster relacionados con nodos se mantenga igual durante las ejecuciones de actualización de la CAU de un clúster.  
  
## <a name="how-does-cau-select-target-nodes-for-clustered-roles"></a>¿Cómo selecciona los nodos de destino para los roles en clúster CAU?

La CAU depende de las API en clúster para coordinar las conmutaciones por error. La implementación de la API de agrupación en clústeres selecciona los nodos de destino basándose en métricas internas y heurística de ubicación inteligente \(como niveles de carga de trabajo\) en todos los nodos de destino.  
  
## <a name="does-cau-load-balance-the-clustered-roles"></a>¿Equilibra la carga de la CAU de los roles en clúster?

CAU no equilibrar los nodos del clúster, pero intenta preservar la distribución de roles en clúster. Cuando la CAU finaliza la actualización de un nodo de clúster, intenta volver a conmutar a ese nodo los roles en clúster hospedados anteriormente. La CAU depende de API en clúster para volver a conmutar los recursos al principio del proceso de pausa. Por lo tanto, en ausencia de conmutaciones por error y de la configuración preferida del propietario, la distribución de roles en clúster debe permanecer sin cambios.  
  
## <a name="how-does-cau-select-the-order-of-nodes-to-update"></a>¿Cómo hace la CAU para seleccionar el orden de los nodos para actualizar?  
De manera predeterminada, la CAU selecciona el orden de los nodos para actualizar en función del nivel de actividad. Los nodos que hospedan la menor cantidad de roles en clúster se actualizan primero. Sin embargo, un administrador puede especificar un orden determinado para actualizar los nodos al especificar un parámetro para la ejecución de actualización en la UI de CAU o mediante los cmdlets de PowerShell.  
  
## <a name="what-happens-if-a-cluster-node-is-offline"></a>¿Qué ocurre si un nodo de clúster está sin conexión?

El administrador que inicializa una ejecución de actualización puede especificar el umbral aceptable para el número de nodos que pueden estar sin conexión. Por lo tanto, una ejecución de actualización puede continuar en un clúster aunque ninguno de los nodos de clúster esté en línea.  
  
## <a name="can-i-use-cau-to-update-only-a-single-node"></a>¿Puedo usar la CAU para actualizar un solo nodo?  
No. La CAU es un clúster\-ámbito actualizar la herramienta, por lo que solo permite seleccionar clústeres para actualizar. Si desea actualizar un solo nodo, puede usar las herramientas de actualización existentes del servidor, independientemente de la CAU.  
  
## <a name="can-cau-report-updates-that-are-initiated-from-outside-cau"></a>¿La CAU puede informar de las actualizaciones que se inician desde fuera de la CAU?  
No. La CAU solo puede informar de ejecuciones de actualización que se inicializan desde la CAU. Sin embargo, cuando un posteriores CAU ejecución de actualización se inicia, las actualizaciones que se instalaron a través no\-métodos de la CAU se consideran adecuadamente para determinar las actualizaciones adicionales que pueden aplicarse a cada nodo del clúster.  
  
## <a name="can-cau-support-my-unique-it-process-needs"></a>¿Puede soporte CAU mi único proceso de TI necesita?  
Sí. La CAU ofrece las siguientes dimensiones de flexibilidad para ajustarse a las necesidades únicas de procesos de TI de los clientes empresariales:  
  
**Las secuencias de comandos** una ejecución de actualización puede especificar un pre\-actualizar script de PowerShell y la entrada\-actualizar script de PowerShell. El anterior\-script de actualización se ejecuta en cada nodo del clúster antes de que el nodo está en pausa. La entrada de blog\-script de actualización se ejecuta en cada nodo del clúster después de instalaron las actualizaciones del nodo.  
  
> [!NOTE]  
> .NET framework 4.6 o 4.5 y PowerShell deben instalarse en cada nodo del clúster en el que desea ejecutar el anterior\-actualizar y registrar\-actualizar las secuencias de comandos. También debe habilitar la comunicación remota de PowerShell en los nodos del clúster. Para conocer los requisitos del sistema detallados, consulte [requisitos y procedimientos recomendados para la actualización compatible con clústeres](cluster-aware-updating-requirements.md).  
  
**Opciones de ejecución de actualización avanzadas** el administrador también puede especificar un conjunto amplio de opciones de ejecución de actualización avanzadas como el número máximo de veces que se vuelve a intentar el proceso de actualización en cada nodo. Estas opciones se pueden especificar mediante la UI de CAU o los cmdlets de PowerShell de CAU. Esta configuración personalizada se puede guardar en un perfil de ejecución de actualización y se puede volver a usar más adelante para las Ejecuciones de actualización.  
  
**Plug pública\-en arquitectura** CAU incluye características para registrar, anular el registro, y seleccione conecta\-ins. CAU viene con dos valores predeterminados plug\-ins: uno coordina el agente de Windows Update \(WUA\) API en cada nodo del clúster; el segundo aplica revisiones que se copian manualmente en un recurso compartido de archivos que sea accesible para los nodos del clúster. Si una empresa tiene necesidades únicas que no se pueden cumplir con estos dos conectores\-ins, la empresa puede crear un nuevo conector CAU\-de acuerdo con la especificación de API pública. Para obtener más información, consulte [clúster\-compatible con Plug actualizando\-en referencia](https://msdn.microsoft.com/library/hh418084(VS.85).aspx).  
  
Para obtener información sobre cómo configurar y personalizar CAU enchufe\-inicios para admitir diferentes situaciones de actualización, consulte [cómo conectar\-funcionan los complementos](assetId:///847b571b-12b3-473c-953f-75a5a1f51333).  
  
## <a name="how-can-i-export-the-cau-preview-and-update-results"></a>¿Cómo puedo exportar la vista previa de la CAU y actualizar los resultados?  
CAU ofrece opciones mediante el comando de exportación\-interfaz de línea y a través de la interfaz de usuario.  
  
**Comando\-opciones de la interfaz de línea:**  
  
-   Obtener una vista previa de resultados mediante el cmdlet de PowerShell **Invoke\-CauScan | ConvertTo\-Xml**. Salida: XML  
  
-   Informe los resultados mediante el cmdlet de PowerShell **Invoke\-CauRun | ConvertTo\-Xml**. Salida: XML  
  
-   Informe los resultados mediante el cmdlet de PowerShell **obtener\-CauReport | Exportar\-CauReport**. Salida: HTML, CSV  
  
**Opciones de interfaz de usuario:**  
  
-   Copie los resultados del informe desde la pantalla **Vista previa de actualizaciones**. Salida: CSV  
  
-   Copie los resultados del informe desde la pantalla **Generar informe** . Salida: CSV  
  
-   Exporte los resultados del informe desde la pantalla **Generar informe** . Salida: HTML  
  
## <a name="how-do-i-install-cau"></a>¿Cómo se instala la CAU?  
Una instalación de CAU se integra perfectamente en la característica de Clúster de conmutación por error. La CAU se instala de la siguiente manera:  
  
-   Cuando se instala la agrupación en clústeres de conmutación por error en un nodo de clúster, el Instrumental de administración de Windows de la CAU \(WMI\) proveedor se instala automáticamente.  
  
-   Cuando se instala la característica herramientas de agrupación en clústeres de conmutación por error en un servidor o equipo cliente, la actualización compatible con clústeres de la interfaz de usuario y cmdlets de PowerShell se instalan automáticamente.
  
## <a name="does-cau-need-components-running-on-the-cluster-nodes-that-are-being-updated"></a>¿Necesita la CAU componentes que se ejecutan en los nodos del clúster que se están actualizando?  
CAU no necesita un servicio que se ejecutan en los nodos del clúster. Sin embargo, esta necesita un componente de software \(el proveedor WMI\) instalado en los nodos del clúster. Este componente se instala con la característica Clúster de conmutación por error.  
  
Para habilitar el autoservicio\-modo de actualización, el rol en clúster de CAU también debe agregarse al clúster.  
  
## <a name="what-is-the-difference-between-using-cau-and-vmm"></a>¿Qué es la diferencia entre el uso de CAU y VMM?  
  
-   System Center Virtual Machine Manager \(VMM\) se centra en la actualización Hyper solo\-V clústeres, mientras que la CAU puede actualizar cualquier tipo de clúster de conmutación por error admitidas, incluida Hyper\-V clústeres.  
  
-   VMM requiere licencias adicionales, mientras que la CAU tiene licencias para todos los servidores de Windows. Las características, las herramientas y la interfaz de usuario de la CAU se instalan con los componentes de Clúster de conmutación por error.  
  
-   Si ya tiene una licencia de System Center, aún puede usar VMM para actualizar Hyper\-V porque ofrece una experiencia de actualización de software y la administración integrada de clústeres.  
  
-   La CAU solo se admite en clústeres que ejecutan Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012. VMM también admite Hyper\-V clústeres en los equipos que ejecutan Windows Server 2008 R2 y Windows Server 2008.  
  
## <a name="can-i-use-remote-updating-on-a-cluster-that-is-configured-for-self-updating"></a>¿Puedo usar remoto\-actualizar en un clúster que está configurado para sí mismo\-actualizar?  
Sí. Un clúster de conmutación por error en un autoservicio\-actualizando la configuración puede actualizarse por medio de remote\-actualizar en\-exigen, tal como se puede forzar una detección de actualizaciones de Windows en cualquier momento en el equipo, incluso si Windows Update está configurado para instalar actualizaciones automáticamente. Sin embargo, debe asegurarse de que no haya una ejecución de actualización en curso.  
  
## <a name="can-i-reuse-my-cluster-update-settings-across-clusters"></a>¿Puedo volver a usar la configuración de actualización de clúster en otros clústeres?  
Sí. La CAU admite una cantidad de opciones de ejecución de actualización que determinan cómo se comporta la ejecución de actualización cuando actualiza el clúster. Estas opciones se pueden guardar como un perfil de ejecución de actualización y se pueden volver a usar en cualquier clúster. Se recomienda guardar y volver a usar la configuración en los clústeres de conmutación por error que tengan necesidades de actualización similares. Por ejemplo, podría crear una "Business\-perfil de ejecución de crítico SQL Server Cluster actualizando" para todos los clústeres de Microsoft SQL Server compatibles con business\-servicios críticos.  
  
## <a name="where-is-the-cau-plug-in-specification"></a>¿Donde es el enchufe CAU\-en la especificación?  
  
-   [Clúster\-Plug la actualización compatible con\-de referencia](https://msdn.microsoft.com/library/hh418084(VS.85).aspx)  
  
-   [Clúster actualización compatible con plug\-de ejemplo](https://code.msdn.microsoft.com/windowsdesktop/Cluster-Aware-Updating-6a8854c9)  
  
## <a name="see-also"></a>Vea también  
  
-   [Clúster\-información general sobre la actualización compatible con](cluster-aware-updating.md)  
  
