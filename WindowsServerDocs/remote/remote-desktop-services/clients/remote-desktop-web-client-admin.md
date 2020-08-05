---
title: Configuración del cliente web de Escritorio remoto para los usuarios
description: Describe cómo un administrador puede configurar el cliente web de Escritorio remoto.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 09/19/2019
ms.topic: article
author: Heidilohr
ms.localizationpriority: medium
ms.openlocfilehash: 12ea9226a1656c6b3c800517818e5e950d142c64
ms.sourcegitcommit: e86ea69254e2f63eaab10010ae3a43622156ab23
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/31/2020
ms.locfileid: "87470698"
---
# <a name="set-up-the-remote-desktop-web-client-for-your-users"></a>Configuración del cliente web de Escritorio remoto para los usuarios

El cliente web de Escritorio remoto permite a los usuarios acceder a la infraestructura de Escritorio remoto de la organización mediante un explorador web compatible. Podrán interactuar con aplicaciones o escritorios remotos como lo harían con un equipo local sin importar dónde se encuentren. Una vez configurado el cliente web de Escritorio remoto, lo único que necesitan los usuarios para empezar es la dirección URL en la que pueden acceder al cliente, sus credenciales y un explorador web compatible.

>[!IMPORTANT]
>El cliente web admite el uso del proxy de aplicación de Azure AD pero no admite en absoluto el Proxy de aplicación web. Consulta [Uso de Servicios de Escritorio remoto con los servicios de proxy de la aplicación](../rds-supported-config.md#using-remote-desktop-services-with-application-proxy-services) para más información.

## <a name="what-youll-need-to-set-up-the-web-client"></a>Lo que necesitas para configurar el cliente web

Antes de comenzar, ten en cuenta lo siguiente:

* Asegúrate de que tu [implementación de Escritorio remoto](../rds-deploy-infrastructure.md) tiene los roles Puerta de enlace de Escritorio remoto, Agente de conexión a Escritorio remoto y Acceso web de Escritorio remoto que se ejecutan en Windows Server 2016 o 2019.
* Asegúrate de que tu implementación esté configurada para [licencias de acceso de cliente por usuario](../rds-client-access-license.md) (CAL) en lugar de por dispositivo; de lo contrario, se consumirán todas las licencias.
* Instala la [actualización KB4025334 de Windows 10](https://support.microsoft.com/help/4025334/windows-10-update-kb4025334) en la puerta de enlace de Escritorio remoto. Es posible que las actualizaciones acumulativas posteriores ya contengan esta KB.
* Asegúrate de que se configuran los certificados de confianza pública para los roles Puerta de enlace de Escritorio remoto y Acceso web de Escritorio remoto.
* Asegúrate de que todos los equipos a los que se conecten tus usuarios estén ejecutando una de las versiones más recientes del sistema operativo:
  * Windows 10
  * Windows Server 2008 R2 o posterior

Los usuarios verán un mejor rendimiento al conectarse a Windows Server 2016 (o posterior) y Windows 10 (versión 1611 o posterior).

>[!IMPORTANT]
>Si has utilizado el cliente web durante el período de versión preliminar y has instalado una versión anterior a la 1.0.0, primero debes desinstalar el cliente anterior antes de pasar a la nueva versión. Si recibes el error "The web client was installed using an older version of RDWebClientManagement and must first be removed before deploying the new version" (El cliente web se instaló utilizando una versión anterior de RDWebClientManagement y se debe quitar antes de implementar la nueva versión), sigue estos pasos:
>
>1. Abre un símbolo del sistema de PowerShell con privilegios elevados.
>2. Ejecuta **Uninstall-Module RDWebClientManagement** para desinstalar el módulo nuevo.
>3. Cierra y vuelve a abrir el símbolo del sistema de PowerShell con privilegios elevados.
>4. Ejecute **Install-Module RDWebClientManagement -RequiredVersion \<old version> para instalar el módulo anterior.**
>5. Ejecuta **Uninstall-RDWebClient** para desinstalar el cliente web anterior.
>6. Ejecuta **Uninstall-Module RDWebClientManagement** para desinstalar el módulo anterior.
>7. Cierra y vuelve a abrir el símbolo del sistema de PowerShell con privilegios elevados.
>8. Sigue con los pasos normales de instalación como se indica a continuación.

## <a name="how-to-publish-the-remote-desktop-web-client"></a>Publicación del cliente web de Escritorio remoto

Para instalar al cliente web por primera vez, sigue estos pasos:

1. En el servidor del Agente de conexión a Escritorio remoto, obtén el certificado usado para las conexiones de Escritorio remoto y expórtalo como archivo .cer. Copia el archivo .cer desde el Agente de conexión a Escritorio remoto en el servidor que ejecuta el rol Acceso web de Escritorio remoto.
2. En el servidor de acceso web de RD, abre un símbolo del sistema de PowerShell con privilegios elevados.
3. En Windows Server 2016, actualiza el módulo PowerShellGet ya que la versión incluida no admite la instalación del módulo de administración de clientes web. Para actualizar PowerShellGet, ejecuta el siguiente cmdlet:
    ```PowerShell
    Install-Module -Name PowerShellGet -Force
    ```

    >[!IMPORTANT]
    >Deberás reiniciar PowerShell antes de que la actualización surta efecto; de lo contrario, es posible que el módulo no funcione.

4. Instala el módulo de PowerShell de administración de clientes web de Escritorio remoto desde la galería de PowerShell con este cmdlet:
    ```PowerShell
    Install-Module -Name RDWebClientManagement
    ```

5. Después, ejecuta el siguiente cmdlet para descargar la última versión del cliente web de Escritorio remoto:
    ```PowerShell
    Install-RDWebClientPackage
    ```

6. A continuación, ejecuta este cmdlet con el valor entre corchetes reemplazado por la ruta de acceso del archivo .cer que ha copiado del agente de Escritorio remoto:
    ```PowerShell
    Import-RDWebClientBrokerCert <.cer file path>
    ```

7. Por último, ejecuta este cmdlet para publicar al cliente web de Escritorio remoto:
    ```PowerShell
    Publish-RDWebClientPackage -Type Production -Latest
    ```
    Asegúrate de que se puedes acceder al cliente web en su dirección URL con el nombre del servidor, con el formato <https://server_FQDN/RDWeb/webclient/index.html>. Es importante utilizar el nombre del servidor que coincida con el certificado público de Acceso web de Escritorio remoto en la dirección URL (normalmente el FQDN de servidor).

    >[!NOTE]
    >Al ejecutar el cmdlet **Publish-RDWebClientPackage**, es posible que aparezca una advertencia que indica que las CAL por dispositivo no se admiten, incluso si tu implementación está configurada para CAL por usuario. Si la implementación utiliza CAL por usuario, puedes ignorar esta advertencia. Lo mostramos para asegurarnos de que conoces la limitación de configuración.
8. Cuando estés listo para que los usuarios accedan al cliente web, simplemente envíales la dirección URL del cliente web que has creado.

>[!NOTE]
>Para ver una lista de todos los cmdlets admitidos para el módulo RDWebClientManagement, ejecuta el siguiente cmdlet de PowerShell:
>```PowerShell
>Get-Command -Module RDWebClientManagement
>```

## <a name="how-to-update-the-remote-desktop-web-client"></a>Actualización del cliente web de Escritorio remoto

Cuando esté disponible una nueva versión del cliente web de Escritorio remoto, sigue estos pasos para actualizar la implementación con el nuevo cliente:

1. Abre un símbolo del sistema de PowerShell con privilegios elevados en el servidor de Acceso web de Escritorio remoto y ejecuta el siguiente cmdlet para descargar la última versión disponible del cliente web:
    ```PowerShell
    Install-RDWebClientPackage
    ```

2. Opcionalmente, puedes publicar el cliente para probarlo antes de su lanzamiento oficial; para ello, ejecuta este cmdlet:
    ```PowerShell
    Publish-RDWebClientPackage -Type Test -Latest
    ```

    El cliente debe aparecer en la dirección URL de prueba que corresponde a la dirección URL de tu cliente web (por ejemplo, <https://server_FQDN/RDWeb/webclient-test/index.html>).
3. Para publicar el cliente para los usuarios, ejecuta el cmdlet siguiente:
    ```PowerShell
    Publish-RDWebClientPackage -Type Production -Latest
    ```

    Esto reemplazará al cliente para todos los usuarios cuando vuelva a iniciar la página web.

## <a name="how-to-uninstall-the-remote-desktop-web-client"></a>Desinstalación del cliente web de Escritorio remoto

Para quitar todos los seguimientos del cliente web, sigue estos pasos:

1. En el servidor de acceso web de RD, abre un símbolo del sistema de PowerShell con privilegios elevados.
2. Cancela la publicación de los clientes de prueba y producción, desinstala todos los paquetes locales y quita la configuración del cliente web:

   ```PowerShell
   Uninstall-RDWebClient
   ```

3. Desinstala el módulo de PowerShell de administración de clientes web de Escritorio remoto:

   ```PowerShell
   Uninstall-Module -Name RDWebClientManagement
   ```

## <a name="how-to-install-the-remote-desktop-web-client-without-an-internet-connection"></a>Instalación del cliente web de Escritorio remoto sin una conexión a Internet

Sigue estos pasos para implementar el cliente web en un servidor de Acceso web de Escritorio remoto que no tiene una conexión a Internet.

> [!NOTE]
> La instalación sin conexión a Internet está disponible en la versión 1.0.1 y superior del módulo RDWebClientManagement de PowerShell.

> [!NOTE]
> Para poder descargar los archivos necesarios antes de transferirlos al servidor sin conexión, es necesario disponer de un equipo de administración con acceso a Internet.

> [!NOTE]
> El equipo del usuario final necesita una conexión a Internet por ahora. Esto se tratará en una futura versión del cliente para proporcionar un escenario sin conexión completo.

### <a name="from-a-device-with-internet-access"></a>Desde un dispositivo con acceso a Internet

1. Abre un símbolo del sistema de PowerShell.

2. Importa el módulo de PowerShell de administración de clientes web de Escritorio remoto desde la galería de PowerShell:
    ```PowerShell
    Import-Module -Name RDWebClientManagement
    ```

3. Descarga la última versión del cliente web de Escritorio remoto para su instalación en un dispositivo diferente:
    ```PowerShell
    Save-RDWebClientPackage "C:\WebClient\"
    ```

4. Descarga la última versión del módulo RDWebClientManagement de PowerShell:
    ```PowerShell
    Find-Module -Name "RDWebClientManagement" -Repository "PSGallery" | Save-Module -Path "C:\WebClient\"
    ```

5. Copia el contenido de "C:\WebClient\" al servidor de Acceso web de Escritorio remoto.

### <a name="from-the-rd-web-access-server"></a>En el servidor de Acceso web de Escritorio remoto

Sigue las instrucciones de [Publicación del cliente web de Escritorio remoto](remote-desktop-web-client-admin.md#how-to-publish-the-remote-desktop-web-client), reemplazando los pasos 4 y 5 por lo siguiente.

4. Tienes dos opciones para recuperar el módulo de PowerShell de administración de clientes web más reciente:
    - Importar el módulo de PowerShell de administración de clientes web de Escritorio remoto:
      ```PowerShell
      Import-Module -Name RDWebClientManagement
      ```
    - Copiar la carpeta RDWebClientManagement descargada en una de las carpetas locales del módulo de PowerShell que aparecen en **$env:p smodulePath**, o bien agregar la ruta de acceso a la carpeta con los archivos descargados en **$env:psmodulePath**.

5. Implementa la última versión del cliente web de Escritorio remoto desde la carpeta local (reemplázala por el archivo zip correspondiente):
    ```PowerShell
    Install-RDWebClientPackage -Source "C:\WebClient\rdwebclient-1.0.1.zip"
    ```

## <a name="connecting-to-rd-broker-without-rd-gateway-in-windows-server-2019"></a>Conexión al agente de Escritorio remoto sin una puerta de enlace de Escritorio remoto en Windows Server 2019
En esta sección se describe cómo habilitar la conexión de un cliente web a un agente de Escritorio remoto sin una puerta de enlace de Escritorio remoto en Windows Server 2019.

### <a name="setting-up-the-rd-broker-server"></a>Configuración del servidor del agente de Escritorio remoto

#### <a name="follow-these-steps-if-there-is-no-certificate-bound-to-the-rd-broker-server"></a>Sigue estos pasos si no hay ningún certificado enlazado con el servidor del agente de Escritorio remoto

1. Abre **Administrador del servidor** > **Servicios de Escritorio remoto**.

2. En la sección **Información general de implementación**, selecciona el menú desplegable **Tareas**.

3. Selecciona **Editar propiedades de la implementación** y se abre una nueva ventana denominada **Propiedades de implementación**.

4. En la ventana **Propiedades de implementación**, selecciona **Certificados** en el menú de la izquierda.

5. En la lista de niveles de certificados, selecciona **Agente de conexión a Escritorio remoto - Habilitar inicio de sesión único**. Tienes dos opciones: (1) crear un nuevo certificado o (2) un certificado existente.

#### <a name="follow-these-steps-if-there-is-a-certificate-previously-bound-to-the-rd-broker-server"></a>Sigue estos pasos si hay un certificado enlazado previamente con el servidor del agente de Escritorio remoto

1. Abre el certificado enlazado al agente y copia el valor de **Huella digital**.

2. Para enlazar este certificado al puerto seguro 3392, abre una ventana PowerShell con privilegios elevados y ejecuta el siguiente comando, sustituyendo **"< thumbprint >"** por el valor copiado en el paso anterior:

    ```PowerShell
    netsh http add sslcert ipport=0.0.0.0:3392 certhash="<thumbprint>" certstorename="Remote Desktop" appid="{00000000-0000-0000-0000-000000000000}"
    ```

    > [!NOTE]
    > Para comprobar si el certificado se ha enlazado correctamente, ejecuta el siguiente comando:
    >
    > ```PowerShell
    > netsh http show sslcert
    > ```
    >
    > En la lista de los enlaces de certificados SSL, asegúrese de que el certificado correcto está enlazado al puerto 3392.

3. Abre el Registro de Windows (regedit), ve a ```HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp``` y busca la clave **WebSocketURI**. El valor debe establecerse en <strong>https://+:3392/rdp/</strong>.

### <a name="setting-up-the-rd-session-host"></a>Configuración del host de sesión de Escritorio remoto
Si el servidor del host de sesión de Escritorio remoto es diferente del servidor del agente de escritorio remoto, sigue estos pasos:

1. Crea un certificado para la máquina de host de sesión de Escritorio remoto, ábrelo y copia el valor de **Huella digital**.

2. Para enlazar este certificado al puerto seguro 3392, abre una ventana PowerShell con privilegios elevados y ejecuta el siguiente comando, sustituyendo **"< thumbprint >"** por el valor copiado en el paso anterior:

    ```PowerShell
    netsh http add sslcert ipport=0.0.0.0:3392 certhash="<thumbprint>" appid="{00000000-0000-0000-0000-000000000000}"
    ```

    > [!NOTE]
    > Para comprobar si el certificado se ha enlazado correctamente, ejecuta el siguiente comando:
    >
    > ```PowerShell
    > netsh http show sslcert
    > ```
    >
    > En la lista de los enlaces de certificados SSL, asegúrese de que el certificado correcto está enlazado al puerto 3392.

3. Abre el Registro de Windows (regedit), ve a ```HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp``` y busca la clave **WebSocketURI**. El valor debe establecerse en <https://+:3392/rdp/>.

### <a name="general-observations"></a>Observaciones generales

* Asegúrate de que tanto el host de sesión de Escritorio remoto como el servidor del agente de Escritorio remoto ejecutan Windows Server 2019.

* Asegúrate de que los certificados públicos de confianza están configurados tanto para el host de sesión de Escritorio remoto como para el servidor del agente de Escritorio remoto.
    > [!NOTE]
    > Si el host de sesión de Escritorio remoto y el servidor del agente de Escritorio remoto comparten el mismo equipo, establece solo el certificado de servidor del agente de Escritorio remoto. Si el host de sesión de Escritorio remoto y el servidor del agente de Escritorio remoto utiliza equipos diferentes, debes configurarlos con certificados únicos.

* Debe establecerse el **nombre alternativo del firmante (SAN)** de cada certificado en el **nombre de dominio completo (FQDN)** de la máquina. El **nombre común (CN)** debe coincidir con el nombre alternativo del firmante de cada certificado.

## <a name="how-to-pre-configure-settings-for-remote-desktop-web-client-users"></a>Configuración previa para usuarios del cliente web de Escritorio remoto
En esta sección se te indicará cómo usar PowerShell para configurar los ajustes para la implementación del cliente web de Escritorio remoto. Estos cmdlets de PowerShell controlan la capacidad de un usuario para cambiar la configuración en función de las preocupaciones de seguridad de la organización o del flujo de trabajo previsto. La siguiente configuración se encuentra en el panel lateral **Configuraciones** del cliente web.

### <a name="suppress-telemetry"></a>Supresión de la telemetría
De forma predeterminada, los usuarios pueden elegir habilitar o deshabilitar la recopilación de datos de telemetría que se envían a Microsoft. Para más información sobre los datos de telemetría que recopila Microsoft, consulta nuestra declaración de privacidad en el vínculo del panel lateral **Acerca de**.

Como administrador, puedes elegir suprimir la recopilación de datos de telemetría para la implementación mediante el siguiente cmdlet de PowerShell:

   ```PowerShell
    Set-RDWebClientDeploymentSetting -Name "SuppressTelemetry" $true
   ```

De forma predeterminada, el usuario puede seleccionar habilitar o deshabilitar la telemetría. Un valor booleano **$false** coincidirá con el comportamiento del cliente predeterminado. Un valor booleano **$true** deshabilita la telemetría e impide que el usuario la habilite.

### <a name="remote-resource-launch-method"></a>Método de inicio del recurso remoto

>[!NOTE]
>Actualmente, esta configuración solo funciona con el cliente web de RDS y no con el cliente web de Windows Virtual Desktop.

De forma predeterminada, los usuarios pueden elegir iniciar los recursos remotos (1) en el explorador o (2) descargar un archivo .rdp para controlar con otro cliente instalado en el equipo. Como administrador, puedes optar por restringir el método de inicio del recurso remoto para la implementación con el siguiente comando de Powershell:

   ```PowerShell
    Set-RDWebClientDeploymentSetting -Name "LaunchResourceInBrowser" ($true|$false)
   ```
 De forma predeterminada, el usuario puede seleccionar cualquiera de estos métodos de inicio. Un valor booleano **$true** obligará al usuario a iniciar los recursos en el explorador. Un valor booleano **$false** obligará al usuario a iniciar los recursos mediante la descarga de un archivo .rdp para tratarlo con un cliente RDP instalado localmente.

### <a name="reset-rdwebclientdeploymentsetting-configurations-to-default"></a>Restablecimiento de las configuraciones RDWebClientDeploymentSetting a los valores predeterminados

Para restablecer un valor del cliente web en el nivel de implementación a la configuración predeterminada, ejecuta el siguiente cmdlet de PowerShell y utiliza el parámetro -name para especificar el valor que deseas configurar:

   ```PowerShell
    Reset-RDWebClientDeploymentSetting -Name "LaunchResourceInBrowser"
    Reset-RDWebClientDeploymentSetting -Name "SuppressTelemetry"
   ```

## <a name="troubleshooting"></a>Solucionar problemas

Si un usuario informa de cualquiera de los siguientes problemas al abrir el cliente web por primera vez, las siguientes secciones le indicarán qué hacer para solucionarlos.

### <a name="what-to-do-if-the-users-browser-shows-a-security-warning-when-they-try-to-access-the-web-client"></a>Qué hacer si el explorador del usuario muestra una advertencia de seguridad cuando intenta acceder al cliente web

Es posible que el rol Acceso web de Escritorio remoto no utilice un certificado de confianza. Asegúrate de que el rol Acceso web de Escritorio remoto está configurada con un certificado de confianza pública.

Si esto no funciona, es posible que el nombre del servidor en la dirección URL del cliente web no coincida con el nombre proporcionado por el certificado web de Escritorio remoto. Asegúrate de que la dirección URL utiliza el FQDN del servidor que hospeda el rol Acceso web de Escritorio remoto.

### <a name="what-to-do-if-the-user-cant-connect-to-a-resource-with-the-web-client-even-though-they-can-see-the-items-under-all-resources"></a>Qué hacer si el usuario no puede conectarse a un recurso con el cliente web a pesar de que puede ver los elementos en Todos los recursos

Si el usuario informa que no puede conectarse con el cliente web aunque pueda ver los recursos enumerados, comprueba lo siguiente:

* ¿Está configurado correctamente el rol Puerta de enlace de Escritorio remoto para utilizar un certificado público de confianza?
* ¿El servidor de Puerta de enlace de Escritorio remoto tiene instaladas las actualizaciones necesarias? Asegúrate de que el servidor tiene la [actualización KB4025334](https://support.microsoft.com/help/4025334/windows-10-update-kb4025334) instalada.

Si el usuario recibe un mensaje de error por un certificado de autenticación de servidor inesperado al intentar conectarse, el mensaje mostrará la huella digital del certificado. Busca el administrador de certificados del servidor del agente de Escritorio remoto con esta huella digital para buscar el certificado correcto. Comprueba que el certificado está configurado para usarse para el rol Agente de Escritorio remoto en la página de propiedades de implementación de Escritorio remoto. Después de asegurarte de que el certificado no ha caducado, copia el certificado en formato de archivo .cer en el servidor de Acceso web de RD y ejecuta el siguiente comando en el servidor de Acceso web de RD con el valor entre corchetes sustituido por la ruta de acceso del archivo del certificado:

```PowerShell
Import-RDWebClientBrokerCert <certificate file path>
```

### <a name="diagnose-issues-with-the-console-log"></a>Diagnóstico de problemas con el registro de consola

Si no puedes resolver el problema basándote en las instrucciones de solución de problemas de este artículo, puedes intentar diagnosticar el origen del problema tú mismo consultando el registro de la consola en el explorador. El cliente web proporciona un método para registrar la actividad de registro de la consola del explorador mientras se utiliza el cliente web para ayudar a diagnosticar problemas.

* Selecciona los puntos suspensivos en la esquina superior derecha y dirígete a la página **Acerca de** en el menú desplegable.
* En **Capture support information** (Capturar información de soporte técnico), selecciona el botón **Start recording** (Iniciar grabación).
* Realiza las operaciones en el cliente web que produjo el problema que está tratando de diagnosticar.
* Dirígete a la página **Acerca de** y selecciona **Stop recording** (Detener grabación).
* El explorador descargará automáticamente un archivo .txt denominado **RD Console Logs.txt**. Este archivo contendrá toda la actividad del registro de la consola generada mientras se reproduce el problema de destino.

También puedes tener acceso a la consola directamente mediante el explorador. La consola por lo general se encuentra en las herramientas de desarrollo. Por ejemplo, puedes acceder al registro de Microsoft Edge presionando la tecla **F12** o seleccionando los puntos suspensivos, y después dirigiéndote a **Más herramientas** > **Herramientas de desarrollo**.

## <a name="get-help-with-the-web-client"></a>Obtención de ayuda con el cliente web

Si tienes un problema que no puede resolverse con la información de este artículo, puedes notificárnoslo en la [Comunidad técnica](https://aka.ms/wvdtc). También puedes solicitar o votar nuevas características en nuestro [cuadro de sugerencias](https://remotedesktop.uservoice.com/forums/911494-remote-desktop-web-client).
