---
ms.assetid: 2f4b6641-0ec2-4b1c-85fb-a1f1d16685c8
title: Opciones avanzadas de actualización compatible con clústeres y perfiles de ejecución de actualización
description: Cómo configurar opciones avanzadas y perfiles de ejecución de actualización para la actualización compatible con clústeres (CAU)
ms.topic: article
manager: lizross
ms.author: jgerend
author: JasonGerend
ms.date: 08/06/2018
ms.openlocfilehash: f5f81edbe1c7eab772d1c4b1bbe90695fa725f8c
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87990900"
---
# <a name="cluster-aware-updating-advanced-options-and-updating-run-profiles"></a>Opciones avanzadas de actualización compatible con clústeres y perfiles de ejecución de actualización

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012.

En este tema se describen las opciones de ejecución de actualización que se pueden configurar para una ejecución de actualización de [actualización compatible con clústeres](cluster-aware-updating.md) (CAU). Estas opciones avanzadas se pueden configurar cuando se usa la interfaz de usuario de CAU o los cmdlets de Windows PowerShell para la CAU para aplicar actualizaciones o para configurar las opciones de auto-actualización.

La mayoría de las opciones de configuración pueden guardarse como un archivo XML llamado perfil de ejecución de actualización, y reutilizarse más adelante para las ejecuciones de actualización. Los valores predeterminados de las opciones de ejecución de actualización proporcionados por CAU también se pueden usar en muchos entornos de clúster.

Para obtener más información acerca de las opciones adicionales que puede especificar para cada ejecución de actualización y de los perfiles de ejecución de actualización, consulte las siguientes secciones más adelante en este tema:

Opciones que especifique al solicitar una ejecución de actualización usar las opciones de perfiles de ejecución de actualización que se pueden establecer en un perfil de ejecución de actualización

En la siguiente tabla se enumeran las opciones que puede establecer en un perfil de ejecución de actualización de CAU.

> [!NOTE]
> Para establecer la opción PreUpdateScript o PostUpdateScript, asegúrese de que Windows PowerShell y .NET Framework 4,6 o 4,5 están instalados y que la comunicación remota de PowerShell está habilitada en cada nodo del clúster. Para obtener más información, vea configurar los nodos para la administración remota en [requisitos y procedimientos recomendados para la actualización compatible con clústeres](cluster-aware-updating-requirements.md).


|Opción|Valor predeterminado|Detalles|
|------------|-------------------|-------------|
|**StopAfter**|Tiempo ilimitado|El tiempo, en minutos, transcurrido el cual la ejecución de actualización se detendrá si no se ha completado. **Nota:**  Si especifica un script de PowerShell anterior o posterior a la actualización, todo el proceso de ejecución de scripts y actualizaciones se debe completar dentro del límite de tiempo de **StopAfter** .|
|**WarnAfter**|De forma predeterminada, no aparece ninguna advertencia.|El tiempo en minutos transcurrido el cual aparecerá una advertencia si no se completó la ejecución de actualización (incluido un script previo y uno posterior a la actualización, si están configurados).|
|**MaxRetriesPerNode**|3|Número máximo de veces que se volverá a intentar realizar el proceso de actualización (incluido un script previo y uno posterior a la actualización, si están configurados) por nodo. El máximo es 64.|
|**MaxFailedNodes**|En la mayoría de los clústeres, un número entero que corresponde a aproximadamente un tercio del número de nodos de clústeres.|El número máximo de nodos en los que una actualización puede ser incorrecta, ya sea a causa de un error en los nodos o bien porque el servicio de clúster dejó de funcionar. Si se produce un error en uno o más nodos, se detiene la ejecución de actualización.<p>El intervalo válido de valores es de 0 a 1 menos que el número de nodos de clúster.|
|**RequireAllNodesOnline**|Ninguno|Especifica que todos los nodos deben estar en línea y ser accesibles antes de que comience la actualización.|
|**RebootTimeoutMinutes**|15|El tiempo, en minutos, que CAU permitirá para que se reinicie un nodo (en caso de que se requiera un reinicio) y para que comiencen los servicios de inicio automático. Si el proceso de reinicio no se completa en este tiempo, la ejecución de actualización en ese nodo se marca como con errores.|
|**PreUpdateScript**|Ninguno|La ruta de acceso y el nombre de un script de PowerShell que se ejecutarán en cada nodo antes de que comience la actualización y antes de que el nodo se ponga en modo de mantenimiento. La extensión de nombre de archivo debe ser **. PS1**y la longitud total de la ruta de acceso más el nombre de archivo no debe superar los 260 caracteres. El procedimiento recomendado es ubicar el script en un disco en el almacenamiento de clúster o en un recurso compartido de archivos en red de alta disponibilidad para asegurar que todos los nodos de clúster puedan acceder siempre a él. Si el script está ubicado en un recurso compartido de archivos en red, asegúrese de configurarlo de manera que el grupo Todos tenga permiso de lectura y de restringir el acceso de escritura para evitar que usuarios no autorizados alteren los archivos.<p> Si especifica un script previo a la actualización, asegúrese de que las opciones de configuración, como los límites de tiempo (por ejemplo, **StopAfter**), estén establecidas para permitir que el script se ejecute correctamente. Estos límites abarcan todo el proceso de ejecución de scripts e instalación de actualizaciones, no solo el proceso de instalación de actualizaciones.|
|**PostUpdateScript**|Ninguno|Ruta de acceso y nombre de archivo de un script de PowerShell que se va a ejecutar después de que se complete la actualización (después de que el nodo abandone el modo de mantenimiento). La extensión de nombre de archivo debe ser **. PS1** y la longitud total de la ruta de acceso más el nombre de archivo no debe superar los 260 caracteres. El procedimiento recomendado es ubicar el script en un disco en el almacenamiento de clúster o en un recurso compartido de archivos en red de alta disponibilidad para asegurar que todos los nodos de clúster puedan acceder siempre a él. Si el script está ubicado en un recurso compartido de archivos en red, asegúrese de configurarlo de manera que el grupo Todos tenga permiso de lectura y de restringir el acceso de escritura para evitar que usuarios no autorizados alteren los archivos.<p> Si especifica un script posterior a la actualización, asegúrese de que las opciones de configuración, como los límites de tiempo (por ejemplo, **StopAfter**), estén establecidas para permitir que el script se ejecute correctamente. Estos límites abarcan todo el proceso de ejecución de scripts e instalación de actualizaciones, no solo el proceso de instalación de actualizaciones.|
|**ConfigurationName**|Esta configuración solo surte efecto si ejecuta scripts.<p> Si especifica un script anterior o posterior a la actualización, pero no especifica **ConfigurationName**, se usa la configuración de sesión predeterminada para PowerShell (Microsoft. PowerShell).|Especifica la configuración de sesión de PowerShell que define la sesión en la que se ejecutan los scripts (especificados por **PreUpdateScript** y **PostUpdateScript**), y puede limitar los comandos que se pueden ejecutar.|
|**CauPluginName**|**Microsoft.WindowsUpdatePlugin**|Un complemento que se configura para que lo use la actualización compatible con clústeres para obtener una vista previa de las actualizaciones o realizar una ejecución de actualización. Para obtener más información, vea [Cómo funcionan los complementos de actualización compatible con clústeres](cluster-aware-updating-plug-ins.md).|
|**CauPluginArguments**|Ninguno|Un conjunto de pares *nombre=valor* (argumentos) para su uso por parte del complemento de actualización; por ejemplo:<p>**Dominio = dominio. local**<p>Estos pares *nombre=valor* deben ser significativos para el complemento que especifique en **CauPluginName**.<p>Para especificar un argumento mediante la interfaz de usuario de CAU, escriba el *nombre*, presione la tecla Tabulador y escriba el *valor* que corresponda. Presione la tecla Tabulador nuevamente para proporcionar el siguiente argumento. Cada *nombre* y *valor* se separan automáticamente con un signo igual (=) . Si hay varios pares, se separan automáticamente con punto y coma.<p>Para el complemento **Microsoft. WindowsUpdatePlugin** predeterminado, no se necesita ningún argumento. Sin embargo, se puede especificar un argumento opcional para, por ejemplo, especificar una cadena de consulta estándar del agente de Windows Update para filtrar las actualizaciones aplicadas por el complemento. Para un *nombre*, use **QueryString**y, para un *valor*, escriba la consulta completa entre comillas.<p> Para obtener más información, vea [Cómo funcionan los complementos de actualización compatible con clústeres](cluster-aware-updating-plug-ins.md).|

##  <a name="options-that-you-specify-when-you-request-an-updating-run"></a><a name="BKMK_runtime"></a>Opciones que se especifican cuando se solicita una ejecución de actualización
 En la siguiente tabla se enumeran las opciones (que no son las que se encuentran en el perfil de ejecución de actualización) que puede especificar al solicitar una ejecución de actualización. Para obtener información sobre las opciones que puede configurar en un perfil de ejecución de actualización, consulte la tabla anterior.

|Opción|Valor predeterminado|Detalles|
|------------|-------------------|-------------|
|**NombreDeClúster**|Ninguno <br>**Nota:**  Esta opción solo se debe establecer cuando la interfaz de usuario de CAU no se ejecuta en un nodo de clúster de conmutación por error o si desea hacer referencia a un clúster de conmutación por error diferente del lugar donde se ejecuta la interfaz de usuario de CAU.|Nombre NetBIOS del clúster en el que se lleva a cabo la ejecución de actualización.|
|**Credential:**|Credenciales actuales de la cuenta|Las credenciales administrativas para el clúster de destino en el que se llevará a cabo la ejecución de actualización. Es posible que ya tenga las credenciales necesarias si inicia la interfaz de usuario de CAU (o abre una sesión de PowerShell, si usa los cmdlets de PowerShell de CAU) desde una cuenta que tenga derechos y permisos de administrador en el clúster.|
|**NodeOrder**|De manera predeterminada, CAU comienza con el nodo que posee el menor número de roles en clúster y, a continuación, sigue con el nodo que tiene el segundo menor número y así sucesivamente.|Los nombres de los nodos del clúster en el orden en que se deben actualizar (si es posible).|

##  <a name="use-updating-run-profiles"></a><a name="BKMK_profile"></a>Usar perfiles de ejecución de actualización
 Cada ejecución de actualización puede asociarse a un perfil de ejecución de actualización específico. El perfil de ejecución de actualización predeterminado se almacena en la carpeta *%WINDIR%\Cluster* Si usa la interfaz de usuario de CAU en modo de actualización remota, puede especificar un perfil de ejecución de actualización en el momento en que aplique las actualizaciones, o puede usar el perfil de ejecución de actualización predeterminado. Si usa la CAU en modo de auto-actualización, puede importar la configuración de un perfil de ejecución de actualización especificado al configurar las opciones de auto-actualización. En ambos casos, puede reemplazar los valores mostrados en las opciones de ejecución de actualización según sus necesidades. Si lo desea, puede guardar las opciones de ejecución de actualización como un perfil de ejecución de actualización con el mismo nombre de archivo o bien con un nombre diferente. La próxima vez que aplique las actualizaciones o configure las opciones de auto-actualización, CAU seleccionará de forma automática el perfil de ejecución de actualización seleccionado anteriormente.

 Puede modificar un perfil de ejecución de actualización existente o crear uno nuevo; para ello, seleccione **crear o modificar Perfil de ejecución de actualización** en la interfaz de usuario de Cau.

Estas son algunas notas importantes sobre el uso de perfiles de ejecución de actualización:

* Un perfil de ejecución de actualización no almacena información específica del clúster, como las credenciales administrativas. Si usa la CAU en modo de auto-actualización, el perfil de ejecución de actualización tampoco almacena la información de la programación de auto-actualización. Esto permite compartir un perfil de ejecución de actualización con todos los clústeres de conmutación por error en una clase específica.
* Si configura las opciones de auto-actualización mediante un perfil de ejecución de actualización y posteriormente modifica el perfil con valores diferentes para las opciones de ejecución de actualización, la configuración de auto-actualización no cambia automáticamente. Para aplicar la nueva configuración de ejecución de actualización, debe volver a configurar las opciones de auto-actualización.
* Desafortunadamente, el editor de perfiles de ejecución no admite rutas de acceso de archivo que incluyen espacios, como *c:\Archivos*de programa. Como solución alternativa, almacene los scripts de actualización anteriores y posteriores en una ruta de acceso que no incluya espacios, o use PowerShell exclusivamente para administrar los perfiles de ejecución, colocando comillas alrededor de la ruta de acceso al ejecutar **Invoke-CauRun**.

### <a name="windows-powershell-equivalent-commands"></a>Comandos equivalentes de Windows PowerShell

 Puede importar la configuración de un perfil de ejecución de actualización al ejecutar el cmdlet **Invoke-CauRun**, **Add-CauClusterRole**o **set-CauClusterRole** .

 En el siguiente ejemplo se realiza un análisis y una ejecución de actualización completa en un clúster llamado *CONTOSO-FC1*, usando las opciones de ejecución de actualización especificadas en *C:\Windows\Cluster\DefaultParameters.xml*. Se usan los valores predeterminados para los parámetros de cmdlet restantes.

```powershell
$MyRunProfile = Import-Clixml C:\Windows\Cluster\DefaultParameters.xml
Invoke-CauRun –ClusterName CONTOSO-FC1 @MyRunProfile
```

 Mediante un perfil de ejecución de actualización, es posible actualizar un clúster de conmutación por error de forma repetible y con configuraciones coherentes en lo que respecta a administración de excepciones, límites de tiempo y otros parámetros operativos. Dado que esta configuración suele ser específica de una clase de clústeres de conmutación por error, como "todos los clústeres de Microsoft SQL Server" o "mis clústeres críticos para la empresa", es posible que desee asignar un nombre a cada perfil de ejecución de actualización según la clase de clústeres de conmutación por error con la que se usará. Asimismo, recomendamos que administre el perfil de ejecución de actualización en un recurso compartido de archivos al que puedan acceder todos los clústeres de conmutación por error de una clase específica de su organización IT.



## <a name="additional-references"></a>Referencias adicionales

-   [Actualización compatible con clústeres](cluster-aware-updating.md)

-   [Cmdlets de actualización compatible con clústeres en Windows PowerShell](/powershell/module/clusterawareupdating/?view=win10-ps)