---
title: Migración de DirectAccess para VPN de Always On
description: Migración de DirectAccess a VPN de Always On, requiere un proceso específico para migrar a los clientes, lo que ayuda a minimiza las condiciones de carrera que surgen al realizar los pasos de migración fuera de servicio.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: pashort
author: shortpatti
ms.date: 06/07/2018
ms.openlocfilehash: 94852b33936ece0b329614f428e845f2c7a2ac6a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854586"
---
# <a name="migrate-to-always-on-vpn-and-decommission-directaccess"></a>Migrar a la VPN de Always On y retirar DirectAccess

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows 10

&#171;[ **Anterior:** Planear el cliente de DirectAccess para la migración de VPN de Always On](da-always-on-migration-planning.md)<br>

Migración de DirectAccess a VPN de Always On, requiere un proceso específico para migrar a los clientes, lo que ayuda a minimiza las condiciones de carrera que surgen al realizar los pasos de migración fuera de servicio. En un nivel alto, el proceso de migración consta de estos cuatro pasos principales:

1.  **Implementar una infraestructura VPN en paralelo.** Después de determinar las fases de migración y las características que desea incluir en la implementación, se implementará la infraestructura de VPN en paralelo con la infraestructura de DirectAccess existente.  

2.  **Implementar certificados y la configuración a los clientes.**  Una vez que la infraestructura de VPN está lista, puede crea y publicar los certificados necesarios al cliente. Cuando los clientes han recibido los certificados, implemente el script de configuración VPN_Profile.ps1. Como alternativa, puede usar Intune para configurar al cliente VPN. Utilice Microsoft System Center Configuration Manager o Microsoft Intune para supervisar implementaciones correctas de configuración de VPN.

3.  [!INCLUDE [remove-da-security-group-shortdesc-include](../includes/remove-da-security-group-shortdesc-include.md)]

4.  [!INCLUDE [decommission-da-shortdesc-include](../includes/decommission-da-shortdesc-include.md)]

Antes de comenzar el proceso de migración de DirectAccess a VPN de Always On, asegúrese de que ha planeado para la migración.  Si no ha planeado la migración, consulte [planear el cliente de DirectAccess para la migración de VPN de Always On](da-always-on-migration-planning.md).

>[!TIP] 
>En esta sección no es una guía de implementación paso a paso para VPN de Always On, pero en su lugar, está diseñada para complementar [siempre en VPN de implementación para Windows Server y Windows 10](../vpn/always-on-vpn/deploy/always-on-vpn-deploy.md) y ofrecen orientación de implementación específico de la migración.

## <a name="deploy-a-side-by-side-vpn-infrastructure"></a>Implementar una infraestructura VPN en paralelo

Implementar la infraestructura de VPN en paralelo con la infraestructura de DirectAccess existente.  Para obtener información detallada paso a paso, consulte [siempre en VPN de implementación para Windows Server y Windows 10](../vpn/always-on-vpn/deploy/always-on-vpn-deploy.md) para instalar y configurar la infraestructura de VPN de Always On. 

Implementación en paralelo se compone de las siguientes tareas de alto nivel:

1.  Cree los grupos de usuarios de VPN, servidores VPN y los servidores NPS.

2.  Crear y publicar las plantillas de certificado es necesario.

3.  Inscriba los certificados de servidor.

4.  Instale y configure el servicio de acceso remoto de VPN de Always On.

5.  Instalar y configurar NPS.

6.  Configurar DNS y reglas de firewall para VPN de Always On.

La imagen siguiente proporciona una referencia visual para que los cambios de infraestructura a lo largo de DirectAccess-a-siempre los sobre la migración de VPN.

![](../../media/DA-to-AlwaysOnVPN/6b64f322f945f837f22a32bf87a228f8.png)

## <a name="deploy-certificates-and-vpn-configuration-script-to-the-clients"></a>Implementar certificados y script de configuración de VPN a los clientes

Aunque la mayor parte de la configuración de cliente VPN en el [implementar VPN de Always On](../vpn/always-on-vpn/deploy/always-on-vpn-deploy-deployment.md) sección, aparecerán dos pasos son necesarios para completar correctamente la migración de DirectAccess a VPN de Always On. 

Debe asegurarse de que el **VPN_Profile.ps1** viene _después_ se ha emitido el certificado para que el cliente VPN no intenta conectarse sin él. Para ello, ejecute un script que agrega solo aquellos usuarios que han inscrito en el certificado al grupo de implementación de VPN listo, que se usa para implementar la configuración de VPN de Always On.

>[!NOTE] 
>Microsoft recomienda que pruebe este proceso antes de ejecutarla en cualquiera de los anillos de migración de usuario.

1.  **Crear y publicar el certificado VPN y habilitar el objeto de directiva de grupo (GPO) de la inscripción automática.** Para las implementaciones de VPN de Windows 10 tradicionales, basada en certificados, se emite un certificado para el dispositivo o usuario para que se pueda autenticar la conexión. Cuando se crea el nuevo certificado de autenticación y se publica para la inscripción automática, debe crear e implementar un GPO con la opción de inscripción automática configurada para el grupo de usuarios de VPN. Para que conocer los pasos configurar certificados y la inscripción automática, consulte [configurar la infraestructura de servidor](../vpn/always-on-vpn/deploy/vpn-deploy-server-infrastructure.md).

2.  **Agregar usuarios al grupo usuarios de VPN.** Agregue cualquier usuarios que migre al grupo usuarios de VPN. Los usuarios permanecerán en ese grupo de seguridad después de haber migrado para que puedan recibir las actualizaciones del certificado en el futuro. Continuar agregando usuarios a este grupo hasta que se han movido todos los usuarios de DirectAccess para VPN de Always On. 

3.  **Identificar a los usuarios que han recibido un certificado de autenticación de VPN.** Va a migrar de DirectAccess, por lo que necesitará agregar un método para identificar cuando un cliente ha recibido el certificado necesario y está listo para recibir la información de configuración de VPN. Ejecute el **GetUsersWithCert.ps1** secuencia de comandos para agregar los usuarios que actualmente se emiten certificados nonrevoked que se origina en el nombre de la plantilla especificada a un grupo de seguridad de AD DS especificado. Por ejemplo, después de ejecutar el **GetUsersWithCert.ps1** secuencia de comandos, cualquier usuario emite un certificado válido de plantilla se agrega al grupo de implementación de VPN listo el certificado de autenticación de VPN.

    >[!NOTE] 
    >Si no tiene un método para identificar cuando un cliente ha recibido el certificado necesario, podría implementar la configuración de VPN antes de que se ha emitido el certificado para el usuario, provocando errores de conexión VPN. Para evitar esta situación, ejecute el **GetUsersWithCert.ps1** script en la entidad de certificación o según una programación para sincronizar los usuarios que han recibido el certificado para el grupo de implementación de VPN listo. A continuación, usará ese grupo de seguridad para la implementación de la configuración de VPN en System Center Configuration Manager o Intune, lo que garantiza que el cliente administrado no recibe la configuración de VPN antes de haber recibido el certificado de destino.
    
    ### <a name="getuserswithcertps1"></a>GetUsersWithCert.ps1
    
    ```powershell
    Import-module ActiveDirectory
    Import-Module AdcsAdministration
    
    $TemplateName = 'VPNUserAuthentication'##Certificate Template Name (not the friendly name)
    $GroupName = 'VPN Deployment Ready' ##Group you will add the users to
    $CSServerName = 'localhost\corp-dc-ca' ##CA Server Information
    
    $users= @()
    $TemplateID = (get-CATemplate | Where-Object {$_.Name -like $TemplateName} | Select-Object oid).oid
    $View = New-Object -ComObject CertificateAuthority.View
    $NULL = $View.OpenConnection($CSServerName)
    $View.SetResultColumnCount(3)
    $i1 = $View.GetColumnIndex($false, "User Principal Name")
    $i2 = $View.GetColumnIndex($false, "Certificate Template")
    $i3 = $View.GetColumnIndex($false, "Revocation Date")
    $i1, $i2, $i3 | %{$View.SetResultColumn($_) }
    $Row= $View.OpenView()
    
    while ($Row.Next() -ne -1){
    $Cert = New-Object PsObject
    $Col = $Row.EnumCertViewColumn()
    [void]$Col.Next()
    do {
    $Cert | Add-Member -MemberType NoteProperty $($Col.GetDisplayName()) -Value $($Col.GetValue(1)) -Force
          }
    until ($Col.Next() -eq -1)
    $col = ''
    
    if($cert."Certificate Template" -eq $TemplateID -and $cert."Revocation Date" -eq $NULL){
       $users= $users+= $cert."User Principal Name"
    $temp = $cert."User Principal Name"
    $user = get-aduser -Filter {UserPrincipalName -eq $temp} –Property UserPrincipalName
    Add-ADGroupMember $GroupName $user
       }
      }
    ```

4. Implementar la configuración de VPN de Always On. Como la VPN se emiten los certificados de autenticación y ejecutar el **GetUsersWithCert.ps1** secuencia de comandos, los usuarios se agregan al grupo de seguridad de implementación de VPN listo.


| Si usas...  | En ese caso... |
| ---- | ---- |
| System Center Configuration Manager | Cree una recopilación de usuarios según la pertenencia a de ese grupo seguridad.<br><br>![](../../media/DA-to-AlwaysOnVPN/b38723b3ffcfacd697b83dd41a177f66.png)!|
| Intune | Basta con tener como destino el grupo de seguridad directamente una vez que se sincronice. |
|
    
Cada vez que ejecute la **GetUsersWithCert.ps1** script de configuración, también debe ejecutar una regla de detección de AD DS para actualizar la pertenencia al grupo de seguridad en System Center Configuration Manager. Además, asegúrese de que la actualización de pertenencia para la recopilación de implementación suele suceder lo suficientemente (alineado con la regla de detección y la secuencia de comandos).

Para obtener más información sobre el uso de System Center Configuration Manager o Intune para implementar la VPN de Always On en los clientes de Windows, consulte [siempre en VPN de implementación para Windows Server y Windows 10](../vpn/always-on-vpn/deploy/always-on-vpn-deploy.md). Asegúrese, sin embargo, para incorporar estas tareas específicas de la migración.

>[!NOTE] 
>Incorporación de estas tareas específicas de la migración es una diferencia fundamental entre una implementación simple de VPN de Always On y la migración de DirectAccess para VPN de Always On. Asegúrese de definir correctamente la colección para el grupo de seguridad en lugar de utilizar el método en la Guía de implementación de destino.


## <a name="remove-devices-from-the-directaccess-security-group"></a>Quitar dispositivos del grupo de seguridad de DirectAccess

Como los usuarios reciben el certificado de autenticación y la **VPN_Profile.ps1** script de configuración, consulte correspondientes implementaciones correctas de script de configuración de VPN en System Center Configuration Manager o Intune. Después de cada implementación, quite los dispositivos del usuario desde el grupo de seguridad de DirectAccess para que más adelante puede quitar DirectAccess. Intune y System Center Configuration Manager contiene información de asignación de dispositivo de usuario para ayudarle a determinar el dispositivo de cada usuario.

>[!NOTE] 
>Si va a aplicar los GPO de DirectAccess a través de unidades organizativas (OU) en lugar de grupos de equipos, mueva el objeto de equipo del usuario fuera de la unidad organizativa.

## <a name="decommission-the-directaccess-infrastructure"></a>Retirar la infraestructura de DirectAccess

Cuando haya terminado de migrar a todos los clientes de DirectAccess a VPN de Always On, puede retirar la infraestructura de DirectAccess y quitar la configuración de DirectAccess de la directiva de grupo. Microsoft recomienda realizar los pasos siguientes para quitar correctamente el entorno de DirectAccess:

1.  [!INCLUDE [remove-config-settings-shortdesc-include](../includes/remove-config-settings-shortdesc-include.md)]

    ![](../../media/DA-to-AlwaysOnVPN/dbdc3d80e8dc1b8665f7b15d7d2ee1f6.png)

2.  [!INCLUDE [remove-da-security-group-shortdesc-include](../includes/remove-da-security-group-shortdesc-include.md)]

3.  **Limpiar DNS.** Asegúrese de quitar todos los registros de servidor DNS interno y servidor DNS público relacionado con DirectAccess, por ejemplo, DA.contoso.com, DAGateway.contoso.com.

4.  [!INCLUDE [decommission-da-shortdesc-include](../includes/decommission-da-shortdesc-include.md)]

5.  **Quite los certificados de DirectAccess de servicios de certificados de Active Directory.** Si utiliza certificados de equipo para la implementación de DirectAccess, quite las plantillas publicadas desde la carpeta de plantillas de certificado en la consola de entidad de certificación.

## <a name="next-step"></a>Paso siguiente
Termine la migración de DirectAccess a VPN de Always On. 

---