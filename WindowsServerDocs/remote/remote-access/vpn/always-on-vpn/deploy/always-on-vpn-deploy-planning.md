---
title: Planear la implementación de VPN de Always On
description: Este tema proporciona instrucciones de planeación para la implementación de VPN de Always On en Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: 3c9de3ec-4bbd-4db0-b47a-03507a315383
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 11/05/2018
ms.openlocfilehash: d629f04abda0ce22deb75e487f5b485f50a60a53
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881916"
---
# <a name="step-1-plan-the-always-on-vpn-deployment"></a>Paso 1. Planear la implementación de VPN de Always On

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10


&#171;  [**Anterior:** Obtenga información sobre el flujo de trabajo para la implementación de VPN de Always On](always-on-vpn-deploy-deployment.md)<br>
&#187;  [**próximo:** Paso 2. Configurar la infraestructura de servidor](vpn-deploy-server-infrastructure.md)

En este paso, primero planear y preparar la implementación de VPN de Always On. Antes de instalar el rol de servidor de acceso remoto en el equipo que tiene previsto usar como un servidor VPN, realice las tareas siguientes. Después de la planeación adecuada, puede implementar la VPN de Always On y, opcionalmente, configure el acceso condicional para la conectividad VPN con Azure AD. 

[!INCLUDE [always-on-vpn-standard-config-considerations-include](../../../includes/always-on-vpn-standard-config-considerations-include.md)]


## <a name="prepare-the-remote-access-server"></a>Preparar el servidor de acceso remoto

Debe hacer lo siguiente en el equipo que se usa como un servidor VPN: 

- **Asegúrese de que es correcta la configuración de hardware y software de servidor VPN**. Instale Windows Server 2016 en el equipo que va a usar como servidor VPN de acceso remoto. Este servidor debe tener instalados dos adaptadores de red físicos, uno para conectarse a la red de perímetro externo y uno para conectarse a la red de perímetro interno.

- **Identificar qué adaptador de red se conecta a Internet y qué adaptador de red se conecta a la red privada**. Configurar el adaptador de red accesible desde Internet con una dirección IP pública, mientras que el adaptador accesible desde la Intranet puede usar una dirección IP de la red local.

    >[!TIP]
    >Si prefiere no usar una dirección IP pública en la red perimetral, puede configurar el Firewall perimetral con una dirección IP pública y, a continuación, configure el firewall para que reenvíe las solicitudes de conexión VPN al servidor VPN.

- **Conectar el servidor VPN a la red**. Instale al servidor VPN en una red perimetral, entre el borde y el firewall perimetral.

## <a name="plan-authentication-methods"></a>Planear métodos de autenticación

IKEv2 es se describe en el protocolo de túnel VPN [Internet Engineering Task Force solicitar para comentarios 7296](https://datatracker.ietf.org/doc/rfc7296/). La ventaja principal de IKEv2 es que tolera interrupciones en la conexión de red subyacente. Por ejemplo, si una pérdida temporal de la conexión o si un usuario mueve a un equipo cliente desde una red a otra, al volver a establecer la conexión de red IKEv2 restaura la conexión VPN automáticamente, sin intervención del usuario.

>[!TIP]
>Puede configurar el servidor de acceso remoto VPN para admitir las conexiones IKEv2 al también deshabilitar protocolos no utilizados, lo que reduce la superficie de seguridad del servidor. 

## <a name="plan-ip-addresses-for-remote-clients"></a>Planear las direcciones IP de los clientes remotos

Puede configurar el servidor VPN para asignar direcciones a los clientes VPN desde un grupo de direcciones estáticas que configure o direcciones IP de un servidor DHCP. 

## <a name="prepare-the-environment"></a>Preparar el entorno

- **Asegúrese de que tiene permisos para configurar el firewall externo y que tienen una dirección IP pública válida**. Abrir puertos en el firewall para admitir conexiones VPN de IKEv2. También necesita una dirección IP pública para aceptar conexiones desde clientes externos.

- **Elija un intervalo de direcciones IP estáticas para los clientes VPN**. Determinar el número máximo de clientes simultáneos de VPN que desea admitir. También, planee un intervalo de direcciones IP estáticas en la red de perímetro interno para satisfacer este requisito, es decir, el *grupo de direcciones estáticas*. Si usa DHCP para suministrar direcciones IP en la red Perimetral interna, es posible que también deberá crear una exclusión para esas direcciones IP estáticas en DHCP.

- **Asegúrese de que se puede modificar la zona DNS pública**. Agregar registros DNS a su dominio DNS público para admitir la infraestructura VPN. 

- **Asegúrese de que todos los usuarios VPN tienen cuentas de usuario en Active Directory User \(AD DS\)**. Antes de que los usuarios pueden conectarse a la red con conexiones VPN, deben tener cuentas de usuario en AD DS.

## <a name="prepare-routing-and-firewall"></a>Preparación de enrutamiento y de Firewall 

Instale al servidor VPN dentro de la red perimetral, lo cual particiona la red perimetral en redes perimetrales internos y externos. Dependiendo de su entorno de red, debe realizar varias modificaciones de enrutamientos.

- **\(Opcional\) configurar reenvío de puerto.** El firewall perimetral debe abrir los puertos y protocolos identificadores asociados con una VPN de IKEv2 y reenviarlos al servidor VPN. En la mayoría de los entornos, si lo hace por lo que requiere que configure el reenvío de puerto. Redirigir los puertos 500 y 4500 de protocolo de datagramas Universal (UDP) en el servidor VPN.

- **Configurar el enrutamiento para que los servidores DNS y servidores VPN pueden acceder a Internet**. Esta implementación usa IKEv2 y la traducción de direcciones de red \(NAT\). Asegúrese de que el servidor VPN puede llegar a todas las redes internas necesarias y los recursos de red. Cualquier recurso accesible desde el servidor VPN o red también es inaccesible a través de conexiones de VPN desde ubicaciones remotas.

En la mayoría de los entornos, para llegar a la nueva red de perímetro interno, ajuste las rutas estáticas en el firewall perimetral y el servidor VPN. Sin embargo, en entornos más complejos, es posible que necesita agregar rutas estáticas a enrutadores internos o ajustar las reglas de firewall interno para el servidor VPN y el bloque de direcciones IP asociadas con los clientes de VPN.

## <a name="next-step"></a>Paso siguiente
[Paso 2. Configurar la infraestructura de servidor](vpn-deploy-server-infrastructure.md): En este paso, instale y configure los componentes de servidor necesarios para admitir la VPN. Los componentes de servidor incluyen la configuración de PKI para distribuir los certificados usados por los usuarios, el servidor VPN y el servidor NPS. 

---