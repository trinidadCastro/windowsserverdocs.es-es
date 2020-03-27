---
title: Introducción a la migración de VPN de acceso remoto Always On
description: Always On VPN aborda los huecos anteriores entre las VPN de Windows y DirectAccess, y cómo migrar de DirectAccess a Always On VPN.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: lizross
author: eross-msft
ms.date: 05/29/2018
ms.openlocfilehash: bd4d0d4d3b165a4e89a00cd2975ace20687aed7d
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80314985"
---
# <a name="overview-of-the-directaccess-to-always-on-vpn-migration"></a>Información general de la migración de DirectAccess a la VPN de Always On 

>Se aplica a: Windows Server (canal semianual), Windows Server 2016 y Windows 10

&#187;[ **Siguiente:** planear la migración de DIRECTACCESS a Always on VPN](da-always-on-migration-planning.md)

En versiones anteriores de la arquitectura de VPN de Windows, las limitaciones de la plataforma dificultaban proporcionar la funcionalidad crítica necesaria para reemplazar DirectAccess, como las conexiones automáticas iniciadas antes de que los usuarios iniciaran sesión. Sin embargo, Always On VPN ha mitigado la mayoría de esas limitaciones o ha ampliado la funcionalidad de VPN más allá de las capacidades de DirectAccess. Always On VPN aborda los huecos anteriores entre las VPN de Windows y DirectAccess.

El proceso de migración de la VPN de DirectAccess a Always On consta de cuatro componentes principales y procesos de alto nivel:


1.  **Planeación de la migración de VPN Always On.** La planeación ayuda a identificar los clientes de destino para la separación de la fase de usuario, así como la infraestructura y la funcionalidad.

    1.  [!INCLUDE [build-migration-rings-shortdesc-include](../includes/build-migration-rings-shortdesc-include.md)]

    2.  [!INCLUDE [review-feature-mapping-shortdesc-include](../includes/review-feature-mapping-shortdesc-include.md)] 

    3.  [!INCLUDE [review-the-enhancements-shortdesc-include](../includes/review-the-enhancements-shortdesc-include.md)] 

    4.  [!INCLUDE [review-the-technology-overview-shortdesc-include](../includes/review-the-technology-overview-shortdesc-include.md)]

2.  **Implementar una infraestructura de VPN en paralelo.** Una vez que haya determinado las fases de migración y las características que desea incluir en la implementación, implemente la Always On infraestructura de VPN en paralelo con la infraestructura de DirectAccess existente.  

3.  **Implementar certificados y configuración en los clientes.**  Una vez que la infraestructura de VPN esté lista, cree y publique los certificados necesarios en el cliente. Cuando los clientes hayan recibido los certificados, implemente el script de configuración de VPN_Profile. ps1. Como alternativa, puede usar Intune para configurar el cliente VPN. Use el punto de conexión de Microsoft Configuration Manager o Microsoft Intune para supervisar las implementaciones correctas de configuración de VPN.

4.  **Quitar y retirar.** Retirar correctamente el entorno después de haber migrado a todos los usuarios de DirectAccess.

    1.  [!INCLUDE [remove-da-from-client-shortdesc-include](../includes/remove-da-from-client-shortdesc-include.md)]

    2.  [!INCLUDE [decommission-da-shortdesc-include](../includes/decommission-da-shortdesc-include.md)]


## <a name="directaccess-deployment-scenario"></a>Escenario de implementación de DirectAccess

En este escenario de implementación, se usa un escenario de implementación de DirectAccess simple como punto de partida para la migración que presenta esta guía. No es necesario que coincida con este escenario de implementación antes de migrar a Always On VPN, pero para muchas organizaciones, esta configuración simple es una representación precisa de la implementación actual de DirectAccess. En la tabla siguiente se proporciona una lista de las características básicas de esta configuración.

Existen muchos escenarios y opciones de implementación de DirectAccess, por lo que es probable que la implementación sea diferente de la que se describe aquí. Si es así, consulte [asignación de características entre DirectAccess y Always on VPN](../vpn/vpn-map-da.md) para determinar la asignación de conjunto de características de Always on VPN para las adiciones actuales y, a continuación, agregue esas características a la configuración. Además, puede consultar el Always On las [mejoras de VPN](../vpn/always-on-vpn/always-on-vpn-enhancements.md) para agregar opciones a la implementación de Always on VPN.

>[!NOTE] 
>En el caso de los dispositivos que no están Unidos a un dominio, existen consideraciones adicionales, como la inscripción de certificados. Para obtener más información, consulte [Always on de la implementación de VPN para Windows Server y Windows 10](../vpn/always-on-vpn/deploy/always-on-vpn-deploy.md).

### <a name="deployment-scenario-feature-list"></a>Lista de características del escenario de implementación

| Característica DirectAccess | Escenario típico |
|-----|----|
| Escenario de implementación                   | Implementar DirectAccess completo para el acceso de cliente y la administración remota                                               |
| Adaptadores de red                      | 2                                                                                                              |
| Autenticación del usuario                   | Credenciales de Active Directory                                                                                   |
| Usar certificados de equipo             | Sí                                                                                                            |
| Grupos de seguridad                       | Sí                                                                                                            |
| Único servidor de DirectAccess            | Sí                                                                                                            |
| Topología de red                      | Traducción de direcciones de red (NAT) detrás de un firewall perimetral con dos adaptadores de red                            |
| Modo de acceso                           | De extremo a extremo                                                                                                    |
| Túneles                             | Túnel dividido                                                                                                   |
| Autenticación                        | Autenticación de infraestructura de clave pública (PKI) estándar con certificado de equipo más Kerberos (no KerbProxy) |
| Protocolos                             | IP sobre HTTPS (IP-HTTPS)                                                                                       |
| Servidor de ubicación de red (NLS) sin conexión | Sí                                                                                                            |

## <a name="always-on-vpn-deployment-scenario"></a>Escenario de implementación de VPN Always On

En este escenario de implementación, se centra en la migración de un entorno de DirectAccess simple a un entorno de VPN de Always On simple, que es la solución de reemplazo de DirectAccess. En la tabla siguiente se proporcionan las características que se usan en esta solución simple. Para obtener información más detallada sobre las mejoras adicionales en el cliente VPN de Always On, consulte Always On de las [mejoras de VPN](../vpn/always-on-vpn/always-on-vpn-enhancements.md).

### <a name="always-on-vpn-features-used-in-the-simple-environment"></a>Always On características de VPN usadas en el entorno simple

| Característica VPN | Configuración del escenario de implementación |
|-----|-----|
| Tipo de conexión | Versión 2 de Intercambio de claves por red nativa (IKEv2) |
| Adaptadores de red   | 2        |
| Autenticación del usuario  | Credenciales de Active Directory            |
| Usar certificados de equipo        | Sí                          |
| Enrutamiento | Tunelización dividida |
| Resolución de nombres | Lista de información de nombres de dominio y sufijo del sistema de nombres de dominio (DNS) |
| Desencadenar | AlwaysOn y detección de redes de confianza |
| Autenticación  | Protocolo de autenticación extensible protegido: seguridad de la capa de transporte (PEAP-TLS) con Módulo de plataforma segura: certificados de usuario protegidos |

## <a name="next-step"></a>Paso siguiente

[Planee la migración de DirectAccess a Always on VPN](da-always-on-migration-planning.md). El objetivo principal de la migración es que los usuarios mantengan la conectividad remota a la oficina a lo largo del proceso.

---