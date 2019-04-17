---
title: Administrar Server Core
description: Aprende a administrar una instalación Server Core de Windows Server
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.author: elizapo
ms.localizationpriority: medium
ms.date: 12/18/2018
ms.openlocfilehash: 39fbb92645d39a46613f2142d0258c78a6ba425b
ms.sourcegitcommit: 4df1bc940338219316627ad03f0525462a05a606
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/19/2018
ms.locfileid: "8978002"
---
# Administrar un servidor Server Core

>Se aplica a: Windows Server (canal semianual) y Windows Server 2016

Dado que Server Core no tiene una interfaz de usuario, debes usar los cmdlets de Windows PowerShell, las herramientas de línea de comandos o herramientas remotas para realizar tareas de administración básicas. Las siguientes secciones proporcionan los comandos que se usa para realizar tareas básicas y los cmdlets de PowerShell. También puedes usar [Windows Admin Center](../../manage/windows-admin-center/overview.md), un portal de administración unificada actualmente en la versión preliminar pública, para administrar la instalación. 

## Tareas administrativas mediante los cmdlets de PowerShell
Usar la siguiente información para realizar tareas administrativas básicas con los cmdlets de Windows PowerShell.

### Establecer una dirección IP estática
Cuando se instala un servidor de Server Core, de manera predeterminada tiene dirección A DHCP. Si necesitas una dirección IP estática, puedes establecer mediante los siguientes pasos.

Para ver la configuración de red actual, usa **Get-NetIPConfiguration**.

Para ver las direcciones IP que ya usas, usar **Get-NetIPAddress**.

Para establecer una dirección IP estática, hacer lo siguiente: 

1. Ejecuta **Get-NetIPInterface**. 
2. Ten en cuenta el número de la columna **IfIndex** para la interfaz IP o la cadena de **InterfaceDescription** . Si tienes más de un adaptador de red, ten en cuenta el número o la cadena que corresponde a la interfaz que quiere establecer la dirección IP estática.
3. Ejecuta el siguiente cmdlet para establecer la dirección IP estática:

   ```powershell
   New-NetIPaddress -InterfaceIndex 12 -IPAddress 192.0.2.2 -PrefixLength 24 -DefaultGateway 192.0.2.1
   ```

   donde:
   - **InterfaceIndex** es el valor de **IfIndex** del paso 2. (En nuestro ejemplo, 12)
   - **Dirección IP** es la dirección IP estática que deseas establecer. (En nuestro ejemplo, 191.0.2.2)
   - **PrefixLength** es la longitud del prefijo (otra forma de la máscara de subred) para la dirección IP que establecer. (En nuestro ejemplo, 24)
   - **DefaultGateway** es la dirección IP a la puerta de enlace predeterminada. (En nuestro ejemplo, 192.0.2.1)
4. Para establecer la dirección del servidor del cliente DNS, ejecuta el siguiente cmdlet: 

   ```powershell
   Set-DNSClientServerAddress –InterfaceIndex 12 -ServerAddresses 192.0.2.4
   ```
   
   donde:
   - **InterfaceIndex** es el valor de IfIndex del paso 2.
   - **ServerAddresses** es la dirección IP del servidor DNS.
5. Para agregar varios servidores DNS, ejecuta el siguiente cmdlet: 

   ```powershell
   Set-DNSClientServerAddress –InterfaceIndex 12 -ServerAddresses 192.0.2.4,192.0.2.5
   ```

   donde, en este ejemplo, **192.0.2.4** y **192.0.2.5** son ambas direcciones IP de los servidores DNS.

Si necesitas cambiar a DHCP, ejecutar **conjunto DnsClientServerAddress: InterfaceIndex 12: ResetServerAddresses**.

### Unirse a un dominio
Usa los siguientes cmdlets para unir un equipo a un dominio.

1. Ejecutar **Agregar equipo**. Se te pedirá las credenciales unir el dominio y el nombre de dominio.
2. Si necesitas agregar una cuenta de usuario de dominio al grupo de administradores locales, ejecuta el siguiente comando en un símbolo del sistema (no en la ventana de PowerShell):

   ```
   net localgroup administrators /add <DomainName>\<UserName>
   ```
3. Reinicia el equipo. Puedes hacerlo mediante la ejecución de **Restart-Computer**.

### Cambiar el nombre del servidor
Usa los siguientes pasos para cambiar el nombre del servidor.

1. Determine el nombre actual del servidor con el comando **ipconfig** o **nombre de host** .
2. Ejecutar **cambiar el nombre de equipo - ComputerName \<new_name\ >**.
3. Reinicia el equipo.

### Activación del servidor

Ejecutar **slmgr.vbs – ipk\<productkey\ >**. A continuación, ejecute **slmgr.vbs – /ato**. Si la activación se realiza correctamente, no obtendrá un mensaje.

> [!NOTE]
> También se puede activar el servidor por teléfono, mediante un [servidor de servicio de administración de claves (KMS)](../../get-started/server-2016-activation.md), o de forma remota. Para activar de forma remota, ejecute el siguiente cmdlet desde un equipo remoto: 

>```powershell
>**cscript windows\system32\slmgr.vbs <ServerName> <UserName> <password>:-ato**
>```
 
### Configurar el Firewall de Windows

Firewall de Windows puede configurar localmente en el equipo de Server Core con scripts y los cmdlets de Windows PowerShell. Consulta [NetSecurity](/powershell/module/netsecurity/?view=win10-ps) para los cmdlets que puedes usar para configurar el Firewall de Windows.

### Habilitar la comunicación remota de Windows PowerShell

Puedes habilitar la comunicación remota de PowerShell de Windows, en la que se escriben los comandos de Windows PowerShell en un equipo que se ejecuta en otro equipo. Habilitar la comunicación remota de Windows PowerShell con **Enable-PSRemoting**.

Para obtener más información, consulta [Sobre remoto preguntas más frecuentes](/powershell/module/microsoft.powershell.core/about/about_remote_faq?view=powershell-5.1)
</br>

## Tareas administrativas desde la línea de comandos
Usa la siguiente información de referencia para realizar tareas administrativas desde la línea de comandos.

### Instalación y configuración
|Tarea | Comando |
|-----|-------|
|Establecer la contraseña administrativa local| **Administrador de usuarios netos** \* |
|Unir un equipo a un dominio| **netdom unión % computername %** **/domain:\<domain\ > /userd:\<domain\\username\ >/passwordd:**\* <br> Reinicia el equipo.|
|Confirma que se ha cambiado el dominio| **set** |
|Quitar un equipo de un dominio|**quitar de Netdom: \ < computername\ >**| 
|Agregar un usuario al grupo Administradores local|**NET los administradores de grupo local / agregar \ < domain\\username\ >** |
|Quitar un usuario del grupo de administradores locales|**NET grupo local Administradores /delete \ < domain\\username\ >** |
|Agregar un usuario en el equipo local|**usuario neto \ < domain\username\ > * / Agregar** |
|Agregar un grupo en el equipo local|**grupo local neto \ < grupo name\ > / Agregar**|
|Cambiar el nombre de un equipo unido al dominio|**netdom renamecomputer % computername % /NewName:\<new equipo name\ > /userd:\<domain\\username\ >/passwordd:** * |
|Confirma que el nuevo nombre de equipo|**set**| 
|Cambiar el nombre de un equipo en un grupo de trabajo|**netdom renamecomputer \ / NewName < currentcomputername\ >: \ < newcomputername\ >** <br>Reinicia el equipo.|
|Deshabilitar la administración de archivos de paginación|**sistema de equipo WMIC donde name = "\ < computername\ >" establecer AutomaticManagedPagefile = False**| 
|Configurar el archivo de paginación|**WMIC pagefileset donde name = "\ < ruta de acceso/filename\ >" establecer Tamaño_inicial = \ < initialsize\ >, MaximumSize = \ < maxsize\ >** <br>Donde *ruta de acceso y nombre de archivo* es la ruta de acceso y el nombre del archivo de paginación, *tamaño_inicial* es el tamaño inicial del archivo de paginación, en bytes, y *maxsize* es el tamaño máximo del archivo de paginación, en bytes.|
|Cambiar a una dirección IP estática|**ipconfig/all** <br>Registra la información pertinente o redirigir a un archivo de texto (**ipconfig/all > ipconfig.txt**).<br>**interfaces de presentación de ipv4 de interfaz de Netsh**<br>Comprueba que hay una lista de interfaces.<br>**netsh interface ipv4 establecer el nombre de la dirección \ origen < Id. de la interfaz list\ > = dirección estática = \ < preferido IP mailto:\[Email address\ > puerta de enlace = \ < puerta de enlace mailto:\[Email address\ >**<br>Ejecutar **ipconfig/all** a vierfy que **No**está establecido DHCP habilitado.|
|Establece una dirección estática de DNS.|**netsh interface ipv4 agregar servidorDNS nombre = \<name o el Id. de la card\ de interfaz de red > dirección = \<IP dirección de la server\ DNS principal > índice = 1 <br> **netsh interface ipv4 agregar servidorDNS name = \<name de secundario server\ DNS > dirección = \<IP dirección de la secundaria server\ DNS > índice = 2 ** <br> Repite según corresponda agregar servidores adicionales.<br>Ejecutar **ipconfig/all** para comprobar que las direcciones son correctas.|
|Cambiar a una dirección IP de DHCP proporcionado por el de una dirección IP estática|**nombre de la dirección del conjunto de netsh interface ipv4 = \ origen < dirección IP de FAT32\ local > = DHCP** <br>Ejecutar **Ipconfig/all** para comprobar que DHCP habilitado está establecida en **Sí**.|
|Escribe una clave de producto|**slmgr.vbs – ipk \ < clave\ del producto >**| 
|Activación del servidor de forma local|**slmgr.vbs - /ato**| 
|Activación del servidor de forma remota|**cscript slmgr.vbs – ipk \ < clave\ producto > \ < servidor name\ > \ < username\ > \ < password\ >** <br>**cscript slmgr.vbs - /ato \ < servername\ > \ < username\ > \ < password\ >** <br>Obtener el GUID del equipo mediante la ejecución de **cscript slmgr.vbs-hiciste** <br> Ejecutar **cscript slmgr.vbs - dli \<GUID\ >** <br>Comprueba que el estado de la licencia se establece en **con licencia (activado)**.


### Las redes y el firewall

|Tarea|Comando| 
|----|-------|
|Configurar el servidor para usar un servidor proxy|**netsh Winhttp establece el proxy \ < servername\ >: \ < puerto number\ >** <br>**Nota:** Las instalaciones de Server Core no pueden acceder a Internet a través de un proxy que requiera una contraseña para permitir conexiones.|
|Configurar el servidor para utilizar al servidor proxy para direcciones de Internet|**netsh winttp establece el proxy \ < servername\ >: \ < puerto number\ > lista de omisión = "\ < local\ >"**| 
|Mostrar o modificar la configuración de IPSEC|**netsh ipsec**| 
|Mostrar o modificar la configuración de NAP|**netsh nap**| 
|Mostrar o modificar IP con traducción de direcciones físico|**arp**| 
|Mostrar o configurar la tabla de enrutamiento local|**ruta**| 
|Ver o configurar el servidor DNS|**nslookup**| 
|Mostrar las estadísticas de protocolos y las conexiones de red TCP/IP actuales|**netstat**| 
|Mostrar las estadísticas de protocolos y las conexiones de TCP/IP actuales mediante NetBIOS a través de TCP/IP (NBT)|**nbtstat**| 
|Mostrar saltos para las conexiones de red|**pathping**| 
|Saltos de seguimiento para las conexiones de red|**tracert**| 
|Mostrar la configuración de enrutador de multidifusión|**mrinfo**| 
|Habilitar la administración remota del servidor de seguridad|**netsh advfirewall firewall establece el grupo de reglas = "Administración remota del Firewall de Windows" nuevo enable = yes**| 
 

### Las actualizaciones, el informe de errores y comentarios

|Tarea|Comando| 
|----|-------|
|Instalar una actualización|**Wusa \ < update\ > .msu /quiet**| 
|Actualizaciones de la lista instalada|**systeminfo**| 
|Quitar una actualización|**expande /f:\* \ < update\ > .msu c:\test** <br>Ve a c:\test\ y abre \ < update\ > .xml en un editor de texto.<br>Reemplazar **instalar** con **Quitar** y guarda el archivo.<br>**pkgmgr /n:\ < update\ > .xml**|
|Configurar las actualizaciones automáticas|Para comprobar la configuración actual: ** cscript %systemroot%\system32\scregedit.wsf/au /v **<br>Para habilitar las actualizaciones automáticas: **cscript scregedit.wsf/au 4** <br>Para desactivar las actualizaciones automáticas: **cscript %systemroot%\system32\scregedit.wsf/au 1**| 
|Habilitar los informes de error|Para comprobar la configuración actual: **/query serverWerOptin** <br>Para enviar automáticamente informes detallados: **serverWerOptin / detallada** <br>Para enviar automáticamente los informes de resumen: **serverWerOptin /summary** <br>Para deshabilitar informe de errores: **/ Disable serverWerOptin**|
|Participar en el programa para la mejora de experiencia (del usuario CEIP) de cliente|Para comprobar la configuración actual: **/query serverCEIPOptin** <br>Para habilitar el CEIP: **/ Enable serverCEIPOptin** <br>Para deshabilitar el CEIP: **/ Disable serverCEIPOptin**|

### Servicios, procesos y rendimiento

|Tarea|Comando| 
|----|-------|
|Lista de los servicios de ejecución|**consulta de SC** o el **comando net start**| 
|Iniciar un servicio|**sc iniciar \<service name\ >** o **del comando net start \<service name\ >**| 
|Detener un servicio|**sc stop \<service name\ >** o **net stop \<service name\ >**| 
|Recuperar una lista de aplicaciones y los procesos asociados en ejecución|**tasklist**||Detener forzar un proceso|Ejecutar **tasklist** recuperar el identificador de proceso (PID) y luego ejecutar **taskkill /PID \<process ID\ >**|
|Administrador de tareas de inicio|**Taskmgr**| 
|Crear y administrar los registros de rendimiento y la sesión de seguimiento de eventos|Para crear un contador, seguimiento, recopilación de datos de configuración o API: **logman cree** <br>A las propiedades del recopilador de datos de consulta: **logman query** <br>Para iniciar o detener la recopilación de datos: **logman start\ | detener** <br>Para eliminar un selector: **logman delete** <br> Para actualizar las propiedades de un selector: **logman update** <br>Para importar un conjunto de recopilador de datos desde un archivo XML o exportar a un archivo XML: **logman import\ | exportar**|

### Registros de eventos

|Tarea|Comando| 
|----|-------|
|Registros de eventos de lista|**el wevtutil**| 
|Eventos de consulta en un registro especificado|**wevtutil qe /f:text \ < registrar name\ >**| 
|Exportar un registro de eventos|**wevtutil epl \ < registrar name\ >**| 
|Borrar un registro de eventos|**wevtutil cl \ < registrar name\ >**| 


### Sistema de archivos y de disco

|Tarea|Comando|
|----|-------|
|Administrar las particiones de disco|Para obtener una lista completa de comandos, ejecutar **diskpart /?**|  
|Administrar el software RAID|Para obtener una lista completa de comandos, ejecutar **diskraid /?**|  
|Administrar los puntos de montaje|Para obtener una lista completa de comandos, ejecutar **mountvol /?**| 
|Desfragmentar un volumen|Para obtener una lista completa de comandos, ejecutar **de desfragmentación /?**|  
|Convertir un volumen al sistema de archivos NTFS|**convertir \ < volumen letter\ > NTFS**| 
|Compacta un archivo|Para obtener una lista completa de comandos, ejecutar **compact /?**|  
|Administrar archivos abiertos|Para obtener una lista completa de comandos, ejecutar **openfiles /?**|  
|Administrar carpetas VSS|Para obtener una lista completa de comandos, ejecutar **vssadmin /?**| 
|Administrar el sistema de archivos|Para obtener una lista completa de comandos, ejecutar **fsutil /?**||Comprobar la firma de un archivo|**¿Sigverif /?**| 
|Tomar posesión de un archivo o carpeta|Para obtener una lista completa de comandos, ejecutar **icacls /?**| 
 
### Hardware

|Tarea|Comando| 
|----|-------|
|Agregar un controlador para un dispositivo de hardware nuevo|Copia el controlador en una carpeta en %homedrive%\\\ < controlador folder\ >. Ejecutar **pnputil -i - un folder\ %homedrive%\\\<driver > \\\<driver\ > .inf**|
|Quitar un controlador para un dispositivo de hardware|Para obtener una lista de controladores cargados, ejecutar **tipo de consulta sc = controlador**. A continuación, ejecute **sc eliminar \<service_name\ >**|
