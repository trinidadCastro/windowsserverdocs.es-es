---
title: Crear perfiles de VPNv2 basados en OMA-DM para dispositivos con Windows 10
description: 'Puede usar uno de estos dos métodos para crear perfiles de VPNv2 basados en OMA-DM. '
ms.topic: article
ms.date: 07/13/2018
ms.author: v-tea
author: Teresa-MOTIV
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: c5619baa5123d0cd611cb9371cd3944fdd91fb3c
ms.sourcegitcommit: 877d6db73d9520e3a23738d6528016235493cff3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2020
ms.locfileid: "90779269"
---
# <a name="step-75-create-oma-dm-based-vpnv2-profiles-to-windows-10-devices"></a>Paso 7.5. Creación de perfiles de VPNv2 basados en OMA-DM en dispositivos de Windows 10

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Anterior:** Paso 7,4. Implementar certificados raíz de acceso condicional en AD local](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)
- [**Siguiente:** Obtener información sobre cómo funciona el acceso condicional para VPN](/windows/access-protection/vpn/vpn-conditional-access)

En este paso, puede crear perfiles de VPNv2 basados en OMA-DM mediante Intune para implementar una directiva de configuración de dispositivos VPN. Si desea usar el punto de conexión de Microsoft Configuration Manager o el script de PowerShell para crear perfiles de VPNv2, consulte [configuración de CSP de VPNv2](/windows/client-management/mdm/vpnv2-csp) para obtener más detalles.

## <a name="managed-deployment-using-intune"></a>Implementación administrada con Intune

Todo lo que se describe en esta sección es el mínimo necesario para que la VPN funcione con el acceso condicional. No se trata la tunelización dividida, el uso de WIP, la creación de perfiles de configuración de dispositivos de Intune personalizados para obtener AutoVPN en funcionamiento o SSO. Integre la configuración siguiente en el perfil de VPN que creó anteriormente en el [paso 5. Configure conexiones VPN Always On de cliente de Windows 10](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md).En este ejemplo, se integran en el [configuración del cliente VPN](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md#configure-the-vpn-client-by-using-intune) mediante la Directiva de Intune.

**Requisitos previos**

El equipo cliente de Windows 10 ya se configuró con una conexión VPN mediante Intune.


**Pasos**

1. En el Azure portal, seleccione **Intune**  >  **configuración de dispositivos**  >  **perfiles** y seleccione el perfil de VPN que creó anteriormente en [configurar el cliente VPN mediante Intune](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md#configure-the-vpn-client-by-using-intune).

2. En el editor de directivas, seleccione **propiedades**  >  **configuración**  >  **base VPN**. Extienda el **XML de EAP** existente para incluir un filtro que proporcione al cliente VPN la lógica que necesita para recuperar el certificado de acceso condicional de AAD desde el almacén de certificados del usuario en lugar de dejarlo a la oportunidad de permitirle usar el primer certificado descubierto.

    >[!NOTE]
    >Sin esto, el cliente VPN podría recuperar el certificado de usuario emitido desde la entidad de certificación local, lo que produciría un error en la conexión VPN.

    ![Portal de Intune](../../media/Always-On-Vpn/intune-eap-xml.png)

3. Busque la sección que termina con **\</AcceptServerName>\</EapType>** e inserte la siguiente cadena entre estos dos valores para proporcionar al cliente VPN la lógica para seleccionar el certificado de acceso condicional de AAD:

    ```XML
    <TLSExtensions xmlns="http://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2"><FilteringInfo xmlns="http://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV3"><EKUMapping><EKUMap><EKUName>AAD Conditional Access</EKUName><EKUOID>1.3.6.1.4.1.311.87</EKUOID></EKUMap></EKUMapping><ClientAuthEKUList Enabled="true"><EKUMapInList><EKUName>AAD Conditional Access</EKUName></EKUMapInList></ClientAuthEKUList></FilteringInfo></TLSExtensions>
    ```

4. Seleccione la hoja **acceso condicional** y active la opción **acceso condicional para esta conexión VPN** a **habilitada**.

   Al habilitar esta opción, se cambia el valor ** \<DeviceCompliance> \<Enabled> true \</Enabled> ** en el perfil XML de VPNv2.

    ![Acceso condicional para Always On VPN-propiedades](../../media/Always-On-Vpn/vpn-conditional-access-azure-ad.png)

5. Seleccione **Aceptar**.

6. Seleccione **asignaciones**, en incluir, seleccione los **grupos que desea incluir**.

7. Seleccione el grupo de **usuarios de VPN** que recibe esta directiva y seleccione **Guardar**.

    ![LÍMITE de usuarios de VPN automática: asignaciones](../../media/Always-On-Vpn/cap-for-auto-vpn-users-assignments.png)

## <a name="force-mdm-policy-sync-on-the-client"></a>Forzar la sincronización de directivas MDM en el cliente

Si el perfil de VPN no aparece en el dispositivo cliente, en configuración \\ red & VPN de Internet \\ , puede forzar la sincronización de la Directiva MDM.

1. Inicie sesión en un equipo cliente unido a un dominio como miembro del grupo de **usuarios de VPN** .

2. En el menú Inicio, escriba **cuenta**y presione Entrar.

3. En el panel de navegación izquierdo, seleccione **acceso profesional o educativo**.

4. En acceso profesional o educativo, seleccione **conectado a < \admins.> MDM**y, a continuación, seleccione **información**.

5. Seleccione **sincronizar** y compruebe que el perfil de VPN aparece en configuración \\ red & VPN de Internet \\ .


## <a name="next-steps"></a>Pasos siguientes

Ha terminado de configurar el perfil de VPN para usar Azure AD el acceso condicional.

|Si desea...  |A continuación, vea...  |
|---------|---------|
|Más información sobre cómo funciona el acceso condicional con las VPN  |[VPN y acceso condicional](/windows/access-protection/vpn/vpn-conditional-access): esta página proporciona más información acerca de cómo funciona el acceso condicional con las VPN.      |
|Más información acerca de las características avanzadas de VPN  |[Características avanzadas de VPN](always-on-vpn/deploy/always-on-vpn-adv-options.md#advanced-vpn-features): en esta página se proporcionan instrucciones sobre cómo habilitar los filtros de tráfico VPN, cómo configurar conexiones VPN automáticas mediante desencadenadores de aplicaciones y cómo configurar NPS para permitir solo las conexiones VPN de los clientes que usan certificados emitidos por Azure ad.        |


## <a name="related-topics"></a>Temas relacionados

- [CSP de VPNv2](/windows/client-management/mdm/vpnv2-csp): en este tema se proporciona información general sobre el CSP de VPNv2. El proveedor de servicios de configuración de VPNv2 permite al servidor de administración de dispositivos móviles (MDM) configurar el perfil de VPN del dispositivo.

- [Configuración de conexiones VPN de Always on cliente de Windows 10](./always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md): en este tema se proporciona información sobre las opciones y el esquema de ProfileXML y cómo crear la VPN ProfileXML. Después de configurar la infraestructura de servidor, debe configurar los equipos cliente de Windows 10 para que se comuniquen con esa infraestructura con una conexión VPN.

- [Configuración del cliente VPN mediante Intune](./always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md#configure-the-vpn-client-by-using-intune): en este tema se proporciona información sobre cómo implementar el acceso remoto de Windows 10 Always on perfiles de VPN. Intune ahora usa grupos de Azure AD. Si Azure AD Connect sincronizar el grupo de usuarios de VPN desde el entorno local a Azure AD, no es necesario configurar el cliente VPN mediante Intune.
