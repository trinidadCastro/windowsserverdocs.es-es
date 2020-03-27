---
title: Administrar el mantenimiento del sistema en Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3043f83b-389c-4f37-a1ff-85afe99314fa
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: bbe05c0564e706ef0227e723a52bd10b2f774756
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80311071"
---
# <a name="manage-system-health-in-windows-server-essentials"></a>Administrar el mantenimiento del sistema en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 En este tema se explica cómo ver y responder a todas las alertas de la red mediante el panel.  
  
> [!NOTE]
>  En Windows Server Essentials y Windows Server 2012 R2 con el rol de experiencia con Windows Server Essentials instalado, las alertas de estado de los equipos cliente y servidor de la red ya no se muestran en el visor de alertas, sino que se pueden ver en la pestaña **informes de mantenimiento** de la página **principal** .  
  
 Windows Server Essentials supervisa activamente cada equipo que está conectado al servidor y alerta al administrador de problemas relacionados con el estado del sistema, incluidas las actualizaciones críticas, la falta de protección contra malware, las definiciones de virus no actualizadas en el cliente. equipos y otros problemas importantes que requieren una acción. Estos problemas se muestran como alertas en el visor de alertas, que se puede iniciar desde el panel del servidor o desde el Launchpad del equipo cliente en Windows Server Essentials, o en la pestaña **informes de mantenimiento** de Windows Server Essentials. De forma predeterminada, las alertas se actualizan cada 30 minutos, pero puede evaluar la red en busca de alertas en cualquier momento haciendo clic en **Actualizar** en el Visor de alertas o en la pestaña **Informes de mantenimiento**.  
  
 Los siguientes temas le ayudarán a comprender, ver y responder a las alertas del Visor de alertas y también le proporcionan instrucciones para configurar el servidor de modo que reciba notificaciones de alerta por correo electrónico:  
  
-   [Acerca del complemento de informes de mantenimiento](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_AddIn)  
  
-   [Ver alertas mediante el visor de alertas](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_View)  
  
-   [Organizar alertas en el visor de alertas](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Organize)  
  
-   [Responder a alertas](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Respond)  
  
-   [Configurar notificaciones por correo electrónico para alertas](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Email)  
  
-   [Posibles alertas de equipo](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Potential)  
  
##  <a name="about-the-health-report-add-in"></a><a name="BKMK_AddIn"></a>Acerca del complemento de informes de mantenimiento  
 El complemento de informes de mantenimiento para Windows Server Essentials le da información consolidada sobre la red de Windows Server Essentials y le permite distribuir esta información a otras personas. Esta información se puede ver en la pestaña **Informes** del panel. Con la pestaña **Informes**, puede hacer lo siguiente:  
  
-   [Generar un informe a petición o según la programación](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Generate)  
  
-   [Personalizar el contenido del informe](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Customize)  
  
-   [Enviar el informe por correo electrónico](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_emailreport)  
  
> [!NOTE]
>  **Windows Server Essentials:** Puede descargar el complemento de informes de mantenimiento para Windows Server Essentials desde el centro de [descarga de Microsoft](https://go.microsoft.com/fwlink/p/?LinkId=266342).  
>   
>  **Windows Server Essentials:** De forma predeterminada, el complemento de informes de mantenimiento se integra con Windows Server Essentials o Windows Server 2012 R2 con el rol de experiencia con Windows Server Essentials instalado, y los informes de mantenimiento se muestran en la pestaña **informes de mantenimiento** de la página **principal** del panel.  
  
###  <a name="generate-a-report-on-demand-or-on-schedule"></a><a name="BKMK_Generate"></a>Generar un informe a petición o según la programación  
 Después de instalar el complemento de informes de mantenimiento y reiniciar el panel, se agrega al panel una nueva pestaña, **Informes**. Puede generar un informe de mantenimiento a petición en cualquier momento haciendo clic en la tarea **Generar un informe de mantenimiento** de la pestaña **Informes**.  
  
 Cuando se genera un informe de mantenimiento, se crea un nuevo elemento en el panel de la lista, identificado por la fecha y la hora en que se generó el informe. Para abrir un elemento, puede hacer doble clic en el panel de la lista o seleccionarlo y después hacer clic en **Abrir el informe de mantenimiento** en el panel de tareas. El informe se muestra en una nueva ventana en formato HTML.  
  
 Además de generar un informe manualmente, también puede generar un informe de forma automática cada día o cada hora si lo prefiere. Para ello, en el panel de tareas, haga clic en **personalizar la configuración del informe de mantenimiento**y, a continuación, haga clic en la pestaña **programación y correo electrónico** . La característica **programación** está desactivada de forma predeterminada y puede activarla activando la casilla **generar un informe de mantenimiento a la hora programada** .  
  
###  <a name="customize-the-content-of-the-report"></a><a name="BKMK_Customize"></a>Personalizar el contenido del informe  
 El informe de mantenimiento contiene lo siguiente:  
  
- **Alertas críticas y advertencias** . Esto es coherente con las alertas críticas y advertencias que aparecen en el Visor de alertas del panel. Las alertas de información no se incluyen en el informe de mantenimiento.  
  
- **Errores críticos en los registros de eventos** . Se analizan los registros de aplicaciones y de servicios, y los errores que se registran en las últimas 24 horas se presentan en la sección **Detalles** del informe.  
  
- **Copia de seguridad del servidor**. La información sobre la última copia de seguridad del servidor se muestra en la sección **Detalles** del informe.  
  
- **Los servicios de inicio automático que no se están ejecutando**. En el momento en que se genera el informe, si no se está ejecutando un servicio de inicio automático, la información sobre este servicio se indica en la sección **Detalles** del informe.  
  
- **Actualizaciones**. Puede ver el estado de actualización del servidor y de todos los equipos cliente de la sección **Detalles**.  
  
- **Almacenamiento**. La lista de unidades y su capacidad aparece en la sección **Detalles**.  
  
  En el informe de mantenimiento, vea primero el **Resumen** y luego, si hay elementos que tengan un icono rojo de error o un icono amarillo de advertencia, haga clic en el vínculo **Detalles** de la misma fila para ver los detalles sobre ellos.  
  
  Si no está interesado en algunos de los puntos de datos que se incluyen en el informe de forma predeterminada, puede personalizar el contenido del informe haciendo clic en **personalizar la configuración del informe de mantenimiento** en el panel de tareas y, a continuación, haciendo clic en la pestaña **contenido** . Desactive las casillas de verificación del contenido que no desea ver en el informe. Por ejemplo, si tiene su propio plan de copia de seguridad del servidor y no quiere ver las advertencias sobre las copias de seguridad del servidor, puede excluir las copias de seguridad del servidor del informe desactivando la casilla **copia de seguridad del servidor** .  
  
###  <a name="email-the-report"></a><a name="BKMK_emailreport"></a>Enviar el informe por correo electrónico  
 Tener que iniciar sesión en el panel para leer informes sigue siendo poco práctico para algunos administradores, especialmente si tienen que administrar más de un servidor. Con la característica de correo electrónico activada, después de generarse un informe, se envía un correo electrónico con el contenido del informe a una lista de direcciones de correo electrónico especificadas. El administrador puede ver fácilmente este informe desde cualquier dispositivo o aplicación cliente y asegurarse de que el servidor está funcionando de forma óptima.  
  
 En el cuadro de diálogo **Personalizar la configuración del informe de mantenimiento**, después de habilitar el correo electrónico, cambiar la configuración de SMTP y especificar una lista de los destinatarios de correo electrónico, observará que aparece una nueva tarea en el panel de tareas: **Enviar el informe de mantenimiento por correo electrónico**. Para más información sobre la configuración de SMTP, vea [Configurar notificaciones por correo electrónico para alertas](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Email).  
  
 Puede seleccionar un informe existente y después hacer clic en **Enviar el informe de mantenimiento por correo electrónico**. También puede generar un nuevo informe y hacer que se envíe automáticamente a su bandeja de entrada. Si ha configurado una programación para que el informe se genere automáticamente, lo recibirá automáticamente en su bandeja de entrada después de generarse cada día (o cada hora) según esté programado.  
  
##  <a name="view-alerts-by-using-the-alert-viewer"></a><a name="BKMK_View"></a>Ver alertas mediante el visor de alertas  
 En esta sección se describe cómo usar el panel o Launchpad para abrir el Visor de alertas a fin de consultar el estado de todos los equipos de la red del servidor.  
  
#### <a name="to-open-the-alert-viewer-by-using-the-dashboard"></a>Para abrir el Visor de alertas mediante el panel  
  
1.  Abra el Panel.  
  
2.  En el panel de navegación, haga clic en cualquiera de los iconos de alertas mostrados (alerta crítica, advertencia o informativa). Se abrirá el Visor de alertas.  
  
#### <a name="to-open-the-alert-viewer-from-the-launchpad"></a>Para abrir el Visor de alertas desde Launchpad  
  
1.  Abra Launchpad en un equipo que esté conectado al servidor. Si se le solicita, inicie sesión en Launchpad con su nombre de usuario y contraseña.  
  
2.  Haga clic en cualquiera de los iconos de alerta mostrados (alerta crítica, advertencia e informativa) en la parte inferior de Launchpad para abrir el Visor de alertas y después siga las instrucciones del panel de detalles del Visor de alertas para resolver la alerta.  
  
##  <a name="organize-alerts-in-the-alert-viewer"></a><a name="BKMK_Organize"></a>Organizar alertas en el visor de alertas  
 Puede organizar las alertas en el Visor de alertas y hacer que se muestren en función de su gravedad (alerta crítica, advertencia o informativa) o en función del nombre del equipo.  
  
#### <a name="to-organize-alerts-in-the-alert-viewer"></a>Para organizar las alertas en el Visor de alertas  
  
1.  Abra el Panel.  
  
2.  En el panel de navegación, haga clic en cualquiera de los iconos de alertas mostrados (alerta crítica, advertencia o informativa). Se abrirá el Visor de alertas.  
  
3.  Expanda la lista desplegable **Organizar** y después elija una de las siguientes acciones:  
  
    1.  Seleccione **Filtrar por equipo** y haga clic en el nombre del equipo del que quiere ver las alertas. Esto muestra solo las alertas del equipo seleccionado en el Visor de alertas.  
  
    2.  Seleccione **Filtrar por tipo de alerta** y haga clic en el tipo de alerta (crítica, advertencia o informativa) que quiere ver. Esto muestra solo el tipo de alerta seleccionado en el Visor de alertas.  
  
##  <a name="respond-to-alerts"></a><a name="BKMK_Respond"></a>Responder a alertas  
 Cuando se detecta una alerta, puede optar por tomar una de las medidas siguientes:  
  
-   [Resolver una alerta](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Resolve)  
  
-   [Omitir una alerta](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Habilitar una alerta](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Eliminar una alerta](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_4)  
  
###  <a name="resolve-an-alert"></a><a name="BKMK_Resolve"></a>Resolver una alerta  
 Siga las instrucciones de resolución del Visor de alertas para resolver la alerta. Una vez resuelta una alerta, aún se muestra en el Visor de alertas hasta que se actualice.  
  
###  <a name="ignore-an-alert"></a><a name="BKMK_3"></a>Omitir una alerta  
 Puede omitir una alerta si prefiere responder a ella más adelante. Cuando se omite una alerta, sigue apareciendo en el Visor de alertas, pero está deshabilitada y atenuada. Una alerta omitida no se incluye en la evaluación del mantenimiento global del equipo. Para solucionar una alerta omitida, primero hay que habilitarla.  
  
##### <a name="to-ignore-an-alert"></a>Omitir una alerta  
  
1. Abra Launchpad en un equipo que esté conectado al servidor de Windows Server Essentials.  
  
2. En Launchpad, haga clic en cualquiera de los iconos de alerta mostrados (alerta crítica, de advertencia e informativa). Se abrirá el Visor de alertas.  
  
3. En el Visor de alertas, seleccione la alerta que quiere omitir y después, en la sección **Tareas**, haga clic en **Omitir la alerta**.  
  
   Para responder a una alerta deshabilitada, deberá habilitarla.  
  
###  <a name="enable-an-alert"></a><a name="BKMK_5"></a>Habilitar una alerta  
 Puede habilitar una alerta que había decidido omitir anteriormente. Después de habilitar la alerta, puede resolverla o eliminarla según sea necesario. La alerta aparece como deshabilitada cuando se marca para omitirse. Cuando se habilita una alerta que se ha deshabilitado previamente, se activa y se vuelve a incluir en la evaluación del mantenimiento general de los equipos.  
  
##### <a name="to-enable-an-alert"></a>Para habilitar una alerta  
  
1.  Abra Launchpad en el equipo que esté conectado al servidor.  
  
2.  En Launchpad, haga clic en cualquiera de los iconos de alerta mostrados (alerta crítica, de advertencia e informativa) para abrir el Visor de alertas.  
  
3.  En el Visor de alertas, haga clic con el botón secundario en la alerta que quiere habilitar y después haga clic en **Habilitar la alerta**.  
  
###  <a name="delete-an-alert"></a><a name="BKMK_4"></a>Eliminar una alerta  
 Puede eliminar una alerta si no quiere resolverla ni omitirla. Puede usar el Visor de alertas en Launchpad para eliminar las alertas generadas para el equipo. Si elimina una alerta y el servidor vuelve a detectar el problema en el siguiente ciclo de evaluación del mantenimiento de la red, se genera una alerta nueva.  
  
##### <a name="to-delete-an-alert"></a>Para eliminar una alerta  
  
1.  Abra Launchpad en el equipo que esté conectado al servidor.  
  
2.  En Launchpad, haga clic en cualquiera de los iconos de alerta mostrados (alerta crítica, de advertencia e informativa) para abrir el Visor de alertas.  
  
3.  En el Visor de alertas, haga clic con el botón secundario en la alerta que quiere eliminar y después haga clic en **Eliminar la alerta**.  
  
##  <a name="set-up-email-notifications-for-alerts"></a><a name="BKMK_Email"></a>Configurar notificaciones por correo electrónico para alertas  
 Puede configurar el servidor para que le notifique por correo electrónico cuando se produzcan alertas. Las notificaciones por correo electrónico para estas alertas contienen información sobre los problemas de red y los pasos de resolución, que es idéntica a la información que se muestra en el Visor de alertas. Algunas de las evaluaciones del mantenimiento de la red se hacen mediante programación.  
  
 Al configurar el servidor para que envíe notificaciones de alertas por correo electrónico, se envía una notificación por correo electrónico de las alertas que se encuentren durante la evaluación del mantenimiento de la red. Sin embargo, no todas las alertas que se muestran en el Visor de alertas se incluyen en el correo electrónico.  
  
 Cada 30 minutos, se ejecuta en el servidor la tarea de evaluación de alertas por correo electrónico, que evalúa la red en busca de alertas. Se envía una notificación por correo electrónico si se produce una alerta que esté configurada para ese tipo de notificación. Si la alerta sigue activa en el siguiente ciclo de evaluación, no se envía una segunda notificación para no saturar su bandeja de entrada. Sin embargo, si se detecta una alerta nueva en un ciclo posterior de evaluación de alertas, se envía una notificación por correo electrónico que incluye las alertas anteriores y las nuevas.  
  
###  <a name="alerts-that-result-in-email-notifications"></a><a name="BKMK_list"></a>Alertas que dan lugar a notificaciones por correo electrónico  
 Las siguientes alertas del Visor de alertas dan lugar a notificaciones por correo electrónico si se configura el servidor para que envíe notificaciones por correo electrónico en caso de alertas:  
  
-   Hay errores en una copia de seguridad del equipo cliente.  
  
-   Se ha reiniciado el servidor.  
  
-   No se están ejecutando uno o varios servicios.  
  
-   No se está ejecutando el servicio de almacenamiento de Windows Server.  
  
-   El período de evaluación ha finalizado.  
  
-   Activar ahora.  
  
-   El servidor de origen se está apagando.  
  
-   Los resultados del examen BPA contienen errores.  
  
-   Los resultados del examen BPA contienen advertencias.  
  
-   No hay disponible ningún certificado para el Acceso desde cualquier lugar.  
  
-   Ha expirado el certificado para el Acceso desde cualquier lugar.  
  
-   El enrutador no está configurado correctamente.  
  
-   El servidor web no está configurado correctamente.  
  
-   Los Servicios de Escritorio remoto no está configurados correctamente.  
  
-   El firewall no está configurado correctamente.  
  
-   El nombre de dominio de Internet ha caducado.  
  
-   No se puede actualizar el nombre de dominio de Internet.  
  
-   Error de licencia: Comprobación de la confianza de bosque.  
  
-   Error de licencia: Comprobación de controlador de dominio.  
  
-   Error de licencia: Comprobación del rol FSMO.  
  
-   Error de licencia: Directivas FSMO de cumplimiento.  
  
-   Error de licencia: Directivas de carga de cumplimiento.  
  
-   Error de licencia: Servicios de dominio de Active Directory.  
  
-   La suscripción a Office 365 ha caducado.  
  
-   La autenticación de Office 365 no se realizó correctamente.  
  
-   La directiva de contraseñas no es correcta.  
  
-   El servicio de sincronización de contraseñas no puede sincronizar contraseñas de usuario con Office 365.  
  
-   Cambie la contraseña de Windows.  
  
-   La contraseña de Office 365 no es la misma que la contraseña de Windows.  
  
-   No se puede conectar al servidor de Exchange.  
  
-   Microsoft Exchange Server tiene un problema.  
  
-   No están conectadas una o varias unidades de discos duros de copia de seguridad del servidor.  
  
-   La unidad de disco duro de copia de seguridad no tiene suficiente espacio libre para la copia de seguridad del servidor.  
  
-   La copia de seguridad del servidor no se ha hecho correctamente porque no se pudo tomar una instantánea de la unidad.  
  
-   Una copia de seguridad programada no se completó correctamente.  
  
-   Faltan una o varias carpetas de servidor predefinidas.  
  
-   Hay poco espacio libre en uno o varios discos duros del servidor.  
  
-   VSS Writer no se está ejecutando para el servicio de almacenamiento.  
  
-   Poca capacidad de almacenamiento en unidades de disco duro.  
  
-   Una o varias unidades no funcionan y están desconectadas.  
  
###  <a name="configuring-smtp-on-your-server-to-send-alert-notifications-by-email-in-windows-server-essentials"></a><a name="BKMK_SMTP"></a>Configuración de SMTP en el servidor para enviar notificaciones de alerta por correo electrónico en Windows Server Essentials  
 En esta sección se explica cómo configurar el servidor para enviar notificaciones por correo electrónico en caso de alertas.  
  
> [!NOTE]
>  Puede descargar el complemento de informes de mantenimiento para Windows Server Essentials desde el centro de [descarga de Microsoft](https://go.microsoft.com/fwlink/p/?LinkId=266342).  
  
##### <a name="to-set-up-email-notification-for-alerts"></a>Para configurar la notificación de alertas por correo electrónico  
  
1.  En el **panel**, abra el **Visor de alertas**.  
  
2.  En el **Visor de alertas**, haga clic en **Configurar notificaciones de alertas por correo electrónico**.  
  
3.  En la ventana **Configurar notificación de alertas por correo electrónico**, haga clic en **Habilitar**.  
  
4.  En la ventana **Configuración SMTP**, haga lo siguiente:  
  
    1.  En **Desde la dirección de correo electrónico**, escriba la dirección de correo electrónico desde la que quiere que se envíen alertas por correo electrónico. Esta dirección de correo electrónico se mostrará como la dirección del remitente en la notificación de alerta.  
  
    2.  En **Nombre del servidor SMTP**, en el cuadro de texto **Desde la dirección de correo electrónico**, escriba el nombre del servidor SMTP que especificó en el paso 4a. (Consulte la tabla 1 para ver una lista de algunos nombres de servidor SMTP).  
  
    3.  En **Puerto SMTP**, escriba el número de puerto que usa el servidor SMTP para enviar y recibir correo electrónico. (Consulte la tabla 1 para ver los números de puerto que usan algunos de los servidores SMTP).  
  
    4.  Seleccione **El servidor requiere una conexión segura (SSL)** si el servidor SMTP usa SSL (consulte la tabla 1).  
  
    5.  Seleccione **Este servidor requiere autenticación** si el servidor SMTP requiere la información de nombre de usuario y contraseña (consulte la tabla 1). Si selecciona esta casilla de verificación, escriba el nombre de usuario y la contraseña de la dirección de correo electrónico que escribió en el campo **Desde la dirección de correo electrónico** en el paso 4a y después haga clic en **Aceptar**.  
  
    > [!NOTE]
    >  Puede obtener la información del nombre, el número de puerto y el uso SSL del servidor SMTP de su proveedor de acceso a Internet.  
  
     **Tabla 1** Ejemplos de nombres, requisitos de autenticación y de cifrado SSL y de números de puerto del servidor SMTP  
  
    |SMTP Server|Requiere SSL|Se requiere autenticación|Número de puerto|Nombre de cuenta/nombre de inicio de sesión|  
    |-----------------|------------------|-----------------------------|-----------------|------------------------------|  
    |smtp.gmail.com|Sí|Sí|587|Proporcione la dirección de correo electrónico completa con el nombre de dominio y la contraseña para la autenticación.|  
    |smtp.live.com|Sí|Sí|587|Proporcione la dirección de correo electrónico completa con el nombre de dominio y la contraseña para la autenticación.|  
    |smtp.comcast.net|Sí|No|587|Proporcione la dirección de correo electrónico completa con el nombre de dominio y la contraseña para la autenticación.|  
    |smtp.mail.yahoo.com|No|Sí|25|Proporcione solo la dirección de correo electrónico sin nombre de dominio para el nombre de usuario.|  
  
5.  En la opción **Destinatarios de correo electrónico** de **Configurar notificación de alertas**, escriba las direcciones de correo electrónico de las personas que quiere que reciban notificaciones de alertas por correo electrónico. Asegúrese de separar cada dirección de correo electrónico con un punto y coma (;).  
  
6.  Para comprobar que haya configurado los valores del servidor SMTP correctamente para enviar notificaciones por correo electrónico en caso de alertas, haga clic en **Aplicar y enviar correo electrónico**.  
  
    > [!NOTE]
    >  Al hacer clic en **Aplicar y enviar correo electrónico**, normalmente recibirá una notificación de prueba por correo electrónico sin ninguna alerta de estado. Sin embargo, si durante este proceso de prueba se identifica una alerta de estado que está configurada para enviar una notificación por correo electrónico, dicha alerta se incluye en el correo electrónico de prueba.  
  
### <a name="configuring-smtp-on-your-server-to-send-health-reports-in-windows-server-essentials"></a>Configuración de SMTP en el servidor para enviar informes de mantenimiento en Windows Server Essentials  
 En esta sección se explica cómo configurar los valores de SMTP para el servidor de modo que pueda recibir informes de mantenimiento por correo electrónico.  
  
> [!NOTE]
>  De forma predeterminada, el complemento de informes de mantenimiento se integra con Windows Server Essentials o Windows Server 2012 R2 con el rol de experiencia con Windows Server Essentials instalado, y los informes de mantenimiento se muestran en la pestaña **informes de mantenimiento** de la página **principal** del panel.  
  
##### <a name="to-set-up-email-notification-for-health-reports"></a>Para configurar la notificación de informes de mantenimiento por correo electrónico  
  
1.  En el **panel**, haga clic en la pestaña **Informes**.  
  
2.  En el panel de tareas **Tareas de informe de mantenimiento**, haga clic en **Personalizar la configuración del informe de mantenimiento**.  
  
3.  En la ventana **Personalizar la configuración del informe de mantenimiento**, haga clic en la pestaña **Programación y correo electrónico**.  
  
4.  En la sección **Correo electrónico** de la pestaña **Programación y correo electrónico**, haga lo siguiente:  
  
    1.  Haga clic en **Habilitar** y escriba la dirección de correo electrónico desde la que quiere que se envíen los informes de mantenimiento. Esta dirección de correo electrónico se mostrará como la dirección del remitente en los informes de mantenimiento que se envían por correo electrónico.  
  
        1.  En **Nombre del servidor SMTP**, escriba el nombre del servidor SMTP. (Consulte la tabla 1 para ver una lista de algunos nombres de servidor SMTP).  
  
        2.  En **Puerto SMTP**, escriba el número de puerto que usa el servidor SMTP para enviar y recibir correo electrónico. (Consulte la tabla 1 para ver los números de puerto que usan algunos de los servidores SMTP).  
  
        3.  Seleccione **El servidor requiere una conexión segura (SSL)** si el servidor SMTP usa SSL (consulte la tabla 1).  
  
        4.  Seleccione **Este servidor requiere autenticación** si el servidor SMTP requiere la información de nombre de usuario y contraseña (consulte la tabla 1). Si selecciona esta casilla de verificación, escriba el nombre de usuario y la contraseña de la dirección de correo electrónico que escribió en el campo **Desde la dirección de correo electrónico** en el paso 4a y después haga clic en **Aceptar**.  
  
            > [!NOTE]
            >  Puede obtener la información del nombre, el número de puerto y el uso SSL del servidor SMTP de su proveedor de acceso a Internet.  
  
            > [!NOTE]
            >  Microsoft recomienda encarecidamente usar SSL porque el informe puede contener un estado del servidor que podrían usar terceros de forma malintencionada para detectar vulnerabilidades (por ejemplo, falta de Windows Update). Al habilitar SSL se cifran los datos en tránsito y se reduce el riesgo de exponer la vulnerabilidad del servidor.  
  
5.  Después de habilitar el correo electrónico, se muestra el vínculo **Cambiar configuración de SMTP**. También se muestra una nueva tarea, **Enviar el informe de mantenimiento por correo electrónico**, en **Tareas de informe de mantenimiento**.  
  
     **Tabla 1** Ejemplos de nombres, requisitos de autenticación y de cifrado SSL y de números de puerto del servidor SMTP  
  
    |SMTP Server|Requiere SSL|Se requiere autenticación|Número de puerto|Nombre de cuenta/nombre de inicio de sesión|  
    |-----------------|------------------|-----------------------------|-----------------|------------------------------|  
    |smtp.gmail.com|Sí|Sí|587|Proporcione la dirección de correo electrónico completa con el nombre de dominio y la contraseña para la autenticación.|  
    |smtp.live.com|Sí|Sí|587|Proporcione la dirección de correo electrónico completa con el nombre de dominio y la contraseña para la autenticación.|  
    |smtp.comcast.net|Sí|No|587|Proporcione la dirección de correo electrónico completa con el nombre de dominio y la contraseña para la autenticación.|  
    |smtp.mail.yahoo.com|No|Sí|25|Proporcione solo la dirección de correo electrónico sin nombre de dominio para el nombre de usuario.|  
  
6.  En la opción **Enviar automáticamente el informe de mantenimiento a los siguientes destinatarios de correo electrónico:** de **Personalizar la configuración del informe de mantenimiento**, escriba las direcciones de correo electrónico de las personas que quiere que reciban informes de mantenimiento por correo electrónico. Asegúrese de separar cada dirección de correo electrónico con un punto y coma (;).  
  
7.  Para comprobar que ha configurado el servidor SMTP correctamente para enviar informes de mantenimiento por correo electrónico, en la pestaña Informe de mantenimiento del panel, seleccione un informe y haga clic en **Enviar el informe de mantenimiento por correo electrónico** desde el panel de tareas.  
  
##  <a name="potential-computer-alerts"></a><a name="BKMK_Potential"></a>Posibles alertas de equipo  
 En esta sección se describe cómo se deben comprender y administrar las alertas que son específicas de su equipo conectado al servidor y que aparecen en el Launchpad de su equipo.  
  
 En la tabla siguiente se enumeran algunas de las alertas de equipo que se pueden generar y se muestran en el Visor de alertas si son aplicables a su equipo.  
  
|Título de la alerta|Impacto y resolución de la alerta|  
|-----------------|---------------------------------|  
|El estado actual del firewall de red ofrece una protección reducida a este equipo.|Es posible que personas o software no autorizados puedan acceder a este equipo si el Firewall de Windows no está activado.|  
|La protección antivirus está desactivada, no está instalada o no está actualizada.|Los datos del equipo están en peligro si la configuración de seguridad **Protección antivirus** está desactivada o no se ha actualizado. [To protect your computer](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Protect), siga los pasos indicados.|  
|La protección contra spyware y software no deseado está desactivada, no se ha instalado o no está actualizada.|Los datos del equipo están en peligro si la **Protección contra spyware y software no deseado** está desactivada o no actualizada. [To protect your computer](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Protect), siga los pasos indicados.|  
|Windows Update está desactivado.|No podrá beneficiarse de las características nuevas y corregidas de las actualizaciones si Windows Update está desactivado. Para activar Windows Update, haga clic en **Abrir Windows Update** en el Visor de alertas.<br /><br /> Si no se muestra la tarea **Abrir Windows Update**, es que no ha iniciado sesión en el equipo donde se ha generado la alerta. Debe haber iniciado sesión en el equipo en el que se ha generado la alerta para ejecutar esta tarea en el Visor de alertas.|  
|Las actualizaciones importantes deben instalarse.|No podrá beneficiarse de las características nuevas y corregidas de las actualizaciones si Windows Update está desactivado. Para activar Windows Update, haga clic en **Abrir Windows Update** en el Visor de alertas.<br /><br /> Si no se muestra la tarea **Abrir Windows Update**, es que no ha iniciado sesión en el equipo donde se ha generado la alerta. Debe haber iniciado sesión en el equipo en el que se ha generado la alerta para ejecutar esta tarea en el Visor de alertas.|  
|Reinicie el equipo para aplicar las actualizaciones.|No podrá beneficiarse de las características nuevas y corregidas de las actualizaciones hasta que se apliquen. Guarde todos los datos y reinicie el equipo para aplicar las actualizaciones.|  
|No hay suficiente espacio disponible en las unidades de disco duro.|Si no se libera espacio, es posible que no pueda guardar más información. Para liberar espacio en el equipo, considere las siguientes opciones:<br /><br /> -Agregar una nueva unidad de disco duro.<br /><br /> -Ejecute el **liberador de espacio en disco** para quitar los archivos antiguos y temporales.<br /><br /> -Mueva los archivos a una carpeta compartida de otro equipo.<br /><br /> -Archivos de almacenamiento en medios extraíbles, como un CD, un DVD o una unidad de disco duro externa.|  
|El agente **Historial de archivos** no está configurado correctamente en este equipo.|No se pueden crear copias de seguridad del Historial de archivos.|  
|No se están ejecutando uno o varios servicios.||  
|Cambie la contraseña de Windows.||  
|La contraseña de Microsoft Office 365 no es la misma que la contraseña de Windows.||  
  
###  <a name="to-protect-your-computer"></a><a name="BKMK_Protect"></a>Para proteger el equipo  
  
1.  Abra el Centro de seguridad.  
  
2.  Determine el estado de la protección antivirus instalada.  
  
3.  Lleve a cabo una de las siguientes tareas en función del estado de protección:  
  
    -   Si no está habilitada, habilítela.  
  
    -   Si no está actualizada, complete el proceso para actualizar las firmas.  
  
    -   Si la protección antivirus no está instalada, considere la posibilidad de instalarla.  
  
## <a name="see-also"></a>Vea también  
  
-   [Usar Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)  
  
-   [Administrar Windows Server Essentials](Manage-Windows-Server-Essentials.md)
