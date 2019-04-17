---
title: Administrar el estado del sistema en Windows Server Essentials
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3043f83b-389c-4f37-a1ff-85afe99314fa
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 91635a58c64fbf74d3b0139be7c9c36365487319
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="manage-system-health-in-windows-server-essentials"></a>Administrar el estado del sistema en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 En este tema se describe cómo ver y responder a todas las alertas de la red mediante el panel.  
  
> [!NOTE]
>  En Windows Server Essentials y Windows Server 2012 R2 con el rol de Windows Server Essentials Experience instalado, ya no se muestran en el Visor de alertas las alertas de estado para los equipos servidor y cliente en la red, pero pueden verse en la **informes de mantenimiento** ficha de la **Home** página.  
  
 Windows Server Essentials supervisa activamente cada equipo que está conectado al servidor y te avisa al administrador problemas relacionados con el estado del sistema s, incluidas las actualizaciones críticas, falta de protección contra malware, definiciones de virus obsoletas en los equipos cliente y otros problemas importantes que requieren una acción. Estos problemas se muestran como alertas en el Visor de alertas, que se puede iniciar desde el servidor s panel o el equipo cliente s Launchpad de Windows Server Essentials, o en la **informes de mantenimiento** ficha en Windows Server Essentials. De forma predeterminada, las alertas se actualizan cada 30 minutos, pero puedes evaluar la red para las alertas en cualquier momento pulsando **actualizar** en el Visor de alertas o en la **informes de mantenimiento** pestaña.  
  
 Los siguientes temas te ayudará a comprender, ver y responder a las alertas en el Visor de alertas y también proporciona instrucciones para configurar el servidor para recibir notificaciones de alerta en un correo electrónico:  
  
-   [Sobre el estado informar de complemento](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_AddIn)  
  
-   [Ver avisos mediante el Visor de alertas](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_View)  
  
-   [Organizar las alertas en el Visor de alertas](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Organize)  
  
-   [Responder a las alertas](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Respond)  
  
-   [Configurar notificaciones de correo electrónico de alertas](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Email)  
  
-   [Posibles alertas de equipo](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Potential)  
  
##  <a name="BKMK_AddIn"></a>Sobre el estado informar de complemento  
 El complemento de informe de estado para Windows Server Essentials proporciona consolidada información acerca de la red de Windows Server Essentials y te permite distribuir esta información a otras personas. Esta información puede verse en la **informes** ficha del panel. Con la **informes** ficha, puedes hacer lo siguiente:  
  
-   [Generar un informe a petición o mediante programación](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Generate)  
  
-   [Personalizar el contenido del informe](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Customize)  
  
-   [El informe de correo electrónico](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_emailreport)  
  
> [!NOTE]
>  **Windows Server Essentials:** puede descargar el complemento de informe de estado para Windows Server Essentials desde el [Microsoft Download Center](https://go.microsoft.com/fwlink/p/?LinkId=266342).  
>   
>  **Windows Server Essentials:** de manera predeterminada, el complemento de informe de estado se integra con Windows Server Essentials o Windows Server 2012 R2 con el rol de Windows Server Essentials Experience instalado, y los informes de estado se muestran en la **informes de mantenimiento** ficha del panel s **Home** página.  
  
###  <a name="BKMK_Generate"></a>Generar un informe a petición o mediante programación  
 Después de instalar el complemento de informe de estado y reiniciar el panel, en una pestaña nueva, **informes** se agrega al panel. Puedes generar un informe de estado a petición en cualquier momento haciendo clic en el **generar un informe de estado** de tareas en la **informes** pestaña.  
  
 Después de que se genera un informe de estado, se crea un nuevo elemento en el panel de lista, identificado por la fecha y hora que se generó el informe. Para abrir un elemento, puede hacer doble clic en el panel lista, o puedes seleccionarlo y, a continuación, haz clic en **abrir el informe estado** en el panel de tareas. El informe se muestra en una ventana nueva en formato HTML.  
  
 Además de generar un informe manualmente, también podrías el informe se generan automáticamente según una programación diariamente. Para ello, en el panel de tareas, haz clic en **personalizar la configuración de informe de estado**y, a continuación, haz clic en el **programación y correo electrónico** pestaña. La **programación** característica está desactivada de manera predeterminada, y activarla seleccionando la **generar un informe de estado a su hora programada** casilla de verificación.  
  
###  <a name="BKMK_Customize"></a>Personalizar el contenido del informe  
 El informe de estado contiene lo siguiente:  
  
-   **Las alertas críticas y advertencias** esto es coherente con las alertas críticas y advertencias que ves en el Visor de alertas en el panel. Alertas de la información no se incluyen en el informe de estado.  
  
-   **Errores críticos en los registros de eventos** se analizan los registros de aplicaciones y servicios, y se presentan los errores que se registran en las últimas 24 horas en el **detalles** sección del informe.  
  
-   **Copia de seguridad del servidor** la información acerca de la última copia de seguridad de servidor se presenta en la **detalles** sección del informe.  
  
-   **Inicio automático servicios no se está ejecutando** en el momento en que se genera el informe, si no se está ejecutando un servicio de inicio automático, la información sobre este servicio se muestran en la **detalles** sección del informe.  
  
-   **Actualizaciones** puedes ver el estado de actualización del servidor y todos los equipos cliente en la **detalles** sección.  
  
-   **Almacenamiento** la lista de unidades y su capacidad se presenta en la **detalles** sección.  
  
 En el informe de estado, ver la **resumen**y para esos elementos con un icono de error rojo o un icono de advertencia amarillo, haz clic en el **detalles** vínculo en la misma fila para ver los detalles sobre el elemento.  
  
 Si no estás interesado en algunos de los puntos de datos que se incluyen en el informe de manera predeterminada, puedes personalizar el contenido del informe haciendo clic en **personalizar la configuración de informe de estado** en el panel de tareas y haces clic en el **contenido**ficha desactiva las casillas de verificación para el contenido que don t quieras ver en el informe. Por ejemplo, si tiene su propio plan de copia de seguridad del servidor y don t quieres ver las advertencias sobre las copias de seguridad del servidor, podrías excluir las copias de seguridad del servidor del informe desactivando el **copia de seguridad del servidor** casilla de verificación.  
  
###  <a name="BKMK_emailreport"></a>El informe de correo electrónico  
 Tener que iniciar sesión en el panel para leer informes es aún incómodo algunos administradores, especialmente si tienen más de un servidor para administrar. Con la característica de correo electrónico activado, después de que se genera un informe, se enviará un correo electrónico a una lista de direcciones de correo electrónico especificada con el contenido del informe. El administrador fácilmente puedes ver este informe desde cualquier dispositivo o cualquier aplicación de cliente y garantizar que el servidor se ejecuta en su estado mejor.  
  
 En la **personalizar la configuración de informe de estado** cuadro de diálogo, después de habilitar el correo electrónico, cambiar la configuración de SMTP y especificar una lista de los destinatarios del correo electrónico, observarás que una nueva tarea aparece en el panel de tareas: **el informe de estado de correo electrónico**. Para obtener más información sobre la configuración SMTP, consulta [configurar notificaciones de correo electrónico de alertas](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Email).  
  
 Puedes seleccionar un informe existente y, a continuación, haz clic en **el informe de estado de correo electrónico**. También puede generar un nuevo informe y que se envíe automáticamente a tu Bandeja de entrada. Si has configurado una programación para el informe se genera automáticamente, el informe se entregarán automáticamente en la Bandeja de entrada después de que se genera cada día (o cada hora), como se ha programado.  
  
##  <a name="BKMK_View"></a>Ver avisos mediante el Visor de alertas  
 En esta sección se describe cómo usar el panel o del Launchpad para abrir el Visor de alertas para ver el estado de todos los equipos en la red del servidor.  
  
#### <a name="to-open-the-alert-viewer-by-using-the-dashboard"></a>Para abrir el Visor de alertas mediante el panel  
  
1.  Abra el panel.  
  
2.  En el panel de navegación, haz clic en cualquiera de los iconos de alertas mostradas (crítico, advertencia o informativos). Así abre el Visor de alertas.  
  
#### <a name="to-open-the-alert-viewer-from-the-launchpad"></a>Para abrir el Visor de alertas en el Launchpad  
  
1.  Desde un equipo que está conectado al servidor, abre el Launchpad. Si se te pide, inicie sesión en el Launchpad con tu nombre de usuario y contraseña.  
  
2.  Haz clic en cualquiera de los iconos alertas mostrados (críticos, advertencias e informativos) en la parte inferior de la Launchpad para abrir el Visor de alertas y, a continuación, sigue las instrucciones en el panel de detalles del Visor de alerta para resolver la alerta.  
  
##  <a name="BKMK_Organize"></a>Organizar las alertas en el Visor de alertas  
 Puedes organizar las alertas en el Visor de alertas y tener mostrarlas en función de su gravedad (crítica, advertencia o informativos) o en el nombre del equipo.  
  
#### <a name="to-organize-alerts-in-the-alert-viewer"></a>Para organizar las alertas en el Visor de alertas  
  
1.  Abra el panel.  
  
2.  En el panel de navegación, haz clic en cualquiera de los iconos de alertas mostradas (crítico, advertencia o informativos). Así abre el Visor de alertas.  
  
3.  Expande la **organizar** lista desplegable y, a continuación, realiza una de las siguientes acciones:  
  
    1.  Selecciona **filtrar por equipo**y haz clic en el nombre del equipo para el que quieres ver las alertas. Esto muestra alertas en el Visor de alertas solo para el equipo seleccionado.  
  
    2.  Selecciona **filtrar por tipo de alerta**y haz clic en el tipo de alerta (crítico, advertencia o informativos) para el que quieres ver las alertas. Esto muestra solo el tipo de alerta seleccionado en el Visor de alertas.  
  
##  <a name="BKMK_Respond"></a>Responder a las alertas  
 Cuando encuentres una alerta, puedes elegir que realiza una de las siguientes acciones:  
  
-   [Resolver una alerta](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Resolve)  
  
-   [Omitir una alerta](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Habilitar una alerta](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Eliminar una alerta](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_4)  
  
###  <a name="BKMK_Resolve"></a>Resolver una alerta  
 Sigue las instrucciones de resolución en el Visor de alertas para resolver la alerta. Una vez resuelto una alerta, aún se muestra en el Visor de alertas hasta que se actualiza.  
  
###  <a name="BKMK_3"></a>Omitir una alerta  
 Puedes pasar por alto una alerta si prefieres que responder a él más adelante. Cuando se pasa por alto una alerta, aún se indica en el Visor de alerta y pero está deshabilitado y atenuado. Una alerta omitida no está incluida en la evaluación de la salud general del equipo. Para mitigar una alerta omitida, debes habilitar la alerta.  
  
##### <a name="to-ignore-an-alert"></a>Para omitir una alerta  
  
1.  En el equipo que está conectado al servidor de Windows Server Essentials, abra el Launchpad.  
  
2.  En el Launchpad, haz clic en cualquiera de los iconos de alerta mostrados (críticos, advertencias e informativos). Así abre el Visor de alertas.  
  
3.  En el Visor de alertas, seleccione la alerta que quieres hacer caso omiso, y, a continuación, en la **tareas** sección, haz clic en **ignorar la alerta**.  
  
 Para responder a una alerta deshabilitada, debes habilitar la alerta.  
  
###  <a name="BKMK_5"></a>Habilitar una alerta  
 Puedes habilitar una alerta que decide omitir anteriormente. Después de habilita la alerta, puedes resolverlo o eliminarla según sea necesario. Aparece una alerta como deshabilitado cuando se marca para que se ignorará. Al habilitar una alerta que previamente ha deshabilitado, se activa y nuevo se incluye en la evaluación de la salud general de equipos.  
  
##### <a name="to-enable-an-alert"></a>Para permitir que una alerta  
  
1.  En el equipo que está conectado al servidor, abra el Launchpad.  
  
2.  En el Launchpad, haz clic en cualquiera de los iconos alertas mostrados (críticos, advertencias e informativos) para abrir el Visor de alertas.  
  
3.  En el Visor de alertas, haz clic en la alerta que quieras habilitar y, a continuación, haz clic en **habilitar la alerta**.  
  
###  <a name="BKMK_4"></a>Eliminar una alerta  
 Puedes eliminar una alerta si no quieres resolver u omitirla. Puedes usar el Visor de alertas en el Launchpad eliminar alertas generadas para el equipo. Si eliminas una alerta y el servidor detecta el problema nuevo en el próximo ciclo de evaluación de estado de red, genera una alerta de nuevo.  
  
##### <a name="to-delete-an-alert"></a>Para eliminar una alerta  
  
1.  En el equipo que está conectado al servidor, abra el Launchpad.  
  
2.  En el Launchpad, haz clic en cualquiera de los iconos alertas mostrados (críticos, advertencias e informativos) para abrir el Visor de alertas.  
  
3.  En el Visor de alertas, haz clic en la alerta que quieras eliminar y, a continuación, haz clic en **eliminar la alerta**.  
  
##  <a name="BKMK_Email"></a>Configurar notificaciones de correo electrónico de alertas  
 Puedes configurar el servidor para recibir una notificación por correo electrónico sobre la aparición de alertas. Las notificaciones de correo electrónico para estas alertas contienen información sobre los problemas de red y los pasos de resolución, que es idéntica a la información que se muestra en el Visor de alertas. Algunas de las evaluaciones de estado de red se realizan mediante programación.  
  
 Cuando configura tu servidor para enviar notificaciones de alertas por correo electrónico, se envía una notificación de correo electrónico para las alertas que se encuentran durante la evaluación de estado de red. Sin embargo, no todas las alertas que se notifican en el Visor de alertas se notifican en un correo electrónico.  
  
 Cada 30 minutos, la tarea de evaluación de correo electrónico de alerta que se ejecuta en el servidor, lo que da como resultado de la red de alertas. Correo electrónico se envía una notificación out si todas las alertas que se establece para la notificación por correo electrónico se produce. No se envía un correo electrónico de segundo si la alerta todavía está activa en el próximo ciclo de evaluación para evitar el desbordamiento de su buzón. Sin embargo, si se detecta una alerta de nuevo en un ciclo de evaluación de alerta de futuras, es una notificación por correo electrónico enviado, que incluye las alertas anteriores y nuevo.  
  
###  <a name="BKMK_list"></a>Avisos que se convertirán en las notificaciones de correo electrónico  
 Las siguientes alertas en el Visor de alertas ocasionar notificaciones por correo electrónico al configurar el servidor para enviar notificaciones de correo electrónico para alertas:  
  
-   Existen errores en una copia de seguridad del equipo cliente.  
  
-   El servidor se reinició.  
  
-   Uno o varios servicios no se están ejecutando.  
  
-   No se está ejecutando el servicio de almacenamiento de Windows Server.  
  
-   El período de evaluación es a través.  
  
-   Activar ahora.  
  
-   Se cierra el servidor de origen.  
  
-   Resultados del examen BPA con errores.  
  
-   Resultados del examen BPA contienen advertencias.  
  
-   Un certificado no está disponible para el acceso desde cualquier lugar.  
  
-   Ha caducado el certificado para el acceso desde cualquier lugar.  
  
-   El enrutador no está correctamente configurado.  
  
-   El servidor Web no está configurado correctamente.  
  
-   Servicios de escritorio remoto no está correctamente configurado.  
  
-   El firewall no está correctamente configurado.  
  
-   El nombre de dominio de Internet ha expirado.  
  
-   No se puede actualizar el nombre de dominio de Internet.  
  
-   Error de licencia: Comprobación de confianza de bosque.  
  
-   Error de licencia: Comprobación de controlador de dominio.  
  
-   Error de licencia: Comprobación de la función FSMO.  
  
-   Error de licencia: Aplicación FSMO directivas.  
  
-   Error de licencia: Directivas de carga de la aplicación.  
  
-   Error de licencia: Los servicios de dominio de Active Directory.  
  
-   Tu suscripción a Office 365 ha expirado.  
  
-   Autenticación de Office 365 no se realizó correctamente.  
  
-   La directiva de contraseña no es correcta.  
  
-   El servicio de sincronización de contraseña no puede sincronizar una contraseña de usuario con Office 365.  
  
-   Cambiar la contraseña de Windows.  
  
-   La contraseña de Office 365 no es el mismo que la contraseña de Windows.  
  
-   No puedes conectarte a Exchange Server.  
  
-   Microsoft Exchange Server tiene un problema.  
  
-   Uno o más discos duros en copia de seguridad del servidor no están conectados.  
  
-   La unidad de disco dura de copia de seguridad no tiene suficiente espacio libre para la copia de seguridad de servidor.  
  
-   Copia de seguridad del servidor no fue correcto porque no se puede tomar una instantánea de la unidad.  
  
-   Una copia de seguridad programada no se completó correctamente.  
  
-   Faltan uno o más carpetas del servidor predefinidas.  
  
-   Espacio libre se está quedando sin uno o más discos duros del servidor.  
  
-   No se está ejecutando el escritor VSS para el servicio de almacenamiento.  
  
-   Capacidad de almacenamiento bajo en unidades de disco duro.  
  
-   Una o varias unidades que no están funcionando y están desconectadas.  
  
###  <a name="BKMK_SMTP"></a>Configuración de SMTP en tu servidor para enviar notificaciones de alertas por correo electrónico en Windows Server Essentials  
 En esta sección se describe cómo configurar el servidor para enviar notificaciones de correo electrónico de alertas.  
  
> [!NOTE]
>  Puedes descargar el complemento de informe de estado para Windows Server Essentials desde el [Microsoft Download Center](https://go.microsoft.com/fwlink/p/?LinkId=266342).  
  
##### <a name="to-set-up-email-notification-for-alerts"></a>Configurar la notificación de correo electrónico de alertas  
  
1.  Desde el **panel**, abre el **Visor de alertas**.  
  
2.  En la **Visor de alertas**, haz clic en **configurar notificaciones de correo electrónico de alerta**.  
  
3.  En la **configurar correo electrónico notificación de alertas** ventana, haz clic en **habilitar**.  
  
4.  En la **SMTP configuración** ventana, haz lo siguiente:  
  
    1.  Para **desde la dirección de correo electrónico**, escribe la dirección de correo electrónico que quieras usar para enviar el correo electrónico avisos de. Esta dirección de correo electrónico se mostrará como la dirección del remitente s en la notificación de alerta.  
  
    2.  Para **nombre del servidor SMTP**, en la **desde la dirección de correo electrónico** texto, escriba el nombre del servidor SMTP que especificaste en el paso 4. (Para obtener una lista de algunos nombres de servidor SMTP, consulte la tabla 1).  
  
    3.  Para **puerto SMTP**, escribe el número de puerto que se usa el servidor SMTP para enviar y recibir correo electrónico. (Para los números de puerto que usan algunos de los servidores SMTP, consulte la tabla 1).  
  
    4.  Selecciona **este servidor requiere una conexión segura (SSL)** si el servidor SMTP usa SSL (consulta la tabla 1).  
  
    5.  Selecciona **este servidor requiere autenticación** si el servidor SMTP requiere una información de nombre y la contraseña de usuario (consulta la tabla 1). Si activas esta casilla, escribe el nombre de usuario y contraseña de la dirección de correo electrónico que proporcionaste en la **desde la dirección de correo electrónico** campo en el paso 4 y, a continuación, haz clic en **Aceptar**.  
  
    > [!NOTE]
    >  Puede obtener la información sobre el nombre del servidor SMTP, el número de puerto y el uso SSL de tu proveedor de servicios de Internet.  
  
     **Tabla 1** nombres de servidor de ejemplos de SMTP, autenticación y los requisitos de cifrado SSL y números de puerto  
  
    |Servidor SMTP|Requerido SSL|Se requiere autenticación|Número de puerto|Nombre de cuenta o nombre de cuenta|  
    |-----------------|------------------|-----------------------------|-----------------|------------------------------|  
    |SMTP.Gmail.com|Sí|Sí|587|Proporcionar la dirección de correo electrónico completa con el nombre de dominio y la contraseña para la autenticación.|  
    |SMTP.Live.com|Sí|Sí|587|Proporcionar la dirección de correo electrónico completa con el nombre de dominio y la contraseña para la autenticación.|  
    |SMTP.Comcast.NET|Sí|No|587|Proporcionar la dirección de correo electrónico completa con el nombre de dominio y la contraseña para la autenticación.|  
    |SMTP.Mail.yahoo.com|No|Sí|25|Proporciona la dirección de correo electrónico sin un nombre de dominio para el nombre de usuario.|  
  
5.  En **configurar la notificación de alertas**, para **destinatarios por correo electrónico**, escribe las direcciones de correo electrónico de las personas que quieres recibir las notificaciones de alertas por correo electrónico. Asegúrate de que separa cada dirección de correo electrónico con un punto y coma (;).  
  
6.  Para comprobar que ha configurado correctamente para enviar notificaciones de correo electrónico para las alertas, haz clic en el servidor de SMTP **aplicar y enviar correo electrónico**.  
  
    > [!NOTE]
    >  Al hacer clic en **aplicar y enviar correo electrónico**, normalmente recibirá una notificación de correo electrónico de ejemplo con ningún alertas de estado que aparece. Sin embargo, si se identifica una alerta de salud que está configurada para enviar una notificación por correo electrónico durante este proceso de prueba, esta alerta se incluye en el correo electrónico de prueba.  
  
### <a name="configuring-smtp-on-your-server-to-send-health-reports-in-windows-server-essentials"></a>Configuración de SMTP en tu servidor para enviar informes de mantenimiento en Windows Server Essentials  
 En esta sección se describe cómo configurar la configuración de SMTP de tu servidor para que pueda recibir informes de estado a través de correo electrónico.  
  
> [!NOTE]
>  De manera predeterminada, el complemento de informe de estado se integra con Windows Server Essentials o Windows Server 2012 R2 con el rol de Windows Server Essentials Experience instalado, y los informes de estado se muestran en la **informes de mantenimiento** ficha del panel s **Home** página.  
  
##### <a name="to-set-up-email-notification-for-health-reports"></a>Configurar la notificación de correo electrónico para los informes de estado  
  
1.  Desde el **panel**, haz clic en el **informes** pestaña.  
  
2.  En la **tareas de informe de estado** panel de tareas, haz clic en **personalizar la configuración de informe de estado**.  
  
3.  En la **personalizar la configuración de informe de estado** ventana, haz clic en el **programación y correo electrónico** pestaña.  
  
4.  En la **programación y correo electrónico** pestaña la **correo electrónico** sección, haz lo siguiente:  
  
    1.  Haz clic en **habilitar**y escribe la dirección de correo electrónico que quieras usar para enviar el estado de informes de. Esta dirección de correo electrónico se mostrará como la dirección del remitente s en los informes de estado que se envían por correo electrónico.  
  
        1.  Para **nombre del servidor SMTP**, escribe el nombre del servidor SMTP. (Para obtener una lista de algunos nombres de servidor SMTP, consulte la tabla 1).  
  
        2.  Para **puerto SMTP**, escribe el número de puerto que se usa el servidor SMTP para enviar y recibir correo electrónico. (Para los números de puerto que usan algunos de los servidores SMTP, consulte la tabla 1).  
  
        3.  Selecciona **este servidor requiere una conexión segura (SSL)** si el servidor SMTP usa SSL (consulta la tabla 1).  
  
        4.  Selecciona **este servidor requiere autenticación** si el servidor SMTP requiere una información de nombre y la contraseña de usuario (consulta la tabla 1). Si activas esta casilla, escribe el nombre de usuario y contraseña de la dirección de correo electrónico que proporcionaste en la **desde la dirección de correo electrónico** campo en el paso 4 y, a continuación, haz clic en **Aceptar**.  
  
            > [!NOTE]
            >  Puede obtener la información sobre el nombre del servidor SMTP, el número de puerto y el uso SSL de tu proveedor de servicios de Internet.  
  
            > [!NOTE]
            >  Microsoft recomienda encarecidamente que puedes usar SSL porque el informe puede contener el estado del servidor que partes malintencionadas podría usar para detectar las vulnerabilidades (por ejemplo: falta actualización de windows). Habilitar SSL se cifrar los datos en tránsito y mitigar el riesgo de revelar la vulnerabilidad en el servidor.  
  
5.  Después de habilitar el correo electrónico, el **cambio SMTP configuración** muestra el vínculo. También es una tarea nueva, **el informe de estado de correo electrónico**, obtiene mostrado en **tareas de informe de estado**.  
  
     **Tabla 1** nombres de servidor de ejemplos de SMTP, autenticación y los requisitos de cifrado SSL y números de puerto  
  
    |Servidor SMTP|Requerido SSL|Se requiere autenticación|Número de puerto|Nombre de cuenta o nombre de cuenta|  
    |-----------------|------------------|-----------------------------|-----------------|------------------------------|  
    |SMTP.Gmail.com|Sí|Sí|587|Proporcionar la dirección de correo electrónico completa con el nombre de dominio y la contraseña para la autenticación.|  
    |SMTP.Live.com|Sí|Sí|587|Proporcionar la dirección de correo electrónico completa con el nombre de dominio y la contraseña para la autenticación.|  
    |SMTP.Comcast.NET|Sí|No|587|Proporcionar la dirección de correo electrónico completa con el nombre de dominio y la contraseña para la autenticación.|  
    |SMTP.Mail.yahoo.com|No|Sí|25|Proporciona la dirección de correo electrónico sin un nombre de dominio para el nombre de usuario.|  
  
6.  En **personalizar la configuración de informe de estado**, para **enviar automáticamente el informe de estado a los siguientes destinatarios del correo electrónico:**, escribe las direcciones de correo electrónico de las personas que quieres recibir informes de estado por correo electrónico. Asegúrate de que separa cada dirección de correo electrónico con un punto y coma (;).  
  
7.  Para comprobar que ha configurado el servidor SMTP configuración correctamente para enviar informes de estado a través de correo electrónico, desde la pestaña informe Helath en el panel, selecciona un informe y haz clic en **el informe de estado de correo electrónico** desde el panel de tareas.  
  
##  <a name="BKMK_Potential"></a>Posibles alertas de equipo  
 Esta sección describe comprender y administrar las alertas que son específicas de su equipo que esté conectado al servidor y que aparecen en el Launchpad de su equipo.  
  
 La siguiente tabla enumeran algunas de las alertas de equipo que pueden generar y muestran en el Visor de alertas si son aplicables a su equipo.  
  
|Título de la alerta|Resolución y el impacto de alerta|  
|-----------------|---------------------------------|  
|El estado actual de Firewall de red ofrece protección reducido para este equipo.|Personas no autorizadas o software posible tener acceso a este equipo si Firewall de Windows no está activado.|  
|Protección antivirus está desactivado, no instalado o no está actualizada.|Los datos en el equipo están en riesgo si la **protección antivirus** configuración de seguridad está desactivada o no actualizado. [Para proteger su equipo](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Protect), siga los pasos indicados.|  
|El spyware y software no deseado protección está desactivada, no instalado o no está actualizada.|Los datos en el equipo están en riesgo si la **Spyware y no deseado de protección de software** desactivada o no actualizados. [Para proteger su equipo](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Protect), siga los pasos indicados.|  
|Windows Update está desactivada.|No podrá beneficiarse de las funcionalidades nuevas y corregida de actualizaciones, a menos que Windows Update esté activado. Para activar Windows Update, en el Visor de alertas, haz clic en **abre Windows Update**.<br /><br /> Si la **abre Windows Update** tarea no se muestra, no ha iniciado sesión en el equipo donde se provocó la alerta. Debes iniciar sesión en el equipo en el que se generó la alerta para ejecutar esta tarea en el Visor de alertas.|  
|Se deben instalar las actualizaciones importantes.|No podrá beneficiarse de las funcionalidades nuevas y corregida de actualizaciones, a menos que Windows Update esté activado. Para activar Windows Update, en el Visor de alertas, haz clic en **abre Windows Update**.<br /><br /> Si la **abre Windows Update** tarea no se muestra, no ha iniciado sesión en el equipo donde se provocó la alerta. Debes iniciar sesión en el equipo en el que se generó la alerta para ejecutar esta tarea en el Visor de alertas.|  
|Reinicia el equipo para aplicar actualizaciones.|No podrá beneficiarse de las funcionalidades nuevas y corregida de las actualizaciones hasta que se aplican. Guarda todos los datos y reinicia el equipo para aplicar las actualizaciones.|  
|Espacio libre es baja en unidades de disco duro.|Si no se ofrezca de espacio, no podrás guardar información adicional. Para aumentar el espacio libre en el equipo, ten en cuenta las siguientes opciones:<br /><br /> -Agregar una nueva unidad de disco dura.<br /><br /> -Ejecuta **Liberador de espacio en disco** para quitar archivos temporales y antiguos.<br /><br /> : Mueve los archivos a una carpeta compartida en otro equipo.<br /><br /> -Los archivos en medios extraíbles, como un CD, DVD o un disco duro externo.|  
|La **historial de archivos** agente en el servidor no está correctamente configurado para ejecutarse en este equipo.|No se puede crear copias de seguridad de historial de archivos.|  
|Uno o varios servicios no se están ejecutando.||  
|Cambiar la contraseña de Windows.||  
|La contraseña de Microsoft Office 365 no es el mismo que la contraseña de Windows.||  
  
###  <a name="BKMK_Protect"></a>Para proteger el equipo  
  
1.  Abrir el centro de seguridad.  
  
2.  Determinar el estado de la protección antivirus instalado.  
  
3.  Realiza una de las siguientes tareas según el estado de protección:  
  
    -   Si no está habilitado, habilitarlo.  
  
    -   Si no está actualizado, completar el proceso para actualizar las firmas.  
  
    -   Si no está instalada la protección contra virus, considera la posibilidad de instalarla.  
  
## <a name="see-also"></a>Consulta también  
  
-   [Usar Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)  
  
-   [Administrar Windows Server Essentials](Manage-Windows-Server-Essentials.md)
