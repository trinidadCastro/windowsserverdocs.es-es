---
title: Implementación de controladora de red con Windows PowerShell
description: En este tema se proporcionan instrucciones sobre el uso de Windows PowerShell para implementar la controladora de red en uno o varios equipos o máquinas virtuales (VM) que ejecutan Windows Server 2019 o 2016.
ms.topic: how-to
ms.assetid: 2448d381-55aa-4c14-997a-202c537c6727
ms.author: anpaul
author: AnirbanPaul
manager: grcusanz
ms.date: 08/23/2018
ms.openlocfilehash: 3e7e020dfa5567608c53d41c4478a54ba89fb720
ms.sourcegitcommit: fb2ae5e6040cbe6dde3a87aee4a78b08f9a9ea7c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2021
ms.locfileid: "98716231"
---
# <a name="deploy-network-controller-using-windows-powershell"></a>Implementación de controladora de red con Windows PowerShell

>Se aplica a: Windows Server 2019, Windows Server 2016

En este tema se proporcionan instrucciones sobre el uso de Windows PowerShell para implementar la controladora de red en una o varias máquinas virtuales (VM) que ejecutan Windows Server 2019 o 2016.

>[!IMPORTANT]
>No implemente el rol del servidor Controladora de red en hosts físicos. Para implementar la controladora de red, debe instalar el rol de servidor de la controladora de red en una máquina virtual de Hyper-V \( \) que esté instalada en un host de Hyper-v. Una vez que haya instalado el controlador de red en las máquinas virtuales en tres hosts de Hyper- \- v diferentes, debe habilitar los \- hosts de Hyper v para las redes definidas por software de \( Sdn \) agregando los hosts a la controladora de red mediante el comando de Windows PowerShell **New-NetworkControllerServer**. Al hacerlo, permite funcionar al equilibrador de carga de las redes definidas por software. Para obtener más información, consulte [New-NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver).

En este tema se incluyen las siguientes secciones.

- [Instalación del rol del servidor Controladora de red](#install-the-network-controller-server-role)

- [Configuración del clúster de Controladora de red](#configure-the-network-controller-cluster)

- [Configuración de la aplicación de Controladora de red](#configure-the-network-controller-application)

- [Validación de la implementación de Controladora de red](#network-controller-deployment-validation)

- [Comandos adicionales de Windows PowerShell para la controladora de red](#additional-windows-powershell-commands-for-network-controller)

- [Script de configuración de Controladora de red de ejemplo](#sample-network-controller-configuration-script)

- [Pasos posteriores a la implementación para implementaciones que no son de Kerberos](#post-deployment-steps-for-non-kerberos-deployments)

## <a name="install-the-network-controller-server-role"></a>Instalación del rol del servidor Controladora de red

Puede usar este procedimiento para instalar el rol de servidor de la controladora de red en una máquina virtual de máquinas virtuales \( \) .

>[!IMPORTANT]
>No implemente el rol del servidor Controladora de red en hosts físicos. Para implementar la controladora de red, debe instalar el rol de servidor de la controladora de red en una máquina virtual de Hyper-V \( \) que esté instalada en un host de Hyper-v. Después de haber instalado el controlador de red en las máquinas virtuales en tres hosts de Hyper- \- v diferentes, debe habilitar los \- hosts de Hyper v para las redes definidas por software \( \) mediante la adición de los hosts a la controladora de red. Al hacerlo, permite funcionar al equilibrador de carga de las redes definidas por software.

El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o grupo equivalente.

>[!NOTE]
>Si desea usar Administrador del servidor en lugar de Windows PowerShell para instalar Controladora de red, consulte [Instalación del rol del servidor Controladora de red mediante Administrador del servidor](../technologies/network-controller/install-the-network-controller-server-role-using-server-manager.md)

Para instalar la controladora de red mediante Windows PowerShell, escriba los siguientes comandos en un símbolo del sistema de Windows PowerShell y, a continuación, presione Entrar.

`Install-WindowsFeature -Name NetworkController -IncludeManagementTools`

La instalación de Controladora de red requiere que reinicie el equipo. Para ello, escriba el siguiente comando y, a continuación, presione Entrar.

`Restart-Computer`

## <a name="configure-the-network-controller-cluster"></a>Configuración del clúster de Controladora de red

El clúster de controladora de red proporciona alta disponibilidad y escalabilidad a la aplicación de controladora de red, que puede configurar después de crear el clúster y que se hospeda en la parte superior del clúster.

>[!NOTE]
>Puede realizar los procedimientos de las siguientes secciones directamente en la máquina virtual en la que instaló el controlador de red, o puede usar el Herramientas de administración remota del servidor para Windows Server 2016 para realizar los procedimientos desde un equipo remoto que ejecute Windows Server 2016 o Windows 10. Además, la pertenencia al grupo **administradores**, o equivalente, es lo mínimo necesario para realizar este procedimiento. Si el equipo o la máquina virtual en la que instaló el controlador de red está unido a un dominio, la cuenta de usuario debe ser miembro de **los usuarios del dominio**.

Puede crear un clúster de Controladora de red mediante la creación de un objeto de nodo y, posteriormente, la configuración del clúster.

### <a name="create-a-node-object"></a>Creación de un objeto de nodo

Debe crear un objeto de nodo para cada máquina virtual que sea miembro del clúster de Controladora de red.

Para crear un objeto de nodo, escriba el siguiente comando en el símbolo del sistema de Windows PowerShell y, a continuación, presione Entrar. Asegúrese de agregar valores para cada parámetro que sean adecuados para su implementación.

```
New-NetworkControllerNodeObject -Name <string> -Server <String> -FaultDomain <string>-RestInterface <string> [-NodeCertificate <X509Certificate2>]
```

En la tabla siguiente se proporcionan descripciones para cada parámetro del comando **New-NetworkControllerNodeObject** .

|Parámetro|Descripción|
|-------------|---------------|
|Nombre|El parámetro **Name** especifica el nombre descriptivo del servidor que desea agregar al clúster.|
|Servidor|El parámetro **Server** especifica el nombre de host, el nombre de dominio completo o la dirección IP del servidor que desea agregar al clúster. En los equipos unidos a dominios se requiere el nombre de dominio completo.|
|FaultDomain|El parámetro **FaultDomain** especifica el dominio de error del servidor que se va a agregar al clúster. Este parámetro define los servidores que pueden experimentar errores al mismo tiempo que el servidor que se va a agregar al clúster. Este error puede deberse a las dependencias físicas compartidas como, por ejemplo, las fuentes de alimentación y las redes. Los dominios de error suelen representar jerarquías que están relacionadas con estas dependencias compartidas, con más servidores con más probabilidades de experimentar errores conjuntamente desde un punto superior del árbol de dominios de error. Durante el tiempo de ejecución, Controladora de red considera los dominios de error del clúster e intenta propagar los servicios de Controladora de red para que estén en dominios de error independientes. Este proceso ayuda a garantizar, en caso de error de cualquier dominio de error, que no se ponga en peligro la disponibilidad del servicio y su estado. Los dominios de error se especifican en un formato jerárquico. Por ejemplo: "Fd:/DC1/Rack1/Host1", donde DC1 es el nombre del centro de datos, Rack1 es el nombre del bastidor y Host1 es el nombre del host en el que está colocado el nodo.|
|RestInterface|El parámetro **RestInterface** especifica el nombre de la interfaz en el nodo donde se termina la comunicación de Transferencia de estado representacional (REST). Esta interfaz de Controladora de red recibe solicitudes de Northbound API desde el nivel de administración de la red.|
|NodeCertificate|El parámetro **NodeCertificate** especifica el certificado que usa Controladora de red para la autenticación del equipo. El certificado es necesario si usa la autenticación basada en certificados para la comunicación dentro del clúster. El certificado también se utiliza para el cifrado del tráfico entre los servicios de Controladora de red. El nombre del firmante del certificado debe ser el mismo que el nombre DNS del nodo.|

### <a name="configure-the-cluster"></a>Configuración del clúster

Para configurar el clúster, escriba el siguiente comando en el símbolo del sistema de Windows PowerShell y, a continuación, presione Entrar. Asegúrese de agregar valores para cada parámetro que sean adecuados para su implementación.

```
Install-NetworkControllerCluster -Node <NetworkControllerNode[]> -ClusterAuthentication <ClusterAuthentication> [-ManagementSecurityGroup <string>][-DiagnosticLogLocation <string>][-LogLocationCredential <PSCredential>] [-CredentialEncryptionCertificate <X509Certificate2>][-Credential <PSCredential>][-CertificateThumbprint <String>] [-UseSSL][-ComputerName <string>][-LogSizeLimitInMBs<UInt32>] [-LogTimeLimitInDays<UInt32>]
```

En la tabla siguiente se proporcionan descripciones para cada parámetro del comando **install-NetworkControllerCluster**  .

|Parámetro|Descripción|
|-------------|---------------|
|ClusterAuthentication|El parámetro **ClusterAuthentication** especifica el tipo de autenticación que se usa para proteger la comunicación entre los nodos y también se usa para el cifrado del tráfico entre los servicios de Controladora de red. Los valores admitidos son **Kerberos**, **X509** y **Ninguno**. La autenticación Kerberos usa cuentas de dominio y solo se puede usar si los nodos de Controladora de red están unidos a un dominio. Si especifica la autenticación basada en X509, debe proporcionar un certificado en el objeto NetworkControllerNode. Además, debe aprovisionar el certificado manualmente antes de ejecutar este comando.|
|ManagementSecurityGroup|El parámetro **ManagementSecurityGroup** especifica el nombre del grupo de seguridad que contiene los usuarios que pueden ejecutar los cmdlets de administración desde un equipo remoto. Esto solo es aplicable si ClusterAuthentication es Kerberos. Debe especificar un grupo de seguridad de un dominio y no un grupo de seguridad en el equipo local.|
|Nodo|El parámetro **Node** especifica la lista de nodos de Controladora de red que ha creado mediante el comando **New-NetworkControllerNodeObject**.|
|DiagnosticLogLocation|El parámetro **DiagnosticLogLocation** especifica la ubicación del recurso compartido donde se cargan periódicamente los registros de diagnóstico. Si no especifica un valor para este parámetro, los registros se almacenan localmente en cada nodo. Los registros se almacenan localmente en la carpeta %systemdrive%\Windows\tracing\SDNDiagnostics. Los registros del clúster se almacenan localmente en la carpeta %systemdrive%\ProgramData\Microsoft\Service Fabric\log\Traces.|
|LogLocationCredential|El parámetro **LogLocationCredential** especifica las credenciales necesarias para acceder a la ubicación del recurso compartido donde se almacenan los registros.|
|CredentialEncryptionCertificate|El parámetro **CredentialEncryptionCertificate** especifica el certificado que usa Controladora de red para cifrar las credenciales que se emplean para acceder a los archivos binarios de Controladora de red y al valor de **LogLocationCredential**, si se especifica. El certificado debe aprovisionarse en todos los nodos de Controladora de red antes de ejecutar este comando y el mismo certificado debe inscribirse en todos los nodos del clúster. Se recomienda usar este parámetro para proteger los registros y los archivos binarios de Controladora de red en entornos de producción. Sin este parámetro, las credenciales se almacenan en texto no cifrado y cualquier usuario no autorizado podría hacer un uso inadecuado de ellas.|
|Credential:|Este parámetro solo es necesario si ejecuta este comando desde un equipo remoto. El parámetro **Credential** especifica una cuenta de usuario que tiene permiso para ejecutar este comando en el equipo de destino.|
|CertificateThumbprint|Este parámetro solo es necesario si ejecuta este comando desde un equipo remoto. El parámetro **CertificateThumbprint** especifica el certificado de clave pública digital (X509) de una cuenta de usuario que tiene permiso para ejecutar este comando en el equipo de destino.|
|UseSSL|Este parámetro solo es necesario si ejecuta este comando desde un equipo remoto. El parámetro **UseSSL** especifica el protocolo de capa de sockets seguros (SSL) que se emplea para establecer una conexión con el equipo remoto. De forma predeterminada, no se usa SSL.|
|ComputerName|El parámetro **ComputerName** especifica el nodo de Controladora de red en el que se ejecuta este comando. Si no especifica un valor para este parámetro, se utiliza el equipo local de manera predeterminada.|
|LogSizeLimitInMBs|Este parámetro especifica el tamaño máximo del registro, en MB, que puede almacenar Controladora de red. Los registros se almacenan de manera circular. Si se proporciona DiagnosticLogLocation, el valor predeterminado de este parámetro es 40 GB. Si no se proporciona DiagnosticLogLocation, los registros se almacenan en los nodos de Controladora de red y el valor predeterminado de este parámetro es 15 GB.|
|LogTimeLimitInDays|Este parámetro especifica el límite de duración, en días, durante el que se almacenan los registros. Los registros se almacenan de manera circular. El valor predeterminado para este parámetro es 3 días.|

## <a name="configure-the-network-controller-application"></a>Configuración de la aplicación de Controladora de red
Para configurar la aplicación de controladora de red, escriba el siguiente comando en el símbolo del sistema de Windows PowerShell y, a continuación, presione Entrar. Asegúrese de agregar valores para cada parámetro que sean adecuados para su implementación.

```
Install-NetworkController -Node <NetworkControllerNode[]> -ClientAuthentication <ClientAuthentication>  [-ClientCertificateThumbprint <string[]>]  [-ClientSecurityGroup <string>] -ServerCertificate <X509Certificate2> [-RESTIPAddress <String>] [-RESTName <String>] [-Credential <PSCredential>][-CertificateThumbprint <String> ] [-UseSSL]
```

En la tabla siguiente se proporcionan descripciones para cada parámetro del comando **install-NetworkController** .

|Parámetro|Descripción|
|-------------|---------------|
|ClientAuthentication|El parámetro **ClientAuthentication** especifica el tipo de autenticación que se usa para proteger la comunicación entre REST y Controladora de red. Los valores admitidos son **Kerberos**, **X509** y **Ninguno**. La autenticación Kerberos usa cuentas de dominio y solo se puede usar si los nodos de Controladora de red están unidos a un dominio. Si especifica la autenticación basada en X509, debe proporcionar un certificado en el objeto NetworkControllerNode. Además, debe aprovisionar el certificado manualmente antes de ejecutar este comando.|
|Nodo|El parámetro **Node** especifica la lista de nodos de Controladora de red que ha creado mediante el comando **New-NetworkControllerNodeObject**.|
|ClientCertificateThumbprint|Este parámetro solo es necesario cuando se usa la autenticación basada en certificados para los clientes de Controladora de red. El parámetro **ClientCertificateThumbprint** especifica la huella digital del certificado que está inscrito en los clientes en el nivel de Northbound.|
|ServerCertificate|El parámetro **ServerCertificate** especifica el certificado que usa Controladora de red para demostrar su identidad a los clientes. El certificado de servidor debe incluir el propósito de autenticación del servidor en las extensiones de uso mejorado de clave y debe emitirse para Controladora de red mediante una entidad de certificación de confianza para los clientes.|
|RESTIPAddress|No es necesario especificar un valor para **RESTIPAddress** con una implementación de un solo nodo de Controladora de red. En el caso de implementaciones de varios nodos, el parámetro **RESTIPAddress** especifica la dirección IP del punto de conexión de REST en la notación CIDR. Por ejemplo: 192.168.1.10/24. El valor Nombre de sujeto de **ServerCertificate** debe resolverse en el valor del parámetro **RESTIPAddress**. Este parámetro debe especificarse para todas las implementaciones de Controladora de red de varios nodos cuando todos los nodos están en la misma subred. Si los nodos están en subredes diferentes, debe usar el parámetro **RestName** en lugar de usar **RESTIPAddress**.|
|RestName|No es necesario especificar un valor para **RestName** con una implementación de un solo nodo de Controladora de red. La única vez que debe especificar un valor para **RestName** es cuando las implementaciones de varios nodos tienen nodos que están en subredes diferentes. En el caso de implementaciones de varios nodos, el parámetro **RestName** especifica el nombre de dominio completo del clúster de Controladora de red.|
|ClientSecurityGroup|El parámetro **ClientSecurityGroup** especifica el nombre del grupo de seguridad de Active Directory cuyos miembros son clientes de Controladora de red. Este parámetro solo es necesario si se usa la autenticación Kerberos para **ClientAuthentication**. El grupo de seguridad debe contener las cuentas desde las que se accede a las API REST y debe crear el grupo de seguridad y agregar miembros antes de ejecutar este comando.|
|Credential:|Este parámetro solo es necesario si ejecuta este comando desde un equipo remoto. El parámetro **Credential** especifica una cuenta de usuario que tiene permiso para ejecutar este comando en el equipo de destino.|
|CertificateThumbprint|Este parámetro solo es necesario si ejecuta este comando desde un equipo remoto. El parámetro **CertificateThumbprint** especifica el certificado de clave pública digital (X509) de una cuenta de usuario que tiene permiso para ejecutar este comando en el equipo de destino.|
|UseSSL|Este parámetro solo es necesario si ejecuta este comando desde un equipo remoto. El parámetro **UseSSL** especifica el protocolo de capa de sockets seguros (SSL) que se emplea para establecer una conexión con el equipo remoto. De forma predeterminada, no se usa SSL.|

Después de completar la configuración de la aplicación de Controladora de red, la implementación de esta ha finalizado.

## <a name="network-controller-deployment-validation"></a>Validación de la implementación de Controladora de red

Para validar la implementación de Controladora de red, puede agregar una credencial a Controladora de red y, a continuación, recuperarla.

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

4. Revise la salida de este comando, la cual debería ser similar a la del ejemplo siguiente.

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

Después de implementar la controladora de red, puede usar comandos de Windows PowerShell para administrar y modificar la implementación. A continuación se muestran algunos de los cambios que puede realizar en su implementación.

- Modificar la configuración del nodo, el clúster y la aplicación de la controladora de red

- Eliminar el clúster y la aplicación de Controladora de red

- Administrar nodos de clúster de Controladora de red, incluida la adición, eliminación, habilitación y deshabilitación de nodos.

En la tabla siguiente se proporciona la sintaxis de los comandos de Windows PowerShell que puede usar para realizar estas tareas.

|Tarea|Get-Help|Sintaxis|
|--------|-------|----------|
|Modificar la configuración del clúster de Controladora de red|Set-NetworkControllerCluster|`Set-NetworkControllerCluster [-ManagementSecurityGroup <string>][-Credential <PSCredential>] [-computerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|Modificar la configuración de la aplicación de Controladora de red|Set-NetworkController|`Set-NetworkController [-ClientAuthentication <ClientAuthentication>] [-Credential <PSCredential>] [-ClientCertificateThumbprint <string[]>] [-ClientSecurityGroup <string>] [-ServerCertificate <X509Certificate2>] [-RestIPAddress <String>] [-ComputerName <String>][-CertificateThumbprint <String> ] [-UseSSL]`
|Modificar la configuración del nodo de Controladora de red|Set-NetworkControllerNode|`Set-NetworkControllerNode -Name <string> > [-RestInterface <string>] [-NodeCertificate <X509Certificate2>] [-Credential <PSCredential>] [-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|Modificar la configuración de diagnóstico de Controladora de red|Set-NetworkControllerDiagnostic|`Set-NetworkControllerDiagnostic [-LogScope <string>] [-DiagnosticLogLocation <string>] [-LogLocationCredential <PSCredential>] [-UseLocalLogLocation] >] [-LogLevel <loglevel>][-LogSizeLimitInMBs <uint32>] [-LogTimeLimitInDays <uint32>] [-Credential <PSCredential>] [-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|Quitar la aplicación de Controladora de red|Uninstall-NetworkController|`Uninstall-NetworkController [-Credential <PSCredential>][-ComputerName <string>] [-CertificateThumbprint <String> ] [-UseSSL]`
|Quitar el clúster de Controladora de red|Uninstall-NetworkControllerCluster|`Uninstall-NetworkControllerCluster [-Credential <PSCredential>][-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|Agregar un nodo al clúster de Controladora de red|Add-NetworkControllerNode|`Add-NetworkControllerNode -FaultDomain <String> -Name <String> -RestInterface <String> -Server <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-Force] [-NodeCertificate <X509Certificate2> ] [-PassThru] [-UseSsl]`
|Deshabilitar un nodo de clúster de Controladora de red|Disable-NetworkControllerNode|`Disable-NetworkControllerNode -Name <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-PassThru] [-UseSsl]`
|Habilitar un nodo de clúster de Controladora de red|Enable-NetworkControllerNode|`Enable-NetworkControllerNode -Name <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-PassThru] [-UseSsl]`
|Quitar un nodo de controladora de red de un clúster|Remove-NetworkControllerNode|`Remove-NetworkControllerNode [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-Force] [-Name <String> ] [-PassThru] [-UseSsl]`

>[!NOTE]
>Los comandos de Windows PowerShell para la controladora de red se encuentran en la biblioteca de TechNet en [cmdlets de controladora de red](https://technet.microsoft.com/library/mt576401.aspx).

## <a name="sample-network-controller-configuration-script"></a>Script de configuración de Controladora de red de ejemplo

En el siguiente script de configuración de ejemplo se muestra cómo crear un clúster de Controladora de red de varios nodos e instalar la aplicación de Controladora de red. Además, la variable $cert selecciona un certificado del almacén de certificados del equipo local que coincide con la cadena del nombre de sujeto "networkController.contoso.com".

```
$a = New-NetworkControllerNodeObject -Name Node1 -Server NCNode1.contoso.com -FaultDomain fd:/rack1/host1 -RestInterface Internal
$b = New-NetworkControllerNodeObject -Name Node2 -Server NCNode2.contoso.com -FaultDomain fd:/rack1/host2 -RestInterface Internal
$c = New-NetworkControllerNodeObject -Name Node3 -Server NCNode3.contoso.com -FaultDomain fd:/rack1/host3 -RestInterface Internal

$cert= get-item Cert:\LocalMachine\My | get-ChildItem | where {$_.Subject -imatch "networkController.contoso.com" }

Install-NetworkControllerCluster -Node @($a,$b,$c)  -ClusterAuthentication Kerberos -DiagnosticLogLocation \\share\Diagnostics - ManagementSecurityGroup Contoso\NCManagementAdmins -CredentialEncryptionCertificate $cert
Install-NetworkController -Node @($a,$b,$c) -ClientAuthentication Kerberos -ClientSecurityGroup Contoso\NCRESTClients -ServerCertificate $cert -RestIpAddress 10.0.0.1/24
```

## <a name="post-deployment-steps-for-non-kerberos-deployments"></a>Pasos posteriores a la implementación para implementaciones que no son de Kerberos

Si no usa Kerberos con la implementación de Controladora de red, debe implementar certificados.

Para más información, consulte los [pasos posteriores a la implementación de Controladora de red](../technologies/network-controller/post-deploy-steps-nc.md).