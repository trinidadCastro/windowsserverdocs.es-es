---
title: Acceso condicional para la conectividad VPN con Azure AD
description: En este paso opcional, puede ajustar cómo autorizado acceso a los usuarios VPN a los recursos mediante el acceso condicional de Azure Active Directory (Azure AD).
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 07/13/2018
ms.reviewer: deverette
ms.openlocfilehash: c87d0075696bf8ab5794667d42c40829c3eb61bd
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749528"
---
# <a name="step-7-optional-conditional-access-for-vpn-connectivity-using-azure-ad"></a>Paso 7. (Opcional) Acceso condicional para la conectividad VPN con Azure AD

- [**Anterior:** Paso 6. Configurar las conexiones VPN de Always On del cliente de Windows 10](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)
- [**Siguiente:** Paso 7.1. Configurar EAP-TLS para omitir la comprobación de la lista de revocación de certificados (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md)

En este paso opcional, puede ajustar cómo los usuarios VPN tener acceso a los recursos mediante [acceso condicional de Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). Con acceso condicional de Azure AD para la conectividad de red privada virtual (VPN), puede ayudar a proteger las conexiones VPN. Acceso condicional es un motor de evaluación basado en directivas que te permite crear reglas de acceso para cualquier aplicación conectada de Azure Active Directory (Azure AD). 

## <a name="prerequisites"></a>Requisitos previos

Está familiarizado con los temas siguientes:
- [Acceso condicional en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)
- [VPN y el acceso condicional](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)

Para configurar el acceso condicional de Azure Active Directory para la conectividad VPN, deberá tener la siguiente configuración:
- [Infraestructura de servidor](always-on-vpn/deploy/vpn-deploy-server-infrastructure.md)
- [Servidor de acceso remoto de VPN de Always On](always-on-vpn/deploy/vpn-deploy-ras.md)
- [Servidor de directivas de red](always-on-vpn/deploy/vpn-deploy-nps.md)
- [Configuración del Firewall y DNS](always-on-vpn/deploy/vpn-deploy-dns-firewall.md)
- [Cliente de Windows 10 siempre en las conexiones VPN](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)

## <a name="step-71-configure-eap-tls-to-ignore-certificate-revocation-list-crl-checkingvpn-config-eap-tls-to-ignore-crl-checkingmd"></a>[Paso 7.1. Configurar EAP-TLS para omitir la comprobación de la lista de revocación de certificados (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md)

En este paso, puede agregar **IgnoreNoRevocationCheck** y configúrelo para permitir la autenticación de clientes cuando el certificado no incluye puntos de distribución CRL. De forma predeterminada, IgnoreNoRevocationCheck se establece en 0 (deshabilitado).

No se puede conectar un cliente EAP-TLS, a menos que el servidor NPS complete una comprobación de revocación de la cadena de certificados (incluido el certificado raíz). Certificados en la nube Azure AD emitidos para el usuario no tiene una CRL porque son los certificados de corta duración con una duración de una hora. EAP en NPS debe configurarse para que omita la ausencia de una CRL. Puesto que el método de autenticación es EAP-TLS, este valor del registro sólo es necesario con **EAP\13**. Si se utilizan otros métodos de autenticación EAP, a continuación, el valor del registro debe agregarse en los que también.

## <a name="step-72-create-root-certificates-for-vpn-authentication-with-azure-advpn-create-root-cert-for-vpn-auth-azure-admd"></a>[Paso 7.2. Crear certificados raíz para la autenticación de VPN con Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md)

En este paso, configurará los certificados raíz para la autenticación de VPN con Azure AD, que crea automáticamente una aplicación de nube del servidor VPN en el inquilino.  

Para configurar el acceso condicional para la conectividad VPN, deberá:
1. Crear un certificado VPN en el portal de Azure (también puede crear más de un certificado).
2. Descargue el certificado VPN.
3. Implementar el certificado en el servidor VPN.

## <a name="step-73-configure-the-conditional-access-policyvpn-config-conditional-access-policymd"></a>[Paso 7.3. Configurar la directiva de acceso condicional](vpn-config-conditional-access-policy.md)

En este paso, configure la directiva de acceso condicional para la conectividad VPN.

Para configurar la directiva de acceso condicional, necesitará:

1. Crear una directiva de acceso condicional que se asigna a los usuarios VPN.
2. Establece la aplicación en la nube en **servidor VPN**.
3. Establece la concesión (control de acceso) en **requerir autenticación multifactor**.  Puede usar otros controles según sea necesario.

## <a name="step-74-deploy-conditional-access-root-certificates-to-on-premises-advpn-deploy-cond-access-root-cert-to-on-premise-admd"></a>[Paso 7.4. Implementar certificados de raíz de acceso condicional en local AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)

En este paso, implemente un certificado raíz de confianza para la autenticación VPN a sus instalaciones en AD.

Para implementar el certificado raíz de confianza, deberá:
1. Agregue el certificado descargado como un *CA raíz de confianza para la autenticación VPN*.
2. Importe el certificado raíz para el servidor VPN y el cliente VPN.
3. Compruebe que los certificados están presentes y se muestran como de confianza.

## <a name="step-75-create-oma-dm-based-vpnv2-profiles-to-windows-10-devicesvpn-create-oma-dm-based-vpnv2-profilesmd"></a>[Paso 7.5. Crear perfiles de VPNv2 basados en OMA-DM para dispositivos con Windows 10](vpn-create-oma-dm-based-vpnv2-profiles.md)

En este paso, puede crear configuración de OMA-DM en función de los perfiles de VPNv2 mediante Intune para implementar una directiva de configuración del dispositivo VPN. Si desea usar SCCM o la secuencia de comandos de PowerShell para crear perfiles de VPNv2, consulte [configuración de CSP de VPNv2](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp) para obtener más detalles. 

## <a name="next-steps"></a>Pasos siguientes

[Paso 7.1. Configurar EAP-TLS para omitir la comprobación de la lista de revocación de certificados (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md): En este paso, debe agregar **IgnoreNoRevocationCheck** y configúrelo para permitir la autenticación de clientes cuando el certificado no incluye puntos de distribución CRL. De forma predeterminada, IgnoreNoRevocationCheck se establece en 0 (deshabilitado).

## <a name="related-topics"></a>Temas relacionados

- [Configurar perfiles de VPNv2](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): El cliente VPN ahora es capaz de integrarse con la plataforma de acceso condicional basado en la nube para proporcionar una opción de cumplimiento del dispositivo para los clientes remotos. En este paso, configurará los perfiles de VPNv2 con  **\<DeviceCompliance > \<habilitado > true\</habilitada >** . 

- [Mejorar el acceso remoto en Windows 10 con un perfil de VPN automática](https://www.microsoft.com/itshowcase/Article/Content/894/Enhancing-remote-access-in-Windows-10-with-an-automatic-VPN-profile): Obtenga información sobre cómo Microsoft implementa el acceso condicional para la conectividad VPN. Perfiles de VPN contienen toda la información que requiere un dispositivo para conectarse a la red corporativa, incluidos los métodos de autenticación que se admiten y el servidor VPN que debe conectarse el dispositivo. Los cambios en Windows 10 Anniversary Update, incluido el acceso condicional y sesión único, hizo posible para que podamos crear nuestro perfil de conexión VPN Always-On. Hemos creado el perfil de conexión para unido al dominio y administrados con Intune los dispositivos de Microsoft mediante la consola de System Center Configuration Manager.

- [Acceso condicional en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal): La seguridad es una preocupación importante para las organizaciones que usan la nube. Un aspecto clave de seguridad en la nube es de identidad y acceso en cuanto a administración de los recursos en la nube. En un mundo de primero móvil, nube primero, los usuarios pueden tener acceso a los recursos de su organización mediante una variedad de dispositivos y aplicaciones desde cualquier lugar. Como consecuencia de ello, solo se centren en quién puede tener acceso a un recurso ya no es suficiente. Para dominar el equilibrio entre seguridad y productividad, los profesionales de TI también deben tener en cuenta cómo se tiene acceso a una decisión de control de acceso a un recurso.

- [VPN y el acceso condicional](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): El cliente VPN ahora es capaz de integrarse con la plataforma de acceso condicional basado en la nube para proporcionar una opción de cumplimiento del dispositivo para los clientes remotos. Acceso condicional es un motor de evaluación basado en directivas que te permite crear reglas de acceso para cualquier aplicación conectada de Azure Active Directory (Azure AD).
