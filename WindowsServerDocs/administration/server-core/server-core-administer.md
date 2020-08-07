---
title: Administrar Server Core
description: Obtener información sobre cómo administrar una instalación Server Core de Windows Server
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.author: elizapo
ms.localizationpriority: medium
ms.date: 12/18/2018
ms.openlocfilehash: 8eba1e081c57c1c668a4e52ecb85fb4716a36fdc
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87895905"
---
# <a name="administer-a-server-core-server"></a>Administrar un servidor Server Core

>Se aplica a: Windows Server 2019, Windows Server 2016 y Windows Server (canal semianual)

Dado que Server Core no tiene una interfaz de usuario, debe usar cmdlets de Windows PowerShell, herramientas de línea de comandos o herramientas remotas para realizar tareas de administración básicas. En las secciones siguientes se describen los cmdlets y comandos de PowerShell que se usan para realizar tareas básicas. También puede usar el [Centro](../../manage/windows-admin-center/overview.md)de administración de Windows, un portal de administración unificado que se encuentra actualmente en versión preliminar pública, para administrar la instalación.

## <a name="administrative-tasks-using-powershell-cmdlets"></a>Tareas administrativas con cmdlets de PowerShell
Use la siguiente información para realizar tareas administrativas básicas con cmdlets de Windows PowerShell.

### <a name="set-a-static-ip-address"></a>Establecer una dirección IP estática
Al instalar un servidor Server Core, de forma predeterminada tiene una dirección DHCP. Si necesita una dirección IP estática, puede establecerla mediante los pasos siguientes.

Para ver la configuración actual de la red, use **Get-NetIPConfiguration**.

Para ver las direcciones IP que ya está usando, use **Get-NetIPAddress**.

Para establecer una dirección IP estática, haga lo siguiente:

1. Ejecute **Get-NetIPInterface**.
2. Anote el número de la **columna de** tipo de dirección de la interfaz IP o la cadena **InterfaceDescription** . Si tiene más de un adaptador de red, tenga en cuenta el número o la cadena correspondiente a la interfaz para la que desea establecer la dirección IP estática.
3. Ejecute el siguiente cmdlet para establecer la dirección IP estática:

   ```powershell
   New-NetIPaddress -InterfaceIndex 12 -IPAddress 192.0.2.2 -PrefixLength 24 -DefaultGateway 192.0.2.1
   ```

   donde:
   - **InterfaceIndex** es el **valor de debajo del paso** 2. (En nuestro ejemplo, 12)
   - **IPAddress** es la dirección IP estática que desea establecer. (En nuestro ejemplo, 191.0.2.2)
   - **PrefixLength** es la longitud del prefijo (otra forma de máscara de subred) para la dirección IP que está estableciendo. (En nuestro ejemplo, 24)
   - **DefaultGateway** es la dirección IP de la puerta de enlace predeterminada. (En nuestro ejemplo, 192.0.2.1)
4. Ejecute el siguiente cmdlet para establecer la dirección del servidor cliente DNS:

   ```powershell
   Set-DNSClientServerAddress –InterfaceIndex 12 -ServerAddresses 192.0.2.4
   ```

   donde:
   - **InterfaceIndex** es el valor de debajo del paso 2.
   - **ServerAddresses** es la dirección IP del servidor DNS.
5. Para agregar varios servidores DNS, ejecute el siguiente cmdlet:

   ```powershell
   Set-DNSClientServerAddress –InterfaceIndex 12 -ServerAddresses 192.0.2.4,192.0.2.5
   ```

   donde, en este ejemplo, **192.0.2.4** y **192.0.2.5** son direcciones IP de los servidores DNS.

Si necesita cambiar al uso de DHCP, ejecute **set-DnsClientServerAddress – InterfaceIndex 12 – ResetServerAddresses**.

### <a name="join-a-domain"></a>Unión a un dominio
Use los cmdlets siguientes para unir un equipo a un dominio.

1. Ejecute **Add-Computer**. Se le pedirán las dos credenciales para unirse al dominio y el nombre de dominio.
2. Si necesita agregar una cuenta de usuario de dominio al grupo de administradores locales, ejecute el siguiente comando en un símbolo del sistema (no en la ventana de PowerShell):

   ```
   net localgroup administrators /add <DomainName>\<UserName>
   ```
3. Reinicie el equipo. Para ello, ejecute **restart-Computer**.

### <a name="rename-the-server"></a>Cambiar el nombre del servidor
Siga los pasos que se indican a continuación para cambiar el nombre del servidor.

1. Determine el nombre actual del servidor con el nombre de **host** o el comando **ipconfig** .
2. Ejecute **Rename-Computer-COMPUTERNAME \<new_name\> **.
3. Reinicie el equipo.

### <a name="activate-the-server"></a>Activar el servidor

Ejecute **slmgr. vbs – IPK \<productkey\> **. A continuación, ejecute **slmgr. vbs – ATO**. Si la activación se realiza correctamente, no recibirá ningún mensaje.

> [!NOTE]
> También puede activar el servidor por teléfono, mediante un [servidor de servicio de administración de claves (kms)](../../get-started/server-2016-activation.md)o de forma remota. Para activar de forma remota, ejecute el siguiente cmdlet desde un equipo remoto:
>
> ```
> cscript windows\system32\slmgr.vbs <ServerName> <UserName> <password>:-ato
> ```

### <a name="configure-windows-firewall"></a>Configurar el Firewall de Windows

Puede configurar el Firewall de Windows de forma local en el equipo Server Core mediante cmdlets y scripts de Windows PowerShell. Consulte [NetSecurity](/powershell/module/netsecurity/?view=win10-ps) para los cmdlets que puede usar para configurar Firewall de Windows.

### <a name="enable-windows-powershell-remoting"></a>Habilitar la comunicación remota de Windows PowerShell

Puede habilitar la comunicación remota de Windows PowerShell, con la cual los comandos de Windows PowerShell escritos en un equipo se ejecutan en otro. Habilite la comunicación remota de Windows PowerShell con **enable-PSRemoting**.

Para obtener más información, vea [acerca de las preguntas más frecuentes sobre Remote](/powershell/module/microsoft.powershell.core/about/about_remote_faq?view=powershell-5.1).

## <a name="administrative-tasks-from-the-command-line"></a>Tareas administrativas desde la línea de comandos
Use la siguiente información de referencia para realizar tareas administrativas desde la línea de comandos.

### <a name="configuration-and-installation"></a>Configuración e instalación

|                             Tarea                              |                                                                                                                                                                                                                 Get-Help                                                                                                                                                                                                                 |
|---------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|             Establecer la contraseña administrativa local             |                                                                                                                                                                                                      **Administrador de usuarios de red**\*                                                                                                                                                                                                      |
|                  Unir un equipo a un dominio                  |                                                                                                                                                       **netdom join% COMPUTERNAME%** **/Domain: \<domain\> /userd: \<domain\\username\> /passwordd:**\* <br> Reinicie el equipo.                                                                                                                                                        |
|              Confirmar que el dominio ha cambiado              |                                                                                                                                                                                                                 **set**                                                                                                                                                                                                                 |
|                Quitar un equipo de un dominio                |                                                                                                                                                                                                   **Netdom Remove\<computername\>**                                                                                                                                                                                                    |
|         Agregar un usuario al grupo local Administradores          |                                                                                                                                                                                       **Administradores de net localgroup/Add\<domain\\username\>**                                                                                                                                                                                       |
|       Quitar un usuario del grupo Administradores local       |                                                                                                                                                                                     **Administradores de net localgroup/Delete\<domain\\username\>**                                                                                                                                                                                      |
|               Agregar un usuario al equipo local                |                                                                                                                                                                                                **net user \<domain\username\> \* /Add**                                                                                                                                                                                                 |
|               Agregar un grupo al equipo local               |                                                                                                                                                                                                 **net localgroup \<group name\> /Add**                                                                                                                                                                                                  |
|          Cambiar el nombre de un equipo unido a un dominio          |                                                                                                                                                           **netdom renamecomputer% COMPUTERNAME%/NewName: \<new computer name\> /userd: \<domain\\username\> /passwordd:**\*                                                                                                                                                            |
|                 Confirmar el nuevo nombre de un equipo                 |                                                                                                                                                                                                                 **set**                                                                                                                                                                                                                 |
|         Cambiar el nombre de un equipo en un grupo de trabajo         |                                                                                                                                                                **Netdom renamecomputer \<currentcomputername\> /NewName:\<newcomputername\>** <br>Reinicie el equipo.                                                                                                                                                                 |
|                Deshabilitar la administración de archivos de paginación                 |                                                                                                                                                                        **WMIC ComputerSystem donde name = " \<computername\> " Set AutomaticManagedPagefile = false**                                                                                                                                                                         |
|                   Configurar el archivo de paginación                   |                                                            **WMIC pagefileset Where name = " \<path/filename\> " Set InitialSize = \<initialsize\> , tamañomáximo =\<maxsize\>** <br>Donde *path/FILENAME* es la ruta de acceso y el nombre del archivo de paginación, *initialsize* es el tamaño inicial del archivo de paginación, en bytes, y *MaxSize* es el tamaño máximo del archivo de paginación, en bytes.                                                             |
|                 Cambiar a una dirección IP estática                 | **ipconfig/all** <br>Grabe la información relevante o redirijala a un archivo de texto (**ipconfig/all >ipconfig.txt**).<br>**netsh interface IPv4 show interfaces**<br>Compruebe que haya una lista de interfaces.<br>**netsh interface IPv4 Set Address \<Name ID from interface list\> source = static Address = \<preferred IP address\> Gateway =\<gateway address\>**<br>Ejecute **ipconfig/all** para comprobar que DHCP habilitado está establecido en **no**. |
|                   Establezca una dirección DNS estática.                   |   <strong>netsh interface IPv4 Add dnsserver name = \<name or ID of the network interface card\> Address = \<IP address of the primary DNS server\> index = 1 <br></strong>netsh interface IPv4 Add dnsserver name = \<name of secondary DNS server\> Address = \<IP address of the secondary DNS server\> index = 2\*\* <br> Repita los pasos necesarios para agregar más servidores.<br>Ejecute **ipconfig/all** para comprobar que las direcciones son correctas.   |
| Cambiar una dirección IP estática por una dirección IP proporcionada por DHCP |                                                                                                                                      **netsh interface IPv4 Set Address name = \<IP address of local system\> source = DHCP** <br>Ejecute **ipconfig/all** para comprobar que DCHP habilitado está establecido en **sí**.                                                                                                                                      |
|                      Especificación de una clave de producto                      |                                                                                                                                                                                                   **slmgr. vbs: IPK\<product key\>**                                                                                                                                                                                                    |
|                  Activar el servidor de forma local                  |                                                                                                                                                                                                           **slmgr. vbs-ATO**                                                                                                                                                                                                            |
|                 Activar el servidor de forma remota                  |                                            **cscript slmgr. vbs – IPK\<product key\>\<server name\>\<username\>\<password\>** <br>**cscript slmgr. vbs-ATO \<servername\> \<username\>\<password\>** <br>Obtener el GUID del equipo mediante la ejecución de **cscript slmgr. vbs-DID** <br> Ejecutar **cscript slmgr. vbs-DLI \<GUID\> ** <br>Compruebe que el estado de la licencia está establecido en con **licencia (activada)**.                                             |

### <a name="networking-and-firewall"></a>Funciones de red y firewall

|Tarea|Get-Help|
|----|-------|
|Configurar el servidor para que use un servidor proxy|**netsh WinHTTP Set proxy \<servername\> :\<port number\>** <br>**Nota:** Las instalaciones Server Core no pueden tener acceso a Internet a través de un proxy que requiera una contraseña para permitir conexiones.|
|Configurar el servidor para omitir el proxy para direcciones de Internet|**netsh WinHTTP Set proxy \<servername\> : \<port number\> bypass-List = " \<local\> "**|
|Mostrar o modificar la configuración de IPSEC|**netsh ipsec**|
|Mostrar o modificar la configuración de NAP|**netsh NAP**|
|Mostrar o modificar la traducción de direcciones IP a física|**arp**|
|Mostrar o configurar la tabla de enrutamiento local|**route**|
|Ver o configurar los valores del servidor DNS|**nslookup**|
|Mostrar las estadísticas de protocolo y las conexiones de red TCP/IP actuales|**netstat**|
|Mostrar las estadísticas de protocolo y las conexiones TCP/IP actuales mediante NetBIOS a través de TCP/IP (NBT)|**nbtstat**|
|Mostrar saltos para conexiones de red|**pathping**|
|Seguimiento de saltos para conexiones de red|**tracert**|
|Mostrar la configuración del router multidifusión|**mrinfo**|
|Habilitar la administración remota del firewall|**netsh advfirewall firewall set Rule Group = "administración remota de Firewall de Windows Defender" New enable = Yes**|


### <a name="updates-error-reporting-and-feedback"></a>Actualizaciones, informes de errores y comentarios

|                               Tarea                                |                                                                                                                               Get-Help                                                                                                                                |
|-------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                         Instalar una actualización                         |                                                                                                                    **WUSA \<update\> . msu/Quiet**                                                                                                                    |
|                      Mostrar las actualizaciones instaladas                       |                                                                                                                            **systeminfo**                                                                                                                            |
|                         Quitar una actualización                          |                                 **Expanda/f: \* \<update\> . msu c:\test** <br>Navegue hasta c:\test\ y abra \<update\>.xml en un editor de texto.<br>Reemplace **instalar** con **quitar** y guarde el archivo.<br>**PkgMgr/n: \<update\> . XML**                                 |
|                    Configuración de actualizaciones automáticas                    |          Para comprobar la configuración actual: **cscript%SystemRoot%\system32\scregedit.wsf/au/v \* \* <br> para habilitar las actualizaciones automáticas: \* \* cscript scregedit. wsf/au 4** <br>Para deshabilitar las actualizaciones automáticas: **cscript%SystemRoot%\SYSTEM32\SCREGEDIT.wsf/au 1**          |
|                      Habilitar el informe de errores                       | Para comprobar la configuración actual: **serverWerOptin/Query** <br>Para enviar automáticamente informes detallados: **serverWerOptin/Detailed** <br>Para enviar automáticamente informes de Resumen: **serverWerOptin/Summary** <br>Para deshabilitar los informes de errores: **serverWerOptin/Disable** |
| Participar en el Programa para la mejora de la experiencia del usuario (CEIP) |                                                     Para comprobar la configuración actual: **serverCEIPOptin/Query** <br>Para habilitar el CEIP: **serverCEIPOptin/enable** <br>Para deshabilitar CEIP: **serverCEIPOptin/Disable**                                                      |

### <a name="services-processes-and-performance"></a>Servicios, procesos y rendimiento

|                               Tarea                               |                                                                                                                                                                                                             Get-Help                                                                                                                                                                                                              |
|------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                    Enumerar los servicios en ejecución                     |                                                                                                                                                                                                  **consulta SC** o **net start**                                                                                                                                                                                                   |
|                         iniciar un servicio.                          |                                                                                                                                                                                 **Inicio \<service name\> de SC** o **net start \<service name\> **                                                                                                                                                                                  |
|                          Detención de un servicio                          |                                                                                                                                                                                  **detención \<service name\> de SC** o **net stop \<service name\> **                                                                                                                                                                                   |
| Recuperar una lista de las aplicaciones en ejecución y los procesos asociados |                                                                                                                                                                                                           **tasklist**                                                                                                                                                                                                           |
|                        Iniciar el Administrador de tareas                        |                                                                                                                                                                                                           **taskmgr**                                                                                                                                                                                                            |
|    Crear y administrar registros de rendimiento y sesión de seguimiento de eventos    | Para crear un contador, un seguimiento, una colección de datos de configuración o una API: **Logman creación** <br>Para consultar las propiedades del recopilador de datos: **Logman Query** <br>Para iniciar o detener la recopilación de datos: **Logman Start \| Stop** <br>Para eliminar un recopilador: **Logman Delete** <br> Para actualizar las propiedades de un recopilador: **Logman Update** <br>Para importar un conjunto de recopiladores de datos desde un archivo XML o exportarlo a un archivo XML: **Logman Import \| Export** |

### <a name="event-logs"></a>Registros de eventos

|Tarea|Get-Help|
|----|-------|
|Enumerar registros de eventos|**wevtutil el**|
|Consultar eventos en un registro especificado|**wevtutil qe/f: texto\<log name\>**|
|Exportar un registro de eventos|**wevtutil EPL\<log name\>**|
|Borrar un registro de eventos|**wevtutil cl\<log name\>**|


### <a name="disk-and-file-system"></a>Disco y sistema de archivos

|                   Tarea                   |                        Get-Help                        |
|------------------------------------------|-------------------------------------------------------|
|          Administrar particiones de disco          | Para obtener una lista completa de comandos, ejecute **DiskPart/?**  |
|           Administrar RAID de software           | Para obtener una lista completa de comandos, ejecute **Diskraid/?**  |
|        Administrar puntos de montaje de volúmenes        | Para obtener una lista completa de comandos, ejecute **Mountvol/?**  |
|           Desfragmentar un volumen            |  Para obtener una lista completa de comandos, ejecute **Defrag/?**   |
| Convertir un volumen en un sistema de archivos NTFS |        **Convert \<volume letter\> /FS: NTFS**         |
|              Compactar un archivo              |  Para obtener una lista completa de comandos, ejecute **Compact/?**  |
|          Administrar archivos abiertos           | Para obtener una lista completa de comandos, ejecute **openfiles/?** |
|          Administrar carpetas VSS          | Para obtener una lista completa de comandos, ejecute **vssadmin/?**  |
|        Administrar el sistema de archivos        |  Para obtener una lista completa de comandos, ejecute **fsutil/?**   |
|    Tomar posesión de un archivo o una carpeta    |  Para obtener una lista completa de comandos, ejecute **icacls/?**   |

### <a name="hardware"></a>Hardware

|Tarea|Get-Help|
|----|-------|
|Agregar un controlador para un nuevo dispositivo de hardware|Copie el controlador en una carpeta en% HOMEDRIVE% \\ \<driver folder\> . Ejecutar **pnputil-i-a% HOMEDRIVE% \\ \<driver folder\> \\ \<driver\> . inf**|
|Quitar un controlador de un dispositivo de hardware|Para obtener una lista de los controladores cargados, ejecute **sc query Type = driver**. Después, ejecute **SC \<service_name\> Delete**|
