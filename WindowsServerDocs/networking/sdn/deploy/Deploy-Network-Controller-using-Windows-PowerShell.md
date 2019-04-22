---
title: Implementación de controladora de red con Windows PowerShell
description: Este tema proporciona instrucciones sobre el uso de Windows PowerShell para implementar la controladora de red en uno o varios equipos o máquinas virtuales (VM) que ejecutan Windows Server 2016.
manager: dougkim
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
ms.date: 08/23/2018
ms.openlocfilehash: 31c1579dc840f6f4eb805ac4e10f51192a6b4c99
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816196"
---
# <a name="deploy-network-controller-using-windows-powershell"></a>Implementación de controladora de red con Windows PowerShell

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Este tema proporciona instrucciones sobre el uso de Windows PowerShell para implementar la controladora de red en una o varias máquinas de virtuales (VM) que ejecutan Windows Server 2016.

>[!IMPORTANT]
>No implemente el rol de servidor de controladora de red en hosts físicos. Para implementar la controladora de red, debe instalar el rol de servidor de controladora de red en una máquina virtual de Hyper-V \(VM\) que está instalado en un host de Hyper-V. Después de haber instalado el controlador de red en máquinas virtuales en Hyper diferentes tres\-hosts V, debe habilitar Hyper\-hosts V para redes definidas por Software \(SDN\) mediante la adición de los hosts al uso de la controladora de red el comando de Windows PowerShell **New NetworkControllerServer**. Al hacerlo, permite que el equilibrador de carga de Software de SDN a función. Para obtener más información, consulte [New NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver).

En este tema se incluyen las siguientes secciones.

- [Instalar el rol de servidor de controladora de red](#bkmk_role)

- [Configurar el clúster de controladora de red](#bkmk_configure)

- [Configurar la aplicación de la controladora de red](#bkmk_app)

- [Validación de la implementación de controladora de red](#bkmk_validation)

- [Comandos de Windows PowerShell adicionales para la controladora de red](#bkmk_ps)

- [Script de configuración de controladora de red de ejemplo](#bkmk_script)

- [Pasos posteriores a la implementación para las implementaciones que no sea de Kerberos](#bkmk_nonkerb)

## <a name="install-the-network-controller-server-role"></a>Instalar el rol de servidor de controladora de red

Puede usar este procedimiento para instalar el rol de servidor de controladora de red en una máquina virtual \(VM\).

>[!IMPORTANT]
>No implemente el rol de servidor de controladora de red en hosts físicos. Para implementar la controladora de red, debe instalar el rol de servidor de controladora de red en una máquina virtual de Hyper-V \(VM\) que está instalado en un host de Hyper-V. Después de haber instalado el controlador de red en máquinas virtuales en Hyper diferentes tres\-hosts V, debe habilitar Hyper\-hosts V para redes definidas por Software \(SDN\) mediante la adición de hosts de la controladora de red. Al hacerlo, permite que el equilibrador de carga de Software de SDN a función.

El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o grupo equivalente.  

>[!NOTE]
>Si desea usar el administrador del servidor en lugar de Windows PowerShell para instalar el controlador de red, consulte [instalar el rol de servidor de controladora de red con el administrador del servidor](https://technet.microsoft.com/library/mt403348.aspx)

Para instalar el controlador de red mediante Windows PowerShell, escriba los siguientes comandos en un símbolo del sistema de Windows PowerShell y, a continuación, presione ENTRAR.

`Install-WindowsFeature -Name NetworkController -IncludeManagementTools`

Instalación de controladora de red requiere que se reinicie el equipo. Para ello, escriba el siguiente comando y, a continuación, presione ENTRAR.

`Restart-Computer`

## <a name="configure-the-network-controller-cluster"></a>Configurar el clúster de controladora de red

El clúster de controladora de red proporciona alta disponibilidad y escalabilidad a la aplicación controladora de red, que puede configurar después de crear el clúster y que se hospeda en la parte superior del clúster.

>[!NOTE]
>Puede realizar los procedimientos en las secciones siguientes, ya sea directamente en la máquina virtual donde instaló el controlador de red, o puede usar remoto Server Administration Tools para Windows Server 2016 para realizar los procedimientos de un equipo remoto que se está ejecutando Windows Server 2016 o Windows 10. Además, la pertenencia en **administradores**, o equivalente, es lo mínimo necesario para llevar a cabo este procedimiento. Si el equipo o máquina virtual en el que instaló el controlador de red está unido a un dominio, la cuenta de usuario debe ser miembro de **los usuarios del dominio**.

Puede crear un clúster de controladora de red mediante la creación de un objeto de nodo y, a continuación, configurar el clúster.

### <a name="create-a-node-object"></a>Crear un objeto de nodo

Deberá crear un objeto de nodo para cada máquina virtual que forme parte del clúster de la controladora de red.

Para crear un objeto de nodo, escriba el siguiente comando en el símbolo del sistema de Windows PowerShell y, a continuación, presione ENTRAR. Asegúrese de agregar valores para cada parámetro que son adecuados para su implementación.  

```
New-NetworkControllerNodeObject -Name <string> -Server <String> -FaultDomain <string>-RestInterface <string> [-NodeCertificate <X509Certificate2>]
```

En la tabla siguiente se proporciona descripciones para cada parámetro de la **New NetworkControllerNodeObject** comando.

|Parámetro|Descripción|
|-------------|---------------|
|Nombre|El **nombre** parámetro especifica el nombre descriptivo del servidor que desea agregar al clúster|
|Servidor|El **Server** parámetro especifica el nombre de host, el nombre de dominio completo (FQDN) o la dirección IP del servidor que desea agregar al clúster. Para los equipos unidos al dominio, el FQDN es necesario.|
|FaultDomain|El **FaultDomain** parámetro especifica el dominio de error para el servidor que va a agregar al clúster. Este parámetro define los servidores que podrían experimentar errores al mismo tiempo que el servidor que va a agregar al clúster. Este error puede ser debido a las dependencias físicas compartidas como la energía y orígenes de redes. Dominios de error suelen representan jerarquías que están relacionados con estas dependencias compartidas, con los servidores más propenso a errores conjuntamente desde un punto posterior en el árbol de dominios de error. En tiempo de ejecución, la controladora de red tiene en cuenta los dominios de error en el clúster e intenta distribuir los servicios de la controladora de red para que estén en dominios de error independientes. Este proceso ayuda a garantizar, en caso de error de cualquier dominio de error, que no está en peligro la disponibilidad del servicio y su estado. Dominios de error se especifican en un formato jerárquico. Por ejemplo: "Fd: / DC1/bastidor1/Host1", donde DC1 es el nombre del centro de datos, bastidor1 es el nombre de la estantería y Host1 es el nombre del host donde se coloca el nodo.|
|RestInterface|El **RestInterface** parámetro especifica el nombre de la interfaz en el nodo donde se finalizó la comunicación de Representational State Transfer (REST). Esta interfaz de controladora de red recibe las solicitudes de API Northbound de capa de administración de la red.|
|NodeCertificate|El **NodeCertificate** parámetro especifica el certificado de la controladora de red se usa para la autenticación de equipo. El certificado es necesario si utiliza la autenticación basada en certificados para la comunicación dentro del clúster. el certificado también se usa para el cifrado de tráfico entre los servicios de la controladora de red. El nombre de sujeto del certificado debe ser igual que el nombre DNS del nodo.|

### <a name="configure-the-cluster"></a>Configurar el clúster

Para configurar el clúster, escriba el siguiente comando en el símbolo del sistema de Windows PowerShell y, a continuación, presione ENTRAR. Asegúrese de agregar valores para cada parámetro que son adecuados para su implementación.

```
Install-NetworkControllerCluster -Node <NetworkControllerNode[]> -ClusterAuthentication <ClusterAuthentication> [-ManagementSecurityGroup <string>][-DiagnosticLogLocation <string>][-LogLocationCredential <PSCredential>] [-CredentialEncryptionCertificate <X509Certificate2>][-Credential <PSCredential>][-CertificateThumbprint <String>] [-UseSSL][-ComputerName <string>][-LogSizeLimitInMBs<UInt32>] [-LogTimeLimitInDays<UInt32>]
```

En la tabla siguiente se proporciona descripciones para cada parámetro de la **Install NetworkControllerCluster** comando.
  
|Parámetro|Descripción|
|-------------|---------------|
|ClusterAuthentication|El **ClusterAuthentication** parámetro especifica el tipo de autenticación que se usa para proteger la comunicación entre nodos y también se usa para el cifrado de tráfico entre los servicios de la controladora de red. Los valores admitidos son **Kerberos**, **X509** y **ninguno**. La autenticación Kerberos utiliza cuentas de dominio y solo puede usarse si los nodos de controladora de red están unidos a un dominio. Si especifica la autenticación basada en X509, debe proporcionar un certificado en el objeto NetworkControllerNode. Además, debe aprovisionar manualmente el certificado antes de ejecutar este comando.|
|ManagementSecurityGroup|El **ManagementSecurityGroup** parámetro especifica el nombre del grupo de seguridad que contiene usuarios que tienen permiso para ejecutar los cmdlets de administración desde un equipo remoto. Esto solo es aplicable si ClusterAuthentication es Kerberos. Debe especificar un grupo de seguridad de dominio y no un grupo de seguridad en el equipo local.|
|Nodo|El **nodo** parámetro especifica la lista de nodos de controladora de red que ha creado mediante el **New NetworkControllerNodeObject** comando.|
|DiagnosticLogLocation|El **DiagnosticLogLocation** parámetro especifica la ubicación del recurso compartido donde están cargando periódicamente los registros de diagnóstico. Si no especifica un valor para este parámetro, los registros se almacenan localmente en cada nodo. Los registros se almacenan localmente en el systemdrive%\Windows\tracing\SDNDiagnostics % carpeta. Los registros de clúster se almacenan localmente en el %systemdrive%\ProgramData\Microsoft\Service carpeta Fabric\log\Traces.|
|LogLocationCredential|El **LogLocationCredential** parámetro especifica las credenciales necesarias para tener acceso a la ubicación del recurso compartido donde se almacenan los registros.|
|CredentialEncryptionCertificate|El **CredentialEncryptionCertificate** parámetro especifica el certificado de la controladora de red se usa para cifrar las credenciales que se usan para tener acceso a archivos binarios de la controladora de red y el  **LogLocationCredential**, si se especifica. El certificado debe aprovisionarse en todos los nodos de controladora de red antes de ejecutar este comando, y el mismo certificado debe estar inscrito en todos los nodos del clúster. Se recomienda el uso de este parámetro para proteger registros y archivos binarios de la controladora de red en entornos de producción. Sin este parámetro, las credenciales se almacenan en texto no cifrado y se pueden usar incorrectamente a cualquier usuario no autorizado.|
|Credential|Este parámetro es necesario sólo si está ejecutando este comando desde un equipo remoto. El **credencial** parámetro especifica una cuenta de usuario que tenga permiso para ejecutar este comando en el equipo de destino.|
|CertificateThumbprint|Este parámetro es necesario sólo si está ejecutando este comando desde un equipo remoto. El **CertificateThumbprint** parámetro especifica digital certificado de clave pública (X509) de una cuenta de usuario que tenga permiso para ejecutar este comando en el equipo de destino.|
|UseSSL|Este parámetro es necesario sólo si está ejecutando este comando desde un equipo remoto. El **UseSSL** parámetro especifica el protocolo de capa de Sockets seguros (SSL) que se usa para establecer una conexión con el equipo remoto. De forma predeterminada, no se usa SSL.|
|ComputerName|El **ComputerName** parámetro especifica el nodo de controladora de red en el que este comando se ejecute. Si no especifica un valor para este parámetro, el equipo local se usa de forma predeterminada.|
|LogSizeLimitInMBs|Este parámetro especifica el tamaño máximo del registro, en MB, que puede almacenar la controladora de red. Los registros se almacenan de manera circular. Si se proporciona DiagnosticLogLocation, el valor predeterminado de este parámetro es de 40 GB. Si no se proporciona DiagnosticLogLocation, los registros se almacenan en los nodos de controladora de red y el valor predeterminado de este parámetro es de 15 GB.|
|LogTimeLimitInDays|Este parámetro especifica el límite de duración en días, para el que se almacenan los registros. Los registros se almacenan de manera circular. El valor predeterminado de este parámetro es 3 días.|

## <a name="configure-the-network-controller-application"></a>Configurar la aplicación de la controladora de red
Para configurar la aplicación de la controladora de red, escriba el siguiente comando en el símbolo del sistema de Windows PowerShell y, a continuación, presione ENTRAR. Asegúrese de agregar valores para cada parámetro que son adecuados para su implementación.

```
Install-NetworkController -Node <NetworkControllerNode[]> -ClientAuthentication <ClientAuthentication>  [-ClientCertificateThumbprint <string[]>]  [-ClientSecurityGroup <string>] -ServerCertificate <X509Certificate2> [-RESTIPAddress <String>] [-RESTName <String>] [-Credential <PSCredential>][-CertificateThumbprint <String> ] [-UseSSL]
```

En la tabla siguiente se proporciona descripciones para cada parámetro de la **Install NetworkController** comando.

|Parámetro|Descripción|
|-------------|---------------|
|ClientAuthentication|El **ClientAuthentication** parámetro especifica el tipo de autenticación que se usa para proteger la comunicación entre REST y la controladora de red. Los valores admitidos son **Kerberos**, **X509** y **ninguno**. La autenticación Kerberos utiliza cuentas de dominio y solo puede usarse si los nodos de controladora de red están unidos a un dominio. Si especifica la autenticación basada en X509, debe proporcionar un certificado en el objeto NetworkControllerNode. Además, debe aprovisionar manualmente el certificado antes de ejecutar este comando.|
|Nodo|El **nodo** parámetro especifica la lista de nodos de controladora de red que ha creado mediante el **New NetworkControllerNodeObject** comando.|
|ClientCertificateThumbprint|Este parámetro es necesario únicamente cuando se usa la autenticación basada en certificados para los clientes de la controladora de red. El **ClientCertificateThumbprint** parámetro especifica la huella digital del certificado inscrito a los clientes de la capa de Northbound.|
|ServerCertificate|El **ServerCertificate** parámetro especifica el certificado de la controladora de red se usa para demostrar su identidad a los clientes. El certificado de servidor debe incluir el propósito de autenticación de servidor en extensiones de uso mejorado de clave y se debe emitir para la controladora de red por una CA de confianza para los clientes.|
|RESTIPAddress|No es necesario especificar un valor para **RESTIPAddress** con una implementación de un nodo de controladora de red. Para las implementaciones de varios nodos, el **RESTIPAddress** parámetro especifica la dirección IP del punto de conexión REST en la notación CIDR. Por ejemplo, 192.168.1.10/24. El valor de nombre de sujeto de **ServerCertificate** debe resolverse en el valor de la **RESTIPAddress** parámetro. Este parámetro debe especificarse para todas las implementaciones de controladora de red de varios nodos cuando todos los nodos están en la misma subred. Si los nodos están en subredes diferentes, se debe utilizar el **RestName** parámetro en lugar de usar **RESTIPAddress**.|
|RestName|No es necesario especificar un valor para **RestName** con una implementación de un nodo de controladora de red. La única vez que debe especificar un valor para **RestName** es cuando las implementaciones de varios nodos tienen nodos que están en subredes diferentes. Para las implementaciones de varios nodos, el **RestName** parámetro especifica el FQDN para el clúster de controladora de red.|
|ClientSecurityGroup|El **ClientSecurityGroup** parámetro especifica el nombre del grupo de seguridad de Active Directory cuyos miembros son los clientes de la controladora de red. Este parámetro es obligatorio solo si usa la autenticación de Kerberos para **ClientAuthentication**. El grupo de seguridad debe contener las cuentas desde el que se tiene acceso a las API de REST, y debe crear el grupo de seguridad y agregar a miembros antes de ejecutar este comando.|
|Credential|Este parámetro es necesario sólo si está ejecutando este comando desde un equipo remoto. El **credencial** parámetro especifica una cuenta de usuario que tenga permiso para ejecutar este comando en el equipo de destino.|
|CertificateThumbprint|Este parámetro es necesario sólo si está ejecutando este comando desde un equipo remoto. El **CertificateThumbprint** parámetro especifica digital certificado de clave pública (X509) de una cuenta de usuario que tenga permiso para ejecutar este comando en el equipo de destino.|
|UseSSL|Este parámetro es necesario sólo si está ejecutando este comando desde un equipo remoto. El **UseSSL** parámetro especifica el protocolo de capa de Sockets seguros (SSL) que se usa para establecer una conexión con el equipo remoto. De forma predeterminada, no se usa SSL.|

Después de completar la configuración de la aplicación de la controladora de red, la implementación de controladora de red está completa.

## <a name="network-controller-deployment-validation"></a>Validación de la implementación de controladora de red

Para validar la implementación de controladora de red, puede agregar una credencial a la controladora de red y, a continuación, recuperar la credencial.

Si está usando Kerberos como el mecanismo de ClientAuthentication pertenencia en el **ClientSecurityGroup** que ha creado es el requisito mínimo para llevar a cabo este procedimiento.

**Procedimiento:**

1.  En un equipo cliente, si está usando Kerberos como el mecanismo de ClientAuthentication, inicie sesión con una cuenta de usuario que sea miembro de su **ClientSecurityGroup**.

2. Abra Windows PowerShell, escriba los siguientes comandos para agregar una credencial para la controladora de red y, a continuación, presione ENTRAR. Asegúrese de agregar valores para cada parámetro que son adecuados para su implementación.

    ```
    $cred=New-Object Microsoft.Windows.Networkcontroller.credentialproperties
    $cred.type="usernamepassword"
    $cred.username="admin"
    $cred.value="abcd"

    New-NetworkControllerCredential -ConnectionUri https://networkcontroller -Properties $cred -ResourceId cred1
    ```

3. Para recuperar la credencial que ha agregado a la controladora de red, escriba el siguiente comando y, a continuación, presione ENTRAR. Asegúrese de agregar valores para cada parámetro que son adecuados para su implementación.

    ```
    Get-NetworkControllerCredential -ConnectionUri https://networkcontroller -ResourceId cred1  
    ```

4. Revise el comando generado, que debe ser similar a la salida del ejemplo siguiente.

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
    > Al ejecutar el **Get NetworkControllerCredential** de comandos, puede asignar la salida del comando a una variable mediante el operador punto para mostrar las propiedades de las credenciales. Por ejemplo, $cred. Propiedades.

## <a name="additional-windows-powershell-commands-for-network-controller"></a>Comandos de Windows PowerShell adicionales para la controladora de red

Después de implementar la controladora de red, puede usar comandos de Windows PowerShell para administrar y modificar la implementación. Estas son algunas de los cambios que puede realizar en la implementación.

- Modificar configuración de la aplicación, clúster y nodo de controladora de red

- Quitar el clúster de la controladora de red y la aplicación

- Administrar los nodos del clúster de controladora de red, como agregar, quitar, habilitar y deshabilitar los nodos.

En la tabla siguiente proporciona que comandos de la sintaxis de Windows PowerShell que puede usar para realizar estas tareas.

|Tarea|Comando|Sintaxis|
|--------|-------|----------|
|Modificar la configuración de clúster de controladora de red|Set-NetworkControllerCluster|`Set-NetworkControllerCluster [-ManagementSecurityGroup <string>][-Credential <PSCredential>] [-computerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|Modificar la configuración de la aplicación controladora de red|Set-NetworkController|`Set-NetworkController [-ClientAuthentication <ClientAuthentication>] [-Credential <PSCredential>] [-ClientCertificateThumbprint <string[]>] [-ClientSecurityGroup <string>] [-ServerCertificate <X509Certificate2>] [-RestIPAddress <String>] [-ComputerName <String>][-CertificateThumbprint <String> ] [-UseSSL]`
|Modificar la configuración de nodo de controladora de red|Set-NetworkControllerNode|`Set-NetworkControllerNode -Name <string> > [-RestInterface <string>] [-NodeCertificate <X509Certificate2>] [-Credential <PSCredential>] [-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|Modificar la configuración de diagnóstico de controladora de red|Set-NetworkControllerDiagnostic|`Set-NetworkControllerDiagnostic [-LogScope <string>] [-DiagnosticLogLocation <string>] [-LogLocationCredential <PSCredential>] [-UseLocalLogLocation] >] [-LogLevel <loglevel>][-LogSizeLimitInMBs <uint32>] [-LogTimeLimitInDays <uint32>] [-Credential <PSCredential>] [-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|Quitar la aplicación de la controladora de red|Uninstall-NetworkController|`Uninstall-NetworkController [-Credential <PSCredential>][-ComputerName <string>] [-CertificateThumbprint <String> ] [-UseSSL]`
|Quitar el clúster de controladora de red|Uninstall-NetworkControllerCluster|`Uninstall-NetworkControllerCluster [-Credential <PSCredential>][-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|Agregar un nodo al clúster de controladora de red|Add-NetworkControllerNode|`Add-NetworkControllerNode -FaultDomain <String> -Name <String> -RestInterface <String> -Server <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-Force] [-NodeCertificate <X509Certificate2> ] [-PassThru] [-UseSsl]`
|Deshabilitar un nodo de clúster de la controladora de red|Disable-NetworkControllerNode|`Disable-NetworkControllerNode -Name <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-PassThru] [-UseSsl]`
|Permitir que un nodo de clúster de la controladora de red|Enable-NetworkControllerNode|`Enable-NetworkControllerNode -Name <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-PassThru] [-UseSsl]`
|Quitar un nodo de la controladora de red de un clúster|Remove-NetworkControllerNode|`Remove-NetworkControllerNode [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-Force] [-Name <String> ] [-PassThru] [-UseSsl]`

>[!NOTE]
>Comandos de Windows PowerShell de controladora de red están en la biblioteca de TechNet en [Cmdlets de controlador de red](https://technet.microsoft.com/library/mt576401.aspx).

## <a name="sample-network-controller-configuration-script"></a>Script de configuración de controladora de red de ejemplo

El siguiente script de configuración de ejemplo muestra cómo crear un clúster de la controladora de red de varios nodo e instalar la aplicación de la controladora de red. Además, la variable $cert selecciona un certificado del almacén de certificados equipo local que coincide con la cadena de nombre de asunto "networkController.contoso.com".

```
$a = New-NetworkControllerNodeObject -Name Node1 -Server NCNode1.contoso.com -FaultDomain fd:/rack1/host1 -RestInterface Internal
$b = New-NetworkControllerNodeObject -Name Node2 -Server NCNode2.contoso.com -FaultDomain fd:/rack1/host2 -RestInterface Internal
$c = New-NetworkControllerNodeObject -Name Node3 -Server NCNode3.contoso.com -FaultDomain fd:/rack1/host3 -RestInterface Internal

$cert= get-item Cert:\LocalMachine\My | get-ChildItem | where {$_.Subject -imatch "networkController.contoso.com" }

Install-NetworkControllerCluster -Node @($a,$b,$c)  -ClusterAuthentication Kerberos -DiagnosticLogLocation \\share\Diagnostics - ManagementSecurityGroup Contoso\NCManagementAdmins -CredentialEncryptionCertificate $cert  
Install-NetworkController -Node @($a,$b,$c) -ClientAuthentication Kerberos -ClientSecurityGroup Contoso\NCRESTClients -ServerCertificate $cert -RestIpAddress 10.0.0.1/24
```

## <a name="post-deployment-steps-for-non-kerberos-deployments"></a>Pasos posteriores a la implementación para las implementaciones que no sea de Kerberos

Si no está usando Kerberos con la implementación de controladora de red, debe implementar certificados.

Para obtener más información, consulte [pasos posteriores a la implementación de controladora de red](../technologies/network-controller/post-deploy-steps-nc.md).


