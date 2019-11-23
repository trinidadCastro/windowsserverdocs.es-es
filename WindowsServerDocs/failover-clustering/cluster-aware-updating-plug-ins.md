---
ms.assetid: d44d4430-41e2-483a-9685-92610cdef32a
title: Cómo funcionan los complementos de actualización compatible con clústeres
ms.topic: article
ms.prod: windows-server
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 04/28/2017
ms.technology: storage-failover-clustering
description: Cómo usar complementos para coordinar las actualizaciones cuando se usa la actualización compatible con clústeres en Windows Server para instalar actualizaciones en un clúster.
ms.openlocfilehash: f6c572a397530704dd91d9c67c5c1758ccc085c4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361286"
---
# <a name="how-cluster-aware-updating-plug-ins-work"></a>Cómo funcionan los complementos de actualización compatible con clústeres

>Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

La [actualización compatible con clústeres](cluster-aware-updating.md) (CAU) usa complementos para coordinar la instalación de actualizaciones en los nodos de un clúster de conmutación por error. En este tema se proporciona información sobre el uso de los\-integrados en los\-de complementos de CAU u otros complementos de\-que se instalan para la CAU.

## <a name="BKMK_INSTALL"></a>Instalar un\-de plug-in  
Un\-de conexión que no sea el\-de plug-ins predeterminado que se instalan con CAU \(**Microsoft. WindowsUpdatePlugin** y **microsoft. HotfixPlugin**\) debe instalarse por separado. Si se usa la CAU en el modo de actualización de\-, el\-de plug-in debe estar instalado en todos los nodos del clúster. Si se usa la CAU en el modo de actualización de\-remoto, el\-de plug-in debe estar instalado en el equipo coordinador de actualizaciones remotas. Un\-de conexión en que instale puede tener requisitos de instalación adicionales en cada nodo.  
  
Para instalar un complemento\-en, siga las instrucciones de la\-de plug-in del publicador. Para registrar manualmente un\-de plug-in con CAU, ejecute el cmdlet [Register-CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin) en cada equipo donde esté instalado el\-de plug-in.  
  
## <a name="specify-a-plug-in-and-plug-in-arguments"></a>Especificar un\-de conexión en y\-de los argumentos  
  
### <a name="specify-a-cau-plug-in"></a>Especificar un\-de conexión de CAU en

En la interfaz de usuario de CAU, seleccione un\-de plug-in en una lista desplegable\-de los complementos de\-disponibles al usar la CAU para realizar las siguientes acciones:  
  
-   Aplicar actualizaciones al clúster  
  
-   Obtener una vista previa de las actualizaciones del clúster  
  
-   Configuración de las opciones de actualización de\-de clúster  
  
De forma predeterminada, la CAU selecciona la\-de conexión de **Microsoft. WindowsUpdatePlugin**. Sin embargo, puede especificar cualquier\-de conexión de que esté instalado y registrado con CAU.

> [!TIP]  
> En la interfaz de usuario de CAU, solo puede especificar un único\-de conexión para que la CAU lo use para obtener una vista previa o para aplicar actualizaciones durante una ejecución de actualización. Mediante el uso de los cmdlets de PowerShell de CAU, puede especificar uno o más complementos de\-. Si necesita instalar varios tipos de actualizaciones en el clúster, normalmente es más eficaz especificar varios complementos de\-en una ejecución de actualización, en lugar de usar una ejecución de actualización independiente para cada\-de plug-in. Por ejemplo, lo normal es que se produzcan menos reinicios de nodos.

Mediante el uso de los cmdlets de PowerShell de CAU que se enumeran en la tabla siguiente, puede especificar uno o varios\-de plug-ins para una ejecución de actualización o un examen pasando el parámetro **– CauPluginName** . Puede especificar varios\-de conexión en nombres separándolos con comas. Si se especifican varios complementos de\-, también se puede controlar el modo en que los\-ins influyen entre sí durante una ejecución de actualización especificando los parámetros **\-RunPluginsSerially**, **\-StopOnPluginFailure**y **– SeparateReboots** . Para obtener más información acerca del uso de varios complementos de\-, use los vínculos proporcionados a la documentación del cmdlet en la tabla siguiente.  
  
|Cmdlet|Descripción|  
|----------|---------------|  
|[Add-CauClusterRole](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/add-cauclusterrole)|Agrega el rol en clúster de CAU que proporciona la funcionalidad de actualización de\-propio al clúster especificado.|  
|[Invoke-CauRun](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/invoke-caurun)|Realiza un examen de los nodos del clúster para buscar actualizaciones aplicables y las instala por medio de una ejecución de actualización en el clúster especificado.|  
|[Invoke-CauScan](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/invoke-causcan)|Realiza un examen de los nodos del clúster para buscar actualizaciones aplicables y devuelve una lista del conjunto inicial de actualizaciones que se aplicarían a cada nodo en el clúster especificado.|  
|[Set-CauClusterRole](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/set-cauclusterrole)|Establece las propiedades de configuración del rol en clúster de CAU en el clúster especificado.|  
  
Si no especifica un complemento de CAU\-en el parámetro con estos cmdlets, el valor predeterminado es el\-de conexión de **Microsoft. WindowsUpdatePlugin**.  
  
### <a name="specify-cau-plug-in-arguments"></a>Especificar\-de complemento de CAU en argumentos  
Al configurar las opciones de ejecución de actualización, puede especificar uno o más pares de *nombre\=valor* \(argumentos\) para que los\-de complementos seleccionados en utilicen. Por ejemplo, en la interfaz de usuario de CAU puedes especificar varios argumentos de la siguiente manera:  
  
**Nombre1\=Value1; Nombre2\=Valor2; Name3\=Value3**  
  
Estos pares de *nombre\=valor* deben ser significativos para el\-de plug-in que especifique. En algunos\-de complementos, los argumentos son opcionales.  
  
La sintaxis de la\-de la conexión de CAU en argumentos sigue estas reglas generales:  
  
-   Varios pares de *nombre\=valor* se separan mediante signos de punto y coma.  
  
-   Un valor que contiene espacios está entre comillas, por ejemplo: **nombre1\="Value with Spaces"** .  
  
-   La sintaxis exacta de *Value* depende de la\-de plug in.  
  
Para especificar la\-de plug-in en los argumentos mediante los cmdlets de PowerShell de CAU que admiten el parámetro **– CauPluginParameters** , pase un parámetro con el formato:  
  
**\-CauPluginArguments @ {Nombre1\=Value1; Nombre2\=Valor2; Name3\=Value3}**  
  
También puede usar una tabla hash de PowerShell predefinida. Para especificar\-de plug-in en argumentos para más de un\-de conexión en, pase varias tablas hash de argumentos, separadas por comas. Pase el\-de conexión de los argumentos de la\-de conexión en orden que se especifica en **CauPluginName**.  
  
### <a name="specify-optional-plug-in-arguments"></a>Especificar\-de plug-in opcional en argumentos  
Los\-de complementos que instala CAU \(**Microsoft. WindowsUpdatePlugin** y **microsoft. HotfixPlugin**\) proporcionan opciones adicionales que puede seleccionar. En la interfaz de usuario de CAU, aparecen en una página **opciones adicionales** después de configurar las opciones de ejecución de actualización para el\-de plug-in. Si usa los cmdlets de PowerShell de CAU, estas opciones se configuran como plug\-opcional en argumentos. Para más información, consulte [Usar Microsoft.WindowsUpdatePlugin](#BKMK_WUP) y [Usar Microsoft.HotfixPlugin](#BKMK_HFP) más adelante en este tema.  
  
## <a name="manage-plug-ins-using-windows-powershell-cmdlets"></a>Administrar complementos de\-mediante cmdlets de Windows PowerShell  
  
|Cmdlet|Descripción|  
|----------|---------------|  
|[Get-CauPlugin](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/get-cauplugin)|Recupera información sobre uno o más complementos de\-de actualización de software que están registrados en el equipo local.|  
|[Register-CauPlugin]((https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/register-cauplugin))|Registra un complemento de actualización de software de CAU\-en en el equipo local.|  
|[Unregister-CauPlugin](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/unregister-cauplugin)|Quita un complemento de actualización de software\-de la lista de complementos de\-que puede usar la CAU. **Nota:** No se puede anular el registro de los complementos de\-que se instalan con CAU \(**Microsoft. WindowsUpdatePlugin** y **microsoft. HotfixPlugin**\).|  
  
## <a name="BKMK_WUP"></a>Usar Microsoft. WindowsUpdatePlugin  

La\-de complementos predeterminada de para CAU, **Microsoft. WindowsUpdatePlugin**, realiza las siguientes acciones:
- Se comunica con el Agente de Windows Update en los nodos de los clústeres de conmutación por error con el fin de aplicar las actualizaciones necesarias para los productos de Microsoft que estén en ejecución en cada nodo.
- Instala las actualizaciones del clúster directamente desde Windows Update o Microsoft Update, o desde una\-Windows Server Update Services local \(servidor WSUS\).
- Instala solo la versión de distribución general seleccionada \(GDR\) actualizaciones. De forma predeterminada, la\-plug in solo aplica actualizaciones de software importantes. No es necesario realizar ninguna configuración. La configuración predeterminada descarga e instala actualizaciones GDR importantes en cada nodo. 

> [!NOTE]
> Para aplicar actualizaciones distintas de las actualizaciones de software importantes que están seleccionadas de forma predeterminada \(por ejemplo, las actualizaciones de controladores\), puede configurar un\-de conexión opcional en el parámetro. Para obtener más información, consulta [Configurar la cadena de consulta del Agente de Windows Update](#BKMK_QUERY).

### <a name="requirements"></a>Requisitos

- El clúster de conmutación por error y el equipo coordinador de actualizaciones remotas \(si se usan\) deben cumplir los requisitos de CAU y la configuración necesaria para la administración remota enumerada en [requisitos y prácticas recomendadas para Cau](cluster-aware-updating-requirements.md).
- Consulta las [Recomendaciones para aplicar actualizaciones de Microsoft](cluster-aware-updating-requirements.md#BKMK_BP_WUA) y, a continuación, realiza los cambios necesarios en la configuración de Microsoft Update para los nodos del clúster de conmutación por error.
- Para obtener los mejores resultados, se recomienda ejecutar el Analizador de procedimientos recomendados de CAU \(BPA\) para asegurarse de que el clúster y el entorno de actualización estén configurados correctamente para aplicar actualizaciones mediante CAU. Para obtener más información, consulta [Probar la preparación para la actualización de CAU](cluster-aware-updating-requirements.md#BKMK_BPA).

> [!NOTE]
> Las actualizaciones que requieren la aceptación de términos de licencia de Microsoft o que requieren la interacción del usuario se excluyen y deben instalarse manualmente.

### <a name="additional-options"></a>Opciones adicionales

Opcionalmente, puede especificar los siguientes\-de complementos en argumentos para aumentar o restringir el conjunto de actualizaciones que se aplican mediante el\-de plug-in:
- Para configurar la\-de plug-in para aplicar las actualizaciones recomendadas además de las actualizaciones importantes en cada nodo, en la interfaz de usuario de CAU, en la página **opciones adicionales** , active la casilla **enviarme actualizaciones recomendadas de la misma manera que recibo actualizaciones importantes** .
<br>Como alternativa, configure el\-de ' **IncludeRecommendedUpdates '\=' true '** en el argumento.
- Para configurar la\-de plug-in para filtrar los tipos de actualizaciones GDR que se aplican a cada nodo de clúster, especifique una cadena de consulta del agente de Windows Update mediante un\-**de cadena de consulta en el argumento** . Para obtener más información, consulta [Configurar la cadena de consulta del Agente de Windows Update](#BKMK_QUERY).

### <a name="BKMK_QUERY"></a>Configurar la cadena de consulta del agente de Windows Update  
Puede configurar un\-de plug-in en el argumento para el\-de complementos predeterminado en, **Microsoft. WindowsUpdatePlugin**, que consta de una cadena de consulta de Windows Update Agent \(WUA\). Esta instrucción usa la API de WUA para identificar uno o más grupos de actualizaciones de Microsoft que se aplicarán a cada nodo, según criterios de selección específicos. Puedes combinar varios criterios mediante un AND lógico o mediante un OR lógico. La cadena de consulta de WUA se especifica en un\-de complemento en el argumento de la manera siguiente:  
  
**QueryString\="Criterio1\=Value1 y\/o Criterion2\=value2 y\/o..."**  
  
Por ejemplo, **Microsoft.WindowsUpdatePlugin** selecciona automáticamente actualizaciones importantes mediante un argumento **QueryString** predeterminado que se construye con los criterios **IsInstalled**, **Type**, **IsHidden** y **IsAssigned**:  
  
**QueryString\="IsInstalled\=0 and Type\=" software "y IsHidden\=0 y IsAssigned\=1"**  
  
Si especifica un argumento **QueryString** , se usa en lugar de la **cadena** de configuración predeterminada que se configura para el\-de complemento en.  
  
#### <a name="example-1"></a>Ejemplo 1
  
Para configurar un argumento **QueryString** que instale una actualización concreta como identificada por el identificador *f6ce46c1\-971c\-43f9\-a2aa\-783df125f003*:  
  
**QueryString\="UpdateID\=' f6ce46c1\-971C\-43f9\-a2aa\-783df125f003 ' y IsInstalled\=0"**  
  
> [!NOTE]  
> El ejemplo anterior es válido para aplicar actualizaciones mediante el Asistente para la actualización compatible con\-de clúster. Si desea instalar una actualización específica mediante la configuración de las opciones de actualización de auto\-con la interfaz de usuario de CAU o mediante el cmdlet **Add\-CauClusterRole** o **set\-CauClusterRole**de PowerShell, debe dar formato al valor UpdateID con dos caracteres de comilla de\-única:  
>   
> **QueryString\="UpdateID\=' ' f6ce46c1\-971C\-43f9\-a2aa\-783df125f003 ' ' y IsInstalled\=0"**  
  
#### <a name="example-2"></a>Ejemplo 2
  
Para configurar un argumento **QueryString** que solo instale controladores:  
  
**QueryString\="IsInstalled\=0 y Type\=" driver "y IsHidden\=0"**  
  
Para obtener más información sobre las cadenas de consulta para el\-de plug-in predeterminado en, **Microsoft. WindowsUpdatePlugin**, los criterios de búsqueda \(como **IsInstalled**\)y la sintaxis que puede incluir en las cadenas de consulta, consulte la sección sobre los criterios de búsqueda en la referencia de la [API del agente de Windows Update (WUA)](https://go.microsoft.com/fwlink/p/?LinkId=223304).  
  
## <a name="BKMK_HFP"></a>Usar Microsoft. HotfixPlugin  
La\-de conexión de **Microsoft. HotfixPlugin** se puede usar para aplicar versiones de distribución limitada de Microsoft \(LDR\) actualizaciones \(también denominadas revisiones y anteriormente llamadas QFE\) que se descargan de forma independiente para solucionar problemas de software de Microsoft específicos. El complemento instala las actualizaciones desde una carpeta raíz en un recurso compartido de archivos SMB y también se puede personalizar para aplicar actualizaciones de controladores, firmware y BIOS que no sean de\-Microsoft.

> [!NOTE]
> A veces, las revisiones están disponibles para su descarga de Microsoft en los artículos de Knowledge base, pero también se proporcionan a los clientes según\-necesiten.

### <a name="requirements"></a>Requisitos

- El clúster de conmutación por error y el equipo coordinador de actualizaciones remotas \(si se usan\) deben cumplir los requisitos de CAU y la configuración necesaria para la administración remota enumerada en [requisitos y prácticas recomendadas para Cau](cluster-aware-updating-requirements.md).
- Consulta [Recomendaciones para usar Microsoft.HotfixPlugin](cluster-aware-updating-requirements.md#BKMK_BP_HF).
- Para obtener los mejores resultados, se recomienda ejecutar la Analizador de procedimientos recomendados de CAU \(modelo de\) BPA para asegurarse de que el clúster y el entorno de actualización estén configurados correctamente para aplicar actualizaciones mediante CAU. Para obtener más información, consulta [Probar la preparación para la actualización de CAU](cluster-aware-updating-requirements.md#BKMK_BPA).
- Obtenga las actualizaciones del publicador y cópielos o extráigalos en un bloque de mensajes del servidor \(SMB\) recurso compartido de archivos \(la carpeta raíz de revisiones\) que admita al menos SMB 2,0 y que sea accesible para todos los nodos del clúster y el equipo coordinador de actualizaciones remotas \(si se usa la CAU en el modo de actualización de\-remoto\). Para obtener más información, consulta [Configurar una estructura de carpetas raíz de revisiones](#BKMK_HF_ROOT) más adelante en este tema. 

    > [!NOTE]
    > De forma predeterminada, esta\-de conexión solo instala revisiones con las siguientes extensiones de nombre de archivo:. msu,. msi y. msp.

- Copie el archivo DefaultHotfixConfig. XML \(que se proporciona en la carpeta **% systemroot%\\System32\\WindowsPowerShell\\v 1.0\\módulos\\ClusterAwareUpdating** de un equipo en el que se instalan las herramientas de Cau\) en la carpeta raíz de revisiones que creó y en la que extrajo las revisiones. Por ejemplo, copie el archivo de configuración en *\\\\de las revisiones\\\\de\\raíz* . 

    > [!NOTE]
    > Para instalar la mayoría de revisiones proporcionadas por Microsoft y otras actualizaciones, se puede usar el archivo de configuración de revisiones predeterminado sin modificaciones. Si tu escenario lo requiere, puedes personalizar el archivo de configuración como tarea avanzada. El archivo de configuración puede incluir reglas personalizadas, por ejemplo, para manipular archivos de revisión que tienen extensiones de archivo específicas o para definir los comportamientos de condiciones de salida específicas. Para obtener más información, consulta [Personalizar el archivo de configuración de revisiones](#BKMK_CONFIG_FILE) más adelante en este tema.

### <a name="configuration"></a>Configuración

Configura las siguientes opciones. Para obtener más información, consulta los vínculos a las secciones más adelante en este tema.
- La ruta de acceso a la carpeta raíz de revisiones compartida que contiene las actualizaciones que se aplicarán y el archivo de configuración de revisiones. Puede escribir esta ruta de acceso en la interfaz de usuario de CAU o configurar la **ruta de acceso de HotfixRootFolderPath\=\<>** PowerShell\-en el argumento. 

   > [!NOTE]
   > Puede especificar la carpeta raíz de revisiones como una ruta de carpeta local o como una ruta de acceso UNC con el formato *\\\\nombreservidor\\recurso compartido\\RootFolderName*. Se puede usar una ruta de acceso de espacio de nombres DFS independiente o basada en\-de dominio. Sin embargo, el\-de conexión de las características que comprueban los permisos de acceso en el archivo de configuración de revisiones son incompatibles con una ruta de acceso de espacio de nombres DFS, por lo que si configura uno, debe deshabilitar la comprobación de los permisos de acceso mediante la interfaz de usuario de CAU o configurando el\-de **DisableAclChecks\=' true '** en el argumento.
- Configuración del servidor que hospeda la carpeta raíz de revisiones para comprobar los permisos adecuados para tener acceso a la carpeta y garantizar la integridad de los datos a los que se tiene acceso desde la carpeta compartida SMB \(la firma SMB o el cifrado SMB\). Para obtener más información, consulte [Restringir el acceso a la carpeta raíz de revisiones](#BKMK_ACL).

### <a name="additional-options"></a>Opciones adicionales

- Opcionalmente, configure la\-de complementos en para que se aplique el cifrado SMB al tener acceso a los datos del recurso compartido de archivos de la revisión. En la interfaz de usuario de CAU, en la página **opciones adicionales** , seleccione la opción **requerir cifrado SMB al tener acceso a la carpeta raíz de revisiones** o configure el\-de **RequireSMBEncryption\=' true '** de PowerShell en el argumento. 
  > [!IMPORTANT]
  > Debes realizar pasos de configuración adicionales en el servidor SMB para habilitar la integridad de datos SMB con la firma o el cifrado SMB. Para obtener más información, consulta el paso 4 de [Restringir el acceso a la carpeta raíz de revisiones](#BKMK_ACL). Si seleccionas la opción para exigir el uso del cifrado SMB y la carpeta raíz de revisiones no está configurada para el acceso mediante el cifrado SMB, la ejecución de actualización generará un error.
- Si lo deseas, puedes deshabilitar las comprobaciones predeterminadas de permisos suficientes para la carpeta raíz de revisiones y el archivo de configuración de revisiones. En la interfaz de usuario de CAU, seleccione **deshabilitar comprobar el acceso de administrador a la carpeta raíz de revisiones y el archivo de configuración**, o configure el\-**DisableAclChecks\=' true '** en el argumento.
- Opcionalmente, configure el argumento **argumento hotfixinstallertimeoutminutes\=<Integer>** para especificar cuánto tiempo espera el complemento de la revisión\-en esperar a que el proceso del instalador de revisiones devuelva. \(el valor predeterminado es 30 minutos.\) por ejemplo, para especificar un período de tiempo de espera de dos horas, establezca **argumento hotfixinstallertimeoutminutes\=120**.
- Opcionalmente, configure el **\= HotfixConfigFileName <name>** \-en el argumento para especificar un nombre para el archivo de configuración de revisiones que se encuentra en la carpeta raíz de revisiones. Si no se especifica, se usa el nombre predeterminado DefaultHotfixConfig.xml.
  
### <a name="BKMK_HF_ROOT"></a>Configurar una estructura de carpetas raíz de revisiones

Para que el complemento de revisiones\-de funcione, las revisiones deben almacenarse en una estructura bien\-definida en un recurso compartido de archivos SMB \(carpeta raíz de revisiones\), y debe configurar el\-de la revisión en con la ruta de acceso a la carpeta raíz de revisiones mediante la interfaz de usuario de CAU o los cmdlets de PowerShell de CAU. Esta ruta de acceso se pasa al\-de plug-in como el argumento **HotfixRootFolderPath** . Puedes elegir una de varias estructuras para la carpeta raíz de revisiones, en función de las necesidades de actualización, como se muestra en los siguientes ejemplos. Los archivos o carpetas que no siguen la estructura se ignoran.  
  
#### <a name="example-1---folder-structure-used-to-apply-hotfixes-to-all-cluster-nodes"></a>Ejemplo 1: estructura de carpetas usada para aplicar revisiones a todos los nodos del clúster
  
Para especificar que las revisiones se apliquen a todos los nodos del clúster, cópielos en una carpeta denominada **CAUHotfix\_** en la carpeta raíz de revisiones. En este ejemplo, el\-de **HotfixRootFolderPath** de complemento en el argumento se establece en *\\\\de la\\revisiones\\raíz* .\\ La **CAUHotfix\_toda** la carpeta contiene tres actualizaciones con las extensiones. msu,. msi y. MSP que se aplicarán a todos los nodos del clúster. Los nombres de los archivos de actualización solo tienen fines ilustrativos.  
  
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
  
#### <a name="example-2---folder-structure-used-to-apply-certain-updates-only-to-a-specific-node"></a>Ejemplo 2: estructura de carpetas usada para aplicar ciertas actualizaciones solo a un nodo específico
  
Para especificar revisiones que se aplican solo a un nodo específico, usa una subcarpeta de la carpeta raíz de revisiones con el nombre del nodo. Usa el nombre NetBIOS del nodo del clúster, por ejemplo, *ContosoNode1*. A continuación, mueve las actualizaciones que solo se aplican a este nodo a esta subcarpeta. En el ejemplo siguiente, el\-de **HotfixRootFolderPath** de complemento en el argumento se establece en *\\\\de la\\revisiones\\raíz* .\\ Las actualizaciones del **CAUHotfix\_todas** las carpetas se aplicarán a todos los nodos del clúster y el *nodo1\_específico\_Update. msu* solo se aplicará a *ContosoNode1*.  
  
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
  
#### <a name="example-3---folder-structure-used-to-apply-updates-other-than-msu-msi-and-msp-files"></a>Ejemplo 3: estructura de carpetas usada para aplicar actualizaciones distintas de archivos. msu,. msi y. MSP
  
De manera predeterminada, **Microsoft.HotfixPlugin** solo aplica actualizaciones con las extensiones .msu, .msi o .msp. No obstante, ciertas actualizaciones podrían tener extensiones diferentes y requerir comandos de instalación distintos. Por ejemplo, es posible que tengas que aplicar una actualización de firmware con la extensión .exe a un nodo del clúster. Puede configurar la carpeta raíz de revisiones con una subcarpeta que indique que se debe instalar un tipo de actualización específico no\-predeterminado. También debes configurar una regla de instalación de carpeta correspondiente que especifique el comando de instalación en el elemento `<FolderRules>` del archivo XML de configuración de revisiones.  
  
En el ejemplo siguiente, el\-de **HotfixRootFolderPath** de complemento en el argumento se establece en *\\\\de la\\revisiones\\raíz* .\\ Se aplicarán varias actualizaciones a todos los nodos del clúster y se aplicará una actualización de firmware *SpecialHotfix1.exe* a *ContosoNode1* mediante *FolderRule1*. Para obtener información sobre la configuración de *FolderRule1* en el archivo de configuración de revisiones, consulte [Personalizar el archivo de configuración de revisiones](#BKMK_CONFIG_FILE) más adelante en este tema.  
  
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
  
**% SystemRoot%\\system32\\WindowsPowerShell\\v 1.0\\módulos\\carpeta ClusterAwareUpdating**  
  
Para personalizar el archivo de configuración de revisiones, copia el archivo de configuración de ejemplo DefaultHotfixConfig.xml desde esta ubicación a la carpeta raíz de revisiones y realiza las modificaciones apropiadas para tu escenario.  
  
> [!IMPORTANT]  
> Para aplicar la mayoría de revisiones proporcionadas por Microsoft y otras actualizaciones, se puede usar el archivo de configuración de revisiones predeterminado sin modificaciones. La personalización del archivo de configuración de revisiones es una tarea solo en escenarios de uso avanzados.  
  
De manera predeterminada, el archivo XML de configuración de revisiones define reglas de instalación y condiciones de salida para las dos categorías de revisiones siguientes:  
  
-   Archivos de revisión con extensiones que el\-de plug-in puede instalar de forma predeterminada \(archivos. msu,. msi y. MSP\).  
  
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
  
-   Revisiones u otros archivos de actualización que no sean archivos. msi,. msu o. MSP, por ejemplo, actualizaciones de controladores, firmware y BIOS que no son de\-.  
  
    Cada tipo de archivo no\-predeterminado se configura como un elemento `<Folder>` en el elemento `<FolderRules>`. El atributo de nombre del elemento `<Folder>` debe ser idéntico al nombre de una carpeta en la carpeta raíz de revisiones que contendrá las actualizaciones del tipo correspondiente. La carpeta puede estar en la carpeta **CAUHotfix\_todas** o en un nodo\-carpeta específica. Por ejemplo, si *FolderRule1* se configura en la carpeta raíz de revisiones, configura el elemento siguiente en el archivo XML para definir una plantilla de instalación y condiciones de salida para las actualizaciones en esa carpeta:  
  
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
|`<Success_RebootRequired>`|De manera opcional, define uno o más códigos de salida que indican que la actualización en cuestión se ha realizado correctamente y que el nodo se debe reiniciar. <br>**Nota:** Opcionalmente, el elemento `<Folder>` puede contener el atributo `alwaysReboot`. Si se establece este atributo, indica que si una revisión instalada por esta regla devolverá uno de los códigos de salida definidos en `<Success>`, se interpreta como condición de salida `<Success_RebootRequired>` .|  
|`<Fail_RebootRequired>`|De manera opcional, define uno o más códigos de salida que indican que la actualización en cuestión se ha realizado de manera incorrecta y que el nodo se debe reiniciar.|  
|`<AlreadyInstalled>`|De manera opcional, define uno o más códigos de salida que indican que la actualización en cuestión no se ha aplicado porque ya está instalada.|  
|`<NotApplicable>`|De manera opcional, define uno o más códigos de salida que indican que la actualización en cuestión no se ha aplicado porque no se aplica al nodo del clúster.|  
  
> [!IMPORTANT]  
> Los códigos de salida que no se definan de manera explícita en `<ExitConditions>` se interpretan como un error de actualización y el nodo no se reinicia.  
  
### <a name="BKMK_ACL"></a>Restringir el acceso a la carpeta raíz de revisiones  
Debe realizar varios pasos para configurar el servidor de archivos y el recurso compartido de archivos SMB para proteger los archivos de la carpeta raíz de revisiones y el archivo de configuración de revisiones para el acceso solo en el contexto de **Microsoft.HotfixPlugin**. Estos pasos habilitan varias características que ayudan a impedir posibles manipulaciones de los archivos de revisión de manera que se pueda poner en peligro el clúster de conmutación por error.  
  
Los pasos generales son los siguientes:  
  
1.  Identifique la cuenta de usuario que se usa para las ejecuciones de actualización mediante el\-plug in  
  
2.  Configurar esta cuenta de usuario en los grupos necesarios en el servidor de archivos SMB  
  
3.  Configurar permisos para tener acceso a la carpeta raíz de revisiones  
  
4.  Configurar las opciones de integridad de datos SMB  
  
5.  Habilitar una regla de Firewall de Windows en el servidor SMB  
  
#### <a name="step-1-identify-the-user-account-that-is-used-for-updating-runs-by-using-the-hotfix-plug-in"></a>Paso 1. Identifique la cuenta de usuario que se usa para las ejecuciones de actualización mediante el\-de complemento de revisiones de
  
La cuenta que se usa en CAU para comprobar la configuración de seguridad mientras se realiza una ejecución de actualización con **Microsoft. HotfixPlugin** depende de si se usa la Cau en modo de actualización de\-remoto o en el modo de actualización automática de\-, como se indica a continuación:  
  
-   **Modo de actualización de\-remoto** Cuenta que tiene privilegios administrativos en el clúster para obtener una vista previa de las actualizaciones y aplicarlas.  
  
-   **Modo de actualización de\-propio** Nombre del objeto de equipo virtual que se configura en Active Directory para el rol en clúster de CAU. Es el nombre de un objeto de equipo virtual preconfigurado en Active Directory para el rol en clúster de CAU o el nombre que CAU genera para el rol en clúster. Para obtener el nombre si lo genera CAU, ejecute el cmdlet de PowerShell **Get\-CauClusterRole** Cau. En la salida, **ResourceGroupName** es el nombre de la cuenta de objeto de equipo virtual generada.  
  
#### <a name="step-2-configure-this-user-account-in-the-necessary-groups-on-an-smb-file-server"></a>Paso 2. Configurar esta cuenta de usuario en los grupos necesarios en el servidor de archivos SMB
  
> [!IMPORTANT]  
> Debes agregar la cuenta que se usa para ejecuciones de actualización como cuenta de administrador local en el servidor SMB. Si las directivas de seguridad de tu organización no lo permiten, configura esta cuenta con los permisos necesarios en el servidor SMB mediante el procedimiento siguiente.  
  
##### <a name="to-configure-a-user-account-on-the-smb-server"></a>Para configurar una cuenta de usuario en el servidor SMB  
  
1.  Agregue la cuenta que se usa para ejecuciones de actualización al grupo Usuarios COM distribuidos y a uno de los siguientes grupos: Usuario avanzado, Operación de servidor u Operador de impresión.  
  
2.  Para habilitar los permisos de WMI necesarios para la cuenta, inicia la consola de administración de WMI en el servidor SMB. Inicie PowerShell y escriba el siguiente comando:  
  
    ```  
    wmimgmt.msc  
    ```  
  
3.  En el árbol de consola, haga clic con el botón secundario\-en **control WMI \(\)local** y, a continuación, haga clic en **propiedades**.  
  
4.  Haz clic en **Seguridad** y, a continuación, expande **Raíz**.  
  
5.  Haz clic en **CIMV2**y, a continuación, haz clic en **Seguridad**.  
  
6.  Agrega la cuenta que se usa para las actualizaciones de ejecución a la lista **Nombres de grupos o usuarios**.  
  
7.  Concede los permisos **Ejecutar métodos** y **Llamada remota habilitada** a la cuenta que se usa para las ejecuciones de actualización.  
  
#### <a name="step-3-configure-permissions-to-access-the-hotfix-root-folder"></a>Paso 3. Configurar permisos para tener acceso a la carpeta raíz de revisiones
  
De forma predeterminada, cuando intenta aplicar actualizaciones, el\-de la revisión en comprueba la configuración de los permisos del sistema de archivos NTFS para el acceso a la carpeta raíz de revisiones. Si los permisos de acceso a la carpeta no están configurados correctamente, es posible que se produzca un error en una ejecución de actualización con la\-de la revisión en.  
  
Si usa la configuración predeterminada del complemento de revisiones\-en, asegúrese de que los permisos de acceso a la carpeta cumplan los siguientes requisitos.  
  
-   El grupo Usuarios tiene permiso de lectura.  
  
-   Si el\-de complemento de aplicará actualizaciones con la extensión. exe, el grupo usuarios tendrá el permiso ejecutar.  
  
-   Solo se permiten ciertas entidades de seguridad \(pero no son necesarias\) tener permiso de escritura o modificación. Las entidades de seguridad permitidas son el grupo Administradores local, SYSTEM, CREATOR OWNER y TrustedInstaller. No se permite a otras cuentas o grupos tener permisos de escritura o modificación en la carpeta raíz de revisiones.  
  
Opcionalmente, puede deshabilitar las comprobaciones anteriores que realiza el\-de plug-in de forma predeterminada. Puedes hacerlo de dos maneras distintas:  
  
-   Si usa los cmdlets de PowerShell de CAU, configure el argumento **DisableAclChecks\=' true '** en el parámetro **CauPluginArguments** para el\-de complemento de la revisión en.  
  
-   Si usas la interfaz de usuario de CAU, selecciona la opción **Deshabilitar comprobación del acceso del administrador a la carpeta raíz de revisiones y el archivo de configuración** en la página **Opciones de actualización adicionales** del asistente que se usa para configurar las opciones de la ejecución de actualización.  
  
No obstante, como procedimiento recomendado en muchos entornos, recomendamos usar la configuración predeterminada para exigir estas comprobaciones.  
  
#### <a name="step-4-configure-settings-for-smb-data-integrity"></a>Paso 4. Configurar las opciones de integridad de datos SMB
  
Para comprobar la integridad de los datos en las conexiones entre los nodos del clúster y el recurso compartido de archivos SMB, el complemento de la revisión\-en requiere que habilite la configuración del recurso compartido de archivos SMB para la firma SMB o el cifrado SMB. El cifrado SMB, que proporciona seguridad mejorada y un mejor rendimiento en muchos entornos, se admite a partir de Windows Server 2012. Puedes habilitar una o ambas configuraciones, de la manera siguiente:  
  
-   Para habilitar la firma SMB, consulte el procedimiento del [artículo 887429](https://support.microsoft.com/kb/887429) en Microsoft Knowledge Base.  
  
-   Para habilitar el cifrado SMB para la carpeta compartida SMB, ejecute el siguiente cmdlet de PowerShell en el servidor SMB:  
  
    ```PowerShell  
    Set-SmbShare <ShareName> -EncryptData $true  
    ```  
  
    Donde <*ShareName*> es el nombre de la carpeta compartida SMB.  
  
Opcionalmente, para exigir el uso del cifrado SMB en las conexiones al servidor SMB, seleccione la opción **requerir cifrado SMB al tener acceso a la carpeta raíz de revisiones** en la interfaz de usuario de Cau o configure el\-**RequireSMBEncryption\=' true '** en el argumento mediante los cmdlets de PowerShell de la Cau.  
  
> [!IMPORTANT]  
> Si seleccionas la opción para exigir el uso del cifrado SMB y la carpeta raíz de revisiones no está configurada para las conexiones que usan el cifrado SMB, la ejecución de actualización generará un error.  
  
#### <a name="step-5-enable-a-windows-firewall-rule-on-the-smb-server"></a>Paso 5. Habilitar una regla de Firewall de Windows en el servidor SMB
  
Debe habilitar la **administración remota del servidor de archivos \(SMB\-en\)** regla en el Firewall de Windows en el servidor de archivos SMB. Esta opción está habilitada de forma predeterminada en Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012.  
  
## <a name="see-also"></a>Consulte también  
  
-   [Información general sobre la actualización compatible con clústeres](cluster-aware-updating.md)
  
-   [Cmdlets de Windows PowerShell para la actualización compatible con clústeres](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating)  
  
-   [Referencia del complemento de actualización compatible con clústeres](https://msdn.microsoft.com/library/hh418084.aspx)  
  
