---
title: Instalar HGS en un bosque bastión existente
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 147610d9dcb36dfedab3aca11ee1a64731715519
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842226"
---
# <a name="install-hgs-in-an-existing-bastion-forest"></a>Instalar HGS en un bosque bastión existente 

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016


## <a name="join-the-hgs-server-to-the-existing-domain"></a>Unir el servidor HGS al dominio existente

En un bosque bastión existente, debe agregarse HGS para el dominio raíz. Use el Administrador de servidor o [Add-Computer](https://go.microsoft.com/fwlink/?LinkId=821564) para unir el servidor HGS para el dominio raíz.

## <a name="add-the-hgs-server-role"></a>Agregar el rol de servidor HGS

Ejecutar todos los comandos de este tema en una sesión de PowerShell con privilegios elevados.

[!INCLUDE [Install the HGS server role](../../../includes/guarded-fabric-install-hgs-server-role.md)] 

Si el centro de datos tiene un bosque bastión seguro donde desea que se unan los nodos HGS, siga estos pasos.
También puede utilizar estos pasos para configurar clústeres HGS de 2 o más independientes que están unidos al mismo dominio.

## <a name="join-the-hgs-server-to-the-existing-domain"></a>Unir el servidor HGS al dominio existente

Use el Administrador de servidor o [Add-Computer](https://go.microsoft.com/fwlink/?LinkId=821564) para unir los servidores HGS para el dominio que desee.

## <a name="prepare-active-directory-objects"></a>Preparar los objetos de Active Directory

Crear una cuenta de servicio administrada de grupo y 2 grupos de seguridad.
También puede ensayar previamente los objetos de clúster si la cuenta con que se está inicializando HGS no tiene permiso para crear objetos de equipo en el dominio.

## <a name="group-managed-service-account"></a>Cuenta de servicio administrada de grupo

La cuenta de servicio administrada de grupo (gMSA) es la identidad con HGS para recuperar y usar sus certificados. Use [New-ADServiceAccount](https://technet.microsoft.com/itpro/powershell/windows/addsadministration/new-adserviceaccount) para crear una gMSA.
Si se trata de la primera gMSA en el dominio, deberá agregar una clave de raíz del servicio de distribución de claves.

Cada nodo HGS deberá tener acceso a la contraseña de gMSA.
La manera más fácil de configurar esto consiste en crear un grupo de seguridad que contiene todos los nodos HGS y conceder ese acceso al grupo de seguridad para recuperar la contraseña de gMSA.

Debe reiniciar el servidor HGS después de agregarlo a un grupo de seguridad para asegurarse de que obtiene su pertenencia nueva.

```powershell
# Check if the KDS root key has been set up
if (-not (Get-KdsRootKey)) {
    # Adds a KDS root key effective immediately (ignores normal 10 hour waiting period)
    Add-KdsRootKey -EffectiveTime ((Get-Date).AddHours(-10))
}

# Create a security group for HGS nodes
$hgsNodes = New-ADGroup -Name 'HgsServers' -GroupScope DomainLocal -PassThru

# Add your HGS nodes to this group
# If your HGS server object is under an organizational unit, provide the full distinguished name instead of "HGS01"
Add-ADGroupMember -Identity $hgsNodes -Members "HGS01"

# Create the gMSA
New-ADServiceAccount -Name 'HGSgMSA' -DnsHostName 'HGSgMSA.yourdomain.com' -PrincipalsAllowedToRetrieveManagedPassword $hgsNodes
```

La gMSA requerirá la derecha para generar eventos en el registro de seguridad en cada servidor HGS.
Si utiliza Directiva de grupo para configurar la asignación de derechos de usuario, asegúrese de que la cuenta de gMSA se le conceden el [generar privilegios de los eventos de auditoría](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn221956%28v=ws.11%29) en los servidores HGS.

> [!NOTE]
> Las cuentas de servicio administradas de grupo están disponibles desde el esquema de Active Directory de Windows Server 2012.
> Para obtener más información, consulte [requisitos de la cuenta de servicio administrada de grupo](https://technet.microsoft.com/library/jj128431.aspx).

## <a name="jea-security-groups"></a>Grupos de seguridad JEA

Al configurar HGS, un [Just Enough Administration (JEA)](https://aka.ms/JEAdocs) punto de conexión de PowerShell está configurado para permitir que los administradores a administrar HGS sin necesidad de privilegios completos de administrador local.
No es necesario para usar JEA para administrar HGS, pero aún debe configurarse al ejecutar Initialize HgsServer.
La configuración del punto de conexión JEA consta de designar 2 grupos de seguridad que contienen su HGS administradores y los revisores HGS.
Pueden agregar, cambiar o quitar directivas de HGS; los usuarios que pertenecen al grupo de administradores los revisores solo pueden ver la configuración actual.

Cree 2 grupos de seguridad para estos grupos JEA mediante herramientas de administración de Active Directory o [New-ADGroup](https://technet.microsoft.com/itpro/powershell/windows/addsadministration/new-adgroup).

```powershell
New-ADGroup -Name 'HgsJeaReviewers' -GroupScope DomainLocal
New-ADGroup -Name 'HgsJeaAdmins' -GroupScope DomainLocal
```

## <a name="cluster-objects"></a>Objetos de clúster

Si la cuenta que usas para configurar HGS no tiene permiso para crear nuevos objetos de equipo en el dominio, deberá ensayo previo de los objetos de clúster.
Estos pasos se explican en [Prestage Cluster Computer Objects in Active Directory Domain Services](https://technet.microsoft.com/library/dn466519(v=ws.11).aspx).

Para configurar el primer nodo HGS, deberá crear un objeto de nombre de clúster (CNO) y un objeto de equipo Virtual (VCO).
El CNO representa el nombre del clúster y se usa principalmente mediante la agrupación en clústeres de conmutación por error.
La VCO representa el servicio HGS que reside en la parte superior del clúster y será el nombre registrado con el servidor DNS.

> [!IMPORTANT]
> El usuario que se ejecutará `Initialize-HgsServer` requiere **Control total** a través de los objetos CNO y VCO en Active Directory.

Para preconfigurar el CNO y VCO el rápidamente, tienen un administrador de Active Directory ejecute los siguientes comandos de PowerShell:

```powershell
# Create the CNO
$cno = New-ADComputer -Name 'HgsCluster' -Description 'HGS CNO' -Enabled $false -Passthru

# Create the VCO
$vco = New-ADComputer -Name 'HgsService' -Description 'HGS VCO' -Passthru

# Give the CNO full control over the VCO
$vcoPath = Join-Path "AD:\" $vco.DistinguishedName
$acl = Get-Acl $vcoPath
$ace = New-Object System.DirectoryServices.ActiveDirectoryAccessRule $cno.SID, "GenericAll", "Allow"
$acl.AddAccessRule($ace)
Set-Acl -Path $vcoPath -AclObject $acl

# Allow time for your new CNO and VCO to replicate to your other Domain Controllers before continuing
```

## <a name="security-baseline-exceptions"></a>Excepciones de la línea de base de seguridad

Si está implementando en un entorno altamente bloqueado HGS, ciertas opciones de directiva de grupo pueden impedir HGS funcionan con normalidad.
Compruebe los objetos de directiva de grupo para las siguientes opciones y siga las instrucciones si se ve afectado:

### <a name="network-logon"></a>Inicio de sesión de red

**Ruta de acceso de la directiva:** Equipo equipo\Configuración Windows\Configuración configuración de seguridad\Directivas locales\Asignación de derechos de asignaciones

**Nombre de la directiva:** Denegar el acceso desde la red a este equipo

**Valor requerido:** Asegúrese de que el valor no bloquea los inicios de sesión de red para todas las cuentas locales. Puede bloquear con seguridad las cuentas de administrador local, sin embargo.

**Motivo:** Agrupación en clústeres de conmutación por error se basa en una cuenta local sin privilegios de administrador denominada CLIUSR para administrar los nodos del clúster. Bloqueo de inicio de sesión de red para este usuario impedirá que el clúster funciona correctamente.

### <a name="kerberos-encryption"></a>Cifrado de Kerberos

**Ruta de acceso de la directiva:** Configuración del equipo\Configuración de Windows\Configuración de seguridad\Directivas locales\Opciones de seguridad

**Nombre de la directiva:** Seguridad de red: Configurar tipos de cifrado permitidos para Kerberos

**Acción**: Si se configura esta directiva, debe actualizar la cuenta de gMSA con [Set-ADServiceAccount](https://docs.microsoft.com/powershell/module/addsadministration/set-adserviceaccount?view=win10-ps) para usar solo los tipos de cifrado admitidos en esta directiva. Por ejemplo, si la directiva solo permite AES128\_HMAC\_SHA1 y AES256\_HMAC\_SHA1, debe ejecutar `Set-ADServiceAccount -Identity HGSgMSA -KerberosEncryptionType AES128,AES256`.



## <a name="next-steps"></a>Pasos siguientes

- Para que obtener los pasos siguientes configurar la atestación de TPM, consulte [inicializar el clúster HGS utilizando el modo TPM en un bosque bastión existente](guarded-fabric-initialize-hgs-tpm-mode-bastion.md).
- Para que obtener los pasos siguientes configurar la atestación de clave de host, consulte [inicializar el clúster HGS utilizando el modo de clave en un bosque bastión existente](guarded-fabric-initialize-hgs-key-mode-bastion.md).
- Durante los próximos pasos para configurar la atestación basada en Administrador (en desuso en Windows Server 2019), consulte [inicializar el clúster HGS utilizando el modo de AD en un bosque bastión existente](guarded-fabric-initialize-hgs-ad-mode-bastion.md).

