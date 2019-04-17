---
title: Planear la implementación de VPN de Always On
description: Este tema proporciona instrucciones de diseño para la implementación de VPN siempre activada en Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: 3c9de3ec-4bbd-4db0-b47a-03507a315383
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 11/05/2018
ms.openlocfilehash: d629f04abda0ce22deb75e487f5b485f50a60a53
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067487"
---
# Paso 1. Planear la implementación de VPN siempre activada

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10


& #171;  [ **Anterior:** más información sobre el flujo de trabajo para la implementación de VPN siempre activada](always-on-vpn-deploy-deployment.md)<br>
& #187;  [ **Siguiente:** paso 2. Configurar la infraestructura de servidor](vpn-deploy-server-infrastructure.md)

En este paso, se iniciará planear y preparar la implementación de VPN siempre activada. Antes de instalar el rol de servidor de acceso remoto en el equipo que tienes previsto sobre el uso de un servidor VPN, realizar las siguientes tareas. Después de una planificación adecuada, puedes implementar VPN siempre activada y, opcionalmente, configurar el acceso condicional para la conectividad VPN con Azure AD. 

[!INCLUDE [always-on-vpn-standard-config-considerations-include](../../../includes/always-on-vpn-standard-config-considerations-include.md)]


## Preparar el servidor de acceso remoto

Debes hacer lo siguiente en el equipo que se usa como un servidor VPN: 

- **Asegúrate de que el software de servidor VPN y configuración de hardware es correcta**. Instale Windows Server 2016 en el equipo que vas a usar como un servidor VPN de acceso remoto. Este servidor debe tener dos adaptadores de red física instalados, uno para conectarse a la red perimetral externo y otro para conectarse a la red perimetral interno.

- **Identificar qué adaptador de red que conecta a Internet y a qué adaptador de red se conecta a tu privada red**. Configurar el adaptador de red orientado a Internet con una dirección IP pública, mientras que el adaptador orientada a la Intranet puede usar una dirección IP de la red local.

    >[!TIP]
    >Si prefieres no usar una dirección IP pública de la red perimetral, puedes configurar el Firewall de Edge con una dirección IP pública y, a continuación, configurar el firewall para reenviar las solicitudes de conexión VPN para el servidor VPN.

- **Conectar el servidor VPN a la red**. Instalar al servidor VPN en una red perimetral, entre el firewall de edge y el firewall del perímetro.

## Planear métodos de autenticación

IKEv2 es un protocolo que se describe en la [Solicitud para Comments7296 de Internet de ingeniería Task Force](https://datatracker.ietf.org/doc/rfc7296/)de túnel de VPN. La principal ventaja de IKEv2 es que tolera interrupciones en la conexión de red subyacente. Por ejemplo, si una pérdida temporal de conexión o si un usuario desplaza un equipo cliente desde una red a otra, al volver a establecer la conexión de red IKEv2 restablecerá la conexión VPN automáticamente, sin la intervención del usuario.

>[!TIP]
>Puedes configurar el servidor VPN de acceso remoto para admitir las conexiones IKEv2 al deshabilitar también protocolos no utilizados, lo que reduce la superficie de seguridad del servidor. 

## Planear las direcciones IP para los clientes remotos

Puedes configurar el servidor VPN para asignar direcciones a los clientes VPN de un conjunto de direcciones estáticas que configurar o direcciones IP de un servidor DHCP. 

## Preparar el entorno

- **Asegúrate de que tienes permisos para configurar el firewall externo y que tiene una dirección IP pública válida**. Puertos abiertos en el firewall para admitir conexiones VPN IKEv2. También debes una dirección IP pública que acepte las conexiones de los clientes externos.

- **Elegir un intervalo de direcciones IP estáticas para los clientes VPN**. Determinar el número máximo de clientes VPN simultáneos que quieres admitir. Además, planea un intervalo de direcciones IP estáticas en la red perimetral interno para cumplir este requisito, es decir, el *conjunto de direcciones estáticas*. Si usas DHCP para proporcionar direcciones IP en el de DMZ interno, es posible que también debes crear una exclusión para las direcciones IP estáticas en DHCP.

- **Asegúrate de que se puede modificar la zona DNS pública**. Agrega los registros de DNS al dominio DNS público para admitir la infraestructura VPN. 

- **Asegúrate de que todos los usuarios VPN tienen cuentas de usuario en usuario de Active Directory \(AD DS\)**. Antes de que los usuarios pueden conectarse a la red con conexiones VPN, deben tener cuentas de usuario en ADDS.

## Preparar el enrutamiento y Firewall 

Instalar al servidor VPN dentro de la red perimetral, que divide la red perimetral en redes perimetrales internos y externos. En función del entorno de red, es posible que debes realizar modificaciones de enrutamiento varios.

- **Reenvío de puerto de \(Optional\) configurar.** El firewall de edge debe abrir los puertos y protocolos identificadores asociados a una VPN IKEv2 y reenviarlos al servidor VPN. En la mayoría de los entornos, esto requiere que configurar el reenvío de puerto. Redirigir los puertos de protocolo de datagramas Universal (UDP) 500 y 4500 con el servidor VPN.

- **Configurar el enrutamiento para que los servidores DNS y servidores VPN pueden conectarse a Internet**. Esta implementación usa IKEv2 y traducción de direcciones de red \(NAT\). Asegúrate de que el servidor VPN puede llegar a todas las redes internas necesarias y los recursos de red. Cualquier red o el recurso no se puede alcanzar desde el servidor VPN también es accesible a través de conexiones de VPN desde ubicaciones remotas.

En la mayoría de los entornos para llegar a la nueva red perimetral interno, ajustar rutas estáticas en el firewall de edge y el servidor VPN. En entornos más complejos, sin embargo, es posible que debes agregar rutas estáticas a enrutadores internos o ajustar las reglas de firewall interno para el servidor VPN y el bloque de direcciones IP asociadas con los clientes VPN.

## Paso siguiente
Paso 2 de [. Configurar la infraestructura de servidor](vpn-deploy-server-infrastructure.md): en este paso, instalar y configurar los componentes del lado servidor necesarios para admitir la VPN. Los componentes del lado servidor incluyen la configuración de PKI para distribuir los certificados usados por los usuarios, el servidor VPN y el servidor NPS. 

---