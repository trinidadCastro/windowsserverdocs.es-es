---
title: "Agregar entradas a configurar, complementos, el estado rápido y vínculos de ayuda"
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c0a8f10d-fd85-4c8d-b9bb-176cb1db1f46
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6d3303f2c6d84932ad9d5dee8a547cd478447732
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="add-entries-to-setup-add-ins-quick-status-and-help-links"></a>Agregar entradas a configurar, complementos, el estado rápido y vínculos de ayuda

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Puedes agregar tareas a la **instalación**, **ADD-INS**, **estado rápido** listas de tareas y puedes agregar vínculos a la sección vínculos de la Comunidad en la página principal del panel. Las tareas y vínculos se agregan a estas listas y la sección colocando un archivo XML llamado OEMHomePageContent.home o un archivo de recursos incrustados llamado OEMHomePageContent.dll en %ProgramFiles%\Windows Server\Bin\Addins\Home. El archivo de recursos incrustados puede usarse para localizar el texto en las tareas y vínculos que agregues. El archivo .home contiene las definiciones XML de las tareas y vínculos.  
  
## <a name="adding-tasks-to-the-setup-add-ins-quick-status-task-lists-and-adding-links-to-help-task"></a>Agregar tareas a la configuración, los complementos, listas de tareas de estado rápido y agregar vínculos a la tarea de ayuda  
 Puede agregar tareas a la **el programa de instalación**, **ADD-INS**, **estado rápido** tareas listas y vínculos a la **ayuda** tarea definiendo las tareas y vínculos con XML, opcionalmente crear el archivo de recursos incrustados e instalar el archivo en el servidor. Si el archivo XML se instala en el servidor sin un archivo de recursos, debe llamarse OEMHomePageContent.home. Si se usa un ensamblado para instalar tanto un archivo XML como un archivo de recursos, debe llamarse OEMHomePageContent.dll y debe estar firmado con Authenticode.  
  
### <a name="define-the-tasks-and-links"></a>Definir las tareas y vínculos  
 Puedes usar un editor de texto como el Bloc de notas para crear el archivo .home o, si también vas a crear un archivo de recursos incrustados puedes usar Visual Studio 2010 o posterior para definir los archivos. El siguiente procedimiento muestra cómo usar Visual Studio 2010 o posterior para crear los archivos.  
  
##### <a name="to-define-the-tasks-and-links"></a>Para definir las tareas y vínculos  
  
1.  Abre Visual Studio 2010 o posterior como administrador haciendo clic en el programa en el menú Inicio y seleccionar **ejecutar como administrador**.  
  
2.  Haz clic en **archivo**, haz clic en **nueva**y, a continuación, haz clic en **proyecto**.  
  
3.  En la **plantillas** panel, haz clic en **biblioteca de clases**, tipo **OEMHomePageContent** en la **nombre** cuadro y, a continuación, haz clic en **Aceptar**.  
  
4.  Eliminar el archivo Class1.cs.  
  
5.  En el nuevo proyecto, haz clic **agregar**y, a continuación, haz clic en **nuevo elemento**.  
  
6.  En la **plantillas** panel, haz clic en **archivo XML**, tipo **OEMHomePageContent.home** en la **nombre** cuadro y, a continuación, haz clic en **agregar**.  
  
    > [!NOTE]
    >  Si el archivo XML se instala sin un archivo de recursos, debe llamarse OEMHomePageContent.home. Si se incluye en un ensamblado, puede recibir cualquier nombre siempre que tenga la extensión .home.  
  
7.  Agrega el siguiente código XML al archivo OEMHomePageContent.home:  
  
    ```  
  
    <Tasks version=?2.0? xmlns=?https://schemas.microsoft.com/WindowsServerSolutions/2010/01/Dashboard>  
       <SetupMyServerTasks>  
          <Task name="MyTask"  
             description="MyTaskDescription"  
             id="GUID">  
                  <Action   
                  name=?MyAction1Name?   
                  image=?IconForAction1?  
                  type=?TaskType?  
                  exelocation=?ActionExeLocation? />  
                  <Action   
                  name=?MyAction2Name?   
                  image=?IconForAction2?  
                  type=?TaskType?  
                  exelocation=?ActionExeLocation? />  
                   ¦  
           </Task>  
                   ¦  
        </SetupMyServerTasks>  
    <MailServiceTasks>  
         <!-- Same schema as in œSetupMyServerTasks? but the tasks are shown in œConnect to Email Service? category. -->  
    </MailServiceTasks>  
    <LineOfBusinessTasks>  
         <!-- Same schema as in œSetupMyServerTasks? but the tasks are shown in œAdd-ins? category. -->  
  
    <GetQuickStatusTasks>  
          <Task name="MyQuickStatusTask1"  
             description="MyQuickStatusTask1Desc   "  
             id="GUID"  
             assembly="AssemblyName of quick status query implementation"  
             class="ClassName of quick status query implementation"           
             replaceid="GUID"/>  
               <!--  Same schema as Actions in œSetupMyServerTasks? -->   
             </Task>  
    </GetQuickStatusTasks>  
       <Links>  
          <Link  
             ID=?GUID?  
             Title="Displayed text of the link"  
             Description="A very short description"  
             ShellExecPath="Path to the application or URL"/>  
       </Links>  
    </Tasks>  
    ```  
  
     Donde:  
  
    |Atributo|Descripción|  
    |---------------|-----------------|  
    |Nombre (tarea)|El nombre que se muestra para la tarea en la lista. Si creas un archivo de recursos incrustados, el valor de este atributo es el recurso de cadena.|  
    |Descripción (tarea)|La descripción de la tarea. Si creas un archivo de recursos incrustados, el valor de este atributo es el recurso de cadena.|  
    |Identificador (tarea)|El identificador de la tarea. Este identificador debe ser un GUID. Crear un nuevo GUID para una **exe** tarea, pero para una **global** tarea, usan el GUID que creaste al definir la tarea para el panel de tareas de la subpestaña. Para obtener más información sobre cómo crear un GUID, consulta [crear Guid (guidgen.exe)](https://go.microsoft.com/fwlink/?LinkId=116098).|  
    |imagen|Este campo se omitirá.|  
    |Nombre (acción)|Muestra el nombre de la tarea.|  
    |Tipo (acción)|Describe el tipo de tarea. La tarea puede ser uno de los siguientes:- **global** tarea, **exe**, o una tarea de dirección url. Un **global** tarea es la misma tarea global que creaste al definir las tareas del panel de tareas en la subpestaña. ¿Para obtener más información sobre cómo crear una tarea global que puede usarse en ambas panel de tareas de la subpestaña y las listas de tareas de inicio o tareas comunes de la página principal, consulta œCreating las clases de soporte? ¿en œHow a: crear un subpestaña? de la [SDK de soluciones de Windows Server](https://go.microsoft.com/fwlink/?LinkID=248648). Un **exe** tarea puede usarse para ejecutar aplicaciones de la tareas de inicio o listas de tareas comunes.|  
    |exelocation|La ruta de acceso a la aplicación que está asociada con la tarea. Este atributo solo se usa para **exe** tareas.|  
    |replaceid|El identificador de la tarea que se reemplaza con esta tarea.|  
    |ensamblado|El nombre del ensamblado que proporciona la clase para implementar la consulta rápida de estado. El ensamblado debe encontrarse en el programa files\ windows server\bin\\.|  
    |clase|El nombre de la clase implementa la consulta rápida de estado. La clase necesita implementar **ITaskStatusQuery** interfaz.|  
    |Título (vínculo)|El texto que se muestra para el vínculo. Si creas un archivo de recursos incrustados, el valor de este atributo es el recurso de cadena.|  
    |Descripción (vínculo)|La descripción del destino del vínculo. Si creas un archivo de recursos incrustados, el valor de este atributo es el recurso de cadena.|  
    |ShellExecPath|La ruta de acceso a la aplicación o la dirección URL.<br /><br /> **Nota:** variables de entorno se admiten en el atributo ShellExecPath.|  
  
     El siguiente ejemplo de código muestra cómo definir un vínculo a una aplicación:  
  
    ```  
    <Links>  
       <Link Title="Calc" Description="Launches Calc" ShellExecPath="%windir%\system32\calc.exe" />  
    </Links>  
    ```  
  
     El siguiente ejemplo de código muestra cómo definir un vínculo a una página Web:  
  
    ```  
    <Links>  
       <Link Title="Browser" Description="Open browser" ShellExecPath="http://www.adventureworks.com/" />  
    </Links>  
    ```  
  
8.  Cambiar los valores de atributo para representar tu tarea o vínculo.  
  
9. En **Explorador de soluciones**, haz clic en **OEMHomePageContent.home**y, a continuación, haz clic en **propiedades**.  En la **propiedades** panel, en **acción de compilación**, selecciona **recursos incrustados**.  
  
10. Guarda el archivo OEMHomePageContent.home.  
  
 Acerca de cómo implementar una consulta de estado rápido, documentos y ejemplos, consulta el [SDK de soluciones de Windows Server](https://go.microsoft.com/fwlink/?LinkID=248648).  
  
#### <a name="change-the-status-of-a-setupadd-ins-task"></a>Cambiar el estado de una tarea SETUP/ADD-INS  
 Se puede alternar las tareas que se enumeran en SETUP y ADD-INS en Estados completado (configurado para Add-ins) y no completado (no configurado para Add-ins).  
  
 Cuando definas la aplicación que está asociada con la nueva tarea, puedes usar el método SetTaskStatus del espacio de nombres Microsoft.WindowsServerSolutions.Administration.ObjectModel.TaskStatusHelper (incluidos, pero no se documenta en el SDK de soluciones de Windows Server) para cambiar el estado de la tarea. Por ejemplo, se puede cambiar la marca de verificación de gris a verde llamando al método SetTaskStatus con el valor de enumeración TaskStatus.Complete (SetTaskStatus (id, TaskStatus.Complete), donde **id** es el identificador de la tarea). Los valores de enumeración que se pueden usar son TaskStatus.Complete, TaskStatus.Incomplete o TaskStatus.Hidden.  
  
##### <a name="replace-tasks"></a>Reemplazo de tareas  
 Puedes reemplazar las tareas predefinidas en las tareas de inicio o las listas de tareas comunes agregando el GUID de la tarea al atributo replaceid de la definición de la tarea. La siguiente tabla enumera las tareas y los identificadores correspondientes que se pueden reemplazar en el panel:  
  
|Nombre de la tarea|Identificador|  
|---------------|----------------|  
|Obtener actualizaciones para otros productos de Microsoft|8412D35A-13EE-4112-AE0B-F7DBC83EA83D|  
|Configurar copia de seguridad del servidor|F68B3F3F-19DE-499D-9ACB-4BB41B8FF420|  
|Configurar acceso desde cualquier lugar|8991302D-676A-4A7C-B244-D1E08AE0EFEA|  
|Notificación de alertas por correo electrónico del programa de instalación|DE6F2B36-F19C-4FAF-998B-9772300E3530|  
|Agregar cuentas de usuario|6D5B5D5F-2EC7-4B1F-9580-4DB084B278B1|  
|Agregar carpetas de servidor|03F1F438-D94E-439B-A9F7-0C817C37D625|  
|Acceso desde cualquier lugar - estado rápido|6093B462-1F04-4212-8804-9BC823070FAD|  
|Copia de seguridad de servidor - estado rápido|156947D8-21DC-45FE-A9A8-2F80AE304191|  
|Carpetas del servidor - estado rápido|C657E8AB-AC1F-4AA1-8E80-5AF6BB27C314|  
|Cuentas de usuario activas - estado rápido|68BCB125-CE8F-467F-B65B-56AD45A614B5|  
|Notificación por correo electrónico alerta - estado rápido|75AB06E7-A679-4D62-A5EC-65362FE4F9DB|  
|Equipos - estado rápido|7966A974-D52D-4F5D-B37F-05C1B73CEEF3|  
  
##### <a name="optional-create-the-resource-file"></a>(Opcional) Crear el archivo de recursos  
 Si quieres localizar el texto de las tareas que agregues a las tareas de inicio, las tareas comunes y vínculos de la Comunidad, debes crear un ensamblado que contenga el archivo .home y un. archivo home.resx que defina las cadenas de texto.  
  
###### <a name="to-create-the-resource-file"></a>Para crear el archivo de recursos  
  
1.  Haz clic en el proyecto que creaste para tus tareas, haz clic en **agregar**y, a continuación, haz clic en **nuevo elemento**.  
  
2.  En la **plantillas** panel, haz clic en **archivo de recursos**, tipo **OEMHomePageContent.home.resx** en la **nombre** cuadro y, a continuación, haz clic en **agregar**.  
  
    > [!NOTE]
    >  El archivo de recursos puede dar cualquier nombre siempre que tiene un. home.resx extensión.  
  
3.  Para cada tarea o vínculo que agregues, debes agregar cadenas y valores al archivo OEMHomePageContent.home.resx que coincidan con los elementos Task y Link definidos en el archivo OEMHomePageContent.home. El siguiente ejemplo de código muestra un ejemplo de un archivo Tasks.xml estructurado para el archivo de recursos:  
  
    ```  
  
    <Tasks version=?2.0? xmlns=?https://schemas.microsoft.com/WindowsServerSolutions/2010/01/Dashboard>  
       <SetupMyServerTasks>  
          <Task name="MyTask"  
             description="MyDescription"  
             id="GUID">  
             <Action  
             name="MyActionname"  
             image="IconForAction"  
             type="TaskType"  
             exelocation="ActionExeLocation" />  
          </Task>  
       </SetupMyServerTasks>  
    </Tasks>  
  
    ```  
  
    > [!NOTE]
    >  Los identificadores de atributos que se usan para la localización no pueden contener espacios.  
  
4.  Agrega los nombres de recursos MyTask, MyTaskDescription, MyActionName y IconForAction al archivo .resx con los valores adecuados.  
  
5.  Guarda el archivo OEMHomePageContent.home.resx y genera la solución.  
  
#####  <a name="BKMK_SignAssembly"></a>Firmar el ensamblado con una firma Authenticode  
 Es necesario que el ensamblado tenga una firma Authenticode para que se use en el sistema operativo. Para obtener más información sobre cómo firmar el ensamblado, consulta [firma y comprobación de código con Authenticode](https://msdn.microsoft.com/library/ms537364\(VS.85\).aspx#SignCode).  
  
##### <a name="install-the-task-files"></a>Instalar los archivos de tareas  
 Después de crear el .home y. home.resx archivos, debes instalarlos en el servidor.  
  
###### <a name="to-install-the-task-files"></a>Para instalar los archivos de tareas  
  
1.  Asegúrate de que la solución se genere sin errores.  
  
2.  Si no ha creado un archivo de recursos incrustados, copia el archivo OEMHomePageContent.home **%ProgramFiles%\Windows Server\Bin\Addins\Home** en el servidor. Si has creado un archivo de recursos incrustados, copia el archivo OEMHomePageContent.dll en **%ProgramFiles%\Windows Server\Bin\Addins\Home** en el servidor.  
  
## <a name="see-also"></a>Consulta también  
 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Prueba la experiencia del cliente](Testing-the-Customer-Experience.md)