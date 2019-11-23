---
title: Configuración del firewall para el tráfico RADIUS
description: En este tema se proporciona información general sobre cómo configurar firewalls para permitir el tráfico RADIUS para el servidor de directivas de redes en Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 58cca2b2-4ef3-4a09-a614-8bdc08d24f15
ms.author: pashort
author: shortpatti
ms.openlocfilehash: eb7f5c495fa09aed584ef37d668d1fa232d7a497
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405450"
---
# <a name="configure-firewalls-for-radius-traffic"></a>Configuración del firewall para el tráfico RADIUS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Los firewalls se pueden configurar para permitir o bloquear los tipos de tráfico IP hacia y desde el equipo o dispositivo en el que se ejecuta el firewall. Si los firewalls no están configurados correctamente para permitir el tráfico RADIUS entre clientes RADIUS, proxy RADIUS y servidores RADIUS, se puede producir un error en la autenticación de acceso a la red, lo que impide que los usuarios tengan acceso a los recursos de red. 

Es posible que tenga que configurar dos tipos de firewalls para permitir el tráfico RADIUS:

- Firewall de Windows Defender con seguridad avanzada en el servidor local que ejecuta el servidor de directivas de redes (NPS).
- Firewalls que se ejecutan en otros equipos o dispositivos de hardware.

## <a name="windows-firewall-on-the-local-nps"></a>Firewall de Windows en el NPS local

De forma predeterminada, NPS envía y recibe tráfico RADIUS mediante el protocolo de datagramas de usuario \(puertos UDP\) 1812, 1813, 1645 y 1646. El Firewall de Windows Defender en el NPS se configura automáticamente con excepciones, durante la instalación de NPS, para permitir el envío y la recepción de este tráfico RADIUS.

Por lo tanto, si usa los puertos UDP predeterminados, no es necesario cambiar la configuración del firewall de Windows Defender para permitir el tráfico RADIUS hacia y desde NPSs.

En algunos casos, es posible que desee cambiar los puertos que usa NPS para el tráfico RADIUS. Si configura NPS y los servidores de acceso a la red para enviar y recibir tráfico RADIUS en puertos distintos de los predeterminados, debe hacer lo siguiente:

- Quite las excepciones que permiten el tráfico RADIUS en los puertos predeterminados.
- Cree nuevas excepciones que permitan el tráfico RADIUS en los nuevos puertos.

Para obtener más información, consulte [configurar la información del puerto UDP de NPS](nps-udp-ports-configure.md).

## <a name="other-firewalls"></a>Otros firewalls

En la configuración más común, el Firewall está conectado a Internet y NPS es un recurso de intranet que está conectado a la red perimetral.

Para llegar al controlador de dominio dentro de la intranet, el NPS podría tener:

- Una interfaz en la red perimetral y una interfaz en la intranet (el enrutamiento IP no está habilitado). 
- Una sola interfaz en la red perimetral. En esta configuración, NPS se comunica con los controladores de dominio a través de otro firewall que conecta la red perimetral a la intranet.

## <a name="configuring-the-internet-firewall"></a>Configuración del firewall de Internet

El firewall que está conectado a Internet debe configurarse con filtros de entrada y salida en su interfaz de Internet \(y, opcionalmente, su interfaz de perímetro de red\), para permitir el reenvío de mensajes RADIUS entre los clientes NPS y RADIUS o los servidores proxy de Internet. Se pueden usar filtros adicionales para permitir el paso del tráfico a los servidores Web, los servidores VPN y otros tipos de servidores de la red perimetral.

Se pueden configurar filtros de paquetes de entrada y salida independientes en la interfaz de Internet y en la interfaz de red perimetral.

### <a name="configure-input-filters-on-the-internet-interface"></a>Configurar filtros de entrada en la interfaz de Internet

Configure los siguientes filtros de paquetes de entrada en la interfaz de Internet del firewall para permitir los siguientes tipos de tráfico:

- Dirección IP de destino de la interfaz de red perimetral y puerto UDP de destino 1812 (0x714) del NPS.  Este filtro permite el tráfico de autenticación RADIUS de los clientes RADIUS basados en Internet al NPS. Este es el puerto UDP predeterminado que usa NPS, tal como se define en RFC 2865. Si usa un puerto diferente, sustituya el número de puerto por 1812.
- Dirección IP de destino de la interfaz de red perimetral y puerto UDP de destino 1813 (0x715) del NPS. Este filtro permite el tráfico de cuentas RADIUS desde clientes RADIUS basados en Internet al NPS. Este es el puerto UDP predeterminado que usa NPS, tal como se define en RFC 2866. Si usa un puerto diferente, sustituya el número de puerto por 1813.
- \(dirección IP de destino\) opcional de la interfaz de red perimetral y el puerto de destino UDP 1645 \(0x66D\) de NPS. Este filtro permite el tráfico de autenticación RADIUS de los clientes RADIUS basados en Internet al NPS. Este es el puerto UDP que usan los clientes RADIUS más antiguos.
- \(dirección IP de destino\) opcional de la interfaz de red perimetral y el puerto de destino UDP 1646 \(0x66E\) de NPS. Este filtro permite el tráfico de cuentas RADIUS desde clientes RADIUS basados en Internet al NPS. Este es el puerto UDP que usan los clientes RADIUS más antiguos.

### <a name="configure-output-filters-on-the-internet-interface"></a>Configurar filtros de salida en la interfaz de Internet

Configure los siguientes filtros de salida en la interfaz de Internet del firewall para permitir los siguientes tipos de tráfico:

- Dirección IP de origen de la interfaz de red perimetral y puerto UDP de origen 1812 (0x714) del NPS. Este filtro permite el tráfico de autenticación RADIUS del NPS a los clientes RADIUS basados en Internet. Este es el puerto UDP predeterminado que usa NPS, tal como se define en RFC 2865. Si usa un puerto diferente, sustituya el número de puerto por 1812.
- Dirección IP de origen de la interfaz de red perimetral y puerto UDP de origen 1813 (0x715) del NPS. Este filtro permite el tráfico de cuentas RADIUS desde el NPS a los clientes RADIUS basados en Internet. Este es el puerto UDP predeterminado que usa NPS, tal como se define en RFC 2866. Si usa un puerto diferente, sustituya el número de puerto por 1813.
- \(dirección IP de origen de\) opcional de la interfaz de red perimetral y el puerto de origen UDP 1645 \(0x66D\) de NPS. Este filtro permite el tráfico de autenticación RADIUS del NPS a los clientes RADIUS basados en Internet. Este es el puerto UDP que usan los clientes RADIUS más antiguos.
- \(dirección IP de origen de\) opcional de la interfaz de red perimetral y el puerto de origen UDP 1646 \(0x66E\) de NPS. Este filtro permite el tráfico de cuentas RADIUS desde el NPS a los clientes RADIUS basados en Internet. Este es el puerto UDP que usan los clientes RADIUS más antiguos.

### <a name="configure-input-filters-on-the-perimeter-network-interface"></a>Configurar filtros de entrada en la interfaz de red perimetral

Configure los siguientes filtros de entrada en la interfaz de red perimetral del firewall para permitir los siguientes tipos de tráfico:

- Dirección IP de origen de la interfaz de red perimetral y puerto UDP de origen 1812 (0x714) del NPS. Este filtro permite el tráfico de autenticación RADIUS del NPS a los clientes RADIUS basados en Internet. Este es el puerto UDP predeterminado que usa NPS, tal como se define en RFC 2865. Si usa un puerto diferente, sustituya el número de puerto por 1812.
- Dirección IP de origen de la interfaz de red perimetral y puerto UDP de origen 1813 (0x715) del NPS. Este filtro permite el tráfico de cuentas RADIUS desde el NPS a los clientes RADIUS basados en Internet. Este es el puerto UDP predeterminado que usa NPS, tal como se define en RFC 2866. Si usa un puerto diferente, sustituya el número de puerto por 1813.
- \(dirección IP de origen de\) opcional de la interfaz de red perimetral y el puerto de origen UDP 1645 \(0x66D\) de NPS. Este filtro permite el tráfico de autenticación RADIUS del NPS a los clientes RADIUS basados en Internet. Este es el puerto UDP que usan los clientes RADIUS más antiguos.
- \(dirección IP de origen de\) opcional de la interfaz de red perimetral y el puerto de origen UDP 1646 \(0x66E\) de NPS. Este filtro permite el tráfico de cuentas RADIUS desde el NPS a los clientes RADIUS basados en Internet. Este es el puerto UDP que usan los clientes RADIUS más antiguos.

### <a name="configure-output-filters-on-the-perimeter-network-interface"></a>Configurar filtros de salida en la interfaz de red perimetral

Configure los siguientes filtros de paquetes de salida en la interfaz de red perimetral del firewall para permitir los siguientes tipos de tráfico:

- Dirección IP de destino de la interfaz de red perimetral y puerto UDP de destino 1812 (0x714) del NPS. Este filtro permite el tráfico de autenticación RADIUS de los clientes RADIUS basados en Internet al NPS. Este es el puerto UDP predeterminado que usa NPS, tal como se define en RFC 2865. Si usa un puerto diferente, sustituya el número de puerto por 1812.
- Dirección IP de destino de la interfaz de red perimetral y puerto UDP de destino 1813 (0x715) del NPS. Este filtro permite el tráfico de cuentas RADIUS desde clientes RADIUS basados en Internet al NPS. Este es el puerto UDP predeterminado que usa NPS, tal como se define en RFC 2866. Si usa un puerto diferente, sustituya el número de puerto por 1813.
- \(dirección IP de destino\) opcional de la interfaz de red perimetral y el puerto de destino UDP 1645 \(0x66D\) de NPS. Este filtro permite el tráfico de autenticación RADIUS de los clientes RADIUS basados en Internet al NPS. Este es el puerto UDP que usan los clientes RADIUS más antiguos.
- \(dirección IP de destino\) opcional de la interfaz de red perimetral y el puerto de destino UDP 1646 \(0x66E\) de NPS. Este filtro permite el tráfico de cuentas RADIUS desde clientes RADIUS basados en Internet al NPS. Este es el puerto UDP que usan los clientes RADIUS más antiguos.

Para mayor seguridad, puede usar las direcciones IP de cada cliente RADIUS que envía los paquetes a través del firewall para definir filtros para el tráfico entre el cliente y la dirección IP del NPS en la red perimetral.

### <a name="filters-on-the-perimeter-network-interface"></a>Filtros en la interfaz de red perimetral

Configure los siguientes filtros de paquetes de entrada en la interfaz de red perimetral del firewall de la intranet para permitir los siguientes tipos de tráfico:

- Dirección IP de origen de la interfaz de red perimetral del NPS. Este filtro permite el tráfico del NPS en la red perimetral.

Configure los siguientes filtros de salida en la interfaz de red perimetral del firewall de la intranet para permitir los siguientes tipos de tráfico:

- Dirección IP de destino de la interfaz de red perimetral del NPS. Este filtro permite el tráfico al NPS en la red perimetral.

### <a name="filters-on-the-intranet-interface"></a>Filtros en la interfaz de la intranet

Configure los siguientes filtros de entrada en la interfaz de la intranet del firewall para permitir los siguientes tipos de tráfico:

- Dirección IP de destino de la interfaz de red perimetral del NPS. Este filtro permite el tráfico al NPS en la red perimetral.

Configure los siguientes filtros de paquetes de salida en la interfaz de intranet del firewall para permitir los siguientes tipos de tráfico:

- Dirección IP de origen de la interfaz de red perimetral del NPS. Este filtro permite el tráfico del NPS en la red perimetral.


Para obtener más información sobre la administración de NPS, consulte [administrar el servidor de directivas de redes](nps-manage-top.md).

Para obtener más información acerca de NPS, consulte [servidor de directivas de redes (NPS)](nps-top.md).




