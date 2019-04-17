---
title: Acceso condicional para la conectividad VPN con Azure AD
description: En este paso opcional, puedes ajustar acceso de usuarios VPN autorizado cómo los recursos con el acceso condicional de Azure Active Directory (AD Azure).
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 07/13/2018
ms.reviewer: deverette
ms.openlocfilehash: c9104a5d9ae3069e753b8b771270502c4264db96
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067475"
---
# Paso 7. (Opcional) Acceso condicional para la conectividad VPN con Azure AD

& #171;  [ **Anterior:** paso 6. Configurar al cliente de Windows 10 siempre en conexiones VPN](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)<br>
& #187; [ **Siguiente:** paso 7.1. Configurar EAP-TLS para omitir la comprobación de la lista de revocación de certificados (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md)

En este paso opcional, puedes ajustar cómo los usuarios VPN acceder a los recursos con el [acceso condicional de Azure Active Directory (AD Azure)](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). Con el acceso condicional de Azure AD para la conectividad de red privada virtual (VPN), puede ayudar a proteger las conexiones VPN. Acceso condicional es un motor de evaluación basado en directivas que te permite crear reglas de acceso para cualquier aplicación conectada de Azure Active Directory (Azure AD). 

## Requisitos previos

Estás familiarizado con los siguientes temas:
- [Acceso condicional en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)
- [VPN y acceso condicional](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)

Para configurar el acceso condicional de Azure Active Directory para la conectividad de VPN, debes tener la siguiente configuración:
- [Infraestructura de servidor](always-on-vpn/deploy/vpn-deploy-server-infrastructure.md)
- [Servidor de acceso remoto para siempre en VPN](always-on-vpn/deploy/vpn-deploy-ras.md)
- [Servidor de directivas de redes](always-on-vpn/deploy/vpn-deploy-nps.md)
- [Configuración de Firewall y de DNS](always-on-vpn/deploy/vpn-deploy-dns-firewall.md)
- [Cliente de Windows 10 siempre en conexiones de VPN](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)

## [Paso 7.1. Configurar EAP-TLS para omitir la comprobación de la lista de revocación de certificados (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md)

En este paso, puedes agregar **IgnoreNoRevocationCheck** y configurarla para permitir la autenticación de los clientes cuando el certificado no incluye los puntos de distribución CRL. De manera predeterminada, IgnoreNoRevocationCheck se establece en 0 (deshabilitado).

A menos que el servidor NPS realiza una comprobación de revocación de la cadena de certificados (incluido el certificado raíz), no se puede conectar un cliente EAP-TLS. En la nube certificados al usuario por Azure AD no tienen una CRL porque son certificados de corta duración con un ciclo de vida de una hora. EAP en NPS debe estar configurada para omitir la ausencia de una CRL. Dado que el método de autenticación es EAP-TLS, este valor del registro solo es necesario con **EAP\13**. Si se usan otros métodos de autenticación de EAP, a continuación, el valor del registro debe agregarse en ellos también. 




## [Paso 7.2. Crear certificados raíz para la autenticación de VPN con Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md)

En este paso, los certificados raíz para la autenticación de VPN se configura con Azure AD, que crea automáticamente una aplicación en la nube de servidor VPN en el inquilino.  

Para configurar el acceso condicional para la conectividad de VPN, debes:
1. Crear un certificado VPN en el portal de Azure (puedes crear más de un certificado).
2. Descargar el certificado VPN.
3. Implementar el certificado en el servidor VPN.

## [Paso 7.3. Configurar la directiva de acceso condicional](vpn-config-conditional-access-policy.md)

En este paso, se configura la directiva de acceso condicional para la conectividad de VPN. 

Para configurar la directiva de acceso condicional, debes:
1. Crear una directiva de acceso condicional que se asigna a los usuarios VPN.
2. Establece la aplicación en la nube en el **servidor VPN**.
3. Establece la concesión (control de acceso) para **requerir la autenticación multifactor**.  Puedes usar otros controles según sea necesario.

## [Paso 7.4. Implementar certificados raíz de acceso condicional en local de AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)

En este paso, se implementa un certificado raíz de confianza para la autenticación de VPN en tus locales AD.

Para implementar el certificado raíz de confianza, debes:
1. Agregar el certificado descargado como una *entidad emisora de certificados para la autenticación de VPN de raíz de confianza*.
2. Importar el certificado raíz para el servidor VPN y el cliente VPN.
3. Comprueba que los certificados estén presentes y mostrarán como de confianza.


## [Paso 7.5. Crear perfiles de VPNv2 basados en OMA-DM para dispositivos Windows 10](vpn-create-oma-dm-based-vpnv2-profiles.md)

En este paso, puedes crear OMA DM en función de los perfiles de VPNv2 usar Intune para implementar una directiva de configuración del dispositivo VPN. Si quieres usar SCCM o Script de PowerShell para crear perfiles de VPNv2, consulte la [configuración de VPNv2 CSP](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp) para obtener más detalles. 


## Paso siguiente
Paso [7.1. Configurar EAP-TLS para omitir la comprobación de la lista de revocación de certificados (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md): en este paso, debes agregar **IgnoreNoRevocationCheck** y configurarla para permitir la autenticación de los clientes cuando el certificado no incluye los puntos de distribución CRL. De manera predeterminada, IgnoreNoRevocationCheck se establece en 0 (deshabilitado).

---

## Temas relacionados
- [Configurar perfiles de VPNv2](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): cliente de la VPN ahora es capaz de integrarse con la plataforma de acceso condicional basado en la nube para proporcionar una opción de cumplimiento del dispositivo para los clientes remotos. En este paso, configurar los perfiles de VPNv2 con **\ < DeviceCompliance > \ < habilitado > true\ < / Enabled >**. 
 
- [Mejora de acceso remoto en Windows 10 con un perfil VPN automática](https://www.microsoft.com/itshowcase/Article/Content/894/Enhancing-remote-access-in-Windows-10-with-an-automatic-VPN-profile): Obtén información sobre cómo Microsoft implementa el acceso condicional para la conectividad de VPN. Perfiles de VPN contengan toda la información que se requiere un dispositivo para conectarse a la red corporativa, incluidos los métodos de autenticación que son compatibles y el servidor VPN que se debe conectar el dispositivo. Los cambios en la actualización de aniversario de Windows 10, incluido el acceso condicional y sesión único, hecho posible para que podamos crear nuestro perfil de conexión VPN Maletín le. Hemos creado para el perfil de conexión unido al dominio y dispositivos de Microsoft Intune: administrados mediante la consola de System Center Configuration Manager. 

- [Condicional acceso en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal): seguridad es una preocupación para las organizaciones que usan la nube. Un aspecto clave de seguridad en la nube es identidad y acceso en cuanto a la administración de los recursos de nube. En un mundo en la nube primero móvil, primero, los usuarios pueden acceder a los recursos de la organización mediante una variedad de dispositivos y aplicaciones desde cualquier lugar. En consecuencia, solo se centran en que puede tener acceso a un recurso no es suficiente ya. Con el fin de maestro y el equilibrio entre la seguridad y la productividad, los profesionales de TI también necesitan repartir cómo un recurso se accede a una decisión de control de acceso.

- [VPN y acceso condicional](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): cliente de la VPN ahora es capaz de integrarse con la plataforma de acceso condicional basado en la nube para proporcionar una opción de cumplimiento del dispositivo para los clientes remotos. Acceso condicional es un motor de evaluación basado en directivas que te permite crear reglas de acceso para cualquier aplicación conectada de Azure Active Directory (Azure AD). 

---