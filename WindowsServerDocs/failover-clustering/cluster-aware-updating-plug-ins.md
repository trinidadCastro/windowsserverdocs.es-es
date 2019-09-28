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

>Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La [actualización compatible con clústeres](cluster-aware-updating.md) (CAU) usa complementos para coordinar la instalación de actualizaciones en los nodos de un clúster de conmutación por error. En este tema se proporciona información sobre el uso de la compilación de no__t-0in de CAU de @ no__t-1ins u otro Plug @ no__t-2ins que se instala para la CAU.

## <a name="BKMK_INSTALL"></a>Instalación de un plug @ no__t-1in  
Se debe instalar por separado un valor de plug @ no__t-0in que no sea el predeterminado de plug @ no__t-1ins que se instalan con CAU \(**Microsoft. WindowsUpdatePlugin** y **microsoft. HotfixPlugin**\). Si se usa la CAU en el modo Self @ no__t-0updating, se debe instalar el plug-no__t-1in en todos los nodos del clúster. Si se usa la CAU en el modo remoto @ no__t-0updating, se debe instalar el plug-no__t-1in en el equipo coordinador de actualizaciones remotas. Un plug @ no__t-0in que instale puede tener requisitos de instalación adicionales en cada nodo.  
  
Para instalar un plug @ no__t-0in, siga las instrucciones del publicador @ no__t-1in. Para registrar manualmente un plug @ no__t-0in con CAU, ejecute el cmdlet [Register-CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin) en todos los equipos en los que esté instalado el complemento Plug @ no__t-2in.  
  
## <a name="specify-a-plug-in-and-plug-in-arguments"></a>Especificar los argumentos de plug @ no__t-0in y plug @ no__t-1in  
  
### <a name="specify-a-cau-plug-in"></a>Especifique una conexión de CAU, @ no__t-0in

En la interfaz de usuario de CAU, seleccione un plug @ no__t-0in de una lista drop @ no__t-1down de plug @ no__t-2ins disponible al usar la CAU para realizar las siguientes acciones:  
  
-   Aplicar actualizaciones al clúster  
  
-   Obtener una vista previa de las actualizaciones del clúster  
  
-   Configuración del clúster Self @ no__t-0updating Options  
  
De forma predeterminada, la CAU selecciona el valor de plug @ no__t-0in **Microsoft. WindowsUpdatePlugin**. Sin embargo, puede especificar cualquier Plug @ no__t-0in que esté instalado y registrado con CAU.

> [!TIP]  
> En la interfaz de usuario de CAU, solo puede especificar un único Plug-no__t-0in para que CAU lo use para obtener una vista previa o para aplicar actualizaciones durante una ejecución de actualización. Mediante el uso de los cmdlets de PowerShell de CAU, puede especificar uno o varios Plug @ no__t-0ins. Si necesita instalar varios tipos de actualizaciones en el clúster, normalmente es más eficaz especificar varios Plug-no__t-0ins en una ejecución de actualización, en lugar de usar una ejecución de actualización independiente para cada Plug @ no__t-1in. Por ejemplo, lo normal es que se produzcan menos reinicios de nodos.

Mediante el uso de los cmdlets de PowerShell de CAU que se enumeran en la tabla siguiente, puede especificar uno o varios Plug @ no__t-0ins para una ejecución de actualización o un examen pasando el parámetro **– CauPluginName** . Puede especificar varios nombres de plug @ no__t-0in separándolos con comas. Si especifica Multiple Plug @ no__t-0ins, también puede controlar el modo en que el método Plug @ no__t-1ins se influye entre sí durante una ejecución de actualización especificando los **\-RunPluginsSerially**, **\-StopOnPluginFailure**y **– SeparateReboots** parámetros. Para obtener más información sobre el uso de varios Plug-no__t-0ins, use los vínculos proporcionados a la documentación del cmdlet en la tabla siguiente.  
  
|Cmdlet|Descripción|  
|----------|---------------|  
|[Add-CauClusterRole](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/add-cauclusterrole)|Agrega el rol en clúster de CAU que proporciona la funcionalidad Self @ no__t-0updating al clúster especificado.|  
|[Invoke-CauRun](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/invoke-caurun)|Realiza un examen de los nodos del clúster para buscar actualizaciones aplicables y las instala por medio de una ejecución de actualización en el clúster especificado.|  
|[Invoke-CauScan](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/invoke-causcan)|Realiza un examen de los nodos del clúster para buscar actualizaciones aplicables y devuelve una lista del conjunto inicial de actualizaciones que se aplicarían a cada nodo en el clúster especificado.|  
|[Set-CauClusterRole](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/set-cauclusterrole)|Establece las propiedades de configuración del rol en clúster de CAU en el clúster especificado.|  
  
Si no especifica un parámetro de la opción de conexión de no__t-0in de CAU con estos cmdlets, el valor predeterminado es el de plug @ no__t-1in **Microsoft. WindowsUpdatePlugin**.  
  
### <a name="specify-cau-plug-in-arguments"></a>Especificar los argumentos de la bujía de CAU @ no__t-0in  
Al configurar las opciones de ejecución de actualización, puede especificar uno o varios pares de *nombre @ no__t-1value* \(arguments @ no__t-3 para que los use el plug @ no__t-4in seleccionado. Por ejemplo, en la interfaz de usuario de CAU puedes especificar varios argumentos de la siguiente manera:  
  
**Nombre1 @ no__t-1Value1; Nombre2 @ no__t-2Value2; Name3 @ no__t-3Value3**  
  
Estos pares *de nombre @ no__t-1value* deben ser significativos para el plug @ no__t-2in que especifique. Para algunos Plug @ no__t-0ins, los argumentos son opcionales.  
  
La sintaxis de los argumentos de plug-no__t-0in de la CAU sigue estas reglas generales:  
  
-   Varios pares *de nombre @ no__t-1value* se separan mediante signos de punto y coma.  
  
-   Un valor que contenga espacio se incluye entre comillas, por ejemplo: **Nombre1 @ no__t-1 "valor con espacios"** .  
  
-   La sintaxis exacta de *Value* depende del valor de plug @ no__t-1in.  
  
Para especificar los argumentos de plug @ no__t-0in mediante los cmdlets de PowerShell de CAU que admiten el parámetro **– CauPluginParameters** , pase un parámetro con el formato:  
  
**\-CauPluginArguments @ {Nombre1 @ no__t-2Value1; Nombre2 @ no__t-3Value2; Name3 @ no__t-4Value3}**  
  
También puede usar una tabla hash de PowerShell predefinida. Para especificar los argumentos de plug @ no__t-0in para más de un plug @ no__t-1in, pase varias tablas hash de argumentos, separadas por comas. Pase los argumentos de plug @ no__t-0in en el orden de plug @ no__t-1in que se especifica en **CauPluginName**.  
  
### <a name="specify-optional-plug-in-arguments"></a>Especificar los argumentos opcionales de plug @ no__t-0in  
El plug @ no__t-0ins que instala CAU \(**Microsoft. WindowsUpdatePlugin** y **microsoft. HotfixPlugin**\) proporcionan opciones adicionales que puede seleccionar. En la interfaz de usuario de CAU, aparecen en una página de **opciones adicionales** después de configurar las opciones de ejecución de actualización para el plug @ no__t-1in. Si usa los cmdlets de PowerShell de CAU, estas opciones se configuran como argumentos opcionales de plug @ no__t-0in. Para más información, consulte [Usar Microsoft.WindowsUpdatePlugin](#BKMK_WUP) y [Usar Microsoft.HotfixPlugin](#BKMK_HFP) más adelante en este tema.  
  
## <a name="manage-plug-ins-using-windows-powershell-cmdlets"></a>Administrar Plug @ no__t-0ins mediante cmdlets de Windows PowerShell  
  
|Cmdlet|Descripción|  
|----------|---------------|  
|[Get-CauPlugin](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/get-cauplugin)|Recupera información sobre uno o más complementos de actualización de software @ no__t-0ins que están registrados en el equipo local.|  
|[Register-CauPlugin]((https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/register-cauplugin))|Registra un complemento de actualización de software CAU @ no__t-0in en el equipo local.|  
|[Unregister-CauPlugin](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/unregister-cauplugin)|Quita un complemento de actualización de software @ no__t-0in de la lista de plug-no__t-1ins que puede usar la CAU. **Nota:** No se puede anular el registro de la conexión de plug @ no__t-0ins que se instalan con CAU \(**Microsoft. WindowsUpdatePlugin** y **microsoft. HotfixPlugin**\).|  
  
## <a name="BKMK_WUP"></a>Usar Microsoft. WindowsUpdatePlugin  

El valor predeterminado de plug @ no__t-0in para CAU, **Microsoft. WindowsUpdatePlugin**, realiza las siguientes acciones:
- Se comunica con el Agente de Windows Update en los nodos de los clústeres de conmutación por error con el fin de aplicar las actualizaciones necesarias para los productos de Microsoft que estén en ejecución en cada nodo.
- Instala las actualizaciones del clúster directamente desde Windows Update o Microsoft Update o desde un servidor de @ no__t-0premises Windows Server Update Services \(WSUS @ no__t-2.
- Instala solo las actualizaciones de la versión de distribución general seleccionada \(GDR @ no__t-1. De forma predeterminada, el valor de plug @ no__t-0in solo aplica actualizaciones de software importantes. No es necesario realizar ninguna configuración. La configuración predeterminada descarga e instala actualizaciones GDR importantes en cada nodo. 

> [!NOTE]
> Para aplicar actualizaciones distintas de las actualizaciones de software importantes que se seleccionan de forma predeterminada @no__t ejemplo de 0for: actualización de controladores @ no__t-1, puede configurar un parámetro opcional Plug @ no__t-2in. Para obtener más información, consulta [Configurar la cadena de consulta del Agente de Windows Update](#BKMK_QUERY).

### <a name="requirements"></a>Requisitos

- El clúster de conmutación por error y el equipo coordinador de actualizaciones remotas \(If usado @ no__t-1 deben cumplir los requisitos de CAU y la configuración necesaria para la administración remota que aparecen en [requisitos y prácticas recomendadas para Cau](cluster-aware-updating-requirements.md).
- Consulta las [Recomendaciones para aplicar actualizaciones de Microsoft](cluster-aware-updating-requirements.md#BKMK_BP_WUA) y, a continuación, realiza los cambios necesarios en la configuración de Microsoft Update para los nodos del clúster de conmutación por error.
- Para obtener los mejores resultados, se recomienda ejecutar la Analizador de procedimientos recomendados CAU \(BPA @ no__t-1 para asegurarse de que el clúster y el entorno de actualización estén configurados correctamente para aplicar actualizaciones mediante CAU. Para obtener más información, consulta [Probar la preparación para la actualización de CAU](cluster-aware-updating-requirements.md#BKMK_BPA).

> [!NOTE]
> Las actualizaciones que requieren la aceptación de términos de licencia de Microsoft o que requieren la interacción del usuario se excluyen y deben instalarse manualmente.

### <a name="additional-options"></a>Opciones adicionales

Opcionalmente, puede especificar los siguientes argumentos de plug @ no__t-0in para aumentar o restringir el conjunto de actualizaciones que se aplican mediante el plug @ no__t-1in:
- Para configurar el método Plug @ no__t-0in para que aplique las actualizaciones recomendadas además de las actualizaciones importantes en cada nodo, en la interfaz de usuario de CAU, en la página **opciones adicionales** , seleccione la misma opción para **que reciba actualizaciones importantes** . cuadro.
<br>Como alternativa, configure el argumento **' IncludeRecommendedUpdates ' \= ' true '** Plug @ no__t-2in.
- Para configurar el parámetro Plug @ no__t-0in para filtrar los tipos de actualizaciones GDR que se aplican a cada nodo de clúster, especifique una cadena de consulta del agente de Windows Update con un argumento **QueryString** PNP @ no__t-2in. Para obtener más información, consulta [Configurar la cadena de consulta del Agente de Windows Update](#BKMK_QUERY).

### <a name="BKMK_QUERY"></a>Configurar la cadena de consulta del agente de Windows Update  
Puede configurar un argumento de plug @ no__t-0in para el valor predeterminado de plug @ no__t-1in, **Microsoft. WindowsUpdatePlugin**, que consta de una cadena de consulta de Windows Update Agent \(WUA @ no__t-4. Esta instrucción usa la API de WUA para identificar uno o más grupos de actualizaciones de Microsoft que se aplicarán a cada nodo, según criterios de selección específicos. Puedes combinar varios criterios mediante un AND lógico o mediante un OR lógico. La cadena de consulta de WUA se especifica en un argumento de plug @ no__t-0in como se indica a continuación:  
  
**QueryString @ no__t-1 "Criterio1 @ no__t-2Value1 y @ no__t-3or Criterion2 @ no__t-4Value2 y @ no__t-5or..."**  
  
Por ejemplo, **Microsoft.WindowsUpdatePlugin** selecciona automáticamente actualizaciones importantes mediante un argumento **QueryString** predeterminado que se construye con los criterios **IsInstalled**, **Type**, **IsHidden** y **IsAssigned**:  
  
**QueryString @ no__t-1 "IsInstalled @ no__t-20 y escriba @ no__t-3'Software" y IsHidden @ no__t-40 y IsAssigned @ no__t-51 "**  
  
Si especifica un argumento **QueryString** , se usa en lugar de la **cadena** de configuración predeterminada que está configurada para el parámetro Plug @ no__t-2in.  
  
#### <a name="example-1"></a>Ejemplo 1
  
Para configurar un argumento **QueryString** que instale una actualización concreta, tal como se identifica mediante *el identificador f6ce46c1 @ no__t-2971c @ no__t-343f9 @ no__t-4a2aa @ no__t-5783df125f003*:  
  
**QueryString @ no__t-1 "UpdateID @ no__t-2'f6ce46c1 @ no__t-3971c @ no__t-443f9 @ no__t-5a2aa @ no__t-6783df125f003" y IsInstalled @ no__t-70 "**  
  
> [!NOTE]  
> El ejemplo anterior es válido para aplicar actualizaciones mediante el Asistente para actualización de Cluster @ no__t-0Aware. Si desea instalar una actualización específica configurando las opciones de Self @ no__t-0updating con la interfaz de usuario de CAU o mediante el cmdlet **Add @ no__t-2CauClusterRole** o **set @ no__t-4CauClusterRole de**PowerShell, debe dar formato al valor UpdateID con dos solo caracteres @ no__t-5quote:  
>   
> **QueryString @ no__t-1 "UpdateID @ no__t-2''f6ce46c1 @ no__t-3971c @ no__t-443f9 @ no__t-5a2aa @ no__t-6783df125f003 ' ' y IsInstalled @ no__t-70"**  
  
#### <a name="example-2"></a>Ejemplo 2
  
Para configurar un argumento **QueryString** que solo instale controladores:  
  
**QueryString @ no__t-1 "IsInstalled @ no__t-20 y escriba @ no__t-3'Driver" y IsHidden @ no__t-40 "**  
  
Para obtener más información sobre las cadenas de consulta para el valor predeterminado de plug @ no__t-0in, **Microsoft. WindowsUpdatePlugin**, los criterios de búsqueda \(Such como **IsInstalled**\) y la sintaxis que se puede incluir en las cadenas de consulta, vea la sección Acerca de los criterios de búsqueda en la referencia de la [API del agente de Windows Update (WUA)](https://go.microsoft.com/fwlink/p/?LinkId=223304).  
  
## <a name="BKMK_HFP"></a>Usar Microsoft. HotfixPlugin  
Se puede usar el método Plug @ no__t-0in **Microsoft. HotfixPlugin** para aplicar versiones de distribución limitada de Microsoft \(LDR @ no__t-3 actualizaciones \(also llamadas revisiones y, anteriormente, llamadas QFE @ no__t-5 que se descargan de forma independiente a la dirección problemas de software de Microsoft específicos. El complemento instala las actualizaciones desde una carpeta raíz en un recurso compartido de archivos SMB y también se puede personalizar para aplicar actualizaciones de controladores, firmware y BIOS que no sean @ no__t-0Microsoft.

> [!NOTE]
> A veces, las revisiones están disponibles para su descarga de Microsoft en los artículos de Knowledge base, pero también se proporcionan a los clientes en un as @ no__t-0needed.

### <a name="requirements"></a>Requisitos

- El clúster de conmutación por error y el equipo coordinador de actualizaciones remotas \(If usado @ no__t-1 deben cumplir los requisitos de CAU y la configuración necesaria para la administración remota que aparecen en [requisitos y prácticas recomendadas para Cau](cluster-aware-updating-requirements.md).
- Consulta [Recomendaciones para usar Microsoft.HotfixPlugin](cluster-aware-updating-requirements.md#BKMK_BP_HF).
- Para obtener los mejores resultados, se recomienda ejecutar el modelo CAU Analizador de procedimientos recomendados \(BPA @ no__t-1 para asegurarse de que el clúster y el entorno de actualización están configurados correctamente para aplicar actualizaciones mediante CAU. Para obtener más información, consulta [Probar la preparación para la actualización de CAU](cluster-aware-updating-requirements.md#BKMK_BPA).
- Obtenga las actualizaciones del publicador y cópielos o extráigalos en un bloque de mensajes del servidor \(SMB @ no__t-1 recurso compartido de archivos \(hotfix carpeta raíz @ no__t-3 que admita al menos SMB 2,0 y que sea accesible para todos los nodos del clúster y la actualización remota. El equipo coordinador \(If CAU se usa en el modo remoto @ no__t-5updating @ no__t-6. Para obtener más información, consulta [Configurar una estructura de carpetas raíz de revisiones](#BKMK_HF_ROOT) más adelante en este tema. 

    > [!NOTE]
    > De forma predeterminada, este plug @ no__t-0in solo instala revisiones con las siguientes extensiones de nombre de archivo:. msu,. msi y. msp.

- Copie el archivo DefaultHotfixConfig. XML \(which se proporciona en la carpeta **% systemroot% \\System32 @ no__t-3WindowsPowerShell @ no__t-4V 1.0 @ no__t-5Modules @ no__t-6ClusterAwareUpdating** en un equipo donde se encuentran las herramientas de Cau se instaló @ no__t-7 en la carpeta raíz de revisiones que creó y en la que extrajo las revisiones. Por ejemplo, copie el archivo de configuración en *\\ @ no__t-2MyFileServer @ no__t-3Hotfixes @ no__t-4Root @ no__t-5*. 

    > [!NOTE]
    > Para instalar la mayoría de revisiones proporcionadas por Microsoft y otras actualizaciones, se puede usar el archivo de configuración de revisiones predeterminado sin modificaciones. Si tu escenario lo requiere, puedes personalizar el archivo de configuración como tarea avanzada. El archivo de configuración puede incluir reglas personalizadas, por ejemplo, para manipular archivos de revisión que tienen extensiones de archivo específicas o para definir los comportamientos de condiciones de salida específicas. Para obtener más información, consulta [Personalizar el archivo de configuración de revisiones](#BKMK_CONFIG_FILE) más adelante en este tema.

### <a name="configuration"></a>Configuración

Configura las siguientes opciones. Para obtener más información, consulta los vínculos a las secciones más adelante en este tema.
- La ruta de acceso a la carpeta raíz de revisiones compartida que contiene las actualizaciones que se aplicarán y el archivo de configuración de revisiones. Puede escribir esta ruta de acceso en la interfaz de usuario de CAU o configurar el argumento **HotfixRootFolderPath @ no__t-1 @ no__t-2Path >** PowerShell Plug @ no__t-3in. 

   > [!NOTE]
   > Puede especificar la carpeta raíz de revisiones como una ruta de acceso de carpeta local o como una ruta de acceso UNC con el formato *\\ @ no__t-2ServerName @ no__t-3Share @ no__t-4RootFolderName*. Se puede usar un dominio @ no__t-0based o una ruta de acceso del espacio de nombres DFS independiente. Sin embargo, las características de plug @ no__t-0in que comprueban los permisos de acceso en el archivo de configuración de revisiones son incompatibles con una ruta de acceso de espacio de nombres DFS, por lo que si configura una, debe deshabilitar la comprobación de los permisos de acceso mediante la interfaz de usuario de CAU o mediante la configuración del  **DisableAclChecks @ no__t-2'True '** Plug @ no__t-3in argumento.
- Configuración del servidor que hospeda la carpeta raíz de revisiones para comprobar los permisos adecuados para tener acceso a la carpeta y garantizar la integridad de los datos a los que se tiene acceso desde la carpeta compartida SMB \(SMB firma o cifrado SMB @ no__t-1. Para obtener más información, consulte [Restringir el acceso a la carpeta raíz de revisiones](#BKMK_ACL).

### <a name="additional-options"></a>Opciones adicionales

- Opcionalmente, configure el archivo Plug @ no__t-0in para que se aplique el cifrado SMB al tener acceso a los datos del recurso compartido de archivos de la revisión. En la interfaz de usuario de CAU, en la página **opciones adicionales** , seleccione la opción **requerir cifrado SMB al tener acceso a la carpeta raíz de revisiones** o configure el argumento **RequireSMBEncryption @ no__t-3'True '** PowerShell Plug @ no__t-4in. 
  > [!IMPORTANT]
  > Debes realizar pasos de configuración adicionales en el servidor SMB para habilitar la integridad de datos SMB con la firma o el cifrado SMB. Para obtener más información, consulta el paso 4 de [Restringir el acceso a la carpeta raíz de revisiones](#BKMK_ACL). Si seleccionas la opción para exigir el uso del cifrado SMB y la carpeta raíz de revisiones no está configurada para el acceso mediante el cifrado SMB, la ejecución de actualización generará un error.
- Si lo deseas, puedes deshabilitar las comprobaciones predeterminadas de permisos suficientes para la carpeta raíz de revisiones y el archivo de configuración de revisiones. En la interfaz de usuario de CAU, seleccione **deshabilitar comprobar el acceso de administrador a la carpeta raíz de revisiones y el archivo de configuración**, o configure el argumento **DisableAclChecks @ no__t-2'True '** Plug @ no__t-3in.
- Opcionalmente, configure el argumento **argumento hotfixinstallertimeoutminutes @ no__t-1 @ no__t-2** para especificar cuánto tiempo tarda la conexión de la revisión @ no__t-3in en esperar a que el proceso del instalador de revisiones devuelva. @no__t el valor predeterminado de 0The es 30 minutos. \) Por ejemplo, para especificar un período de tiempo de espera de dos horas, establezca **argumento hotfixinstallertimeoutminutes @ no__t-1120**.
- Opcionalmente, configure el argumento **HotfixConfigFileName \= <name>** Plug @ no__t-3in para especificar un nombre para el archivo de configuración de revisiones que se encuentra en la carpeta raíz de revisiones. Si no se especifica, se usa el nombre predeterminado DefaultHotfixConfig.xml.
  
### <a name="BKMK_HF_ROOT"></a>Configurar una estructura de carpetas raíz de revisiones

Para que el paquete de revisión @ no__t-0in funcione, las revisiones deben almacenarse en una estructura Well @ no__t-1defined en un recurso compartido de archivos SMB \(hotfix carpeta raíz @ no__t-3, y debe configurar la revisión PNP @ no__t-4in con la ruta de acceso a la carpeta raíz de revisiones mediante la interfaz de usuario de CAU o los cmdlets de PowerShell de CAU. Esta ruta de acceso se pasa al parámetro Plug @ no__t-0in como argumento **HotfixRootFolderPath** . Puedes elegir una de varias estructuras para la carpeta raíz de revisiones, en función de las necesidades de actualización, como se muestra en los siguientes ejemplos. Los archivos o carpetas que no siguen la estructura se ignoran.  
  
#### <a name="example-1---folder-structure-used-to-apply-hotfixes-to-all-cluster-nodes"></a>Ejemplo 1: estructura de carpetas usada para aplicar revisiones a todos los nodos del clúster
  
Para especificar que las revisiones se apliquen a todos los nodos del clúster, cópielos en una carpeta denominada **CAUHotfix @ no__t-1All** en la carpeta raíz de revisiones. En este ejemplo, el argumento **HotfixRootFolderPath** Plug @ no__t-1in se establece en *\\ @ no__t-4MyFileServer @ no__t-5Hotfixes @ no__t-6Root @ no__t-7*. La carpeta **CAUHotfix @ no__t-1All** contiene tres actualizaciones con las extensiones. msu,. msi y. MSP que se aplicarán a todos los nodos del clúster. Los nombres de los archivos de actualización solo tienen fines ilustrativos.  
  
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
  
Para especificar revisiones que se aplican solo a un nodo específico, usa una subcarpeta de la carpeta raíz de revisiones con el nombre del nodo. Usa el nombre NetBIOS del nodo del clúster, por ejemplo, *ContosoNode1*. A continuación, mueve las actualizaciones que solo se aplican a este nodo a esta subcarpeta. En el ejemplo siguiente, el argumento **HotfixRootFolderPath** Plug @ no__t-1in se establece en *\\ @ no__t-4MyFileServer @ no__t-5Hotfixes @ no__t-6Root @ no__t-7*. Las actualizaciones de la carpeta **CAUHotfix @ no__t-1All** se aplicarán a todos los nodos del clúster y el *nodo1 @ no__t-3Specific\_Update.msu* solo se aplicará a *ContosoNode1*.  
  
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
  
De manera predeterminada, **Microsoft.HotfixPlugin** solo aplica actualizaciones con las extensiones .msu, .msi o .msp. No obstante, ciertas actualizaciones podrían tener extensiones diferentes y requerir comandos de instalación distintos. Por ejemplo, es posible que tengas que aplicar una actualización de firmware con la extensión .exe a un nodo del clúster. Puede configurar la carpeta raíz de revisiones con una subcarpeta que indique que se debe instalar un tipo de actualización específico no @ no__t-0default. También debes configurar una regla de instalación de carpeta correspondiente que especifique el comando de instalación en el elemento `<FolderRules>` del archivo XML de configuración de revisiones.  
  
En el ejemplo siguiente, el argumento **HotfixRootFolderPath** Plug @ no__t-1in se establece en *\\ @ no__t-4MyFileServer @ no__t-5Hotfixes @ no__t-6Root @ no__t-7*. Se aplicarán varias actualizaciones a todos los nodos del clúster y se aplicará una actualización de firmware *SpecialHotfix1.exe* a *ContosoNode1* mediante *FolderRule1*. Para obtener información sobre la configuración de *FolderRule1* en el archivo de configuración de revisiones, consulte [Personalizar el archivo de configuración de revisiones](#BKMK_CONFIG_FILE) más adelante en este tema.  
  
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
  
**% SystemRoot% \\System32 @ no__t-2WindowsPowerShell @ no__t-versión 1.0 @ no__t-4Modules @ no__t-5ClusterAwareUpdating**  
  
Para personalizar el archivo de configuración de revisiones, copia el archivo de configuración de ejemplo DefaultHotfixConfig.xml desde esta ubicación a la carpeta raíz de revisiones y realiza las modificaciones apropiadas para tu escenario.  
  
> [!IMPORTANT]  
> Para aplicar la mayoría de revisiones proporcionadas por Microsoft y otras actualizaciones, se puede usar el archivo de configuración de revisiones predeterminado sin modificaciones. La personalización del archivo de configuración de revisiones es una tarea solo en escenarios de uso avanzados.  
  
De manera predeterminada, el archivo XML de configuración de revisiones define reglas de instalación y condiciones de salida para las dos categorías de revisiones siguientes:  
  
-   Archivos de revisión con extensiones que el archivo Plug @ no__t-0in puede instalar de forma predeterminada @no__t -1. msu,. msi y. MSP @ no__t-2.  
  
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
  
-   Revisiones u otros archivos de actualización que no sean archivos. msi,. msu o. MSP, por ejemplo, controladores que no sean @ no__t-0Microsoft, firmware y actualizaciones del BIOS.  
  
    Cada tipo de archivo que no es @ no__t-0default se configura como un elemento `<Folder>` en el elemento `<FolderRules>`. El atributo de nombre del elemento `<Folder>` debe ser idéntico al nombre de una carpeta en la carpeta raíz de revisiones que contendrá las actualizaciones del tipo correspondiente. La carpeta puede estar en la carpeta **CAUHotfix @ no__t-1All** o en una carpeta node @ no__t-2specific. Por ejemplo, si *FolderRule1* se configura en la carpeta raíz de revisiones, configura el elemento siguiente en el archivo XML para definir una plantilla de instalación y condiciones de salida para las actualizaciones en esa carpeta:  
  
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
  
1.  Identifique la cuenta de usuario que se usa para las ejecuciones de actualización mediante el plug @ no__t-0in  
  
2.  Configurar esta cuenta de usuario en los grupos necesarios en el servidor de archivos SMB  
  
3.  Configurar permisos para tener acceso a la carpeta raíz de revisiones  
  
4.  Configurar las opciones de integridad de datos SMB  
  
5.  Habilitar una regla de Firewall de Windows en el servidor SMB  
  
#### <a name="step-1-identify-the-user-account-that-is-used-for-updating-runs-by-using-the-hotfix-plug-in"></a>Paso 1. Identifique la cuenta de usuario que se usa para las ejecuciones de actualización mediante el uso de la revisión Plug @ no__t-0in
  
La cuenta que se usa en CAU para comprobar la configuración de seguridad mientras se realiza una ejecución de actualización con **Microsoft. HotfixPlugin** depende de si se usa la Cau en modo remoto @ no__t-1updating o el modo Self @ no__t-2updating, como se indica a continuación:  
  
-   **Modo remoto @ no__t-1updating** Cuenta que tiene privilegios administrativos en el clúster para obtener una vista previa de las actualizaciones y aplicarlas.  
  
-   **Self @ no__t-1updating Mode** Nombre del objeto de equipo virtual que se configura en Active Directory para el rol en clúster de CAU. Es el nombre de un objeto de equipo virtual preconfigurado en Active Directory para el rol en clúster de CAU o el nombre que CAU genera para el rol en clúster. Para obtener el nombre si lo genera CAU, ejecute el cmdlet de PowerShell **de Cau Get @ no__t-1CauClusterRole** . En la salida, **ResourceGroupName** es el nombre de la cuenta de objeto de equipo virtual generada.  
  
#### <a name="step-2-configure-this-user-account-in-the-necessary-groups-on-an-smb-file-server"></a>Paso 2. Configurar esta cuenta de usuario en los grupos necesarios en el servidor de archivos SMB
  
> [!IMPORTANT]  
> Debes agregar la cuenta que se usa para ejecuciones de actualización como cuenta de administrador local en el servidor SMB. Si las directivas de seguridad de tu organización no lo permiten, configura esta cuenta con los permisos necesarios en el servidor SMB mediante el procedimiento siguiente.  
  
##### <a name="to-configure-a-user-account-on-the-smb-server"></a>Para configurar una cuenta de usuario en el servidor SMB  
  
1.  Agrega la cuenta que se usa para las ejecuciones de actualización al grupo Usuarios COM distribuidos y a uno de los grupos siguientes: Usuario avanzado, Operación de servidor u Operador de impresión.  
  
2.  Para habilitar los permisos de WMI necesarios para la cuenta, inicia la consola de administración de WMI en el servidor SMB. Inicie PowerShell y escriba el siguiente comando:  
  
    ```  
    wmimgmt.msc  
    ```  
  
3.  En el árbol de consola, haga clic con el botón derecho en @ no__t-0click **WMI Control \(Local @ no__t-3**y, a continuación, haga clic en **propiedades**.  
  
4.  Haz clic en **Seguridad** y, a continuación, expande **Raíz**.  
  
5.  Haz clic en **CIMV2**y, a continuación, haz clic en **Seguridad**.  
  
6.  Agrega la cuenta que se usa para las actualizaciones de ejecución a la lista **Nombres de grupos o usuarios**.  
  
7.  Concede los permisos **Ejecutar métodos** y **Llamada remota habilitada** a la cuenta que se usa para las ejecuciones de actualización.  
  
#### <a name="step-3-configure-permissions-to-access-the-hotfix-root-folder"></a>Paso 3. Configurar permisos para tener acceso a la carpeta raíz de revisiones
  
De forma predeterminada, cuando intenta aplicar actualizaciones, la revisión Plug @ no__t-0in comprueba la configuración de los permisos del sistema de archivos NTFS para tener acceso a la carpeta raíz de revisiones. Si los permisos de acceso a la carpeta no están configurados correctamente, es posible que se produzca un error en una ejecución de actualización con la revisión Plug @ no__t-0in.  
  
Si usa la configuración predeterminada de la revisión Plug @ no__t-0in, asegúrese de que los permisos de acceso a la carpeta cumplan los siguientes requisitos.  
  
-   El grupo Usuarios tiene permiso de lectura.  
  
-   Si el archivo Plug @ no__t-0in aplicará actualizaciones con la extensión. exe, el grupo usuarios tendrá el permiso ejecutar.  
  
-   Solo se permiten ciertas entidades de seguridad \(but no son necesarias @ no__t-1 para tener permiso de escritura o modificación. Las entidades de seguridad permitidas son el grupo Administradores local, SYSTEM, CREATOR OWNER y TrustedInstaller. No se permite a otras cuentas o grupos tener permisos de escritura o modificación en la carpeta raíz de revisiones.  
  
Opcionalmente, puede deshabilitar las comprobaciones anteriores que el valor de plug @ no__t-0in realiza de forma predeterminada. Puedes hacerlo de dos maneras distintas:  
  
-   Si usa los cmdlets de PowerShell de CAU, configure el argumento **DisableAclChecks @ no__t-1'True** en el parámetro **CauPluginArguments** para la revisión PNP @ no__t-3in.  
  
-   Si usas la interfaz de usuario de CAU, selecciona la opción **Deshabilitar comprobación del acceso del administrador a la carpeta raíz de revisiones y el archivo de configuración** en la página **Opciones de actualización adicionales** del asistente que se usa para configurar las opciones de la ejecución de actualización.  
  
No obstante, como procedimiento recomendado en muchos entornos, recomendamos usar la configuración predeterminada para exigir estas comprobaciones.  
  
#### <a name="step-4-configure-settings-for-smb-data-integrity"></a>Paso 4. Configurar las opciones de integridad de datos SMB
  
Para comprobar la integridad de los datos en las conexiones entre los nodos del clúster y el recurso compartido de archivos SMB, la revisión Plug @ no__t-0in requiere que habilite la configuración del recurso compartido de archivos SMB para la firma SMB o el cifrado SMB. El cifrado SMB, que proporciona seguridad mejorada y un mejor rendimiento en muchos entornos, se admite a partir de Windows Server 2012. Puedes habilitar una o ambas configuraciones, de la manera siguiente:  
  
-   Para habilitar la firma SMB, consulte el procedimiento del [artículo 887429](https://support.microsoft.com/kb/887429) en Microsoft Knowledge Base.  
  
-   Para habilitar el cifrado SMB para la carpeta compartida SMB, ejecute el siguiente cmdlet de PowerShell en el servidor SMB:  
  
    ```PowerShell  
    Set-SmbShare <ShareName> -EncryptData $true  
    ```  
  
    Donde <*ShareName*> es el nombre de la carpeta compartida SMB.  
  
Opcionalmente, para exigir el uso del cifrado SMB en las conexiones al servidor SMB, seleccione la opción **requerir cifrado SMB al tener acceso a la carpeta raíz de revisiones** en la interfaz de usuario de Cau o configure **RequireSMBEncryption @ no__t-2'True '** Plug @ No__ argumento t-3in con los cmdlets de PowerShell de la CAU.  
  
> [!IMPORTANT]  
> Si seleccionas la opción para exigir el uso del cifrado SMB y la carpeta raíz de revisiones no está configurada para las conexiones que usan el cifrado SMB, la ejecución de actualización generará un error.  
  
#### <a name="step-5-enable-a-windows-firewall-rule-on-the-smb-server"></a>Paso 5. Habilitar una regla de Firewall de Windows en el servidor SMB
  
Debe habilitar la regla de **administración remota del servidor de archivos \(SMB @ no__t-2in @ no__t-3** en el Firewall de Windows en el servidor de archivos SMB. Esta opción está habilitada de forma predeterminada en Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012.  
  
## <a name="see-also"></a>Vea también  
  
-   [Información general sobre la actualización compatible con clústeres](cluster-aware-updating.md)
  
-   [Cmdlets de Windows PowerShell para la actualización compatible con clústeres](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating)  
  
-   [Referencia del complemento de actualización compatible con clústeres](https://msdn.microsoft.com/library/hh418084.aspx)  
  
