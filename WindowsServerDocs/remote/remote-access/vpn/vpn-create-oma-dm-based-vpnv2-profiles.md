---
title: Crear perfiles de VPNv2 basados en OMA-DM para dispositivos con Windows 10
description: 'Puedes usar uno de dos métodos para crear OMA DM en función de los perfiles de VPNv2. '
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
ms.openlocfilehash: 1ce20d09c304b26e3708429cc45da06d020e5809
ms.sourcegitcommit: 4147e092d77d9458696e6686bccefe3788c90508
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2019
ms.locfileid: "9031299"
---
# Paso 7.5. Crear perfiles de VPNv2 a dispositivos Windows 10 basados en OMA-DM

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10

& #171;  [ **Anterior:** paso 7.4. Implementar certificados raíz de acceso condicional local de AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)<br>
& #187; [ **Siguiente:** obtener acceso condicional cómo para VPN funciona](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)

En este paso, puedes crear OMA DM en función de los perfiles de VPNv2 usar Intune para implementar una directiva de configuración del dispositivo VPN. Si quieres usar el script SCCM o PowerShell para crear perfiles de VPNv2, consulta la [configuración de VPNv2 CSP](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp) para obtener más detalles. 

## Implementación administrada con Intune

Todo lo que se tratan en esta sección es el mínimo necesario para que la VPN funcione con el acceso condicional. No cubre túnel dividido, con WIP, crear perfiles de configuración de dispositivo de Intune personalizados para obtener AutoVPN trabajando o SSO. Integrar la configuración a continuación en el perfil de VPN que creaste anteriormente en [paso 5. Configurar el cliente de Windows 10 siempre en conexiones VPN](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md).En este ejemplo, estamos integrando con la directiva de [Configurar el cliente VPN mediante Intune](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md#configure-the-vpn-client-by-using-intune) . 

**Requisito previo:**<p>
Equipo cliente de Windows 10 ya se ha configurado con una conexión VPN mediante Intune.   


**Procedimiento:**

1. En el portal de Azure, haz clic en **Intune** > **Configuración del dispositivo** > **perfiles** y selecciona el perfil de VPN que se creó anteriormente en [Configurar el cliente VPN mediante Intune](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md#configure-the-vpn-client-by-using-intune).
    
2. En el editor de directivas, selecciona **Propiedades** > **configuración** > **VPN de Base**. Ampliar **EAP Xml** existente para incluir un filtro que proporciona al cliente VPN la lógica que necesita para recuperar el certificado de acceso condicional de AAD de almacén de certificados del usuario en lugar de dejarlo al azar que permite usar el primer certificado detectado.

    >[!NOTE]
    >Sin esta opción, el cliente VPN pudo recuperar el certificado de usuario emitido por la entidad de certificación local, lo que provoca un error en la conexión VPN.

    ![Portal de Intune](../../media/Always-On-Vpn/intune-eap-xml.png)

3. Busca la sección que termine con **\</AcceptServerName>\</EapType>** e insertar la siguiente cadena entre estos dos valores para proporcionar al cliente VPN con la lógica para seleccionar el certificado de acceso condicional de AAD:

    ```XML
    <TLSExtensions xmlns="http://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2"><FilteringInfo xmlns="http://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV3"><EKUMapping><EKUMap><EKUName>AAD Conditional Access</EKUName><EKUOID>1.3.6.1.4.1.311.87</EKUOID></EKUMap></EKUMapping><ClientAuthEKUList Enabled="true"><EKUMapInList><EKUName>AAD Conditional Access</EKUName></EKUMapInList></ClientAuthEKUList></FilteringInfo></TLSExtensions>
    ```

4. Seleccione la hoja de **Acceso condicional** y toogle **acceso condicional para esta conexión VPN** en **Enabled**.<p>Habilitar esta cambia la configuración de la **\<DeviceCompliance>\<Enabled>true\</Enabled>** configuración en el XML de perfil de VPNv2.

    ![Acceso condicional para VPN - propiedades de Always On](../../media/Always-On-Vpn/vpn-conditional-access-azure-ad.png)

6. Haz clic en **Aceptar**.

6. Selecciona **las asignaciones**en incluir, haz clic en **Seleccionar grupos a incluir**.

7. Selecciona el grupo de **Usuarios de VPN** que recibe esta directiva y haga clic en **Guardar**.

    ![LÍMITE para usuarios VPN automática - asignaciones](../../media/Always-On-Vpn/cap-for-auto-vpn-users-assignments.png)

## Forzar la sincronización de la directiva MDM en el cliente
Si el perfil de VPN no aparezcan en el dispositivo cliente, en Settings\\Network & Internet\\VPN, puede forzar la directiva MDM para sincronizar.

1. Inicia sesión como un miembro del grupo de **Usuarios de VPN** en un equipo cliente unido al dominio.

2. En el menú Inicio, escribe la **cuenta**y presiona ENTRAR.

3.  En el panel de navegación izquierdo, haz clic en **obtener acceso a trabajo o escuela**.

5.  En obtener acceso a trabajo o escuela, haz clic en **conectadas a la <\domain> MDM** y haz clic en **información**.

6.  Haz clic en **la sincronización** y comprueba que el perfil de VPN aparece bajo Settings\\Network & Internet\\VPN.


## Paso siguiente
Haya terminado de configurar el perfil VPN para usar el acceso condicional de Azure AD. 

|Si quieres …  |A continuación, consulta …  |
|---------|---------|
|Más información sobre cómo condicional access funciona con las redes privadas virtuales  |[VPN y acceso condicional](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): esta página proporciona más información sobre el acceso condicional cómo funciona con las redes privadas virtuales.      |
|Más información sobre las características avanzadas de VPN  |[Características avanzadas de VPN](always-on-vpn/deploy/always-on-vpn-adv-options.md#advanced-vpn-features): esta página contiene instrucciones sobre cómo habilitar filtros de tráfico VPN, cómo configurar conexiones VPN automática mediante desencadenadores de la aplicación y cómo configurar NPS para permitir solo las conexiones VPN de clientes que usan certificados emitidos por Azure AD.        |


---

## Temas relacionados
- [VPNv2 CSP](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-csp): este tema proporciona una visión general de VPNv2 CSP. El proveedor de servicios de configuración de VPNv2 permite que el servidor de administración (MDM) de dispositivos móviles configurar el perfil VPN del dispositivo.

- [Configurar Windows 10 siempre en conexiones VPN de clientes](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections): en este tema proporciona información sobre las opciones de ProfileXML y el esquema y cómo crear la VPN ProfileXML. Después de configurar la infraestructura de servidor, debes configurar los equipos de cliente de Windows 10 para comunicarse con el que la infraestructura con una conexión VPN. 

- [Configurar el cliente VPN mediante Intune](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections#configure-the-vpn-client-by-using-intune): en este tema se proporciona información sobre cómo implementar perfiles de Windows 10 siempre en VPN de acceso remoto. Ahora, Intune usa grupos de Azure AD. Si Azure AD Connect sincroniza el grupo de usuarios de VPN local a Azure AD, no es necesario para configurar al cliente VPN con Intune.

---
