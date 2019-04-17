---
ms.assetid: d44d4430-41e2-483a-9685-92610cdef32a
title: "Cómo funcionan los complementos actualizar clústeres"
ms.topic: article
ms.prod: windows-server-threshold
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 4/28/2017
ms.technology: storage-failover-clustering
description: "Cómo usar complementos para coordinar las actualizaciones al utilizar la actualización de clústeres en Windows Server para instalar las actualizaciones en un clúster."
ms.openlocfilehash: eb606dfbe6596ecabd35e6ac36624fab2b4436b9
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="how-cluster-aware-updating-plug-ins-work"></a>Cómo funcionan los complementos actualizar clústeres

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

[Actualizar clústeres](cluster-aware-updating.md) (PRU) usa complementos para coordinar la instalación de actualizaciones a través de los nodos de un clúster de conmutación por error. Este tema proporciona información sobre cómo usar los built\ en PRU plug\ complementos u otros complementos plug\ que instalas para PRU.

## <a name="BKMK_INSTALL"></a>Instalar un plug\  
Un plug\ en los que no sea el predeterminado plug\ complementos que se instalan con PRU \ (**Microsoft.WindowsUpdatePlugin** y **Microsoft.HotfixPlugin**\) debe instalarse por separado. Si PRU se usa en modo de actualización self\, plug\ en deben instalarse en todos los nodos del clúster. Si PRU se usa en modo de actualización remote\, plug\ en deben instalarse en el equipo remoto del Coordinador de actualización. En la plug\ instalar probablemente tenga requisitos de instalación adicionales en cada nodo.  
  
Para instalar un plug\, sigue las instrucciones del publicador en plug\. Para registrar manualmente en la plug\ con PRU, ejecute el [registro CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin) cmdlet en cada equipo donde está instalado en plug\.  
  
## <a name="specify-a-plug-in-and-plug-in-arguments"></a>Especifica un argumentos en plug\ y plug\ en  
  
### <a name="specify-a-cau-plug-in"></a>Especificar un PRU plug\ en

En la UI CAU, selecciona un plug\ en de una lista de complementos disponibles plug\ drop\ cuando usas PRU para realizar las siguientes acciones:  
  
-   Aplicar actualizaciones al clúster  
  
-   Actualizaciones de vista previa para el clúster  
  
-   Configurar las opciones de actualización self\ de clúster  
  
De manera predeterminada, selecciona PRU plug\ en **Microsoft.WindowsUpdatePlugin**. Sin embargo, puedes especificar cualquier plug\ en que está instalado y registrado con PRU.

> [!TIP]  
> En la UI CAU, solo puedes especificar una sola plug\ de PRU utilizar para obtener una vista previa o para aplicar actualizaciones durante una actualización ejecutar. Mediante los cmdlets de PowerShell PRU, puedes especificar uno o más plug\ complementos. Si necesitas instalar varios tipos de actualizaciones en el clúster, normalmente, es más eficiente para especificar varios plug\ complementos en ejecución de una actualización, en lugar de usar otro actualizar ejecutarla para cada uno de plug\. Por ejemplo, por lo general se producen menos reinicios del nodo.

Mediante los cmdlets de PowerShell de PRU que se enumeran en la siguiente tabla, puedes especificar uno o más plug\ complementos para una actualización ejecutar o el examen pasando el **: CauPluginName** parámetro. Puedes especificar varios nombres de plug\ separándolos con comas. Si especificas varios plug\ complementos, también puedes controlar cómo los complementos plug\ influyen entre sí durante una actualización ejecutar especificando la **\-RunPluginsSerially**, **\-StopOnPluginFailure**, y **: SeparateReboots** parámetros. Para obtener más información sobre el uso de varios plug\ complementos, usar los vínculos proporcionados para la documentación de cmdlet en la siguiente tabla.  
  
|Cmdlet|Descripción|  
|----------|---------------|  
|[CauClusterRole agregar](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/add-cauclusterrole)|Agrega el rol clúster PRU que proporciona la funcionalidad de actualización self\ al clúster especificado.|  
|[CauRun invocar](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-caurun)|Realiza un examen de nodos del clúster aplicable actualizaciones e instala las actualizaciones a través de una actualización de ejecutarse en el clúster especificado.|  
|[CauScan invocar](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-causcan)|Realiza un examen de los nodos del clúster actualizaciones aplicables y devuelve una lista del conjunto inicial de actualizaciones que se aplicaría a cada nodo del clúster especificado.|  
|[Conjunto CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/set-cauclusterrole)|Establece las propiedades de configuración para el rol clúster PRU en el clúster especificado.|  
  
Si no especificas un parámetro de plug\ de PRU mediante el uso de estos cmdlets, el valor predeterminado es plug\ en **Microsoft.WindowsUpdatePlugin**.  
  
### <a name="specify-cau-plug-in-arguments"></a>Especificar los argumentos de plug\ PRU  
Al configurar las opciones de ejecutar la actualización, puedes especificar uno o más *equipo\ = valor* pares \(arguments\) seleccionado plug\ de usar. Por ejemplo, en la UI CAU, puedes especificar varios argumentos como sigue:  
  
**Name1\ = valor1; Name2\ = valor2; Name3\ = Valor3**  
  
Estos *equipo\ = valor* pares deben ser significativos en el plug\ que especifiques. Para algunos complementos plug\ los argumentos son opcionales.  
  
La sintaxis de los argumentos de plug\ de PRU sigue estas reglas generales:  
  
-   Varios *equipo\ = valor* pares están separados por punto y coma.  
  
-   Un valor que contenga espacios es entre comillas, por ejemplo: **Name1\ = "Valor con espacios"**.  
  
-   La sintaxis exacta de *valor* depende en plug\.  
  
Para especificar los argumentos de plug\ mediante los cmdlets de PowerShell PRU que admiten la **: CauPluginParameters** parámetro, pase un parámetro del formulario:  
  
**\-CauPluginArguments @{Name1\ = valor1; Name2\ = valor2; Name3\ = Valor3}**  
  
También puedes usar una tabla hash de PowerShell predefinida. Para especificar los argumentos plug\ en más de uno en plug\, pasar varias tablas de hash de argumentos, separados por comas. Pasar los argumentos de plug\ en el orden plug\ en que se especifica en **CauPluginName**.  
  
### <a name="specify-optional-plug-in-arguments"></a>Especificar los argumentos de plug\ opcionales  
Los complementos plug\ que instala PRU \ (**Microsoft.WindowsUpdatePlugin** y **Microsoft.HotfixPlugin**\) proporcionan opciones adicionales que se pueden seleccionar. En la UI CAU, aparecen en una **opciones adicionales** después de configurar opciones de actualización de ejecución para plug\ en la página. Si estás usando los cmdlets de PowerShell PRU, estas opciones se configuran como argumentos de plug\ opcionales. Para obtener más información, consulta [usar la Microsoft.WindowsUpdatePlugin](#BKMK_WUP) y [usar la Microsoft.HotfixPlugin](#BKMK_HFP) más adelante en este tema.  
  
## <a name="manage-plug-ins-using-windows-powershell-cmdlets"></a>Administrar complementos plug\ mediante los cmdlets de Windows PowerShell  
  
|Cmdlet|Descripción|  
|----------|---------------|  
|[Get-CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/get-cauplugin)|Recupera información sobre el software de uno o más actualizar plug\ complementos que se registran en el equipo local.|  
|[Registro CauPlugin]((https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin))|Registra un software PRU actualizar plug\ en el equipo local.|  
|[CauPlugin anular el registro](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/unregister-cauplugin)|Quita un software de actualización plug\ en de la lista de plug\ complementos que puede usarse en PRU. **Nota:** plug\-ins que se instalan con PRU \ (**Microsoft.WindowsUpdatePlugin** y **Microsoft.HotfixPlugin**\) no se puede anular.|  
  
## <a name="BKMK_WUP"></a>Usando el Microsoft.WindowsUpdatePlugin  

El valor predeterminado plug\ de PRU, **Microsoft.WindowsUpdatePlugin**, realiza las siguientes acciones:
- Se comunica con el agente de actualización de Windows en cada nodo del clúster de conmutación por error para aplicar actualizaciones que son necesarios para los productos de Microsoft que se ejecutan en cada nodo.
- Instala las actualizaciones de clúster directamente desde Windows Update o Microsoft Update, o desde un servidor de Windows Server Update Services \(WSUS\) on\ local.
- Instala solo seleccionado, distribución general lanzamos actualizaciones del \(GDR\). De manera predeterminada, en plug\ aplica solo las actualizaciones de software importantes. Se requiere ninguna configuración. La configuración predeterminada descarga e instala actualizaciones importantes de GDR en cada nodo. 

> [!NOTE]
> Para aplicar actualizaciones que no sean las actualizaciones de software importantes que están activadas de manera predeterminada \ (por ejemplo, actualizaciones de controlador productos\), puedes configurar un parámetro de plug\ opcional. Para obtener más información, consulta [configurar la cadena de consulta de agente de Windows Update](#BKMK_QUERY).

### <a name="requirements"></a>Requisitos

- El clúster de conmutación por error y remoto del Coordinador de actualización equipo \(if used\) deben cumplir los requisitos para PRU y la configuración necesaria para la administración remota enumerados en [requisitos y los procedimientos recomendados para PRU](cluster-aware-updating-requirements.md).
- Revisión [recomendaciones para aplicar actualizaciones de Microsoft](cluster-aware-updating-requirements.md#BKMK_BP_WUA)y, a continuación, realiza los cambios necesarios en la configuración de Microsoft Update para los nodos del clúster de conmutación por error.
- Para obtener mejores resultados, te recomendamos que ejecutes el \(BPA\) PRU analizador de procedimientos recomendados para garantizar que el entorno de clúster y update estén configurados correctamente para aplicar actualizaciones mediante PRU. Para obtener más información, consulta [PRU prueba actualizar readiness](cluster-aware-updating-requirements.md#BKMK_BPA).

> [!NOTE]
> Se excluyen las actualizaciones que requieran la aceptación de términos de licencia de Microsoft o requieren interacción del usuario, y debe instalarse manualmente.

### <a name="additional-options"></a>Opciones adicionales

Opcionalmente, puedes especificar los argumentos de plug\ siguientes para aumentar o restringir el conjunto de actualizaciones que se aplican por plug\ en:
- Para configurar la plug\ en para aplicar las actualizaciones recomendadas, además de las actualizaciones importantes en cada nodo, en la UI CAU, en la **opciones adicionales** página, seleccione la **Ofrecerme actualizaciones recomendadas de la misma forma que recibo las actualizaciones importantes** casilla de verificación.
<br>Como alternativa, configurar la **'IncludeRecommendedUpdates' \ = 'True'** plug\ de argumento.
- Para configurar el plug\ para filtrar los tipos de actualizaciones GDR que se aplican a cada nodo del clúster, especificar una cadena de consulta de agente de Windows Update mediante un **cadena de consulta** plug\ de argumento. Para obtener más información, consulta [configurar la cadena de consulta de agente de Windows Update](#BKMK_QUERY).

### <a name="BKMK_QUERY"></a>Configurar la cadena de consulta de agente de Windows Update  
Puedes configurar un argumento en plug\ para el valor predeterminado, plug\ en **Microsoft.WindowsUpdatePlugin**, que consta de una cadena de consulta \(WUA\) agente de Windows Update. Esta instrucción usa la API de WUA para identificar uno o varios grupos de actualizaciones de Microsoft para aplicar a cada nodo, según los criterios de selección específica. Puedes combinar varios criterios mediante una lógica y o un o lógico. La cadena de consulta WUA se especifica un argumento de plug\ como sigue:  
  
**QueryString\ = "Criterion1\ = valor1 y\ / o Criterion2\ = valor2 y\ / o..."**  
  
Por ejemplo, **Microsoft.WindowsUpdatePlugin** selecciona automáticamente las actualizaciones importantes utilizando un valor predeterminado **cadena de consulta** argumento que se construye usando el **IsInstalled**, **tipo**, **IsHidden**, y **IsAssigned** criterios:  
  
**QueryString\ = "IsInstalled\ = 0 y Type\ = 'Software' y IsHidden\ = 0 y IsAssigned\ = 1"**  
  
Si especificas un **cadena de consulta** argumento, se usa en lugar del predeterminado **cadena de consulta** que está configurado para el inicio de la plug\.  
  
#### <a name="example-1"></a>Ejemplo 1
  
Para configurar un **cadena de consulta** argumento que se instala una actualización específica identificado por ID *f6ce46c1\ 971c\ 43f9\ a2aa\ 783df125f003*:  
  
**QueryString\ = "UpdateID\ = 'f6ce46c1\-971c\-43f9\-a2aa\-783df125f003' y IsInstalled\ = 0"**  
  
> [!NOTE]  
> El ejemplo anterior es válido para aplicar actualizaciones mediante el Asistente para actualización de Cluster\ cuenta. Si quieres instalar una actualización específica mediante la configuración de opciones de actualización self\ con la UI CAU o mediante la **Add\ CauClusterRole** o **Set CauClusterRole**cmdlet de PowerShell, debes formatear el valor UpdateID con dos caracteres de comillas single\:  
>   
> **QueryString\ = "UpdateID\ ='' f6ce46c1\-971c\-43f9\-a2aa\-783df125f003'' y IsInstalled\ = 0"**  
  
#### <a name="example-2"></a>Ejemplo 2
  
Para configurar un **cadena de consulta** argumento que se instala solo los controladores:  
  
**QueryString\ = "IsInstalled\ = 0 y Type\ = 'Controlador' y IsHidden\ = 0"**  
  
Para obtener más información acerca de las cadenas de consulta para el valor predeterminado, plug\ en **Microsoft.WindowsUpdatePlugin**, los criterios de búsqueda \ (como **IsInstalled**\), y la sintaxis que se pueden incluir en las cadenas de consulta, consulta la sección acerca de los criterios de búsqueda en la [referencia de API de Windows Update Agent (WUA)](http://go.microsoft.com/fwlink/p/?LinkId=223304).  
  
## <a name="BKMK_HFP"></a>Usar el Microsoft.HotfixPlugin  
En la plug\ **Microsoft.HotfixPlugin** puede usarse para aplicar actualizaciones \(LDR\) de versión de distribución limitada de Microsoft \ (también denominado revisiones y anteriormente denominada QFEs\) descargar por separado para abordar problemas específicos de software de Microsoft. El complemento instala actualizaciones desde una carpeta raíz de un recurso compartido de archivos SMB y también se puede personalizar para aplicar actualizaciones de BIOS, el firmware y el controlador non\ de Microsoft.

> [!NOTE]
> Revisiones en ocasiones están disponibles para su descarga de Microsoft en los artículos de Knowledge Base, pero también se proporcionan a los clientes según sea necesario as\.

### <a name="requirements"></a>Requisitos

- El clúster de conmutación por error y remoto del Coordinador de actualización equipo \(if used\) deben cumplir los requisitos para PRU y la configuración necesaria para la administración remota enumerados en [requisitos y los procedimientos recomendados para PRU](cluster-aware-updating-requirements.md).
- Revisión [recomendaciones para utilizar la Microsoft.HotfixPlugin](cluster-aware-updating-requirements.md#BKMK_BP_HF).
- Para obtener mejores resultados, te recomendamos que ejecutes el modelo \(BPA\) PRU analizador de procedimientos recomendados para garantizar que el entorno de clúster y update estén configurados correctamente para aplicar actualizaciones mediante PRU. Para obtener más información, consulta [PRU prueba actualizar readiness](cluster-aware-updating-requirements.md#BKMK_BPA).
- Obtener las actualizaciones en el publicador y copiarlos o extraer en un recurso compartido de archivos de bloque de mensajes de servidor \(SMB\) \ (revisión raíz folder\) que admite al menos SMB 2.0 y que sea accesible a todos los nodos del clúster y el equipo remoto del Coordinador de actualización \ (si PRU se usa en modo de conexión a actualizar remote\ errores\). Para obtener más información, consulta [configurar una estructura de carpetas de la raíz de revisión](#BKMK_HF_ROOT) más adelante en este tema. 

    > [!NOTE]
    > De manera predeterminada, este plug\ de solo instala revisiones con las siguientes extensiones de nombre de archivo: .msu, .msi y .msp.

- Copia el archivo DefaultHotfixConfig.xml \ (que se proporciona en la **%systemroot%\\System32\\WindowsPowerShell\\v1.0\\Modules\\ClusterAwareUpdating** carpeta en un equipo donde las herramientas de PRU son installed\) a la carpeta raíz de revisión que creaste y en las que extraer las revisiones. Por ejemplo, copia el archivo de configuración *\\\MyFileServer\\Hotfixes\\Root\\*. 

    > [!NOTE]
    > Para instalar la mayoría de las revisiones proporcionada por Microsoft y otras actualizaciones, el archivo de configuración de revisión predeterminado puede usarse sin modificaciones. Si tu escenario lo requiere, puedes personalizar el archivo de configuración como una tarea avanzada. El archivo de configuración puede incluir reglas personalizadas, por ejemplo, para administrar archivos de revisiones que tienen extensiones específicas, o para definir los comportamientos de las condiciones de salida específico. Para obtener más información, consulta [personalizar el archivo de configuración de revisión](#BKMK_CONFIG_FILE) más adelante en este tema.

### <a name="configuration"></a>Configuración

Configura las opciones siguientes. Para obtener más información, consulta los vínculos a las secciones más adelante en este tema.
- La ruta de acceso a la carpeta raíz de revisión compartida que contenga las actualizaciones para aplicar y que contiene el archivo de configuración de revisión. Puede escribir esta ruta de acceso en la UI CAU o configurar el **HotfixRootFolderPath\ = \ < Path >** PowerShell plug\ de argumento. 

   > [!NOTE]
   > Puedes especificar la carpeta raíz de revisión como una ruta de acceso de la carpeta local o como una ruta de acceso UNC del formulario *\\\ServerName\\Share\\RootFolderName*. Puede usarse una ruta de acceso de DFS Namespace basado en domain\ o independiente. Sin embargo, las características de plug\ que comprueba los permisos de acceso en el archivo de configuración de revisión no son compatibles con una ruta de acceso de DFS Namespace, por lo que si configurar uno, debes deshabilitar la comprobación de permisos de acceso mediante la UI CAU o mediante la configuración de la **DisableAclChecks\ = 'True'** plug\ de argumento.
- La configuración en el servidor que hospeda la carpeta raíz de revisión para comprobar si los permisos adecuados para acceder a la carpeta y garantizar la integridad de los datos de acceder a él desde el SMB comparte la carpeta \ (la firma SMB o Encryption\ SMB). Para obtener más información, consulta [restringir el acceso a la carpeta raíz de revisión](#BKMK_ACL).

### <a name="additional-options"></a>Opciones adicionales

- Opcionalmente, configura el plug\ de para que el cifrado de SMB se aplica al acceder a datos desde el recurso compartido de archivos de revisiones. En la UI CAU, en la **opciones adicionales** página, seleccione la **requerir cifrado de SMB en el acceso a la carpeta raíz de revisión** opción o configurar el **RequireSMBEncryption\ = 'True'** PowerShell plug\ de argumento. 
  > [!IMPORTANT]
  > Debes realizar pasos de configuración adicionales en el servidor SMB para habilitar la integridad de los datos con la firma SMB o cifrado SMB SMB. Para obtener más información, consulta el paso 4 en [restringir el acceso a la carpeta raíz de revisión](#BKMK_ACL). Si seleccionas la opción para forzar el uso del cifrado de SMB y la carpeta raíz de revisión no está configurado para acceder mediante el cifrado de SMB, se producirá un error al ejecutar la actualización.
- Opcionalmente, desactivar las comprobaciones de forma predeterminada los permisos necesarios para la carpeta raíz de revisión y el archivo de configuración de revisión. En la UI CAU, selecciona **Deshabilitar comprobación de acceso de administrador para el archivo de configuración y la carpeta de la raíz de revisión**, o configurar el **DisableAclChecks\ = 'True'** plug\ de argumento.
- Opcionalmente, configura el **HotfixInstallerTimeoutMinutes\ =<Integer> ** argumento para especificar cuánto tiempo espera en el proceso de instalador de revisión para devolver la revisión de plug\. \ (El valor predeterminado es 30 minutos. \) por ejemplo, para especificar un período de tiempo de espera de dos horas, establece **HotfixInstallerTimeoutMinutes\ = 120**.
- Opcionalmente, configura el **HotfixConfigFileName \ = <name> ** plug\ de argumento para especificar un nombre para el archivo de configuración de revisión que se encuentra en la carpeta raíz de revisión. Si no se especifica, se usa el nombre predeterminado DefaultHotfixConfig.xml.
  
### <a name="BKMK_HF_ROOT"></a>Configurar una estructura de carpetas de la raíz de revisión

Para la revisión de plug\ para trabajar, revisiones deben almacenarse en una estructura definida por el well\ en un recurso compartido de archivos SMB \ (revisión raíz folder\), y debes configurar la revisión de plug\ con la ruta de acceso a la carpeta raíz de revisión mediante el uso de la UI de CAU o los cmdlets de PowerShell PRU. Esta ruta de acceso se pasa a la plug\ en como la **HotfixRootFolderPath** argumento. Puedes elegir entre varias estructuras de la carpeta raíz de revisión, según las necesidades de la actualización, como se muestra en los siguientes ejemplos. Se omiten los archivos o carpetas que no cumple con la estructura.  
  
#### <a name="example-1---folder-structure-used-to-apply-hotfixes-to-all-cluster-nodes"></a>Ejemplo 1: estructura de carpetas que se usa para aplicar revisiones a todos los nodos del clúster
  
Para especificar que las revisiones aplicables a todos los nodos del clúster, copiarlos en una carpeta denominada **CAUHotfix\_All** en la carpeta raíz de revisión. En este ejemplo, el **HotfixRootFolderPath** plug\ de argumento se establece en *\\\MyFileServer\\Hotfixes\\Root\\*. La **CAUHotfix\_All** carpeta contiene tres actualizaciones con las extensiones .msu, .msi y .msp que se aplicará a todos los nodos del clúster. Los nombres de archivo de actualización son solo para fines de ilustración.  
  
> [!NOTE]  
> En este y en los ejemplos siguientes, se muestra el archivo de configuración de revisión con su nombre predeterminado DefaultHotfixConfig.xml en su ubicación necesaria en la carpeta raíz de revisión.  
  
```
\\MyFileServer\Hotfixes\Root\   
   DefaultHotfixConfig.xml  
   CAUHotfix_All\   
      Update1.msu   
      Update2.msi   
      Update3.msp  
      ...  
```  
  
#### <a name="example-2---folder-structure-used-to-apply-certain-updates-only-to-a-specific-node"></a>Ejemplo 2: estructura de carpetas que se usa para aplicar algunas actualizaciones solo a un nodo específico
  
Para especificar las revisiones que se aplican solo a un nodo específico, usa una subcarpeta en la carpeta raíz de revisión con el nombre del nodo. Usa el nombre NetBIOS del nodo del clúster, por ejemplo, *ContosoNode1*. A continuación, mueve las actualizaciones que se aplican solo a este nodo a esta subcarpeta. En el siguiente ejemplo, el **HotfixRootFolderPath** plug\ de argumento se establece en *\\\MyFileServer\\Hotfixes\\Root\\*. Actualizaciones en la **CAUHotfix\_All** carpeta se aplicarán a todos los nodos del clúster, y *Node1\_Specific\_Update.msu* se aplica solo a *ContosoNode1*.  
  
```
\\MyFileServer\Hotfixes\Root\   
   DefaultHotfixConfig.xml  
   CAUHotfix_All\   
      Update1.msu   
      Update2.msi   
      Update3.msp  
      ...  
   ContosoNode1\   
      Node1_Specific_Update.msu   
      ...  
```  
  
#### <a name="example-3---folder-structure-used-to-apply-updates-other-than-msu-msi-and-msp-files"></a>Ejemplo 3: estructura de carpetas que se usa para aplicar actualizaciones que no sean los archivos .msu, .msi y .msp
  
De manera predeterminada, **Microsoft.HotfixPlugin** solo se aplica a las actualizaciones con la extensión .msu, .msi o .msp. Sin embargo, ciertas actualizaciones pueden con extensiones diferentes y requieren comandos otra instalación. Por ejemplo, es posible que debas aplicar una actualización de firmware con la extensión .exe a un nodo en un clúster. Puedes configurar la carpeta raíz de revisión con una subcarpeta que indica un determinado, se debe instalar el tipo de actualización non\ predeterminado. También debes configurar una regla de instalación de carpeta correspondiente que especifica el comando de instalación en el `<FolderRules>` elemento en el archivo XML de configuración de revisión.  
  
En el siguiente ejemplo, el **HotfixRootFolderPath** plug\ de argumento se establece en *\\\MyFileServer\\Hotfixes\\Root\\*. Varias actualizaciones se aplicarán a todos los nodos del clúster y una actualización de firmware *SpecialHotfix1.exe* se aplicarán a *ContosoNode1* usando *FolderRule1*. Para obtener información acerca de cómo configurar *FolderRule1* en el archivo de configuración de revisión, consulta [personalizar el archivo de configuración de revisión](#BKMK_CONFIG_FILE) más adelante en este tema.  
  
```
\\MyFileServer\Hotfixes\Root\   
   DefaultHotfixConfig.xml  
   CAUHotfix_All\   
      Update1.msu   
      Update2.msi   
      Update3.msp  
      ...  
  
   ContosoNode1\   
      FolderRule1\  
          SpecialHotfix1.exe  
      ...  
```

### <a name="BKMK_CONFIG_FILE"></a>Personalizar el archivo de configuración de revisión  
La configuración de revisión del archivo controles cómo **Microsoft.HotfixPlugin** tipos de archivo de revisión específica se instala en un clúster de conmutación por error. El esquema XML para el archivo de configuración se define en HotfixConfigSchema.xsd, que se encuentra en la siguiente carpeta en un equipo donde se instalan las herramientas de PRU:  
  
**carpeta %SystemRoot%\\System32\\WindowsPowerShell\\v1.0\\Modules\\ClusterAwareUpdating**  
  
Para personalizar el archivo de configuración de revisión, copia el archivo de configuración de ejemplo DefaultHotfixConfig.xml desde esta ubicación en la carpeta raíz de revisión y efectuar las modificaciones pertinentes para tu escenario.  
  
> [!IMPORTANT]  
> Para aplicar la mayoría de las revisiones proporcionada por Microsoft y otras actualizaciones, el archivo de configuración de revisión predeterminado puede usarse sin modificaciones. Personalización del archivo de configuración de revisión es una tarea únicamente en escenarios de uso avanzado.  
  
De manera predeterminada, el archivo XML de configuración de revisión define las reglas de instalación y condiciones de salida de las siguientes dos categorías de revisiones:  
  
-   Revisión de archivos con extensiones plug\ en instalar de manera predeterminada \ (files\ .msu, .msi y .msp).  
  
    Estos se definen como `<ExtensionRules>` elementos en la `<DefaultRules>` elemento. Hay una `<Extension>` elemento para cada uno de los tipos de archivo compatibles predeterminados. La estructura general de XML es el siguiente:  
  
    ```xml  
    <DefaultRules>  
        <ExtensionRules>  
          <Extension name="MSI">  
            <!-- Template and ExitConditions elements for installation of .msi files follow -->  
             ...  
          </Extension>  
          <Extension name="MSU">  
            <!-- Template and ExitConditions elements for installation of .msu files follow -->  
             ...  
          </Extension>  
          <Extension name="MSP">  
            <!-- Template and ExitConditions elements for installation of .msp files follow -->  
             ...  
          </Extension>  
             ...  
       </ExtensionRules>  
    </DefaultRules>  
    ```  
  
    Si necesitas ciertos tipos de actualizaciones se aplican a todos los nodos del clúster en tu entorno, puedes definir adicionales `<Extension>` elementos.  
  
-   Revisión u otros archivos de actualización que no sean .msi, .msu o archivos .msp, por ejemplo, los controladores de non\ Microsoft, el firmware y el BIOS actualizaciones.  
  
    Cada tipo de archivo predeterminado non\ está configurado como un `<Folder>` elemento en la `<FolderRules>` elemento. El atributo de nombre de la `<Folder>` elemento debe ser idéntico en el nombre de una carpeta en la carpeta raíz de revisión que contenga las actualizaciones del tipo correspondiente. La carpeta puede estar en la **CAUHotfix\_All** carpeta o en una carpeta específica de node\. Por ejemplo, si *FolderRule1* está configurado en la carpeta raíz de revisión, configurar el siguiente elemento en el archivo XML para definir una plantilla de instalación y salir de condiciones de las actualizaciones en la carpeta:  
  
    ```xml  
    <FolderRules>  
          <Folder name="FolderRule1">  
            <!-- Template and ExitConditions elements for installation of updates in FolderRule1 follow -->  
             ...  
          </Folder>  
          ...  
    </FolderRules>  
    ```  
  
Las tablas siguientes se describen los `<Template>` atributos y los posibles `<ExitConditions>` subelementos.  
  
|`<Template>` atributo|Descripción|  
|--------------------------|---------------|  
|`path`|La ruta de acceso completa al programa de instalación para el tipo de archivo que se define en la `<Extension name>` atributo.<br /><br />Para especificar la ruta de acceso a un archivo de actualización en la estructura de carpetas de la raíz de revisión, usar `$update$`.|  
|`parameters`|Una cadena de los parámetros necesarios y opcionales del programa que se especifica en `path`.<br /><br />Para especificar un parámetro que es la ruta de acceso a un archivo de actualización en la estructura de carpetas de la raíz de revisión, usa `$update$`.|  
  
|`<ExitConditions>` subelemento|Descripción|  
|---------------------------------|---------------|  
|`<Success>`|Define uno o varios códigos de salida que indican la actualización especificada se realizó correctamente. Se trata de un subelemento necesario.|  
|`<Success_RebootRequired>`|Opcionalmente, define uno o varios códigos de salida que indican la actualización especificada se realizó correctamente y debe reiniciar el nodo. <br>**Nota:** opcionalmente, el `<Folder>` elemento puede contener la `alwaysReboot` atributo. Si se establece este atributo, lo que indica si una revisión instalada esta regla devuelve uno de los códigos de salida que se define en `<Success>`, que se interpreta como una `<Success_RebootRequired>` salir de la condición.|  
|`<Fail_RebootRequired>`|Opcionalmente, define uno o varios códigos de salida que indican la actualización especificada no pudo y debes reiniciar el nodo.|  
|`<AlreadyInstalled>`|Opcionalmente, define uno o varios códigos de salida que indican que la actualización especificada no se aplicó porque ya está instalada.|  
|`<NotApplicable>`|Opcionalmente, define uno o varios códigos de salida que indican que la actualización especificada no se aplicó porque no se aplica al nodo del clúster.|  
  
> [!IMPORTANT]  
> Cualquier código que no está definido explícitamente en de salida `<ExitConditions>` se interpreta como la actualización no se pudo y no se reinicie el nodo.  
  
### <a name="BKMK_ACL"></a>Restringir el acceso a la carpeta raíz de revisión  
Debes realizar varios pasos para configurar el SMB servidor y el archivo de recurso compartido de archivos para ayudar a proteger los archivos de carpetas de raíz de revisión y el archivo de configuración de hofix solo en el contexto de obtener acceso a **Microsoft.HotfixPlugin**. Estos pasos habilita varias características que ayudan a evitar posibles alteraciones con los archivos de revisiones de manera que podrían poner en peligro el clúster de conmutación por error.  
  
Los pasos generales son los siguientes:  
  
1.  Identificar la cuenta de usuario que se usa para actualizar las ejecuciones mediante el plug\ de  
  
2.  Configurar esta cuenta de usuario en los grupos necesarios en un servidor de archivos SMB  
  
3.  Configurar los permisos de acceso a la carpeta raíz de revisión  
  
4.  Establecer la configuración de la integridad de los datos SMB  
  
5.  Habilitar una regla de Firewall de Windows en el servidor SMB  
  
#### <a name="step-1-identify-the-user-account-that-is-used-for-updating-runs-by-using-the-hotfix-plug-in"></a>Paso 1. Identificar la cuenta de usuario que se usa para actualizar las ejecuciones mediante el uso de la revisión de plug\
  
La cuenta que se usa en PRU para comprobar la configuración de seguridad al realizar una actualización ejecutar usa **Microsoft.HotfixPlugin** depende de si se usa PRU en actualizar remote\ modo o actualizar self\, como sigue:  
  
-   **Modo de actualización Remote\** la cuenta que tenga privilegios administrativos en el clúster para obtener una vista previa y aplicar las actualizaciones.  
  
-   **Modo de actualización Self\** el nombre del objeto de equipo virtual que está configurado en Active Directory para la PRU agrupado rol. Este es el nombre de un objeto de equipo virtual preconfigurados en Active Directory para el rol clúster PRU o el nombre que se genera por PRU para el rol del clúster. Para obtener el nombre si lo genera PRU, ejecute el **Get\ CauClusterRole** cmdlet de PowerShell PRU. En el resultado, **ResourceGroupName** es el nombre de la cuenta del objeto de equipo virtual generado.  
  
#### <a name="step-2-configure-this-user-account-in-the-necessary-groups-on-an-smb-file-server"></a>Paso 2. Configurar esta cuenta de usuario en los grupos necesarios en un servidor de archivos SMB
  
> [!IMPORTANT]  
> Debes agregar la cuenta que se usa para actualizar las ejecuciones como una cuenta de administrador local en el servidor SMB. Si esto no se permite debido a las directivas de seguridad de la organización, configurar esta cuenta con los permisos necesarios en el servidor SMB mediante el siguiente procedimiento.  
  
##### <a name="to-configure-a-user-account-on-the-smb-server"></a>Para configurar una cuenta de usuario en el servidor SMB  
  
1.  Agrega la cuenta que se usa para actualizar las ejecuciones para el grupo de usuarios de COM distribuido y a uno de los siguientes grupos: usuario avanzado, funcionamiento del servidor o el operador de impresión.  
  
2.  Para habilitar los permisos de WMI necesarios para la cuenta, inicia la consola de administración de WMI en el servidor SMB. Inicia PowerShell y, a continuación, escribe el siguiente comando:  
  
    ```  
    wmimgmt.msc  
    ```  
  
3.  En el árbol de consola, right\ clic **Control WMI \(Local\)**y, a continuación, haz clic en **propiedades**.  
  
4.  Haz clic en **seguridad**y después expande **raíz**.  
  
5.  Haz clic en **CIMV2**y, a continuación, haz clic en **seguridad**.  
  
6.  Agrega la cuenta que se usa para la actualización se ejecuta la **nombres de grupo o usuario** lista.  
  
7.  Conceder el **ejecutar métodos** y **habilitar remoto** permisos a la cuenta que se usa para actualizar las ejecuciones.  
  
#### <a name="step-3-configure-permissions-to-access-the-hotfix-root-folder"></a>Paso 3. Configurar los permisos de acceso a la carpeta raíz de revisión
  
De forma predeterminada, al intentar aplicar actualizaciones, la revisión de plug\ comprueba la configuración de los permisos del sistema de archivos NTFS para tener acceso a la carpeta raíz de revisión. Si no están configurados correctamente los permisos de acceso de la carpeta, una actualización ejecutar con la revisión de plug\ podría no.  
  
Si usas la configuración predeterminada de la revisión de plug\, asegúrate de que los permisos de acceso de carpeta cumplan los requisitos siguientes.  
  
-   El grupo usuarios tiene el permiso de lectura.  
  
-   Si en plug\ aplicará las actualizaciones con la extensión .exe, el grupo usuarios tiene permiso de ejecución.  
  
-   Se permiten solo en determinadas entidades de seguridad \ (pero no son required\) escritura o modificación. Los principales permitidos son el grupo, sistema, Creador propietario y TrustedInstaller de administradores local. Otras cuentas o grupos, no se permiten tener escritura o modificación en la carpeta raíz de revisión.  
  
Opcionalmente, puedes deshabilitar las comprobaciones de anteriores en plug\ realiza de forma predeterminada. Puedes hacerlo de dos maneras:  
  
-   Si estás usando los cmdlets de PowerShell PRU, configurar la **DisableAclChecks\ = 'True'** argumento en la **CauPluginArguments** parámetro para la revisión de plug\.  
  
-   Si estás usando la UI CAU, selecciona el **Deshabilitar comprobación de acceso de administrador para el archivo de configuración y la carpeta de la raíz de revisión** opción en la **opciones adicionales de actualización** página del Asistente para la que se usa para configurar opciones de actualización de ejecución.  
  
Sin embargo, como una práctica recomendada en muchos entornos, te recomendamos que uses la configuración predeterminada para aplicar estas comprobaciones.  
  
#### <a name="step-4-configure-settings-for-smb-data-integrity"></a>Paso 4. Establecer la configuración de la integridad de los datos SMB
  
Para comprobar la integridad de los datos en las conexiones entre los nodos del clúster y el recurso compartido de archivos SMB, la revisión de plug\ requiere que habilita la configuración en el recurso compartido de archivos SMB para SMB cifrado o firma SMB. Cifrado de SMB, que ofrece seguridad mejorada y un mejor rendimiento en muchos entornos, se admite a partir de Windows Server 2012. Puedes habilitar una o ambas opciones, como sigue:  
  
-   Para habilitar la firma SMB, consulta el procedimiento descrito en el [artículo 887429](http://support.microsoft.com/kb/887429) en Microsoft Knowledge Base.  
  
-   Para habilitar el cifrado de SMB para la carpeta compartida de SMB, ejecuta el siguiente cmdlet de PowerShell en el servidor SMB:  
  
    ```PowerShell  
    Set-SmbShare <ShareName> -EncryptData $true  
    ```  
  
    Donde <*nombre de recurso compartido*> es el nombre de la carpeta compartida de SMB.  
  
Opcionalmente, para cumplir con el uso de cifrado de SMB en las conexiones con el servidor SMB, activa la **requerir cifrado de SMB en el acceso a la carpeta raíz de revisión** opción en la UI CAU o configurar el **RequireSMBEncryption\ = 'True'** plug\ de argumento mediante los cmdlets de PowerShell PRU.  
  
> [!IMPORTANT]  
> Si seleccionas la opción para forzar el uso del cifrado de SMB y la carpeta raíz de revisión no está configurada para las conexiones que utilizan cifrado SMB, se producirá un error al ejecutar la actualización.  
  
#### <a name="step-5-enable-a-windows-firewall-rule-on-the-smb-server"></a>Paso 5. Habilitar una regla de Firewall de Windows en el servidor SMB
  
Debes habilitar la **administración remota del servidor de archivos \(SMB\-in\)** regla de Firewall de Windows en el servidor de archivos SMB. Esto está habilitado de manera predeterminada en Windows Server 2012, Windows Server 2012 R2 y Windows Server 2016.  
  
## <a name="see-also"></a>Consulta también  
  
-   [Información general de actualización compatible con clústeres](cluster-aware-updating.md)
  
-   [Compatible con clústeres Cmdlets de PowerShell de actualización de Windows](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating)  
  
-   [Compatible con clústeres actualizar la referencia de complemento](http://msdn.microsoft.com/library/hh418084.aspx)  
  
