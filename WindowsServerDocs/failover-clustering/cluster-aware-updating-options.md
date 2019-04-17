---
ms.assetid: 2f4b6641-0ec2-4b1c-85fb-a1f1d16685c8
title: "Reconocimiento de lustre Actualizar opciones avanzadas y actualizar los perfiles de ejecución"
ms.topic: article
ms.prod: storage-failover-clustering
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 6/7/2017
description: "Cómo configurar opciones avanzadas y actualizar los perfiles de ejecución para clústeres actualizar (PRU)"
ms.openlocfilehash: 5b6f035791a946ff96ff6a95a1f753ef505d54b4
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="cluster-aware-updating-advanced-options-and-updating-run-profiles"></a>Actualizar clústeres opciones avanzadas y actualizar los perfiles de ejecución

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tema describen opciones de actualización ejecutar que pueden configurarse para un [actualizar clústeres](cluster-aware-updating.md) actualizar ejecutar (PRU). Estas opciones avanzadas pueden configurarse al usar la UI CAU o los cmdlets de PowerShell de Windows PRU para aplicar actualizaciones o para configurar las opciones de actualización automática.

La mayoría de los valores de configuración pueden guardarse como un archivo XML llamado un perfil de la ejecución de actualización y reutiliza las ejecuciones posteriores actualizar. Los valores predeterminados de las opciones de actualización ejecutar proporcionados por PRU también pueden usarse en muchos entornos de clúster.

Para obtener información sobre las opciones adicionales que se pueden especificar para cada actualización ejecutar y perfiles de ejecutar la actualización, consulte las siguientes secciones más adelante en este tema:

Opciones que especifican al solicitar una actualización ejecutar usar actualizar ejecutar perfiles de opciones que se pueden establecer en un perfil de la ejecución de actualización

La siguiente tabla enumera las opciones que se pueden establecer en un perfil de ejecutar PRU actualizar. 

> [!NOTE] 
> Para establecer la opción PreUpdateScript o PostUpdateScript, asegúrate de que se hayan instalado Windows PowerShell y .NET Framework 4.6 o 4.5 y que PowerShell remoto está habilitada en todos los nodos del clúster. Para obtener más información, consulta el tema configurar los nodos de administración remota en [requisitos y los procedimientos recomendados para actualizar clústeres](cluster-aware-updating-requirements.md).


|Opción|Valor predeterminado|Detalles|  
|------------|-------------------|-------------|  
|**StopAfter**|Tiempo ilimitado|Tiempo en minutos tras el cual la actualización de ejecución se detendrá si no se ha completado. **Nota:** si especificas un anteriores a la actualización o un script de PowerShell posteriores a la actualización, todo el proceso de ejecutar los scripts y realizar las actualizaciones deben completarse dentro de la **StopAfter** límite de tiempo.|  
|**WarnAfter**|De manera predeterminada, no aparece ninguna advertencia|Tiempo en minutos después de que aparezca una advertencia si no ha finalizado la actualización de ejecución (como un script de actualización anterior y un script posteriores a la actualización, si se configuran).|  
|**MaxRetriesPerNode**|3|Número máximo de veces que se volverá al proceso de actualización (como un script de actualización anterior y un script posteriores a la actualización, si se configuran) por cada nodo. El máximo es 64.|  
|**MaxFailedNodes**|La mayoría de los clústeres, un entero que es aproximadamente un tercio del número de los nodos del clúster|Número máximo de nodos en el servicio de clúster detiene ejecutando o que la actualización puede producir un error, ya sea porque producirá un error en los nodos. Si se produce un error en un nodo más, se detiene la actualización de ejecución.<br /><br /> El intervalo de valores válido es menor de 0 a 1 que el número de los nodos del clúster.|  
|**RequireAllNodesOnline**|Ninguno|Especifica que todos los nodos deben estar conectados y comienza accesible antes de actualizar.|  
|**RebootTimeoutMinutes**|15|Tiempo en minutos que te permitirá PRU para reiniciar un nodo (si es necesario reiniciar) e iniciar todos los servicios de inicio automático. Si no se completa el proceso de reinicio en este tiempo, actualizar ejecutar en ese nodo se marca como errónea.|  
|**PreUpdateScript**|Ninguno|La ruta de acceso y el nombre de un script de PowerShell para ejecutarlo en cada nodo antes de que comience la actualización, y antes de que el nodo se coloca en modo de mantenimiento. La extensión de nombre de archivo debe ser **. ps1**, y la longitud total de la ruta de acceso más nombre de archivo no debe superar 260 caracteres. Como procedimiento recomendado, el script debe estar ubicado en un disco en el almacenamiento de clúster o en un recurso compartido de archivos de red de alta disponibilidad, para garantizar que siempre es accesible para todos los nodos del clúster. Si el script se encuentra en un recurso compartido de red, asegúrate de que configures el recurso compartido de archivos para obtener permiso de lectura para todos de grupo y restringir el acceso de escritura para prevenir la manipulación de los archivos de usuarios no autorizados.<br /><br /> Si especificas un script anteriores a la actualización, asegúrate de que limita la configuración como la hora (por ejemplo, **StopAfter**) están configurados para permitir que el script se ejecute correctamente. Estos límites abarcan todo el proceso de ejecución de scripts e instalar actualizaciones, no solo el proceso de instalación de actualizaciones.|  
|**PostUpdateScript**|Ninguno|La ruta de acceso y el nombre de un script de PowerShell ejecutar una vez completada la actualización (después de que el nodo sale del modo de mantenimiento). La extensión de nombre de archivo debe ser **. ps1** y la longitud total de la ruta de acceso más nombre de archivo no debe superar 260 caracteres. Como procedimiento recomendado, el script debe estar ubicado en un disco en el almacenamiento de clúster o en un recurso compartido de archivos de red de alta disponibilidad, para garantizar que siempre es accesible para todos los nodos del clúster. Si el script se encuentra en un recurso compartido de red, asegúrate de que configures el recurso compartido de archivos para obtener permiso de lectura para todos de grupo y restringir el acceso de escritura para prevenir la manipulación de los archivos de usuarios no autorizados.<br /><br /> Si especificas un script posteriores a la actualización, asegúrate de que limita la configuración como la hora (por ejemplo, **StopAfter**) están configurados para permitir que el script se ejecute correctamente. Estos límites abarcan todo el proceso de ejecución de scripts e instalar actualizaciones, no solo el proceso de instalación de actualizaciones.|  
|**ConfigurationName**|Esta configuración solo tiene un efecto si ejecutas scripts.<br /><br /> Si especificas una secuencia de comandos anteriores a la actualización o posteriores a la actualización, pero no se especifica un **ConfigurationName**, la sesión de forma predeterminada se usa la configuración para PowerShell (Microsoft.PowerShell).|Especifica la configuración de la sesión de PowerShell que define la sesión en los scripts (especificada por **PreUpdateScript** y **PostUpdateScript**) se ejecutan y puede limitar los comandos que se pueden ejecutar.|  
|**CauPluginName**|**Microsoft.WindowsUpdatePlugin**|Complemento que configures actualizar clústeres a usar para obtener una vista previa de las actualizaciones o realizar una actualización ejecutar. Para obtener más información, consulta [complementos cómo actualizar clústeres funcionan](cluster-aware-updating-plug-ins.md).|  
|**CauPluginArguments**|Ninguno|Un conjunto de *nombre = valor* pares (argumentos) para la actualización complemento usar, por ejemplo:<br /><br /> **Domain=Domain.local**<br /><br /> Estos *nombre = valor* pares deben ser significativos para el complemento que especifiques en **CauPluginName**.<br /><br /> Para especificar un argumento mediante la UI CAU, escribe el *nombre*, presiona la tecla Tab y, a continuación, escribe el correspondiente *valor*. Presiona la tecla Tab a fin de proporcionar el argumento siguiente. Cada *nombre* y *valor* automáticamente se separan con un signo igual (=). Varios pares automáticamente están separadas por punto y coma.<br /><br /> Para obtener el predeterminado **Microsoft.WindowsUpdatePlugin** complemento, se necesita ningún argumento. Sin embargo, puedes especificar un argumento opcional, por ejemplo, para especificar una cadena de consulta de agente de Windows Update estándar para filtrar el conjunto de actualizaciones que se aplican por el complemento. Para una *nombre*, usar **cadena de consulta**y para una *valor*, escriba la consulta completa entre comillas.<br /><br /> Para obtener más información, consulta [complementos cómo actualizar clústeres funcionan](cluster-aware-updating-plug-ins.md).|  
  
##  <a name="BKMK_runtime"></a>Opciones que especifiques al solicitar una actualización ejecutar  
 La siguiente tabla enumera las opciones (excepto las de un perfil de la ejecución de actualización) que se pueden especificar cuando lo solicite una actualización ejecutar. Para obtener información sobre las opciones que se pueden establecer en un perfil de ejecutar la actualización, consulta la tabla anterior.  
  
|Opción|Valor predeterminado|Detalles|  
|------------|-------------------|-------------|  
|**NombreDeClúster**|Ninguno <br>**Nota:** debe establecerse esta opción solo cuando la UI CAU no se ejecuta en un nodo del clúster de conmutación por error, o que quieras hacer referencia a un clúster de conmutación por error diferente de dónde se ejecute la UI CAU.|Nombre NetBIOS del clúster en el que se va a realizar la actualización de ejecución.|  
|**Credenciales**|Credenciales de cuenta actual|Las credenciales administrativas para el destino del clúster en el que ejecutar la actualización se realizará. Puede que ya tengas las credenciales necesarias si iniciar la UI CAU (o abra una sesión de PowerShell, si usas los cmdlets de PowerShell PRU) desde una cuenta que tenga derechos de administrador y permisos en el clúster.|  
|**NodeOrder**|De manera predeterminada, PRU comienza con el nodo que posee el menor número de funciones clústeres, a continuación, avanza al nodo que tiene el número más pequeño en segundo lugar y así sucesivamente.|Nombres de los nodos del clúster en el orden en que deben actualizarse (si es posible).|  
  
##  <a name="BKMK_profile"></a>Usar la actualización de perfiles de ejecución  
 Cada actualización ejecutar pueden asociarse con un perfil de ejecución de la actualización específica. El valor predeterminado que actualizar el perfil de ejecución se almacena en la *%windir%\cluster* carpeta. Si estás usando la UI CAU Actualizar remoto modo, puedes especificar un perfil de la ejecución de actualización en el momento en que se apliquen actualizaciones, o puedes usar el perfil predeterminado de actualizar a ejecutar. Si estás usando PRU en modo de actualización automática, puede importar la configuración de un perfil de ejecución de actualización especificada cuando se configuración las opciones de actualización automática. En ambos casos, puedes invalidar los valores mostrados para las opciones de actualización ejecutar según sus necesidades. Si lo desea, puede guardar las opciones de actualización ejecutar como un perfil de ejecutar la actualización con el mismo nombre de archivo o un nombre de archivo diferentes. La próxima vez que aplicar actualizaciones o configurar automáticamente las opciones de actualización automática, PRU selecciona la actualización perfil de ejecución que se ha seleccionado.  
  
 Puedes modificar un perfil de ejecución de actualización existente o crear uno nuevo seleccionando **crear o modificar actualizar perfil ejecutar** en la UI CAU.

Estas son algunas notas importantes sobre el uso de perfiles de ejecutar la actualización:

* Un perfil de ejecutar la actualización no almacenar información específica del clúster como credenciales administrativas. Si estás usando PRU en modo de actualización automática, el perfil de la ejecución de actualización también no almacena la información de la programación de actualización automática. Esto hace posible compartir un perfil de la ejecución de actualizar entre todos los clústeres de conmutación por error en una clase especificada.
* Si configurar las opciones de actualización automática con un perfil de la ejecución de actualización y modificar más adelante el perfil con distintos valores de las opciones de actualización a ejecutar, no cambia automáticamente la configuración de actualización automática. Para aplicar la nueva configuración de actualización a ejecutar, debe configurar las opciones de actualización automática de nuevo.
* El Editor de perfiles ejecutar Desafortunadamente no admite rutas de acceso de archivos que se incluyen espacios en blanco, como *C:\Program Files*. Como solución alternativa, almacenar tu pre y publicar los scripts de actualización en una ruta de acceso que no puede incluir espacios, o usar PowerShell exclusivamente para administrar perfiles ejecutar, colocar usan comillas en la ruta de acceso cuando se ejecuta **Invoke CauRun**.

### <a name="windows-powershell-equivalent-commands"></a>Comandos equivalentes de Windows PowerShell
  
 Puede importar la configuración de un perfil de ejecutar la actualización cuando ejecutas la **Invoke CauRun**, **agregar CauClusterRole**, o **conjunto CauClusterRole** cmdlet.  
  
 El siguiente ejemplo realiza un examen y una completa actualizar ejecutar en el clúster denominado *CONTOSO FC1*, mediante las opciones de actualización ejecutar que se especifican en *C:\Windows\Cluster\DefaultParameters.xml*. Se usan valores predeterminados para los demás parámetros de cmdlet.  
  
```powershell  
$MyRunProfile = Import-Clixml C:\Windows\Cluster\DefaultParameters.xml  
Invoke-CauRun –ClusterName CONTOSO-FC1 @MyRunProfile   
```  
  
 Al usar un perfil de ejecutar la actualización, puedes actualizar un clúster de conmutación por error reproducible con una configuración de administración de excepciones, los límites de tiempo y otros parámetros operativos coherente. Porque estas opciones de configuración son normalmente específicos de una clase de clústeres de conmutación por error, como "Todos los clústeres de Microsoft SQL Server" o "Mis clústeres fundamentales de la empresa", que es posible que quieras cada perfil de la ejecución de actualización según la clase de clústeres de conmutación por error que se usará con el nombre. Además, es posible que quieras administrar el perfil de la ejecución de actualización en un recurso compartido de archivos que sea accesible para todos los clústeres de conmutación por error de una clase específica de la organización de TI.  
  
  
  
## <a name="see-also"></a>Consulta también

-   [Actualizar clústeres](cluster-aware-updating.md)
  
-   [Compatible con clústeres Cmdlets de actualización en Windows PowerShell](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating)