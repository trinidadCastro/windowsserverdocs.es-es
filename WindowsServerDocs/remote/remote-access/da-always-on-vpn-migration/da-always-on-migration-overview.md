---
title: Introducción a la migración siempre en VPN de acceso remoto
description: VPN de Always On resuelve los anteriores espacios entre las redes privadas virtuales de Windows y DirectAccess y cómo migrar de DirectAccess a VPN de Always On.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: pashort
author: shortpatti
ms.date: 05/29/2018
ms.openlocfilehash: 402d8ff72fe869572c9e6129cdf1aa7e755c354a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845986"
---
# <a name="overview-of-the-directaccess-to-always-on-vpn-migration"></a>Información general de la migración de DirectAccess a la VPN de Always On 

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows 10

&#187;[ **Siguiente:** Planear el cliente de DirectAccess para la migración de VPN de Always On](da-always-on-migration-planning.md)

En versiones anteriores de la arquitectura de VPN de Windows, limitaciones de la plataforma resultaba difícil proporcionar la funcionalidad crítica necesaria para reemplazar DirectAccess, por ejemplo, las conexiones automáticas que se inicia antes de que los usuarios inician sesión. VPN de Always On, sin embargo, ha mitigado la mayoría de esas limitaciones o expandir la funcionalidad VPN más allá de las características de DirectAccess. VPN de Always On aborda las brechas entre las redes privadas virtuales de Windows y DirectAccess anteriores.

DirectAccess – a – siempre en el proceso de migración de VPN consta de cuatro componentes principales y los procesos de alto nivel:


1.  **Planear la migración de VPN de Always On.** Planeamiento ayuda a identificar a los clientes de destino para la separación de la fase de usuario, así como la infraestructura y la funcionalidad.

    1.  [!INCLUDE [build-migration-rings-shortdesc-include](../includes/build-migration-rings-shortdesc-include.md)]

    2.  [!INCLUDE [review-feature-mapping-shortdesc-include](../includes/review-feature-mapping-shortdesc-include.md)] 

    3.  [!INCLUDE [review-the-enhancements-shortdesc-include](../includes/review-the-enhancements-shortdesc-include.md)] 

    4.  [!INCLUDE [review-the-technology-overview-shortdesc-include](../includes/review-the-technology-overview-shortdesc-include.md)]

2.  **Implementar una infraestructura VPN en paralelo.** Después de determinar las fases de migración y las características que desea incluir en la implementación, implementa la infraestructura de VPN de Always On en paralelo con la infraestructura de DirectAccess existente.  

3.  **Implementar certificados y la configuración a los clientes.**  Una vez que la infraestructura de VPN está lista, puede crea y publicar los certificados necesarios al cliente. Cuando los clientes han recibido los certificados, implemente el script de configuración VPN_Profile.ps1. Como alternativa, puede usar Intune para configurar al cliente VPN. Utilice Microsoft System Center Configuration Manager o Microsoft Intune para supervisar implementaciones correctas de configuración de VPN.

4.  **Quitar y dar de baja.** Retirar correctamente el entorno después de haber migrado todos los usuarios desactivar DirectAccess.

    1.  [!INCLUDE [remove-da-from-client-shortdesc-include](../includes/remove-da-from-client-shortdesc-include.md)]

    2.  [!INCLUDE [decommission-da-shortdesc-include](../includes/decommission-da-shortdesc-include.md)]


## <a name="directaccess-deployment-scenario"></a>Escenario de implementación de DirectAccess

En este escenario de implementación, utilice un sencillo escenario de implementación de DirectAccess como punto de partida para la migración de que esta guía se presentan. No es necesario para que coincida con este escenario de implementación antes de migrar a la VPN de Always On, pero para muchas organizaciones, esta configuración simple es una representación precisa de su implementación actual de DirectAccess. En la tabla siguiente proporciona una lista de características básicas de la instalación.

Muchos escenarios de implementación de DirectAccess y opciones existen, por lo que es probable que sea diferente del que se describe aquí la implementación. Si es así, consulte [asignación de función entre DirectAccess y VPN de Always On](../vpn/vpn-map-da.md) para determinar la característica de VPN de Always On establece la asignación de las adiciones actuales y, a continuación, agregue estas características a la configuración. Además, puede hacer referencia a la [mejoras de VPN de Always On](../vpn/always-on-vpn/always-on-vpn-enhancements.md) para agregar las opciones para la implementación de VPN de Always On.

>[!NOTE] 
>Para dispositivos Unidos a permaneciendo, hay consideraciones adicionales, como la inscripción de certificados. Para obtener más información, consulte [siempre en VPN de implementación para Windows Server y Windows 10](../vpn/always-on-vpn/deploy/always-on-vpn-deploy.md).

### <a name="deployment-scenario-feature-list"></a>Lista de características del escenario de implementación

| Característica de DirectAccess | Escenario típico |
|-----|----|
| Escenario de implementación                   | Implementar DirectAccess completo para acceso de cliente y la administración remota                                               |
| Adaptadores de red                      | 2                                                                                                              |
| Autenticación del usuario                   | Credenciales de Active Directory                                                                                   |
| Usar certificados de equipo             | Sí                                                                                                            |
| Grupos de seguridad                       | Sí                                                                                                            |
| Servidor de DirectAccess            | Sí                                                                                                            |
| Topología de red                      | Dirección traducción de red (NAT) detrás de un firewall perimetral con dos adaptadores de red                            |
| Modo de acceso                           | Terminar con borde                                                                                                    |
| Tunelización                             | Túnel dividido                                                                                                   |
| Autenticación                        | Autenticación de la infraestructura de clave pública (PKI) estándar con Kerberos (no KerbProxy) además de certificado de equipo |
| Protocolos                             | IP a través de HTTPS (IP-HTTPS)                                                                                       |
| Ubicación (NLS) del servidor-cuadro de red | Sí                                                                                                            |

## <a name="always-on-vpn-deployment-scenario"></a>Escenario de implementación de VPN de Always On

En este escenario de implementación, centrarse en migrar un entorno simple de DirectAccess a un entorno de VPN de Always On simple, que es la solución de reemplazo de DirectAccess. En la tabla siguiente proporciona las características usadas en esta solución simple. Para obtener más información acerca de las mejoras adicionales en el cliente de VPN de Always On, vea [mejoras de VPN de Always On](../vpn/always-on-vpn/always-on-vpn-enhancements.md).

### <a name="always-on-vpn-features-used-in-the-simple-environment"></a>Características de VPN de Always On usadas en el entorno simple

| Característica VPN | Configuración del escenario de implementación |
|-----|-----|
| Tipo de conexión | Nativo intercambio de claves por red versión 2 (IKEv2) |
| Adaptadores de red   | 2        |
| Autenticación del usuario  | Credenciales de Active Directory            |
| Usar certificados de equipo        | Sí                          |
| Enrutamiento | Tunelización dividida |
| Resolución de nombres | Lista de información de nombre de dominio y el sufijo del sistema de nombres de dominio (DNS) |
| Desencadenar | Detección de red siempre encendido y confianza |
| Autenticación  | Protegido Extensible Authentication Protocol-Transport Layer Security (PEAP-TLS) con certificados de usuario protegidos por el módulo de plataforma segura |

## <a name="next-step"></a>Paso siguiente

[Planear el cliente de DirectAccess para la migración de VPN de Always On](da-always-on-migration-planning.md). El objetivo principal de la migración es para que los usuarios mantener la conectividad remota a la oficina durante el proceso.

---