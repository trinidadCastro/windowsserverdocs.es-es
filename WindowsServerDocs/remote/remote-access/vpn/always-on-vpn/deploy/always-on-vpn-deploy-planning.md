---
title: Planear la implementación de VPN de Always On
description: En este tema se proporcionan instrucciones de planeación para implementar Always On VPN en Windows Server 2016.
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 3c9de3ec-4bbd-4db0-b47a-03507a315383
ms.localizationpriority: medium
ms.author: v-tea
author: Teresa-MOTIV
ms.date: 11/05/2018
ms.openlocfilehash: c1e85f2ee44d241bdc04e63d20de36e5cdeafb1a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80814498"
---
# <a name="step-1-plan-the-always-on-vpn-deployment"></a>Paso 1. Planear la implementación de VPN de Always On

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Anterior:** Más información sobre el flujo de trabajo para implementar Always On VPN](always-on-vpn-deploy-deployment.md)
- [**Siguiente:** Paso 2. Configurar la infraestructura de servidor](vpn-deploy-server-infrastructure.md)

En este paso, comenzará a planear y preparar la implementación de VPN Always On. Antes de instalar el rol de servidor de acceso remoto en el equipo en el que está planeando usar como servidor VPN, realice las siguientes tareas. Después de la planeación adecuada, puede implementar Always On VPN y, opcionalmente, configurar el acceso condicional para la conectividad VPN mediante Azure AD.

[!INCLUDE [always-on-vpn-standard-config-considerations-include](../../../includes/always-on-vpn-standard-config-considerations-include.md)]

## <a name="prepare-the-remote-access-server"></a>Preparar el servidor de acceso remoto

Debe hacer lo siguiente en el equipo que se usa como servidor VPN:

- Asegúrese **de que el software y la configuración de hardware del servidor VPN son correctos**. Instale Windows Server 2016 en el equipo que va a usar como servidor VPN de acceso remoto. Este servidor debe tener instalados dos adaptadores de red físicos, uno para conectarse a la red perimetral externa y otro para conectarse a la red perimetral interna.

- **Identifique qué adaptador de red se conecta a Internet y qué adaptador de red se conecta a la red privada**. Configure el adaptador de red orientado a Internet con una dirección IP pública, mientras que el adaptador orientado a la intranet puede usar una dirección IP de la red local.

    >[!TIP]
    >Si prefiere no usar una dirección IP pública en la red perimetral, puede configurar el firewall perimetral con una dirección IP pública y, a continuación, configurar el firewall para que reenvíe las solicitudes de conexión VPN al servidor VPN.

- **Conecte el servidor VPN a la red**. Instale el servidor VPN en una red perimetral, entre el firewall perimetral y el firewall perimetral.

## <a name="plan-authentication-methods"></a>Planear métodos de autenticación

IKEv2 es un protocolo de túnel VPN que se describe en [solicitud de fuerza de la tarea de ingeniería de Internet para los comentarios 7296](https://datatracker.ietf.org/doc/rfc7296/). La principal ventaja de IKEv2 es que tolera las interrupciones en la conexión de red subyacente. Por ejemplo, si se produce una pérdida temporal de conexión o si un usuario mueve un equipo cliente de una red a otra, al volver a establecer la conexión de red IKEv2 restaura la conexión VPN automáticamente, sin la intervención del usuario.

>[!TIP]
>Puede configurar el servidor VPN de acceso remoto para que admita conexiones IKEv2 y, al mismo tiempo, deshabilitar los protocolos no utilizados, lo que reduce la superficie de seguridad del servidor. 

## <a name="plan-ip-addresses-for-remote-clients"></a>Planear direcciones IP para clientes remotos

Puede configurar el servidor VPN para asignar direcciones a clientes VPN desde un grupo de direcciones estáticas que configure o direcciones IP desde un servidor DHCP. 

## <a name="prepare-the-environment"></a>Preparar el entorno

- Asegúrese de que **tiene permisos para configurar el firewall externo y que tiene una dirección IP pública válida**. Abra puertos en el firewall para admitir conexiones VPN de IKEv2. También necesita una dirección IP pública para aceptar conexiones de clientes externos.

- **Elija un intervalo de direcciones IP estáticas para los clientes VPN**. Determine el número máximo de clientes VPN simultáneos que desea admitir. Además, planee un intervalo de direcciones IP estáticas en la red perimetral interna para satisfacer ese requisito, es decir, el *grupo de direcciones estáticas*. Si usa DHCP para proporcionar direcciones IP en la red perimetral interna, es posible que también tenga que crear una exclusión para estas direcciones IP estáticas en DHCP.

- **Asegúrese de que puede editar la zona DNS pública**. Agregue registros DNS a su dominio DNS público para admitir la infraestructura de VPN. 

- Asegúrese **de que todos los usuarios de VPN tengan cuentas de usuario en Active Directory usuario (AD DS)** . Para que los usuarios puedan conectarse a la red con conexiones VPN, deben tener cuentas de usuario en AD DS.

## <a name="prepare-routing-and-firewall"></a>Preparar el enrutamiento y el Firewall 

Instale el servidor VPN dentro de la red perimetral, que crea particiones de la red perimetral en redes perimetrales internas y externas. Dependiendo de su entorno de red, es posible que deba realizar varias modificaciones de enrutamiento.

- **Opta Configurar el reenvío de puertos.** El firewall perimetral debe abrir los puertos y los ID. de protocolo asociados a una VPN de IKEv2 y reenviarlos al servidor VPN. En la mayoría de los entornos, para ello es necesario configurar el reenvío de puertos. Redirija los puertos 500 y 4500 del Protocolo de datagramas universal (UDP) al servidor VPN.

- **Configure el enrutamiento para que los servidores DNS y los servidores VPN puedan tener acceso a Internet**. Esta implementación usa IKEv2 y la traducción de direcciones de red (NAT). Asegúrese de que el servidor VPN puede tener acceso a todas las redes internas y los recursos de red necesarios. No se puede tener acceso a cualquier red o recurso que no sea accesible desde el servidor VPN a través de conexiones VPN desde ubicaciones remotas.

En la mayoría de los entornos, para llegar a la nueva red perimetral interna, ajuste las rutas estáticas en el firewall perimetral y el servidor VPN. Sin embargo, en entornos más complejos, es posible que tenga que agregar rutas estáticas a enrutadores internos o ajustar las reglas de Firewall internas para el servidor VPN y el bloque de direcciones IP asociadas a los clientes de VPN.

## <a name="next-steps"></a>Pasos siguientes

[Paso 2. Configurar la infraestructura de servidor](vpn-deploy-server-infrastructure.md): en este paso, instalará y configurará los componentes del lado servidor necesarios para admitir la VPN. Los componentes del lado servidor incluyen la configuración de la PKI para distribuir los certificados usados por los usuarios, el servidor VPN y el servidor NPS.