---
title: Ejecutar exámenes del analizador de prácticas recomendadas y administrar Results_1 de examen
description: Administrador de servidores
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 232f1c80-88ef-4a39-8014-14be788c2766
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 58fdf35e1d06fe7176b122155b2768094f2b01b4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838226"
---
# <a name="run-best-practices-analyzer-scans-and-manage-scan-results"></a>Ejecución de análisis del Analizador de procedimientos recomendados y administración de los resultados de los análisis

>Se aplica a: Windows Server 2016

En la administración de Windows, los *procedimientos recomendados* son instrucciones que se consideran ideales, en circunstancias típicas, para configurar un servidor, según la definición de los expertos. Por ejemplo, para la mayoría de las aplicaciones del servidor, se recomienda mantener abiertos solo aquellos puertos necesarios para que se comuniquen las aplicaciones con otros equipos en red y bloquear los puertos que no se usan. Aunque no seguir los procedimientos recomendados, incluso los cruciales, no tiene por qué presentar problemas, puede conllevar configuraciones de servidor con un rendimiento bajo, confiabilidad insuficiente, conflictos inesperados, mayores riesgos de seguridad u otros problemas posibles.

Best Practices Analyzer (BPA) es una herramienta de administración de servidor que está disponible en Windows Server 2012 R2, Windows Server 2012 y Windows Server 2008 R2. BPA puede ayudar a los administradores a reducir las infracciones de práctica examinando los roles que están instalados en servidores administrados que ejecutan Windows Server 2012 o Windows Server 2008 R2, y seguir los procedimientos recomendados para el Administrador de informes.

Se puede ejecutar análisis de Best Practices Analyzer (BPA) desde el administrador del servidor, mediante la GUI del BPA o mediante cmdlets de Windows PowerShell. a partir de Windows Server 2012, se pueden analizar uno o varios roles al mismo tiempo, en varios servidores, si utiliza el icono de Best Practices Analyzer en la consola de administrador del servidor o cmdlets de Windows PowerShell para ejecutar exámenes. También puede indicar al BPA que excluya u omita resultados de exámenes que no desee ver.

En este tema se incluyen las siguientes secciones.

-   [Buscar BPA](#BKMK_find)

-   [Cómo funciona BPA](#BKMK_how)

-   [Analizador de procedimientos recomendados de realizar exámenes en roles](#BKMK_BPAscan)

-   [Administrar los resultados del examen](#BKMK_manage)

## <a name="BKMK_find"></a>Buscar BPA
Puede encontrar el icono de Best Practices Analyzer en el rol y las páginas de grupos de servidor del administrador del servidor en Windows Server 2012 R2 y Windows Server 2012 o pueden abrir una sesión de Windows PowerShell con derechos de usuario con privilegios elevados para ejecutar los cmdlets del analizador de procedimientos recomendados.

## <a name="BKMK_how"></a>Cómo funciona BPA
BPA realiza mediante la medición del cumplimiento de un rol con reglas de procedimientos recomendados en ocho categorías diferentes de eficacia, confiabilidad y la confiabilidad. Los resultados de las mediciones pueden enmarcarse en uno de los tres niveles de gravedad descritos en la siguiente tabla.

|Nivel de gravedad|Descripción|
|---------|--------|
|Error|Se devuelven resultados de error cuando un rol no cumple con las reglas de procedimientos recomendados y pueden preverse problemas de funcionalidad.|
|Información de|Se devuelven resultados de información cuando un rol cumple con las reglas de procedimientos recomendados.|
|Advertencia|Se devuelven resultados de advertencia cuando los resultados de incumplimiento pueden causar problemas si no se realizan cambios. La aplicación puede ser compatible tal y como funciona en ese momento, pero es posible que no cumpla las condiciones de una regla si no se realizan cambios en su configuración o las opciones de directiva. Por ejemplo, es posible que un examen de Servicios de Escritorio remoto muestre un resultado de advertencia si un servidor de licencias no está disponible para el rol porque, aunque no haya conexiones remotas activas en el momento del examen, no disponer de un servidor de licencias impide que las nuevas conexiones remotas obtengan licencias de acceso de cliente válidas.|

### <a name="rule-categories"></a>Categorías de las reglas
La tabla siguiente describen las categorías de reglas de procedimientos recomendados contra qué roles se evalúan durante un analizador de procedimientos recomendados de análisis.

|Nombre de la categoría|Descripción|
|---------|--------|
|Seguridad|Se aplican las reglas de seguridad para medir el riesgo relativo de un rol de exposición a amenazas como usuarios no autorizados o malintencionados, la pérdida o robo de datos confidenciales o patentados.|
|Rendimiento|Se aplican las reglas de rendimiento para medir la capacidad de un rol para procesar solicitudes y realizar sus tareas prescritas en la empresa en esperado períodos de tiempo, dada la carga de trabajo del rol.|
|Configuración|Las reglas de configuración se aplican para identificar valores de rol que es posible que deban modificarse para que el rol funcione de forma óptima. Las reglas de configuración pueden ayudar a impedir conflictos en configuraciones que pueden producir mensajes de error o impedir que el rol realice sus tareas prescritas en una empresa.|
|Directiva|Se aplican las reglas de directiva para identificar la configuración del registro de directiva de grupo o de Windows que puedan requerir modificaciones para un rol funcione de forma óptima y segura.|
|Operación|Se aplican las reglas de operación para identificar posibles errores de un rol al realizar tareas prescritas en la empresa.|
|Antes de la implementación|Las reglas previas a la implementación se aplican antes de que un rol instalado se implemente en la empresa. Antes de que el rol se use en producción, permiten que los administradores evalúen si se cumple con las recomendaciones.|
|Después de la implementación|Las reglas posteriores a la implementación se aplican una vez que se hayan iniciado todos los servicios necesarios para un rol y antes de que se esté ejecutando en la empresa.|
|Requisitos previos|Las reglas de requisitos previos explican las opciones de configuración, las configuraciones de directiva y las características necesarias para un rol de modo que BPA pueda aplicar reglas específicas de otras categorías. Si aparece un requisito previo en los resultados del examen, indica una configuración incorrecta, un programa que falta, una directiva deshabilitada o habilitada incorrectamente, una configuración de clave del Registro u otro tipo de configuración que impidió que BPA aplicara una o varias reglas durante un examen. Un resultado de requisito previo no indica la compatibilidad o incompatibilidad. Significa que no se pudo aplicar una regla y, por tanto, no forma parte de los resultados del examen.|

## <a name="BKMK_BPAscan"></a>Analizador de procedimientos recomendados de realizar exámenes en roles
Puede realizar exámenes del BPA en los roles mediante el uso la GUI del BPA en el administrador del servidor, o mediante cmdlets de Windows PowerShell.

En Windows Server 2012 R2 y Windows Server 2012, algunos roles solicitará que especifique los parámetros adicionales, como los nombres de los recursos compartidos que ejecutan partes del rol o los identificadores de submodelo, antes de iniciar un examen del BPA o servidores específicos. Para los exámenes del BPA en modelos que requieren que se especifiquen parámetros adicionales, use cmdlets del BPA; la GUI del BPA no puede aceptar parámetros adicionales como los identificadores del submodelo. Por ejemplo, el identificador de submodelo **FSRM** representa el submodelo de BPA Servicios de archivo del Administrador de recursos del servidor de archivos, un servicio de rol de Servicios de archivo y almacenamiento. Para ejecutar un examen solo en el servicio de rol de administrador de recursos del servidor de archivos, ejecute un análisis BPA mediante cmdlets de Windows PowerShell y agregue el parámetro `SubmodelId` al cmdlet.

Aunque no se puede pasar parámetros adicionales a un análisis iniciado en la GUI del BPA, el mosaico de BPA en el administrador del servidor muestra los resultados del análisis BPA más reciente, independientemente de cómo se inició el análisis.

-   [Exámenes de roles mediante la GUI del BPA](#BKMK_GUIscan)

-   [Exámenes de roles mediante cmdlets de Windows PowerShell](#BKMK_PSscan)

### <a name="BKMK_GUIscan"></a>Exámenes de roles mediante la GUI del BPA
Siga estos pasos para examinar uno o más roles en la GUI del BPA.

##### <a name="to-scan-roles-by-using-the-bpa-gui"></a>Para examinar roles mediante la GUI del BPA

1.  Realice una de las siguientes acciones para abrir el administrador del servidor si no está ya abierto.

    -   En la barra de tareas de Windows, haga clic en el botón Administrador del servidor.

    -   En el **iniciar** pantalla, haga clic en el icono de administrador del servidor.

2.  En el panel de navegación, abra una página de rol o grupo.

    Al ejecutar análisis BPA desde una página de rol o grupo, se examinan todos los roles que se instalan en servidores de ese grupo.

3.  En el **tareas** menú de la **Best Practices Analyzer** icono, haga clic en **iniciar examen BPA**.

4.  Según la cantidad de reglas que se evalúen para el rol o grupo que ha seleccionado, el análisis BPA puede tardar algunos minutos en terminarse.

### <a name="BKMK_PSscan"></a>Exámenes de roles mediante cmdlets de Windows PowerShell
Utilice los procedimientos siguientes para examinar uno o varios roles mediante cmdlets de Windows PowerShell.

> [!NOTE]
> En los procedimientos de esta sección no se muestran todos los cmdlets y parámetros del BPA. Para obtener más información acerca de las operaciones del BPA en Windows PowerShell, en la sesión de Windows PowerShell, escriba **Get-help***BPACmdlet***-full**, donde *BPACmdlet* puede ser uno de los siguientes valores. También puede encontrar temas de Ayuda de cmdlet BPA en el [TechCenter de Windows Server](https://go.microsoft.com/fwlink/p/?LinkId=240177).

-   **Get-BPAmodel**

-   **Invoke-BPAmodel**

-   **Get-BPAResult**

-   **Set-BPAResult**

#### <a name="BKMK_singlerole"></a>Para examinar un único rol mediante cmdlets de Windows PowerShell

1.  Realice una de las siguientes acciones para ejecutar Windows PowerShell con derechos de usuario elevados.

    -   Para ejecutar Windows PowerShell como administrador desde el **iniciar** pantalla, haga clic en el **Windows PowerShell** disponer en mosaico en el **aplicaciones** resultados y, a continuación, en la barra de la aplicación, haga clic en **Ejecutar como administrador**.

    -   Para ejecutar Windows PowerShell como administrador desde el escritorio, haga clic en el **Windows PowerShell** acceso directo en la barra de tareas y, a continuación, haga clic en **ejecutar como administrador**.

2.  a partir de Windows PowerShell 3.0, los módulos de cmdlets se importan automáticamente en la sesión de Windows PowerShell la primera vez que se usa un cmdlet del módulo. No es necesario importar ni cargar el módulo cmdlet de BPA.

3.  para buscar los identificadores de modelo de todos los roles que BPA se puedan realizar análisis escribiendo el **Get-Bpamodel** cmdlet, como se muestra en el ejemplo siguiente.

    `Get-Bpamodel`

4.  En los resultados del paso 3, busque los identificadores de los modelos del rol para los que desea ejecutar un análisis BPA.

5.  Escriba uno de los siguientes comandos para iniciar el análisis BPA para un rol específico. Para varios roles, separe los identificadores del modelo con comas.

    `Invoke-BPAmodel -modelId <modelID_from_Step3>`

    `Invoke-BPAmodel <modelID_from_Step3>`

    **Ejemplo:**`Invoke-BPAmodel -modelId Microsoft/Windows/DNSServer,Microsoft/Windows/FileServices`

    > [!NOTE]
    > El identificador de modelo incluye la ruta de acceso completa que se muestra en la columna **Id.**, por ejemplo, **Microsoft/Windows/Hyper-V**.

    También puede iniciar un análisis en un rol específico de los resultados del paso 3 mediante la canalización de los resultados del cmdlet `Get-BPAmodel` al cmdlet `Invoke-BPAmodel` , como se muestra en el siguiente ejemplo.

    `Get-BPAmodel <model_ID> | Invoke-BPAmodel`

    Si ejecuta este cmdlet sin especificar un identificador de modelo canaliza todos los modelos devuelven por la `Get-BPAmodel` cmdlet en el `Invoke-BPAmodel` cmdlet, que se inician análisis en todos los modelos que están disponibles en los servidores que se han agregado al grupo de servidores de administrador del servidor.

#### <a name="BKMK_allroles"></a>Para examinar todos los roles mediante cmdlets de Windows PowerShell

1.  Abra una sesión de Windows PowerShell con derechos de usuario con privilegios elevados, si uno no está abierto. Consulte el procedimiento siguiente para conocer las instrucciones.

2.  Canalice todos los roles para los que se puedan realizar análisis BPA al cmdlet `Invoke-BPAmodel` para iniciar los análisis.

    `Get-BPAmodel | Invoke-BPAmodel`

3.  Una vez completada la detección, Windows PowerShell devuelve resultados similares a los siguientes para cada rol analizado.

    ```
    modelId                 :  Microsoft/Windows/FileServices
    SubmodelId              :
    Success                 :  True
    Scantime                :  1/01/2012  12:18:40 PM
    ScantimeUtcOffset       :  -08:00:00
    detail                  :  {server_name1, server_name2}

    ```

## <a name="BKMK_manage"></a>Administrar los resultados del examen
Después de completarse un análisis BPA en la GUI, puede visualizar los resultados del examen en el mosaico de BPA. Cuando selecciona un resultado del mosaico, en un panel de previsualización del mosaico se muestran las propiedades de los resultados, incluida una indicación acerca de si el rol cumple con los procedimientos recomendados asociados. Si un resultado no es compatible, y desea saber cómo resolver los problemas descritos en las propiedades del resultado, los hipervínculos de propiedades de resultados de advertencia y error abren los temas de Ayuda de resolución detallados en Windows Server TechCenter.

> [!NOTE]
> Los resultados de los análisis BPA no se guardan ni se archivan de forma automática. La ejecución de un nuevo examen en un modelo o submodelo sobreescribe los resultados del último examen. Para guardar los resultados del análisis BPA para archivarlos, imprimirlos o enviarlos a otros usuarios, vea el tema sobre cómo [ver los resultados del BPA desde sesiones de Windows PowerShell en diferentes formatos](#BKMK_formats) en esta sección.

### <a name="exclude-and-include-bpa-results"></a>Excluir e incluir los resultados del BPA
Si no necesita ver algunos resultados BPA, como los resultados que se producen con frecuencia en los análisis BPA pero requiere que no tiene solución, puede excluir los resultados, mediante la GUI del BPA o los cmdlets del BPA en Windows PowerShell. Puede volver a incluir los resultados cuando lo desee.

> [!NOTE]
> Cuando se excluyen resultados, también se excluyen de la visualización en los servidores administrados. Otros administradores no pueden ver los resultados excluidos en los servidores administrados. Para excluir resultados de la vista en sólo una consola de administrador del servidor local, cree una consulta personalizada en lugar de usar el **excluir resultado** comando.

#### <a name="BKMK_exclude"></a>Excluir los resultados del examen
El valor de configuración **Excluir** es persistente, es decir, los resultados excluidos permanecen de ese modo en los análisis futuros del mismo modelo en el mismo equipo, a menos que se vuelvan a incluir.

Puede excluir resultados de análisis mediante el cmdlet `Set-BPAResult` con el parámetro `Exclude` . Como se muestra en el icono de Best Practices Analyzer en el administrador del servidor, puede excluir objetos de resultados individuales o puede excluir también un conjunto de resultados cuyos campos (categoría, título y gravedad, por ejemplo) sean iguales o contienen valores especificados. Por ejemplo, puede excluir todos los resultados de **Rendimiento** de un conjunto de resultados del análisis para un modelo.

> [!NOTE]
> Debe ejecutar al menos un análisis BPA en un modelo para poder usar los procedimientos de esta sección.

###### <a name="to-exclude-scan-results-by-using-the-gui"></a>Para excluir resultados de exámenes mediante la GUI

1.  Abra la página de un rol o el servidor de grupo en Administrador del servidor.

2.  En el icono de Best Practices Analyzer para el rol o grupo de servidores, haga clic en un resultado de la lista y, a continuación, haga clic en **excluir resultado**.

    El resultado ya no se muestra en la lista de resultados.

3.  Para ver los resultados excluidos en la GUI, ejecute la consulta **Resultados excluidos** integrada. Haga clic en **Consultas de búsqueda guardadas**y, a continuación, en **Resultados excluidos**.

    Una vez ejecutada la consulta **Resultados excluidos**, observe que el texto del subtítulo del icono, una descripción de los resultados mostrados en la lista, cambia a **Resultados excluidos**. En la lista, solo se muestran los resultados excluidos.

###### <a name="to-exclude-scan-results-by-using-windows-powershell-cmdlets"></a>Para excluir resultados de exámenes mediante cmdlets de Windows PowerShell

1.  Abra una sesión de Windows PowerShell con derechos de usuario elevados.

2.  Excluya resultados específicos de un análisis de modelo ejecutando el siguiente comando:

    `Get-BPAResult -modelId <model ID> | Where { $_.<Field Name> -eq "Value"} | Set-BPAResult -Exclude $true`

    El comando anterior recupera los elementos de resultado del análisis BPA para el identificador de modelo representado por *Id. del modelo*.

    La segunda sección del comando filtra los resultados del cmdlet `Get-BPAResult` para recuperar solo los resultados del análisis para los que el valor de un campo de resultado, representado por *Nombre del campo*, coincide con el texto entre comillas.

    La sección final del comando, después del segundo carácter de canalización, excluye los resultados que se filtran en la sección anterior del cmdlet.

    **Ejemplo:**`Get-BPAResult -Microsoft/Windows/FileServices | Where { $_.Severity -eq "Information"} | Set-BPAResult -Exclude $true`

#### <a name="include-scan-results"></a>Incluir resultados de exámenes
Si desea ver resultados de exámenes que se han excluido, puede incluirlos. El valor de configuración **Incluir** es persistente, es decir, los resultados incluidos permanecen de ese modo en los análisis futuros del mismo modelo en el mismo equipo.

##### <a name="BKMK_gui"></a>Para incluir resultados de exámenes mediante la GUI

1.  Abra la página de un rol o el servidor de grupo en Administrador del servidor.

2.  En el icono de Best Practices Analyzer para el rol o grupo de servidores, haga clic en un resultado excluido en la **resultados excluidos** consulta de lista y, a continuación, haga clic en **incluir resultado**.

    El resultado ya no se muestra en la lista de resultados excluidos. Borre la consulta mediante un clic en **Borrar todo** para ver el resultado incluido en la lista de todos los resultados incluidos.

##### <a name="BKMK_cmdlets"></a>Para incluir resultados de exámenes mediante cmdlets de Windows PowerShell

1.  Abra una sesión de Windows PowerShell con derechos de usuario elevados.

2.  Para incluir resultados específicos del análisis de un modelo, escriba el siguiente comando y, a continuación, presione **Entrar**.

    `Get-BPAResult -modelId <model Id> | Where { $_.<Field Name> -eq "Value" } | Set-BPAResult -Exclude $false`

    El comando anterior recupera los elementos de resultados de examen BPA para el modelo representado por *Id. del modelo*.

    La segunda parte del comando, a continuación del primer carácter de canalización ( **|** ) filtra los resultados de la **Get-BPAResult** cmdlet para recuperar solo los resultados del análisis para que el valor del resultado campo, representado por *nombre de campo*, coincide con el texto entre comillas.

    La parte final del comando, a continuación del segundo carácter de canalización, incluye los resultados que se filtran en la segunda parte del cmdlet, estableciendo el valor del parámetro **-Exclude** en **false**.

    **Ejemplo:**`Get-BPAResult -Microsoft/Windows/FileServices | Where { $_.Severity -eq "Information"} | Set-BPAResult -Exclude $false`

### <a name="view-and-export-bpa-scan-results-in-windows-powershell"></a>Visualizar y exportar los resultados del análisis BPA en Windows PowerShell
Para ver y administrar resultados de exámenes mediante cmdlets de Windows PowerShell, consulte los procedimientos siguientes. Antes de que pueda usar cualquiera de los siguientes procedimientos, ejecute un análisis BPA en un modelo o submodelo, como mínimo.

#### <a name="BKMK_recentPS"></a>Para ver los resultados del examen más reciente de un rol mediante Windows PowerShell

1.  Abra una sesión de Windows PowerShell con derechos de usuario elevados.

2.  Obtenga los resultados del examen más reciente para un identificador de modelo especificado. Escriba lo siguiente, donde el modelo está representado por *Id. del modelo*y, a continuación, presione **ENTRAR**. Puede obtener los resultados de los identificadores de varios modelos al separar los identificadores del modelo con comas.

    `Get-BPAResult <model ID>`

    **Ejemplo:** `Get-BPAResult Microsoft/Windows/DNSServer,Microsoft/Windows/FileServices`

    Si ha examinado el submodelo de un modelo, por ejemplo, un servicio de rol, obtener los resultados solo para ese submodelo incluyendo el identificador de submodelo en el cmdlet.

    **Ejemplo:** `Get-BPAResult Microsoft/Windows/FileServices -SubmodelID FSRM`

#### <a name="BKMK_formats"></a>Para ver o guardar los resultados del BPA desde sesiones de Windows PowerShell en diferentes formatos

-   En Windows PowerShell, cada resultado del BPA es similar al siguiente.

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

    Realice una de las siguientes acciones:

    -   Para dar formato a los resultados de BPA, ejecute el siguiente cmdlet, agregando las propiedades de los resultados que desea ver desde el ejemplo anterior.

        `Get-BPAResult model ID | format-Table -Property <property1,property2,property3...>`

        **Ejemplo:**`Get-BPAResult Microsoft/Windows/FileServices | format-Table -Property modelId,SubmodelId,computerName,Source,Severity,Category,Title,Problem,Impact,Resolution,compliance,help`

    -   Para dar formato a los resultados de BPA en un visualizador de cuadrículas basado en GUI, con un filtro de cadena de texto, y encabezados de columnas en las que se puede hacer clic para ordenar los resultados, ejecute el siguiente cmdlet.

        `Get-BPAResult <model ID> | OGV`

    -   Para exportar los resultados del BPA a un archivo HTML que pueda archivarse o enviarse a destinatarios de correo electrónico, ejecute el siguiente cmdlet, donde *ruta* representa el nombre de ruta de acceso y a la que desea guardar los resultados HTML.

        `Get-BPAResult <model ID> | convertTo-Html | Set-Content <path>`

        **Ejemplo:**`Get-BPAResult Microsoft/Windows/FileServices | convertTo-Html | Set-Content C:\BPAResults\FileServices.htm`

    -   Para exportar los resultados del BPA a un archivo de texto de valores separados por comas (CSV), ejecute el siguiente cmdlet, donde *ruta* representa el nombre de archivo de texto y la ruta de acceso a la que desea guardar los resultados CSV. Los resultados CSV pueden importarse en Microsoft Excel u otros programas que muestran datos en hojas de cálculo o cuadrículas.

        `Get-BPAResult <model ID> | Export-CSV <path>`

        **Ejemplo:**`Get-BPAResult Microsoft/Windows/FileServices | Export-CSV C:\BPAResults\FileServices.txt`

## <a name="see-also"></a>Vea también
[Contenido de resolución del analizador de procedimientos recomendados en Windows Server TechCenter](https://go.microsoft.com/fwlink/p/?LinkId=241597)
[filtrar, ordenar y consultar los datos en iconos del administrador del servidor](filter-sort-and-query-data-in-server-manager-tiles.md)
[Manage Multiple, remote Servers con el administrador del servidor](manage-multiple-remote-servers-with-server-manager.md)
