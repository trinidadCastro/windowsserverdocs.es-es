---
ms.assetid: d44d4430-41e2-483a-9685-92610cdef32a
title: Cómo funcionan los complementos Cluster-Aware Updating
ms.topic: article
ms.prod: windows-server-threshold
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 04/28/2017
ms.technology: storage-failover-clustering
description: Cómo usar los complementos para coordinar las actualizaciones cuando se usa la actualización de clústeres en Windows Server para instalar actualizaciones en un clúster.
ms.openlocfilehash: d09addb5e6787a8386d50570c0d27640646aa587
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854566"
---
# <a name="how-cluster-aware-updating-plug-ins-work"></a>Cómo funcionan los complementos Cluster-Aware Updating

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

[Actualización de clústeres](cluster-aware-updating.md) (CAU) usa complementos para coordinar la instalación de actualizaciones en todos los nodos en un clúster de conmutación por error. En este tema se proporciona información sobre el uso de la compilación\-en plug CAU\-otro plug o complementos\-ins que instalar para CAU.

## <a name="BKMK_INSTALL"></a>Instalar un conector\-en  
Un enchufe\-en distinto del predeterminado enchufe\-que se instalan con CAU \( **Microsoft.WindowsUpdatePlugin** y **Microsoft.HotfixPlugin** \)deben instalarse por separado. Si se usa CAU en self\-modo, el conector de actualización\-debe estar instalada en todos los nodos del clúster. Si se usa CAU en remoto\-modo, el conector de actualización\-en debe instalarse en el equipo remoto del Coordinador de actualizaciones. Un enchufe\-en que instale pueden tener requisitos de instalación adicionales en cada nodo.  
  
Para instalar un conector\-, siga las instrucciones desde el enchufe\-en el publicador. Para registrar manualmente un enchufe\-con CAU, ejecute el [Register-CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin) cmdlet en cada equipo donde el enchufe\-está instalado.  
  
## <a name="specify-a-plug-in-and-plug-in-arguments"></a>Especifique un enchufe\-en y conecte\-argumentos de entrada  
  
### <a name="specify-a-cau-plug-in"></a>Especifique un enchufe CAU\-en

En la UI de CAU, selecciona un enchufe\-en de una caída\-hacia abajo en la lista de complemento disponible\-ins cuando uses CAU para realizar las siguientes acciones:  
  
-   Aplicar actualizaciones al clúster  
  
-   Obtener una vista previa de las actualizaciones del clúster  
  
-   Configurar clúster self\-opciones de actualización  
  
De forma predeterminada, CAU selecciona el enchufe\-en **Microsoft.WindowsUpdatePlugin**. Sin embargo, puede especificar cualquier plug\-en que está instalado y registrado con CAU.

> [!TIP]  
> En la UI de CAU, puede especificar solo un único conector\-de para CAU lo use para obtener una vista previa o aplicar actualizaciones durante una ejecución de actualización. Mediante el uso de los cmdlets de PowerShell de CAU, puede especificar uno o más plug\-ins. Si necesita instalar varios tipos de actualizaciones en el clúster, es más eficaz suele ser especificar varios plug\-inicios en una ejecución de actualización, en lugar de utilizar una ejecución de actualización independientes para cada plug\-en. Por ejemplo, lo normal es que se produzcan menos reinicios de nodos.

Mediante los cmdlets de PowerShell de CAU que se enumeran en la tabla siguiente, puede especificar uno o más plug\-inicios para una ejecución de actualización o análisis pasando el **– CauPluginName** parámetro. Puede especificar varios plug\-en nombres separándolos con comas. Si especifica varios plug\-ins, también puede controlar cómo el enchufe\-se influyen mutuamente durante una ejecución de actualización mediante la especificación de la  **\-RunPluginsSerially**,  **\- StopOnPluginFailure**, y **– SeparateReboots** parámetros. Para obtener más información sobre el uso de varios plug\-ins, use los vínculos proporcionan a la documentación del cmdlet en la tabla siguiente.  
  
|Cmdlet|Descripción|  
|----------|---------------|  
|[Add-CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/add-cauclusterrole)|Agrega el rol en clúster de CAU que proporciona el valor self\-actualización de las funciones para el clúster especificado.|  
|[Invoke-CauRun](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-caurun)|Realiza un examen de los nodos del clúster para buscar actualizaciones aplicables y las instala por medio de una ejecución de actualización en el clúster especificado.|  
|[Invoke-CauScan](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-causcan)|Realiza un examen de los nodos del clúster para buscar actualizaciones aplicables y devuelve una lista del conjunto inicial de actualizaciones que se aplicarían a cada nodo en el clúster especificado.|  
|[Set-CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/set-cauclusterrole)|Establece las propiedades de configuración del rol en clúster de CAU en el clúster especificado.|  
  
Si no especifica un enchufe CAU\-en parámetro mediante el uso de estos cmdlets, el valor predeterminado es el enchufe\-en **Microsoft.WindowsUpdatePlugin**.  
  
### <a name="specify-cau-plug-in-arguments"></a>Especificar CAU enchufe\-argumentos de entrada  
Al configurar las opciones de ejecución de actualización, puede especificar uno o varios *nombre\=valor* pares \(argumentos\) para el conector seleccionado\-sesión para usar. Por ejemplo, en la interfaz de usuario de CAU puedes especificar varios argumentos de la siguiente manera:  
  
**Nombre1\=valor1; Nombre2\=valor2; Nombre3\=Value3**  
  
Estos *nombre\=valor* pares deben ser significativos para el enchufe\-en que especifique. Para algunos otros se conectan\-inicios de los argumentos son opcionales.  
  
La sintaxis de la clavija CAU\-argumentos de entrada sigue estas reglas generales:  
  
-   Varios *nombre\=valor* pares están separados por punto y coma.  
  
-   Un valor que contenga espacio se incluye entre comillas, por ejemplo: **Nombre1\="Value with Spaces"**.  
  
-   La sintaxis exacta de *valor* depende el enchufe\-en.  
  
Para especificar plug\-argumentos de entrada mediante los cmdlets de PowerShell de CAU que admiten la **– CauPluginParameters** parámetro, pase un parámetro del formulario:  
  
**\-CauPluginArguments @{Name1\=valor1; Nombre2\=valor2; Nombre3\=Valor3}**  
  
También puede usar una tabla hash de PowerShell predefinida. Para especificar plug\-en argumentos de más de un plug\-, pasar varias tablas hash de argumentos, separados por comas. Pase el enchufe\-argumentos de entrada en el enchufe\-para que se especifica en **CauPluginName**.  
  
### <a name="specify-optional-plug-in-arguments"></a>Especificar plug opcional\-argumentos de entrada  
El enchufe\-complementos que instala CAU \( **Microsoft.WindowsUpdatePlugin** y **Microsoft.HotfixPlugin** \) proporcionan opciones adicionales que se pueden seleccionar. En la UI de CAU, aparecen en una **opciones adicionales** página después de configurar las opciones de ejecución de actualización para el enchufe\-en. Si usas los cmdlets de PowerShell de CAU, estas opciones se configuran como plug opcional\-argumentos de entrada. Para más información, consulte [Usar Microsoft.WindowsUpdatePlugin](#BKMK_WUP) y [Usar Microsoft.HotfixPlugin](#BKMK_HFP) más adelante en este tema.  
  
## <a name="manage-plug-ins-using-windows-powershell-cmdlets"></a>Administrar plug\-ins mediante cmdlets de Windows PowerShell  
  
|Cmdlet|Descripción|  
|----------|---------------|  
|[Get-CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/get-cauplugin)|Recupera información sobre uno o más plug de actualización de software\-complementos que están registrados en el equipo local.|  
|[Register-CauPlugin]((https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin))|Registra un plug de actualización de software CAU\-en el equipo local.|  
|[Unregister-CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/unregister-cauplugin)|Quita un plug de actualización de software\-en la lista de complemento\-complementos que se pueden usar CAU. **Nota:** El enchufe\-que se instalan con CAU \( **Microsoft.WindowsUpdatePlugin** y **Microsoft.HotfixPlugin** \) no se puede anular el registro.|  
  
## <a name="BKMK_WUP"></a>Usar Microsoft.WindowsUpdatePlugin  

El enchufe predeterminada\-de para CAU, **Microsoft.WindowsUpdatePlugin**, realiza las acciones siguientes:
- Se comunica con el Agente de Windows Update en los nodos de los clústeres de conmutación por error con el fin de aplicar las actualizaciones necesarias para los productos de Microsoft que estén en ejecución en cada nodo.
- Instalaciones de clúster actualizaciones directamente desde Windows Update o Microsoft Update o desde un en\-local de Windows Server Update Services \(WSUS\) server.
- Instala solo se selecciona, la versión de distribución general \(GDR\) actualizaciones. De forma predeterminada, el enchufe\-en se aplica solo actualizaciones de software importantes. No es necesario realizar ninguna configuración. La configuración predeterminada descarga e instala actualizaciones GDR importantes en cada nodo. 

> [!NOTE]
> Para aplicar actualizaciones distintas de las actualizaciones de software importantes seleccionadas de manera predeterminada \(por ejemplo, las actualizaciones de controladores\), puede configurar una clavija opcional\-en el parámetro. Para obtener más información, consulta [Configurar la cadena de consulta del Agente de Windows Update](#BKMK_QUERY).

### <a name="requirements"></a>Requisitos

- El clúster de conmutación por error y el equipo Coordinador de actualizaciones remoto \(si usa\) debe cumplir los requisitos para CAU y la configuración necesaria para la administración remota que aparecen en [requisitos y procedimientos recomendados para CAU ](cluster-aware-updating-requirements.md).
- Consulta las [Recomendaciones para aplicar actualizaciones de Microsoft](cluster-aware-updating-requirements.md#BKMK_BP_WUA) y, a continuación, realiza los cambios necesarios en la configuración de Microsoft Update para los nodos del clúster de conmutación por error.
- Para obtener mejores resultados, se recomienda ejecutar Best Practices Analyzer de CAU \(BPA\) para asegurarse de que el entorno de clúster y actualización estén configurados correctamente para aplicar actualizaciones mediante CAU. Para obtener más información, consulta [Probar la preparación para la actualización de CAU](cluster-aware-updating-requirements.md#BKMK_BPA).

> [!NOTE]
> Las actualizaciones que requieren la aceptación de términos de licencia de Microsoft o que requieren la interacción del usuario se excluyen y deben instalarse manualmente.

### <a name="additional-options"></a>Opciones adicionales

Si lo desea, puede especificar el enchufe siguiente\-argumentos de entrada para aumentar o restringir el conjunto de actualizaciones aplicadas por el enchufe\-en:
- Para configurar el enchufe\-en aplicar las actualizaciones recomendadas además de las actualizaciones importantes en cada nodo, en la UI de CAU, en el **opciones adicionales** página, seleccione el **dar recomienda actualiza la misma forma en que Recibo las actualizaciones importantes** casilla de verificación.
<br>Como alternativa, configure el **'IncludeRecommendedUpdates'\='True'** enchufe\-en argumento.
- Para configurar el enchufe\-en para filtrar los tipos de actualizaciones GDR que se aplican a cada nodo del clúster, se puede especificar una cadena de consulta del agente de Windows Update con una **QueryString** enchufe\-en argumento. Para obtener más información, consulta [Configurar la cadena de consulta del Agente de Windows Update](#BKMK_QUERY).

### <a name="BKMK_QUERY"></a>Configurar la cadena de consulta del agente de Windows Update  
Puede configurar un conector\-argumento para el valor predeterminado de enchufe\-en, **Microsoft.WindowsUpdatePlugin**, que consta de un agente de Windows Update \(WUA\) cadena de consulta. Esta instrucción usa la API de WUA para identificar uno o más grupos de actualizaciones de Microsoft que se aplicarán a cada nodo, según criterios de selección específicos. Puedes combinar varios criterios mediante un AND lógico o mediante un OR lógico. La cadena de consulta WUA se especifica en un enchufe\-argumento como sigue:  
  
**Cadena de consulta\="Criterio1\=Value1 y\/o criterio2\=Value2 y\/o …"**  
  
Por ejemplo, **Microsoft.WindowsUpdatePlugin** selecciona automáticamente actualizaciones importantes mediante un argumento **QueryString** predeterminado que se construye con los criterios **IsInstalled**, **Type**, **IsHidden** y **IsAssigned**:  
  
**Cadena de consulta\="IsInstalled\=0 y el tipo\="Software"y IsHidden\=0 y IsAssigned\=1"**  
  
Si especifica un **QueryString** argumento, se usa en lugar del predeterminado **QueryString** que está configurado para el enchufe\-en.  
  
#### <a name="example-1"></a>Ejemplo 1
  
Para configurar un **QueryString** argumento que instale una actualización concreta identificada por el Id. de *f6ce46c1\-971c\-43f9\-a2aa\-783df125f003*:  
  
**Cadena de consulta\="UpdateID\=' f6ce46c1\-971c\-43f9\-a2aa\-783df125f003' y IsInstalled\=0"**  
  
> [!NOTE]  
> El ejemplo anterior es válido para aplicar actualizaciones mediante el clúster\-compatible con el Asistente de la actualización. Si desea instalar una actualización específica mediante la configuración de self\-opciones de actualización con la UI de CAU o mediante el **agregar\-CauClusterRole** o **establecer\-CauClusterRole**Cmdlet de PowerShell, debe dar formato al valor UpdateID con solo dos\-caracteres de comilla:  
>   
> **Cadena de consulta\="UpdateID\='' f6ce46c1\-971c\-43f9\-a2aa\-783df125f003'' y IsInstalled\=0"**  
  
#### <a name="example-2"></a>Ejemplo 2
  
Para configurar un argumento **QueryString** que solo instale controladores:  
  
**Cadena de consulta\="IsInstalled\=0 y el tipo\='Controlador' y IsHidden\=0"**  
  
Para obtener más información acerca de las cadenas de consulta predeterminado para el enchufe\-en, **Microsoft.WindowsUpdatePlugin**, los criterios de búsqueda \(como **IsInstalled**\), y la sintaxis que se pueden incluir en las cadenas de consulta, vea la sección sobre criterios de búsqueda en el [referencia de API de Windows Update Agent (WUA)](https://go.microsoft.com/fwlink/p/?LinkId=223304).  
  
## <a name="BKMK_HFP"></a>Usar Microsoft.HotfixPlugin  
El enchufe\-en **Microsoft.HotfixPlugin** puede utilizarse para aplicar la versión de distribución limitada de Microsoft \(LDR\) actualizaciones \(también se denomina revisiones y anteriormente denominada QFE\) que descargue de manera independiente para solucionar problemas de software de Microsoft específicos. El complemento instala las actualizaciones desde una carpeta raíz de un recurso compartido de archivos SMB y también se pueden personalizar para aplicar no\-las actualizaciones de BIOS, firmware y controlador de Microsoft.

> [!NOTE]
> Revisiones a veces están disponibles para su descarga de Microsoft en los artículos de Knowledge Base, pero también se proporcionan a los clientes en un objeto como\-necesario.

### <a name="requirements"></a>Requisitos

- El clúster de conmutación por error y el equipo Coordinador de actualizaciones remoto \(si usa\) debe cumplir los requisitos para CAU y la configuración necesaria para la administración remota que aparecen en [requisitos y procedimientos recomendados para CAU ](cluster-aware-updating-requirements.md).
- Consulta [Recomendaciones para usar Microsoft.HotfixPlugin](cluster-aware-updating-requirements.md#BKMK_BP_HF).
- Para obtener mejores resultados, se recomienda ejecutar Best Practices Analyzer de CAU \(BPA\) modelo para asegurarse de que el entorno de clúster y actualización estén configurados correctamente para aplicar actualizaciones mediante CAU. Para obtener más información, consulta [Probar la preparación para la actualización de CAU](cluster-aware-updating-requirements.md#BKMK_BPA).
- Obtener las actualizaciones desde el publicador y copiarlos o extráigalos en un bloque de mensajes del servidor \(SMB\) recurso compartido de archivos \(carpeta raíz de revisiones\) que admita como mínimo SMB 2.0 y que sea accesible para todo el clúster los nodos y el equipo Coordinador de actualizaciones remoto \(si se usa CAU en remoto\-modo de actualización\). Para obtener más información, consulta [Configurar una estructura de carpetas raíz de revisiones](#BKMK_HF_ROOT) más adelante en este tema. 

    > [!NOTE]
    > De forma predeterminada, este plug\-en solo instala revisiones con las siguientes extensiones de nombre de archivo: .msu, .msi y .msp.

- Copie el archivo DefaultHotfixConfig.xml \(que se proporciona en el **% systemroot %\\System32\\WindowsPowerShell\\v1.0\\módulos\\ ClusterAwareUpdating** carpeta en un equipo donde se instalan las herramientas de CAU\) a la carpeta raíz de revisiones que creó y en la que extrajo las revisiones. Por ejemplo, copie el archivo de configuración para  *\\ \\MyFileServer\\revisiones\\raíz\\*. 

    > [!NOTE]
    > Para instalar la mayoría de revisiones proporcionadas por Microsoft y otras actualizaciones, se puede usar el archivo de configuración de revisiones predeterminado sin modificaciones. Si tu escenario lo requiere, puedes personalizar el archivo de configuración como tarea avanzada. El archivo de configuración puede incluir reglas personalizadas, por ejemplo, para manipular archivos de revisión que tienen extensiones de archivo específicas o para definir los comportamientos de condiciones de salida específicas. Para obtener más información, consulta [Personalizar el archivo de configuración de revisiones](#BKMK_CONFIG_FILE) más adelante en este tema.

### <a name="configuration"></a>Configuración

Configura las siguientes opciones. Para obtener más información, consulta los vínculos a las secciones más adelante en este tema.
- La ruta de acceso a la carpeta raíz de revisiones compartida que contiene las actualizaciones que se aplicarán y el archivo de configuración de revisiones. Puede escribir esta ruta de acceso en la UI de CAU o configurar la **HotfixRootFolderPath\=\<ruta de acceso >** PowerShell plug\-en argumento. 

   > [!NOTE]
   > Puede especificar la carpeta raíz de revisiones como una ruta de acceso de carpeta local o como una ruta de acceso UNC del formulario  *\\ \\ServerName\\Share\\RootFolderName*. Un dominio\-se puede usar la ruta de acceso de Namespace DFS independiente o en. Sin embargo, el enchufe\-en características que comprueban el acceso a los permisos en el archivo de configuración de revisiones son incompatibles con una ruta de acceso DFS Namespace, por lo que si configura uno, debe deshabilitar la comprobación de permisos de acceso mediante el uso de la UI de CAU o mediante la configuración de la **DisableAclChecks\='True'** enchufe\-en argumento.
- Carpeta compartida de configuración en el servidor que hospeda la carpeta raíz de revisiones para comprobar los permisos adecuados acceder a la carpeta y garantizar la integridad de los datos que se obtiene acceso desde el SMB \(firma SMB o el cifrado SMB\). Para obtener más información, consulte [Restringir el acceso a la carpeta raíz de revisiones](#BKMK_ACL).

### <a name="additional-options"></a>Opciones adicionales

- Opcionalmente, configure el enchufe\-en lo que el cifrado SMB se aplica al acceso a datos desde el recurso compartido de archivos de revisión. En la UI de CAU, en el **opciones adicionales** página, seleccione el **exigir el cifrado SMB al tener acceso a la carpeta raíz de revisiones** o configure el **RequireSMBEncryption\= 'True'** PowerShell plug\-en argumento. 
  > [!IMPORTANT]
  > Debes realizar pasos de configuración adicionales en el servidor SMB para habilitar la integridad de datos SMB con la firma o el cifrado SMB. Para obtener más información, consulta el paso 4 de [Restringir el acceso a la carpeta raíz de revisiones](#BKMK_ACL). Si seleccionas la opción para exigir el uso del cifrado SMB y la carpeta raíz de revisiones no está configurada para el acceso mediante el cifrado SMB, la ejecución de actualización generará un error.
- Si lo deseas, puedes deshabilitar las comprobaciones predeterminadas de permisos suficientes para la carpeta raíz de revisiones y el archivo de configuración de revisiones. En la UI de CAU, seleccione **Deshabilitar comprobación del acceso de administrador en el archivo de configuración y la carpeta de raíz de revisiones**, o configurar el **DisableAclChecks\='True'** enchufe\-en argumento.
- Opcionalmente, configure el **HotfixInstallerTimeoutMinutes\= <Integer>**  enchufe de argumento para especificar cuánto tiempo la revisión\-en espera a que el proceso del instalador de revisiones devolver. \(El valor predeterminado es 30 minutos.\) Por ejemplo, para especificar un período de tiempo de espera de dos horas, establezca **HotfixInstallerTimeoutMinutes\=120**.
- Opcionalmente, configure el **HotfixConfigFileName \= <name>**  enchufe\-en el argumento para especificar un nombre para el archivo de configuración de revisiones que se encuentra en la carpeta raíz de revisiones. Si no se especifica, se usa el nombre predeterminado DefaultHotfixConfig.xml.
  
### <a name="BKMK_HF_ROOT"></a>Configurar una estructura de carpetas raíz de revisiones

Para la revisión enchufe\-en para trabajar, las revisiones deben almacenarse en un área\-estructura definida en un recurso compartido de archivos SMB \(carpeta raíz de revisiones\), y debe configurar el conector de revisión\-con la ruta de acceso a la carpeta raíz de revisiones mediante el uso de la UI de CAU o los cmdlets de PowerShell de CAU. Esta ruta de acceso se pasa al enchufe\-en como el **HotfixRootFolderPath** argumento. Puedes elegir una de varias estructuras para la carpeta raíz de revisiones, en función de las necesidades de actualización, como se muestra en los siguientes ejemplos. Los archivos o carpetas que no siguen la estructura se ignoran.  
  
#### <a name="example-1---folder-structure-used-to-apply-hotfixes-to-all-cluster-nodes"></a>Ejemplo 1: estructura de carpetas usada para aplicar revisiones a todos los nodos del clúster
  
Para especificar qué revisiones se aplican a todos los nodos del clúster, cópielas en una carpeta denominada **CAUHotfix\_todas** bajo la carpeta raíz de revisiones. En este ejemplo, el **HotfixRootFolderPath** enchufe\-en el argumento está establecido en *\\ \\MyFileServer\\revisiones\\raíz\\*. El **CAUHotfix\_todas** carpeta contiene tres actualizaciones con las extensiones .msu, .msi y .msp que se aplicarán a todos los nodos del clúster. Los nombres de los archivos de actualización solo tienen fines ilustrativos.  
  
> [!NOTE]  
> En este y los ejemplos siguientes, el archivo de configuración de revisiones con su nombre predeterminado DefaultHotfixConfig.xml se muestra en su ubicación requerida en la carpeta raíz de revisiones.  
  
```
\\MyFileServer\Hotfixes\Root\   
   DefaultHotfixConfig.xml  
   CAUHotfix_All\   
      Update1.msu   
      Update2.msi   
      Update3.msp  
      ...  
```  
  
#### <a name="example-2---folder-structure-used-to-apply-certain-updates-only-to-a-specific-node"></a>Ejemplo 2: estructura de carpetas que se usa para aplicar ciertas actualizaciones solo a un nodo específico
  
Para especificar revisiones que se aplican solo a un nodo específico, usa una subcarpeta de la carpeta raíz de revisiones con el nombre del nodo. Usa el nombre NetBIOS del nodo del clúster, por ejemplo, *ContosoNode1*. A continuación, mueve las actualizaciones que solo se aplican a este nodo a esta subcarpeta. En el ejemplo siguiente, la **HotfixRootFolderPath** enchufe\-en el argumento está establecido en  *\\ \\MyFileServer\\revisiones\\raíz\\*. Las actualizaciones en el **CAUHotfix\_todos los** carpeta se aplicarán a todos los nodos del clúster y *Node1\_específico\_Update.msu* se aplicará sólo a *ContosoNode1*.  
  
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
  
#### <a name="example-3---folder-structure-used-to-apply-updates-other-than-msu-msi-and-msp-files"></a>Ejemplo 3: estructura de carpetas que se usa para aplicar las actualizaciones que no sean archivos .msu, .msi y .msp
  
De manera predeterminada, **Microsoft.HotfixPlugin** solo aplica actualizaciones con las extensiones .msu, .msi o .msp. No obstante, ciertas actualizaciones podrían tener extensiones diferentes y requerir comandos de instalación distintos. Por ejemplo, es posible que tengas que aplicar una actualización de firmware con la extensión .exe a un nodo del clúster. Puede configurar la carpeta raíz de revisiones con una subcarpeta que indique un determinado, no\-debe instalarse el tipo de actualización predeterminado. También debes configurar una regla de instalación de carpeta correspondiente que especifique el comando de instalación en el elemento `<FolderRules>` del archivo XML de configuración de revisiones.  
  
En el ejemplo siguiente, la **HotfixRootFolderPath** enchufe\-en el argumento está establecido en  *\\ \\MyFileServer\\revisiones\\raíz\\*. Se aplicarán varias actualizaciones a todos los nodos del clúster y se aplicará una actualización de firmware *SpecialHotfix1.exe* a *ContosoNode1* mediante *FolderRule1*. Para obtener información sobre la configuración de *FolderRule1* en el archivo de configuración de revisiones, consulte [Personalizar el archivo de configuración de revisiones](#BKMK_CONFIG_FILE) más adelante en este tema.  
  
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

### <a name="BKMK_CONFIG_FILE"></a>Personalizar el archivo de configuración de revisiones  
El archivo de configuración de revisiones controla cómo **Microsoft.HotfixPlugin** instala tipos de archivo de revisión específicos en un clúster de conmutación por error. El esquema XML para el archivo de configuración se define en HotfixConfigSchema.xsd, que está ubicado en la siguiente carpeta en un equipo con las herramientas CAU instaladas:  
  
**% systemroot %\\System32\\WindowsPowerShell\\v1.0\\módulos\\ClusterAwareUpdating carpeta**  
  
Para personalizar el archivo de configuración de revisiones, copia el archivo de configuración de ejemplo DefaultHotfixConfig.xml desde esta ubicación a la carpeta raíz de revisiones y realiza las modificaciones apropiadas para tu escenario.  
  
> [!IMPORTANT]  
> Para aplicar la mayoría de revisiones proporcionadas por Microsoft y otras actualizaciones, se puede usar el archivo de configuración de revisiones predeterminado sin modificaciones. La personalización del archivo de configuración de revisiones es una tarea solo en escenarios de uso avanzados.  
  
De manera predeterminada, el archivo XML de configuración de revisiones define reglas de instalación y condiciones de salida para las dos categorías de revisiones siguientes:  
  
-   Los archivos de revisión con extensiones que el enchufe\-en puede instalar de forma predeterminada \(archivos .msu, .msi y .msp\).  
  
    Se definen como elementos `<ExtensionRules>` en el elemento `<DefaultRules>`. Hay un elemento `<Extension>` para cada uno de los tipos de archivo compatibles predeterminados. La estructura XML general es la siguiente:  
  
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
  
    Si tienes que aplicar ciertos tipos de actualizaciones a todos los nodos de clúster del entorno, puedes definir elementos `<Extension>` adicionales.  
  
-   Revisión u otros actualizar archivos que no son .msi, .msu o .msp archivos, por ejemplo, no\-las actualizaciones de BIOS, firmware y controladores de Microsoft.  
  
    Cada uno de ellos no\-tipo de archivo predeterminado está configurado como un `<Folder>` elemento en el `<FolderRules>` elemento. El atributo de nombre del elemento `<Folder>` debe ser idéntico al nombre de una carpeta en la carpeta raíz de revisiones que contendrá las actualizaciones del tipo correspondiente. La carpeta puede estar en el **CAUHotfix\_todas** carpeta o en un nodo\-carpeta específica. Por ejemplo, si *FolderRule1* se configura en la carpeta raíz de revisiones, configura el elemento siguiente en el archivo XML para definir una plantilla de instalación y condiciones de salida para las actualizaciones en esa carpeta:  
  
    ```xml  
    <FolderRules>  
          <Folder name="FolderRule1">  
            <!-- Template and ExitConditions elements for installation of updates in FolderRule1 follow -->  
             ...  
          </Folder>  
          ...  
    </FolderRules>  
    ```  
  
En las tablas siguientes se describen los atributos `<Template>` y los posibles elementos secundarios `<ExitConditions>` .  
  
|Atributo `<Template>`|Descripción|  
|--------------------------|---------------|  
|`path`|La ruta de acceso completa al programa de instalación del tipo de archivo que se define en el atributo `<Extension name>` .<br /><br />Para especificar la ruta de acceso a un archivo de actualización en la estructura de la carpeta raíz de revisiones, use `$update$`.|  
|`parameters`|Una cadena de parámetros obligatorios y opcionales para el programa que se especifica en `path`.<br /><br />Para especificar un parámetro que sea la ruta de acceso a un archivo de actualización en la estructura de la carpeta raíz de revisiones, use `$update$`.|  
  
|Elemento secundario `<ExitConditions>`|Descripción|  
|---------------------------------|---------------|  
|`<Success>`|Define uno o más códigos de salida que indican que la actualización en cuestión se ha realizado correctamente. Es un elemento secundario obligatorio.|  
|`<Success_RebootRequired>`|De manera opcional, define uno o más códigos de salida que indican que la actualización en cuestión se ha realizado correctamente y que el nodo se debe reiniciar. <br>**Nota:** De manera opcional, el elemento `<Folder>` puede contener el atributo `alwaysReboot`. Si se establece este atributo, indica que si una revisión instalada por esta regla devolverá uno de los códigos de salida definidos en `<Success>`, se interpreta como condición de salida `<Success_RebootRequired>` .|  
|`<Fail_RebootRequired>`|De manera opcional, define uno o más códigos de salida que indican que la actualización en cuestión se ha realizado de manera incorrecta y que el nodo se debe reiniciar.|  
|`<AlreadyInstalled>`|De manera opcional, define uno o más códigos de salida que indican que la actualización en cuestión no se ha aplicado porque ya está instalada.|  
|`<NotApplicable>`|De manera opcional, define uno o más códigos de salida que indican que la actualización en cuestión no se ha aplicado porque no se aplica al nodo del clúster.|  
  
> [!IMPORTANT]  
> Los códigos de salida que no se definan de manera explícita en `<ExitConditions>` se interpretan como un error de actualización y el nodo no se reinicia.  
  
### <a name="BKMK_ACL"></a>Restringir el acceso a la carpeta raíz de revisiones  
Debe realizar varios pasos para configurar el servidor de archivos y el recurso compartido de archivos SMB para proteger los archivos de la carpeta raíz de revisiones y el archivo de configuración de revisiones para el acceso solo en el contexto de **Microsoft.HotfixPlugin**. Estos pasos habilitan varias características que ayudan a impedir posibles manipulaciones de los archivos de revisión de manera que se pueda poner en peligro el clúster de conmutación por error.  
  
Los pasos generales son los siguientes:  
  
1.  Identificar la cuenta de usuario que se usa para ejecuciones de actualización mediante el enchufe\-en  
  
2.  Configurar esta cuenta de usuario en los grupos necesarios en el servidor de archivos SMB  
  
3.  Configurar permisos para tener acceso a la carpeta raíz de revisiones  
  
4.  Configurar las opciones de integridad de datos SMB  
  
5.  Habilitar una regla de Firewall de Windows en el servidor SMB  
  
#### <a name="step-1-identify-the-user-account-that-is-used-for-updating-runs-by-using-the-hotfix-plug-in"></a>Paso 1. Identificar la cuenta de usuario que se usa para ejecuciones de actualización mediante el conector de revisión\-en
  
La cuenta que se usa en CAU para comprobar la configuración de seguridad mientras se realiza una ejecución de actualización con **Microsoft.HotfixPlugin** depende de si se usa CAU en remoto\-actualizar modo o self\-actualización modo, como se indica a continuación:  
  
-   **Remoto\-modo de actualización** la cuenta que tenga privilegios administrativos en el clúster para obtener una vista previa y aplicar las actualizaciones.  
  
-   **Self\-modo de actualización** rol en clúster el nombre del objeto de equipo virtual que está configurado en Active Directory para la CAU. Es el nombre de un objeto de equipo virtual preconfigurado en Active Directory para el rol en clúster de CAU o el nombre que CAU genera para el rol en clúster. Para obtener el nombre si lo genera CAU, ejecute el **obtener\-CauClusterRole** cmdlet de PowerShell de CAU. En la salida, **ResourceGroupName** es el nombre de la cuenta de objeto de equipo virtual generada.  
  
#### <a name="step-2-configure-this-user-account-in-the-necessary-groups-on-an-smb-file-server"></a>Paso 2. Configurar esta cuenta de usuario en los grupos necesarios en el servidor de archivos SMB
  
> [!IMPORTANT]  
> Debes agregar la cuenta que se usa para ejecuciones de actualización como cuenta de administrador local en el servidor SMB. Si las directivas de seguridad de tu organización no lo permiten, configura esta cuenta con los permisos necesarios en el servidor SMB mediante el procedimiento siguiente.  
  
##### <a name="to-configure-a-user-account-on-the-smb-server"></a>Para configurar una cuenta de usuario en el servidor SMB  
  
1.  Agrega la cuenta que se usa para las ejecuciones de actualización al grupo Usuarios COM distribuidos y a uno de los grupos siguientes: Usuario avanzado, Operación de servidor u Operador de impresión.  
  
2.  Para habilitar los permisos de WMI necesarios para la cuenta, inicia la consola de administración de WMI en el servidor SMB. Inicie PowerShell y, a continuación, escriba el siguiente comando:  
  
    ```  
    wmimgmt.msc  
    ```  
  
3.  En el árbol de consola, a la derecha\-haga clic en **Control WMI \(Local\)** y, a continuación, haga clic en **propiedades**.  
  
4.  Haz clic en **Seguridad** y, a continuación, expande **Raíz**.  
  
5.  Haz clic en **CIMV2**y, a continuación, haz clic en **Seguridad**.  
  
6.  Agrega la cuenta que se usa para las actualizaciones de ejecución a la lista **Nombres de grupos o usuarios**.  
  
7.  Concede los permisos **Ejecutar métodos** y **Llamada remota habilitada** a la cuenta que se usa para las ejecuciones de actualización.  
  
#### <a name="step-3-configure-permissions-to-access-the-hotfix-root-folder"></a>Paso 3. Configurar permisos para tener acceso a la carpeta raíz de revisiones
  
De forma predeterminada, cuando se intenta aplicar actualizaciones, conecte la revisión\-en comprueba la configuración de los permisos del sistema de archivos NTFS para el acceso a la carpeta raíz de revisiones. Si los permisos de acceso de carpeta no están configurados correctamente, una ejecución de actualización mediante el conector de revisión\-en podría producir un error.  
  
Si usa la configuración predeterminada de la clavija de revisión\-, asegúrese de que los permisos de acceso de carpeta cumplen los requisitos siguientes.  
  
-   El grupo Usuarios tiene permiso de lectura.  
  
-   Si el enchufe\-en aplicará actualizaciones con la extensión .exe, el grupo usuarios tiene el permiso Execute.  
  
-   Se permiten solo ciertas entidades de seguridad \(pero no son necesarios\) tener permiso de escritura o modificar los permisos. Las entidades de seguridad permitidas son el grupo Administradores local, SYSTEM, CREATOR OWNER y TrustedInstaller. No se permite a otras cuentas o grupos tener permisos de escritura o modificación en la carpeta raíz de revisiones.  
  
Si lo desea, puede deshabilitar las comprobaciones anteriores que el enchufe\-en realiza de forma predeterminada. Puedes hacerlo de dos maneras distintas:  
  
-   Si usas los cmdlets de PowerShell de CAU, configure el **DisableAclChecks\='True'** argumento en el **CauPluginArguments** parámetro para el conector de revisión\-en.  
  
-   Si usas la interfaz de usuario de CAU, selecciona la opción **Deshabilitar comprobación del acceso del administrador a la carpeta raíz de revisiones y el archivo de configuración** en la página **Opciones de actualización adicionales** del asistente que se usa para configurar las opciones de la ejecución de actualización.  
  
No obstante, como procedimiento recomendado en muchos entornos, recomendamos usar la configuración predeterminada para exigir estas comprobaciones.  
  
#### <a name="step-4-configure-settings-for-smb-data-integrity"></a>Paso 4. Configurar las opciones de integridad de datos SMB
  
Para comprobar la integridad de datos en las conexiones entre los nodos del clúster y el recurso compartido de archivos SMB, conecte la revisión\-en requiere que habilite la configuración en el recurso compartido de archivos SMB para la firma SMB o el cifrado SMB. Cifrado de SMB, que ofrece seguridad mejorada y un mejor rendimiento en muchos entornos, se admite a partir de Windows Server 2012. Puedes habilitar una o ambas configuraciones, de la manera siguiente:  
  
-   Para habilitar la firma SMB, consulte el procedimiento del [artículo 887429](https://support.microsoft.com/kb/887429) en Microsoft Knowledge Base.  
  
-   Para habilitar el cifrado SMB para la carpeta compartida SMB, ejecute el siguiente cmdlet de PowerShell en el servidor SMB:  
  
    ```PowerShell  
    Set-SmbShare <ShareName> -EncryptData $true  
    ```  
  
    Donde <*ShareName*> es el nombre de la carpeta compartida SMB.  
  
Opcionalmente, para exigir el uso del cifrado SMB en las conexiones al servidor SMB, seleccione el **exigir el cifrado SMB al tener acceso a la carpeta raíz de revisiones** en la UI de CAU o configure el **RequireSMBEncryption \='True'** enchufe\-argumento mediante los cmdlets de PowerShell de CAU.  
  
> [!IMPORTANT]  
> Si seleccionas la opción para exigir el uso del cifrado SMB y la carpeta raíz de revisiones no está configurada para las conexiones que usan el cifrado SMB, la ejecución de actualización generará un error.  
  
#### <a name="step-5-enable-a-windows-firewall-rule-on-the-smb-server"></a>Paso 5. Habilitar una regla de Firewall de Windows en el servidor SMB
  
Debe habilitar el **administración remota del servidor de archivos \(SMB\-en\)**  regla de Firewall de Windows en el servidor de archivos SMB. Esta opción está habilitada de forma predeterminada en Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012.  
  
## <a name="see-also"></a>Vea también  
  
-   [Introducción a la actualización compatible con clústeres](cluster-aware-updating.md)
  
-   [Cmdlets de PowerShell de Windows actualización compatible con clústeres](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating)  
  
-   [Compatible con clústeres referencia del complemento de actualización](https://msdn.microsoft.com/library/hh418084.aspx)  
  
