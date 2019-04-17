---
title: Implementar el controlador de red mediante Windows PowerShell
description: Este tema proporciona instrucciones sobre cómo usar Windows PowerShell para implementar el controlador de red en uno o más equipos o máquinas virtuales (VM) que ejecutan Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 2448d381-55aa-4c14-997a-202c537c6727
ms.author: pashort
author: shortpatti
ms.openlocfilehash: cfd06662f317381fb77bf31db5ed60c2489ff871
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-network-controller-using-windows-powershell"></a>Implementar el controlador de red mediante Windows PowerShell

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Este tema proporciona instrucciones sobre cómo usar Windows PowerShell para implementar el controlador de red en uno o más equipos de virtual (VM) que ejecutan Windows Server 2016.

>[!IMPORTANT]
>No se implementará el rol de servidor de controlador de red en hosts físicos. Para implementar el controlador de red, debes instalar el rol de servidor de controlador de red en una máquina virtual de Hyper-V \(VM\) que está instalada en un host de Hyper-V. Después de haber instalado el controlador de red en máquinas virtuales en tres distintos hosts Hyper\-V, debe habilitar los hosts Hyper\-V para Software definido redes \(SDN\) agregando los hosts en los controladores de red mediante el comando de Windows PowerShell **nueva NetworkControllerServer**. Al hacerlo, se habilita el equilibrado de carga de Software SDN a función. Para obtener más información, consulta [nueva NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver).

Este tema contiene las siguientes secciones.

- [Instalar el rol de servidor de controlador de red](#bkmk_role)

- [Configurar el clúster de controlador de red](#bkmk_configure)

- [Configurar la aplicación de controlador de red](#bkmk_app)

- [Validación de la implementación de controlador de red](#bkmk_validation)

- [Comandos de Windows PowerShell adicionales para el controlador de red](#bkmk_ps)

- [Script de configuración del controlador de red de muestra](#bkmk_script)

- [POST-Post-Deployment Pasos para las implementaciones de Kerberos no](#bkmk_nonkerb)

## <a name="bkmk_role"></a>Instalar el rol de servidor de controlador de red

Puedes usar este procedimiento para instalar el rol de servidor de controlador de red en una máquina virtual \(VM\).

>[!IMPORTANT]
>No se implementará el rol de servidor de controlador de red en hosts físicos. Para implementar el controlador de red, debes instalar el rol de servidor de controlador de red en una máquina virtual de Hyper-V \(VM\) que está instalada en un host de Hyper-V. Después de haber instalado el controlador de red en máquinas virtuales en tres distintos hosts Hyper\-V, debe habilitar los hosts Hyper\-V para Software definido redes \(SDN\) agregando los hosts del controlador de red. Al hacerlo, se habilita el equilibrado de carga de Software SDN a función.

Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para realizar este procedimiento.  

>[!NOTE]
>Si quieres usar el administrador del servidor en lugar de Windows PowerShell para instalar el controlador de red, consulte [instalar el rol de servidor de controlador de red mediante el administrador del servidor](https://technet.microsoft.com/library/mt403348.aspx)

Para instalar el controlador de red mediante Windows PowerShell, escribe los siguientes comandos en un símbolo del sistema de Windows PowerShell y, a continuación, presione ENTRAR.

`Install-WindowsFeature -Name NetworkController -IncludeManagementTools`

Instalación del controlador de red requiere que se reinicie el equipo. Para ello, escribe el siguiente comando y, a continuación, presione ENTRAR.

`Restart-Computer`

## <a name="bkmk_configure"></a>Configurar el clúster de controlador de red

El clúster de controlador de red proporciona una alta disponibilidad y escalabilidad a la aplicación del controlador de red, que puedes configurar después de crear el clúster y que está hospedado en la parte superior del clúster.

>[!NOTE]
>Puedes realizar los procedimientos descritos en las siguientes secciones directamente en la máquina virtual donde había instalado el controlador de red, o puedes usar para realizar los procedimientos de un equipo remoto que ejecuta Windows Server 2016 o Windows 10 remoto Server Administration Tools para Windows Server 2016. Además, la pertenencia a **administradores**, o equivalente, es lo mínimo necesario para realizar este procedimiento. Si el equipo o máquina virtual en el que ha instalado el controlador de red está unido a un dominio, tu cuenta de usuario debe ser miembro del **los usuarios del dominio**.

Puedes crear un clúster de controlador de red al crear un objeto de nodo y, a continuación, configurar el clúster.

### <a name="create-a-node-object"></a>Crear un objeto de nodo

Deberás crear un objeto de nodo para cada máquina virtual que sea miembro del clúster de controlador de red.

Para crear un objeto de nodo, escribe el siguiente comando en el símbolo del sistema de Windows PowerShell y, a continuación, presione ENTRAR. Asegúrate de que agregues valores para cada parámetro que son adecuados para la implementación.  

```
New-NetworkControllerNodeObject -Name <string> -Server <String> -FaultDomain <string>-RestInterface <string> [-NodeCertificate <X509Certificate2>]
```

La siguiente tabla proporciona descripciones de cada parámetro de la **nueva NetworkControllerNodeObject** comando.

|Parámetro|Descripción|
|-------------|---------------|
|Nombre|La **nombre** parámetro especifica el nombre descriptivo del servidor que quieres agregar al clúster|
|Servidor|La **Server** parámetro especifica el nombre de host, el nombre de dominio completo (FQDN) o la dirección IP del servidor que quieras agregar al clúster. Para los equipos unidos a un dominio, es necesario FQDN.|
|FaultDomain|La **FaultDomain** parámetro especifica el dominio de error del servidor que vas a agregar al clúster. Este parámetro define los servidores que pueden producir un error al mismo tiempo que el servidor que vas a agregar al clúster. Este error puede deberse a compartida dependencias físicas como energía y orígenes de red. Dominios de error suelen representan jerarquías que están relacionados con estas dependencias compartidas, con servidores más probablemente no pase juntos desde un punto superior en el árbol de dominio de errores. Durante el tiempo de ejecución, el controlador de red tiene en cuenta los dominios de error en el clúster e intenta propagarse un vistazo a los servicios del controlador de red para que estén en dominios de error independiente. Este proceso ayuda a garantizar que, en caso de error de cualquier dominio de un error, que no se ve afectada la disponibilidad de servicio y su estado. Dominios de error se especifican en un formato jerárquico. Por ejemplo: "Fd: / DC1, cada Rack1/Host1", donde DC1 es el nombre del centro de datos, cada Rack1 es el nombre de bastidor y Host1 es el nombre del host donde se coloca el nodo.|
|RestInterface|La **RestInterface** parámetro especifica el nombre de la interfaz en el nodo donde se termina la comunicación de transferencia de estado representacional (REST). Esta interfaz de controlador de red recibe solicitudes de API Northbound de capa de administración de la red.|
|NodeCertificate|La **NodeCertificate** parámetro especifica el certificado que se usa el controlador de red para la autenticación de equipo. El certificado es necesario si usas la autenticación basada en certificados para la comunicación dentro del clúster. También se usa el certificado para el cifrado del tráfico entre los servicios de controlador de red. El nombre de firmante del certificado debe ser igual que el nombre DNS del nodo.|

### <a name="configure-the-cluster"></a>Configurar el clúster

Para configurar el clúster, escribe el siguiente comando en el símbolo del sistema de Windows PowerShell y, a continuación, presione ENTRAR. Asegúrate de que agregues valores para cada parámetro que son adecuados para la implementación.

```
Install-NetworkControllerCluster -Node <NetworkControllerNode[]> -ClusterAuthentication <ClusterAuthentication> [-ManagementSecurityGroup <string>][-DiagnosticLogLocation <string>][-LogLocationCredential <PSCredential>] [-CredentialEncryptionCertificate <X509Certificate2>][-Credential <PSCredential>][-CertificateThumbprint <String>] [-UseSSL][-ComputerName <string>][-LogSizeLimitInMBs<UInt32>] [-LogTimeLimitInDays<UInt32>]
```

La siguiente tabla proporciona descripciones de cada parámetro de la **instalación NetworkControllerCluster** comando.
  
|Parámetro|Descripción|
|-------------|---------------|
|ClusterAuthentication|La **ClusterAuthentication** parámetro especifica el tipo de autenticación que se usa para proteger la comunicación entre los nodos y también se usa para el cifrado del tráfico entre los servicios de controlador de red. Los valores admitidos son **Kerberos**, **X509** y **ninguno**. Autenticación de Kerberos usa cuentas de dominio y puede usarse solamente si los nodos de controlador de red están unidos a un dominio. Si especificas autenticación basada en X509, debes proporcionar un certificado en el objeto NetworkControllerNode. Además, debe aprovisionar el certificado manualmente antes de ejecutar este comando.|
|ManagementSecurityGroup|La **ManagementSecurityGroup** parámetro especifica el nombre del grupo de seguridad que contiene los usuarios que se pueden ejecutar los cmdlets de administración de un equipo remoto. Esto solo es aplicable si ClusterAuthentication es Kerberos. Debes especificar un grupo de seguridad de dominio y no a un grupo de seguridad en el equipo local.|
|Nodo|La **nodo** parámetro especifica la lista de nodos de controlador de red que crean mediante el **nueva NetworkControllerNodeObject** comando.|
|DiagnosticLogLocation|La **DiagnosticLogLocation** parámetro especifica la ubicación del recurso compartido donde los registros de diagnóstico se cargan de forma periódica. Si no especificas un valor de este parámetro, los registros se almacenan localmente en cada nodo. Los registros se almacenan localmente en el systemdrive%\Windows\tracing\SDNDiagnostics % carpeta. Registros de clúster se almacenan localmente en el %systemdrive%\ProgramData\Microsoft\Service carpeta Fabric\log\Traces.|
|LogLocationCredential|La **LogLocationCredential** parámetro especifica las credenciales necesarias para acceder a la ubicación del recurso compartido donde se almacenan los registros.|
|CredentialEncryptionCertificate|La **CredentialEncryptionCertificate** parámetro especifica el certificado de controlador de red que se usa para cifrar las credenciales que se usan para acceder a archivos binarios de controlador de red y la **LogLocationCredential**, si se especifica. El certificado debe estar aprovisionado en todos los nodos de controlador de red antes de ejecutar este comando y el mismo certificado se debe inscribir en todos los nodos del clúster. En entornos de producción, se recomienda usar este parámetro para proteger los registros y archivos binarios de controlador de red. Sin este parámetro, las credenciales se almacenan en un texto claro y pueden ser utilizadas por cualquier usuario no autorizado.|
|Credenciales|Este parámetro es necesario solo si se ejecuta este comando desde un equipo remoto. La **credenciales** parámetro especifica una cuenta de usuario que tenga permiso para ejecutar este comando en el equipo de destino.|
|CertificateThumbprint|Este parámetro es necesario solo si se ejecuta este comando desde un equipo remoto. La **CertificateThumbprint** parámetro especifica el digital certificado de clave pública (X509) de una cuenta de usuario que tiene permiso para ejecutar este comando en el equipo de destino.|
|UseSSL|Este parámetro es necesario solo si se ejecuta este comando desde un equipo remoto. La **UseSSL** parámetro especifica el protocolo de capa de Sockets seguros (SSL) que se usa para establecer una conexión con el equipo remoto. De manera predeterminada, no se usa SSL.|
|ComputerName|La **ComputerName** parámetro especifica el nodo del controlador de red en el que este comando es ejecutar. Si no especificas un valor de este parámetro, se usa el equipo local de manera predeterminada.|
|LogSizeLimitInMBs|Este parámetro especifica el tamaño máximo del registro, en MB, que puede almacenar el controlador de red. Registros se almacenan de forma circular. Si se proporciona DiagnosticLogLocation, el valor predeterminado de este parámetro es de 40 GB. Si no se proporciona DiagnosticLogLocation, los registros se almacenan en los nodos del controlador de red y el valor predeterminado de este parámetro es de 15 GB.|
|LogTimeLimitInDays|Este parámetro especifica el límite de duración, en días, para que se almacenan los registros. Registros se almacenan de forma circular. El valor predeterminado de este parámetro es 3 días.|

## <a name="bkmk_app"></a>Configurar la aplicación de controlador de red
Para configurar la aplicación de controlador de red, escribe el siguiente comando en el símbolo del sistema de Windows PowerShell y, a continuación, presione ENTRAR. Asegúrate de que agregues valores para cada parámetro que son adecuados para la implementación.

```
Install-NetworkController -Node <NetworkControllerNode[]> -ClientAuthentication <ClientAuthentication>  [-ClientCertificateThumbprint <string[]>]  [-ClientSecurityGroup <string>] -ServerCertificate <X509Certificate2> [-RESTIPAddress <String>] [-RESTName <String>] [-Credential <PSCredential>][-CertificateThumbprint <String> ] [-UseSSL]
```

La siguiente tabla proporciona descripciones de cada parámetro de la **instalación NetworkController** comando.

|Parámetro|Descripción|
|-------------|---------------|
|ClientAuthentication|La **ClientAuthentication** parámetro especifica el tipo de autenticación que se usa para proteger la comunicación entre REST y controlador de red. Los valores admitidos son **Kerberos**, **X509** y **ninguno**. Autenticación de Kerberos usa cuentas de dominio y puede usarse solamente si los nodos de controlador de red están unidos a un dominio. Si especificas autenticación basada en X509, debes proporcionar un certificado en el objeto NetworkControllerNode. Además, debe aprovisionar el certificado manualmente antes de ejecutar este comando.|
|Nodo|La **nodo** parámetro especifica la lista de nodos de controlador de red que crean mediante el **nueva NetworkControllerNodeObject** comando.|
|ClientCertificateThumbprint|Este parámetro es necesario solo cuando se usa la autenticación basada en certificados para los clientes de controlador de red. La **ClientCertificateThumbprint** parámetro especifica la huella digital del certificado que se inscribe a los clientes de la capa Northbound.|
|Certificados|La **certificados** parámetro especifica el certificado que se usa el controlador de red para probar su identidad a los clientes. El certificado de servidor debe incluir el propósito de autenticación de servidores en extensiones de uso mejorado de claves y se debe emitir para el controlador de red por una CA que es de confianza para los clientes.|
|RESTIPAddress|No es necesario especificar un valor para **RESTIPAddress** con una implementación de nodo único de controlador de red. Para las implementaciones de varios nodos, la **RESTIPAddress** parámetro especifica la dirección IP del extremo REST en notación CIDR. Por ejemplo, 192.168.1.10/24. El valor de nombre de firmante de **certificados** debe resolverse en el valor de la **RESTIPAddress** parámetro. Este parámetro debe especificarse de todas las implementaciones de controlador de red de varios nodos cuando todos los nodos se encuentran en la misma subred. Si los nodos que se encuentran en diferentes subredes, debes usar el **RestName** parámetro en lugar de usar **RESTIPAddress**.|
|RestName|No es necesario especificar un valor para **RestName** con una implementación de nodo único de controlador de red. La única vez que debe especificar un valor para **RestName** es tan nodos que se encuentran en subredes diferentes implementaciones de varios nodos. Para las implementaciones de varios nodos, la **RestName** parámetro especifica el nombre completo del clúster de controlador de red.|
|ClientSecurityGroup|La **ClientSecurityGroup** parámetro especifica el nombre del grupo de seguridad de Active Directory cuyos miembros son clientes de controlador de red. Este parámetro es necesario solamente si usas la autenticación Kerberos para **ClientAuthentication**. El grupo de seguridad debe contener las cuentas desde el que se accede a las API de REST y tienes que crear el grupo de seguridad y agregar a miembros antes de ejecutar este comando.|
|Credenciales|Este parámetro es necesario solo si se ejecuta este comando desde un equipo remoto. La **credenciales** parámetro especifica una cuenta de usuario que tenga permiso para ejecutar este comando en el equipo de destino.|
|CertificateThumbprint|Este parámetro es necesario solo si se ejecuta este comando desde un equipo remoto. La **CertificateThumbprint** parámetro especifica el digital certificado de clave pública (X509) de una cuenta de usuario que tiene permiso para ejecutar este comando en el equipo de destino.|
|UseSSL|Este parámetro es necesario solo si se ejecuta este comando desde un equipo remoto. La **UseSSL** parámetro especifica el protocolo de capa de Sockets seguros (SSL) que se usa para establecer una conexión con el equipo remoto. De manera predeterminada, no se usa SSL.|

Después de completar la configuración de la aplicación de controlador de red, la implementación de controlador de red está completa.

## <a name="bkmk_validation"></a>Validación de la implementación de controlador de red

Para validar la implementación del controlador de red, puedes agregar una credencial en el controlador de red y, a continuación, se recupera la credencial.

Si estás usando Kerberos como mecanismo de ClientAuthentication, pertenencia a la **ClientSecurityGroup** que creaste es lo mínimo necesario para realizar este procedimiento.

#### <a name="to-validate-deployment-of-network-controller"></a>Para validar la implementación de controlador de red

1.  En un equipo cliente, si estás usando Kerberos como mecanismo de ClientAuthentication, inicia sesión con una cuenta de usuario que es un miembro de tu **ClientSecurityGroup**.

2. Abre Windows PowerShell, escribe los siguientes comandos para agregar una credencial en el controlador de red y, a continuación, presione ENTRAR. Asegúrate de que agregues valores para cada parámetro que son adecuados para la implementación.

    ```
    $cred=New-Object Microsoft.Windows.Networkcontroller.credentialproperties
    $cred.type="usernamepassword"
    $cred.username="admin"
    $cred.value="abcd"

    New-NetworkControllerCredential -ConnectionUri https://networkcontroller -Properties $cred -ResourceId cred1
    ```

3. Para recuperar la credencial que se agregaron al controlador de red, escribe el siguiente comando y, a continuación, presione ENTRAR. Asegúrate de que agregues valores para cada parámetro que son adecuados para la implementación.

    ```
    Get-NetworkControllerCredential -ConnectionUri https://networkcontroller -ResourceId cred1  
    ```

4. Revisa lo resultados de comando, que debe ser similar al siguiente resultado del ejemplo.

    ```
    Tags                   :
    ResourceRef     : /credentials/cred1
    CreatedTime    : 1/1/0001 12:00:00 AM
    InstanceId        : e16ffe62-a701-4d31-915e-7234d4bc5a18
    Etag                  : W/"1ec59631-607f-4d3e-ac78-94b0822f3a9d"
    ResourceMetadata :
    ResourceId       : cred1
    Properties       : Microsoft.Windows.NetworkController.CredentialProperties
    ```

    > [!NOTE]
    > Cuando ejecutas la **Get NetworkControllerCredential** comando, puedes asignar el resultado del comando a una variable mediante el operador de punto para mostrar las propiedades de las credenciales. Por ejemplo, $cred. Propiedades.

## <a name="bkmk_ps"></a>Comandos de Windows PowerShell adicionales para el controlador de red

Después de implementar el controlador de red, puedes usar comandos de Windows PowerShell para administrar y modificar la implementación. A continuación es algunos de los cambios que puedes realizar en la implementación.

- Modificar el controlador de red nodo clúster y configuración de la aplicación

- Quitar el clúster de controlador de red y la aplicación

- Administrar los nodos del clúster de controlador de red, como agregar, quitar, habilitar y deshabilitar los nodos.

La siguiente tabla se proporciona que la sintaxis para Windows PowerShell comandos que puedes usar para realizar estas tareas.

|Tarea|Comando|Sintaxis|
|--------|-------|----------|
|Modificar la configuración de clúster de controlador de red|Set-NetworkControllerCluster|`Set-NetworkControllerCluster [-ManagementSecurityGroup <string>][-Credential <PSCredential>] [-computerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|Modificar la configuración de la aplicación de controlador de red|Set-NetworkController|`Set-NetworkController [-ClientAuthentication <ClientAuthentication>] [-Credential <PSCredential>] [-ClientCertificateThumbprint <string[]>] [-ClientSecurityGroup <string>] [-ServerCertificate <X509Certificate2>] [-RestIPAddress <String>] [-ComputerName <String>][-CertificateThumbprint <String> ] [-UseSSL]`
|Modificar la configuración del nodo de controlador de red|Set-NetworkControllerNode|`Set-NetworkControllerNode -Name <string> > [-RestInterface <string>] [-NodeCertificate <X509Certificate2>] [-Credential <PSCredential>] [-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|Modificar la configuración de diagnóstico de controlador de red|Set-NetworkControllerDiagnostic|`Set-NetworkControllerDiagnostic [-LogScope <string>] [-DiagnosticLogLocation <string>] [-LogLocationCredential <PSCredential>] [-UseLocalLogLocation] >] [-LogLevel <loglevel>][-LogSizeLimitInMBs <uint32>] [-LogTimeLimitInDays <uint32>] [-Credential <PSCredential>] [-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|Quitar la aplicación de controlador de red|Uninstall-NetworkController|`Uninstall-NetworkController [-Credential <PSCredential>][-ComputerName <string>] [-CertificateThumbprint <String> ] [-UseSSL]`
|Quitar el clúster de controlador de red|Uninstall-NetworkControllerCluster|`Uninstall-NetworkControllerCluster [-Credential <PSCredential>][-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|Agrega un nodo al clúster de controlador de red|Add-NetworkControllerNode|`Add-NetworkControllerNode -FaultDomain <String> -Name <String> -RestInterface <String> -Server <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-Force] [-NodeCertificate <X509Certificate2> ] [-PassThru] [-UseSsl]`
|Deshabilitar un nodo del clúster de controlador de red|Disable-NetworkControllerNode|`Disable-NetworkControllerNode -Name <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-PassThru] [-UseSsl]`
|Habilitar un nodo del clúster de controlador de red|Enable-NetworkControllerNode|`Enable-NetworkControllerNode -Name <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-PassThru] [-UseSsl]`
|Quitar un nodo de controlador de red de un clúster|Remove-NetworkControllerNode|`Remove-NetworkControllerNode [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-Force] [-Name <String> ] [-PassThru] [-UseSsl]`

>[!NOTE]
>Comandos de Windows PowerShell para el controlador de red están en la biblioteca de TechNet en [Cmdlets del controlador de red](https://technet.microsoft.com/library/mt576401.aspx).

## <a name="bkmk_script"></a>Script de configuración del controlador de red de muestra

El siguiente script de configuración de ejemplo muestra cómo crear un clúster de controlador de red de varios nodos e instalar la aplicación de controlador de red. Además, la variable $cert selecciona un certificado del almacén de certificados de equipo local que coincida con la cadena de nombre de asunto "networkController.contoso.com".

```
$a = New-NetworkControllerNodeObject -Name Node1 -Server NCNode1.contoso.com -FaultDomain fd:/rack1/host1 -RestInterface Internal
$b = New-NetworkControllerNodeObject -Name Node2 -Server NCNode2.contoso.com -FaultDomain fd:/rack1/host2 -RestInterface Internal
$c = New-NetworkControllerNodeObject -Name Node3 -Server NCNode3.contoso.com -FaultDomain fd:/rack1/host3 -RestInterface Internal

$cert= get-item Cert:\LocalMachine\My | get-ChildItem | where {$_.Subject -imatch "networkController.contoso.com" }

Install-NetworkControllerCluster -Node @($a,$b,$c)  -ClusterAuthentication Kerberos -DiagnosticLogLocation \\share\Diagnostics - ManagementSecurityGroup Contoso\NCManagementAdmins -CredentialEncryptionCertificate $cert  
Install-NetworkController -Node @($a,$b,$c) -ClientAuthentication Kerberos -ClientSecurityGroup Contoso\NCRESTClients -ServerCertificate $cert -RestIpAddress 10.0.0.1/24
```

## <a name="bkmk_nonkerb"></a>Posterior a la implementación pasos para no - Kerberos implementaciones

Si no estás usando Kerberos con la implementación de controlador de red, debes implementar los certificados.

Para obtener más información, consulta [pasos posteriores a la implementación de controlador de red](../technologies/network-controller/post-deploy-steps-nc.md).


