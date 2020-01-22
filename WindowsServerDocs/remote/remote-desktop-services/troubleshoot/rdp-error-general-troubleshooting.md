---
title: Solución de problemas generales de conexión con Escritorio remoto
description: Solución del error "Clase no registrada" con la conexión a Escritorio remoto.
audience: itpro
ms.custom: na
ms.reviewer: rklemen
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.assetid: ''
author: kaushika-msft
manager: dcscontentpm
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: b934a585b3058cc2eec642cdb1234c8c9a015544
ms.sourcegitcommit: c5709021aa98abd075d7a8f912d4fd2263db8803
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/18/2020
ms.locfileid: "76265957"
---
# <a name="general-remote-desktop-connection-troubleshooting"></a>Solución de problemas generales de conexión con Escritorio remoto

Sigue estos pasos si un cliente de Escritorio remoto no se puede conectar a un escritorio remoto, pero no se proporcionan mensajes u otros síntomas que ayuden a identificar la causa.

## <a name="check-the-status-of-the-rdp-protocol"></a>Comprobación del estado del RDP

### <a name="check-the-status-of-the-rdp-protocol-on-a-local-computer"></a>Comprobación del estado del RDP en un equipo local

Para comprobar y cambiar el estado de RDP en un equipo local, consulta [Cómo habilitar Escritorio remoto](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-allow-access#how-to-enable-remote-desktop).

> [!NOTE]  
> Si las opciones de Escritorio remoto no están disponibles, consulta [Compruebe si un objeto de directiva de grupo (GPO) está bloqueando el RDP en un equipo local](#check-whether-a-group-policy-object-gpo-is-blocking-rdp-on-a-local-computer).

### <a name="check-the-status-of-the-rdp-protocol-on-a-remote-computer"></a>Comprobación del estado de RDP en un equipo remoto

> [!IMPORTANT]  
> Sigue detenidamente las instrucciones de esta sección. Se pueden producir problemas graves si el Registro se modifica de forma incorrecta. Antes de empezar a modificar el Registro, [haz una copia de seguridad del Registro](https://support.microsoft.com/help/322756) para poder restaurarlo en caso de que se produzca algún error.

Para comprobar el estado de RDP en un equipo remoto y cambiarlo usa una conexión del registro de red:

1. En primer lugar, vete al menú **Inicio** y selecciona **Ejecutar**. En el cuadro de texto que aparece, escribe **regedt32**.
2. En el Editor del Registro, selecciona **Archivo** y después **Conectar al Registro de red**.
3. En el cuadro de diálogo **Seleccionar Equipo**, escriba el nombre del equipo remoto, seleccione **Comprobar nombres**y, después, selecciona **Aceptar**.
4. Vete a **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server**.  
   ![Editor del Registro, que muestra la entrada fDenyTSConnections](../media/troubleshoot-remote-desktop-connections/RegEntry_fDenyTSConnections.png)
   - Si el valor de la clave **fDenyTSConnections** es **0**, RDP está habilitado.
   - Si el valor de la clave **fDenyTSConnections** es **1**, RDP está deshabilitado.
5. Para habilitar RDP, cambia el valor de **fDenyTSConnections** de **1** a **0**.

### <a name="check-whether-a-group-policy-object-gpo-is-blocking-rdp-on-a-local-computer"></a>Comprueba si hay algún un objeto de directiva de grupo (GPO) que bloquee el RDP en un equipo local

Si RDP no se puede activar en la interfaz de usuario o si el valor de **fDenyTSConnections** se revierte a **1** después de que lo hayas cambiado, es posible que un GPO reemplace la configuración de nivel de equipo.

Para comprobar la configuración de la directiva de grupo en un equipo local, abre una ventana del símbolo del sistema como administrador y escribe el siguiente comando:

```cmd
gpresult /H c:\gpresult.html
```

Cuando este comando finalice, abre gpresult.html. En **Configuración del equipo\\Plantillas administrativas\\Componentes de Windows\\Servicios de Escritorio remoto\\Host de sesión de Escritorio remoto \\Conexiones**, busca la directiva **Permitir que los usuarios se conecten de forma remota mediante Servicios de Escritorio remoto**.

- Si el valor de esta directiva es **Habilitado**, la directiva de grupo no bloquea las conexiones RDP.
- Si el valor de esta directiva es **Deshabilitado**, comprueba **GPO prevalente**. Éste es el GPO que bloquea conexiones RDP.
  ![Un segmento de ejemplo de gpresult.html, en el que el GPO de nivel de dominio ** Bloque RDP ** está deshabilitando RDP.](../media/troubleshoot-remote-desktop-connections/GPResult_RDSH_Connections_GP.png)
   
  ![Un segmento de ejemplo de gpresult.html, en el que **Directiva de grupo local** está deshabilitando RDP.](../media/troubleshoot-remote-desktop-connections/GPResult_RDSH_Connections_LGP.png)

### <a name="check-whether-a-gpo-is-blocking-rdp-on-a-remote-computer"></a>Comprueba si un GPO bloquea el RDP en un equipo remoto

Para comprobar la configuración de la directiva de grupo en un equipo remoto, el comando es prácticamente el mismo que en un equipo local:

```cmd
gpresult /S <computer name> /H c:\gpresult-<computer name>.html
```

El archivo que genera este comando (**gpresult -\<nombre de equipo\>.html**) usa el mismo formato de información que la versión del equipo local (**gpresult.html**).

### <a name="modifying-a-blocking-gpo"></a>Modificación de un GPO de bloqueo

Esta configuración se puede modificar en el Editor de objetos de directiva de grupo (GPE) y en la Consola de administración de directivas de grupo (GPM). Para más información acerca de cómo usar la directiva de grupo, consulta [Administración avanzada de directivas de grupo](https://docs.microsoft.com/microsoft-desktop-optimization-pack/agpm/).

Para modificar la directiva de bloqueo, usa uno de estos métodos:

- En GPE, accede al nivel apropiado de GPO (como local o dominio), y vete a **Configuración del equipo** > **Plantillas administrativas** > **Componentes de Windows** > **Servicios de Escritorio remoto** > **Host de sesión de Escritorio remoto** > **Conexiones** > **Permitir que los usuarios se conecten de forma remota mediante Servicios de Escritorio remoto**.  
   1. Establece la directiva en **Habilitado** o **No configurado**.
   2. En los equipos afectados, abre una ventana del símbolo del sistema como administrador y ejecuta el comando **gpupdate /force**.
- En GPM, vete a la unidad organizativa (UO) en la que la directiva de bloqueo se aplica a los equipos afectados y elimínala.

## <a name="check-the-status-of-the-rdp-services"></a>Comprobación del estado de los servicios de RDP

Los siguientes servicios deben estar en ejecución tanto en el equipo local (cliente) como en el remoto (destino):

- Servicios de Escritorio remoto (TermService)
- Redirector de puerto en modo usuario de Servicios de Escritorio remoto (UmRdpService)

Puedes usar el complemento MMC Servicios para administrar los servicios de forma local o remota. También puedes usar PowerShell para administrar los servicios de forma local o remota (si el equipo remoto está configurado para aceptar cmdlets de PowerShell remotos).

![Servicios de Escritorio remoto en el complemento MMC Servicios. No modifiques la configuración predeterminada del servicio.](../media/troubleshoot-remote-desktop-connections/RDSServiceStatus.png)

Si alguno de los servicios, o ambos, no están en ejecución en cualquiera de los equipos, inícialos.

> [!NOTE]  
> Si inicias el servicio Servicios de Escritorio remoto, haz clic en **Sí** para reiniciar automáticamente el Redirector de puerto en modo usuario de Servicios de Escritorio remoto.

## <a name="check-that-the-rdp-listener-is-functioning"></a>Comprobación de que el cliente de escucha de RDP funciona

> [!IMPORTANT]  
> Sigue detenidamente las instrucciones de esta sección. Se pueden producir problemas graves si el Registro se modifica de forma incorrecta. Antes de empezar a modificar el Registro, [haz una copia de seguridad del Registro](https://support.microsoft.com/help/322756) para poder restaurarlo en caso de que se produzca algún error.

### <a name="check-the-status-of-the-rdp-listener"></a>Comprobación del estado del cliente de escucha de RDP

Para este procedimiento, usa una instancia de PowerShell que tenga permisos administrativos. En los equipos locales, también puedes usar un símbolo del sistema en el que tengas permisos administrativos. Pero en este procedimiento se usa PowerShell porque los mismos cmdlets funcionan tanto local como remotamente.

1. Para conectarse a un equipo remoto, ejecuta el cmdlet siguiente:

   ```powershell
   Enter-PSSession -ComputerName <computer name>
   ```

2. Escribe **qwinsta**. 
    ![El comando qwinsta enumera los procesos que escuchan en los puertos del equipo.](../media/troubleshoot-remote-desktop-connections/WPS_qwinsta.png)
3. Si la lista incluye **rdp-tcp** con el estado **Escuchar**, significa que el cliente de escucha de RDP está trabajando. Pasa a [Comprobación del puerto del cliente de escucha de RDP](#check-the-rdp-listener-port). De lo contrario, vete al paso 4.
4. Exporta la configuración del cliente de escucha de RDP desde un equipo de trabajo.
    1. Inicia sesión en un equipo que tenga la misma versión del sistema operativo que el equipo afectado y accede al Registro de dicho equipo (por ejemplo, mediante el Editor del Registro).
    2. Vete a la siguiente entrada del Registro:  
        **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\RDP-Tcp**
    3. Exporta la entrada a un archivo. reg. Por ejemplo, en el Editor del registro, haz clic con el botón derecho en la entrada, selecciona **Exportar**y escriba un nombre de archivo para la configuración exportada.
    4. Copia el archivo .reg exportado en el equipo afectado.
5. Para importar la configuración del cliente de escucha de RDP, abre una ventana de PowerShell que tenga permisos administrativos en el equipo afectado (o abre la ventana de PowerShell y conéctate de forma remota al equipo afectado).
   1. Para realizar una copia de seguridad de la entrada del Registro existente, escribe el cmdlet siguiente:  
   
      ```powershell  
      cmd /c 'reg export "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-tcp" C:\Rdp-tcp-backup.reg'   
      ```  
   

   2. Para eliminar la entrada del Registro existente, escribe los cmdlets siguientes:  
   
      ```powershell  
      Remove-Item -path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-tcp' -Recurse -Force  
      ```
   
   3. Para importar la nueva entrada del registro y después reiniciar el servicio, escribe los cmdlets siguientes:  
   
      ```powershell  
      cmd /c 'regedit /s c:\<filename>.reg'  
      Restart-Service TermService -Force  
      ```
      
      Reemplaza \<nombreDeArchivo\> por el nombre del archivo .reg exportado.

6. Para probar la configuración vuelve a intentar establecer la conexión a Escritorio remoto. Si sigues sin poder conectarte, reinicia el equipo afectado.
7. Si sigues sin poder conectarte, [comprueba el estado del certificado autofirmado de RDP](#check-the-status-of-the-rdp-self-signed-certificate).

### <a name="check-the-status-of-the-rdp-self-signed-certificate"></a>Comprobación del estado del certificado autofirmado de RDP

1. Si sigues sin poder conectarte, abre el complemento MMC Certificados. Cuando se te pida que selecciones el almacén de certificados que deseas administrar, selecciona **Cuenta de equipo**y, después, selecciona el equipo afectado.
2. En la carpeta **Certificates** de **Remote Desktop**, elimina el certificado autofirmado de RDP. 
    ![Certificados de Escritorio remoto en el complemento MMC Certificados.](../media/troubleshoot-remote-desktop-connections/MMCCert_Delete.png)
3. En el equipo afectado, reinicia el servicio Servicios de Escritorio remoto.
4. Actualiza el complemento Certificados.
5. Si no se ha vuelto a crear el certificado autofirmado de RDP, [comprueba los permisos de la carpeta MachineKeys](#check-the-permissions-of-the-machinekeys-folder).

### <a name="check-the-permissions-of-the-machinekeys-folder"></a>Comprobación de los permisos de la carpeta MachineKeys

1. En el equipo afectado, abre el explorador y vete a **C:\\ProgramData\\Microsoft\\Crypto\\RSA\\** .
2. Haz clic en **MachineKeys**, selecciona **Propiedades**, **Seguridad**y, después, selecciona **Avanzadas**.
3. Asegúrate de que están configurados los siguientes permisos:
      - Builtin\\administradores: Control total
      - Todos: Lectura y escritura

## <a name="check-the-rdp-listener-port"></a>Comprobación del puerto del cliente de escucha de RDP

El cliente de escucha de RDP debería escuchar en el puerto 3389 tanto en el equipo local (cliente) como en el remoto (destino). Ninguna otra aplicación debería usar dicho puerto.

> [!IMPORTANT]  
> Sigue detenidamente las instrucciones de esta sección. Se pueden producir problemas graves si el Registro se modifica de forma incorrecta. Antes de empezar a modificar el Registro, [haz una copia de seguridad del Registro](https://support.microsoft.com/help/322756) para poder restaurarlo en caso de que se produzca algún error.

Para comprobar o cambiar el puerto de RDP, usa el Editor del Registro:

1. En el menú Inicio, selecciona **Ejecutar** y, después, escribe **regedt32** en el cuadro de texto que aparece.
      - Para establecer una conexión con un equipo remoto, selecciona **Archivo**y, después, selecciona **Conectar al Registro de red**.
      - En el cuadro de diálogo **Seleccionar Equipo**, escriba el nombre del equipo remoto, seleccione **Comprobar nombres**y, después, selecciona **Aceptar**.
2. Abre el registro y vete al cliente de escucha **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\\<\>** . 
    ![La subclave PortNumber de RDP.](../media/troubleshoot-remote-desktop-connections/RegEntry_PortNumber.png)
3. Si el valor de **PortNumber** no es **3389**, cámbialo a **3389**. 
   > [!IMPORTANT]  
    > Servicios de Escritorio remoto se puede manejar desde otro puerto. Pero no se recomienda hacerlo. En este artículo no se explica cómo solucionar problemas en ese tipo de configuración.
4. Después de cambiar el número de puerto, reinicia el servicio Servicios de Escritorio remoto.

### <a name="check-that-another-application-isnt-trying-to-use-the-same-port"></a>Comprueba que otra aplicación no intenta usar el mismo puerto

Para este procedimiento, usa una instancia de PowerShell que tenga permisos administrativos. En los equipos locales, también puedes usar un símbolo del sistema en el que tengas permisos administrativos. Pero en este procedimiento se usa PowerShell porque los mismos cmdlets funcionan de forma local y remota.

1. Abra una ventana de PowerShell. Para conectarse a un equipo remoto, escribe **Enter-PSSession - ComputerName \<nombrede equipo\>** .
2. Escriba el comando siguiente:  
   
     ```powershell  
    cmd /c 'netstat -ano | find "3389"'  
    ```
  
    ![El comando netstat genera una lista de puertos y los servicios que los escuchan.](../media/troubleshoot-remote-desktop-connections/WPS_netstat.png)
3. Busque una entrada para el puerto TCP 3389 (o el puerto RDP asignado) con el estado **Escuchando**. 
    > [!NOTE]  
   > El PID (identificador de proceso) del proceso o servicio que usa ese puerto aparece en la columna PID.
4. Para determinar qué aplicación utiliza el puerto 3389 (o el puerto de RDP asignado), escribe el siguiente comando:  
   
     ```powershell  
    cmd /c 'tasklist /svc | find "<pid listening on 3389>"'  
    ```  
  
    ![El comando tasklist informa de los detalles de un proceso específico.](../media/troubleshoot-remote-desktop-connections/WPS_tasklist.png)
5. Busca una entrada que contenga el número de PID asociado al puerto (desde la salida de **netstat**). Los servicios o los procesos asociados con ese PID aparecen en la columna derecha.
6. Si una aplicación o servicio que no sea Servicios de Escritorio remoto (TermServ.exe) usa el puerto, el conflicto se puede mediante uno de estos métodos:
      - Configura la otra aplicación o servicio para que utilicen un puerto diferente (se recomienda).
      - Desinstala la otra aplicación o servicio.
      - Configura RDP para que utilice otro puerto diferente y, después, reinicia el servicio Servicios de Escritorio remoto (no se recomienda).

### <a name="check-whether-a-firewall-is-blocking-the-rdp-port"></a>Comprobación de si un firewall está bloqueando el puerto de RDP

Usa la herramienta **psping** para probar si puedes acceder al equipo afectado a través del puerto 3389.

1. Ve a otro equipo que no esté afectado y descarga **psping** desde <https://live.sysinternals.com/psping.exe>.
2. Abre una ventana del símbolo del sistema como administrador, cambia al directorio en el que has instalado **psping** y, después, escribe el comando siguiente:  
   
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
