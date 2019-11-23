---
title: Instalación de HGS en un bosque bastión existente
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 5c503331893dbea65c99d79eb7444893d5f3b657
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403599"
---
# <a name="install-hgs-in-an-existing-bastion-forest"></a>Instalación de HGS en un bosque bastión existente 

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016


## <a name="join-the-hgs-server-to-the-existing-domain"></a>Unir el servidor de HGS al dominio existente

En un bosque bastión existente, se debe agregar HGS al dominio raíz. Use Administrador del servidor o [Add-Computer](https://go.microsoft.com/fwlink/?LinkId=821564) para unir el servidor de HGS al dominio raíz.

## <a name="add-the-hgs-server-role"></a>Agregar el rol de servidor HGS

Ejecute todos los comandos de este tema en una sesión de PowerShell con privilegios elevados.

[!INCLUDE [Install the HGS server role](../../../includes/guarded-fabric-install-hgs-server-role.md)] 

Si el centro de seguridad tiene un bosque bastión seguro al que desea unir nodos HGS, siga estos pasos.
También puede usar estos pasos para configurar 2 o más clústeres de HGS independientes que están Unidos al mismo dominio.

## <a name="join-the-hgs-server-to-the-existing-domain"></a>Unir el servidor de HGS al dominio existente

Use Administrador del servidor o [Add-Computer](https://go.microsoft.com/fwlink/?LinkId=821564) para unir los servidores HGS al dominio deseado.

## <a name="prepare-active-directory-objects"></a>Preparar objetos Active Directory

Cree una cuenta de servicio administrada de grupo y dos grupos de seguridad.
También puede preconfigurar los objetos de clúster si la cuenta con la que está inicializando HGS no tiene permiso para crear objetos de equipo en el dominio.

## <a name="group-managed-service-account"></a>Cuenta de servicio administrada de grupo

La cuenta de servicio administrada de grupo (gMSA) es la identidad que usa HGS para recuperar y usar sus certificados. Use [New-ADServiceAccount](https://technet.microsoft.com/itpro/powershell/windows/addsadministration/new-adserviceaccount) para crear un gMSA.
Si esta es la primera gMSA del dominio, deberá agregar una clave raíz del servicio de distribución de claves.

Cada nodo de HGS tendrá que permitir el acceso a la contraseña de gMSA.
La forma más fácil de configurar esto es crear un grupo de seguridad que contenga todos los nodos de HGS y conceder a ese grupo de seguridad acceso para recuperar la contraseña de gMSA.

Debe reiniciar el servidor HGS después de agregarlo a un grupo de seguridad para asegurarse de que obtiene la nueva pertenencia a grupos.

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

El gMSA requerirá el derecho a generar eventos en el registro de seguridad de cada servidor de HGS.
Si usa directiva de grupo para configurar la asignación de derechos de usuario, asegúrese de que la cuenta gMSA tenga el [privilegio generar eventos de auditoría](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn221956%28v=ws.11%29) en los servidores HGS.

> [!NOTE]
> Las cuentas de servicio administradas de grupo están disponibles a partir del esquema de Active Directory de Windows Server 2012.
> Para obtener más información, consulte requisitos de la [cuenta de servicio administrada de grupo](https://technet.microsoft.com/library/jj128431.aspx).

## <a name="jea-security-groups"></a>Grupos de seguridad de JEA

Al configurar HGS, se configura un punto de conexión de PowerShell de [Administración suficiente (jea)](https://aka.ms/JEAdocs) para permitir que los administradores administren HGS sin necesidad de privilegios de administrador local completos.
No es necesario usar JEA para administrar HGS, pero todavía debe configurarse al ejecutar Initialize-HgsServer.
La configuración del punto de conexión de JEA consiste en designar dos grupos de seguridad que contienen los administradores de HGS y los revisores de HGS.
Los usuarios que pertenecen al grupo de administración pueden agregar, cambiar o quitar directivas en HGS; los revisores solo pueden ver la configuración actual.

Cree dos grupos de seguridad para estos grupos de JEA mediante Active Directory herramientas de administración o [New-AdGroup](https://technet.microsoft.com/itpro/powershell/windows/addsadministration/new-adgroup).

```powershell
New-ADGroup -Name 'HgsJeaReviewers' -GroupScope DomainLocal
New-ADGroup -Name 'HgsJeaAdmins' -GroupScope DomainLocal
```

## <a name="cluster-objects"></a>Objetos de clúster

Si la cuenta que usa para configurar HGS no tiene permiso para crear nuevos objetos de equipo en el dominio, deberá preconfigurar los objetos de clúster.
Estos pasos se explican en los [objetos de equipo de clúster preconfigurados en Active Directory Domain Services](https://technet.microsoft.com/library/dn466519(v=ws.11).aspx).

Para configurar el primer nodo de HGS, tendrá que crear un objeto de nombre de clúster (CNO) y un objeto de equipo virtual (VCO).
El CNO representa el nombre del clúster y se usa principalmente internamente en los clústeres de conmutación por error.
La VCO representa el servicio HGS que se encuentra en la parte superior del clúster y será el nombre registrado con el servidor DNS.

> [!IMPORTANT]
> El usuario que va a ejecutar `Initialize-HgsServer` requiere un **control total** sobre los objetos CNO y VCO en Active Directory.

Para preconfigurar rápidamente el CNO y el VCO, haga que un administrador de Active Directory ejecute los siguientes comandos de PowerShell:

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

## <a name="security-baseline-exceptions"></a>Excepciones de línea base de seguridad

Si va a implementar HGS en un entorno de alta disponibilidad, ciertos valores de directiva de grupo pueden impedir que HGS funcione con normalidad.
Compruebe los objetos de directiva de grupo para ver la configuración siguiente y siga las instrucciones si se ve afectado:

### <a name="network-logon"></a>Inicio de sesión de red

**Ruta de acceso de directiva:** Configuración del equipo \ configuración de seguridad\Directivas locales \ asignaciones de derechos

**Nombre de la Directiva:** Denegar el acceso a este equipo desde la red

**Valor obligatorio:** Asegúrese de que el valor no bloquea los inicios de sesión de red para todas las cuentas locales. Sin embargo, puede bloquear de forma segura las cuentas de administrador local.

**Motivo:** Los clústeres de conmutación por error se basan en una cuenta local que no es de administrador denominada CLIUSR para administrar los nodos de clúster. Si se bloquea el inicio de sesión de red para este usuario, impedirá que el clúster funcione correctamente.

### <a name="kerberos-encryption"></a>Cifrado Kerberos

**Ruta de acceso de directiva:** Configuración del equipo \ configuración de seguridad\Directivas Locales\opciones de opciones

**Nombre de la Directiva:** Seguridad de red: configurar tipos de cifrado permitidos para Kerberos

**Acción**: si se configura esta Directiva, debe actualizar la cuenta de GMSA con [set-ADServiceAccount](https://docs.microsoft.com/powershell/module/addsadministration/set-adserviceaccount?view=win10-ps) para usar solo los tipos de cifrado admitidos en esta Directiva. Por ejemplo, si la Directiva solo permite AES128\_HMAC\_SHA1 y AES256\_HMAC\_SHA1, debe ejecutar `Set-ADServiceAccount -Identity HGSgMSA -KerberosEncryptionType AES128,AES256`.



## <a name="next-steps"></a>Pasos siguientes

- Para conocer los pasos siguientes para configurar la atestación basada en TPM, consulte [inicializar el clúster de HGS mediante el modo TPM en un bosque bastión existente](guarded-fabric-initialize-hgs-tpm-mode-bastion.md).
- Para conocer los pasos siguientes para configurar la atestación de clave de host, consulte [inicializar el clúster de HGS mediante el modo de clave en un bosque bastión existente](guarded-fabric-initialize-hgs-key-mode-bastion.md).
- Para conocer los pasos siguientes para configurar la atestación basada en el administrador (en desuso en Windows Server 2019), consulte [inicializar el clúster HGS mediante el modo ad en un bosque bastión existente](guarded-fabric-initialize-hgs-ad-mode-bastion.md).

