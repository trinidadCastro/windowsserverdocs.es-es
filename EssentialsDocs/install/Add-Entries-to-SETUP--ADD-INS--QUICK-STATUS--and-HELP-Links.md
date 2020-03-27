---
title: Agregar entradas a CONFIGURAR, COMPLEMENTOS, ESTADO RÁPIDO Y VÍNCULOS DE AYUDA
description: Describe cómo usar Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c0a8f10d-fd85-4c8d-b9bb-176cb1db1f46
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 074e4e638a1fe96bedf2c8340ec71848a8fa4ac4
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80310278"
---
# <a name="add-entries-to-setup-add-ins-quick-status-and-help-links"></a>Agregar entradas a CONFIGURAR, COMPLEMENTOS, ESTADO RÁPIDO Y VÍNCULOS DE AYUDA

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Puede agregar tareas a las listas de tareas **CONFIGURAR**, **COMPLEMENTOS**o **ESTADO RÁPIDO**, y puede agregar vínculos a la sección Vínculos de la comunidad en la página principal del Panel. Para agregar las tareas y enlaces a las listas y secciones, copie un archivo XML llamado OEMHomePageContent.home o un archivo de recursos incrustado llamado OEMHomePageContent.dll en %ProgramFiles%\Windows Server\Bin\Addins\Home. El archivo de recursos incrustado se puede utilizar para localizar el texto en las tareas y los vínculos que se agregan. El archivo .home contiene las definiciones XML de las tareas y vínculos.  
  
## <a name="adding-tasks-to-the-setup-add-ins-quick-status-task-lists-and-adding-links-to-help-task"></a>Agregar tareas a las listas de tareas CONFIGURAR, COMPLEMENTOS o ESTADO RÁPIDO y agregar vínculos a la tarea AYUDA  
 Puede agregar tareas a las listas de tareas **CONFIGURAR**, **COMPLEMENTOS**o **ESTADO RÁPIDO**, y vínculos a la **AYUDA**; para ello, defina las tareas y los enlaces en el archivo XML y, de manera opcional, cree el archivo de recursos incrustado e instálelo en el servidor. Si el archivo XML se instala en el servidor sin un archivo de recursos, debe tener como nombre OEMHomePageContent.home. Si se está usando un ensamblado para instalar el archivo XML y el archivo de recursos, se debe denominar OEMHomePageContent.dll y firmar con Authenticode.  
  
### <a name="define-the-tasks-and-links"></a>Definición de tareas y enlaces  
 Puede utilizar un editor de textos como el Bloc de notas para crear el archivo .home; si también va a crear un archivo de recursos incrustado, puede utilizar Visual Studio 2010 o superior para definir los archivos. En el procedimiento siguiente se muestra cómo utilizar Visual Studio 2010 o superior para crear los archivos.  
  
##### <a name="to-define-the-tasks-and-links"></a>Para definir las tareas y enlaces  
  
1. Abra Visual Studio 2010 o superior como administrador; para ello, haga clic con el botón secundario en el menú Inicio y seleccione **Ejecutar como administrador**.  
  
2. Haga clic en **Archivo**, **Nuevo** y a continuación haga clic en **Proyecto**.  
  
3. En el panel **Plantillas**, haga clic en **Biblioteca de clase**, escriba **OEMHomePageContent** en la casilla **Nombre** y a continuación haga clic en **Aceptar**.  
  
4. Elimine el archivo Class1.cs.  
  
5. Haga clic con el botón secundario en el nuevo proyecto, seleccione **Agregar** y a continuación haga clic en **Nuevo elemento**.  
  
6. En el panel **Plantillas**, haga clic en **Archivo XML**, escriba **OEMHomePageContent.home** en la casilla **Nombre** y después haga clic en **Agregar**.  
  
   > [!NOTE]
   >  Si el archivo XML se instala sin un archivo de recursos, debe tener como nombre OEMHomePageContent.home. Si se incluye en un conjunto, puede tener cualquier nombre siempre y cuando tenga la extensión .home.  
  
7. Añada el siguiente código XML al archivo OEMHomePageContent.home:  
  
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
   |Nombre (tarea)|El nombre de la tarea que se muestra en la lista. Si crea un archivo de recursos incrustado, el valor de este atributo será el recurso de cadena.|  
   |descripción (tarea)|La descripción de la tarea. Si crea un archivo de recursos incrustado, el valor de este atributo será el recurso de cadena.|  
   |Id. (tarea)|El identificador de la tarea. El identificador debe ser un GUID. Puede crear un nuevo GUID para una tarea **exe** , pero para una tarea **global** , use el GUID que creó al definir la tarea para el panel de tareas de la subpestaña. Para obtener más información acerca de cómo crear un GUID, consulte [crear GUID (Guidgen. exe)](https://go.microsoft.com/fwlink/?LinkId=116098).|  
   |image|Se omitirá este campo.|  
   |Nombre (acción)|Muestra el nombre de la tarea.|  
   |Tipo (acción)|Describe el tipo de tarea. La tarea puede ser: una tarea **global**, **exe** o de URL. Una tarea **global** es la misma tarea global que creó al definir las tareas del panel de tareas en la subpestaña. Para obtener más información sobre la creación de una tarea global que se puede usar en el panel de tareas de la subpestaña y en las listas de tareas de Introducción o tareas comunes de la Página principal, vea œCreating las clases de soporte técnico? en cómo: ¿desea crear una subpestaña? del [SDK de soluciones de Windows Server](https://go.microsoft.com/fwlink/?LinkID=248648). Las tareas **exe** se pueden utilizar para ejecutar aplicaciones desde las listas Tareas de introducción o Tareas comunes.|  
   |exelocation|La ruta a la aplicación asociada con la tarea. Este atributo solo se utiliza con las tareas **exe**.|  
   |replaceid|El identificador de la tarea que es sustituida por esta tarea.|  
   |conjunto|AssemblyName del conjunto que proporciona la clase para implementar la consulta de estado rápido. El ensamblado debe encontrarse en archivos de programa \ Windows Server\Bin\\.|  
   |clase|Nombre de la clase que implementa la consulta de estado rápido. La clase debe implementar la interfaz **ITaskStatusQuery**.|  
   |Título (vínculo)|El texto del enlace. Si crea un archivo de recursos incrustado, el valor de este atributo será el recurso de cadena.|  
   |Descripción (vínculo)|La descripción del destino del enlace. Si crea un archivo de recursos incrustado, el valor de este atributo será el recurso de cadena.|  
   |ShellExecPath|La ruta a la aplicación o la dirección URL.<br /><br /> **Nota:** Las variables de entorno se admiten en el atributo ShellExecPath.|  
  
    En el siguiente código de ejemplo se muestra cómo definir un enlace para una aplicación:  
  
   ```  
   <Links>  
      <Link Title="Calc" Description="Launches Calc" ShellExecPath="%windir%\system32\calc.exe" />  
   </Links>  
   ```  
  
    En el siguiente código de ejemplo se muestra cómo definir un enlace para una página web:  
  
   ```  
   <Links>  
      <Link Title="Browser" Description="Open browser" ShellExecPath="http://www.adventureworks.com/" />  
   </Links>  
   ```  
  
8. Cambie los valores del atributo para representar la tarea o el enlace.  
  
9. En el **Explorador de soluciones**, haga clic con el botón secundario en **OEMHomePageContent.home** y, a continuación, haga clic en **Propiedades**.  En el panel de **Propiedades**, en **Acción de generación**, seleccione **Recurso incrustado**.  
  
10. Guarde el archivo OEMHomePageContent.home.  
  
    Para ver cómo implementar una consulta de estado rápido, consulte los documentos y ejemplos del [SDK de Soluciones de Windows Server](https://go.microsoft.com/fwlink/?LinkID=248648).  
  
#### <a name="change-the-status-of-a-setupadd-ins-task"></a>Cambiar el estado de la tarea CONFIGURAR/COMPLEMENTOS  
 Las tareas que se indican en CONFIGURAR y COMPLEMENTOS pueden alternar entre el estado completadas (configuradas para Complementos) y no completadas (no configuradas para Complementos).  
  
 Cuando defina la aplicación asociada a la nueva tarea, puede usar el método SetTaskStatus del espacio de nombres Microsoft.WindowsServerSolutions.Administration.ObjectModel.TaskStatusHelper (incluido, aunque no documentado, en el SDK de Soluciones de Windows Server) para cambiar el estado de la tarea. Así, por ejemplo, puede cambiar la marca de verificación de gris a verde si invoca el método SetTaskStatus con el valor de enumeración TaskStatus.Complete (SetTaskStatus(id, TaskStatus.Complete), donde **id** corresponde al identificador de la tarea). Los valores de enumeración que se pueden usar son TaskStatus.Complete, TaskStatus.Incomplete o TaskStatus.Hidden.  
  
##### <a name="replace-tasks"></a>Sustituir tareas  
 Para sustituir las tareas predefinidas en la lista de Tareas de introducción o la lista de Tareas comunes, agregue el GUID de la tarea al atributo replaceid de la definición de la tarea. En la tabla siguiente encontrará una lista de las tareas y sus identificadores correspondientes que se pueden sustituir en el Panel:  
  
|Nombre de tarea|Identificador|  
|---------------|----------------|  
|Obtener actualizaciones para otros productos de Microsoft|8412D35A-13EE-4112-AE0B-F7DBC83EA83D|  
|Configurar copia de seguridad del servidor|F68B3F3F-19DE-499D-9ACB-4BB41B8FF420|  
|Configurar Acceso desde cualquier lugar|8991302D-676A-4A7C-B244-D1E08AE0EFEA|  
|Configurar notificación de alerta por correo electrónico|DE6F2B36-F19C-4FAF-998B-9772300E3530|  
|Agregar una cuenta de usuario|6D5B5D5F-2EC7-4B1F-9580-4DB084B278B1|  
|Agregar carpetas de servidor|03F1F438-D94E-439B-A9F7-0C817C37D625|  
|Acceso desde cualquier lugar - Estado rápido|6093B462-1F04-4212-8804-9BC823070FAD|  
|Copia de seguridad del servidor - Estado rápido|156947D8-21DC-45FE-A9A8-2F80AE304191|  
|Carpetas del servidor - Estado rápido|C657E8AB-AC1F-4AA1-8E80-5AF6BB27C314|  
|Cuenta de usuario activas - Estado rápido|68BCB125-CE8F-467F-B65B-56AD45A614B5|  
|Notificación de alerta por correo electrónico - Estado rápido|75AB06E7-A679-4D62-A5EC-65362FE4F9DB|  
|Equipos - Estado rápido|7966A974-D52D-4F5D-B37F-05C1B73CEEF3|  
  
##### <a name="optional-create-the-resource-file"></a>(Opcional) Crear el archivo de recursos  
 Para localizar el texto de las tareas que agregue a las Tareas de introducción, Tareas comunes y los Vínculos de la comunidad, cree un ensamblado que contenga el archivo .home y un archivo .home.resx que defina las cadenas de texto.  
  
###### <a name="to-create-the-resource-file"></a>Para crear el archivo de recursos  
  
1.  Haga clic con el botón secundario en el proyecto que haya creado para las tareas, haga clic en **Agregar** y a continuación en **Nuevo elemento**.  
  
2.  En el panel **Plantillas**, haga clic en **Archivo de recursos**, escriba **OEMHomePageContent.home.resx** en la casilla **Nombre** y a continuación haga clic en **Agregar**.  
  
    > [!NOTE]
    >  El archivo de recursos puede tener cualquier nombre, siempre que posea la extensión .home.resx.  
  
3.  Por cada tarea que agregue, deberá añadir cadenas y valores al archivo OEMHomePageContent.home.resx que coincidan con los elementos de Tareas y de Enlaces definidos en el archivo OEMHomePageContent.home. En el código de muestra que aparece a continuación se muestra un ejemplo de archivo Tasks.xml estructurado para el archivo de recursos:  
  
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
    >  Los identificadores de los atributos que se utilicen para la localización no pueden contener espacios.  
  
4.  Agregue los nombres de recursos MyTask, MyTaskDescription, MyActionName, y IconForAction en el archivo .resx con los valores adecuados.  
  
5.  Guarde el archivo OEMHomePageContent.home.resx y genere la solución.  
  
#####  <a name="sign-the-assembly-with-an-authenticode-signature"></a><a name="BKMK_SignAssembly"></a>Firmar el ensamblado con una firma Authenticode  
 Para poder utilizar el ensamblado en el sistema operativo es necesario firmarlo mediante Authenticode. Para obtener más información acerca de cómo firmar el ensamblado, consulte [Signing and Checking Code with Authenticode (Firma y comprobación de código con Authenticode)](https://msdn.microsoft.com/library/ms537364\(VS.85\).aspx#SignCode).  
  
##### <a name="install-the-task-files"></a>Instale los archivos de tareas  
 Después de crear los archivos .home y .home.resx, deberá instalarlos en el servidor.  
  
###### <a name="to-install-the-task-files"></a>Para instalar los archivos de tareas  
  
1.  Asegúrese de que la solución no genere ningún error.  
  
2.  Si no ha creado ningún archivo de recursos incrustado, copie el archivo OEMHomePageContent.home en la carpeta **%ProgramFiles%\Windows Server\Bin\Addins\Home** del servidor. Si ha creado un archivo de recursos incrustado, copie el archivo OEMHomePageContent.dll en la carpeta **%ProgramFiles%\Windows Server\Bin\Addins\Home** del servidor.  
  
## <a name="see-also"></a>Consulta también  
 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Probar la experiencia del cliente](Testing-the-Customer-Experience.md)