---
title: Acceso condicional para la conectividad VPN con Azure AD
description: En este paso opcional, puede ajustar el modo en que los usuarios de VPN autorizados acceden a los recursos mediante el acceso condicional de Azure Active Directory (Azure AD).
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.localizationpriority: medium
ms.author: v-tea
author: Teresa-MOTIV
ms.date: 06/28/2019
ms.reviewer: deverette
ms.openlocfilehash: b524e2040706c7e4b2a897d213e28d6b4c91c8f7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856068"
---
# <a name="step-7-optional-conditional-access-for-vpn-connectivity-using-azure-ad"></a>Paso 7. Opta Acceso condicional para la conectividad VPN con Azure AD

- [**Anterior:** Paso 6. Configuración de conexiones VPN de Always On cliente de Windows 10](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)
- [**Siguiente:** Paso 7,1. Configurar EAP-TLS para omitir la comprobación de la lista de revocación de certificados (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md)

En este paso opcional, puede ajustar el modo en que los usuarios de VPN acceden a los recursos mediante el [acceso condicional de Azure Active Directory (Azure ad)](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). Con Azure AD acceso condicional para la conectividad de la red privada virtual (VPN), puede ayudar a proteger las conexiones VPN. Acceso condicional es un motor de evaluación basado en directivas que te permite crear reglas de acceso para cualquier aplicación conectada de Azure Active Directory (Azure AD).

## <a name="prerequisites"></a>Requisitos previos

Está familiarizado con los temas siguientes:

- [Acceso condicional en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)
- [VPN y acceso condicional](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)

Para configurar Azure Active Directory el acceso condicional para la conectividad VPN, debe tener configurados los siguientes:

- [Infraestructura de servidor](always-on-vpn/deploy/vpn-deploy-server-infrastructure.md)
- [Servidor de acceso remoto para VPN Always On](always-on-vpn/deploy/vpn-deploy-ras.md)
- [Servidor de directivas de redes](always-on-vpn/deploy/vpn-deploy-nps.md)
- [Configuración de DNS y firewall](always-on-vpn/deploy/vpn-deploy-dns-firewall.md)
- [Conexiones VPN de cliente Always On de Windows 10](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)

## <a name="step-71-configure-eap-tls-to-ignore-certificate-revocation-list-crl-checking"></a>[Paso 7,1. Configurar EAP-TLS para omitir la comprobación de la lista de revocación de certificados (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md)

En este paso, puede agregar **IgnoreNoRevocationCheck** y establecerlo para permitir la autenticación de clientes cuando el certificado no incluya puntos de distribución de CRL. De forma predeterminada, IgnoreNoRevocationCheck se establece en 0 (deshabilitado).

Un cliente EAP-TLS no puede conectarse a menos que el servidor NPS complete una comprobación de revocación de la cadena de certificados (incluido el certificado raíz). Los certificados de nube emitidos para el usuario por Azure AD no tienen una CRL porque son certificados de corta duración con una duración de una hora. EAP en NPS debe configurarse para omitir la ausencia de una CRL. Dado que el método de autenticación es EAP-TLS, este valor del registro solo es necesario en **EAP\13**. Si se usan otros métodos de autenticación de EAP, el valor del registro debe agregarse también en ellos.

## <a name="step-72-create-root-certificates-for-vpn-authentication-with-azure-ad"></a>[Paso 7,2. Crear certificados raíz para la autenticación VPN con Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md)

En este paso, se configuran los certificados raíz para la autenticación VPN con Azure AD, que crea automáticamente una aplicación de nube de servidor VPN en el inquilino.  

Para configurar el acceso condicional para la conectividad VPN, debe:

1. Cree un certificado VPN en el Azure Portal.
2. Descargue el certificado VPN.
3. Implemente el certificado en el servidor VPN.

> [!IMPORTANT]
> Una vez creado un certificado VPN en el Azure Portal, Azure AD comenzará a usarlo inmediatamente para emitir certificados de corta duración al cliente VPN. Es fundamental que el certificado VPN se implemente inmediatamente en el servidor VPN para evitar problemas con la validación de credenciales del cliente VPN.

## <a name="step-73-configure-the-conditional-access-policy"></a>[Paso 7,3. Configuración de la Directiva de acceso condicional](vpn-config-conditional-access-policy.md)

En este paso, configurará la Directiva de acceso condicional para la conectividad VPN.

Para configurar la Directiva de acceso condicional, debe:

1. Cree una directiva de acceso condicional que se asigne a los usuarios de VPN.
2. Establezca la aplicación en la nube en el **servidor VPN**.
3. Establezca la concesión (control de acceso) para **requerir la autenticación multifactor**.  Puede usar otros controles según sea necesario.

## <a name="step-74-deploy-conditional-access-root-certificates-to-on-premises-ad"></a>[Paso 7,4. Implementar certificados raíz de acceso condicional en AD local](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)

En este paso, implementará un certificado raíz de confianza para la autenticación de VPN en su instancia de AD local.

Para implementar el certificado raíz de confianza, debe:

1. Agregue el certificado descargado como *entidad de certificación raíz de confianza para la autenticación de VPN*.
2. Importe el certificado raíz en el servidor VPN y el cliente VPN.
3. Compruebe que los certificados están presentes y se muestran como de confianza.

## <a name="step-75-create-oma-dm-based-vpnv2-profiles-to-windows-10-devices"></a>[Paso 7,5. Creación de perfiles de VPNv2 basados en OMA-DM en dispositivos de Windows 10](vpn-create-oma-dm-based-vpnv2-profiles.md)

En este paso, puede crear perfiles de VPNv2 basados en OMA-DM mediante Intune para implementar una directiva de configuración de dispositivos VPN. Si desea usar Configuration Manager o el script de PowerShell para crear perfiles de VPNv2, consulte [configuración de VPNV2 CSP](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp) para obtener más detalles.

## <a name="next-steps"></a>Pasos siguientes

[Paso 7,1. Configurar EAP-TLS para omitir la comprobación de la lista de revocación de certificados (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md): en este paso, debe agregar **IgnoreNoRevocationCheck** y establecerlo para permitir la autenticación de los clientes cuando el certificado no incluya puntos de distribución de CRL. De forma predeterminada, IgnoreNoRevocationCheck se establece en 0 (deshabilitado).

## <a name="related-topics"></a>Temas relacionados

- [Configuración de perfiles de VPNv2](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): el cliente VPN ahora puede integrarse con la plataforma de acceso condicional basado en la nube para proporcionar una opción de cumplimiento de dispositivos para clientes remotos. En este paso, se configuran los perfiles de VPNv2 con **\<DeviceCompliance > \<habilitado > true\</Enabled >** .

- [Mejorar el acceso remoto en Windows 10 con un perfil de VPN automático](https://www.microsoft.com/itshowcase/Article/Content/894/Enhancing-remote-access-in-Windows-10-with-an-automatic-VPN-profile): Obtenga información sobre cómo Microsoft implementa el acceso condicional para la conectividad VPN. Los perfiles de VPN contienen toda la información que necesita un dispositivo para conectarse a la red corporativa, incluidos los métodos de autenticación que se admiten y el servidor VPN al que se debe conectar el dispositivo. Los cambios en la actualización de aniversario de Windows 10, incluido el acceso condicional y el inicio de sesión único, nos permiten crear el perfil de conexión VPN de AlwaysOn.

- [Acceso condicional en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal): la seguridad es una de las principales preocupaciones para las organizaciones que usan la nube. Un aspecto clave de la seguridad en la nube es la identidad y el acceso cuando se trata de administrar los recursos en la nube. En un mundo primero móvil y en la nube, los usuarios pueden acceder a los recursos de su organización mediante una variedad de dispositivos y aplicaciones desde cualquier lugar. Como resultado de esto, solo tiene que centrarse en quién puede acceder a un recurso ya no es suficiente. Para dominar el equilibrio entre seguridad y productividad, los profesionales de ti también necesitan factorizar cómo se accede a un recurso en una decisión de control de acceso.

- [VPN y acceso condicional](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): el cliente VPN ahora puede integrarse con la plataforma de acceso condicional basado en la nube para proporcionar una opción de cumplimiento de dispositivos para clientes remotos. Acceso condicional es un motor de evaluación basado en directivas que te permite crear reglas de acceso para cualquier aplicación conectada de Azure Active Directory (Azure AD).
