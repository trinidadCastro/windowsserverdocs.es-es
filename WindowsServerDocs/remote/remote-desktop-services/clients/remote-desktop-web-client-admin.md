---
title: Configurar el cliente de web de Escritorio remoto para los usuarios
description: Describe cómo un administrador puede configurar el cliente de web de escritorio remoto.
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 11/2/2018
ms.topic: article
author: Heidilohr
ms.localizationpriority: medium
ms.openlocfilehash: 2cb819a7f91646c61b84c3ee70550af6033ba340
ms.sourcegitcommit: 491c94b25501732c4a4abda533cc62b8bd278ed2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/22/2019
ms.locfileid: "9099188"
---
# Configurar el cliente de web de Escritorio remoto para los usuarios

El cliente de web de escritorio remoto permite a los usuarios acceder a la infraestructura de escritorio remoto de la organización a través de un explorador web compatible. Podrán interactuar con aplicaciones remotas o equipos de escritorio al igual que lo harían con un equipo local, independientemente de dónde se encuentran. Una vez configurado el cliente de web de escritorio remoto, todo lo que necesitan los usuarios para empezar a trabajar es la dirección URL dónde pueden tener acceso al cliente, sus credenciales y un explorador web compatible.

>[!IMPORTANT]
>El cliente web no admite actualmente con el Proxy de aplicación de Azure y no es compatible con Proxy de aplicación Web en absoluto. Para obtener más información, consulte [Utilizar RDS con servicios de proxy de aplicación](../rds-supported-config.md#using-remote-desktop-services-with-application-proxy-services) .

## Lo que tendrás que configurar el cliente web

Antes de comenzar, ten en cuenta lo siguiente:

* Asegúrese de que la [implementación de escritorio remoto](../rds-deploy-infrastructure.md) tiene una puerta de enlace de escritorio remoto, un agente de conexión a Escritorio remoto y acceso Web de RD que se ejecutan en Windows Server 2016 o 2019.
* Asegúrese de que la implementación se configura para [licencias de acceso de cliente por usuario](../rds-client-access-license.md) (CAL) en lugar de cada dispositivo, en caso contrario que se consume todas las licencias.
* Instalar la [actualización de Windows 10 KB4025334](https://support.microsoft.com/en-us/help/4025334/windows-10-update-kb4025334) en la puerta de enlace de escritorio remoto. Actualizaciones acumulativas más adelante ya es posible que contiene este artículo de KB.
* Comprueba que los certificados de confianza públicos están configurados para las funciones de puerta de enlace de escritorio remoto y acceso Web de RD.
* Asegúrate de que todos los usuarios se conectan a los equipos están ejecutando una de las siguientes versiones de sistema operativo:
  * Windows 10
  * Windows Server 2008 R2 o posterior

Los usuarios verán un mejor rendimiento conectarse a Windows Server 2016 (o posterior) y Windows 10 (versión 1611 o posterior).

>[!IMPORTANT]
>Si se usa al cliente web durante el período de vista previa y había instalado una versión anterior de la versión 1.0.0, primero debe desinstalar al cliente antiguo antes de pasar a la nueva versión. Si recibes un error que dice "el cliente web se instaló con una versión anterior de RDWebClientManagement y primero debe quitarse antes de implementar la nueva versión", sigue estos pasos:
>
>1. Abre un símbolo del sistema con privilegios elevados de PowerShell.
>2. Ejecutar **RDWebClientManagement de módulo de desinstalación** para desinstalar el módulo de nuevo.
>3. Cierra y vuelve a abrir el símbolo del sistema con privilegios elevados de PowerShell.
>4. Ejecutar **version> de \<old parámetros-RequiredVersion RDWebClientManagement del módulo de instalación para instalar el módulo antiguo.**
>5. Ejecutar **RDWebClient de desinstalación** para desinstalar al cliente de web antiguos.
>6. Ejecutar **RDWebClientManagement de módulo de desinstalación** para desinstalar el módulo anterior.
>7. Cierra y vuelve a abrir el símbolo del sistema con privilegios elevados de PowerShell.
>8. Continúe con los pasos de instalación normal como sigue:

## Cómo publicar al cliente de web de escritorio remoto

Para instalar al cliente web por primera vez, sigue estos pasos:

1. En el servidor de agente de conexión a Escritorio remoto, obtener el certificado usado para conexiones a Escritorio remoto y exportarla como un archivo .cer. Copia el archivo .cer desde el agente de conexión a Escritorio remoto en el servidor que ejecuta el rol de Web de escritorio remoto.
2. En el servidor de acceso Web de RD, abre un símbolo del sistema con privilegios elevados de PowerShell.
3. En Windows Server 2016, actualiza el módulo PowerShellGet dado que la versión de la Bandeja de entrada no admite la instalación del módulo de administración de cliente web. Para actualizar PowerShellGet, ejecuta el siguiente cmdlet:
    ```PowerShell
    Install-Module -Name PowerShellGet -Force
    ```

    >[!IMPORTANT]
    >Tendrás que reiniciar PowerShell para que surta efecto, de lo contrario de que no funcionen el módulo la actualización.

4. Instalar el módulo de PowerShell de administración de cliente de escritorio remoto web desde la Galería de PowerShell con este cmdlet:
    ```PowerShell
    Install-Module -Name RDWebClientManagement
    ```

5. Después, ejecuta el siguiente cmdlet para descargar la versión más reciente del cliente de web de escritorio remoto:
    ```PowerShell
    Install-RDWebClientPackage
    ```

6. A continuación, ejecuta este cmdlet con el valor entre corchetes reemplazado por la ruta de acceso del archivo .cer que copió desde el agente de escritorio remoto:
    ```PowerShell
    Import-RDWebClientBrokerCert <.cer file path>
    ```

7. Por último, ejecuta este cmdlet para publicar al cliente de web de escritorio remoto:
    ```PowerShell
    Publish-RDWebClientPackage -Type Production -Latest
    ```
    Asegúrese de que puede tener acceso al cliente web en la dirección URL de cliente de web con el nombre del servidor, como el formato <https://server_FQDN/RDWeb/webclient/index.html>. Es importante usar el nombre del servidor que coincida con el certificado público de acceso Web de RD en la dirección URL (por lo general, el FQDN del servidor).

    >[!NOTE]
    >Cuando se ejecuta el cmdlet **Publicar RDWebClientPackage** , verás una advertencia que dice CAL por dispositivo no se admiten, incluso si la implementación se configura para CAL por usuario. Si la implementación usa CAL por usuario, puede omitir esta advertencia. Lo mostramos para asegurarse de que eres consciente de la limitación de configuración.
8. Cuando estés listo para que los usuarios acceso al cliente web, simplemente enviarles la dirección URL de cliente de web que creaste.

>[!NOTE]
>Para ver una lista de todos los cmdlets admitidos para el módulo RDWebClientManagement, ejecuta el siguiente cmdlet de PowerShell:
>```PowerShell
>Get-Command -Module RDWebClientManagement
>```

## Cómo actualizar al cliente de web de escritorio remoto

Cuando está disponible una nueva versión del cliente de web de escritorio remoto, sigue estos pasos para actualizar la implementación con el nuevo cliente:

1. Abre un símbolo de PowerShell con privilegios elevados en el servidor de acceso Web de RD y ejecuta el siguiente cmdlet para descargar la versión más reciente del cliente web:
    ```PowerShell
    Install-RDWebClientPackage
    ```

2. Opcionalmente, puedes publicar al cliente para las pruebas antes de su lanzamiento oficial, ejecuta este cmdlet:
    ```PowerShell
    Publish-RDWebClientPackage -Type Test -Latest
    ```

    El cliente debe aparecer en la dirección URL de prueba que corresponde a la dirección URL del cliente web (por ejemplo, <https://server_FQDN/RDWeb/webclient-test/index.html>).
3. Publicar al cliente para los usuarios, ejecuta el siguiente cmdlet:
    ```PowerShell
    Publish-RDWebClientPackage -Type Production -Latest
    ```

    Esto reemplazará al cliente para todos los usuarios cuando se vuelve a iniciar la página web.

## Cómo desinstalar al cliente de web de escritorio remoto

Para quitar todas las trazas del cliente web, sigue estos pasos:

1. En el servidor de acceso Web de RD, abre un símbolo del sistema con privilegios elevados de PowerShell.
2. Anular la publicación de los clientes de prueba y producción, desinstalar todos los paquetes locales y quitar la configuración de cliente de web:

   ```PowerShell
   Uninstall-RDWebClient
   ```

3. Desinstalar el módulo de PowerShell de administración del cliente de web de escritorio remoto:

   ```PowerShell
   Uninstall-Module -Name RDWebClientManagement
   ```

## Cómo instalar al cliente de web de escritorio remoto sin una conexión a internet

Sigue estos pasos para implementar al cliente de web a un servidor de acceso Web de RD que no tiene una conexión a internet.

> [!NOTE]
> Instalación sin conexión a internet está disponible en la versión 1.0.1 y versiones posteriores del módulo de RDWebClientManagement PowerShell.

> [!NOTE]
> Todavía tienes un equipo de administrador con acceso a internet para descargar los archivos necesarios antes de transferirlos al servidor sin conexión.

> [!NOTE]
> El equipo del usuario final, necesita una conexión a internet por ahora. Esto se resolverá en una versión futura del cliente para proporcionar un escenario sin conexión completo.

### Desde un dispositivo con acceso a internet

1. Abre un símbolo del sistema de PowerShell.

2. Importar el módulo de PowerShell de administración del cliente de web de escritorio remoto de la Galería de PowerShell:
    ```PowerShell
    Import-Module -Name RDWebClientManagement
    ```

3. Descarga la última versión del cliente de web de escritorio remoto para la instalación en otro dispositivo:
    ```PowerShell
    Save-RDWebClientPackage "C:\WebClient\"
    ```

4. Descargar la versión más reciente del módulo de RDWebClientManagement PowerShell:
    ```PowerShell
    Find-Module -Name "RDWebClientManagement" -Repository "PSGallery" | Save-Module -Path "C:\WebClient\"
    ```

5. Copia el contenido de "C:\WebClient\" en el servidor de acceso Web de RD.

### Desde el servidor de acceso Web de RD

Sigue las instrucciones en [cómo publicar al cliente de web de escritorio remoto](remote-desktop-web-client-admin.md#how-to-publish-the-remote-desktop-web-client), sustituyendo los pasos 4 y 5 con lo siguiente.

4. Importar el módulo de PowerShell de administración de cliente de escritorio remoto web desde la carpeta local:
    ```PowerShell
    Import-Module -Name "C:\WebClient\"
    ```

5. Implementar la versión más reciente del cliente de web de escritorio remoto de la carpeta local (reemplaza con el archivo zip adecuado):
    ```PowerShell
    Install-RDWebClientPackage -Source "C:\WebClient\rdwebclient-1.0.1.zip"
    ```

## Conexión a Escritorio remoto agente sin puerta de enlace de escritorio remoto en Windows Server 2019
En esta sección se describe cómo habilitar una conexión de cliente de web a un agente de escritorio remoto sin una puerta de enlace de escritorio remoto en Windows Server 2019.

### Configurar el servidor de agente de escritorio remoto

#### Sigue estos pasos si no hay ningún certificado enlazado con el servidor de agente de escritorio remoto

1. Abre el **Administrador del servidor** > **Servicios de escritorio remoto**.

2. En la sección **Introducción a la implementación** , selecciona el menú desplegable de **tareas** .

3. Se abrirá selecciona **Editar las propiedades de implementación**, una nueva ventana titulada **Las propiedades de implementación** .

4. En la ventana de **Propiedades de implementación** , selecciona **los certificados** en el menú izquierdo.

5. En la lista de niveles de certificados, seleccione **Agente de conexión a Escritorio remoto - habilitar inicio de sesión único en**. Tienes dos opciones: (1) crea un nuevo certificado o (2) un certificado existente.

#### Sigue estos pasos si hay un certificado enlazado previamente en el servidor de agente a Escritorio remoto

1. Abre el certificado enlazado con el agente de y copia el valor de **huella digital** .

2. Para enlazar este certificado al puerto seguro 3392, abre una ventana de PowerShell con privilegios elevados y ejecuta el siguiente comando, sustituyendo **"< huella digital >"** con el valor copiado del paso anterior:

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
    > En la lista de enlaces de certificado SSL, asegúrate de que el certificado correcto está enlazado al puerto 3392.

3. Abrir el registro de Windows (regedit) y nagivate a ```HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp``` y busca la clave **WebSocketURI**. El valor debe establecerse en **https://+:3392/rdp/**.

### Configurar el Host de sesión de escritorio remoto
Si el servidor Host RD es diferente del servidor de agente de escritorio remoto, sigue estos pasos:

1. Crear un certificado para el equipo Host de sesión de escritorio remoto, ábrelo y copia el valor de **huella digital** .

2. Para enlazar este certificado al puerto seguro 3392, abre una ventana de PowerShell con privilegios elevados y ejecuta el siguiente comando, sustituyendo **"< huella digital >"** con el valor copiado del paso anterior:

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
    > En la lista de enlaces de certificado SSL, asegúrate de que el certificado correcto está enlazado al puerto 3392.

3. Abrir el registro de Windows (regedit) y nagivate a ```HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp``` y busca la clave **WebSocketURI**. El valor debe establecerse en **https://+:3392/rdp/**.

### Observaciones generales

* Asegúrate de que el Host de sesión de escritorio remoto y el agente a Escritorio remoto de servidor están ejecutando Windows Server 2019.

* Asegúrate de que esa pública de confianza se configuran los certificados de servidor la Host de sesión de escritorio remoto y el agente a Escritorio remoto.
    > [!NOTE]
    > Si el Host de sesión de escritorio remoto y el servidor de agente de RD comparten la misma máquina, establecer únicamente el certificado de servidor de agente a Escritorio remoto. Si el servidor Host de sesión de escritorio remoto y agente a Escritorio remoto usa diferentes máquinas, ambos deben configurarse con certificados únicos.

* El **Nombre de alternativa de firmante (SAN)** para cada certificado debe establecerse en **Nombre de dominio completo (FQDN)** de la máquina. El **Nombre común (CN)** debe coincidir con el SAN para cada certificado.

## Solución de problemas

Si un usuario informa de cualquiera de los siguientes problemas al abrir al cliente web por primera vez, las siguientes secciones te indicará qué debe hacer para solucionarlos.

### Qué hacer si el explorador del usuario muestra una advertencia de seguridad al intentar acceder al cliente web

El rol de acceso Web de RD no es posible que se usa un certificado de confianza. Asegúrese de que el rol de acceso Web de RD está configurado con un certificado de confianza pública.

Si esto no funciona, el nombre del servidor del sitio Web dirección URL del cliente puede no coincidir con el nombre proporcionado por el certificado de Web de escritorio remoto. Asegúrate de que la dirección URL usa el FQDN del servidor que aloja el rol de Web de escritorio remoto.

### Qué hacer si el usuario no se puede conectar a un recurso con el cliente de web, aunque pueden ver los elementos en todos los recursos

Si el usuario informa de que no se puede conectar con el cliente de web, aunque pueden ver los recursos que aparecen, comprueba lo siguiente:

* ¿Está configurado correctamente el rol de puerta de enlace de escritorio remoto para usar un certificado de confianza público?
* ¿Tiene el servidor de puerta de enlace de escritorio remoto las actualizaciones necesarias instaladas? Asegúrate de que el servidor tiene [la KB4025334 actualizar](https://support.microsoft.com/en-us/help/4025334/windows-10-update-kb4025334) instalado.

Si el usuario recibe un error "se ha recibido el certificado de autenticación de servidor inesperado" mensaje cuando intenten conectarse, a continuación, el mensaje muestra la huella digital del certificado. Buscar el Administrador de certificados del servidor de agente de escritorio remoto con esa huella digital para encontrar el certificado correcto. Comprueba que el certificado está configurado para usarse para la función de agente a Escritorio remoto en la página de propiedades de la implementación de escritorio remoto. Después de asegurarse de que el certificado no ha expirado, copiar el certificado en formato de archivo .cer en el servidor de acceso Web de RD y ejecuta el siguiente comando en el servidor de acceso Web de RD con el valor entre corchetes reemplazado por la ruta de acceso de archivo del certificado:

```PowerShell
Import-RDWebClientBrokerCert <certificate file path>
```

### Diagnosticar problemas con el registro de la consola

Si no se puede resolver el problema en función de las instrucciones de solución de problemas de este artículo, puedes intentar diagnosticar el origen del problema al mirar la consola de iniciar sesión en el explorador. El cliente de web proporciona un método para registrar la actividad de registro de la consola de explorador mientras se usa al cliente de web para ayudar a diagnosticar problemas.

* Selecciona los puntos suspensivos en la esquina superior derecha y navegar a la página **sobre** en el menú desplegable.
* En la **información de soporte técnico de captura** selecciona el botón para **iniciar la grabación** .
* Realizar las operaciones en el cliente de web que generó el problema que estás intentando diagnosticar.
* Ve a la página **acerca de** y seleccione **Detener la grabación**.
* El explorador descargará automáticamente un archivo .txt **Logs.txt de la consola de escritorio remoto**. Este archivo contendrá la actividad de registro de consola completa generada durante la reproducción el problema de destino.

También puede tener acceso a la consola directamente a través del explorador. Por lo general, la consola se encuentra en las herramientas de desarrollo. Por ejemplo, puede tener acceso el inicio de sesión en Microsoft Edge presionando la tecla **F12** o seleccionando los puntos suspensivos y navegar a **más herramientas** > **Herramientas de desarrollo**.

## Obtén ayuda con el cliente web

Si se ha encontrado un problema que no se resuelve mediante la información de este artículo, puedes [envíanos un correo electrónico](mailto:rdwbclnt@microsoft.com) para informar de él. También puede solicitar o votar las nuevas características en el [cuadro de sugerencias](https://aka.ms/rdwebfbk).