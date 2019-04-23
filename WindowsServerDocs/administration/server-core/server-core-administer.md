---
title: Administrar Server Core
description: Obtenga información sobre cómo administrar una instalación Server Core de Windows Server
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.author: elizapo
ms.localizationpriority: medium
ms.date: 12/18/2018
ms.openlocfilehash: 39fbb92645d39a46613f2142d0258c78a6ba425b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842666"
---
# <a name="administer-a-server-core-server"></a>Administrar un servidor Server Core

>Se aplica a: Windows Server (canal semianual) y Windows Server 2016

Puesto que Server Core no tiene una interfaz de usuario, deberá usar cmdlets de Windows PowerShell, las herramientas de línea de comandos o herramientas remotas para realizar tareas de administración básicas. Las siguientes secciones describen los cmdlets de PowerShell y comandos que se usan para realizar tareas básicas. También puede usar [Windows Admin Center](../../manage/windows-admin-center/overview.md), un portal de administración unificada actualmente en versión preliminar pública, para administrar la instalación. 

## <a name="administrative-tasks-using-powershell-cmdlets"></a>Tareas administrativas mediante los cmdlets de PowerShell
Utilice la siguiente información para realizar tareas administrativas básicas con cmdlets de Windows PowerShell.

### <a name="set-a-static-ip-address"></a>Establecer una dirección IP estática
Cuando se instala un servidor server Core, de forma predeterminada tiene dirección A DHCP. Si necesita una dirección IP estática, puede establecer mediante los pasos siguientes.

Para ver la configuración de red actual, use **Get NetIPConfiguration**.

Para ver las direcciones IP que ya está usando, use **Get-NetIPAddress**.

Para establecer una dirección IP estática, realice lo siguiente: 

1. Ejecute **Get-NetIPInterface**. 
2. Tenga en cuenta el número en el **IfIndex** columna para la interfaz IP o el **InterfaceDescription** cadena. Si tiene más de un adaptador de red, tenga en cuenta el número o cadena correspondiente a la interfaz que desea establecer la dirección IP estática.
3. Ejecute el siguiente cmdlet para establecer la dirección IP estática:

   ```powershell
   New-NetIPaddress -InterfaceIndex 12 -IPAddress 192.0.2.2 -PrefixLength 24 -DefaultGateway 192.0.2.1
   ```

   Donde:
   - **InterfaceIndex** es el valor de **IfIndex** del paso 2. (En nuestro ejemplo, 12)
   - **IPAddress** es la dirección IP estática que desea establecer. (En nuestro ejemplo, 191.0.2.2)
   - **PrefixLength** es la longitud del prefijo (otra forma de máscara de subred) para la dirección IP que está configurando. (Para nuestro ejemplo, 24)
   - **DefaultGateway** es la dirección IP para la puerta de enlace predeterminada. (Para nuestro ejemplo, 192.0.2.1)
4. Para configurar al cliente DNS de dirección del servidor, ejecute el siguiente cmdlet: 

   ```powershell
   Set-DNSClientServerAddress –InterfaceIndex 12 -ServerAddresses 192.0.2.4
   ```
   
   Donde:
   - **InterfaceIndex** es el valor de IfIndex del paso 2.
   - **ServerAddresses** es la dirección IP del servidor DNS.
5. Para agregar varios servidores DNS, ejecute el siguiente cmdlet: 

   ```powershell
   Set-DNSClientServerAddress –InterfaceIndex 12 -ServerAddresses 192.0.2.4,192.0.2.5
   ```

   WHERE, en este ejemplo, **tanto 192.0.2.4** y **como 192.0.2.5** son direcciones IP de servidores DNS.

Si tiene que cambiar para usar DHCP, ejecute **Set-DnsClientServerAddress – InterfaceIndex 12: ResetServerAddresses**.

### <a name="join-a-domain"></a>Unirse a un dominio
Use los siguientes cmdlets para unir un equipo a un dominio.

1. Ejecute **Add-Computer**. Se le pedirá tanto las credenciales para unirse al dominio y el nombre de dominio.
2. Si tiene que agregar una cuenta de usuario de dominio al grupo de administradores local, ejecute el siguiente comando en un símbolo del sistema (no en la ventana de PowerShell):

   ```
   net localgroup administrators /add <DomainName>\<UserName>
   ```
3. Reinicie el equipo. Puede hacerlo mediante la ejecución de **Restart-Computer**.

### <a name="rename-the-server"></a>Cambiar el nombre del servidor
Siga estos pasos para cambiar el nombre del servidor.

1. Determine el nombre actual del servidor con el **hostname** o **ipconfig** comando.
2. Ejecute **Rename-Computer - ComputerName \<new_name\>**.
3. Reinicie el equipo.

### <a name="activate-the-server"></a>Activar el servidor

Ejecute **slmgr.vbs – ipk\<productkey\>**. A continuación, ejecute **slmgr.vbs – ato**. Si la activación se realiza correctamente, no obtendrá un mensaje.

> [!NOTE]
> También puede activar el servidor por teléfono, mediante un [servidor del servicio de administración de claves (KMS)](../../get-started/server-2016-activation.md), o de forma remota. Para activar de forma remota, ejecute el siguiente cmdlet desde un equipo remoto: 

>```powershell
>**cscript windows\system32\slmgr.vbs <ServerName> <UserName> <password>:-ato**
>```
 
### <a name="configure-windows-firewall"></a>Configurar el Firewall de Windows

Puede configurar el Firewall de Windows de forma local en el equipo Server Core mediante cmdlets y scripts de Windows PowerShell. Consulte [NetSecurity](/powershell/module/netsecurity/?view=win10-ps) para los cmdlets que puede usar para configurar el Firewall de Windows.

### <a name="enable-windows-powershell-remoting"></a>Habilitar la comunicación remota de Windows PowerShell

Puede habilitar la comunicación remota de Windows PowerShell, con la cual los comandos de Windows PowerShell escritos en un equipo se ejecutan en otro. Habilitar la comunicación remota de Windows PowerShell con **Enable-PSRemoting**.

Para obtener más información, consulte [sobre remoto preguntas más frecuentes](/powershell/module/microsoft.powershell.core/about/about_remote_faq?view=powershell-5.1)
</br>

## <a name="administrative-tasks-from-the-command-line"></a>Tareas administrativas desde la línea de comandos
Utilice la siguiente información de referencia para realizar tareas administrativas desde la línea de comandos.

### <a name="configuration-and-installation"></a>Configuración e instalación
|Tarea | Comando |
|-----|-------|
|Establecer la contraseña administrativa local| **Administrador de red del usuario** \* |
|Unir un equipo a un dominio| **Netdom join % computername %** **/Domain:\<dominio\> /userd:\<dominio\\username \> /passwordd:**\* <br> Reinicie el equipo.|
|Confirmar que el dominio ha cambiado| **set** |
|Quitar un equipo de un dominio|**Netdom remove \<computername\>**| 
|Agregar un usuario al grupo Administradores local|**net localgroup Administrators / agregar \<dominio\\nombre de usuario\>** |
|Quitar un usuario del grupo Administradores local|**net localgroup administradores /delete \<dominio\\nombre de usuario\>** |
|Agregar un usuario al equipo local|**usuario neto \<dominio\nombre de usuario\> * / agregar** |
|Agregar un grupo al equipo local|**net localgroup \<group name\> /add**|
|Cambiar el nombre de un equipo unido a un dominio|**netdom renamecomputer % computername % /NewName:\<nuevo nombre de equipo\> /userd:\<dominio\\username \> /passwordd:** * |
|Confirmar el nuevo nombre de un equipo|**set**| 
|Cambiar el nombre de un equipo en un grupo de trabajo|**netdom renamecomputer \<nombreDeEquipoActual\> /newname:\<nombreDeEquipoNuevo\>** <br>Reinicie el equipo.|
|Deshabilitar la administración de archivos de paginación|**WMIC computersystem donde nombre = "\<computername\>" establecer AutomaticManagedPagefile = False**| 
|Configurar el archivo de paginación|**WMIC pagefileset donde nombre = "\<path/filename\>" establecer InitialSize =\<initialsize\>, MaximumSize =\<maxsize\>** <br>Donde *path/filename* es la ruta de acceso y nombre del archivo de paginación, *initialsize* es el tamaño inicial del archivo de paginación, en bytes, y *maxsize* es el tamaño máximo de la archivo de paginación, en bytes.|
|Cambiar a una dirección IP estática|**ipconfig /all** <br>Registre la información pertinente o redirigirla a un archivo de texto (**ipconfig/all > ipconfig.txt**).<br>**netsh interfaz ipv4 show interfaces**<br>Compruebe que hay una lista de interfaces.<br>**nombre de la dirección del conjunto de netsh interface ipv4 \<Id. de la lista de interfaces\> origen = dirección estática =\<preferido de la dirección IP\> puerta de enlace =\<dirección de puerta de enlace\>**<br>Ejecute **ipconfig/all** a vierfy que DHCP habilitado se establece en **No**.|
|Establecer una dirección DNS estática.|**netsh interface ipv4 Agregar nombre dnsserver =\<nombre o Id. de la tarjeta de interfaz de red\> dirección =\<dirección IP del servidor DNS principal\> índice = 1 <br>** agregar netsh interface ipv4 nombre de DNSServer =\<nombre del servidor DNS secundario\> dirección =\<dirección IP del servidor DNS secundario\> index = 2 ** <br> Repita según corresponda agregar servidores adicionales.<br>Ejecute **ipconfig/all** para comprobar que las direcciones son correctas.|
|Cambiar una dirección IP estática por una dirección IP proporcionada por DHCP|**nombre de la dirección del conjunto de netsh interface ipv4 =\<dirección IP del sistema local\> origen = DHCP** <br>Ejecute **Ipconfig/all** para comprobar que DHCP habilitado se establece en **Sí**.|
|Escribir una clave de producto|**slmgr.vbs – ipk \<clave de producto\>**| 
|Activar el servidor de forma local|**slmgr.vbs -ato**| 
|Activar el servidor de forma remota|**cscript slmgr.vbs – ipk \<clave de producto\>\<nombre del servidor\>\<username\>\<contraseña\>** <br>**cscript slmgr.vbs - ato \<servername\> \<username\> \<contraseña\>** <br>Obtener el GUID del equipo ejecutando **cscript slmgr.vbs-hizo** <br> Run **cscript slmgr.vbs -dli \<GUID\>** <br>Compruebe que el estado de la licencia está establecido en **con licencia (activado)**.


### <a name="networking-and-firewall"></a>Funciones de red y firewall

|Tarea|Comando| 
|----|-------|
|Configure el servidor para usar un servidor proxy|**establecimiento de proxy de Netsh Winhttp \<servername\>:\<número de puerto\>** <br>**Nota:** Las instalaciones Server Core no pueden acceder a Internet a través de un proxy que requiere una contraseña para permitir las conexiones.|
|Configure el servidor para omitir al proxy para direcciones de Internet|**establecimiento de proxy de Netsh winttp \<servername\>:\<número de puerto\> lista de omisión = "\<local\>"**| 
|Mostrar o modificar la configuración de IPSEC|**netsh ipsec**| 
|Mostrar o modificar la configuración de NAP|**netsh nap**| 
|Mostrar o modificar la dirección IP a la traducción de dirección física|**arp**| 
|Mostrar o configurar la tabla de enrutamiento local|**route**| 
|Ver o configurar la configuración del servidor DNS|**nslookup**| 
|Mostrar las estadísticas de protocolo y las conexiones de red TCP/IP actuales|**netstat**| 
|Mostrar las estadísticas de protocolo y conexiones TCP/IP actuales con NetBIOS sobre TCP/IP (NBT)|**nbtstat**| 
|Mostrar saltos en conexiones de red|**pathping**| 
|Saltos de seguimiento para las conexiones de red|**tracert**| 
|Mostrar la configuración del router multidifusión|**mrinfo**| 
|Habilitar la administración remota del servidor de seguridad|**netsh advfirewall firewall establece grupo de reglas = "Administración remota de Windows Firewall" nuevo enable = yes**| 
 

### <a name="updates-error-reporting-and-feedback"></a>Las actualizaciones, informes de errores y comentarios

|Tarea|Comando| 
|----|-------|
|Instalar una actualización|**wusa \<update\>.msu /quiet**| 
|Mostrar las actualizaciones instaladas|**systeminfo**| 
|Quitar una actualización|**expand /f:\* \<update\>.msu c:\test** <br>Vaya a c:\test\ y abra \<actualizar\>.xml en un editor de texto.<br>Reemplace **instalar** con **quitar** y guarde el archivo.<br>**pkgmgr /n:\<update\>.xml**|
|Configurar actualizaciones automáticas|Para comprobar la configuración actual: ** cscript %systemroot%\system32\scregedit.wsf/au /v **<br>Para habilitar las actualizaciones automáticas: **scregedit.wsf cscript/au 4** <br>Para deshabilitar las actualizaciones automáticas: **%systemroot%\system32\scregedit.wsf cscript/au 1**| 
|Habilitar los informes de errores|Para comprobar la configuración actual: **serverWerOptin /query** <br>Para enviar automáticamente informes detallados: **serverWerOptin / detallada** <br>Para enviar automáticamente informes de resumen:   **/Summary serverWerOptin** <br>Para deshabilitar informe de errores: **serverWerOptin /disable**|
|Participar en el Programa para la mejora de la experiencia del usuario (CEIP)|Para comprobar la configuración actual: **serverCEIPOptin /query** <br>Para habilitar el CEIP: **serverCEIPOptin /enable** <br>Para deshabilitar el CEIP: **serverCEIPOptin /disable**|

### <a name="services-processes-and-performance"></a>Servicios, procesos y rendimiento

|Tarea|Comando| 
|----|-------|
|Lista de los servicios en ejecución|**consulta de SC** o **net start**| 
|Iniciar un servicio|**inicio de SC \<nombre del servicio\>**  o **net start \<nombre del servicio\>**| 
|Detener un servicio|**detención de SC \<nombre del servicio\>**  o **net stop \<nombre del servicio\>**| 
|Recuperar una lista de las aplicaciones en ejecución y los procesos asociados|**tasklist**||Detener un proceso a la fuerza|Ejecute **tasklist** recuperar el identificador de proceso (PID) y luego ejecute **taskkill /PID \<Id. de proceso\>**|
|Iniciar el Administrador de tareas|**taskmgr**| 
|Crear y administrar registros de rendimiento y de sesión de seguimiento de eventos|Para crear un contador, seguimiento, recopilación de datos de configuración o API: **logman cree** <br>Al consultar las propiedades de recopilador de datos: **logman consulta** <br>Para iniciar o detener la recopilación de datos: **logman start\|detener** <br>Para eliminar un recopilador: **logman delete** <br> Para actualizar las propiedades de un recopilador: **logman update** <br>Para importar un conjunto de recopiladores de datos desde un archivo XML o exportarlo a un archivo XML: **logman import\|exportar**|

### <a name="event-logs"></a>Registros de eventos

|Tarea|Comando| 
|----|-------|
|Registros de eventos de lista|**wevtutil el**| 
|Consultar eventos en un registro especificado|**wevtutil qe /f:text \<log name\>**| 
|Exportar un registro de eventos|**wevtutil epl \<nombre del registro\>**| 
|Borrar un registro de eventos|**wevtutil cl \<nombre del registro\>**| 


### <a name="disk-and-file-system"></a>Disco y sistema de archivos

|Tarea|Comando|
|----|-------|
|Administrar particiones de disco|¿Para obtener una lista completa de comandos, ejecute **diskpart /?**|  
|Administrar RAID de software|¿Para obtener una lista completa de comandos, ejecute **diskraid /?**|  
|Administrar puntos de montaje de volúmenes|¿Para obtener una lista completa de comandos, ejecute **mountvol /?**| 
|Desfragmentar un volumen|¿Para obtener una lista completa de comandos, ejecute **defrag /?**|  
|Convertir un volumen en un sistema de archivos NTFS|**convertir \<letra de volumen \> /FS: NTFS**| 
|Compactar un archivo|¿Para obtener una lista completa de comandos, ejecute **compact /?**|  
|Administrar archivos abiertos|¿Para obtener una lista completa de comandos, ejecute **openfiles /?**|  
|Administrar carpetas VSS|¿Para obtener una lista completa de comandos, ejecute **vssadmin /?**| 
|Administrar el sistema de archivos|¿Para obtener una lista completa de comandos, ejecute **fsutil /?**||Comprobar una firma de archivo|**sigverif /?**| 
|Tomar posesión de un archivo o una carpeta|¿Para obtener una lista completa de comandos, ejecute **icacls /?**| 
 
### <a name="hardware"></a>Hardware

|Tarea|Comando| 
|----|-------|
|Agregar un controlador para un nuevo dispositivo de hardware|Copie el controlador en una carpeta en % homedrive %\\\<carpeta controlador\>. Ejecute **pnputil -i - a % homedrive %\\\<carpeta controlador\>\\\<controlador\>.inf**|
|Quitar un controlador de un dispositivo de hardware|Para obtener una lista de los controladores cargados, ejecute **tipo de consulta de sc = driver**. A continuación, ejecute **sc delete \<service_name\>**|
