---
title: Ejecutar análisis y administrar Analizador de procedimientos recomendados Results_1
description: Administrador de servidores
ms.prod: windows-server
ms.technology: manage-server-manager
ms.topic: article
ms.assetid: 232f1c80-88ef-4a39-8014-14be788c2766
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6ff854bcb25e4f5891e56f1e094fd4f387cf023f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851488"
---
# <a name="run-best-practices-analyzer-scans-and-manage-scan-results"></a>Ejecución de análisis del Analizador de procedimientos recomendados y administración de los resultados de los análisis

>Se aplica a: Windows Server 2016

En la administración de Windows, los *procedimientos recomendados* son instrucciones que se consideran ideales, en circunstancias típicas, para configurar un servidor, según la definición de los expertos. Por ejemplo, para la mayoría de las aplicaciones del servidor, se recomienda mantener abiertos solo aquellos puertos necesarios para que se comuniquen las aplicaciones con otros equipos en red y bloquear los puertos que no se usan. Aunque no seguir los procedimientos recomendados, incluso los cruciales, no tiene por qué presentar problemas, puede conllevar configuraciones de servidor con un rendimiento bajo, confiabilidad insuficiente, conflictos inesperados, mayores riesgos de seguridad u otros problemas posibles.

Analizador de procedimientos recomendados (BPA) es una herramienta de administración del servidor que está disponible en Windows Server 2012 R2, Windows Server 2012 y Windows Server 2008 R2. BPA puede ayudar a los administradores a reducir las infracciones de los procedimientos recomendados mediante el examen de los roles que están instalados en los servidores administrados que ejecutan Windows Server 2012 o Windows Server 2008 R2 y la notificación de las infracciones de procedimientos recomendados al administrador.

Puede ejecutar análisis de Analizador de procedimientos recomendados (BPA) desde Administrador del servidor, mediante la GUI del BPA o mediante cmdlets de Windows PowerShell. a partir de Windows Server 2012, puede examinar un rol o varios roles al mismo tiempo, en varios servidores, independientemente de que use el icono de Analizador de procedimientos recomendados en la consola de Administrador del servidor o los cmdlets de Windows PowerShell para ejecutar los análisis. También puede indicar al BPA que excluya u omita resultados de exámenes que no desee ver.

Este tema contiene las siguientes secciones.

-   [buscar BPA](#BKMK_find)

-   [Funcionamiento de BPA](#BKMK_how)

-   [Realización de análisis de Analizador de procedimientos recomendados en roles](#BKMK_BPAscan)

-   [Administrar resultados de exámenes](#BKMK_manage)

## <a name="find-bpa"></a><a name=BKMK_find></a>buscar BPA
Puede encontrar el icono de Analizador de procedimientos recomendados en las páginas de roles y grupos de servidores de Administrador del servidor en Windows Server 2012 R2 y Windows Server 2012, o puede abrir una sesión de Windows PowerShell con derechos de usuario elevados para ejecutar los cmdlets de Analizador de procedimientos recomendados.

## <a name="how-bpa-works"></a><a name=BKMK_how></a>Funcionamiento de BPA
BPA realiza una medición del cumplimiento de un rol con las reglas de procedimientos recomendados en ocho categorías diferentes de eficacia, fiabilidad y confiabilidad. Los resultados de las mediciones pueden enmarcarse en uno de los tres niveles de gravedad descritos en la siguiente tabla.

|Nivel de gravedad|Descripción|
|---------|--------|
|Error|Se devuelven resultados de error cuando un rol no cumple con las reglas de procedimientos recomendados y pueden preverse problemas de funcionalidad.|
|Información|Se devuelven resultados de información cuando un rol cumple con las reglas de procedimientos recomendados.|
|advertencia|Se devuelven resultados de advertencia cuando los resultados de incumplimiento pueden causar problemas si no se realizan cambios. La aplicación puede ser compatible tal y como funciona en ese momento, pero es posible que no cumpla las condiciones de una regla si no se realizan cambios en su configuración o las opciones de directiva. Por ejemplo, es posible que un examen de Servicios de Escritorio remoto muestre un resultado de advertencia si un servidor de licencias no está disponible para el rol porque, aunque no haya conexiones remotas activas en el momento del examen, no disponer de un servidor de licencias impide que las nuevas conexiones remotas obtengan licencias de acceso de cliente válidas.|

### <a name="rule-categories"></a>Categorías de las reglas
En la tabla siguiente se describen las categorías de reglas de procedimientos recomendados en las que se miden los roles durante un examen Analizador de procedimientos recomendados.

|Nombre de categoría|Descripción|
|---------|--------|
|Seguridad|Se aplican reglas de seguridad para medir el riesgo relativo de la exposición de un rol a amenazas como usuarios no autorizados o malintencionados, o la pérdida o el robo de datos confidenciales o de propiedad.|
|Rendimiento|Se aplican reglas de rendimiento para medir la capacidad de un rol para procesar solicitudes y realizar sus tareas prescritas en la empresa en los períodos de tiempo esperados según la carga de trabajo del rol.|
|Configuración|Las reglas de configuración se aplican para identificar valores de rol que es posible que deban modificarse para que el rol funcione de forma óptima. Las reglas de configuración pueden ayudar a impedir conflictos en configuraciones que pueden producir mensajes de error o impedir que el rol realice sus tareas prescritas en una empresa.|
|Directiva|Las reglas de Directiva se aplican para identificar directiva de grupo o la configuración del registro de Windows que pueda requerir modificaciones para que un rol funcione de forma óptima y segura.|
|Operación|Se aplican las reglas de operación para identificar posibles errores de un rol al realizar tareas prescritas en la empresa.|
|Antes de la implementación|Las reglas previas a la implementación se aplican antes de que un rol instalado se implemente en la empresa. Antes de que el rol se use en producción, permiten que los administradores evalúen si se cumple con las recomendaciones.|
|Después de la implementación|Las reglas posteriores a la implementación se aplican una vez que se hayan iniciado todos los servicios necesarios para un rol y antes de que se esté ejecutando en la empresa.|
|Requisitos previos|Las reglas de requisitos previos explican las opciones de configuración, las configuraciones de directiva y las características necesarias para un rol de modo que BPA pueda aplicar reglas específicas de otras categorías. Si aparece un requisito previo en los resultados del examen, indica una configuración incorrecta, un programa que falta, una directiva deshabilitada o habilitada incorrectamente, una configuración de clave del Registro u otro tipo de configuración que impidió que BPA aplicara una o varias reglas durante un examen. Un resultado de requisito previo no indica la compatibilidad o incompatibilidad. Significa que no se pudo aplicar una regla y, por tanto, no forma parte de los resultados del examen.|

## <a name="performing-best-practices-analyzer-scans-on-roles"></a><a name=BKMK_BPAscan></a>Realización de análisis de Analizador de procedimientos recomendados en roles
Puede realizar análisis BPA en los roles mediante la GUI del BPA en Administrador del servidor o mediante cmdlets de Windows PowerShell.

En Windows Server 2012 R2 y Windows Server 2012, algunos roles solicitan que se especifiquen parámetros adicionales, como los nombres de servidores o recursos compartidos específicos que ejecutan partes del rol, o los identificadores de submodelos, antes de iniciar un análisis BPA. Para los exámenes del BPA en modelos que requieren que se especifiquen parámetros adicionales, use cmdlets del BPA; la GUI del BPA no puede aceptar parámetros adicionales como los identificadores del submodelo. Por ejemplo, el identificador de submodelo **FSRM** representa el submodelo de BPA Servicios de archivo del Administrador de recursos del servidor de archivos, un servicio de rol de Servicios de archivo y almacenamiento. Para ejecutar un examen solo en el servicio de rol servidor de archivos Administrador de recursos, ejecute un análisis BPA mediante cmdlets de Windows PowerShell y agregue el parámetro `SubmodelId` al cmdlet.

Aunque no se pueden pasar parámetros adicionales a un análisis iniciado en la GUI del BPA, el icono BPA en Administrador del servidor muestra los resultados del análisis BPA más reciente, independientemente de cómo se iniciara el examen.

-   [Exámenes de roles mediante la GUI del BPA](#BKMK_GUIscan)

-   [Exámenes de roles mediante cmdlets de Windows PowerShell](#BKMK_PSscan)

### <a name="scanning-roles-by-using-the-bpa-gui"></a><a name=BKMK_GUIscan></a>Exámenes de roles mediante la GUI del BPA
Siga estos pasos para examinar uno o más roles en la GUI del BPA.

##### <a name="to-scan-roles-by-using-the-bpa-gui"></a>Para examinar roles mediante la GUI del BPA

1.  Realice una de las siguientes acciones para abrir Administrador del servidor si aún no está abierto.

    -   En la barra de tareas de Windows, haga clic en el botón Administrador del servidor.

    -   En la pantalla **Inicio** , haga clic en el icono de administrador del servidor.

2.  En el panel de navegación, abra una página de rol o grupo.

    Al ejecutar análisis BPA desde una página de rol o grupo, se examinan todos los roles que se instalan en servidores de ese grupo.

3.  En el menú **tareas** del icono de **analizador de procedimientos recomendados** , haga clic en **iniciar examen BPA**.

4.  Según la cantidad de reglas que se evalúen para el rol o grupo que ha seleccionado, el análisis BPA puede tardar algunos minutos en terminarse.

### <a name="scanning-roles-by-using-windows-powershell-cmdlets"></a><a name=BKMK_PSscan></a>Exámenes de roles mediante cmdlets de Windows PowerShell
Use los procedimientos siguientes para examinar uno o más roles mediante cmdlets de Windows PowerShell.

> [!NOTE]
> En los procedimientos de esta sección no se muestran todos los cmdlets y parámetros del BPA. Para obtener más información sobre las operaciones de BPA en Windows PowerShell, en la sesión de Windows PowerShell, escriba **Get-Help***BPACmdlet***-Full**, donde *BPACmdlet* puede ser uno de los siguientes valores. También puede encontrar temas de ayuda de los cmdlets del BPA en [Windows Server TechCenter](https://go.microsoft.com/fwlink/p/?LinkId=240177).

-   **Get-BPAmodel**

-   **Invoke-BPAmodel**

-   **Get-BPAResult**

-   **Set-BPAResult**

#### <a name="to-scan-a-single-role-by-using-windows-powershell-cmdlets"></a><a name=BKMK_singlerole></a>Para examinar un único rol mediante cmdlets de Windows PowerShell

1.  Realice una de las siguientes acciones para ejecutar Windows PowerShell con permisos de usuario elevados.

    -   Para ejecutar Windows PowerShell como administrador desde la pantalla **Inicio** , haga clic con el botón derecho en el icono de **Windows PowerShell** en los resultados de **aplicaciones** y, a continuación, en la barra de la aplicación, haga clic en **Ejecutar como administrador**.

    -   Para ejecutar Windows PowerShell como administrador desde el escritorio, haga clic con el botón secundario en el acceso directo de **Windows PowerShell** en la barra de tareas y, a continuación, haga clic en **Ejecutar como administrador**.

2.  a partir de Windows PowerShell 3,0, los módulos de cmdlets se importan automáticamente en la sesión de Windows PowerShell la primera vez que se usa un cmdlet del módulo. No es necesario importar ni cargar el módulo cmdlet de BPA.

3.  para buscar los identificadores de modelo de todos los roles para los que se pueden realizar análisis BPA, escriba el cmdlet **Get-Bpamodel** , tal como se muestra en el ejemplo siguiente.

    `Get-Bpamodel`

4.  En los resultados del paso 3, busque los identificadores de los modelos del rol para los que desea ejecutar un análisis BPA.

5.  Escriba uno de los siguientes comandos para iniciar el análisis BPA para un rol específico. Para varios roles, separe los identificadores del modelo con comas.

    `Invoke-BPAmodel -modelId <modelID_from_Step3>`

    `Invoke-BPAmodel <modelID_from_Step3>`

    **Ejemplo:** `Invoke-BPAmodel -modelId Microsoft/Windows/DNSServer,Microsoft/Windows/FileServices`

    > [!NOTE]
    > El identificador de modelo incluye la ruta de acceso completa que se muestra en la columna **Id.** , por ejemplo, **Microsoft/Windows/Hyper-V**.

    También puede iniciar un análisis en un rol específico de los resultados del paso 3 mediante la canalización de los resultados del cmdlet `Get-BPAmodel` al cmdlet `Invoke-BPAmodel` , como se muestra en el siguiente ejemplo.

    `Get-BPAmodel <model_ID> | Invoke-BPAmodel`

    Al ejecutar este cmdlet sin especificar un identificador de modelo, se canalizan todos los modelos devueltos por el cmdlet `Get-BPAmodel` al cmdlet `Invoke-BPAmodel`, iniciando exámenes en todos los modelos que están disponibles en los servidores que se han agregado al grupo de servidores de Administrador del servidor.

#### <a name="to-scan-all-roles-by-using-windows-powershell-cmdlets"></a><a name=BKMK_allroles></a>Para examinar todos los roles mediante cmdlets de Windows PowerShell

1.  Abra una sesión de Windows PowerShell con derechos de usuario elevados, si aún no hay ninguna abierta. Consulte el procedimiento siguiente para conocer las instrucciones.

2.  Canalice todos los roles para los que se puedan realizar análisis BPA al cmdlet `Invoke-BPAmodel` para iniciar los análisis.

    `Get-BPAmodel | Invoke-BPAmodel`

3.  Una vez completado el análisis, Windows PowerShell devuelve resultados similares a los siguientes para cada rol que se examinó.

    ```
    modelId                 :  Microsoft/Windows/FileServices
    SubmodelId              :
    Success                 :  True
    Scantime                :  1/01/2012  12:18:40 PM
    ScantimeUtcOffset       :  -08:00:00
    detail                  :  {server_name1, server_name2}

    ```

## <a name="manage-scan-results"></a><a name=BKMK_manage></a>Administrar resultados de exámenes
Después de completarse un análisis BPA en la GUI, puede visualizar los resultados del examen en el mosaico de BPA. Cuando selecciona un resultado del mosaico, en un panel de previsualización del mosaico se muestran las propiedades de los resultados, incluida una indicación acerca de si el rol cumple con los procedimientos recomendados asociados. Si un resultado no es compatible y desea saber cómo resolver los problemas descritos en las propiedades de los resultados, los hipervínculos de las propiedades de resultados de errores y advertencias abren los temas de ayuda de resolución detallados en Windows Server TechCenter.

> [!NOTE]
> Los resultados de los análisis BPA no se guardan ni se archivan de forma automática. La ejecución de un nuevo examen en un modelo o submodelo sobreescribe los resultados del último examen. Para guardar los resultados del análisis BPA para archivarlos, imprimirlos o enviarlos a otros usuarios, vea el tema sobre cómo [ver los resultados del BPA desde sesiones de Windows PowerShell en diferentes formatos](#BKMK_formats) en esta sección.

### <a name="exclude-and-include-bpa-results"></a>Excluir e incluir los resultados del BPA
Si no necesita ver algunos resultados del BPA, como los resultados que se producen con frecuencia en los exámenes del BPA pero no requiere ninguna resolución, puede excluir los resultados mediante el uso de la GUI del BPA o de los cmdlets del BPA en Windows PowerShell. Puede volver a incluir los resultados cuando lo desee.

> [!NOTE]
> Cuando se excluyen resultados, también se excluyen de la visualización en los servidores administrados. Otros administradores no pueden ver los resultados excluidos en los servidores administrados. Para excluir los resultados de la vista solo en una consola de Administrador del servidor local, cree una consulta personalizada en lugar de usar el comando **excluir resultado** .

#### <a name="exclude-scan-results"></a><a name=BKMK_exclude></a>Excluir resultados de exámenes
El valor de configuración **Excluir** es persistente, es decir, los resultados excluidos permanecen de ese modo en los análisis futuros del mismo modelo en el mismo equipo, a menos que se vuelvan a incluir.

Puede excluir resultados de análisis mediante el cmdlet `Set-BPAResult` con el parámetro `Exclude` . Como en el icono de Analizador de procedimientos recomendados en Administrador del servidor, puede excluir objetos de resultados individuales o puede excluir también un conjunto de resultados cuyos campos (categoría, título y gravedad, por ejemplo) sean iguales a los valores especificados o los contengan. Por ejemplo, puede excluir todos los resultados de **Rendimiento** de un conjunto de resultados del análisis para un modelo.

> [!NOTE]
> Debe ejecutar al menos un análisis BPA en un modelo para poder usar los procedimientos de esta sección.

###### <a name="to-exclude-scan-results-by-using-the-gui"></a>Para excluir resultados de exámenes mediante la GUI

1.  Abra una página de rol o grupo de servidores en Administrador del servidor.

2.  En el icono de Analizador de procedimientos recomendados del rol o grupo de servidores, haga clic con el botón secundario en un resultado de la lista y, a continuación, haga clic en **excluir resultado**.

    El resultado ya no se muestra en la lista de resultados.

3.  Para ver los resultados excluidos en la GUI, ejecute la consulta **Resultados excluidos** integrada. Haga clic en **Consultas de búsqueda guardadas**y, a continuación, en **Resultados excluidos**.

    Una vez ejecutada la consulta **Resultados excluidos**, observe que el texto del subtítulo del icono, una descripción de los resultados mostrados en la lista, cambia a **Resultados excluidos**. En la lista, solo se muestran los resultados excluidos.

###### <a name="to-exclude-scan-results-by-using-windows-powershell-cmdlets"></a>Para excluir resultados de exámenes mediante cmdlets de Windows PowerShell

1.  Abra una sesión de Windows PowerShell con derechos de usuario elevados.

2.  Excluya resultados específicos de un análisis de modelo ejecutando el siguiente comando:

    `Get-BPAResult -modelId <model ID> | Where { $_.<Field Name> -eq Value} | Set-BPAResult -Exclude $true`

    El comando anterior recupera los elementos de resultados del análisis BPA para el identificador de modelo representado por el *identificador de modelo*.

    La segunda sección del comando filtra los resultados del cmdlet `Get-BPAResult` para recuperar solo los resultados del análisis para los que el valor de un campo de resultado, representado por *Nombre del campo*, coincide con el texto entre comillas.

    La sección final del comando, después del segundo carácter de canalización, excluye los resultados que se filtran en la sección anterior del cmdlet.

    **Ejemplo:** `Get-BPAResult -Microsoft/Windows/FileServices | Where { $_.Severity -eq Information} | Set-BPAResult -Exclude $true`

#### <a name="include-scan-results"></a>Incluir resultados de exámenes
Si desea ver resultados de exámenes que se han excluido, puede incluirlos. El valor de configuración **Incluir** es persistente, es decir, los resultados incluidos permanecen de ese modo en los análisis futuros del mismo modelo en el mismo equipo.

##### <a name="to-include-scan-results-by-using-the-gui"></a><a name=BKMK_gui></a>Para incluir resultados de exámenes mediante la GUI

1.  Abra una página de rol o grupo de servidores en Administrador del servidor.

2.  En el icono de Analizador de procedimientos recomendados del rol o grupo de servidores, haga clic con el botón secundario en un resultado excluido en la lista de consultas de **resultados excluidos** y, a continuación, haga clic en **incluir resultado**.

    El resultado ya no se muestra en la lista de resultados excluidos. Borre la consulta mediante un clic en **Borrar todo** para ver el resultado incluido en la lista de todos los resultados incluidos.

##### <a name="to-include-scan-results-by-using-windows-powershell-cmdlets"></a><a name=BKMK_cmdlets></a>Para incluir resultados de exámenes mediante cmdlets de Windows PowerShell

1.  Abra una sesión de Windows PowerShell con derechos de usuario elevados.

2.  Para incluir resultados específicos del análisis de un modelo, escriba el siguiente comando y, a continuación, presione **Entrar**.

    `Get-BPAResult -modelId <model Id> | Where { $_.<Field Name> -eq Value } | Set-BPAResult -Exclude $false`

    El comando anterior recupera los elementos de resultados del análisis BPA para el modelo representado por el *identificador de modelo*.

    La segunda parte del comando, después del primer carácter de canalización ( **|** ) filtra los resultados del cmdlet **Get-BPAResult** para recuperar solo los resultados del análisis para los que el valor del campo de resultado, representado por *el nombre de campo*, coincide con el texto entre comillas.

    La parte final del comando, a continuación del segundo carácter de canalización, incluye los resultados que se filtran en la segunda parte del cmdlet, estableciendo el valor del parámetro **-Exclude** en **false**.

    **Ejemplo:** `Get-BPAResult -Microsoft/Windows/FileServices | Where { $_.Severity -eq Information} | Set-BPAResult -Exclude $false`

### <a name="view-and-export-bpa-scan-results-in-windows-powershell"></a>Visualizar y exportar los resultados del análisis BPA en Windows PowerShell
Para ver y administrar los resultados de exámenes mediante cmdlets de Windows PowerShell, consulte los procedimientos siguientes. Antes de que pueda usar cualquiera de los siguientes procedimientos, ejecute un análisis BPA en un modelo o submodelo, como mínimo.

#### <a name="to-view-results-of-the-most-recent-scan-of-a-role-by-using-windows-powershell"></a><a name=BKMK_recentPS></a>Para ver los resultados del examen más reciente de un rol mediante Windows PowerShell

1.  Abra una sesión de Windows PowerShell con derechos de usuario elevados.

2.  Obtenga los resultados del examen más reciente para un identificador de modelo especificado. Escriba lo siguiente, en el que el modelo se representa mediante el *identificador de modelo*y, a continuación, presione **entrar**. Puede obtener los resultados de los identificadores de varios modelos al separar los identificadores del modelo con comas.

    `Get-BPAResult <model ID>`

    **Ejemplo:** `Get-BPAResult Microsoft/Windows/DNSServer,Microsoft/Windows/FileServices`

    Si examinó un submodelo de un modelo, como un servicio de rol, obtenga los resultados solo para ese submodelo incluyendo el identificador del submodelo en el cmdlet.

    **Ejemplo:** `Get-BPAResult Microsoft/Windows/FileServices -SubmodelID FSRM`

#### <a name="to-view-or-save-bpa-results-from-windows-powershell-sessions-in-different-formats"></a><a name=BKMK_formats></a>Para ver o guardar los resultados de BPA desde las sesiones de Windows PowerShell en diferentes formatos

-   En Windows PowerShell, cada resultado del BPA se asemeja a lo siguiente.

    ```
    ResultNumber     :  14
    ResultId         :  1557706192
    modelId          :  Microsoft/Windows/FileServices
    SubmodelId       :  FSRM
    RuleId           :  16
    computerName     :  server_name1, server_name2
    Context          :  FileServices
    Source           :  server_name1
    Severity         :  Information
    Category         :  Configuration
    Title            :  Access Denied remediation requires remote management be enabled on this server
    Problem          :
    Impact           :
    Resolution       :
    compliance       :  The File Server Best Practices Analyzer scan has determined that you are in compliance with this best practice.
    help             :
    Excluded         :  False

    ```

    Lleve a cabo cualquiera de las siguientes opciones.

    -   Para dar formato a los resultados de BPA, ejecute el siguiente cmdlet, agregando las propiedades de los resultados que desea ver desde el ejemplo anterior.

        `Get-BPAResult model ID | format-Table -Property <property1,property2,property3...>`

        **Ejemplo:** `Get-BPAResult Microsoft/Windows/FileServices | format-Table -Property modelId,SubmodelId,computerName,Source,Severity,Category,Title,Problem,Impact,Resolution,compliance,help`

    -   Para dar formato a los resultados de BPA en un visualizador de cuadrículas basado en GUI, con un filtro de cadena de texto, y encabezados de columnas en las que se puede hacer clic para ordenar los resultados, ejecute el siguiente cmdlet.

        `Get-BPAResult <model ID> | OGV`

    -   Para exportar los resultados del BPA a un archivo HTML que se puede archivar o enviar a los destinatarios de correo electrónico, ejecute el siguiente cmdlet, donde *path* representa la ruta de acceso y el nombre del archivo en el que desea guardar los resultados HTML.

        `Get-BPAResult <model ID> | convertTo-Html | Set-Content <path>`

        **Ejemplo:** `Get-BPAResult Microsoft/Windows/FileServices | convertTo-Html | Set-Content C:\BPAResults\FileServices.htm`

    -   Para exportar los resultados del BPA a un archivo de texto de valores separados por comas (CSV), ejecute el siguiente cmdlet, donde *path* representa la ruta de acceso y el nombre del archivo de texto en el que desea guardar los resultados CSV. Los resultados de CSV se pueden importar en Microsoft Excel u otros programas que muestran datos en hojas de cálculo o cuadrículas.

        `Get-BPAResult <model ID> | Export-CSV <path>`

        **Ejemplo:** `Get-BPAResult Microsoft/Windows/FileServices | Export-CSV C:\BPAResults\FileServices.txt`

## <a name="see-also"></a>Consulta también
[Analizador de procedimientos recomendados el contenido de la resolución en el TechCenter de Windows Server](https://go.microsoft.com/fwlink/p/?LinkId=241597)
[filtrar, ordenar y consultar datos en Administrador del servidor mosaicos](filter-sort-and-query-data-in-server-manager-tiles.md)
[administrar varios servidores remotos con administrador del servidor](manage-multiple-remote-servers-with-server-manager.md)
