---
title: 'Paso 2: configurar WSUS'
description: 'Tema de Windows Server Update Service (WSUS): configurar WSUS es el paso dos en un proceso de cuatro pasos para implementar WSUS'
ms.prod: windows-server-threshold
ms.reviewer: na
ms.technology: manage-wsus
ms.topic: article
ms.assetid: d4adc568-1f23-49f3-9a54-12a7bec5f27c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 386ef1d8683b75bdc94fc1aa4ac7cb8acf6cd6fa
ms.sourcegitcommit: 6f968368c12b9dd699c197afb3a3d13c2211f85b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68544489"
---
# <a name="step-2-configure-wsus"></a>Paso 2: Configurar WSUS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Después de instalar el rol de servidor de WSUS en su servidor, debe configurarlo de forma apropiada. La siguiente lista de comprobación resume los pasos necesarios para realizar la configuración inicial del servidor WSUS.

|Tarea|Descripción|
|----|--------|
|[2,1. Configurar conexiones de red](#21-configure-network-connections)|Configure la red en clúster con el Asistente para configuración de red.|
|[2,2. Configurar WSUS mediante el Asistente para configuración de WSUS](#22-configure-wsus-by-using-the-wsus-configuration-wizard)|Utilizar el Asistente para configuración de WSUS a fin de realizar la configuración básica de WSUS.|
|[2,3. Configurar grupos de equipos de WSUS](#23-configure-wsus-computer-groups)|Cree grupos de equipos en la consola de administración de WSUS para administrar las actualizaciones de su organización.|
|[2,4. Configurar actualizaciones de cliente](#24-configure-client-updates)|Especifique cómo y cuándo se aplican las actualizaciones automáticas en los equipos cliente.|
|[2,5. Proteger WSUS con el protocolo de Capa de sockets seguros](#25-secure-wsus-with-the-secure-sockets-layer-protocol)|Configure el protocolo de Capa de sockets seguros (SSL) para ayudar a proteger Windows Server Update Services (WSUS).|

## <a name="21-configure-network-connections"></a>2.1. Configurar conexiones de red
Antes de iniciar el proceso de configuración, asegúrese de saber responder las siguientes preguntas:

1.  ¿El firewall del servidor está configurado para permitir el acceso de clientes?

2.  ¿Puede este equipo conectarse al servidor que precede en la cadena (como el servidor designado para descargar actualizaciones de Microsoft Update)?

3.  ¿Tiene el nombre del servidor proxy y las credenciales de usuario para este servidor, si los necesita?

De manera predeterminada, WSUS se configura para usar Microsoft Update como ubicación donde obtener actualizaciones. Si tiene un servidor proxy en la red, puede configurar WSUS para que use el servidor proxy. Si hay un Firewall corporativo entre WSUS e Internet, es posible que tenga que configurar el firewall para asegurarse de que WSUS puede obtener las actualizaciones.

> [!TIP]
> Si bien se requiere conectividad a Internet para descargar actualizaciones de Microsoft Update, WSUS ofrece la posibilidad de importar actualizaciones en redes no conectadas a Internet.

Si tiene las respuestas de estas preguntas, puede comenzar a configurar los siguientes parámetros de red de WSUS:

-   **Actualizaciones** Especifique de qué manera este servidor obtendrá actualizaciones (desde Microsoft Update o desde otro servidor WSUS).

-   **Proxy** si identificó que WSUS necesita usar un servidor proxy para tener acceso a Internet, debe configurar los valores de proxy en el servidor WSUS.

-   **Firewall** si identificó que WSUS está detrás de un Firewall corporativo, se deben realizar algunos pasos adicionales en el dispositivo perimetral para permitir el tráfico de WSUS correctamente.

### <a name="211-connection-from-the-wsus-server-to-the-internet"></a>2.1.1. Conexión desde el servidor WSUS a Internet
Si hay un firewall corporativo entre WSUS e Internet, quizás deba configurar el firewall para asegurarse de que WSUS obtenga actualizaciones. Para obtener actualizaciones de Microsoft Update, el servidor WSUS usa el puerto 443 para el protocolo HTTPS. Aunque la mayoría de los firewalls corporativos permiten este tipo de tráfico, hay algunas compañías que restringen el acceso a Internet desde los servidores debido a las directivas de seguridad de la compañía. Si su compañía restringe el acceso, deberá obtener autorización para permitir el acceso a Internet desde WSUS a la siguiente lista de direcciones URL:

- http://windowsupdate.microsoft.com

- http://*.windowsupdate.microsoft.com

- https://*.windowsupdate.microsoft.com

- http://*.update.microsoft.com

- https://*.update.microsoft.com

- http://*.windowsupdate.com

- http://download.windowsupdate.com

- https://download.microsoft.com

- http://*.download.windowsupdate.com

- http://wustat.windows.com

- http://ntservicepack.microsoft.com

- http://go.microsoft.com

- http://dl.delivery.mp.microsoft.com

- https://dl.delivery.mp.microsoft.com

> [!IMPORTANT]
> Para ver un escenario en el que WSUS no puede obtener actualizaciones debido a las configuraciones de firewall, consulte el [artículo 885819](https://support.microsoft.com/kb/885819) en Microsoft Knowledge base.

En la siguiente sección se describe cómo configurar un firewall corporativo entre WSUS e Internet. Dado que WSUS inicia todo el tráfico de red, no es necesario configurar Firewall de Windows en el servidor WSUS. Si bien la conexión entre Microsoft Update y WSUS requiere que los puertos 80 y 443 estén abiertos, se pueden configurar varios servidores WSUS para que se sincronicen con un puerto personalizado.

### <a name="212-connection-between-wsus-servers"></a>2.1.2. Conexión entre servidores WSUS
Los servidores WSUS que preceden y siguen en la cadena se sincronizarán en el puerto que configure el administrador de WSUS. De forma predeterminada, estos puertos se configuran así:

-   En WSUS 3.2 y versiones anteriores, el puerto 80 para HTTP y el 443 para HTTPS.

-   En WSUS 6,2 y versiones posteriores (al menos Windows Server 2012), se usa el puerto 8530 para HTTP y 8531 para HTTPS.

El firewall del servidor WSUS debe configurarse para permitir tráfico entrante en estos puertos.

### <a name="213-connection-between-clients-windows-update-agent-and-wsus-servers"></a>2.1.3. Conexión entre clientes (Agente de Windows Update) y servidores WSUS
Los puertos e interfaces de escucha se configuran en los sitios de IIS para WSUS y en las configuraciones de directivas de grupo que se usan para configurar los equipos cliente. Los puertos predeterminados son los mismos que los que se especifican en la sección anterior, **Conexión entre servidores WSUS**, y el firewall del servidor WSUS también debe configurarse para permitir el tráfico entrante en esos puertos.

## <a name="configure-the-proxy-server"></a>Configuración del servidor proxy
Si la red corporativa usa servidores proxy, estos deben admitir protocolos HTTP y SSL y utilizar la autenticación básica o autenticación de Windows. Estos requisitos pueden cumplirse mediante una de las siguientes configuraciones:

1.  Un servidor proxy único que admita dos canales de protocolos. En este caso, establezca que un canal use HTTP y el otro HTTPS.

    > [!NOTE]
    > Puede configurar un servidor proxy que controle los dos protocolos de WSUS durante la instalación del software del servidor WSUS.

2.  Dos servidores proxy, cada uno de los cuales admite un único protocolo. En este caso, un servidor proxy se configura para que use HTTP y el otro para HTTPS.

Para configurar dos servidores proxy, cada uno de los cuales controlará un protocolo para WSUS, aplique el procedimiento siguiente:

#### <a name="to-set-up-wsus-to-use-two-proxy-servers"></a>Configuración de WSUS para usar dos servidores proxy

1.  Inicie sesión en el equipo que actuará como servidor WSUS con una cuenta que sea miembro del grupo de administradores locales.

2.  Instalación del rol de servidor WSUS Durante el Asistente para configuración de WSUS (que se explica en la sección siguiente), no especifique un servidor proxy.

3.  Abra un símbolo del sistema (Cmd.exe) como administrador. Para abrir un símbolo del sistema como administrador, vaya a **Inicio**. En **Iniciar búsqueda**, escriba **símbolo del sistema**. en la parte superior del menú Inicio, haga clic con el botón secundario en **símbolo del sistema**y, a continuación, haga clic en **Ejecutar como administrador**. Si aparece el cuadro de diálogo **control de cuentas de usuario** , escriba las credenciales adecuadas (si se solicitan), confirme que la acción que se muestra es la que desea y, a continuación, haga clic en **continuar**.

4.  En la ventana del símbolo del sistema, vaya a la carpeta C:\Archivos de Programa\update Services\Tools Escriba el siguiente comando:

    **WSUSUTIL ConfigureSSlproxy [< proxy_server proxy_port >]-enable**, donde:

    1.  proxy_server es el nombre del servidor proxy que admite HTTPS.

    2.  proxy_port es el número del puerto del servidor proxy.

5.  Cierre la ventana del símbolo del sistema.

Para agregar el servidor proxy que usa el protocolo HTTP para la configuración de WSUS, siga el procedimiento que se muestra a continuación:

#### <a name="to-add-a-proxy-server-that-uses-the-http-protocol"></a>Adición de un servidor proxy que usa el protocolo HTTP

1.  Abre la consola de administración de WSUS.

2.  En el panel izquierdo, expanda el nombre del servidor y haga clic en **Opciones**.

3.  En el panel **Opciones**, haga clic en **Actualizar origen y servidor** y luego haga clic en la pestaña **Servidor proxy**.

4.  Use las siguientes opciones para modificar la configuración del servidor proxy existente:

    ###### <a name="to-change-or-add-a-proxy-server-to-the-wsus-configuration"></a>Cambio o adición de un servidor proxy a la configuración de WSUS

    1.  Seleccione la casilla **Usar un servidor proxy al sincronizarse**.

    2.  En el cuadro de texto **Nombre del servidor proxy**, escriba el nombre del servidor proxy.

    3.  En el cuadro de texto **Número del puerto del proxy**, escriba el número del puerto del servidor proxy. El número de puerto predeterminado es 80.

    4.  FF el servidor proxy requiere que use una cuenta de usuario específica, active la casilla **usar credenciales de usuario para conectarse al servidor proxy** . Escriba el nombre de usuario, el dominio y la contraseña necesarios en los cuadros de texto correspondientes.

    5.  Si el servidor proxy admite la autenticación básica, active la casilla **Permitir la autenticación básica (la contraseña se envía en texto no cifrado)** .

    6.  Haga clic en **Aceptar**.

    ###### <a name="to-remove-a-proxy-server-from-the-wsus-configuration"></a>Eliminación de un servidor proxy de la configuración de WSUS

    1.  Para quitar un servidor proxy de la configuración de WSUS, desactive la casilla **Usar un servidor proxy al sincronizarse**.

    2.  Haga clic en **Aceptar**.

## <a name="22-configure-wsus-by-using-the-wsus-configuration-wizard"></a>2.2. Configurar WSUS con el Asistente para la configuración de WSUS
En este procedimiento, se supone que está usando el Asistente para la configuración de WSUS, que aparece la primera vez que inicia la Consola de administración de WSUS. Más adelante en este tema, obtendrá información acerca de cómo configurar estos parámetros mediante la página **Opciones** :

#### <a name="to-configure-wsus"></a>Para configurar WSUS

1.  En el panel de navegación del Administrador del servidor, haga clic en **Panel**, en **Herramientas** y, a continuación, en **Windows Server Update Services**.

    > [!NOTE]
    > Si aparece el cuadro de diálogo **completar instalación de WSUS** , haga clic en **Ejecutar**. En el cuadro de diálogo **completar instalación de WSUS** , haga clic en **cerrar** cuando finalice correctamente la instalación.

2.  Se abrirá el Asistente para Windows Server Update Services. En la página **Antes de comenzar** , revise la información y haga clic en **Siguiente**.

3.  Lea las instrucciones en la página **Unirse al programa de mejora de Microsoft Update** y determine si desea participar. Si quiere participar en el programa. Conserve la selección predeterminada, o desactive la casilla y, a continuación, haga clic en **siguiente**.

4.  En la página **Elegir servidor que precede en la cadena** , hay dos opciones:

    1.  Sincronizar las actualizaciones con Microsoft Update.

    2.  Sincronizar desde otro servidor de Windows Server Update Services.

        -   Si decide sincronizar desde otro servidor WSUS, especifique el nombre del servidor y el puerto en el que este servidor se comunicará con el servidor que precede en la cadena.

        -   Para usar SSL, active la casilla **Usar SSL al sincronizar la información de actualización** . Los servidores usarán el puerto 443 para la sincronización. (Asegúrese de que este servidor y el servidor que precede en la cadena sean compatibles con SSL).

        -   Si se trata de un servidor de réplicas, active la casilla **esta es una réplica del servidor que precede en la cadena** .

5.  Una vez que seleccione las opciones apropiadas para la implementación, haga clic en **Siguiente** para continuar.

6.  En la página **Especificar servidor proxy** , active la casilla **Usar un servidor proxy al sincronizar** y, a continuación, escriba el nombre del servidor proxy y el número del puerto (puerto 80, de manera predeterminada) en los cuadros correspondientes.

    > [!IMPORTANT]
    > Debe completar este paso si observa que WSUS necesita un servidor proxy para obtener acceso a Internet.

7.  Si desea conectarse al servidor proxy con credenciales de usuario específicas, active la casilla **usar credenciales de usuario para conectarse al servidor proxy** y, a continuación, escriba el nombre de usuario, el dominio y la contraseña del usuario en los cuadros correspondientes. Si desea habilitar la autenticación básica para el usuario que se está conectando al servidor proxy, active la casilla **permitir autenticación básica (la contraseña se envía en texto no cifrado)** .

8.  Haga clic en **Next**. En la página **conectar al servidor que precede** en la cadena, haga clic en **iniciar conexión**.

9. Cuando se conecte, haga clic en **Siguiente** para continuar.

10. En la página **elegir idiomas** , tiene la opción de seleccionar los idiomas en los que WSUS recibirá actualizaciones: todos los idiomas o un subconjunto de idiomas. al seleccionar un subconjunto de idiomas, se ahorra espacio en disco, pero es importante elegir todos los idiomas que necesitan todos los clientes de este servidor WSUS. Si elige obtener actualizaciones solo para idiomas específicos, seleccione **Descargar actualizaciones solo en estos idiomas**y, a continuación, seleccione los idiomas para los que desea actualizaciones. de lo contrario, deje la selección predeterminada.

    > [!WARNING]
    > Si selecciona **Descargar actualizaciones solo en estos idiomas** y este servidor tiene un servidor WSUS que sigue en la cadena, esta opción forzará al servidor que sigue en la cadena a usar los idiomas seleccionados.

11. Una vez que seleccione las opciones de idioma apropiadas para la implementación, haga clic en **Siguiente** para continuar.

12. La página **Elegir productos** le permite especificar los productos para los que desea actualizaciones. Seleccione las categorías de productos, como Windows, o productos específicos, como Windows Server 2008. al seleccionar una categoría de producto, se seleccionan todos los productos de esa categoría.

13. Seleccione las opciones de producto apropiadas para la implementación y, a continuación, haga clic en **siguiente**.

14. En la página **Elegir clasificaciones** , seleccione las clasificaciones de actualización que desea obtener. Elija todas las clasificaciones o un subconjunto de ellas y haga clic en **Siguiente**.

15. En la página **Establecer una programación de sincronización** se permite seleccionar si se debe realizar la sincronización de forma manual o automática.

    -   Si elige **sincronizar manualmente**, debe iniciar el proceso de sincronización desde la consola de administración de WSUS.

    -   Si elige **sincronizar automáticamente**, el servidor WSUS se sincronizará a intervalos establecidos.

    Establezca la hora de la **Primera sincronización**y especifique el número de **Sincronizaciones por día** que quiere que realice este servidor. Por ejemplo, si especifica que deberán realizarse cuatro sincronizaciones por día a partir de las 03:00 a.m., las sincronizaciones se producirán a las 03:00 a.m., 09:00 a.m., 03:00 p.m. y 09:00 p.m.

16. Una vez que seleccione las opciones de sincronización apropiadas para la implementación, haga clic en **Siguiente** para continuar.

17. En la página **Finalizado**, tiene la opción de iniciar la sincronización ahora al activar la casilla **Iniciar sincronización inicial**. Si no selecciona esta opción, deberá usar la consola de administración de WSUS para realizar la sincronización inicial. Haga clic en **Siguiente** , si desea leer más información sobre parámetros adicionales de configuración o haga clic en **Finalizar** , para concluir el asistente y finalizar la configuración inicial de WSUS.

18. Después de que haga clic en **Finalizar**, aparece la Consola de administración de WSUS.

Ahora que ya ha establecido la configuración básica de WSUS, lea las siguientes secciones para obtener más detalles sobre cómo cambiar la configuración mediante la Consola de administración de WSUS.

## <a name="23-configure-wsus-computer-groups"></a>2.3. Configurar grupos de equipos de WSUS
los grupos de equipos son una parte importante de las implementaciones de Windows Server Update Services (WSUS). Los grupos de equipos le permiten probar y dirigir actualizaciones a equipos específicos. Hay dos grupos de equipos predeterminados: Todos los equipos y equipos sin asignar. De manera predeterminada, cuando cada equipo cliente se comunica por primera vez con el servidor WSUS, el servidor agrega ese equipo cliente en ambos grupos.

Puede crear tantos grupos de equipos personalizados como desee para administrar actualizaciones en la organización. Como procedimiento recomendado, cree al menos un grupo de equipos para probar actualizaciones antes de implementarlas en otros equipos de la organización.

Use el siguiente procedimiento para crear un grupo nuevo y asignar un equipo a este grupo:

#### <a name="to-create-a-computer-group"></a>Para crear un grupo de equipos

1.  En la consola de administración de WSUS, en **servicios de actualización**, expanda el servidor WSUS, expanda **equipos**, haga clic con el botón secundario en **todos los equipos**y, a continuación, haga clic en **Agregar grupo de equipos**.

2.  En el cuadro de diálogo **Agregar grupo de equipos** , en **nombre**, especifique el nombre del nuevo grupo y haga clic en **Agregar**.

3.  Haga clic en **equipos**y, a continuación, seleccione los equipos que desea asignar a este grupo nuevo.

4.  Haga clic con el botón secundario en los nombres de equipo que seleccionó en el paso anterior y, a continuación, haga clic en **cambiar pertenencia**.

5.  En el cuadro de diálogo **establecer pertenencia a grupo de equipos** , seleccione el grupo de prueba que creó y, a continuación, haga clic en **Aceptar**.

## <a name="24-configure-client-updates"></a>2.4. Configurar actualizaciones de cliente
El programa de instalación de WSUS configura ISS de forma automática para que se distribuya la versión más reciente del servicio Actualizaciones automáticas a cada equipo cliente que se contacte con el servidor WSUS. La mejor manera de configurar Actualizaciones automáticas depende del entorno de red.

-   En un entorno que utiliza el servicio de directorio de Active Directory, puede usar un objeto de directiva de grupo basado en dominio (GPO) existente o crear un nuevo GPO.

-   En un entorno sin Active Directory, use el editor de directiva de grupo local para configurar Actualizaciones automáticas y, a continuación, señale los equipos cliente al servidor WSUS.

> [!IMPORTANT]
> Los procedimientos siguientes suponen que la red ejecuta Active Directory. Además se supone que usted está familiarizado con la directiva de grupo y que la usa para administrar la red.

Use los siguientes procedimientos para configurar Actualizaciones automáticas para equipos cliente:

-   [Paso 4: Configurar la directiva de grupo para las actualizaciones automáticas](4-configure-group-policy-settings-for-automatic-updates.md)

-   [2,3. Configurar grupos](#23-configure-wsus-computer-groups) de equipos en este tema

### <a name="configure-automatic-updates-in-group-policy"></a>Configurar Actualizaciones automáticas en la directiva de grupo

Si ha configurado Active Directory en la red, puede configurar uno o varios equipos al mismo tiempo incluyéndolos en un objeto de directiva de grupo (GPO) y, a continuación, configurando ese GPO con la configuración de WSUS. Se recomienda que cree un GPO nuevo, que contenga solo la configuración de WSUS.

Vincule este GPO de WSUS a un contenedor de Active Directory que sea adecuado para su entorno. En un entorno simple, podría unir un GPO de WSUS único al dominio. En un entorno complejo, podría vincular varios GPO de WSUS a varias unidades organizativas (OU), lo cual permite aplicar parámetros de configuración de directiva de WSUS diferentes en distintos tipos de equipos.

##### <a name="to-enable-wsus-through-a-domain-gpo"></a>Para habilitar WSUS en un GPO de dominio

1.  En el Consola de administración de directivas de grupo (GPMC), busque el GPO en el que desea configurar WSUS y, a continuación, haga clic en **Editar**.

2.  En GPMC, expanda **configuración del equipo**, expanda **directivas**, expanda **plantillas administrativas**, expanda **componentes de Windows**y, a continuación, haga clic en **Windows Update**.

3.  En el panel de detalles, haga doble clic en **Configurar actualizaciones automáticas**. Se abrirá la directiva **Configurar actualizaciones automáticas** .

4.  Haga clic en **Habilitada** y, después, seleccione una de las siguientes opciones del parámetro **Configurar actualización automática**:

    -   **Notificar descarga y notificar instalación**. Esta opción notifica al usuario administrativo con sesión iniciada sobre las actualizaciones, antes de que se descarguen e instalen.

    -   **Descargar automáticamente y notificar instalación**. Esta opción comienza automáticamente a descargar actualizaciones y luego notifica al usuario administrativo con sesión iniciada antes de instalarlas. Esta opción está seleccionada de forma predeterminada.

    -   **Descargar automáticamente y programar la instalación**. Esta opción comienza automáticamente a descargar actualizaciones y luego las instala el día y a la hora especificados.

    -   **Permitir que el administrador local elija la opción**. Esta opción permite a los administradores locales usar Actualizaciones automáticas en el Panel de control para seleccionar una opción de configuración. Por ejemplo, pueden elegir una hora de instalación programada. Los administradores locales no pueden deshabilitar Actualizaciones automáticas.

5.  Seleccione **Habilitar destinatarios del lado cliente**, seleccione **habilitado**y, a continuación, escriba el nombre del grupo de equipos de WSUS al que desea agregar este equipo en el cuadro **nombre de grupo de destino para este equipo** .

    > [!NOTE]
    > La opción **Habilitar destinatarios del lado cliente** permite que los equipos cliente se agreguen ellos mismos a grupos de equipos de destino en el servidor WSUS, cuando las actualizaciones automáticas se redirigen a un servidor WSUS. Si el estado se establece en habilitado, este equipo se identificará como miembro de un grupo de equipos concreto cuando envíe información al servidor WSUS, que lo utilizará para determinar qué actualizaciones se han implementado en este equipo. Este valor indica al servidor WSUS el grupo que usará el equipo cliente. Debe crear el grupo en el servidor WSUS y agregar equipos miembros del dominio a ese grupo.

6.  Haga clic en **Aceptar** para cerrar la directiva **Habilitar destinatarios del lado cliente** y volver al panel de detalles de Windows Update.

7.  Haga clic en **Aceptar** para cerrar la directiva **Configurar actualizaciones automáticas** y volver al panel de detalles de Windows Update.

8.  En el panel de detalles de **Windows Update** , haga doble clic en **Especificar la ubicación del servicio Windows Update en la intranet**.

9. Haga clic en **Habilitado** y escriba la misma dirección URL del servidor WSUS en los cuadros de texto **Establecer el servicio de actualización de la intranet para detectar actualizaciones** y **Establecer el servidor de estadísticas de la intranet**. Por ejemplo, escriba *http://servername* en ambos cuadros (donde *ServerName* es el nombre del servidor WSUS).

    > [!WARNING]
    > Cuando escriba la dirección de intranet de su servidor WSUS, asegúrese de especificar el puerto que se va a usar. De manera predeterminada, WSUS usará el puerto 8530 para HTTP y 8531 para HTTPS. Por ejemplo, si usa HTTP, debe escribir **http://servername:8530** .

10. Haga clic en **Aceptar**.

Después de configurar un equipo cliente, el equipo tardará varios minutos en aparecer en la página **equipos** de la consola de administración de WSUS. Con equipos cliente configurados con un objeto de directiva de grupo basado en dominio, pueden transcurrir alrededor de 20 minutos para que la directiva de grupo se aplique a la nueva configuración del equipo cliente. De forma predeterminada, directiva de grupo actualiza en segundo plano cada 90 minutos, con un desplazamiento aleatorio de 0-30 minutos. Si desea actualizar directiva de grupo antes, puede abrir una ventana del símbolo del sistema en el equipo cliente y escribir gpupdate/force.

en el caso de los equipos cliente que se configuran mediante el editor de directiva de grupo local, el GPO se aplica inmediatamente y la actualización tarda aproximadamente 20 minutos. Si comienza la detección de forma manual, no tiene que esperar 20 minutos para que el equipo cliente se ponga en contacto con WSUS.

Dado que la espera de la detección a veces requiere bastante tiempo, con el siguiente procedimiento puede iniciar la detección de inmediato.

##### <a name="to-initiate-wsus-detection"></a>Para iniciar la detección de WSUS

1.  En el equipo cliente, abra una ventana del símbolo del sistema con privilegios elevados.

2.  Escriba wuauclt. exe/detectnow y, a continuación, presione Entrar.

## <a name="25-secure-wsus-with-the-secure-sockets-layer-protocol"></a>2.5. Proteger WSUS con el protocolo de Capa de sockets seguros

Puede usar el protocolo de Capa de sockets seguros (SSL) para ayudar a proteger la implementación de WSUS. WSUS utiliza SSL para autenticar en el servidor WSUS los equipos cliente y los servidores WSUS que siguen en la cadena. WSUS también usa SSL para cifrar los metadatos de actualización.

> [!IMPORTANT]
> Los clientes y los servidores que siguen en la cadena que están configurados para usar Seguridad de la capa de transporte (TLS) o HTTPS también deben configurarse para utilizar un nombre de dominio completo (FQDN) para su servidor WSUS que precede en la cadena.

WSUS usa SSL solo para los metadatos, no para los archivos de actualización. Se trata de la misma manera en que Microsoft Update distribuye actualizaciones. Microsoft reduce el riesgo de enviar archivos de actualización a través de un canal no cifrado mediante la firma de cada actualización. Además, se calcula un hash y se envía junto con los metadatos de cada actualización. Cuando se descarga una actualización, WSUS comprueba la firma digital y el hash. Si la actualización ha cambiado, no se instala.

### <a name="limitations-of-wsus-ssl-deployments"></a>Limitaciones de las implementaciones SSL de WSUS
Debe tener en cuenta las siguientes limitaciones cuando use SSL para proteger una implementación de WSUS:

1.  El uso de SSL aumenta la carga de trabajo del servidor. Debe esperar una pérdida de rendimiento del 10 por ciento, debido al costo de cifrar todos los metadatos que se envían a través de la red.

2.  Si usa WSUS con una base de datos de SQL Server remota, la conexión entre el servidor WSUS y el servidor de la base de datos no está protegida con SSL. Si la conexión de base de datos debe estar protegida, tenga en cuenta las siguientes recomendaciones:

-   Mueva la base de datos de WSUS al servidor WSUS.

-   Mueva el servidor de base de datos remoto y el servidor WSUS a una red privada.

-   Implemente el protocolo de seguridad de Internet (IPsec) para ayudar a proteger el tráfico de red. Para más información sobre IPsec, consulte [Creación y uso de directivas de IPsec](https://go.microsoft.com/fwlink/?LinkID=203841).

### <a name="configure-ssl-on-the-wsus-server"></a>Configurar SSL en el servidor WSUS
WSUS requiere dos puertos para SSL: un puerto que use HTTPS para enviar metadatos cifrados y otro que utilice HTTP para enviar las actualizaciones. Al configurar WSUS para que use SSL, tenga en cuenta lo siguiente:

-   No se puede configurar que el sitio web de WSUS completo requiera SSL porque entonces debería cifrarse todo el tráfico al sitio de WSUS. WSUS solo cifra los metadatos de actualización. Si un equipo intenta recuperar los archivos de actualización en el Puerto HTTPS, se producirá un error en la transferencia.

    Debe requerir SSL solo para las raíces virtuales siguientes:

    -   **SimpleAuthWebService**

    -   **DSSAuthWebService**

    -   **ServerSyncWebService**

    -   **APIremoting30**

    -   **ClientWebService**

    No debe requerir SSL para las raíces virtuales siguientes:

    -   **Contenido**

    -   **Stock**

    -   **ReportingWebService**

    -   **SelfUpdate**

-   El certificado de la entidad de certificación (CA) debe importarse en el almacén de CA raíz de confianza del equipo local o el de Windows Server Update Service en los servidores WSUS que siguen en la cadena. Si el certificado solo se importa en el almacén de CA raíz de confianza del usuario local, el servidor WSUS que sigue en la cadena no se autenticará en el servidor que precede en la cadena.

    para obtener más información sobre cómo usar los certificados SSL en IIS, vea [requerir capa de sockets seguros (IIS 7)](https://go.microsoft.com/fwlink/?LinkID=203846).

-   Debe importar el certificado en todos los equipos que se comunicarán con el servidor WSUS. Se incluyen todos los equipos cliente, servidores que siguen en la cadena y equipos que ejecutan la consola de administración de WSUS. El certificado debe importarse en el almacén de CA raíz de confianza del equipo local o el de Windows Server Update Service.

-   Puede usar cualquier puerto para SSL. Sin embargo, el puerto que configure para SSL también determina el puerto que WSUS usa para enviar el tráfico HTTP sin cifrar. Revisa los siguientes ejemplos:

    -   Si utiliza el puerto estándar del sector de 443 para el tráfico HTTPS, WSUS usa el puerto estándar del sector 80 para el tráfico HTTP sin cifrar.

    -   Si usa un puerto distinto de 443 para el tráfico HTTPS, WSUS enviará tráfico HTTP sin cifrar a través del puerto que se incluye numéricamente antes del puerto para HTTPS. Por ejemplo, si utiliza el puerto 8531 para HTTPS, WSUS usará el puerto 8530 para HTTP.

-   Debe volver a inicializar *ClientServicingProxy* si se cambia el nombre del servidor, la configuración de SSL o el número del puerto.

##### <a name="to-configure-ssl-on-the-wsus-root-server"></a>Configuración de SSL en el servidor raíz WSUS

1.  Inicie sesión en el servidor WSUS con una cuenta que sea miembro de los grupos de administradores o administradores locales de WSUS.

2.  Vaya a **Inicio**, escriba **cmd**, haga clic con el botón secundario en **símbolo del sistema**y, a continuación, haga clic en **Ejecutar como administrador**.

3.  Navegue hasta la *%ProgramFiles%* **\Update Services\Tools\\* * carpeta.

4.  En la ventana del símbolo del sistema, escriba el siguiente comando:

    **WSUSUTIL configuressl** *certificateName*

    Donde:

    *nombreCertificado* es el nombre DNS del servidor WSUS.

### <a name="configure-ssl-on-client-computers"></a>Configurar SSL en equipos cliente
Al configurar SSL en los equipos cliente, debe considerar lo siguiente:

-   Debe incluir la dirección URL de un puerto seguro en el servidor WSUS. Dado que no se requiere SSL en el servidor, la única forma de asegurarse de que los equipos cliente pueden usar un canal de seguridad es mediante una dirección URL que especifique HTTPS. Si usa un puerto distinto de 443 para SSL, también debe incluir ese puerto en la dirección URL.

-   El certificado de un equipo cliente debe importarse en el almacén de CA raíz de confianza del equipo local o en el almacén de CA raíz de confianza del servicio de actualización automática. Si el certificado se importa solo en el almacén de CA raíz de confianza del usuario local, Actualizaciones automáticas producirá un error en la autenticación del servidor.

-   Los equipos cliente deben confiar en el certificado que se enlace al servidor WSUS. Según el tipo de certificado que se use, podría tener que configurar un servicio para permitir que los equipos cliente confíen en el certificado que está enlazado al servidor WSUS.

### <a name="configure-ssl-for-downstream-wsus-servers"></a>Configurar SSL para servidores WSUS que siguen en la cadena
Las instrucciones siguientes configuran un servidor que sigue en la cadena para que se sincronice con uno que precede en la cadena y usa SSL.

##### <a name="to-synchronize-a-downstream-server-to-an-upstream-server-that-uses-ssl"></a>Sincronización de un servidor que sigue en la cadena a uno que precede en la cadena y usa SSL

1.  Inicie sesión en el equipo con una cuenta de usuario que sea miembro de los grupos de administradores o administradores locales de WSUS.

2.  Haga clic en **Inicio**, en **todos los programas**, en **herramientas administrativas**y, a continuación, en **Windows Server Update Service**.

3.  En el panel derecho, expanda el nombre del servidor.

4.  Haga clic en **Opciones**y después en **Actualizar origen y servidor proxy**.

5.  En la página **Actualizar origen** , seleccione **Sincronizar desde otro servidor de Windows Server Update Services**.

6.  Escriba el nombre del servidor que precede en la cadena en el cuadro de texto **nombre del servidor** . Escriba el número de puerto que el servidor usa para las conexiones SSL en el cuadro de texto **número de Puerto** .

7.  Active la casilla **usar SSL al sincronizar la información de actualización** y, a continuación, haga clic en **Aceptar**.

### <a name="additional-ssl-resources"></a>recursos SSL adicionales
Los pasos necesarios para configurar una entidad de certificación, enlazar el certificado al sitio web de WSUS y establecer una confianza entre los equipos cliente y el certificado están fuera del ámbito de esta guía. Para más información y para instrucciones sobre cómo instalar certificados y configurar este entorno, consulte los temas siguientes:

-   [Guía paso a paso de Suite B PKI](https://go.microsoft.com/fwlink/?LinkID=203858)

-   [Implementación y administración de plantillas de certificado](https://go.microsoft.com/fwlink/?LinkID=203859)

-   [Guía de migración y actualización de servicios de certificados de Active Directory](https://go.microsoft.com/fwlink/?LinkID=203860)

-   [Configurar la inscripción automática de certificados](https://go.microsoft.com/fwlink/?LinkID=203861)

### <a name="26-complete-iis-configuration"></a>2.6. Completar la configuración de IIS
De forma predeterminada, el acceso de lectura anónimo está habilitado en los sitios web de IIS predeterminados y en todos los nuevos. Algunas aplicaciones, especialmente Windows SharePoint Services, pueden quitar el acceso anónimo. Si esto se ha producido, debe volver a habilitar el acceso de lectura anónimo antes de poder instalar y operar correctamente WSUS.

Para habilitar el acceso de lectura anónimo, siga los pasos de la versión concreta de IIS:

1.  [Habilitar la autenticación anónima (IIS 7)](https://go.microsoft.com/fwlink/?LinkID=205316), como se documenta en la Guía de operaciones de IIS 7.

2.  [Habilitación de la autenticación anónima (IIS 6.0)](https://go.microsoft.com/fwlink/?LinkId=211391), como se documenta en la Guía de operaciones de IIS 6.0.

### <a name="27-configure-a-signing-certificate"></a>2.7. Configurar un certificado de firma
WSUS tiene la capacidad de publicar paquetes de actualización personalizados para actualizar productos de Microsoft y de otros desarrolladores. WSUS puede firmar automáticamente estos paquetes de actualización con un certificado Authenticode. Para habilitar la firma de actualización personalizada, debe instalar un certificado de firma de paquetes en el servidor WSUS. Hay varias consideraciones que debe tener en cuenta relacionados con la firma de actualización personalizada.

1.  **Distribución de certificados**. La clave privada debe instalarse en el servidor WSUS y la clave pública debe instalarse de forma explícita en el almacén de certificados de confianza en todos los servidores y equipos cliente que recibirán las actualizaciones de firma personalizada.

2.  **Expiración**. Cuando el certificado autofirmado expire o esté próximo a hacerlo, WSUS registrará los eventos en el registro de eventos.

3.  **Actualizaciones y revocación de certificados**. Si desea actualizar o revocar un certificado (es decir, después de detectar que expiró), WSUS no ofrecía ninguna funcionalidad para habilitarlo. Para realizar esto, era necesario realizar una tarea manual que era muy complicada de hacer a mano o automatizar correctamente.


