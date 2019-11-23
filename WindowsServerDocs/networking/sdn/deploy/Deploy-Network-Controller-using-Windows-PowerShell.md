---
title: Implementación de controladora de red con Windows PowerShell
description: En este tema se proporcionan instrucciones sobre el uso de Windows PowerShell para implementar la controladora de red en uno o varios equipos o máquinas virtuales (VM) que ejecutan Windows Server 2016.
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 2448d381-55aa-4c14-997a-202c537c6727
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: 294466ef70a9ffc230953b48bb292938be519eac
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406118"
---
# <a name="deploy-network-controller-using-windows-powershell"></a>Implementación de controladora de red con Windows PowerShell

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se proporcionan instrucciones sobre el uso de Windows PowerShell para implementar la controladora de red en una o varias máquinas virtuales (VM) que ejecutan Windows Server 2016.

>[!IMPORTANT]
>No implemente el rol de servidor de la controladora de red en hosts físicos. Para implementar la controladora de red, debe instalar el rol de servidor de la controladora de red en una máquina virtual de Hyper-V \(máquina virtual\) instalada en un host de Hyper-V. Después de haber instalado el controlador de red en las máquinas virtuales en tres hosts de Hyper\-V diferentes, debe habilitar los hosts de Hyper\-V para redes definidas por software \(SDN\) agregando los hosts a la controladora de red mediante el comando de Windows PowerShell **New-NetworkControllerServer**. Al hacerlo, habilita el Load Balancer de software de SDN para que funcione. Para obtener más información, consulte [New-NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver).

En este tema se incluyen las siguientes secciones.

- [Instalar el rol de servidor de controlador de red](#install-the-network-controller-server-role)

- [Configuración del clúster de controladora de red](#configure-the-network-controller-cluster)

- [Configurar la aplicación de controladora de red](#configure-the-network-controller-application)

- [Validación de la implementación de la controladora de red](#network-controller-deployment-validation)

- [Comandos adicionales de Windows PowerShell para la controladora de red](#additional-windows-powershell-commands-for-network-controller)

- [Script de configuración de controladora de red de ejemplo](#sample-network-controller-configuration-script)

- [Pasos posteriores a la implementación para implementaciones que no son de Kerberos](#post-deployment-steps-for-non-kerberos-deployments)

## <a name="install-the-network-controller-server-role"></a>Instalar el rol de servidor de controlador de red

Puede usar este procedimiento para instalar el rol de servidor de la controladora de red en una máquina virtual \(\)de máquinas virtuales.

>[!IMPORTANT]
>No implemente el rol de servidor de la controladora de red en hosts físicos. Para implementar la controladora de red, debe instalar el rol de servidor de la controladora de red en una máquina virtual de Hyper-V \(máquina virtual\) instalada en un host de Hyper-V. Después de haber instalado el controlador de red en las máquinas virtuales en tres hosts de Hyper\-V diferentes, debe habilitar los hosts de Hyper\-V para redes definidas por software \(SDN\) agregando los hosts a la controladora de red. Al hacerlo, habilita el Load Balancer de software de SDN para que funcione.

El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o grupo equivalente.  

>[!NOTE]
>Si desea usar Administrador del servidor en lugar de Windows PowerShell para instalar el controlador de red, consulte [instalación del rol de servidor de la controladora de red mediante administrador del servidor](https://technet.microsoft.com/library/mt403348.aspx)

Para instalar la controladora de red mediante Windows PowerShell, escriba los siguientes comandos en un símbolo del sistema de Windows PowerShell y, a continuación, presione Entrar.

`Install-WindowsFeature -Name NetworkController -IncludeManagementTools`

La instalación de la controladora de red requiere que reinicie el equipo. Para ello, escriba el siguiente comando y, a continuación, presione Entrar.

`Restart-Computer`

## <a name="configure-the-network-controller-cluster"></a>Configuración del clúster de controladora de red

El clúster de controladora de red proporciona alta disponibilidad y escalabilidad a la aplicación de controladora de red, que puede configurar después de crear el clúster y que se hospeda en la parte superior del clúster.

>[!NOTE]
>Puede realizar los procedimientos de las siguientes secciones directamente en la máquina virtual en la que instaló el controlador de red, o puede usar el Herramientas de administración remota del servidor para Windows Server 2016 para realizar los procedimientos desde un equipo remoto que ejecuta Windows Server 2016 o Windows 10. Además, la pertenencia al grupo **administradores**, o equivalente, es lo mínimo necesario para realizar este procedimiento. Si el equipo o la máquina virtual en la que instaló el controlador de red está unido a un dominio, la cuenta de usuario debe ser miembro de **los usuarios del dominio**.

Puede crear un clúster de controladora de red mediante la creación de un objeto de nodo y, a continuación, la configuración del clúster.

### <a name="create-a-node-object"></a>Crear un objeto de nodo

Debe crear un objeto de nodo para cada máquina virtual que sea miembro del clúster de la controladora de red.

Para crear un objeto de nodo, escriba el siguiente comando en el símbolo del sistema de Windows PowerShell y, a continuación, presione Entrar. Asegúrese de agregar valores para cada parámetro que sean adecuados para su implementación.  

```
New-NetworkControllerNodeObject -Name <string> -Server <String> -FaultDomain <string>-RestInterface <string> [-NodeCertificate <X509Certificate2>]
```

En la tabla siguiente se proporcionan descripciones para cada parámetro del comando **New-NetworkControllerNodeObject** .

|Parámetro|Descripción|
|-------------|---------------|
|Nombre|El parámetro **Name** especifica el nombre descriptivo del servidor que desea agregar al clúster.|
|Servidor|El parámetro **Server** especifica el nombre de host, el nombre de dominio completo (FQDN) o la dirección IP del servidor que desea agregar al clúster. En el caso de los equipos Unidos a un dominio, se requiere FQDN.|
|FaultDomain|El parámetro **FaultDomain** especifica el dominio de error del servidor que se va a agregar al clúster. Este parámetro define los servidores que pueden experimentar errores al mismo tiempo que el servidor que se va a agregar al clúster. Este error puede deberse a las dependencias físicas compartidas, como fuentes de alimentación y redes. Normalmente, los dominios de error representan jerarquías que están relacionadas con estas dependencias compartidas, con más servidores que es probable que produzcan errores juntos desde un punto superior en el árbol de dominios de error. Durante el tiempo de ejecución, la controladora de red considera los dominios de error del clúster e intenta distribuir los servicios de la controladora de red para que estén en dominios de error independientes. Este proceso ayuda a garantizar, en caso de error de un dominio de error, que la disponibilidad de ese servicio y su estado no se vean comprometidas. Los dominios de error se especifican en un formato jerárquico. Por ejemplo: "FD:/DC1/Bastidor1/Host1", donde DC1 es el nombre del centro de recursos, Bastidor1 es el nombre del bastidor y Host1 es el nombre del host en el que se coloca el nodo.|
|RestInterface|El parámetro **RestInterface** especifica el nombre de la interfaz en el nodo donde se termina la comunicación de transferencia de estado REPRESENTACIONAL (REST). Esta interfaz de controladora de red recibe solicitudes de API de Northbound desde el nivel de administración de la red.|
|NodeCertificate|El parámetro **NodeCertificate** especifica el certificado que usa la controladora de red para la autenticación del equipo. El certificado es necesario si usa la autenticación basada en certificados para la comunicación dentro del clúster. el certificado también se utiliza para el cifrado del tráfico entre los servicios de la controladora de red. El nombre del firmante del certificado debe ser el mismo que el nombre DNS del nodo.|

### <a name="configure-the-cluster"></a>Configuración del clúster

Para configurar el clúster, escriba el siguiente comando en el símbolo del sistema de Windows PowerShell y, a continuación, presione Entrar. Asegúrese de agregar valores para cada parámetro que sean adecuados para su implementación.

```
Install-NetworkControllerCluster -Node <NetworkControllerNode[]> -ClusterAuthentication <ClusterAuthentication> [-ManagementSecurityGroup <string>][-DiagnosticLogLocation <string>][-LogLocationCredential <PSCredential>] [-CredentialEncryptionCertificate <X509Certificate2>][-Credential <PSCredential>][-CertificateThumbprint <String>] [-UseSSL][-ComputerName <string>][-LogSizeLimitInMBs<UInt32>] [-LogTimeLimitInDays<UInt32>]
```

En la tabla siguiente se proporcionan descripciones para cada parámetro del comando **install-NetworkControllerCluster** .
  
|Parámetro|Descripción|
|-------------|---------------|
|ClusterAuthentication|El parámetro **ClusterAuthentication** especifica el tipo de autenticación que se usa para proteger la comunicación entre los nodos y también se usa para el cifrado del tráfico entre los servicios de la controladora de red. Los valores admitidos son **Kerberos**, **X509** y **None**. La autenticación Kerberos usa cuentas de dominio y solo se puede usar si los nodos de la controladora de red están Unidos a un dominio. Si especifica la autenticación basada en X509, debe proporcionar un certificado en el objeto NetworkControllerNode. Además, debe aprovisionar el certificado manualmente antes de ejecutar este comando.|
|ManagementSecurityGroup|El parámetro **ManagementSecurityGroup** especifica el nombre del grupo de seguridad que contiene los usuarios que tienen permiso para ejecutar los cmdlets de administración desde un equipo remoto. Esto solo es aplicable si ClusterAuthentication es Kerberos. Debe especificar un grupo de seguridad de dominio y no un grupo de seguridad en el equipo local.|
|Nodo|El parámetro **node** especifica la lista de nodos de controladora de red que creó con el comando **New-NetworkControllerNodeObject** .|
|DiagnosticLogLocation|El parámetro **DiagnosticLogLocation** especifica la ubicación del recurso compartido donde se cargan periódicamente los registros de diagnóstico. Si no especifica un valor para este parámetro, los registros se almacenan localmente en cada nodo. Los registros se almacenan localmente en la carpeta%systemdrive%\Windows\tracing\SDNDiagnostics. Los registros del clúster se almacenan localmente en la carpeta%systemdrive%\ProgramData\Microsoft\Service Fabric\log\Traces.|
|LogLocationCredential|El parámetro **LogLocationCredential** especifica las credenciales necesarias para tener acceso a la ubicación del recurso compartido donde se almacenan los registros.|
|CredentialEncryptionCertificate|El parámetro **CredentialEncryptionCertificate** especifica el certificado que la controladora de red usa para cifrar las credenciales que se usan para tener acceso a los archivos binarios de la controladora de red y **LogLocationCredential**, si se especifica. El certificado debe aprovisionarse en todos los nodos de la controladora de red antes de ejecutar este comando y el mismo certificado debe inscribirse en todos los nodos del clúster. Se recomienda usar este parámetro para proteger los registros y los archivos binarios de la controladora de red en entornos de producción. Sin este parámetro, las credenciales se almacenan en texto no cifrado y se pueden usar de forma inusada por cualquier usuario no autorizado.|
|Credential|Este parámetro solo es necesario si ejecuta este comando desde un equipo remoto. El parámetro **Credential** especifica una cuenta de usuario que tiene permiso para ejecutar este comando en el equipo de destino.|
|CertificateThumbprint|Este parámetro solo es necesario si ejecuta este comando desde un equipo remoto. El parámetro **CertificateThumbprint** especifica el certificado de clave pública digital (X509) de una cuenta de usuario que tiene permiso para ejecutar este comando en el equipo de destino.|
|UseSSL|Este parámetro solo es necesario si ejecuta este comando desde un equipo remoto. El parámetro **UseSSL** especifica el protocolo capa de sockets seguros (SSL) que se usa para establecer una conexión con el equipo remoto. De forma predeterminada, no se usa SSL.|
|ComputerName|El parámetro **ComputerName** especifica el nodo de la controladora de red en el que se ejecuta este comando. Si no especifica un valor para este parámetro, se utiliza el equipo local de forma predeterminada.|
|LogSizeLimitInMBs|Este parámetro especifica el tamaño máximo del registro, en MB, que puede almacenar la controladora de red. Los registros se almacenan de manera circular. Si se proporciona DiagnosticLogLocation, el valor predeterminado de este parámetro es 40 GB. Si no se proporciona DiagnosticLogLocation, los registros se almacenan en los nodos de la controladora de red y el valor predeterminado de este parámetro es 15 GB.|
|LogTimeLimitInDays|Este parámetro especifica el límite de duración, en días, durante el que se almacenan los registros. Los registros se almacenan de manera circular. El valor predeterminado de este parámetro es 3 días.|

## <a name="configure-the-network-controller-application"></a>Configurar la aplicación de controladora de red
Para configurar la aplicación de controladora de red, escriba el siguiente comando en el símbolo del sistema de Windows PowerShell y, a continuación, presione Entrar. Asegúrese de agregar valores para cada parámetro que sean adecuados para su implementación.

```
Install-NetworkController -Node <NetworkControllerNode[]> -ClientAuthentication <ClientAuthentication>  [-ClientCertificateThumbprint <string[]>]  [-ClientSecurityGroup <string>] -ServerCertificate <X509Certificate2> [-RESTIPAddress <String>] [-RESTName <String>] [-Credential <PSCredential>][-CertificateThumbprint <String> ] [-UseSSL]
```

En la tabla siguiente se proporcionan descripciones para cada parámetro del comando **install-NetworkController** .

|Parámetro|Descripción|
|-------------|---------------|
|ClientAuthentication|El parámetro **ClientAuthentication** especifica el tipo de autenticación que se usa para proteger la comunicación entre Rest y la controladora de red. Los valores admitidos son **Kerberos**, **X509** y **None**. La autenticación Kerberos usa cuentas de dominio y solo se puede usar si los nodos de la controladora de red están Unidos a un dominio. Si especifica la autenticación basada en X509, debe proporcionar un certificado en el objeto NetworkControllerNode. Además, debe aprovisionar el certificado manualmente antes de ejecutar este comando.|
|Nodo|El parámetro **node** especifica la lista de nodos de controladora de red que creó con el comando **New-NetworkControllerNodeObject** .|
|ClientCertificateThumbprint|Este parámetro solo es necesario cuando se usa la autenticación basada en certificados para los clientes de la controladora de red. El parámetro **ClientCertificateThumbprint** especifica la huella digital del certificado que está inscrito en los clientes en el nivel de Northbound.|
|ServerCertificate|El parámetro **ServerCertificate** especifica el certificado que usa la controladora de red para demostrar su identidad a los clientes. El certificado de servidor debe incluir el propósito de autenticación del servidor en las extensiones de uso mejorado de clave y debe emitirse para la controladora de red mediante una CA de confianza para los clientes.|
|RESTIPAddress|No es necesario especificar un valor para **RESTIPAddress** con una implementación de un solo nodo de la controladora de red. En el caso de implementaciones de varios nodos, el parámetro **RESTIPAddress** especifica la dirección IP del punto de conexión Rest en la notación CIDR. Por ejemplo, 192.168.1.10/24. El valor de nombre de sujeto de **ServerCertificate** debe resolverse en el valor del parámetro **RESTIPAddress** . Este parámetro debe especificarse para todas las implementaciones de controlador de red de varios nodos cuando todos los nodos están en la misma subred. Si los nodos están en subredes diferentes, debe usar el parámetro **RestName** en lugar de usar **RESTIPAddress**.|
|RestName|No es necesario especificar un valor para **RestName** con una implementación de un solo nodo de la controladora de red. La única vez que debe especificar un valor para **RestName** es cuando las implementaciones de varios nodos tienen nodos que se encuentran en subredes diferentes. En el caso de implementaciones de varios nodos, el parámetro **RestName** especifica el FQDN del clúster de controladora de red.|
|ClientSecurityGroup|El parámetro **ClientSecurityGroup** especifica el nombre del grupo de seguridad de Active Directory cuyos miembros son clientes de la controladora de red. Este parámetro solo es necesario si se usa la autenticación Kerberos para **ClientAuthentication**. El grupo de seguridad debe contener las cuentas desde las que se tiene acceso a las API de REST y debe crear el grupo de seguridad y agregar miembros antes de ejecutar este comando.|
|Credential|Este parámetro solo es necesario si ejecuta este comando desde un equipo remoto. El parámetro **Credential** especifica una cuenta de usuario que tiene permiso para ejecutar este comando en el equipo de destino.|
|CertificateThumbprint|Este parámetro solo es necesario si ejecuta este comando desde un equipo remoto. El parámetro **CertificateThumbprint** especifica el certificado de clave pública digital (X509) de una cuenta de usuario que tiene permiso para ejecutar este comando en el equipo de destino.|
|UseSSL|Este parámetro solo es necesario si ejecuta este comando desde un equipo remoto. El parámetro **UseSSL** especifica el protocolo capa de sockets seguros (SSL) que se usa para establecer una conexión con el equipo remoto. De forma predeterminada, no se usa SSL.|

Después de completar la configuración de la aplicación de la controladora de red, se completa la implementación de la controladora de red.

## <a name="network-controller-deployment-validation"></a>Validación de la implementación de la controladora de red

Para validar la implementación de la controladora de red, puede Agregar una credencial a la controladora de red y, a continuación, recuperar la credencial.

Si usa Kerberos como mecanismo de ClientAuthentication, la pertenencia al **ClientSecurityGroup** que ha creado es el requisito mínimo para realizar este procedimiento.

**Pasos**

1.  En un equipo cliente, si usa Kerberos como mecanismo ClientAuthentication, inicie sesión con una cuenta de usuario que sea miembro de su **ClientSecurityGroup**.

2. Abra Windows PowerShell, escriba los siguientes comandos para agregar una credencial a la controladora de red y, a continuación, presione Entrar. Asegúrese de agregar valores para cada parámetro que sean adecuados para su implementación.

    ```
    $cred=New-Object Microsoft.Windows.Networkcontroller.credentialproperties
    $cred.type="usernamepassword"
    $cred.username="admin"
    $cred.value="abcd"

    New-NetworkControllerCredential -ConnectionUri https://networkcontroller -Properties $cred -ResourceId cred1
    ```

3. Para recuperar la credencial que ha agregado a la controladora de red, escriba el siguiente comando y, a continuación, presione Entrar. Asegúrese de agregar valores para cada parámetro que sean adecuados para su implementación.

    ```
    Get-NetworkControllerCredential -ConnectionUri https://networkcontroller -ResourceId cred1  
    ```

4. Revise la salida del comando, que debe ser similar a la siguiente salida de ejemplo.

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
    > Al ejecutar el comando **Get-NetworkControllerCredential** , puede asignar la salida del comando a una variable mediante el operador de punto para mostrar las propiedades de las credenciales. Por ejemplo, $cred. Propiedades.

## <a name="additional-windows-powershell-commands-for-network-controller"></a>Comandos adicionales de Windows PowerShell para la controladora de red

Después de implementar la controladora de red, puede usar comandos de Windows PowerShell para administrar y modificar la implementación. A continuación se muestran algunos de los cambios que puede realizar en su implementación de.

- Modificar la configuración del nodo, el clúster y la aplicación de la controladora de red

- Quitar el clúster de la controladora de red y la aplicación

- Administrar nodos de clúster de controladora de red, incluida la adición, eliminación, habilitación y deshabilitación de nodos.

En la tabla siguiente se proporciona la sintaxis de los comandos de Windows PowerShell que puede usar para realizar estas tareas.

|Tarea|Comando|Sintaxis|
|--------|-------|----------|
|Modificar la configuración del clúster de controladora de red|Set-NetworkControllerCluster|`Set-NetworkControllerCluster [-ManagementSecurityGroup <string>][-Credential <PSCredential>] [-computerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|Modificar la configuración de la aplicación de controladora de red|Set-NetworkController|`Set-NetworkController [-ClientAuthentication <ClientAuthentication>] [-Credential <PSCredential>] [-ClientCertificateThumbprint <string[]>] [-ClientSecurityGroup <string>] [-ServerCertificate <X509Certificate2>] [-RestIPAddress <String>] [-ComputerName <String>][-CertificateThumbprint <String> ] [-UseSSL]`
|Modificar la configuración del nodo de controladora de red|Set-NetworkControllerNode|`Set-NetworkControllerNode -Name <string> > [-RestInterface <string>] [-NodeCertificate <X509Certificate2>] [-Credential <PSCredential>] [-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|Modificar la configuración de diagnóstico de la controladora de red|Set-NetworkControllerDiagnostic|`Set-NetworkControllerDiagnostic [-LogScope <string>] [-DiagnosticLogLocation <string>] [-LogLocationCredential <PSCredential>] [-UseLocalLogLocation] >] [-LogLevel <loglevel>][-LogSizeLimitInMBs <uint32>] [-LogTimeLimitInDays <uint32>] [-Credential <PSCredential>] [-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|Quitar la aplicación de controladora de red|Uninstall-NetworkController|`Uninstall-NetworkController [-Credential <PSCredential>][-ComputerName <string>] [-CertificateThumbprint <String> ] [-UseSSL]`
|Quitar el clúster de controladora de red|Uninstall-NetworkControllerCluster|`Uninstall-NetworkControllerCluster [-Credential <PSCredential>][-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|Agregar un nodo al clúster de la controladora de red|Add-NetworkControllerNode|`Add-NetworkControllerNode -FaultDomain <String> -Name <String> -RestInterface <String> -Server <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-Force] [-NodeCertificate <X509Certificate2> ] [-PassThru] [-UseSsl]`
|Deshabilitar un nodo de clúster de controladora de red|Disable-NetworkControllerNode|`Disable-NetworkControllerNode -Name <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-PassThru] [-UseSsl]`
|Habilitación de un nodo de clúster de controladora de red|Enable-NetworkControllerNode|`Enable-NetworkControllerNode -Name <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-PassThru] [-UseSsl]`
|Quitar un nodo de controladora de red de un clúster|Remove-NetworkControllerNode|`Remove-NetworkControllerNode [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-Force] [-Name <String> ] [-PassThru] [-UseSsl]`

>[!NOTE]
>Los comandos de Windows PowerShell para la controladora de red se encuentran en la biblioteca de TechNet en [cmdlets de controladora de red](https://technet.microsoft.com/library/mt576401.aspx).

## <a name="sample-network-controller-configuration-script"></a>Script de configuración de controladora de red de ejemplo

En el siguiente script de configuración de ejemplo se muestra cómo crear un clúster de controladora de red de varios nodos e instalar la aplicación de controladora de red. Además, la variable $cert selecciona un certificado del almacén de certificados del equipo local que coincide con la cadena del nombre de sujeto "networkController.contoso.com".

```
$a = New-NetworkControllerNodeObject -Name Node1 -Server NCNode1.contoso.com -FaultDomain fd:/rack1/host1 -RestInterface Internal
$b = New-NetworkControllerNodeObject -Name Node2 -Server NCNode2.contoso.com -FaultDomain fd:/rack1/host2 -RestInterface Internal
$c = New-NetworkControllerNodeObject -Name Node3 -Server NCNode3.contoso.com -FaultDomain fd:/rack1/host3 -RestInterface Internal

$cert= get-item Cert:\LocalMachine\My | get-ChildItem | where {$_.Subject -imatch "networkController.contoso.com" }

Install-NetworkControllerCluster -Node @($a,$b,$c)  -ClusterAuthentication Kerberos -DiagnosticLogLocation \\share\Diagnostics - ManagementSecurityGroup Contoso\NCManagementAdmins -CredentialEncryptionCertificate $cert  
Install-NetworkController -Node @($a,$b,$c) -ClientAuthentication Kerberos -ClientSecurityGroup Contoso\NCRESTClients -ServerCertificate $cert -RestIpAddress 10.0.0.1/24
```

## <a name="post-deployment-steps-for-non-kerberos-deployments"></a>Pasos posteriores a la implementación para implementaciones que no son de Kerberos

Si no usa Kerberos con la implementación de la controladora de red, debe implementar certificados.

Para obtener más información, consulte [pasos posteriores a la implementación de la controladora de red](../technologies/network-controller/post-deploy-steps-nc.md).


