---
title: Solución de problemas de las conexiones a Escritorio remoto
description: Procedimientos de solución de problemas organizados por síntoma
ms.custom: na
ms.reviewer: rklemen;josh.bender
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.assetid: ''
author: jobende
manager: ''
ms.author: v-tea;kaushika;rklemen;josh.bender
ms.date: 02/22/2019
ms.localizationpriority: medium
ms.openlocfilehash: 8965df09fd57decf7f4a1b6b89861e5a67034a0a
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297404"
---
# Solución de problemas de las conexiones a Escritorio remoto
Para obtener una explicación breve de algunos de los problemas más comunes de servicios de escritorio remoto (RDS), consulta [preguntas frecuentes sobre los clientes de escritorio remoto](https://review.docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-client-faq). Este artículo describe varios enfoques más avanzados para solucionar problemas de conexión. Muchos de estos procedimientos son aplicables si estás tratando de solucionar una configuración sencilla, por ejemplo, un equipo físico que se conecta a otro equipo físico o una configuración más complicada. Algunos procedimientos solucionan problemas que se producen solo en escenarios de varios usuarios más complicados. Para obtener más información acerca de los componentes de escritorio remotos y cómo funcionan juntas, consulte la [arquitectura de servicios de escritorio remoto](https://docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/desktop-hosting-logical-architecture).

> [!NOTE]  
> Muchos de los procedimientos que se describen en este artículo requieren acceso a varios equipos, algunas de las cuales que pudieras tener a acceder de forma remota. Para obtener más información sobre cómo configurarlos y herramientas de administración remota, consulte [Las herramientas de administración de servidor remoto (RSAT) para sistemas operativos Windows](https://support.microsoft.com/en-us/help/2693643/remote-server-administration-tools-rsat-for-windows-operating-systems).

En la lista siguiente, identifican el tipo de síntoma que experimenten tú (o los usuarios).

- [El cliente de escritorio remoto no se puede conectar con el escritorio remoto, pero no hay síntomas específicos o mensajes (pasos de solución de problemas generales)](#no-specific-symptoms-or-messages-general-troubleshooting-steps)
- [El cliente de escritorio remoto no se puede conectar con el escritorio remoto y recibe un mensaje "Clase no registrada"](#client-cannot-connect-class-not-registered)
- [El cliente de escritorio remoto no se puede conectar con el escritorio remoto y no recibe unos "licencias disponibles" o el mensaje "error de seguridad"](#client-cannot-connect-no-licenses-available)
- [El usuario recibe un mensaje de "Acceso denegado" o debe proporcionar credenciales dos veces](#user-cannot-authenticate-or-must-authenticate-twice)
- [Acerca de cómo conectar, el recibe un mensaje "El servicio de escritorio remoto está actualmente ocupado"](#on-connecting-user-receives-remote-desktop-service-is-currently-busy-message)
- [El cliente de escritorio remoto se desconecta y no se puede conectar a la misma sesión](#rd-client-disconnects-and-cannot-reconnect-to-the-same-session)
- [El usuario se conecta a un equipo portátil remoto a través de una red inalámbrica, y, a continuación, el equipo portátil se desconecta de la red](#remote-laptop-disconnects-from-wireless-network).
- [El usuario experimenta problemas con las aplicaciones remotas o un rendimiento deficiente](#user-experiences-poor-performance-or-application-problems)

> [!NOTE]  
> Para obtener una lista de códigos RDP desconectar, consulta la [enumeración ExtendedDisconnectReasonCode](https://docs.microsoft.com/en-us/windows/desktop/TermServ/extendeddisconnectreasoncode). 

## No hay síntomas específicos o mensajes (pasos de solución de problemas generales)

Siga estos pasos cuando un cliente de escritorio remoto no se puede conectar a un escritorio remoto, pero no proporciona mensajes u otros síntomas que podría ayudar a identifican la causa. Para resolver muchas de las causas más comunes de este tipo de problema, usa los siguientes métodos:

- [Comprobar el estado del protocolo RDP](#check-the-status-of-the-rdp-protocol)
- [Comprobar el estado de los servicios RDP](#check-the-status-of-the-rdp-services)
- [Compruebe que está funcionando el agente de escucha RDP](#check-that-the-rdp-listener-is-functioning)
- [Comprobar el puerto de escucha RDP](#check-the-rdp-listener-port)

### Comprobar el estado del protocolo RDP

#### Comprobar el estado del protocolo RDP en un equipo local

Para buscar y cambiar el estado del protocolo RDP en un equipo local, consulta [cómo habilitar Escritorio remoto](https://docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-allow-access#how-to-enable-remote-desktop).

> [!NOTE]  
> Si las opciones de escritorio remotas no están disponibles, consulta [comprobar si un objeto de directiva de grupo está bloqueando RDP](#check-whether-a-group-policy-object-gpo-is-blocking-rdp-on-a-local-computer).

#### Comprobar el estado del protocolo RDP en un equipo remoto

> [!IMPORTANT]  
> Sigue los pasos descritos en esta sección cuidadosamente. Pueden producirse problemas graves si modificas el registro de manera incorrecta. Antes, modificarla, [copia de seguridad del registro para la restauración](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) en caso de que se producen problemas.

Para buscar y cambiar el estado del protocolo RDP en un equipo remoto, usa una conexión de red del registro:

1. Selecciona el **Inicio**, seleccione **Ejecutar**y, a continuación, especifica **regedt32**.
2. En el Editor del registro, selecciona el **archivo**y, a continuación, selecciona **Conectar al registro de red**.
3. En el cuadro de diálogo **Seleccionar equipo** , escribe el nombre del equipo remoto, selecciona **Comprobar nombres**y, a continuación, selecciona **Aceptar**.
4. Ve a **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server**.  
   ![Editor del registro, que muestra la entrada de fDenyTSConnections](..\media\troubleshoot-remote-desktop-connections\RegEntry_fDenyTSConnections.png)
   - Si el valor de la clave de **fDenyTSConnections** es **0**, RDP está habilitado
   - Si el valor de la clave de **fDenyTSConnections** es **1**, RDP está deshabilitada
5. Para habilitar RDP, cambia el valor de **fDenyTSConnections** de **1** a **0**.

#### Comprueba si un objeto de directiva de grupo (GPO) está bloqueando RDP en un equipo local

Si no se puede activar RDP en la interfaz de usuario o si el valor de **fDenyTSConnections** se revierte a **1** después de que se han cambiado, un GPO puede estar reemplazando la configuración de nivel de equipo.

Para comprobar la configuración de directiva de grupo en un equipo local, abre una ventana del símbolo del sistema como administrador y escribe el siguiente comando:
```
gpresult /H c:\gpresult.html
```
Después de que finalice este comando, abra gpresult.html. En **Equipo\\plantillas administrativas\\componentes de Components\\Remote de escritorio Services\\Remote de sesión de escritorio Host\\Connections**, busca la directiva de **Permitir a los usuarios conectarse de forma remota mediante el uso de servicios de escritorio remoto** .

- Si la configuración de esta directiva está **habilitada**, la directiva de grupo no bloquea las conexiones RDP.
- Si la configuración de esta directiva está **deshabilitado**, comprueba **Ganando GPO**. Este es el GPO que está bloqueando las conexiones de RDP.
![Un segmento de ejemplo de gpresult.html, en el que el GPO de nivel de dominio ** bloque RDP ** es deshabilitar RDP.](..\media\troubleshoot-remote-desktop-connections\GPResult_RDSH_Connections_GP.png)
   
  ![Un segmento de ejemplo de gpresult.html, en el que ** la directiva de grupo Local ** es deshabilitar RDP.](..\media\troubleshoot-remote-desktop-connections\GPResult_RDSH_Connections_LGP.png)

#### Comprueba si un GPO está bloqueando RDP en un equipo remoto

Para comprobar la configuración de directiva de grupo en un equipo remoto, el comando es casi de los mismos que para un equipo local:
```
gpresult /S <computer name> /H c:\gpresult-<computer name>.html
```
El archivo que se produce este comando (**gpresult-\<computer name\>.html**) usa el mismo formato de información que use la versión del equipo local (**gpresult.html**).

#### Modificar un GPO de bloqueo

Puedes modificar estas opciones de configuración en el Editor de objeto de directiva de grupo (GPE) y la consola de administración de directivas de grupo (GPM). Para obtener más información sobre cómo usar la directiva de grupo, consulta [La administración avanzada de directivas de grupo](https://docs.microsoft.com/en-us/microsoft-desktop-optimization-pack/agpm/).

Para modificar la directiva de bloqueo, usa uno de los siguientes métodos:

- En GPE, tener acceso al nivel apropiado de GPO (por ejemplo, local o de dominio) y navegar a **Equipo\\plantillas administrativas\\componentes de Components\\Remote de escritorio Services\\Remote de sesión de escritorio Host\\Connections**\\**Permitir los usuarios se conecten de forma remota mediante el uso de servicios de escritorio remoto**.  
   1. Establece la directiva en **habilitado** o **No configurado**.
   2. En los equipos afectados, abre una ventana del símbolo del sistema como administrador y ejecuta el **comando/Force gpupdate** .
- En GPM, ve a la unidad organizativa en la que se aplica la directiva de bloqueo en los equipos afectados y eliminar la directiva de la unidad organizativa.

### Comprobar el estado de los servicios RDP

En el equipo local (cliente) y el equipo remoto (de destino), deben estar ejecutando los siguientes servicios:

- Servicios de escritorio remoto (inicialización)
- Redirector de puerto de modo de usuario de servicios de escritorio remoto (UmRdpService)

Puedes usar el complemento MMC de servicios para administrar los servicios de forma local o remota. También puedes usar PowerShell local o remota (si el equipo remoto está configurado para aceptar los comandos de PowerShell remotos).

![Servicios de escritorio remoto en el complemento MMC de servicios. No se modifica la configuración predeterminada del servicio.](..\media\troubleshoot-remote-desktop-connections\RDSServiceStatus.png)

En cualquiera de los equipos, si uno o ambos servicios no se está ejecutando, iniciarlas.

> [!NOTE]  
> Si se inicia el servicio de servicios de escritorio remoto, haz clic en **Sí** para reiniciar automáticamente el servicio de Redirector de puerto de modo de usuario de servicios de escritorio remoto.

### Compruebe que está funcionando el agente de escucha RDP

> [!IMPORTANT]  
> Sigue los pasos descritos en esta sección cuidadosamente. Pueden producirse problemas graves si modificas el registro de manera incorrecta. Antes, modificarla, [copia de seguridad del registro para la restauración](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) en caso de que se producen problemas.

#### Comprobar el estado del agente de escucha RDP

Para este procedimiento, usa una instancia de PowerShell con permisos administrativos. Para un equipo local, también puedes usar un símbolo del sistema que tiene permisos administrativos. Sin embargo, este procedimiento usa PowerShell porque los mismos comandos funcionan ambos local y remota.

1. Abre una ventana de PowerShell. Para conectarse a un equipo remoto, escribe **\<computer name\> Enter-PSSession-ComputerName**.
2. Escribe **qwinsta**. 
    ![El comando qwinsta enumera los procesos que escucha en los puertos del equipo.](..\media\troubleshoot-remote-desktop-connections\WPS_qwinsta.png)
3. Si la lista incluye **rdp-tcp** con un estado de **escucha**, el agente de escucha RDP está trabajando. Continúa para [comprobar el puerto de escucha RDP](#check-the-rdp-listener-port). De lo contrario, continúa en el paso 4.
4. Exportar la configuración de agente de escucha RDP desde un equipo de trabajo.
    1. Inicia sesión en un equipo que tenga la misma versión de sistema operativo que el equipo afectado y el acceso del registro del equipo (por ejemplo, mediante el Editor del registro).
    2. Ve a la entrada del registro siguiente:  
        **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\RDP-Tcp**
    3. Exportar la entrada a un archivo. reg. Por ejemplo, en el Editor del registro, haz clic en la entrada, selecciona **Exportar**y, a continuación, escribe un nombre de archivo para la configuración exportada.
    4. Copia el archivo .reg exportado en el equipo afectado.
5. Para importar la configuración de agente de escucha RDP, abre una ventana de PowerShell que tenga permisos administrativos en el equipo afectado (o abre la ventana de PowerShell y conéctese al equipo afectado de forma remota).
   1. Para respaldar la entrada del registro existente, escribe el siguiente comando:  
   
      ```powershell  
      cmd /c 'reg export "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-tcp" C:\Rdp-tcp-backup.reg'   
      ```  
   

   2. Para quitar la entrada del registro existente, escribe los siguientes comandos:  
   
      ```powershell  
      Remove-Item -path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-tcp' -Recurse -Force  
      ```
   
   3. Para importar la nueva entrada de registro y, a continuación, reinicia el servicio, escribe los siguientes comandos:  
   
      ```powershell  
      cmd /c 'regedit /s c:\<filename>.reg'  
      Restart-Service TermService -Force  
      ```   
      donde \<filename\> es el nombre del archivo. reg exportado.
6. Probar la configuración, intentar volver a la conexión a Escritorio remoto. Si aún no se puede conectar, reinicia el equipo afectado.
7. Si aún no se puede conectar, [comprobar el estado del certificado autofirmado RDP](#check-the-status-of-the-rdp-self-signed-certificate).

#### Comprobar el estado del certificado autofirmado RDP

1. Si aún no se puede conectar, abre el complemento MMC de certificados. Cuando se le pida seleccionar el almacén de certificados para administrar, selecciona la **cuenta de equipo**y, a continuación, selecciona el equipo afectado.
2. En la carpeta **certificados** en **Escritorio remoto**, eliminar el certificado autofirmado de RDP. 
    ![Certificados de escritorio remotos en el complemento MMC certificados.](..\media\troubleshoot-remote-desktop-connections\MMCCert_Delete.png)
3. En el equipo afectado, reinicie el servicio de servicios de escritorio remoto.
4. Actualizar el complemento de certificados.
5. Si el certificado autofirmado RDP no se haya creado, [revisa los permisos de la carpeta MachineKeys](#check-the-permissions-of-the-machinekeys-folder).

#### Revisa los permisos de la carpeta MachineKeys

1. En el equipo afectado, abre el explorador y, a continuación, ve a **C:\\ProgramData\\Microsoft\\Crypto\\RSA\\**.
2. Con el botón derecho **MachineKeys**, selecciona **Propiedades**, seleccione **la seguridad**y, a continuación, seleccione **Avanzadas**.
3. Asegúrate de que se configuran los permisos siguientes:
      - Builtin\\Administrators: El control total
      - Todos los usuarios: Leer, escribir

### Comprobar el puerto de escucha RDP

En el equipo local (cliente) y el equipo remoto (de destino), el agente de escucha RDP debería estar escuchando en el puerto 3389. Ninguna otra aplicación debe estar utilizando este puerto.

> [!IMPORTANT]  
> Sigue los pasos descritos en esta sección cuidadosamente. Pueden producirse problemas graves si modificas el registro de manera incorrecta. Antes, modificarla, [copia de seguridad del registro para la restauración](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) en caso de que se producen problemas.

Para comprobar o cambiar el puerto RDP, usa el Editor del registro:

1. Selecciona el inicio, seleccione **Ejecutar**y, a continuación, especifica **regedt32**.
      - Para conectarse a un equipo remoto, selecciona el **archivo**y, a continuación, selecciona **Conectar al registro de red**.
      - En el cuadro de diálogo **Seleccionar equipo** , escribe el nombre del equipo remoto, selecciona **Comprobar nombres**y, a continuación, selecciona **Aceptar**.
2. Abra el registro y ve a **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\\<listener\>**. 
    ![La subclave númeroDePuerto para el protocolo RDP.](..\media\troubleshoot-remote-desktop-connections\RegEntry_PortNumber.png)
3. Si **númeroDePuerto** tiene un valor distinto de **3389**, cámbielo a **3389**. 
   > [!IMPORTANT]  
    > Puede trabajar con los servicios de escritorio remoto con otro puerto. Sin embargo, no recomendamos hacerlo. Solución de problemas de este tipo de configuración está fuera del ámbito de este artículo.
4. Después de cambiar el número de puerto, reinicie el servicio de servicios de escritorio remoto.

#### Comprobación de que otra aplicación no está intentando usar el mismo puerto

Para este procedimiento, usa una instancia de PowerShell con permisos administrativos. Para un equipo local, también puedes usar un símbolo del sistema que tiene permisos administrativos. Sin embargo, este procedimiento usa PowerShell porque los mismos comandos funcionan de forma local y remota.

1. Abre una ventana de PowerShell. Para conectarse a un equipo remoto, escribe **\<computer name\> Enter-PSSession-ComputerName**.
2. Escribe el siguiente comando:  
   
     ```powershell  
    cmd /c 'netstat -ano | find "3389"'  
    ```
  
    ![El comando netstat genera una lista de los puertos y los servicios de escucha a ellos.](..\media\troubleshoot-remote-desktop-connections\WPS_netstat.png)
1. Busca una entrada para el puerto TCP 3389 (o el puerto RDP asignado) con un estado de **escucha**. 
    > [!NOTE]  
   > El PID (identificador de proceso) del proceso o servicio con dicho puerto aparece en la columna PID.
1. Para determinar qué aplicación está usando el puerto 3389 (o el puerto RDP asignado), escribe el siguiente comando:  
   
     ```powershell  
    cmd /c 'tasklist /svc | find "<pid listening on 3389>"'  
    ```  
  
    ![El comando tasklist informa de los detalles de un proceso específico.](..\media\troubleshoot-remote-desktop-connections\WPS_tasklist.png)
1. Busca una entrada para el número de PID que está asociado con el puerto (de la salida de **netstat** ). Los servicios o los procesos que están asociados a ese PID aparecen en la parte derecha.
1. Si una aplicación o un servicio que no sean de servicios de escritorio remoto (TermServ.exe) usa el puerto, puede resolver los conflictos mediante uno de los siguientes métodos:
      - Configurar la otra aplicación o el servicio para usar un puerto diferente (recomendado).
      - Desinstalar la otra aplicación o servicio.
      - Configurar RDP para usar un puerto diferente y, a continuación, reinicia el servicio de servicios de escritorio remoto (no recomendado).

#### Comprueba si hay un firewall bloqueando el puerto de RDP

Usa la herramienta **psping** para comprobar si puede ponerse en el equipo afectado a través del puerto 3389.

1. En un equipo que sea diferente del equipo afectado, descargar **psping** desde <https://live.sysinternals.com/psping.exe>.
2. Abre una ventana del símbolo del sistema como administrador, cambie al directorio en el que instalaste **psping**y, a continuación, escribe el siguiente comando:  
   
   ```  
   psping -accepteula <computer IP>:3389  
   ```
   
3. Comprobar el resultado del comando **psping** para obtener resultados como los siguientes:  
      - **Conectarse a \<computer IP\>**: el equipo remoto es accesible.
      - **(0% perdidos)**: si se realizó correctamente en todos los intentos de conexión.
      - **El equipo remoto ha rechazado la conexión de red**: el equipo remoto no es accesible.
      - **(pérdida de 100%)**: error en todos los intentos de conexión.
1. Ejecutar **psping** en varios equipos para probar su capacidad para conectar con el equipo afectado.
1. Ten en cuenta si el equipo afectado bloquea las conexiones desde todos los demás equipos, algunos otros equipos o un solo equipo.
1. Recomienda los pasos siguientes:
      - Atraer a los administradores de red para comprobar que la red permite el tráfico RDP en el equipo afectado.
      - Investigar las configuraciones de firewalls entre los equipos de origen y el equipo afectado (incluido el Firewall de Windows en el equipo afectado) para determinar si un firewall está bloqueando el puerto de RDP.

## No se puede conectar cliente, "Clase no registrada"

Al intentar conectarse a un equipo remoto mediante el uso de un cliente que se está ejecutando Windows 10, versión 1709 o posterior, el cliente no puede conectarse mientras el servidor Host de sesión de escritorio remoto informa de un mensaje que contiene el código de error "Clase no registrada (0 x 80040154)".

Este problema se produce si el usuario que está intentando conectarse tiene un perfil de usuario obligatorios. Para resolver este problema, instale el [24 de julio de 2018: KB4338817 (16299.579 de compilación del sistema operativo)](https://support.microsoft.com/en-us/help/4338817/windows-10-update-kb4338817) actualización de Windows 10.

## No se puede conectar cliente, no hay licencias disponibles

Esta situación se aplica a las implementaciones que incluyen un servidor RDSH y un servidor de licencias de escritorio remoto.

Identifica qué tipo de comportamiento de los usuarios ven:

- La sesión se desconectó porque no hay ninguna licencia disponible o no hay disponible ningún servidor de licencia
- Se denegó el acceso debido a un error de seguridad

Inicia sesión en el Host de sesión de escritorio remoto como un administrador de dominio y abre el Diagnoser de licencia de escritorio remoto. Buscar mensajes, como los siguientes:

  - El período de gracia para el control remoto de servidor de Host de sesión de escritorio ha expirado, pero el servidor Host de sesión de escritorio remoto no se ha configurado con los servidores de licencias. conexiones con el servidor Host de sesión de escritorio remoto se denegarán a menos que se configura un servidor de licencias para el servidor Host de sesión de escritorio remoto.
  - Licencia server \<computer name\> no está disponible. Esto puede deberse a problemas de conectividad de red, se detiene el servicio de licencias de escritorio remoto en el servidor de licencias o licencias de escritorio remoto ya no está instalada en el equipo.

Estos problemas suelen estar asociado con los siguientes mensajes de usuario:

  - La sesión remota se desconectó porque no hay ningún licencias de acceso de cliente de escritorio remoto disponibles para este equipo.
  - La sesión remota se desconectó porque no hay ningún servidor de licencias de escritorio remoto disponibles para proporcionar una licencia.

En este caso, [Configurar el servicio de licencias de escritorio remoto](#configure-the-rd-licensing-service).

Si el Diagnoser de licencia de escritorio remoto se enumeran otros problemas, como "el componente de protocolo RDP X.224 detectó un error en el flujo de protocolos y ha desconectado el cliente," puede ser un problema que afecta a los certificados de licencia. Estos problemas suelen ser asociados con los mensajes de usuario, como los siguientes:

Debido a un error de seguridad, el cliente no pudo conectarse al servidor de Terminal. Después de asegurarse de que has iniciado sesión en la red, intenta volver a conectar al servidor.

En este caso, [las claves del registro de certificados de actualización del X509](#refresh-the-x509-certificate-registry-keys).

### Configurar el servicio de licencias de escritorio remoto

El siguiente procedimiento usa el administrador del servidor para realizar los cambios de configuración. Para obtener información sobre cómo configurar y usar el administrador del servidor, consulta el [Administrador del servidor](https://docs.microsoft.com/en-us/windows-server/administration/server-manager/server-manager).

1. Abre el administrador del servidor y, a continuación, ve a **Servicios de escritorio remoto**.
2. **Introducción a la implementación**, selecciona **las tareas**y, a continuación, selecciona **Editar las propiedades de implementación**.
3. Selecciona las **Licencias de escritorio remoto**y, a continuación, selecciona el modo de licencia adecuado para la implementación (**Por dispositivo** o **Por usuario**).
4. Escribe el nombre de dominio completo (FQDN) de tu servidor de licencias de escritorio remoto y, a continuación, selecciona **Agregar**.
5. Si tienes más de un servidor de licencias de escritorio remoto, repite el paso 4 para cada servidor. 
    ![Opciones de configuración del servidor de licencias de escritorio remoto en el administrador del servidor.](..\media\troubleshoot-remote-desktop-connections\RDLicensing_Configure.png)

### Las claves del registro de certificados de actualización del X509

> [!IMPORTANT]  
> Sigue los pasos descritos en esta sección cuidadosamente. Pueden producirse problemas graves si modificas el registro de manera incorrecta. Antes, modificarla, [copia de seguridad del registro para la restauración](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) en caso de que se producen problemas.

Para resolver este problema, copia de seguridad y, a continuación, las claves del registro de certificados de quitar el X509, reinicia el equipo y, a continuación, el servidor de licencias de reactivar el escritorio remoto. Para ello, sigue estos pasos.

> [!NOTE]  
> Realizar el procedimiento siguiente en cada uno de los servidores RDSH.

1. Abre el Editor del registro y navegar a **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\RCM**.
2. En el menú del registro, seleccione **Exportar el archivo de registro**.
3. Escribe el **Certificado exportado** en el cuadro de **nombre de archivo** y, a continuación, selecciona **Guardar**.
4. Haz clic en cada uno de los siguientes valores, seleccione **Eliminar**y, a continuación, seleccione **Sí** para comprobar la eliminación:  
      - **Certificado**
      - **X509 de certificados**
      - **Id. de certificado de X509**
      - **X509 Certificate2**
5. Salir del Editor del registro y, a continuación, reinicia el servidor RDSH.

## Usuario no se puede autenticar o debe autenticarse dos veces

Hay varios problemas que pueden causar problemas que afectan a la autenticación de usuario. Esta sección abordan los siguientes casos:

  - [Un usuario se deniega el acceso con un mensaje "restringidos el tipo de inicio de sesión"](#access-denied-restricted-type-of-logon)
  - [Un usuario o una aplicación se deniega el acceso con el evento 16965, "una llamada remota a la base de datos SAM se ha denegado"](#access-denied-a-remote-call-to-the-sam-database-has-been-denied)
  - [Un usuario no puede iniciar sesión con una tarjeta inteligente](#a-user-cannot-sign-in-by-using-a-smart-card)
  - [Si el equipo remoto está bloqueado, un usuario necesita que escriba una contraseña de dos veces](#if-the-remote-pc-is-locked-a-user-needs-to-enter-a-password-twice)
  - [Un usuario no puede iniciar sesión y recibe "autenticación mensajes de error" y "Corrección de CredSSP cifrado oracle"](#user-cannot-sign-in-and-receives-authentication-error-and-credssp-encryption-oracle-remediation-messages)
  - [Después de actualizar los equipos cliente, algunos usuarios que iniciar sesión en dos veces](#after-you-update-client-computers-some-users-need-to-sign-in-twice).
  - [Los usuarios se deniegan el acceso en una implementación que usa RemoteGuard con varios agentes de conexión a Escritorio remoto](#users-are-denied-access-on-a-deployment-that-uses-remote-credential-guard-with-multiple-rd-connection-brokers)

### Acceso denegado, tipo restringido de inicio de sesión

En esta situación, un usuario de Windows 10 que intentan conectarse a los equipos de Windows 10 o Windows Server 2016 se deniega el acceso con el siguiente mensaje:

> Conexión a Escritorio remoto:  
> El administrador del sistema restringió el tipo de inicio de sesión (red o interactivo) que pueda usar. Para obtener ayuda, ponte en contacto con el administrador del sistema o el soporte técnico.

Este problema se produce cuando la autenticación de nivel de red (NLA) es necesario para las conexiones RDP y el usuario no es un miembro del grupo de **Usuarios de escritorio remoto** . También puede ocurrir si el grupo de **Usuarios de escritorio remoto** no se ha asignado para el derecho de usuario de **acceso a este equipo desde la red** .

Para solucionar este problema, sigue uno de estos pasos:

  - [Modificar la pertenencia a grupos del usuario o la asignación de derechos de usuario](#modify-the-users-group-membership-or-user-rights-assignment).
  - Desactivar NLA (no recomendado).
  - Usar a los clientes de escritorio remoto que no sean de Windows 10. Por ejemplo, los clientes de Windows 7 no tienen este problema.

#### Modificar la pertenencia a grupos del usuario o la asignación de derechos de usuario

Si este problema afecta a un solo usuario, la solución más sencilla para este problema es agregar el usuario al grupo de **Usuarios de escritorio remoto** .

Si el usuario ya es un miembro de este grupo (o si varios miembros del grupo tienen el mismo problema), comprueba la configuración de derechos de usuario en el equipo remoto de Windows 10 o Windows Server 2016.

1. Abre el Editor de objeto de directiva de grupo (GPE) y conéctate a la directiva local del equipo remoto.
2. Ve a la **Asignación de equipos equipo\\configuración de windows\\configuración Settings\\Configuración Policies\\User derechos**, haz clic en el **acceso a este equipo desde la red**y, a continuación, selecciona **Propiedades**.
3. Compruebe la lista de usuarios y grupos de **Usuarios de escritorio remoto** (o un grupo primario).
4. Si la lista no incluye **Usuarios de escritorio remoto** (o un grupo de elemento primario, como **todos los usuarios**) necesita para agregarla a la lista (si tienes más de uno o dos equipos en la implementación, usa un objeto de directiva de grupo).  
    Por ejemplo, la pertenencia de forma predeterminada para **tener acceso a este equipo desde la red** incluye **todo el mundo**. Si la implementación usa un objeto de directiva de grupo para quitar **todos los usuarios**, puede que tengas restaurar el acceso actualizando el objeto de directiva de grupo para agregar **Usuarios de escritorio remoto**.

### Acceso denegado, se ha denegado una llamada remota a la base de datos SAM

Este comportamiento es más probable que ocurra si los controladores de dominio ejecutan Windows Server 2016 o posterior, y los usuarios intentan conectarse mediante el uso de una aplicación de conexión personalizada. En concreto, las aplicaciones que tienen acceso a información de perfil del usuario en Active Directory se denegarán el acceso.

Este comportamiento da como resultado de un cambio a Windows. En Windows Server 2012 R2 y versiones anteriores versiones, cuando un usuario inicia sesión en un escritorio, contactos de administrador de conexión remota (equipo) remoto el controlador de dominio (DC) para consultar las configuraciones que son específicas de escritorio remoto en el objeto de usuario de dominio de Active Directory Services (AD DS). Esta información se muestra en la pestaña de perfil de servicios de escritorio remoto de propiedades del objeto del usuario en el complemento MMC de equipos y usuarios de Active Directory.

A partir de Windows Server 2016, equipo ya no consulta el objeto de los usuarios en AD DS. Si necesita que el equipo consultar AD DS porque estás usando los atributos de servicios de escritorio remoto, debes habilitar manualmente el comportamiento de equipo anterior.

> [!IMPORTANT]  
> Sigue los pasos descritos en esta sección cuidadosamente. Pueden producirse problemas graves si modificas el registro de manera incorrecta. Antes, modificarla, [copia de seguridad del registro para la restauración](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) en caso de que se producen problemas.

Para habilitar el comportamiento heredado de equipo en un servidor Host de sesión de escritorio remoto, configurar las siguientes entradas del registro y, a continuación, reinicia el servicio de **Servicios de escritorio remoto** :  
  - **Servicios de NT\\Terminal HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Policies\\Microsoft\\Windows**
  - **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\\<Winstation name\>\\**  
      - Nombre: **fQueryUserConfigFromDC**
      - Tipo: **Reg\_DWORD**
      - Valor: **1** (Decimal)

Para habilitar el comportamiento heredado de equipo en un servidor que no sean de un servidor Host RD, configurar estas entradas del registro y la siguiente entrada de registro adicionales (y, a continuación, reinicia el servicio):
  - **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server**

Para obtener más información acerca de este comportamiento, vea KB 3200967 [cambios de Connection Manager para remoto en Windows Server](https://support.microsoft.com/en-us/help/3200967/changes-to-remote-connection-manager-in-windows-server).

### Un usuario no puede iniciar sesión con una tarjeta inteligente

Este artículo trata tres circunstancias comunes en los que un usuario no puede iniciar sesión un equipo de escritorio remoto mediante el uso de una tarjeta inteligente:  
  - [El usuario no puede iniciar sesión en una sucursal que usa un solo lectura controlador de dominio (RODC)](#cannot-sign-in-with-a-smart-card-in-a-branch-office-with-a-rodc)
  - [El usuario no puede iniciar sesión en un equipo de Windows Server 2008 SP2 que se ha actualizado recientemente](#cannot-sign-in-with-a-smart-card-to-a-windows-server-2008-sp2-computer)
  - [El usuario no puede mantener la sesión en un equipo de Windows que se ha actualizado recientemente, y el servicio de servicios de escritorio remoto se convierte en no responde (se bloquea)](#cannot-stay-signed-in-with-a-smart-card-and-remote-desktop-services-service-hangs)

#### No se puede iniciar sesión con una tarjeta inteligente en una sucursal con un RODC

Este problema se produce en las implementaciones que incluyen un servidor RDSH en un sitio de sucursales que usa un RODC. El servidor RDSH está alojado en el dominio raíz. Los usuarios en el sitio de rama pertenecen a un dominio secundario y usan tarjetas inteligentes para la autenticación. El RODC está configurado en caché las contraseñas de usuario (RODC pertenece al **Grupo de replicación de contraseñas de RODC permitido**). Cuando los usuarios intenten iniciar sesión en sesiones en el servidor RDSH, recibir mensajes como "el inicio de sesión intentado no es válido. Esto es debido a una información de autenticación o nombre de usuario incorrecto".

Este es un problema conocido en la forma en que la raíz del controlador de dominio y el RODC administran el cifrado de las credenciales de usuario. La raíz del controlador de dominio usa una clave de cifrado para cifra las credenciales y el RODC proporciona una clave de descifrado al cliente. Sin embargo, las dos claves no coinciden.

Para evitar este problema, considera la posibilidad de cambiar la topología de controlador de dominio, cómo desactivar el almacenamiento en caché de contraseña en el RODC o implementar un controlador de dominio puede escribir en el sitio de rama. Es un método alternativo mover el servidor RDSH al mismo dominio secundario que los usuarios. Otra alternativa es permitir que los usuarios iniciar sesión sin tarjetas inteligentes. Cualquiera de estas soluciones puede requerir compromisos en el nivel de seguridad o rendimiento.

#### No se puede iniciar sesión con una tarjeta inteligente a un equipo de Windows Server 2008 SP2

Este problema se produce cuando los usuarios inician sesión en un equipo de Windows Server 2008 SP2 que se ha actualizado con KB4093227 (2018.4B). Cuando los usuarios intenten iniciar sesión con una tarjeta inteligente, se les deniega el acceso a mensajes como "No se encuentra certificados válidos. Comprobar que la tarjeta se inserta correctamente y se ajusta estrechamente". Al mismo tiempo, el equipo de Windows Server registra el evento de la aplicación "se produjo un error al recuperar un certificado digital de la tarjeta inteligente insertada. Firma no válida."

Para resolver este problema, actualiza el equipo de Windows Server con la 2018.06Bre-versión de 4093227 KB, [Descripción de la actualización de seguridad para el protocolo de escritorio remoto (RDP) vulnerabilidad de denegación de servicio en Windows Server 2008: 10 de abril de 2018](https://support.microsoft.com/en-us/help/4093227/security-update-for-vulnerabilities-in-windows-server-2008).

#### No se puede mantener la sesión con una tarjeta inteligente y bloqueos de servicio de servicios de escritorio remoto

Este problema se produce cuando los usuarios iniciar sesión en un equipo de Windows o Windows Server que se ha actualizado con 4056446 KB. En primer lugar, el usuario podrá iniciar sesión en el sistema mediante el uso de una tarjeta inteligente, pero, a continuación, recibe un mensaje de error "SCARD\_E\_NO\_SERVICE". Es posible que responda al equipo remoto.

Para evitar este problema, reinicia el equipo remoto.

Para resolver este problema, actualiza el equipo remoto con la solución apropiada:

  - Windows Server 2008 SP2: KB 4090928, [controladores en el proceso de lsm.exe la pérdida de Windows y aplicaciones de tarjeta inteligente pueden mostrar errores de "SCARD\_E\_NO\_SERVICE"](https://support.microsoft.com/en-us/help/4090928/scard-e-no-service-errors-when-windows-leaks-handles-in-the-lsm-exe)
  - Windows Server 2012 R2: KB 4103724, [17 de mayo de 2018: KB4103724 (vista previa del paquete acumulativo de actualizaciones mensuales)](https://support.microsoft.com/en-us/help/4103724/windows-81-update-kb4103724)
  - Windows Server 2016 y Windows 10, versión 1607: 4103720 KB, [17 de mayo de 2018: KB4103720 (14393.2273 de compilación del sistema operativo)](https://support.microsoft.com/en-us/help/4103720/windows-10-update-kb4103720)

### Si el equipo remoto está bloqueado, un usuario necesita que escriba una contraseña de dos veces

Este problema puede ocurrir cuando un usuario intenta conectarse a un equipo de escritorio remoto que ejecuta Windows 10, versión 1709 en una implementación en la que las conexiones RDP no requieren NLA. En estas condiciones, si ha bloqueado el escritorio remoto, el usuario tiene que escribir sus credenciales dos veces al conectarse.

Para resolver este problema, actualiza el equipo de la versión 1709 de Windows 10 con 4343893 KB, [30 de agosto de 2018: KB4343893 (16299.637 de compilación del sistema operativo)](https://support.microsoft.com/en-us/help/4343893/windows-10-update-kb4343893).

### Usuario no puede iniciar sesión y recibe "autenticación mensajes de error" y "Corrección de CredSSP cifrado oracle"

Cuando los usuarios intenten iniciar sesión con cualquier versión de Windows en Windows Vista SP2 y versiones posteriores o Windows Server 2008 SP2 y versiones posteriores, que se deniegan el acceso y reciban mensajes, como los siguientes:

  - Se ha producido un error de autenticación. No se admite la función solicitada.
  - Esto puede deberse a la corrección de oracle cifrado CredSSP

"Corrección de oracle de cifrado CredSSP" hace referencia a un conjunto de actualizaciones de seguridad que se publicó en marzo y abril, mayo de 2018. CredSSP es un proveedor de autenticación que procesa las solicitudes de autenticación para otras aplicaciones. El 13 de marzo de 2018, "3B" y las actualizaciones posteriores solucionado una vulnerabilidad de seguridad en el que un atacante podría transmitir las credenciales de usuario para ejecutar código en el sistema de destino.

Las actualizaciones iniciales agregan compatibilidad para un nuevo objeto de directiva de grupo, **Corrección de Oracle de cifrado**, que tiene los siguientes valores posibles:

  - **Vulnerable**. Las aplicaciones de cliente que usan CredSSP pueden retroceder a versiones no seguras, pero este comportamiento expone los escritorios remotos a los ataques. Servicios que usan CredSSP aceptan a los clientes que no se han actualizado.
  - **Mitigado**. Aplicaciones cliente que utilizan CredSSP no se vuelven a versiones no seguras, pero los servicios que usan CredSSP aceptan a los clientes que no se han actualizado.
  - **Fuerza actualiza los clientes**. Las aplicaciones de cliente que usan CredSSP no se vuelven a versiones no seguras y servicios que usan CredSSP no aceptará a los clientes. 
    Nota: Esta configuración no se pueden implementar hasta que todos los hosts remotos compatible con la versión más reciente.

La actualización de 8 de mayo de 2018 había cambiado la configuración de **Corrección de Oracle de cifrado** predeterminada de **Vulnerable** a **mitigado**. Con este cambio en su lugar, los clientes de escritorio remoto que tienen las actualizaciones no se pueden conectar a los servidores que no tienen (o actualizan los servidores que no se haya reiniciado). Para obtener más información acerca de los efectos de las actualizaciones y los tipos de comunicación que bloquean, consulta 4093492 KB, [actualizaciones CredSSP CVE-2018-0886](https://support.microsoft.com/en-us/help/4093492/credssp-updates-for-cve-2018-0886-march-13-2018).

Para resolver este problema, asegúrese de que todos los sistemas son totalmente actualizados y reiniciarse. Para obtener una lista completa de las actualizaciones y obtener más información acerca de las vulnerabilidades, consulta [CVE-2018-0886 | Vulnerabilidad de ejecución de código de control remoto CredSSP](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-0886).

Para evitar este problema hasta que se completan las actualizaciones, comprueba 4093492 KB para los tipos de aplicaciones permitidos de conexiones. Si no hay ninguna alternativa factible considerar uno de los siguientes métodos:

  - Para los equipos cliente afectado, Establece la directiva de **Cifrado Oracle corrección** a **Vulnerable**.
  - Modificar las siguientes directivas en la carpeta de directiva de grupo de **Equipo\\plantillas administrativas\\componentes de Components\\Remote de escritorio Services\\Remote de sesión de escritorio Host\\Security** :  
      - **Requerir el uso de la capa de seguridad específica para las conexiones remotas de (RDP)**: establece como **habilitada** y seleccione **RDP**.
      - **Requerir la autenticación de usuario para las conexiones remotas mediante autenticación a nivel de red**: establecido como **deshabilitado**.
      > [!IMPORTANT]  
      > Estas modificaciones reducen la seguridad de la implementación. Solo debe temporales, si se usan en absoluto.

Para obtener más información sobre cómo trabajar con la directiva de grupo, consulta la [modificación de un GPO de bloqueo](#modifying-a-blocking-gpo).

### Después de actualizar los equipos cliente, algunos usuarios que iniciar sesión en dos veces

Los usuarios en alguna versión de Windows 7 o Windows 10 1709 inicia sesión en una sesión de escritorio remoto y ver inmediatamente una segunda pantalla de inicio de sesión. Este problema se produce si los equipos cliente han recibido las actualizaciones siguientes:

  - Windows 7: KB 4103718, [8 de mayo de 2018: KB4103718 (paquete acumulativo de actualizaciones mensuales)](https://support.microsoft.com/en-us/help/4103718/windows-7-update-kb4103718)
  - Windows 10 1709: KB 4103727, [8 de mayo de 2018: KB4103727 (compilación del SO 16299.431)](https://support.microsoft.com/en-us/help/4103727/windows-10-update-kb4103727)

Para resolver este problema, asegúrese de que los equipos que desean que los usuarios conectarse (así como los servidores RDSH o RDVI) se actualizan completamente a través de junio de 2018. Esto incluye las actualizaciones siguientes:

  - Windows Server 2016: KB 4284880, [12 de junio de 2018: KB4284880 (compilación del SO 14393.2312)](https://support.microsoft.com/en-us/help/4284880/windows-10-update-kb4284880)
  - Windows Server 2012 R2: KB 4284815, [12 de junio de 2018: KB4284815 (paquete acumulativo de actualizaciones mensuales)](https://support.microsoft.com/en-us/help/4284815/windows-81-update-kb4284815)
  - Windows Server 2012: KB 4284855, [12 de junio de 2018: KB4284855 (paquete acumulativo de actualizaciones mensuales)](https://support.microsoft.com/en-us/help/4284855/windows-server-2012-update-kb4284855)
  - Windows Server 2008 R2: KB 4284826, [12 de junio de 2018: KB4284826 (paquete acumulativo de actualizaciones mensuales)](https://support.microsoft.com/en-us/help/4284826/windows-7-update-kb4284826)
  - Windows Server 2008 Service Pack 2: KB4056564, [Descripción de la actualización de seguridad para la vulnerabilidad de ejecución de código remoto CredSSP en Windows Server 2008, Windows Embedded POSReady 2009 y 2009 estándar de Windows Embedded: 13 de marzo de 2018](https://support.microsoft.com/en-us/help/4056564/security-update-for-vulnerabilities-in-windows-server-2008)

### Los usuarios se deniegan el acceso en una implementación que usa a Credential Guard remoto con varios agentes de conexión a Escritorio remoto

Este problema se produce en las implementaciones de alta disponibilidad que usan a dos o más remoto escritorio agentes de conexión, si remoto Credential Guard de Windows Defender está en uso. Los usuarios no pueden iniciar sesión en equipos de escritorio remotos.

Este problema se produce porque Credential Guard remoto utiliza Kerberos para la autenticación y restringe NTLM. Sin embargo, en una configuración de alta disponibilidad con equilibrio de carga, los agentes de conexión a Escritorio remoto no se admiten las operaciones de Kerberos.

Si necesitas usar una configuración de alta disponibilidad con agentes de conexión a Escritorio remoto con equilibrio de carga, puede evitar este problema al deshabilitar Credential Guard remoto. Para obtener más información sobre cómo administrar a remoto Credential Guard de Windows Defender, consulta [las credenciales de escritorio remoto proteger con remoto Credential Guard de Windows Defender](https://docs.microsoft.com/en-us/windows/security/identity-protection/remote-credential-guard#enable-windows-defender-remote-credential-guard).

## Acerca de cómo conectar, el usuario recibe mensaje "El servicio de escritorio remoto está actualmente ocupado"

Para determinar una respuesta adecuada a este problema, utilice los siguientes pasos:

1. ¿El servicio de servicios de escritorio remoto no responde (por ejemplo, el cliente de escritorio remoto aparece "bloqueo" en la pantalla de bienvenida).  
      - Si el servicio deja de responder, consulta el [problema de memoria del servidor RDSH](#rdsh-server-memory-issue).
      - Si parece que el cliente puede interactuar con el servicio normalmente, continúe con el paso siguiente.
2. ¿Si uno o varios usuarios desconectarán sus sesiones de escritorio remoto, pueden a los usuarios conectarse de nuevo?  
   - Si el servicio continúa denegar conexiones independientemente de cuántos usuarios desconectarán sus sesiones, consulta el [problema de agente de escucha de escritorio remoto](#rd-listener-issue).
   - Si el servicio comienza a aceptar las conexiones de nuevo después de un número de usuarios que desconectado sus sesiones, [comprueba la directiva de límite de conexión](#check-the-connection-limit-policy).

### Problema de memoria del servidor RDSH

Se ha encontrado una fuga de memoria en algunos servidores de Windows Server 2012 R2 RDSH. Con el tiempo, estos servidores comienzan a no permitir conexiones a Escritorio remoto e inicios de sesión de consola local con los mensajes, como los siguientes:

> La tarea que estás intentando no se puede completar porque el servicio de escritorio remoto está ocupado. Vuelve a intentarlo en unos minutos. Otros usuarios aún deben ser capaz de iniciar sesión.

Los clientes de escritorio remoto intentan conectarse también dejan de responder.

Para evitar este problema, reinicie el servidor RDSH.

Para resolver este problema, aplicar 4093114 KB, [10 de abril de 2018: KB4093114 (paquete acumulativo de actualizaciones mensuales)] (file:///C:\\Users\\v-jesits\\AppData\\Local\\Microsoft\\Windows\\INetCache\\Content.Outlook\\FUB8OO45\\April%2010,%202018—KB4093114%20\(Monthly% 20Rollup\)), a los servidores RDSH.

### Problema de agente de escucha de escritorio remoto

Se ha detectado un problema en algunos servidores RDSH que se han actualizado directamente desde Windows Server 2008 R2 a Windows Server 2012 R2 o Windows Server 2016. Cuando un cliente de escritorio remoto se conecta al servidor RDSH, del servidor RDSH crea un agente de escucha de escritorio remoto para la sesión del usuario. Los servidores afectados mantén un recuento de los agentes de escucha de escritorio remoto que aumenta a medida que los usuarios se conectan, pero nunca se reduce.

Para evitar este problema, puedes usar los siguientes métodos:

  - Reiniciar el servidor RDSH para restablecer el recuento de los agentes de escucha de escritorio remoto
  - Modificar la directiva de límite de conexión, si se establece en un valor muy grande. Para obtener más información acerca de cómo administrar la directiva de límite de conexión, consulta la [comprobación de la directiva de límite de conexión](#check-the-connection-limit-policy).

Para resolver este problema, se aplican las actualizaciones siguientes para los servidores RDSH:

  - Windows Server 2012 R2: KB 4343891, [30 de agosto de 2018: KB4343891 (vista previa del paquete acumulativo de actualizaciones mensuales)](https://support.microsoft.com/en-us/help/4343891/windows-81-update-kb4343891)
  - Windows Server 2016: KB 4343884, [30 de agosto de 2018: KB4343884 (compilación del SO 14393.2457)](https://support.microsoft.com/en-us/help/4343884/windows-10-update-kb4343884)

### Comprobar la directiva de límite de conexión

Puedes establecer el límite en el número de conexiones a Escritorio remoto simultáneas en el nivel de equipo individual o mediante la configuración de un objeto de directiva de grupo (GPO). De manera predeterminada, no se establece el límite.

Para comprobar la configuración actual e identificar los GPO existentes en el servidor RDSH, abre una ventana del símbolo del sistema como administrador y escribe el siguiente comando:
  
```
gpresult /H c:\gpresult.html
```
   
Después de que finalice este comando, abre gpresult.html y en **Equipo\\plantillas administrativas\\componentes de Components\\Remote de escritorio Services\\Remote de sesión de escritorio Host\\Connections**, busca el **limitar el número de conexiones** Directiva.

  - Si la configuración de esta directiva está **deshabilitada**, la directiva de grupo no limita conexiones RDP.
  - Si la configuración de esta directiva está **habilitado**, a continuación, comprueba **Ganando GPO**. Si necesitas quitar o cambiar el límite de conexión, editar este GPO.

Para aplicar los cambios en la directiva, abre una ventana del símbolo del sistema en el equipo afectado y escribe el siguiente comando:
  
```
gpupdate /force
```
  
## Cliente de escritorio remoto se desconecta y no se puede conectar a la misma sesión

Después de cliente de escritorio remoto pierde la conexión a Escritorio remoto, el cliente no puede volver a conectar inmediatamente. El usuario recibe mensajes de error, como los siguientes:

  - Debido a un error de seguridad, el cliente no pudo conectarse al servidor de Terminal. Después de asegurarse de que has iniciado sesión en la red, intenta volver a conectar al servidor.
  - Escritorio remoto desconectado. Debido a un error de seguridad, el cliente no pudo conectarse al equipo remoto. Comprueba que se registran en la red y, a continuación, intenta volver a conectar.

En un momento posterior, vuelve a conectar el cliente de escritorio remoto para el escritorio remoto. Sin embargo, en lugar de conexión del cliente a la sesión original, del servidor RDSH conecta al cliente a una nueva sesión. Comprobación del servidor RDSH revela que la sesión original no especifica un estado desconectado, pero en su lugar permanece activo.

Para evitar este problema, puedes habilitar la directiva de **intervalo de conexión persistente de configurar** en los **equipo\\plantillas administrativas\\componentes de Components\\Remote de escritorio Services\\Remote de sesión de escritorio Host\\Connections **carpeta de directiva de grupo. Si habilita esta directiva, debes escribir un intervalo de mantenimiento. El intervalo persistente determina la frecuencia, en minutos, el servidor comprueba el estado de sesión.

Este problema puede deberse a errores de configuración de las autenticación y opciones de configuración. Puedes configurar estas opciones de configuración en el nivel de servidor o mediante el uso de los GPO. La configuración de directiva de grupo está disponible en la carpeta de directiva de grupo de **Equipo\\plantillas administrativas\\componentes de Components\\Remote de escritorio Services\\Remote de sesión de escritorio Host\\Security** .

1. En el servidor Host de sesión de escritorio remoto, abre la configuración de Host de sesión de escritorio remoto.
2. En **las conexiones**, haz clic en el nombre de la conexión y, a continuación, haga clic en **Propiedades**.
3. En el cuadro de diálogo de **Propiedades** para la conexión, en la pestaña **General** , en el nivel de **seguridad** , selecciona un método de seguridad.
4. En el **nivel de cifrado**, haz clic en el nivel que quieras. Puedes seleccionar **bajo**, **Cliente Compatible**, **alto**o **Compatible con FIPS**.

> [!NOTE]  
>  - Cuando las comunicaciones entre clientes y servidores Host de sesión de escritorio remoto requieren el máximo nivel de cifrado, utilice el cifrado compatible con FIPS.
>  - Cualquier configuración de nivel de cifrado que se configura en la directiva de grupo invalida la configuración que se establece mediante la herramienta de configuración de servicios de escritorio remoto. Además, si se habilita el [criptografía de sistema: usar compatible con algoritmos FIPS para cifrado, firma y operaciones hash](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc780081\(v=ws.10\)) directiva, esta opción reemplaza la directiva de **establecer el nivel de cifrado de conexión de cliente** . La directiva de criptografía del sistema está en la carpeta del **Equipo equipo\\configuración seguridad Settings\\Configuración Policies\\Security opciones** .
>  - Cuando se cambia el nivel de cifrado, el nuevo nivel de cifrado surte efecto la próxima vez que un usuario inicia sesión. Si necesita varios niveles de cifrado en un servidor, instale a varios adaptadores de red y configurar cada adaptador por separado.
>  - Para comprobar que el certificado tenga una clave privada correspondiente, en configuración de servicios de escritorio remoto, haz clic en la conexión para la que quieres ver el certificado, seleccione **General**, selecciona **Editar**, selecciona el certificado que quieras ver y, a continuación, selecciona **Ver certificado**. En la parte inferior de la pestaña **General** , debería aparecer la instrucción "tiene una clave privada que corresponde a este certificado". También puedes ver esta información mediante el complemento de certificados.
>  - Cifrado compatible con FIPS (el **criptografía de sistema: usar compatible con algoritmos FIPS para cifrado, firma y operaciones hash** directiva o la configuración **Compatible con FIPS** en configuración del servidor de escritorio remoto) cifra y descifra los datos enviados desde el cliente al servidor y desde el servidor al cliente, con los algoritmos de cifrado de 140-1 Federal Information Processing Standard (FIPS), mediante los módulos criptográficos de Microsoft. Para obtener más información, consulta [FIPS 140 validación](https://docs.microsoft.com/en-us/windows/security/threat-protection/fips-140-validation).
>  - La opción de **alta** cifra los datos enviados desde el cliente al servidor y desde el servidor al cliente mediante el uso de cifrado de 128 bits.
>  - La configuración de **Cliente Compatible** cifra los datos enviados entre el cliente y el servidor en la intensidad de clave máxima admitida por el cliente.
>  - La configuración de **baja** cifra los datos enviados desde el cliente al servidor mediante el cifrado de 56 bits.

## Portátil remoto se desconecta de la red inalámbrica

Este problema puede ocurrir cuando un cliente de escritorio remoto se conecta a un equipo portátil mediante el uso de una red inalámbrica 802.1X. De forma intermitente, el equipo portátil se desconecta de la red inalámbrica y no volver a conectar automáticamente.

Se trata de un problema conocido que se produce cuando la configuración de autenticación de red para la conexión de red inalámbrica es la **autenticación de usuario**.

Para evitar este problema, Establece la configuración de autenticación de red para el **equipo**o la **autenticación de usuario o equipo** .

 > [!NOTE]  
> Para cambiar la configuración de autenticación de red en un solo equipo, debes usar el panel de control de centro de redes y para crear una nueva conexión inalámbrica con la nueva configuración.

Para obtener una descripción completa de cómo configurar las opciones de red inalámbrica mediante los GPO, consulta [las directivas de configurar la red inalámbrica (IEEE 802.11)](https://docs.microsoft.com/en-us/windows-server/networking/core-network-guide/cncg/wireless/e-wireless-access-deployment#bkmk_policies).

## Problemas de la aplicación o un rendimiento deficiente de experiencias de usuario

Esta sección abordan varios problemas comunes que los usuarios pueden experimentar al usar la funcionalidad de escritorio remota:

  - [Problemas de usuarios que se conecten a las máquinas virtuales nuevas Microsoft Azure de forma intermitente](#intermittent-problems-with-new-microsoft-azure-virtual-machines).
  - [Los usuarios que se conecten a equipos de escritorio remotos de Windows 10 versión 1709 pueden tener problemas para reproducir vídeo](#video-playback-issues-on-windows-10-version-1709).
  - [Los usuarios con perfiles de usuario de solo lectura que se conectan a los equipos de escritorio remotos de Windows 10 no pueden compartir sus equipos de escritorio.](#desktop-sharing-issues-on-windows-10)
  - [Los usuarios se conectan a escritorios remotos de Windows 10 con los clientes que se ejecutan en versiones anteriores de Windows 10 experimentan un rendimiento deficiente cuando está deshabilitado NLA](#performance-issues-when-mixing-versions-of-windows-10-if-nla-is-disabled)
  - [Cuando los usuarios ejecutan varias aplicaciones en sesiones de escritorio remoto y desconectan y volver a conectar con frecuencia, puede producir una pantalla negra en volver a conectar.](#black-screen-issue)

### Problemas repetitivos con nuevas máquinas virtuales de Microsoft Azure

Este problema afecta a máquinas virtuales que se han aprovisionado recientemente. Después de que el usuario se conecta a la máquina virtual, la sesión de escritorio remoto no puede cargar todos los la configuración del usuario correctamente.

Para evitar este problema, desconecta de la máquina virtual, esperar al menos 20 minutos y, a continuación, vuelve a conectar.

Para resolver este problema, se aplican las actualizaciones siguientes para las máquinas virtuales, según corresponda:

  - Windows 10 y Windows Server 2016: KB 4343884, [30 de agosto de 2018: KB4343884 (compilación del SO 14393.2457)](https://support.microsoft.com/en-us/help/4343884/windows-10-update-kb4343884)
  - Windows Server 2012 R2: KB 4343891, [30 de agosto de 2018: KB4343891 (vista previa del paquete acumulativo de actualizaciones mensuales)](https://support.microsoft.com/en-us/help/4343891/windows-81-update-kb4343891)

### Problemas de reproducción de vídeo en Windows 10, versión 1709

Este problema se produce cuando los usuarios se conectan a los equipos remotos que ejecutan Windows 10, versión 1709. Cuando estos usuarios reproducción el vídeo mediante la VMR9 códec (vídeo MixingRenderer9), el jugador muestra una ventana en negro.

Este es un problema conocido en Windows 10, versión 1709. El problema no se produce en Windows 10 versión 1703.

### Escritorio compartido problemas en Windows 10

Este problema se produce cuando el usuario tiene un perfil de usuario de solo lectura (y en el subárbol del registro asociado), como en un escenario de pantalla completa. Cuando ese usuario se conecta a un equipo remoto que ejecuta Windows 10, versión 1803, no pueden compartir su escritorio.

Para corregir este problema, aplicar la actualización de Windows 10 4340917, [24 de julio de 2018: KB4340917 (17134.191 de compilación del sistema operativo)](https://support.microsoft.com/en-us/help/4340917/windows-10-update-kb4340917).

### Problemas de rendimiento cuando se combinan las versiones de Windows 10 si se deshabilita NLA

Este problema se produce cuando está deshabilitado NLA y equipos de cliente de escritorio remoto que ejecuten Windows 10 se conectan a escritorios remotos que ejecutan distintas versiones de Windows 10. Los usuarios de clientes de escritorio remoto en equipos que ejecutan Windows 10, versión 1709 o versiones anteriores experimentar un rendimiento deficiente cuando se conectan a los escritorios remotos que ejecuten Windows 10, versión 1803 o una versión posterior.

Esto ocurre porque, cuando está deshabilitado NLA, los equipos cliente antiguos usan un protocolo más lento cuando intenten conectarse a Windows 10, versión 1803 o una versión posterior.

Para resolver este problema, aplicar 4340917 KB, [24 de julio de 2018: KB4340917 (17134.191 de compilación del sistema operativo)](https://support.microsoft.com/en-us/help/4340917/windows-10-update-kb4340917).

### Problema con una pantalla negra

Este problema se produce en Windows 8.0, Windows 8.1, Windows 10 RTM y Windows Server 2012 R2. Un usuario inicia varias aplicaciones en un equipo de escritorio remoto y luego se desconecta de la sesión. Periódicamente, el usuario vuelve a conectar con el escritorio remoto para interactuar con las aplicaciones y, a continuación, vuelve a desconectar. En algún momento, cuando el usuario vuelve a conectar, la sesión de escritorio remoto solo muestra una pantalla negra. El usuario debe terminar la sesión de escritorio remoto de consola del equipo remoto (o la consola del servidor RDSH). Esta acción impide que las aplicaciones.

Para resolver este problema, se aplican las actualizaciones siguientes según corresponda:

  - Windows 8 y Windows Server 2012: KB4103719, [17 de mayo de 2018: KB4103719 (vista previa del paquete acumulativo de actualizaciones mensuales)](https://support.microsoft.com/en-us/help/4103719/windows-server-2012-update-kb4103719)
  - Windows 8.1 y Windows Server 2012 R2: KB4103724, [17 de mayo de 2018: KB4103724 (vista previa del paquete acumulativo de actualizaciones mensuales)](https://support.microsoft.com/en-us/help/4103724/windows-81-update-kb4103724) y KB 4284863, [21 de junio de 2018: KB4284863 (vista previa del paquete acumulativo de actualizaciones mensuales)](https://support.microsoft.com/en-us/help/4284863/windows-81-update-kb4284863)
  - Windows 10: Fijo en KB4284860, [12 de junio de 2018: KB4284860 (compilación del SO 10240.17889)](https://support.microsoft.com/en-us/help/4284860/windows-10-update-kb4284860)
