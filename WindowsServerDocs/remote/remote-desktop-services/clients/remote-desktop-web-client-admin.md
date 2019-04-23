---
title: Configurar el cliente de web de Escritorio remoto para los usuarios
description: Describe cómo un administrador puede configurar el cliente web de escritorio remoto.
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 11/2/2018
ms.topic: article
author: Heidilohr
ms.localizationpriority: medium
ms.openlocfilehash: 2cb819a7f91646c61b84c3ee70550af6033ba340
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865976"
---
# <a name="set-up-the-remote-desktop-web-client-for-your-users"></a>Configurar el cliente de web de Escritorio remoto para los usuarios

El cliente web de escritorio remoto permite a los usuarios tener acceso a la infraestructura de escritorio remoto de su organización a través de un explorador web compatible. Podrán interactuar con aplicaciones remotas o escritorios como harían con un equipo local independientemente de donde estén. Una vez configurado el cliente web de escritorio remoto, todos los usuarios necesitan para comenzar es la dirección URL donde puede acceder a un explorador web compatible, sus credenciales y el cliente.

>[!IMPORTANT]
>El cliente web no admite actualmente mediante el Proxy de aplicación de Azure y no es compatible con Proxy de aplicación Web en absoluto. Consulte [utilizar RDS con servicios de proxy de aplicación](../rds-supported-config.md#using-remote-desktop-services-with-application-proxy-services) para obtener más información.

## <a name="what-youll-need-to-set-up-the-web-client"></a>Lo que necesitará configurar el cliente web

Antes de comenzar, tenga en cuenta lo siguiente:

* Asegúrese de que su [implementación de escritorio remoto](../rds-deploy-infrastructure.md) tiene una puerta de enlace de escritorio remoto, un agente de conexión a Escritorio remoto y acceso Web de escritorio remoto que se ejecutan en Windows Server 2016 o de 2019.
* Asegúrese de que la implementación está configurada para [licencias de acceso de cliente por usuario](../rds-client-access-license.md) (CAL) en lugar de por dispositivo, en caso contrario, todas las licencias se consumirán.
* Instalar el [actualización Windows 10 KB4025334](https://support.microsoft.com/en-us/help/4025334/windows-10-update-kb4025334) en la puerta de enlace de escritorio remoto. Más adelante las actualizaciones acumulativas ya es posible que contiene este artículo de KB.
* Asegúrese de que se configuran los certificados de confianza públicos para las funciones de puerta de enlace de escritorio remoto y acceso Web de escritorio remoto.
* Asegúrese de que todos los equipos que se conectarán los usuarios que ejecutan una de las siguientes versiones de sistema operativo:
  * Windows 10
  * Windows Server 2008 R2 o posterior

Los usuarios verán un mejor rendimiento que se conecta a Windows Server 2016 (o posterior) y Windows 10 (versión 1611 o posterior).

>[!IMPORTANT]
>Si usa al cliente web durante el período de vista previa y había instalada una versión anterior a 1.0.0, primero debe desinstalar al cliente anterior antes de pasar a la nueva versión. Si recibe un error que dice "el cliente web se instaló con una versión anterior de RDWebClientManagement y debe quitarse primero antes de implementar la nueva versión", siga estos pasos:
>
>1. Abra un símbolo del sistema con privilegios elevados de PowerShell.
>2. Ejecute **RDWebClientManagement Uninstall-Module** para desinstalar el módulo nuevo.
>3. Cierre y vuelva a abrir el símbolo del sistema con privilegios elevados de PowerShell.
>4. Ejecute **RDWebClientManagement Install-Module - RequiredVersion \<versión antigua > para instalar el módulo anterior.**
>5. Ejecute **desinstalar RDWebClient** para desinstalar el cliente web anterior.
>6. Ejecute **RDWebClientManagement Uninstall-Module** para desinstalar el módulo anterior.
>7. Cierre y vuelva a abrir el símbolo del sistema con privilegios elevados de PowerShell.
>8. Haga lo siguiente con los pasos de instalación normal.

## <a name="how-to-publish-the-remote-desktop-web-client"></a>Cómo publicar al cliente web de escritorio remoto

Para instalar al cliente web por primera vez, siga estos pasos:

1. En el servidor de agente de conexión a Escritorio remoto, obtenga el certificado usado para las conexiones de escritorio remoto y expórtelo como archivo .cer. Copie el archivo .cer desde el agente de conexión a Escritorio remoto en el servidor que ejecuta el rol Web de escritorio remoto.
2. En el servidor de acceso Web de RD, abra un símbolo del sistema con privilegios elevados de PowerShell.
3. En Windows Server 2016, actualice el módulo PowerShellGet, ya que la versión de la Bandeja de entrada no admite la instalación del módulo de administración de cliente web. Para actualizar PowerShellGet, ejecute el siguiente cmdlet:
    ```PowerShell
    Install-Module -Name PowerShellGet -Force
    ```

    >[!IMPORTANT]
    >Deberá reiniciar PowerShell para que la actualización surta efecto, en caso contrario, que el módulo no funcionen.

4. Instalar el módulo de PowerShell de administración del cliente de web de escritorio remoto desde la Galería de PowerShell con este cmdlet:
    ```PowerShell
    Install-Module -Name RDWebClientManagement
    ```

5. Después, ejecute el siguiente cmdlet para descargar la versión más reciente del cliente web de escritorio remoto:
    ```PowerShell
    Install-RDWebClientPackage
    ```

6. A continuación, ejecute este cmdlet con el valor entre corchetes reemplazado por la ruta de acceso del archivo .cer que ha copiado en el agente de escritorio remoto:
    ```PowerShell
    Import-RDWebClientBrokerCert <.cer file path>
    ```

7. Por último, ejecute este cmdlet para publicar al cliente web de escritorio remoto:
    ```PowerShell
    Publish-RDWebClientPackage -Type Production -Latest
    ```
    Asegúrese de que se puede acceder el cliente web en la dirección URL del cliente web con el nombre del servidor, con el formato <https://server_FQDN/RDWeb/webclient/index.html>. Es importante usar el nombre del servidor que coincida con el certificado público del acceso Web de escritorio remoto en la dirección URL (normalmente el FQDN del servidor).

    >[!NOTE]
    >Cuando se ejecuta el **publicar RDWebClientPackage** cmdlet, puede ver una advertencia que dice CAL por dispositivo no se admiten, incluso si la implementación está configurada para CAL por usuario. Si la implementación usa CAL por usuario, puede omitir esta advertencia. Lo mostramos para asegurarnos de que tener en cuenta la limitación de la configuración.
8. Cuando esté listo para que los usuarios tener acceso al cliente web, envíelos a la dirección URL del cliente web que creó.

>[!NOTE]
>Para ver una lista de todos los cmdlets compatibles para el módulo RDWebClientManagement, ejecute el siguiente cmdlet de PowerShell:
>```PowerShell
>Get-Command -Module RDWebClientManagement
>```

## <a name="how-to-update-the-remote-desktop-web-client"></a>Cómo actualizar al cliente web de escritorio remoto

Cuando hay disponible una versión nueva del cliente web de escritorio remoto, siga estos pasos para actualizar la implementación con el nuevo cliente:

1. Abra un símbolo de PowerShell con privilegios elevados en el servidor de acceso Web de escritorio remoto y ejecute el siguiente cmdlet para descargar la última versión disponible del cliente web:
    ```PowerShell
    Install-RDWebClientPackage
    ```

2. Si lo desea, puede publicar al cliente antes del lanzamiento oficial de prueba, ejecute este cmdlet:
    ```PowerShell
    Publish-RDWebClientPackage -Type Test -Latest
    ```

    El cliente debe aparecer en la dirección URL de prueba que se corresponde con la dirección URL del cliente web (por ejemplo, <https://server_FQDN/RDWeb/webclient-test/index.html>).
3. Publicar al cliente para los usuarios, ejecute el siguiente cmdlet:
    ```PowerShell
    Publish-RDWebClientPackage -Type Production -Latest
    ```

    Esto reemplazará al cliente para todos los usuarios cuando vuelva a iniciar la página web.

## <a name="how-to-uninstall-the-remote-desktop-web-client"></a>Cómo desinstalar al cliente web de escritorio remoto

Para quitar todos los seguimientos del cliente web, siga estos pasos:

1. En el servidor de acceso Web de RD, abra un símbolo del sistema con privilegios elevados de PowerShell.
2. Cancelar la publicación de los clientes de prueba y producción, desinstale todos los paquetes locales y quitar la configuración de cliente web:

   ```PowerShell
   Uninstall-RDWebClient
   ```

3. Desinstalar el módulo de PowerShell de administración del cliente de web de escritorio remoto:

   ```PowerShell
   Uninstall-Module -Name RDWebClientManagement
   ```

## <a name="how-to-install-the-remote-desktop-web-client-without-an-internet-connection"></a>Cómo instalar al cliente web de escritorio remoto sin una conexión a internet

Siga estos pasos para implementar al cliente web en un servidor de acceso Web de escritorio remoto que no tiene una conexión a internet.

> [!NOTE]
> Instalación sin conexión a internet está disponible en la versión 1.0.1 y versiones posteriores del módulo RDWebClientManagement PowerShell.

> [!NOTE]
> Necesita un administrador de equipo con acceso a internet para descargar los archivos necesarios antes de transferirlos al servidor sin conexión.

> [!NOTE]
> El equipo del usuario final necesita una conexión a internet por ahora. Esto se solucionará en una versión futura del cliente para proporcionar un escenario completo sin conexión.

### <a name="from-a-device-with-internet-access"></a>Desde un dispositivo con acceso a internet

1. Abra un símbolo del sistema de PowerShell.

2. Importar el módulo de PowerShell de administración del cliente de web de escritorio remoto desde la Galería de PowerShell:
    ```PowerShell
    Import-Module -Name RDWebClientManagement
    ```

3. Descargue la versión más reciente del cliente web de escritorio remoto para su instalación en un dispositivo diferente:
    ```PowerShell
    Save-RDWebClientPackage "C:\WebClient\"
    ```

4. Descargue la versión más reciente del módulo RDWebClientManagement PowerShell:
    ```PowerShell
    Find-Module -Name "RDWebClientManagement" -Repository "PSGallery" | Save-Module -Path "C:\WebClient\"
    ```

5. Copie el contenido de "C:\WebClient\" al servidor de acceso Web de escritorio remoto.

### <a name="from-the-rd-web-access-server"></a>Desde el servidor de acceso Web de RD

Siga las instrucciones de [cómo publicar el cliente web de escritorio remoto](remote-desktop-web-client-admin.md#how-to-publish-the-remote-desktop-web-client), reemplazando los pasos 4 y 5, por lo siguiente.

4. Importar el módulo de PowerShell de administración del cliente de web de escritorio remoto desde la carpeta local:
    ```PowerShell
    Import-Module -Name "C:\WebClient\"
    ```

5. Implementar la versión más reciente del cliente de web de escritorio remoto desde la carpeta local (reemplazar por el archivo zip adecuado):
    ```PowerShell
    Install-RDWebClientPackage -Source "C:\WebClient\rdwebclient-1.0.1.zip"
    ```

## <a name="connecting-to-rd-broker-without-rd-gateway-in-windows-server-2019"></a>Conectarse al agente de escritorio remoto sin puerta de enlace de escritorio remoto en Windows Server de 2019
En esta sección se describe cómo habilitar una conexión de cliente web a un agente de escritorio remoto sin una puerta de enlace de escritorio remoto en Windows Server 2019.

### <a name="setting-up-the-rd-broker-server"></a>Configurar el servidor de agente de escritorio remoto

#### <a name="follow-these-steps-if-there-is-no-certificate-bound-to-the-rd-broker-server"></a>Siga estos pasos si no hay ningún certificado enlazado con el servidor de agente de escritorio remoto

1. Abra **administrador del servidor** > **servicios de escritorio remoto**.

2. En **Deployment Overview** sección, seleccione el **tareas** menú desplegable.

3. Seleccione **editar las propiedades de implementación**, una nueva ventana titulada **las propiedades de implementación** se abrirá.

4. En el **las propiedades de implementación** ventana, seleccione **certificados** en el menú izquierdo.

5. En la lista de niveles de certificados, seleccione **RD Connection Broker - habilitar el inicio de sesión único**. Tiene dos opciones: (1) crear un nuevo certificado o (2) un certificado existente.

#### <a name="follow-these-steps-if-there-is-a-certificate-previously-bound-to-the-rd-broker-server"></a>Siga estos pasos si hay un certificado enlazado anteriormente al servidor de agente de escritorio remoto

1. Abra el certificado enlazado al agente y copia el **huella digital** valor.

2. Para enlazar este certificado al puerto seguro 3392, abra una ventana de PowerShell con privilegios elevados y ejecute el siguiente comando, reemplazando **"< thumbprint >"** con el valor copiado en el paso anterior:

    ```PowerShell
    netsh http add sslcert ipport=0.0.0.0:3392 certhash="<thumbprint>" certstorename="Remote Desktop" appid="{00000000-0000-0000-0000-000000000000}"
    ```

    > [!NOTE]
    > Para comprobar si el certificado se ha enlazado correctamente, ejecute el siguiente comando:
    >
    > ```PowerShell
    > netsh http show sslcert
    > ```
    >
    > En la lista de los enlaces de certificados SSL, asegúrese de que el certificado correcto está enlazado al puerto 3392.

3. Abra el registro de Windows (regedit) y nagivate a ```HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp``` y busque la clave **WebSocketURI**. El valor debe establecerse en **https://+:3392/rdp/**.

### <a name="setting-up-the-rd-session-host"></a>Configurar el Host de sesión de escritorio remoto
Si el servidor Host de sesión de escritorio remoto es diferente del servidor de agente de escritorio remoto, siga estos pasos:

1. Crear un certificado para la máquina Host de sesión de escritorio remoto, ábralo y copie el **huella digital** valor.

2. Para enlazar este certificado al puerto seguro 3392, abra una ventana de PowerShell con privilegios elevados y ejecute el siguiente comando, reemplazando **"< thumbprint >"** con el valor copiado en el paso anterior:

    ```PowerShell
    netsh http add sslcert ipport=0.0.0.0:3392 certhash="<thumbprint>" appid="{00000000-0000-0000-0000-000000000000}"
    ```

    > [!NOTE]
    > Para comprobar si el certificado se ha enlazado correctamente, ejecute el siguiente comando:
    >
    > ```PowerShell
    > netsh http show sslcert
    > ```
    >
    > En la lista de los enlaces de certificados SSL, asegúrese de que el certificado correcto está enlazado al puerto 3392.

3. Abra el registro de Windows (regedit) y nagivate a ```HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp``` y busque la clave **WebSocketURI**. El valor debe establecerse en **https://+:3392/rdp/**.

### <a name="general-observations"></a>Observaciones generales

* Asegúrese de que el Host de sesión de escritorio remoto y el agente a Escritorio remoto de servidor ejecutan Windows Server 2019.

* Asegúrese de que ese público de confianza se configuran los certificados de servidor del Host de sesión de escritorio remoto y el agente a Escritorio remoto.
    > [!NOTE]
    > Si el Host de sesión de escritorio remoto y el servidor de agente de escritorio remoto comparten el mismo equipo, establezca solo el certificado de servidor de agente a Escritorio remoto. Si el servidor Host de sesión de escritorio remoto y el agente a Escritorio remoto utiliza equipos diferentes, debe configurar con certificados únicos.

* El **nombre alternativo de sujeto (SAN)** para cada certificado debe estar establecido en la máquina **nombre de dominio completo (FQDN)**. El **nombre común (CN)** debe coincidir con la SAN para cada certificado.

## <a name="troubleshooting"></a>Solución de problemas

Si un usuario informa de cualquiera de los siguientes problemas al abrir al cliente web por primera vez, las siguientes secciones le indicará qué hacer para solucionarlos.

### <a name="what-to-do-if-the-users-browser-shows-a-security-warning-when-they-try-to-access-the-web-client"></a>Qué hacer si el explorador del usuario muestra una advertencia de seguridad cuando intenten tener acceso al cliente web

El rol de acceso Web de escritorio remoto no es posible que esté utilizando un certificado de confianza. Asegúrese de que el rol acceso Web de escritorio remoto se configura con un certificado de confianza pública.

Si esto no funciona, el nombre del servidor en la web cliente URL podría no coincidir con el nombre proporcionado por el certificado Web de escritorio remoto. Asegúrese de que la dirección URL usa el FQDN del servidor que hospeda el rol Web de escritorio remoto.

### <a name="what-to-do-if-the-user-cant-connect-to-a-resource-with-the-web-client-even-though-they-can-see-the-items-under-all-resources"></a>Qué hacer si el usuario no se puede conectar a un recurso con el cliente web, aunque pueden ver los elementos en todos los recursos

Si el usuario informa de que no se puede conectar con el cliente web, aunque pueden ver los recursos enumerados, compruebe lo siguiente:

* ¿Está configurado correctamente el rol de puerta de enlace de escritorio remoto para usar un certificado de confianza público?
* ¿El servidor de puerta de enlace de escritorio remoto tiene instaladas las actualizaciones necesarias? Asegúrese de que el servidor tiene [la actualización KB4025334](https://support.microsoft.com/en-us/help/4025334/windows-10-update-kb4025334) instalado.

Si el usuario recibe un error "se ha recibido el certificado de autenticación de servidor inesperado" del mensaje cuando intenten conectarse, a continuación, el mensaje mostrará la huella digital del certificado. Buscar el Administrador de certificados del servidor de agente de escritorio remoto con ese huella digital para buscar el certificado correcto. Compruebe que el certificado está configurado para usarse para el rol de agente a Escritorio remoto en la página de propiedades de implementación de escritorio remoto. Después de asegurarse de que el certificado no ha expirado, copie el certificado en formato de archivo .cer en el servidor de acceso Web de escritorio remoto y ejecute el siguiente comando en el servidor de acceso Web de escritorio remoto con el valor entre corchetes reemplazado por la ruta de acceso de archivo del certificado:

```PowerShell
Import-RDWebClientBrokerCert <certificate file path>
```

### <a name="diagnose-issues-with-the-console-log"></a>Diagnosticar problemas con el registro de consola

Si no se puede resolver el problema según las instrucciones de solución de problemas en este artículo, puede intentar diagnosticar la causa del problema a través de la consola de registro en el explorador. El cliente web proporciona un método para registrar la actividad de registro de la consola de explorador mientras se usa al cliente web para ayudar a diagnosticar problemas.

* Seleccione los puntos suspensivos en la esquina superior derecha y navegue hasta la **sobre** página en el menú desplegable.
* En **captura información de soporte técnico** seleccione la **iniciar la grabación** botón.
* Realizar las operaciones en el cliente web que produjo el problema que intenta diagnosticar.
* Navegue hasta la **sobre** página y seleccione **Detener grabación**.
* El explorador descargará automáticamente un archivo .txt titulado **Logs.txt de la consola de escritorio remoto**. Este archivo contendrá la actividad de registro de consola completa generada mientras se reproduce el problema de destino.

También puede tener acceso a la consola directamente a través del explorador. La consola generalmente se encuentra en las herramientas de desarrollo. Por ejemplo, puede tener acceso al registro en Microsoft Edge presionando el **F12** clave o seleccionando el botón de puntos suspensivos, a continuación, vaya a **más herramientas** > **deherramientasdedesarrollo**.

## <a name="get-help-with-the-web-client"></a>Obtener ayuda con el cliente web

Si se ha encontrado un problema que no puede resolverse mediante la información de este artículo, puede [envíenos un correo electrónico](mailto:rdwbclnt@microsoft.com) para notificarlo. También puede solicitar o votar nuevas características en nuestras [sugerencias](https://aka.ms/rdwebfbk).