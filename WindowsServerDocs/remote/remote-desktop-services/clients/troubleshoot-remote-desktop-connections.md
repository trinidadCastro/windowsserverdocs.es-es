---
title: Solución de problemas de las conexiones de Escritorio remoto
description: Solución de problemas de procedimientos organizados por síntoma
ms.custom: na
ms.reviewer: rklemen; josh.bender
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.assetid: ''
author: RobVyK
manager: ''
ms.author: kaushika; rklemen; josh.bender; v-tea
ms.date: 02/22/2019
ms.localizationpriority: medium
ms.openlocfilehash: c6ce719ffa24cfc6704348c17548fe5cf33d9271
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284147"
---
# <a name="troubleshooting-remote-desktop-connections"></a>Solución de problemas de las conexiones de Escritorio remoto
Para obtener una sucinta explicación de varios de los problemas más comunes de Servicios de Escritorio remoto (RDS), consulte [Preguntas más frecuentes acerca de los clientes de Escritorio remoto](https://review.docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-client-faq). En este artículo se describen varios enfoques más avanzados para solucionar problemas de conexión. Muchos de estos procedimientos se aplican tanto si se está solucionando una configuración simple, como un equipo físico que se conecta a otro equipo físico, como una configuración más complicada. Algunos procedimientos solucionan los problemas que se producen solo en escenarios más complicados, con varios usuarios. Para más información acerca de los componentes de Escritorio remoto y cómo funcionan conjuntamente, consulta [Arquitectura de Servicios de Escritorio remoto](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/desktop-hosting-logical-architecture).

> [!NOTE]  
> Muchos de los procedimientos que se describen en este artículo requieren tener acceso a varios equipos, a algunos de los cuales es posible que deba acceder de forma remota. Para más información acerca de las herramientas de administración remota y cómo configurarlas, consulta [Herramientas de administración de servidor remoto (RSAT) para sistemas operativos Windows](https://support.microsoft.com/en-us/help/2693643/remote-server-administration-tools-rsat-for-windows-operating-systems).

En la lista siguiente, identifica el tipo de síntoma de experimentas.

- [El cliente de Escritorio remoto no se puede conectar al escritorio remoto, pero no hay síntomas o mensajes específicos (pasos generales para la solución de problemas)](#no-specific-symptoms-or-messages-general-troubleshooting-steps)
- [El cliente de Escritorio remoto no se puede conectar al escritorio remoto y recibe el mensaje "Clase no registrada"](#client-cannot-connect-class-not-registered)
- [El cliente de Escritorio remoto no se puede conectar al escritorio remoto y recibe el mensaje "Clase no registrada"](#client-cannot-connect-no-licenses-available)
- [El usuario recibe el mensaje "Acceso denegado" o debe escribir las credenciales dos veces](#user-cannot-authenticate-or-must-authenticate-twice)
- [Al conectarse, el usuario recibe el mensaje "Remote Desktop Service is currently busy" (El servicio Escritorio remoto está ocupado actualmente)](#on-connecting-user-receives-remote-desktop-service-is-currently-busy-message)
- [El cliente de Escritorio remoto se desconecta y no puede volver a conectarse a la misma sesión](#rd-client-disconnects-and-cannot-reconnect-to-the-same-session)
- [El usuario se conecta a un portátil remoto a través de una red inalámbrica y, después, el portátil se desconecta de la red](#remote-laptop-disconnects-from-wireless-network).
- [El usuario experimenta un rendimiento deficiente o tiene problemas con las aplicaciones remotas](#user-experiences-poor-performance-or-application-problems)

> [!NOTE]  
> Para obtener una lista actual de los códigos de desconexión del RDP, consulta [ExtendedDisconnectReasonCode enumeración](https://docs.microsoft.com/windows/desktop/TermServ/extendeddisconnectreasoncode). 

## <a name="no-specific-symptoms-or-messages-general-troubleshooting-steps"></a>No hay síntomas o mensajes específicos (pasos generales para la solución de problemas)

Sigue estos pasos si un cliente de Escritorio remoto no se puede conectar a un escritorio remoto, pero no muestra mensajes u otros síntomas que ayuden a identificar la causa. Para resolver muchas de las causas más comunes de este tipo de problema, usa estos métodos:

- [Comprobación del estado del RDP](#check-the-status-of-the-rdp-protocol)
- [Comprobación del estado de los servicios de RDP](#check-the-status-of-the-rdp-services)
- [Comprobación de que el cliente de escucha de RDP funciona](#check-that-the-rdp-listener-is-functioning)
- [Comprobación del puerto del cliente de escucha de RDP](#check-the-rdp-listener-port)

### <a name="check-the-status-of-the-rdp-protocol"></a>Comprobación del estado del RDP

#### <a name="check-the-status-of-the-rdp-protocol-on-a-local-computer"></a>Comprobación del estado del RDP en un equipo local

Para comprobar y cambiar el estado de RDP en un equipo local, consulta [Cómo habilitar Escritorio remoto](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-allow-access#how-to-enable-remote-desktop).

> [!NOTE]  
> Si las opciones de Escritorio remoto no están disponibles, consulta [Compruebe si un objeto de directiva de grupo (GPO) está bloqueando el RDP en un equipo local](#check-whether-a-group-policy-object-gpo-is-blocking-rdp-on-a-local-computer).

#### <a name="check-the-status-of-the-rdp-protocol-on-a-remote-computer"></a>Comprobación del estado de RDP en un equipo remoto

> [!IMPORTANT]  
> Sigue meticulosamente los pasos que se describen en esta sección. Pueden producirse problemas graves si modifica el Registro de manera incorrecta. Antes de modificarlo, [haz una copia de seguridad del registro para restaurarlo](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22), por si se produjeran problemas.

Para comprobar el estado de RDP en un equipo remoto y cambiarlo usa una conexión del registro de red:

1. Selecciona **Inicio**, selecciona **Ejecutar** y escribe **regedt32**.
2. En el Editor del Registro, selecciona **Archivo**y, después, selecciona **Conectar al Registro de red**.
3. En el cuadro de diálogo **Seleccionar Equipo**, escriba el nombre del equipo remoto, seleccione **Comprobar nombres**y, después, selecciona **Aceptar**.
4. Vete a **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server**.  
   ![Editor del Registro, que muestra la entrada fDenyTSConnections](../media/troubleshoot-remote-desktop-connections/RegEntry_fDenyTSConnections.png)
   - Si el valor de la clave **fDenyTSConnections** es **0**, RDP está habilitado
   - Si el valor de la clave **fDenyTSConnections** es **1**, RDP está deshabilitado
5. Para habilitar RDP, cambia el valor de **fDenyTSConnections** de **1** a **0**.

#### <a name="check-whether-a-group-policy-object-gpo-is-blocking-rdp-on-a-local-computer"></a>Comprueba si hay algún un objeto de directiva de grupo (GPO) que bloquee el RDP en un equipo local

Si RDP no se puede activar en la interfaz de usuario o si el valor de **fDenyTSConnections** se revierte a **1** después de que lo haya cambiado, es posible que un GPO pueda reemplazar la configuración a nivel de equipo.

Para comprobar la configuración de la directiva de grupo en un equipo local, abre una ventana del símbolo del sistema como administrador y escribe el siguiente comando:
```
gpresult /H c:\gpresult.html
```
Cuando este comando finalice, abre gpresult.html. En **Configuración del equipo\\Plantillas administrativas\\Componentes de Windows\\Servicios de Escritorio remoto\\Host de sesión de Escritorio remoto \\Conexiones**, busca la directiva **Permitir que los usuarios se conecten de forma remota mediante Servicios de Escritorio remoto**.

- Si el valor de esta directiva es **Habilitado**, la directiva de grupo no bloquea las conexiones RDP.
- Si el valor de esta directiva es **Deshabilitado**, comprueba **GPO prevalente**. Éste es el GPO que bloquea conexiones RDP.
  ![Un segmento de ejemplo de gpresult.html, en el que el GPO de nivel de dominio ** Bloque RDP ** está deshabilitando RDP.](../media/troubleshoot-remote-desktop-connections/GPResult_RDSH_Connections_GP.png)
   
  ![Un segmento de ejemplo de gpresult.html, en el que **Directiva de grupo local** está deshabilitando RDP.](../media/troubleshoot-remote-desktop-connections/GPResult_RDSH_Connections_LGP.png)

#### <a name="check-whether-a-gpo-is-blocking-rdp-on-a-remote-computer"></a>Comprueba si un GPO bloquea el RDP en un equipo remoto

Para comprobar la configuración de la directiva de grupo en un equipo remoto, el comando es prácticamente el mismo que en un equipo local:
```
gpresult /S <computer name> /H c:\gpresult-<computer name>.html
```
El archivo que genera este comando (**gpresult -\<nombre de equipo\>.html**) usa el mismo formato de información que la versión del equipo local (**gpresult.html**).

#### <a name="modifying-a-blocking-gpo"></a>Modificación de un GPO de bloqueo

Esta configuración se puede modificar en el Editor de objetos de directiva de grupo (GPE) y en la Consola de administración de directivas de grupo (GPM). Para más información acerca de cómo usar la directiva de grupo, consulta [Administración avanzada de directivas de grupo](https://docs.microsoft.com/microsoft-desktop-optimization-pack/agpm/).

Para modificar la directiva de bloqueo, usa uno de estos métodos:

- En GPE, accede al nivel apropiado de GPO (como local o dominio), y vete **Configuración del equipo\\Plantillas administrativas\\Componentes de Windows\\Servicios de Escritorio remoto\\Host de sesión de Escritorio remoto \\Conexiones**, \\**Permitir que los usuarios se conecten de forma remota mediante Servicios de Escritorio remoto**.  
   1. Establece la directiva en **Habilitado** o **No configurado**.
   2. En los equipos afectados, abre una ventana del símbolo del sistema como administrador y ejecuta el comando **gpupdate /force**.
- En GPM, vete a la unidad organizativa en la que la directiva de bloqueo se aplica a los equipos afectados y elimina dicha directiva.

### <a name="check-the-status-of-the-rdp-services"></a>Comprobación del estado de los servicios de RDP

Los siguientes servicios deben estar en ejecución tanto en el equipo local (cliente) como en el remoto (destino):

- Servicios de Escritorio remoto (TermService)
- Redirector de puerto en modo usuario de Servicios de Escritorio remoto (UmRdpService)

Puedes usar el complemento MMC Servicios para administrar los servicios de forma local o remota. También puedes usar PowerShell de forma local o remota (si el equipo remoto está configurado para aceptar comandos de PowerShell remotos).

![Servicios de Escritorio remoto en el complemento MMC Servicios. No modifiques la configuración predeterminada del servicio.](../media/troubleshoot-remote-desktop-connections/RDSServiceStatus.png)

Si alguno de los servicios, o ambos, no están en ejecución en cualquiera de los equipos, inícialos.

> [!NOTE]  
> Si inicias el servicio Servicios de Escritorio remoto, haz clic en **Sí** para reiniciar automáticamente el Redirector de puerto en modo usuario de Servicios de Escritorio remoto.

### <a name="check-that-the-rdp-listener-is-functioning"></a>Comprobación de que el cliente de escucha de RDP funciona

> [!IMPORTANT]  
> Sigue meticulosamente los pasos que se describen en esta sección. Pueden producirse problemas graves si modifica el Registro de manera incorrecta. Antes de modificarlo, [haz una copia de seguridad del registro para restaurarlo](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22), por si se produjeran problemas.

#### <a name="check-the-status-of-the-rdp-listener"></a>Comprobación del estado del cliente de escucha de RDP

Para este procedimiento, usa una instancia de PowerShell que tenga permisos administrativos. En los equipos locales, también puedes usar un símbolo del sistema en el que tengas permisos administrativos. Sin embargo, este procedimiento usa PowerShell, porque los mismos comandos funcionan tanto local como remotamente.

1. Abra una ventana de PowerShell. Para conectarse a un equipo remoto, escribe **Enter-PSSession - ComputerName \<nombre de equipo\>** .
2. Escribe **qwinsta**. 
    ![El comando qwinsta enumera los procesos que escuchan en los puertos del equipo.](../media/troubleshoot-remote-desktop-connections/WPS_qwinsta.png)
3. Si la lista incluye **rdp-tcp** con el estado **Escuchar**, significa que el cliente de escucha de RDP está trabajando. Pasa a [Comprobación del puerto del cliente de escucha de RDP](#check-the-rdp-listener-port). De lo contrario, vete al paso 4.
4. Exporta la configuración del cliente de escucha de RDP desde un equipo de trabajo.
    1. Inicia sesión en un equipo que tenga la misma versión del sistema operativo que el equipo afectado y acceso al Registro de dicho equipo (por ejemplo, mediante el Editor del Registro).
    2. Vete a la siguiente entrada del Registro:  
        **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\RDP-Tcp**
    3. Exporta la entrada a un archivo. reg. Por ejemplo, en el Editor del registro, haz clic con el botón derecho en la entrada, selecciona **Exportar**y escriba un nombre de archivo para la configuración exportada.
    4. Copia el archivo .reg exportado en el equipo afectado.
5. Para importar la configuración del cliente de escucha de RDP, abre una ventana de PowerShell que tenga permisos administrativos en el equipo afectado (o abre la ventana de PowerShell y conéctate de forma remota al equipo afectado).
   1. Para realizar una copia de seguridad de la entrada del Registro existente, escribe el siguiente comando:  
   
      ```powershell  
      cmd /c 'reg export "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-tcp" C:\Rdp-tcp-backup.reg'   
      ```  
   

   2. Para eliminar la entrada del Registro existente, escribe los siguientes comandos:  
   
      ```powershell  
      Remove-Item -path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-tcp' -Recurse -Force  
      ```
   
   3. Para importar la nueva entrada del registro y, después, reiniciar el servicio, escribe los siguientes comandos:  
   
      ```powershell  
      cmd /c 'regedit /s c:\<filename>.reg'  
      Restart-Service TermService -Force  
      ```   
      donde \<nombreDeArchivo\> es el nombre del archivo .reg exportado.
6. Para probar la configuración vuelve a intentar establecer la conexión a Escritorio remoto. Si sigues sin poder conectarse, reinicia el equipo afectado.
7. Si sigues sin poder conectarse, [comprueba el estado del certificado autofirmado de RDP](#check-the-status-of-the-rdp-self-signed-certificate).

#### <a name="check-the-status-of-the-rdp-self-signed-certificate"></a>Comprobación del estado del certificado autofirmado de RDP

1. Si sigues sin poder conectarse, abre el complemento MMC Certificados. Cuando se te pida que selecciones el almacén de certificados que deseas administrar, selecciona **Cuenta de equipo**y, después, selecciona el equipo afectado.
2. En la carpeta **Certificates** de **Remote Desktop**, elimina el certificado autofirmado de RDP. 
    ![Certificados de Escritorio remoto en el complemento MMC Certificados.](../media/troubleshoot-remote-desktop-connections/MMCCert_Delete.png)
3. En el equipo afectado, reinicia el servicio Servicios de Escritorio remoto.
4. Actualiza el complemento Certificados.
5. Si no se ha vuelto a crear el certificado autofirmado de RDP [comprueba los permisos de la carpeta MachineKeys](#check-the-permissions-of-the-machinekeys-folder).

#### <a name="check-the-permissions-of-the-machinekeys-folder"></a>Comprobación de los permisos de la carpeta MachineKeys

1. En el equipo afectado, abre el explorador y vete a **C:\\ProgramData\\Microsoft\\Crypto\\RSA\\** .
2. Haz clic en **MachineKeys**, selecciona **Propiedades**, **Seguridad**y, después, selecciona **Avanzadas**.
3. Asegúrate de que están configurados los siguientes permisos:
      - Builtin\\administradores: Control total
      - Todos: Lectura y escritura

### <a name="check-the-rdp-listener-port"></a>Comprobación del puerto del cliente de escucha de RDP

El cliente de escucha de RDP debería escuchar en el puerto 3389 tanto en el equipo local (cliente) como en el remoto (destino). Ninguna otra aplicación debería usar dicho puerto.

> [!IMPORTANT]  
> Sigue meticulosamente los pasos que se describen en esta sección. Pueden producirse problemas graves si modifica el Registro de manera incorrecta. Antes de modificarlo, [haz una copia de seguridad del registro para restaurarlo](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22), por si se produjeran problemas.

Para comprobar o cambiar el puerto de RDP, utiliza el Editor del Registro:

1. Selecciona Inicio, selecciona **Ejecutar** y escribe **regedt32**.
      - Para establecer una conexión con un equipo remoto, selecciona **Archivo**y, después, selecciona **Conectar al Registro de red**.
      - En el cuadro de diálogo **Seleccionar Equipo**, escriba el nombre del equipo remoto, seleccione **Comprobar nombres**y, después, selecciona **Aceptar**.
2. Abre el registro y vete al cliente de escucha **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\\<\>** . 
    ![La subclave PortNumber de RDP.](../media/troubleshoot-remote-desktop-connections/RegEntry_PortNumber.png)
3. Si el valor de **PortNumber** no es **3389**, cámbialo a **3389**. 
   > [!IMPORTANT]  
    > Servicios de Escritorio remoto se puede manejar desde otro puerto. Sin embargo, no es aconsejable hacerlo. La solución de los problemas que puede acarrear dicha configuración está fuera del ámbito de este artículo.
4. Después de cambiar el número de puerto, reinicia el servicio Servicios de Escritorio remoto.

#### <a name="check-that-another-application-is-not-trying-to-use-the-same-port"></a>Comprueba que otra aplicación no intenta usar el mismo puerto

Para este procedimiento, usa una instancia de PowerShell que tenga permisos administrativos. En los equipos locales, también puedes usar un símbolo del sistema en el que tengas permisos administrativos. Sin embargo, este procedimiento usa PowerShell, porque los mismos comandos funcionan tanto local como remotamente.

1. Abra una ventana de PowerShell. Para conectarse a un equipo remoto, escribe **Enter-PSSession - ComputerName \<nombrede equipo\>** .
2. Escriba el comando siguiente:  
   
     ```powershell  
    cmd /c 'netstat -ano | find "3389"'  
    ```
  
    ![El comando netstat genera una lista de puertos y los servicios que los escuchan.](../media/troubleshoot-remote-desktop-connections/WPS_netstat.png)
3. Busque una entrada para el puerto TCP 3389 (o el puerto RDP asignado) con el estado **Escuchando**. 
    > [!NOTE]  
   > El PID (identificador de proceso) del proceso o servicio que utiliza el puerto aparece en la columna PID.
4. Para determinar qué aplicación utiliza el puerto 3389 (o el puerto de RDP asignado), escribe el siguiente comando:  
   
     ```powershell  
    cmd /c 'tasklist /svc | find "<pid listening on 3389>"'  
    ```  
  
    ![El comando tasklist informa de los detalles de un proceso específico.](../media/troubleshoot-remote-desktop-connections/WPS_tasklist.png)
5. Busca una entrada que contenga el número de PID asociado al puerto (desde la salida de **netstat**). Los servicios o los procesos asociados con dicho PID aparecen a la derecha.
6. Si una aplicación o servicio que no sea Servicios de Escritorio remoto (TermServ.exe) usa el puerto, el conflicto se puede mediante uno de estos métodos:
      - Configura la otra aplicación o servicio para que utilicen un puerto diferente (se recomienda).
      - Desinstala la otra aplicación o servicio.
      - Configura RDP para que utilice otro puerto diferente y, después, reinicia el servicio Servicios de Escritorio remoto (no se recomienda).

#### <a name="check-whether-a-firewall-is-blocking-the-rdp-port"></a>Comprobación de si un firewall está bloqueando el puerto de RDP

Usa la herramienta **psping** para probar si puedes acceder al equipo afectado a través del puerto 3389.

1. En un equipo que no sea el afectado, descarga **psping** de <https://live.sysinternals.com/psping.exe>.
2. Abre una ventana del símbolo del sistema como administrador, cambia al directorio en el que instalaste **psping**y escribe el siguiente comando:  
   
   ```  
   psping -accepteula <computer IP>:3389  
   ```
   
3. Comprueba si la salida del comando **psping** contiene resultados como los siguientes:  
      - **Conectando a \<IP de equipo\>** : se puede acceder al equipo remoto.
      - **(0 % perdidos)** : todos los intentos de conexión han fructificado.
      - **El equipo remoto ha rechazado la conexión de red**: no se puede acceder al equipo remoto.
      - **(100 % perdidos)** : error en todos los intentos de conexión.
1. Ejecuta **psping** en varios equipos para probar su capacidad para conectarse al equipo afectado.
1. Observa si el equipo afectado bloquea las conexiones de todos los demás equipos, de algunos equipos o solo del otro equipo.
1. Siguientes pasos recomendados:
      - Póngase en contacto con los administradores de red para comprobar que la red permite que llegue tráfico RDP al equipo afectado.
      - Investiga las configuraciones de los firewalls que haya entre los equipos de origen y el equipo afectado (incluido Windows Firewall en el equipo afectado) para determinar si alguno de ellos está bloqueando el puerto RDP.

## <a name="client-cannot-connect-class-not-registered"></a>El cliente no se puede conectar, "Clase no registrada"

Al intentar conectarse a un equipo remoto mediante un cliente que utiliza la versión 1709 de Windows 10, o cualquier versión posterior, el cliente no puede conectar mientras el servidor de Host de sesión de Escritorio remoto muestra un mensaje que contiene el código de error "Clase no registrada (0 x 80040154)".

Este problema se produce si el usuario que intenta conectarse tiene un perfil de usuario obligatorio. Para resolverlo, instale la actualización de Windows 10 de [24 de julio de 2018: KB4338817 (compilación de sistema operativo 16299.579)](https://support.microsoft.com/en-us/help/4338817/windows-10-update-kb4338817) actualizaciones de Windows 10.

## <a name="client-cannot-connect-no-licenses-available"></a>El cliente no se puede conectar, no hay licencias disponibles

Esta situación se aplica a las implementaciones que incluyen un servidor RDSH y un servidor de administración de licencias de Escritorio remoto.

Identificar qué tipo de comportamiento ven los usuarios:

- La sesión se desconectó porque no hay licencias disponibles o no hay ningún servidor de licencias disponible
- Se denegó el acceso debido a un error de seguridad

Inicia sesión en el host de sesión de Escritorio remoto como administrador de dominio y abre la herramienta Diagnóstico de licencias de Escritorio remoto. Busca mensajes como el siguiente:

  - El período de gracia del servidor remoto ha expirado en servidor host de sesión de Escritorio remoto, pero no se configuró el servidor host de sesión de Escritorio remoto con los servidores de licencias. Las conexiones al servidor host de sesión de Escritorio remoto se denegarán, salvo que haya un servidor de licencias configurado para el servidor host de sesión de Escritorio remoto.
  - El servidor de licencias \<nombre de equipo\> no está disponible. Esto puede deberse a problemas de conectividad de la red, el servicio Administración de licencias de Escritorio remoto se detiene en el servidor de licencias o no está instalado en el equipo.

Estos problemas tienden a asociarse con los siguientes mensajes de usuario:

  - La sesión remota se desconectó porque no hay licencias de acceso de cliente de Escritorio remoto disponibles para este equipo.
  - La sesión remota se desconectó porque no hay servidores de licencias de Escritorio remoto disponibles proporcionar una licencia.

En este caso, [configure el servicio Administración de licencias de Escritorio remoto](#configure-the-rd-licensing-service).

Si la herramienta Diagnóstico de licencias de Escritorio remoto muestra otros problemas, como "El componente de protocolo RDP X.224 detectó un error en el flujo de protocolos y ha desconectado el cliente", es posible que haya un problema que afecta a los certificados de licencia. Dichos problemas tienden a asociarse con mensajes al usuario como los siguientes:

Debido a un error de seguridad, el cliente no se pudo conectar al servidor de Terminal Server. Cuando te hayas asegurado de que has iniciado sesión la red, intenta volver a conectarte al servidor.

En este caso, [actualiza las claves del Registro del certificado X509](#refresh-the-x509-certificate-registry-keys).

### <a name="configure-the-rd-licensing-service"></a>Configuración del servicio Administración de licencias de Escritorio remoto

El siguiente procedimiento usa el Administrador de servidores para realizar los cambios en la configuración. Para obtener información acerca de cómo configurar y usar el Administrador de servidores, consulta [Administrador de servidores](https://docs.microsoft.com/windows-server/administration/server-manager/server-manager).

1. Abre el Administrador de servidores y vete a **Servicios de Escritorio remoto**.
2. En **Información general sobre la implementación** , selecciona **Tareas**y, después, selecciona **Edit Deployment Properties** (Editar propiedades de implementación).
3. Seleccione **licencias de escritorio remoto**y, a continuación, seleccione el modo de licencia adecuado para su implementación (**por dispositivo** o **por usuario**).
4. Escribe el nombre de dominio completo (FQDN) del servidor de licencias de RD y, después, selecciona **Agregar**.
5. Si tienes más de un servidor de licencias de RD, repite el paso 4 en cada uno de ellos. 
    ![Opciones de configuración del servidor de licencias de RD en el Administrador de servidores.](../media/troubleshoot-remote-desktop-connections/RDLicensing_Configure.png)

### <a name="refresh-the-x509-certificate-registry-keys"></a>Actualización de las claves del Registro del certificado X509

> [!IMPORTANT]  
> Sigue meticulosamente los pasos que se describen en esta sección. Pueden producirse problemas graves si modifica el Registro de manera incorrecta. Antes de modificarlo, [haz una copia de seguridad del registro para restaurarlo](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22), por si se produjeran problemas.

Para resolver este problema, haz una copia de seguridad y, después, elimina las claves del Registro de certificado X509, reinicia el equipo y reactiva el servidor de licencias de RD. Para ello, siga estos pasos.

> [!NOTE]  
> Realiza el procedimiento siguiente en cada uno de los servidores RDSH.

1. Abre el Editor del Registro y vete a **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\RCM**.
2. En el menú Registro, selecciona **Exportar archivo del Registro**.
3. Escribe **exported- Certificate** en el cuadro **Nombre de archivo** cuadro y, a continuación, selecciona **guardar**.
4. Haz clic en cada uno de los siguientes valores, selecciona **Eliminar**y, después, selecciona **Sí** para comprobar la eliminación:  
      - **Certificado**
      - **Certificado X509**
      - **Identificador de certificado X509**
      - **Certificado X509 2**
5. Sal del Editor del Registro y reinicia el servidor de RDSH.

## <a name="user-cannot-authenticate-or-must-authenticate-twice"></a>El usuario no se puede autenticar o debe autenticarse dos veces

Hay varios problemas que pueden provocar problemas que afectan a la autenticación de usuario. En esta sección se tratan los siguientes casos:

  - [Se deniega el acceso a un usuario con el mensaje "tipo de inicio de sesión restringido"](#access-denied-restricted-type-of-logon)
  - [SE deniega el acceso a un usuario o aplicación con el evento 16965, "Se ha denegado una llamada remota a la base de datos SAM"](#access-denied-a-remote-call-to-the-sam-database-has-been-denied)
  - [Un usuario no puede iniciar sesión con una tarjeta inteligente](#a-user-cannot-sign-in-by-using-a-smart-card)
  - [Si se bloquea el equipo remoto, el usuario debe escribir una contraseña dos veces](#if-the-remote-pc-is-locked-a-user-needs-to-enter-a-password-twice)
  - [Un usuario no puede iniciar sesión y recibe los mensajes "error de autenticación" y "corrección de cifrado CredSSP de Oracle"](#user-cannot-sign-in-and-receives-authentication-error-and-credssp-encryption-oracle-remediation-messages)
  - [Después de actualizar los equipos cliente, algunos usuarios necesitan iniciar sesión dos veces](#after-you-update-client-computers-some-users-need-to-sign-in-twice).
  - [Se deniega el acceso a los usuarios en una implementación que usa RemoteGuard con varios agentes de conexión a Escritorio remoto](#users-are-denied-access-on-a-deployment-that-uses-remote-credential-guard-with-multiple-rd-connection-brokers)

### <a name="access-denied-restricted-type-of-logon"></a>Acceso denegado, tipo de inicio de sesión restringido

En esta situación, se deniega el acceso a un usuario de Windows 10 que intenta conectarse a equipos con Windows 10 o Windows Server 2016 con el siguiente mensaje:

> Conexión a Escritorio remoto:  
> El administrador del sistema ha restringido el tipo de inicio de sesión (red o interactiva) que pueda usar. Para obtener ayuda, ponte en contacto con el administrador del sistema o con el personal de soporte técnico.

Este problema se produce cuando se requiere autenticación a nivel de red (NLA) para las conexiones RDP y el usuario no es miembro del grupo**Usuarios de escritorio remoto**. También puede producirse si no se ha asignado el grupo**Usuarios de escritorio remoto** al derecho de usuario **Tener acceso a este equipo desde la red**.

Para solucionar este problema, sigue uno de estos pasos:

  - [Modifica la asignación de permisos del usuario o la pertenencia a un grupo del usuario](#modify-the-users-group-membership-or-user-rights-assignment).
  - Desactiva NLA (no se recomienda).
  - Usa clientes de escritorio remoto que no sean de Windows 10. Por ejemplo, los clientes de Windows 7 no tienen este problema.

#### <a name="modify-the-users-group-membership-or-user-rights-assignment"></a>Modificación de la asignación de permisos del usuario o la pertenencia a un grupo del usuario

Si este problema afecta a un solo usuario, la solución más sencilla consiste en agregar el usuario al grupo **Usuarios de escritorio remoto**.

Si el usuario ya es miembro de este grupo (o si varios miembros del grupo tienen el mismo problema), compruebe la configuración de los permisos del usuario en el equipo remoto con Windows 10 o Windows Server 2016.

1. Abre el Editor de objetos de directiva de grupo (GPE) y conéctese a la directiva local del equipo remoto.
2. Vete a **Configuración del equipo \\Configuración de Windows \\Configuración de seguridad\\Directivas locales\\** , haz clic con el botón derecho en **Tener acceso a este equipo desde la red** y luego selecciona **Propiedades**.
3. Consulta en la lista de usuarios y grupos **Usuarios de escritorio remoto** (o un grupo primario).
4. Si la lista no incluye **Usuarios de escritorio remoto** (o un grupo primario como **Todos**) es preciso que lo agregues a la lista (si tienes más de uno o dos equipos en su implementación, utiliza un objeto de directiva de grupo) .  
    Por ejemplo, la pertenencia predeterminada de **Tener acceso a este equipo desde la red** incluye **Todos**. Si la implementación usa un objeto de directiva de grupo para quitar **Todos**, es posible que debas restaurar el acceso mediante la actualización del objeto de directiva de grupo para agregar **Usuarios de escritorio remoto**.

### <a name="access-denied-a-remote-call-to-the-sam-database-has-been-denied"></a>Acceso denegado, se ha denegado una llamada remota a la base de datos de SAM

Es muy probable que este comportamiento se produzca si el controlador de dominio utiliza Windows Server 2016, o cualquier versión o posterior, y los usuarios intentan conectarse mediante una aplicación de conexión personalizada. En concreto, se denegará el acceso a las aplicaciones que acceden a la información del perfil del usuario en Active Directory.

Este comportamiento es consecuencia de un cambio en Windows. Tanto en Windows Server 2012 R2 como en las versiones anteriores, cuando un usuario inicia sesión en un escritorio remoto, el Administrador de conexiones remotas (RCM) se pone en contacto con el controlador de dominio (DC) para consultar las configuraciones que son específicas de Escritorio remoto en el objeto de usuario en Active Directory Domain Services (AD DS). Esta información se muestra en la pestaña Perfil de Servicios de Escritorio remoto de las propiedades del objeto de un usuario en el complemento MMC Usuarios y equipos de Active Directory.

A partir de Windows Server 2016, RCM no consulta el objeto usuarios en AD DS. Si necesitas que RCM consulte AD DS porque usa los atributos de Servicios de Escritorio remoto, debes habilitar manualmente el comportamiento anterior de RCM.

> [!IMPORTANT]  
> Sigue meticulosamente los pasos que se describen en esta sección. Pueden producirse problemas graves si modifica el Registro de manera incorrecta. Antes de modificarlo, [haz una copia de seguridad del registro para restaurarlo](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22), por si se produjeran problemas.

Para habilitar el comportamiento heredado de RCM en un servidor host de sesión de Escritorio remoto, configura las siguientes entradas del Registro y reinicia el servicio **Servicios de Escritorio remoto**:  
  - **HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Policies\\Microsoft\\Windows NT\\Terminal Services**
  - **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\\<NombreDeWinstation\>\\**  
      - Nombre: **fQueryUserConfigFromDC**
      - Tipo: **Reg\_DWORD**
      - Valor: **1** (decimal)

Para habilitar el comportamiento heredado de RCM en cualquier servidor que no sea el servidor host de sesión de Escritorio remoto, configura estas entradas del Registro y la siguiente entrada del Registro adicional (y reinicia el servicio):
  - **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server**

Para más información acerca de este comportamiento, consulta el KB 3200967 [Cambios en el Administrador de conexiones remotas en Windows Server](https://support.microsoft.com/en-us/help/3200967/changes-to-remote-connection-manager-in-windows-server).

### <a name="a-user-cannot-sign-in-by-using-a-smart-card"></a>Un usuario no puede iniciar sesión con una tarjeta inteligente

Este artículo aborda las tres circunstancias comunes en las que un usuario no puede iniciar sesión en un escritorio remoto mediante una tarjeta inteligente:  
  - [El usuario no puede iniciar sesión en una sucursal que utiliza un controlador de dominio (RODC) de solo lectura](#cannot-sign-in-with-a-smart-card-in-a-branch-office-with-a-rodc)
  - [El usuario no puede iniciar sesión un equipo con Windows Server 2008 SP2 que se haya ha actualizado recientemente](#cannot-sign-in-with-a-smart-card-to-a-windows-server-2008-sp2-computer)
  - [El usuario no puede mantener la sesión iniciada en un equipo con Windows que se ha actualizado recientemente y el servicio Servicios de Escritorio remoto deja de responder (se bloquea)](#cannot-stay-signed-in-with-a-smart-card-and-remote-desktop-services-service-hangs)

#### <a name="cannot-sign-in-with-a-smart-card-in-a-branch-office-with-a-rodc"></a>No se puede iniciar sesión con una tarjeta inteligente en una sucursal con un RODC

Este problema se produce en las implementaciones que incluyen un servidor RDSH en un sitio de sucursal que usa un RODC. El servidor RDSH está hospedado en el dominio raíz. Los usuarios del sitio de rama pertenecen a un dominio secundario y usan tarjetas inteligentes para la autenticación. El RODC está configurado para almacenar en la caché las contraseñas de usuario (el RODC pertenece al **Grupo de replicación de contraseña RODC permitida**). Cuando los usuarios intentan iniciar sesión en sesiones del servidor RDSH, reciben mensajes, como "El inicio de sesión intentado no es válido. Esto se debe a un nombre de usuario o a información de autenticación incorrectos."

Se trata de un problema conocido, en el sentido de la forma en que el controlador de dominio raíz y el RODC administran el cifrado de las credenciales del usuario. El controlador de dominio raíz usa una clave de cifrado para cifrar las credenciales y el RODC proporciona una clave de descifrado al cliente. Sin embargo, las dos claves no coinciden.

Para solucionar este problema, considera la posibilidad de cambiar la topología del controlador de dominio mediante la desactivación del almacenamiento en caché de la contraseña en el RODC manualmente, o bien mediante la implementación de un controlador de dominio grabable en el sitio de sucursal. Un método alternativo es mover el servidor RDSH al dominio secundario en que se encuentran los usuarios. Otra alternativa es permitir a los usuarios iniciar sesión sin tarjetas inteligentes. Cualquiera de estas soluciones pueden requerir pone en peligro el nivel de seguridad o el rendimiento.

#### <a name="cannot-sign-in-with-a-smart-card-to-a-windows-server-2008-sp2-computer"></a>No se puede iniciar sesión con una tarjeta inteligente en un equipo con Windows Server 2008 SP2

Este problema se produce cuando los usuarios inician sesión en un equipo con Windows Server 2008 SP2 que se ha actualizado con KB4093227 (2018.4B). Cuando los usuarios intentan iniciar sesión mediante una tarjeta inteligente, se les deniega el acceso con mensajes como "No se encontraron certificados válidos. Comprueba que la tarjeta se ha insertado correctamente y encaja perfectamente." Al mismo tiempo, el equipo con Windows Server registra el evento de programa "Error al recuperar un certificado digital de la tarjeta inteligente insertada. Firma no válida."

Para resolver este problema, actualiza el equipo con Windows Server con el nuevo lanzamiento de 2018.06 B de 4093227 KB, [Descripción de la actualización de seguridad para la vulnerabilidad de denegación de servicio de Protocolo de escritorio remoto (RDP) en Windows Server 2008: 10 de abril de 2018](https://support.microsoft.com/en-us/help/4093227/security-update-for-vulnerabilities-in-windows-server-2008).

#### <a name="cannot-stay-signed-in-with-a-smart-card-and-remote-desktop-services-service-hangs"></a>No se puede mantener la sesión iniciada sesión con una tarjeta inteligente y el servicio Servicios de Escritorio remoto se bloquea

Este problema se produce cuando los usuarios inician sesión en un equipo con Windows o Windows Server que se ha actualizado con KB 4056446. Al principio, el usuario puede iniciar sesión en el sistema mediante el uso de una tarjeta inteligente, pero luego recibe el mensaje de error "SCARD\_E\_NO\_SERVICE". Es posible que el equipo remoto deje de responder.

Para solucionar este problema, reinicia el equipo remoto.

Para resolver este problema, actualice el equipo remoto con la corrección adecuada:

  - Windows Server 2008 SP2: KB 4090928, [Windows leaks handles in the lsm.exe process and smart card applications may display "SCARD\_E\_NO\_SERVICE" errors](https://support.microsoft.com/en-us/help/4090928/scard-e-no-service-errors-when-windows-leaks-handles-in-the-lsm-exe) (Windows pierde controladores en el proceso de lsm.exe y las aplicaciones de la tarjeta inteligente pueden mostrar errores "SCARD_E_NO_SERVICE")
  - Windows Server 2012 R2: KB 4103724, [17 de mayo de 2018: KB4103724 (versión preliminar del paquete acumulativo mensual)](https://support.microsoft.com/en-us/help/4103724/windows-81-update-kb4103724)
  - Windows Server 2016 y Windows 10, versión 1607: KB 4103720, [17 de mayo de 2018: KB4103720 (compilación de sistema operativo 14393.2273)](https://support.microsoft.com/en-us/help/4103720/windows-10-update-kb4103720)

### <a name="if-the-remote-pc-is-locked-a-user-needs-to-enter-a-password-twice"></a>Si se bloquea el equipo remoto, el usuario debe escribir la contraseña dos veces

Este problema puede producirse cuando un usuario intenta conectarse a un escritorio remoto que usa Windows 10, versión 1709, en una implementación en la que las conexiones de RDP no requieren NLA. En estas condiciones, si el escritorio remoto se ha bloqueado, el usuario debe escribir sus credenciales dos veces al conectarse.

Para resolver este problema, actualiza el equipo con Windows 10, versión 1709, con KB 4343893, [30 de agosto de 2018: KB4343893 (compilación del sistema operativo 16299.637)](https://support.microsoft.com/en-us/help/4343893/windows-10-update-kb4343893).

### <a name="user-cannot-sign-in-and-receives-authentication-error-and-credssp-encryption-oracle-remediation-messages"></a>El usuario no puede iniciar sesión y recibe los mensajes "error de autenticación" y "corrección de oráculo de cifrado de CredSSP"

Cuando los usuarios intentan iniciar sesión con cualquier versión de Windows a partir de Windows Vista SP2 o Windows Server 2008 SP2, se les deniega el acceso y reciben mensajes como los siguientes:

  - Se ha producido un error de autenticación. No se admite la función solicitada.
  - Esto puede deberse a la corrección de oráculo de cifrado de CredSSP

"Corrección de oráculo de cifrado de CredSSP" es un conjunto de actualizaciones de seguridad que se lanzó en marzo, abril y mayo de 2018. CredSSP es un proveedor de autenticación que procesa solicitudes de autenticación de otras aplicaciones. Tanto la actualización "3B", de 13 de marzo de 2018, como las actualizaciones posteriores solucionaban una vulnerabilidad de seguridad en la que un atacante podría transmitir credenciales de usuario para ejecutar código en el sistema de destino.

Las actualizaciones iniciales agregaban compatibilidad con un nuevo objeto de directiva de grupo, **Corrección de oráculo de cifrado**, que tiene los siguientes valores posibles:

  - **Vulnerable**. Las aplicaciones cliente que usan CredSSP pueden revertirse a versiones no seguras, pero este comportamiento expone los escritorios remotos a ataques. Los servicios que usan CredSSP aceptan clientes que no se hayan actualizado.
  - **Mitigado**. Las aplicaciones cliente que usan CredSSP no pueden revertirse a versiones no seguras, pero los servicios que usan CredSSP aceptan a los clientes que no se han actualizado.
  - **Forzar clientes actualizados**. Las aplicaciones cliente que usan CredSSP no pueden revertirse a versiones no seguras y los servicios que usan CredSSP no aceptarán clientes sin revisiones. 
    Nota: Este valor no se debe implementar hasta que todos los hosts remotos admitan la versión más reciente.

La actualización del 8 de mayo de 2018 cambió el valor de **Corrección de oráculo de cifrado** predeterminado de **Vulnerable** a **Mitigado**. Con este cambio, los clientes de Escritorio remoto que tengan las actualizaciones no se pueden conectar a servidores que no las tengan (ni a servidores actualizados que no se hayan reiniciado). Para más información acerca de los efectos de las actualizaciones y los tipos de comunicación que bloquean, consulta KB 4093492, [Actualizaciones de CredSSP para CVE-2018-0886](https://support.microsoft.com/en-us/help/4093492/credssp-updates-for-cve-2018-0886-march-13-2018).

Para resolver este problema, asegúrate de que todos los sistemas están completamente actualizados y reiniciados. Para ver una lista completa de las actualizaciones y más información acerca de las vulnerabilidades, consulta [CVE-2018-0886 | CredSSP Remote Code Execution Vulnerability](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-0886) (CVE-2018-0886 | Vulnerabilidad de ejecución de código remoto de CredSSP).

Para solucionar este problema hasta que se completen las actualizaciones, consulta en KB 4093492 los tipos de conexiones permitidos. Si no hay alternativas viables, puedes considerar la posibilidad de usar uno de los métodos siguientes:

- En el caso de los equipos cliente afectados, devuelve la directiva **Corrección de oráculo de cifrado** al valor **Vulnerable**.
- Modifica las siguientes directivas en la carpeta de directivas de grupo **Configuración del equipo\\Plantillas administrativas\\Componentes de Windows\\Servicios de Escritorio remoto\\Host de sesión de Escritorio remoto\\ Seguridad**:  
  - **Requerir el uso de un nivel de seguridad específico para conexiones remotas (RDP)** : establécelo en **Habilitado** y selecciona **RDP**.
  - **Requerir la autenticación de usuario para las conexiones remotas mediante Autenticación a nivel de red**: establécelo en **Deshabilitado**.
    > [!IMPORTANT]  
    > Estas modificaciones reducen la seguridad de la implementación. Si se llegan a usar, debe ser de forma estrictamente temporales.

Para más información acerca de cómo trabajar con directivas de grupo, consulta [Modificación de un GPO de bloqueo](#modifying-a-blocking-gpo).

### <a name="after-you-update-client-computers-some-users-need-to-sign-in-twice"></a>Después de actualizar los equipos cliente, algunos usuarios necesitan iniciar sesión dos veces

Algunos usuarios que tienen Windows 7 o Windows 10, versión 1709, inician una sesión de Escritorio remoto y ven de inmediato una segunda pantalla de inicio de sesión. Este problema se produce si los equipos cliente han recibido las siguientes actualizaciones:

  - Windows 7: KB 4103718, [8 de mayo de 2018: KB4103718 (paquete acumulativo mensual)](https://support.microsoft.com/en-us/help/4103718/windows-7-update-kb4103718)
  - Windows 10 1709: KB 4103727, [8 de mayo de 2018: KB4103727 (compilación de sistema operativo 16299.431)](https://support.microsoft.com/en-us/help/4103727/windows-10-update-kb4103727)

Para resolver este problema, asegúrate de que los equipos a los que desean conectarse los usuarios (así como los servidores RDSH o RDVI) están totalmente actualizados en junio de 2018. Esto incluye las siguientes actualizaciones:

  - Windows Server 2016: KB 4284880, [12 de junio de 2018: KB4284880 (compilación del sistema operativo 14393.2312)](https://support.microsoft.com/en-us/help/4284880/windows-10-update-kb4284880)
  - Windows Server 2012 R2: KB 4284815, [12 de junio de 2018: KB4284815 (paquete acumulativo mensual)](https://support.microsoft.com/en-us/help/4284815/windows-81-update-kb4284815)
  - Windows Server 2012: KB 4284855, [12 de junio de 2018: KB4284855 (paquete acumulativo mensual)](https://support.microsoft.com/en-us/help/4284855/windows-server-2012-update-kb4284855)
  - Windows Server 2008 R2: KB 4284826, [12 de junio de 2018: KB4284826 (paquete acumulativo mensual)](https://support.microsoft.com/en-us/help/4284826/windows-7-update-kb4284826)
  - Windows Server 2008 SP2: KB4056564, [Descripción de la actualización de seguridad para la vulnerabilidad de ejecución de código remoto de CredSSP de Windows Server 2008, Windows Embedded POSReady 2009 y Windows Embedded Standard 2009: 13 de marzo de 2018](https://support.microsoft.com/en-us/help/4056564/security-update-for-vulnerabilities-in-windows-server-2008)

### <a name="users-are-denied-access-on-a-deployment-that-uses-remote-credential-guard-with-multiple-rd-connection-brokers"></a>Se deniega el acceso a los usuarios en una implementación que usa Credential Guard remoto con varios agentes de conexión a Escritorio remoto

Este problema se produce en las implementaciones de alta disponibilidad que utilizan dos, o más, agentes de conexión a Escritorio remoto, siempre que Credential Guard remoto de Windows Defender esté en uso. Los usuarios no pueden iniciar sesión en escritorios remotos.

Este problema se produce porque Credential Guard remoto usa Kerberos para la autenticación y restringe NTLM. Sin embargo, en una configuración de alta disponibilidad con equilibrio de carga, los agentes de conexión a Escritorio remoto no admiten operaciones de Kerberos.

Si necesitas usar una configuración de alta disponibilidad con agentes de conexión a Escritorio remoto con equilibrio de carga, puedes solucionar el problema mediante la deshabilitación de Credential Guard remoto. Para más información acerca de cómo administrar Credential Guard remoto de Windows Defender, consulta [Protección de credenciales de escritorio remoto con Credential Guard remoto de Windows Defender](https://docs.microsoft.com/windows/security/identity-protection/remote-credential-guard#enable-windows-defender-remote-credential-guard).

## <a name="on-connecting-user-receives-remote-desktop-service-is-currently-busy-message"></a>Al conectarse, el usuario recibe el mensaje "Remote Desktop Service is currently busy" (El servicio Escritorio remoto está ocupado actualmente)

Para determinar una respuesta adecuada para este problema, sigue estos pasos:

1. El servicio Servicios de Escritorio remoto deja de responder (por ejemplo, el cliente de Escritorio remoto parece que se"bloquea" en la pantalla de inicio de sesión).  
      - Si el servicio no responde, consulta [Problema de memoria del servidor RDSH](#rdsh-server-memory-issue).
      - Si parece que el cliente interactúa con el servicio de forma normal, vete al paso siguiente.
2. Si uno o varios usuarios desconectan sus sesiones de Escritorio remoto, ¿se pueden volver a conectar?  
   - Si el servicio sigue denegando las conexiones, independientemente del número de usuarios que desconecten sus sesiones, consulta [Problema del cliente de escucha de Escritorio remoto](#rd-listener-issue).
   - Si el servicio vuelve a aceptar conexiones después de que varios usuarios hayan desconectado sus sesiones, [comprueba la directiva del límite de conexiones](#check-the-connection-limit-policy).

### <a name="rdsh-server-memory-issue"></a>Problema de memoria del servidor RDSH

Se ha encontrado una fuga de memoria en algunos servidores RDSH de Windows Server 2012 R2. Con el tiempo, estos servidores empiezan a rechazar tanto las conexiones a Escritorio remoto como los inicios de sesión de la consola local con los mensajes como estos:

> La tarea que intentas realizar no se puede completar porque Servicios de Escritorio remoto está ocupado actualmente. Inténtalo de nuevo al cabo de un rato. Otros usuarios podrán iniciar sesión.

Los clientes de Escritorio remoto que intentan conectarse también dejan de responder.

Para solucionar este problema, reinicia el servidor RDSH.

Para solucionar este problema, aplica KB 4093114, [10 de abril de 2018: KB4093114 (paquete acumulativo mensual)](file:///C:/Users/v-jesits/AppData/Local/Microsoft/Windows/INetCache/Content.Outlook/FUB8OO45/April%2010,%202018—KB4093114%20(Monthly%20Rollup)) a los servidores RDSH.

### <a name="rd-listener-issue"></a>Problema del cliente de escucha de Escritorio remoto

Se ha detectado un problema en algunos servidores RDSH que se han actualizado directamente desde Windows Server 2008 R2 a Windows Server 2012 R2 o Windows Server 2016. Cuando un cliente de Escritorio remoto se conecta al servidor RDSH, este crea un cliente de escucha de escritorio remoto para la sesión del usuario. Los servidores afectados mantienen un recuento de los agentes de escucha de escritorio remoto que aumenta a medida que se conectan usuarios, pero nunca disminuye.

Para solucionar este problema, puedes usar los siguientes métodos:

  - Reinicia el servidor RDSH para restablecer el número de clientes de escucha de escritorio remoto.
  - Modifica la directiva de límite de conexiones, establécela en un valor muy grande. Para más información acerca de la administración de una directiva del límite de conexiones, consulta [Comprobación de la directiva del límite de conexiones](#check-the-connection-limit-policy).

Para resolver este problema, aplique las siguientes actualizaciones a los servidores RDSH:

  - Windows Server 2012 R2: KB 4343891, [30 de agosto de 2018: KB4343891 (versión preliminar del paquete acumulativo mensual)](https://support.microsoft.com/en-us/help/4343891/windows-81-update-kb4343891)
  - Windows Server 2016: KB 4343884, [30 de agosto de 2018: KB4343884 (compilación del sistema operativo 14393.2457)](https://support.microsoft.com/en-us/help/4343884/windows-10-update-kb4343884)

### <a name="check-the-connection-limit-policy"></a>Comprobación de la directiva de límite de conexiones

Puede establecer el límite del número de conexiones simultáneas de Escritorio remotas a nivel de equipo individual o mediante la configuración de un objeto de directiva de grupo (GPO). De manera predeterminada, no se establece el límite.

Para comprobar la configuración actual e identificar todos los GPO existentes en el servidor RDSH, abra una ventana del símbolo del sistema como administrador y escriba el siguiente comando:
  
```
gpresult /H c:\gpresult.html
```
   
Cuando este comando finalice, abre gpresult.html y en **Configuración del equipo\\Plantillas administrativas\\Componentes de Windows\\Servicios de Escritorio remoto\\Host de sesión de Escritorio remoto \\Conexiones**, busca la directiva **Limitar el número de conexiones**.

  - Si el valor de esta directiva es **Deshabilitado**, la directiva de grupo no limita las conexiones RDP.
  - Si el valor de esta directiva es **Habilitado**, comprueba **GPO prevalente**. Si tienes que quitar o cambiar el límite de conexiones, edita este GPO.

Para forzar los cambios en la directiva, abra una ventana del símbolo del sistema en el equipo afectado y escriba el siguiente comando:
  
```
gpupdate /force
```
  
## <a name="rd-client-disconnects-and-cannot-reconnect-to-the-same-session"></a>El cliente de Escritorio remoto se desconecta y no puede volver a conectarse a la misma sesión

Después de que el cliente de Escritorio remoto pierda su conexión con Escritorio remoto, el cliente no se puede volver a conectar de inmediato. El usuario recibe mensajes de error como el siguiente:

  - Debido a un error de seguridad, el cliente no se pudo conectar al servidor de Terminal Server. Cuando te hayas asegurado de que has iniciado sesión la red, intenta volver a conectarte al servidor.
  - Escritorio remoto desconectado. Debido a un error de seguridad, el cliente no se pudo conectar al equipo remoto. Comprueba que has iniciado sesión en la red y vuelve a intentar conectarse.

En un momento posterior, el cliente de Escritorio remoto vuelve a conectar con Escritorio remoto. Sin embargo, en lugar de conectar al cliente a la sesión original, el servidor RDSH conecta el cliente a una nueva sesión. La comprobación del servidor RDSH revela que la sesión original no entró en un estado desconectado, sino que permanece activo.

Para solucionar este problema, puede habilitar la en la carpeta de directivas de grupo **Configurar intervalo entre mensajes de mantenimiento de conexión**  en **Configuración del equipo\\Plantillas administrativas\\Componentes de Windows\\Servicios de Escritorio remoto\\Host de sesión de Escritorio remoto\\Conexiones**. Si habilitas esta directiva, es preciso especificar un intervalo entre mensajes de mantenimiento. Dicho intervalo determina la frecuencia, en minutos,con que el servidor comprueba el estado de sesión.

Este problema puede deberse a una configuración incorrecta de la autenticación. Dichos valores se pueden configurar a nivel de servidor o mediante los GPO. La configuración de la directiva de grupo está disponible en la carpeta de directivas de grupo **Configuración del equipo\\Plantillas administrativas\\Componentes de Windows\\Servicios de Escritorio remoto\\Host de sesión de Escritorio remoto\\ Seguridad**.

1. En el servidor host de sesión de Escritorio remoto, abra la Configuración del host de sesión de Escritorio remoto.
2. En **Conexiones**, haz clic con el botón derecho en el nombre de la conexión y, después, haz clic en **Propiedades**.
3. En el cuadro de diálogo **Propiedades** de la conexión, en la pestaña **General**, en la capa de **Seguridad**, seleccione un método de seguridad.
4. En **Nivel de cifrado**, haga clic en el nivel que desee. Puede seleccionar **Bajo**, **Compatible con el cliente** , **Alto** o **Compatible con FIPS**.

> [!NOTE]  
>  - Cuando las comunicaciones entre los clientes y los servidores de Host de sesión de Escritorio remoto requieran el máximo nivel de cifrado, utilice el cifrado compatible con FIPS.
>  - Todos los valores de nivel de cifrado que configure en la directiva de grupo anulan la configuración que establezca mediante el uso de la herramienta de configuración de Servicios de Escritorio remoto. Además, si habilita la directiva[Criptografía de sistema: usar algoritmos que cumplan FIPS para el cifrado, la firma y las operaciones hash ](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc780081\(v=ws.10\)), esta configuración anula la directiva**Establecer el nivel de cifrado de conexión de cliente** . La directiva de criptografía del sistema está en la carpeta **Configuración del equipo\\Configuración de Windows\\Configuración de seguridad\\Directivas locales\\Opciones de seguridad**.
>  - Al cambiar de nivel de cifrado, el nuevo nivel de cifrado surte efecto la próxima vez un usuario inicia sesión. Si requiere varios niveles de cifrado en un servidor, instale varios adaptadores de red y configure cada uno de ellos por separado.
>  - Para comprobar que el certificado tiene una clave privada correspondiente, en la configuración de Servicios de Escritorio remoto, haga clic en la conexión cuyo certificado desea ver, seleccione **General**, seleccione **Editar**, seleccione el certificado que desea ver y, después, seleccione **Ver certificado**. En la parte inferior de la pestaña **General**, debe aparecer la instrucción, "Tiene una clave privada correspondiente a este certificado". Esta información también se puede ver mediante el complemento Certificados.
>  - El cifrado compatible con FIPS (la directiva **Criptografía de sistema: usar algoritmos que cumplan la norma FIPS para cifrado, aplicación de algoritmo hash y operaciones hash** o el valor **Compatible con FIPS** de la configuración de Servidor de Escritorio remoto) cifra y descifra los datos enviados tanto desde el cliente al servidor como desde el servidor al cliente, con los algoritmos de cifrado del Estándar federal de procesamiento de información (FIPS) 140-1, para lo que se usan los módulos criptográficos de Microsoft. Para más información, consulte [Validación FIPS 140](https://docs.microsoft.com/windows/security/threat-protection/fips-140-validation).
>  - El valor **Alto** cifra los datos enviados desde el cliente al servidor, y viceversa, mediante el uso de cifrado de 128 bits de alta seguridad.
>  - El valor **Compatible con el cliente** cifra los datos enviados entre el cliente y el servidor en el nivel máximo de seguridad de la clave que admita el cliente.
>  - El valor **Baja** cifra los datos enviados desde el cliente al servidor mediante cifrado de 56 bits.

## <a name="remote-laptop-disconnects-from-wireless-network"></a>Los portátiles remotos se desconectan de la red inalámbrica

Este problema puede producirse cuando un cliente de Escritorio remoto se conecta a un portátil mediante una red inalámbrica 802.1X. De forma intermitente, el portátil se desconecta de la red inalámbrica y no se vuelve a conectar automáticamente.

Se trata de un problema conocido que se produce cuando la configuración de autenticación de la red para la conexión de la red inalámbrica es **Autenticación de usuario**.

Para solucionar este problema, establezca la configuración de autenticación de red en **Autenticación de usuarios o equipos**  o en **Autenticación de equipo** .

 > [!NOTE]  
> Para cambiar la configuración de autenticación de la red en un equipo individual, es posible que tenga que usar el panel de control del Centro de redes y recursos compartidos para crear una nueva conexión inalámbrica con la nueva configuración.

Para obtener una descripción completa de cómo configurar una red inalámbrica mediante GPO, consulte [Configuración de directivas de redes inalámbricas (IEEE 802.11)](https://docs.microsoft.com/windows-server/networking/core-network-guide/cncg/wireless/e-wireless-access-deployment#bkmk_policies).

## <a name="user-experiences-poor-performance-or-application-problems"></a>El usuario experimenta un rendimiento deficiente o tiene problemas con las aplicaciones

En esta sección se abordan varios problemas comunes que los usuarios pueden sufrir al usar la funcionalidad de escritorio remoto:

  - [Los usuarios que se conectan a nuevas máquinas virtuales de Microsoft Azure de forma sufren problemas de forma intermitente](#intermittent-problems-with-new-microsoft-azure-virtual-machines).
  - [Los usuarios que se conectan a escritorios remotos de la versión 1709 de Windows 10 pueden tener problemas al reproducir vídeo](#video-playback-issues-on-windows-10-version-1709).
  - [Los usuarios con perfiles de usuario de solo lectura que se conectan a escritorios remotos de Windows 10 no pueden compartir sus escritorios.](#desktop-sharing-issues-on-windows-10)
  - [Los usuarios que se conectan a escritorios remotos de Windows 10 mediante clientes que se ejecutan en versiones anteriores de Windows 10 experimentan un rendimiento lento cuando se deshabilita NLA](#performance-issues-when-mixing-versions-of-windows-10-if-nla-is-disabled)
  - [Cuando los usuarios ejecutan varias aplicaciones en sesiones de Escritorio remoto y se desconectan y conectan con frecuencia, puede encontrarse una pantalla negra al volver a conectarse.](#black-screen-issue)

### <a name="intermittent-problems-with-new-microsoft-azure-virtual-machines"></a>Problemas intermitentes con nuevas máquinas virtuales de Microsoft Azure

Este problema afecta a las máquinas virtuales que se han aprovisionado recientemente. Después de que el usuario se conecta a la máquina virtual, la sesión de Escritorio remoto no puede cargar toda la configuración del usuario correctamente.

Para solucionar este problema, desconecte la máquina virtual, espere al menos 20 minutos y vuelva a conectarla.

Para resolver este problema, aplique las siguientes actualizaciones a las maquinas virtuales, como sea apropiado:

  - Windows 10 y Windows Server 2016: KB 4343884, [30 de agosto de 2018: KB4343884 (compilación del sistema operativo 14393.2457)](https://support.microsoft.com/en-us/help/4343884/windows-10-update-kb4343884)
  - Windows Server 2012 R2: KB 4343891, [30 de agosto de 2018: KB4343891 (versión preliminar del paquete acumulativo mensual)](https://support.microsoft.com/en-us/help/4343891/windows-81-update-kb4343891)

### <a name="video-playback-issues-on-windows-10-version-1709"></a>Problemas de reproducción de vídeo en Windows 10 versión 1709

Este problema se produce cuando los usuarios se conectan a equipos remotos que ejecutan Windows 10, versión 1709. Cuando estos usuarios reproduce vídeo mediante el códec VMR9 (Video Mixing Renderer 9), el reproductor muestra solo una ventana en negro.

Se trata de un problema conocido en Windows 10, versión 1709. El problema no se produce en la versión 1703 de Windows 10.

### <a name="desktop-sharing-issues-on-windows-10"></a>Problemas de uso compartido de escritorio en Windows 10

Este problema se produce cuando el usuario tiene un perfil de usuario de solo lectura (y el subárbol del Registro asociado), como en un escenario de quiosco multimedia. Cuando dicho usuario se conecta a un equipo remoto que ejecuta la versión 1803 de Windows 10, versión 1803, no puede compartir su escritorio.

Para corregir este problema, aplique la actualización 4340917 de Windows 10, [24 de julio de 2018: KB4340917 (compilación de sistema operativo 17134.191)](https://support.microsoft.com/en-us/help/4340917/windows-10-update-kb4340917).

### <a name="performance-issues-when-mixing-versions-of-windows-10-if-nla-is-disabled"></a>Problemas de rendimiento al mezclar versiones de Windows 10, si se deshabilita NLA

Este problema se produce cuando se deshabilita NLA y los equipos cliente de Escritorio remoto que ejecutan Windows 10 se conectan a escritorios remotos que ejecutan diferentes versiones de Windows 10. Los usuarios de clientes de Escritorio remoto en equipos con la versión 1709 de Windows 10, o cualquier versión anterior, experimentan un rendimiento deficiente al conectarse a escritorios remotos que ejecutan la versión 1803 de Windows 10, versión 1803, o cualquier versión posterior.

Esto se debe a que, cuando NLA se deshabilita, los equipos cliente anteriores usan un protocolo más lento cuando se conectan a Windows 10, versión 1803, o a cualquier versión posterior.

Para resolver este problema, aplique el KB 4340917, [24 de julio de 2018: KB4340917 (compilación del sistema operativo 17134.191)](https://support.microsoft.com/en-us/help/4340917/windows-10-update-kb4340917).

### <a name="black-screen-issue"></a>Problema de la pantalla en negro

Este problema se produce en Windows 8.0, Windows 8.1, Windows 10 RTM y Windows Server 2012 R2. Un usuario inicia varias aplicaciones en un equipo de escritorio remoto y luego se desconecta de la sesión. Periódicamente, el usuario se vuelve a conectar con el escritorio remoto para interactuar con las aplicaciones y, después, se vuelve a desconectar. En algún momento, cuando el usuario se vuelve a conectar, la sesión de Escritorio remoto solo muestra una pantalla en negro. El usuario debe terminar la sesión de Escritorio remoto desde la consola del equipo remoto (o la consola del servidor RDSH). Esta acción detiene las aplicaciones.

Para resolver este problema, aplica las siguientes actualizaciones, según sea pertinente:

  - Windows 8 y Windows Server 2012: KB4103719, [17 de mayo de 2018: KB4103719 (versión preliminar del paquete acumulativo mensual)](https://support.microsoft.com/en-us/help/4103719/windows-server-2012-update-kb4103719)
  - Windows 8.1 y Windows Server 2012 R2: KB4103724, [17 de mayo de 2018: KB4103724 (versión preliminar del paquete acumulativo mensual)](https://support.microsoft.com/en-us/help/4103724/windows-81-update-kb4103724) y KB 4284863, [21 de junio de 2018: KB4284863 (versión preliminar del paquete acumulativo mensual)](https://support.microsoft.com/en-us/help/4284863/windows-81-update-kb4284863)
  - Windows 10: Se ha corregido en al KB4284860, [12 de junio de 2018: KB4284860 (compilación del sistema operativo 10240.17889)](https://support.microsoft.com/en-us/help/4284860/windows-10-update-kb4284860)
