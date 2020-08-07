---
title: Migración de DirectAccess a Always On VPN
description: La migración de DirectAccess a Always On VPN requiere un proceso específico para migrar los clientes, lo que ayuda a minimizar las condiciones de carrera que surgen al seguir los pasos de migración sin orden.
manager: dougkim
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: lizross
author: eross-msft
ms.date: 06/07/2018
ms.openlocfilehash: a452c9ab1a24304a9acfec8357bc98a3d058e03c
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87971752"
---
# <a name="migrate-to-always-on-vpn-and-decommission-directaccess"></a>Migrar a Always On VPN y retirar DirectAccess

>Se aplica a: Windows Server (canal semianual), Windows Server 2016 y Windows 10

&#171; [ **anterior:** planear la migración de DIRECTACCESS a Always on VPN](da-always-on-migration-planning.md)<br>

La migración de DirectAccess a Always On VPN requiere un proceso específico para migrar los clientes, lo que ayuda a minimizar las condiciones de carrera que surgen al seguir los pasos de migración sin orden. En un nivel alto, el proceso de migración consta de estos cuatro pasos principales:

1.  **Implementar una infraestructura de VPN en paralelo.** Una vez que haya determinado las fases de migración y las características que desea incluir en la implementación, implementará la infraestructura de VPN en paralelo con la infraestructura de DirectAccess existente.

2.  **Implementar certificados y configuración en los clientes.**  Una vez que la infraestructura de VPN esté lista, cree y publique los certificados necesarios en el cliente. Cuando los clientes hayan recibido los certificados, implemente el script de configuración de VPN_Profile.ps1. Como alternativa, puede usar Intune para configurar el cliente VPN. Use el punto de conexión de Microsoft Configuration Manager o Microsoft Intune para supervisar las implementaciones correctas de configuración de VPN.

3.  [!INCLUDE [remove-da-security-group-shortdesc-include](../includes/remove-da-security-group-shortdesc-include.md)]

4.  [!INCLUDE [decommission-da-shortdesc-include](../includes/decommission-da-shortdesc-include.md)]

Antes de iniciar el proceso de migración de DirectAccess a Always On VPN, asegúrese de que ha planeado la migración.  Si no ha planeado la migración, consulte [planear la migración de DirectAccess a Always on VPN](da-always-on-migration-planning.md).

>[!TIP]
>Esta sección no es una guía de implementación paso a paso para Always On VPN, sino que está diseñada para complementar [Always on implementación de VPN para Windows Server y Windows 10](../vpn/always-on-vpn/deploy/always-on-vpn-deploy.md) , y para proporcionar una guía de implementación específica de la migración.

## <a name="deploy-a-side-by-side-vpn-infrastructure"></a>Implementación de una infraestructura de VPN en paralelo

Implementa la infraestructura de VPN en paralelo con la infraestructura de DirectAccess existente.  Para obtener información paso a paso, consulte [Always on de la implementación de VPN para Windows Server y Windows 10](../vpn/always-on-vpn/deploy/always-on-vpn-deploy.md) para instalar y configurar la infraestructura de vpn de Always on.

La implementación en paralelo consta de las siguientes tareas de alto nivel:

1.  Cree los grupos usuarios de VPN, servidores VPN y servidores NPS.

2.  Cree y publique las plantillas de certificado necesarias.

3.  Inscriba los certificados de servidor.

4.  Instale y configure el servicio de acceso remoto para VPN Always On.

5.  Instalar y configurar NPS.

6.  Configure las reglas de firewall y DNS para Always On VPN.

La imagen siguiente proporciona una referencia visual para los cambios de infraestructura a través de la migración de VPN de DirectAccess a Always On.

![Diagrama de cambios de infraestructura en la migración de VPN de DirectAccess a Always On](../../media/DA-to-AlwaysOnVPN/6b64f322f945f837f22a32bf87a228f8.png)

## <a name="deploy-certificates-and-vpn-configuration-script-to-the-clients"></a>Implementar certificados y scripts de configuración de VPN en los clientes

Aunque la mayor parte de la configuración del cliente VPN en la sección [implementar Always on VPN](../vpn/always-on-vpn/deploy/always-on-vpn-deploy-deployment.md) , se necesitan dos pasos adicionales para completar la migración de DirectAccess a Always on VPN correctamente.

Debe asegurarse de que el **VPN_Profile.ps1** proceda _después_ de emitir el certificado para que el cliente VPN no intente conectarse sin él. Para ello, ejecute un script que agregue solo los usuarios que se han inscrito en el certificado al grupo de implementación de VPN preparado, que se usa para implementar la Always On configuración de VPN.

>[!NOTE]
>Microsoft recomienda que pruebe este proceso antes de realizarlo en cualquiera de los anillos de migración de usuario.

1.  **Cree y publique el certificado VPN y habilite el objeto de directiva de grupo de inscripción automática (GPO).** En el caso de las implementaciones tradicionales de VPN de Windows 10 basadas en certificados, se emite un certificado para el dispositivo o el usuario para que pueda autenticar la conexión. Cuando el nuevo certificado de autenticación se crea y se publica para la inscripción automática, debe crear e implementar un GPO con la configuración de inscripción automática configurada para el grupo de usuarios de VPN. Para conocer los pasos para configurar certificados y la inscripción automática, consulte [configuración de la infraestructura de servidor](../vpn/always-on-vpn/deploy/vpn-deploy-server-infrastructure.md).

2.  **Agregue usuarios al grupo usuarios de VPN.** Agregue los usuarios que migre al grupo de usuarios de VPN. Esos usuarios permanecen en ese grupo de seguridad después de migrarlos para que puedan recibir actualizaciones de certificados en el futuro. Continúe agregando usuarios a este grupo hasta que haya migrado todos los usuarios de DirectAccess a Always On VPN.

3.  **Identifique a los usuarios que recibieron un certificado de autenticación de VPN.** Está migrando desde DirectAccess, por lo que tendrá que agregar un método para identificar cuándo un cliente ha recibido el certificado necesario y está listo para recibir la información de configuración de VPN. Ejecute el script **GetUsersWithCert.ps1** para agregar a los usuarios que actualmente emiten certificados no revocados que proceden del nombre de plantilla especificado a un grupo de seguridad de AD DS especificado. Por ejemplo, después de ejecutar el script de **GetUsersWithCert.ps1** , cualquier usuario emitió un certificado válido de la plantilla de certificado de autenticación de VPN se agrega al grupo de implementación de VPN preparado.

    >[!NOTE]
    >Si no tiene un método para identificar cuándo un cliente ha recibido el certificado necesario, podría implementar la configuración de VPN antes de que el certificado se haya emitido para el usuario, lo que provocaría un error en la conexión VPN. Para evitar esta situación, ejecute el script de **GetUsersWithCert.ps1** en la entidad de certificación o según una programación para sincronizar a los usuarios que han recibido el certificado en el grupo de implementación de VPN preparado. A continuación, usará ese grupo de seguridad para la implementación de la configuración de VPN en el punto de conexión de Microsoft Configuration Manager o Intune, lo que garantiza que el cliente administrado no reciba la configuración de VPN antes de que se haya recibido el certificado.

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

4. Implemente la configuración de VPN de Always On. A medida que se emiten los certificados de autenticación de VPN y se ejecuta el script de **GetUsersWithCert.ps1** , los usuarios se agregan al grupo de seguridad de implementación de VPN preparado.


| Si está utilizando...  | En ese caso... |
| ---- | ---- |
| Configuration Manager | Cree una recopilación de usuarios basada en la pertenencia a ese grupo de seguridad.<br><br>![Cuadro de diálogo Propiedades de criterio](../../media/DA-to-AlwaysOnVPN/b38723b3ffcfacd697b83dd41a177f66.png)|
| Intune | Simplemente se dirige al grupo de seguridad directamente una vez que está sincronizado. |

Cada vez que ejecute el script de configuración de **GetUsersWithCert.ps1** , también debe ejecutar una regla de detección de AD DS para actualizar la pertenencia al grupo de seguridad en Configuration Manager. Además, asegúrese de que la actualización de pertenencia de la colección de implementación se produce con frecuencia suficiente (alineada con el script y la regla de detección).

Para obtener información adicional sobre el uso de Configuration Manager o Intune para implementar Always On VPN en clientes Windows, consulte [Always on la implementación de VPN para Windows Server y Windows 10](../vpn/always-on-vpn/deploy/always-on-vpn-deploy.md). No obstante, asegúrese de incorporar estas tareas específicas de la migración.

>[!NOTE]
>La incorporación de estas tareas específicas de la migración es una diferencia fundamental entre una sencilla Always On la implementación de VPN y la migración de DirectAccess a Always On VPN. Asegúrese de definir correctamente la colección para establecer como destino el grupo de seguridad en lugar de usar el método en la guía de implementación.


## <a name="remove-devices-from-the-directaccess-security-group"></a>Quitar dispositivos del grupo de seguridad de DirectAccess

A medida que los usuarios reciben el certificado de autenticación y el script de configuración de **VPN_Profile.ps1** , verá implementaciones de scripts de configuración de VPN correctas correspondientes en Configuration Manager o Intune. Después de cada implementación, quite el dispositivo de ese usuario del grupo de seguridad de DirectAccess para que pueda quitar después DirectAccess. Tanto Intune como Configuration Manager contienen información de asignación de dispositivos de usuario para ayudarle a determinar el dispositivo de cada usuario.

>[!NOTE]
>Si va a aplicar GPO de DirectAccess a través de unidades organizativas (OU) en lugar de grupos de equipos, mueva el objeto de equipo del usuario fuera de la unidad organizativa.

## <a name="decommission-the-directaccess-infrastructure"></a>Retirar la infraestructura de DirectAccess

Cuando haya terminado de migrar todos los clientes de DirectAccess a Always On VPN, puede retirar la infraestructura de DirectAccess y quitar la configuración de DirectAccess de directiva de grupo. Microsoft recomienda realizar los pasos siguientes para quitar DirectAccess de su entorno correctamente:

1. **Quite los valores de configuración.** Quite los GPO y el acceso remoto de configuración de directiva de grupo de acceso remoto creado; para ello, abra la consola de administración de acceso remoto y seleccione quitar opciones de configuración, tal como se muestra en la imagen siguiente. Si quita el grupo antes de quitar la configuración, es probable que se produzcan errores.

    ![Proceso para quitar DirectAccess de su entorno](../../media/DA-to-AlwaysOnVPN/dbdc3d80e8dc1b8665f7b15d7d2ee1f6.png)

2. **Quite los dispositivos del grupo de seguridad de DirectAccess.** A medida que los usuarios migran correctamente, se quitan los dispositivos del grupo de seguridad de DirectAccess. Antes de quitar DirectAccess de su entorno, compruebe si el grupo de seguridad de DirectAccess está vacío. No quite el grupo de seguridad si todavía contiene miembros. Si quita el grupo de seguridad que contiene miembros, corre el riesgo de que los usuarios no tengan acceso remoto desde sus dispositivos. Use el punto de conexión de Microsoft Configuration Manager o Microsoft Intune para determinar la información de asignación de dispositivos y detectar qué dispositivo pertenece a cada usuario.

3. **Limpie DNS.** Asegúrese de quitar todos los registros del servidor DNS interno y el servidor DNS público relacionado con DirectAccess, por ejemplo, DA.contoso.com, DAGateway.contoso.com.

4. **Retirar el servidor de DirectAccess.** Cuando haya quitado correctamente los valores de configuración y los registros DNS, estará listo para anular el servidor de DirectAccess. Para ello, quite el rol en Administrador del servidor o retire el servidor y quítelo de AD DS.

5. **Quite los certificados de DirectAccess de Active Directory servicios de Certificate Server.** Si usó certificados de equipo para la implementación de DirectAccess, quite las plantillas publicadas de la carpeta plantillas de certificado en la consola entidad de certificación.

## <a name="next-step"></a>Paso siguiente

Ha terminado la migración de DirectAccess a Always On VPN.
