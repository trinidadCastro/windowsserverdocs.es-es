---
title: Crear perfiles de VPNv2 basados en OMA-DM para dispositivos con Windows 10
description: 'Puede usar uno de los dos métodos para crear la configuración de OMA-DM en función de los perfiles de VPNv2. '
services: active-directory
ms.prod: windows-server-threshold
ms.technology: networking-ras
documentationcenter: ''
ms.assetid: ''
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 9051a5b4dc8055885bdc1f8f727514b6e049d74d
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749505"
---
# <a name="step-75-create-oma-dm-based-vpnv2-profiles-to-windows-10-devices"></a>Paso 7.5. Crear configuración de OMA-DM basado VPNv2 perfiles para dispositivos Windows 10

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Anterior:** Paso 7.4. Implementar certificados de raíz de acceso condicional en local AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)
- [**Siguiente:** Obtenga información sobre cómo el acceso condicional para VPN funciona](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)

En este paso, puede crear configuración de OMA-DM en función de los perfiles de VPNv2 mediante Intune para implementar una directiva de configuración del dispositivo VPN. Si desea usar el script de PowerShell o SCCM para crear perfiles de VPNv2, consulte [configuración de CSP de VPNv2](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp) para obtener más detalles. 

## <a name="managed-deployment-using-intune"></a>Implementación administrada con Intune

Todo lo descrito en esta sección es el mínimo necesario para que VPN funcione con acceso condicional. No se cubre el túnel dividido, con WIP, creación de perfiles de configuración de dispositivo de Intune personalizados para obtener AutoVPN trabajar o inicio de sesión único. Integración de la configuración a continuación en el perfil de VPN que creó anteriormente en [paso 5. Configuración de cliente Windows 10 siempre en las conexiones VPN](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md).  En este ejemplo, estamos integrando en la [configurar el cliente VPN mediante Intune](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md#configure-the-vpn-client-by-using-intune) directiva. 

**Requisito previo:**

Ya se ha configurado el equipo cliente de Windows 10 con una conexión VPN mediante Intune.   


**Procedimiento:**

1. En el portal de Azure, seleccione **Intune** > **configuración del dispositivo** > **perfiles** y seleccione el perfil VPN que creó anteriormente en [ Configurar el cliente VPN mediante Intune](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md#configure-the-vpn-client-by-using-intune).
    
2. En el editor de directivas, seleccione **propiedades** > **configuración** > **VPN Base**. Extender existente **Xml de EAP** para incluir un filtro que da al cliente VPN en la lógica que necesita para recuperar el certificado de acceso condicional de AAD del almacén de certificados del usuario en lugar de dejarlo al azar lo que le permite usar la primera certificado detectado.

    >[!NOTE]
    >Sin esto, el cliente de VPN pudo recuperar el certificado de usuario emitido por la entidad de certificación en el entorno local, lo que produce un error en la conexión VPN.

    ![Portal de Intune](../../media/Always-On-Vpn/intune-eap-xml.png)

3. Busque la sección que termina con  **\</AcceptServerName >\</EapType >** e inserte la siguiente cadena de entre estos dos valores para proporcionar al cliente VPN con la lógica para seleccionar el condicional de AAD Certificado de acceso:

    ```XML
    <TLSExtensions xmlns="http://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2"><FilteringInfo xmlns="http://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV3"><EKUMapping><EKUMap><EKUName>AAD Conditional Access</EKUName><EKUOID>1.3.6.1.4.1.311.87</EKUOID></EKUMap></EKUMapping><ClientAuthEKUList Enabled="true"><EKUMapInList><EKUName>AAD Conditional Access</EKUName></EKUMapInList></ClientAuthEKUList></FilteringInfo></TLSExtensions>
    ```

4. Seleccione el **acceso condicional** hoja y alternancia **acceso condicional para esta conexión VPN** a **habilitado**.
   
   Si se habilita cambios de configuración el  **\<DeviceCompliance >\<habilitado > true\</habilitada >** en el XML del perfil VPNv2.

    ![Acceso condicional para VPN - propiedades de Always On](../../media/Always-On-Vpn/vpn-conditional-access-azure-ad.png)

5. Seleccione **Aceptar**.

6. Seleccione **asignaciones**, en incluir, seleccione **seleccionar grupos para incluir**.

7. Seleccione el **usuarios de VPN** grupo que recibe esta directiva y seleccione **guardar**.

    ![LÍMITE para usuarios VPN automática - asignaciones](../../media/Always-On-Vpn/cap-for-auto-vpn-users-assignments.png)

## <a name="force-mdm-policy-sync-on-the-client"></a>Forzar la sincronización de directivas MDM en el cliente

Si el perfil de VPN no se muestra en el dispositivo cliente, en la configuración de\\red e Internet\\VPN, puede forzar la directiva MDM para sincronizar.

1. Inicie sesión en un equipo cliente unido al dominio como un miembro de la **usuarios de VPN** grupo.

2. En el menú Inicio, escriba **cuenta**, y presione ENTRAR.

3. En el panel de navegación izquierdo, seleccione **acceso profesional o educativo**.

4. En acceso profesional o educativa, seleccione **conectado a < \domain > MDM**, a continuación, seleccione **información**.

5. Seleccione **sincronización** y compruebe el perfil de VPN aparece en la configuración de\\red e Internet\\VPN.


## <a name="next-steps"></a>Pasos siguientes

Termine de configurar el perfil VPN para usar el acceso condicional de Azure AD. 

|Si desea...  |A continuación, vea...  |
|---------|---------|
|Más información sobre cómo condicional funciona el acceso con las redes privadas virtuales  |[VPN y el acceso condicional](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): Esta página proporciona más información sobre cómo el acceso condicional funciona con VPN.      |
|Más información sobre las características avanzadas de VPN  |[Características VPN avanzadas](always-on-vpn/deploy/always-on-vpn-adv-options.md#advanced-vpn-features): Esta página proporciona instrucciones sobre cómo habilitar los filtros de tráfico VPN, cómo configurar conexiones VPN automáticas mediante desencadenadores de aplicación y cómo configurar NPS para permitir solo conexiones VPN de los clientes que usan los certificados emitidos por Azure AD.        |


## <a name="related-topics"></a>Temas relacionados

- [VPNv2 CSP](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-csp):  En este tema se proporciona información general sobre CSP de VPNv2. El proveedor de servicios de configuración VPNv2 permite que el servidor de administración (MDM) de dispositivo móvil configurar el perfil de VPN del dispositivo.

- [Configuración de cliente Windows 10 siempre en las conexiones VPN](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections): Este tema proporciona información sobre las opciones de ProfileXML y esquema y cómo crear la VPN ProfileXML. Después de configurar la infraestructura de servidor, debe configurar los equipos cliente de Windows 10 para comunicarse con esa infraestructura con una conexión VPN. 

- [Configurar el cliente VPN mediante Intune](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections#configure-the-vpn-client-by-using-intune): En este tema se proporciona información sobre cómo implementar perfiles de Windows 10 siempre en VPN de acceso remoto. Ahora Intune usa grupos de Azure AD. Si Azure AD Connect ha sincronizado el grupo de usuarios de VPN desde local a Azure AD, no hay ninguna necesidad de configurar al cliente VPN con Intune.
