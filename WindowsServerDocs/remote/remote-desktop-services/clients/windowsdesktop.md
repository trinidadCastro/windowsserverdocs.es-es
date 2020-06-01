---
title: Introducción al cliente de escritorio de Windows
description: Información básica sobre el cliente del escritorio de Windows.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
author: heidilohr
manager: lizross
ms.author: helohr
ms.date: 05/26/2020
ms.localizationpriority: medium
ms.openlocfilehash: c229eefbc0cc00ed1af940cd986c89e979873d29
ms.sourcegitcommit: 4fec7d82f0772d03a9e8cac20092a4309b0f796e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2020
ms.locfileid: "84025506"
---
# <a name="get-started-with-the-windows-desktop-client"></a>Introducción al cliente de escritorio de Windows

>Se aplica a: Windows 10, Windows 10 IoT Enterprise y Windows 7

El cliente de Escritorio remoto para el escritorio de Windows se puede usar para acceder a aplicaciones y escritorios de Windows de forma remota desde otro dispositivo Windows.

> [!NOTE]
> - Esta documentación no se aplica al cliente de Conexión a Escritorio remoto (MSTSC) que se distribuye con Windows. Se aplica al nuevo cliente de Escritorio remoto (MSRDC).
> - Actualmente, este cliente solo admite el acceso a aplicaciones y escritorios remotos desde el [Escritorio virtual de Windows](https://aka.ms/wvd).
> - ¿Quieres más información acerca de las nuevas versiones del cliente del escritorio de Windows? Consulta las [Novedades del cliente de escritorio de Windows](windowsdesktop-whatsnew.md)

## <a name="install-the-client"></a>Instalar el cliente

Elige el cliente que coincida con la versión de Windows. El nuevo cliente de Escritorio remoto (MSRDC) es compatible con dispositivos cliente Windows 10, Windows 10 IoT Enterprise y Windows 7.

- [Windows de 64 bits](https://go.microsoft.com/fwlink/?linkid=2068602)
- [Windows de 32 bits](https://go.microsoft.com/fwlink/?linkid=2098960)
- [Windows ARM64](https://go.microsoft.com/fwlink/?linkid=2098961)

Puedes instalar el cliente para el usuario actual, lo que no requiere derechos de administrador, o bien el administrador puede instalar y configurar el cliente para que todos los usuarios del dispositivo puedan acceder a él.

Una vez instalado, el cliente se puede iniciar desde el menú Inicio al buscar **Escritorio remoto.**

## <a name="update-the-client"></a>Actualizar el cliente

Se te enviará una notificación cuando haya una nueva versión del cliente disponible siempre y cuando el administrador no haya deshabilitado las notificaciones. La notificación aparecerá en Connection Center (Centro de conexión) o en el centro de actividades de Windows. Para actualizar el cliente, solo tienes que seleccionar la notificación.

También puedes buscar manualmente nuevas actualizaciones para el cliente:

1. En el centro de conexiones, pulsa en el menú de desbordamiento ( **...** ) en la barra de comandos de la parte superior del cliente.
2. Selecciona **Acerca de** en el menú desplegable.
3. El cliente busca actualizaciones automáticamente.
4. Si hay una actualización disponible, pulsa en **Instalar actualización** para actualizar el cliente.

## <a name="workspaces"></a>Áreas de trabajo

Obtén la lista de recursos administrados a los que puedes tener acceso, como aplicaciones y equipos de escritorio, al suscribirte al área de trabajo que te proporcionó el administrador. Al suscribirte, los recursos están disponibles en el equipo local. Actualmente, el cliente del escritorio de Windows admite los recursos publicados desde el escritorio virtual de Windows.

### <a name="subscribe-to-a-workspace"></a>Suscripción a un área de trabajo

Hay dos maneras de suscribirse a un área de trabajo. El cliente puede intentar detectar los recursos disponibles en tu cuenta profesional o educativa, o bien puedes especificar directamente la dirección URL en la que se encuentran los recursos para los casos en los que el cliente no pueda encontrarlos. Una vez que te hayas suscrito a un área de trabajo, podrás iniciar los recursos con uno de los métodos siguientes:

- Ve al centro de conexiones y haz doble clic en un recurso para iniciarlo.
- También puedes ir al menú Inicio y buscar una carpeta con el nombre del área de trabajo o escribir el nombre del recurso en la barra de búsqueda.

#### <a name="subscribe-with-a-user-account"></a>Suscripción con una cuenta de usuario

1. En la página principal del cliente, pulsa **Subscribirse**.
2. Inicia sesión con tu cuenta de usuario cuando se te solicite.
3. Los recursos aparecerán en el centro de conexiones agrupados por área de trabajo.

#### <a name="subscribe-with-url"></a>Suscripción con dirección URL

1. En la página principal del cliente, pulsa **Subscribe with URL** (Subscribirse con dirección URL).
2. Escribe la dirección URL del área de trabajo o tu dirección de correo electrónico:
   - Si usas la **URL del espacio de trabajo**, usa la que te proporcionó el administrador. Si accedes a los recursos de Windows Virtual Desktop, puedes usar una de las siguientes direcciones URL:
     - Windows Virtual Desktop de otoño de 2019: `https://rdweb.wvd.microsoft.com/api/feeddiscovery/webfeeddiscovery.aspx`
     - Windows Virtual Desktop de primavera de 2020: `https://rdweb.wvd.microsoft.com/api/arm/feeddiscovery`
   - Para usar un **correo electrónico**, escribe tu dirección de correo. Esto indica al cliente que busque una dirección URL asociada a tu dirección de correo electrónico si el administrador ha configurado la [detección de correo electrónico](../rds-email-discovery.md).
3. Pulsa **Siguiente**.
4. Inicia sesión con tu cuenta de usuario cuando se te solicite.
5. Los recursos aparecerán en el centro de conexiones agrupados por área de trabajo.

### <a name="workspace-details"></a>Detalles del área de trabajo

Después de suscribirte, puedes ver información adicional sobre un área de trabajo en el panel de detalles:

- Nombre del área de trabajo
- URL y nombre de usuario usados para suscribirse
- Número de aplicaciones y escritorios
- Fecha y hora de la última actualización
- Estado de la última actualización

Acceso al panel de detalles:

1. En el centro de conexión, pulsa el menú de desbordamiento ( **...** ) que hay al lado del área de trabajo.
2. Selecciona **Detalles** en el menú desplegable.
3. El panel de detalles aparece en el lado derecho del cliente.

Después de suscribirte, el área de trabajo se actualizará automáticamente de forma periódica. Se pueden agregar, cambiar o quitar recursos en función de los cambios que realiza el administrador.

También puedes buscar manualmente las actualizaciones de los recursos cuando sea necesario. Para ello, selecciona **Actualizar** en el panel de detalles.

### <a name="refreshing-a-workspace"></a>Actualización de un área de trabajo

Para actualizar manualmente un área de trabajo, selecciona **Actualizar** en el menú de desbordamiento ( **...** ) situado junto al área de trabajo.

### <a name="unsubscribe-from-a-workspace"></a>Cancelación de una suscripción a un área de trabajo

En esta sección se indica el proceso para cancelar tu suscripción a un área de trabajo. Puedes cancelar la suscripción para suscribirte de nuevo con una cuenta diferente o quitar los recursos del sistema.

1. En el centro de conexión, pulsa el menú de desbordamiento ( **...** ) que hay al lado del área de trabajo.
2. Selecciona **Cancelar suscripción** en el menú desplegable.
3. Revisa el cuadro de diálogo y selecciona **Continuar**.

## <a name="managed-desktops"></a>Escritorios administrados

Las áreas de trabajo pueden contener varios recursos administrados, incluidos los escritorios. Al acceder a un escritorio administrado, tienes acceso a todas las aplicaciones que instaló el administrador.

### <a name="desktop-settings"></a>Configuración del escritorio

Puedes configurar algunas de las opciones de configuración de los recursos de escritorio para asegurarte de que la experiencia satisface tus necesidades. Para acceder a la lista de opciones de configuración disponibles, haz clic con el botón derecho en el recurso de escritorio y selecciona **Configuración**.

El cliente usará las opciones que configuró el administrador, a menos que se desactive la opción **Usar configuración predeterminada**. Esto te permite configurar las siguientes opciones:

- **Display configuration** (Configuración de pantalla): selecciona las pantallas que se van a usar para la sesión de escritorio y afecta a la configuración adicional disponible.
  - **All displays** (Todas las pantallas): garantiza que la sesión siempre usa todas las pantallas locales, aunque algunas de ellas se agreguen o se quiten más adelante.
  - **Single display** (Pantalla única) garantiza que la sesión siempre usará una sola pantalla y permite configurar sus propiedades.
  - **Select displays** (Seleccionar pantallas) permite elegir las pantallas que se usarán en la sesión y ofrece la opción de cambiar dinámicamente la lista de pantallas durante la sesión.
- La opción **Select the displays to use for the session** (Seleccionar las pantallas que se van a usar para la sesión) especifica qué pantallas locales se usarán para la sesión. Todas las pantallas seleccionadas deben ser adyacentes entre sí. Esta opción solo está disponible en el modo **Select display** (Seleccionar pantalla).
- **Maximize to current displays** (Maximizar a las pantallas actuales) determina qué pantallas usarán las sesiones al pasar al modo de pantalla completa. Cuando está habilitada, la sesión pasa a pantalla completa en las pantallas tocadas por la ventana de sesión. Esto te permite cambiar las pantallas durante la sesión. Cuando está deshabilitada, la sesión pasa a pantalla completa en las mismas pantallas en las que estaba la última vez que pasó a pantalla completa. Esta opción solo está disponible en el modo **Select display** (Seleccionar pantalla); en todos los demás, está deshabilitada.
- **Single display when windowed** (Pantalla única en ventana) determina qué pantallas están disponibles en la sesión al salir de la pantalla completa. Cuando está habilitada, la sesión cambia a una sola pantalla en el modo de ventana. Cuando está deshabilitada, la sesión conserva las mismas pantallas en el modo de ventana que en el modo de pantalla completa. Esta opción solo está disponible en los modos **All displays** (Todas las pantallas) y **Select display** (Seleccionar pantalla), y está deshabilitada en todos los demás.
- La opción **Start in full screen** (Iniciar en pantalla completa) determina si la sesión se iniciará en modo de pantalla completa o de ventana. Esta opción solo está disponible en el modo **Select display** (Seleccionar pantalla) y está deshabilitada en todos los demás.
- La opción **Fit session to window** (Ajustar sesión a la ventana) determina cómo se muestra la sesión cuando la resolución del escritorio remoto difiere del tamaño de la ventana local. Cuando se habilite, se cambiará el tamaño del contenido de la sesión para ajustarse dentro de la ventana, a la vez que se conservará la relación de aspecto de la sesión. Cuando se deshabilite, las barras de desplazamiento o las áreas negras se mostrarán cuando la resolución y el tamaño de la ventana no coincidan. Esta opción está disponible en todos los modos.
- La opción **Update the resolution on resize** (Actualizar la resolución al cambiar de tamaño) hace que la resolución del escritorio remoto se actualice automáticamente al cambiar el tamaño de la sesión en el modo de ventana. Cuando está deshabilitada, la sesión siempre permanece en la resolución que especifiques en **Resolución**. Esta opción solo está disponible en el modo **Select display** (Seleccionar pantalla) y está deshabilitada en todos los demás.
- La opción **Resolución** te permite especificar la resolución del escritorio remoto. La sesión conservará esta resolución durante toda la duración. Esta opción solo está disponible en el modo **Single display** (Pantalla única) y cuando **Update the resolution on resize** (Actualizar la resolución al cambiar de tamaño) está deshabilitada.
- La opción **Change the size of the text and apps** (Cambiar el tamaño del texto y las aplicaciones) especifica el tamaño del contenido de la sesión. Esta configuración solo se aplica al conectarse con Windows 8.1 y versiones posteriores o Windows Server 2012 R2 y versiones posteriores. Esta opción solo está disponible en el modo **Single display** (Pantalla única) y cuando **Update the resolution on resize** (Actualizar la resolución al cambiar de tamaño) está deshabilitada.

## <a name="provide-feedback"></a>Proporciona comentarios

¿Tienes alguna sugerencia de característica o quieres informar de un problema? Para indicárnoslo, utiliza el [Centro de opiniones](feedback-hub://?tabid=2&contextid=883). También puedes acceder al Centro de opiniones a través del cliente:

1. En el Centro de conexión, pulsa la opción **Enviar comentarios** de la barra de comandos en la parte superior del cliente para abrir la aplicación Centro de opiniones.
2. Escribe la información necesaria en los campos **Resumen** y **Detalles**. Cuando termines, pulsa **Siguiente**.
3. Selecciona **Problema** o **Sugerencia**, según corresponda.
4. Comprueba si la categoría está en **Aplicaciones** > **Escritorio remoto**. Si es así, pulsa **Siguiente**.
5. Revisa los comentarios existentes para ver si otra persona ha comunicado el mismo problema. Si no es así, selecciona **Crear un nuevo error** y, a continuación, pulsa **Siguiente**.
6. En la página siguiente, puedes proporcionarnos más información para que podamos ayudarte a resolver el problema. Puedes escribir información más detallada, enviar capturas de pantalla e incluso realizar una grabación del problema para mostrarnos lo que ha sucedido. Para realizar una grabación, selecciona **Iniciar grabación** y, después, repite lo que hiciste hasta que se produjo el problema. Cuando termines, vuelve al Centro de opiniones y selecciona **Detener grabación**.
7. Cuando estés satisfecho con la información, pulsa **Enviar**.
8. En la página "Gracias por tus comentarios", pulsa **Share my feedback** (Compartir mis comentarios) para generar un vínculo a tus comentarios que puedas compartir con otros usuarios cuando sea necesario.

### <a name="access-client-logs"></a>Acceder a los registros de cliente

Es posible que necesites los registros de cliente al investigar un problema.

Para recuperar los registros de cliente:

1. Asegúrate de que no haya ninguna sesión activa y de que el proceso del cliente no se esté ejecutando en segundo plano. Para ello, haz clic con el botón derecho en el icono de **Escritorio remoto** en la bandeja del sistema y selecciona **Desconectarse de todas las sesiones**.
2. Abre el **Explorador de archivos**.
3. Navega hasta la carpeta **%temp%\DiagOutputDir\RdClientAutoTrace**.
