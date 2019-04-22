---
title: Configuración del firewall para el tráfico RADIUS
description: Este tema proporciona información general sobre cómo configurar los firewalls para permitir el tráfico RADIUS para el servidor de directivas de redes en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 58cca2b2-4ef3-4a09-a614-8bdc08d24f15
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 94b03349f21a40f9bf42508d5a2878a5cf2946cb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819456"
---
# <a name="configure-firewalls-for-radius-traffic"></a>Configuración del firewall para el tráfico RADIUS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Firewalls pueden configurarse para permitir o bloquear tipos de tráfico IP hacia y desde el equipo o dispositivo en el que se está ejecutando el servidor de seguridad. Si los firewalls no están configurados correctamente para permitir el tráfico RADIUS entre clientes RADIUS, proxy RADIUS y servidores RADIUS, autenticación de acceso de red puede producir un error, impide que los usuarios tener acceso a los recursos de red. 

Es posible que deba configurar dos tipos de firewalls para permitir el tráfico RADIUS:

- Firewall de Windows Defender con seguridad avanzada en el servidor local que ejecuta el servidor de directivas de redes (NPS).
- Los firewalls que se ejecutan en otros equipos o dispositivos de hardware.

## <a name="windows-firewall-on-the-local-nps"></a>Firewall de Windows en el NPS local

De forma predeterminada, NPS envía y recibe tráfico RADIUS mediante el protocolo de datagramas de usuario \(UDP\) los puertos 1812, 1813, 1645 y 1646. Firewall de Windows Defender en el NPS se configura automáticamente con excepciones, durante la instalación de NPS para permitir el tráfico RADIUS a ser enviados y recibidos.

Por lo tanto, si usa los puertos UDP predeterminados, no es necesario cambiar la configuración de Firewall de Windows Defender para permitir el tráfico RADIUS NPSs.

En algunos casos, es posible que desea cambiar los puertos que usa NPS para el tráfico RADIUS. Si configura NPS y los servidores de acceso de red para enviar y recibir tráfico RADIUS en puertos distintos de los valores predeterminados, haga lo siguiente:

- Quite las excepciones que permitan el tráfico RADIUS en los puertos predeterminados.
- Cree nuevas excepciones que permitan el tráfico RADIUS en los nuevos puertos.

Para obtener más información, consulte [configurar información de puertos UDP de NPS](nps-udp-ports-configure.md).

## <a name="other-firewalls"></a>Otros firewalls

En la configuración más común, el firewall está conectado a Internet y el NPS es un recurso de intranet que está conectado a la red perimetral.

Para alcanzar el controlador de dominio dentro de la intranet, podría tener el NPS:

- Una interfaz en la red perimetral y una interfaz de la intranet (no está habilitado el enrutamiento IP). 
- Una única interfaz en la red perimetral. En esta configuración, NPS se comunica con los controladores de dominio mediante otro firewall que conecta la red perimetral a la intranet.

## <a name="configuring-the-internet-firewall"></a>Configuración del firewall de Internet

El firewall que esté conectado a Internet debe configurarse con filtros de entrada y salidos en la interfaz de Internet \(y, opcionalmente, su interfaz de red perimetral\), para permitir el reenvío de mensajes RADIUS entre el NPS y los clientes RADIUS o servidores proxy en Internet. Filtros adicionales pueden utilizarse para permitir que el paso de tráfico a servidores Web, servidores VPN y otros tipos de servidores en la red perimetral.

Los filtros se pueden configurar en la interfaz de Internet y la interfaz de red perimetral de paquetes de entrada y salida independientes.

### <a name="configure-input-filters-on-the-internet-interface"></a>Configurar filtros de entrada en la interfaz de Internet

Configure los siguientes filtros de paquetes entrantes en la interfaz de Internet del firewall para permitir a los siguientes tipos de tráfico:

- Dirección IP de destino de la interfaz de red perimetral y puerto UDP de destino 1812 (0 x 714) de NPS.  Este filtro permite el tráfico de autenticación RADIUS de los clientes RADIUS basados en Internet a NPS. Este es el puerto UDP predeterminado que usa NPS, tal como se define en RFC 2865. Si usa un puerto diferente, sustituya ese número de puerto por 1812.
- Dirección IP de destino de la interfaz de red perimetral y puerto UDP de destino 1813 (0 x 715) de NPS. Este filtro permite el tráfico de cuentas RADIUS de los clientes RADIUS basados en Internet a NPS. Este es el puerto UDP predeterminado que usa NPS, tal como se define en RFC 2866. Si usa un puerto diferente, sustituya ese número de puerto para 1813.
- \(Opcional\) dirección IP de destino de la interfaz de red perimetral y puerto UDP de destino de 1645 \(0x66D\) de NPS. Este filtro permite el tráfico de autenticación RADIUS de los clientes RADIUS basados en Internet a NPS. Este es el puerto UDP que usa los clientes RADIUS antiguos.
- \(Opcional\) dirección IP de destino de la interfaz de red perimetral y puerto UDP de destino de 1646 \(0x66E\) de NPS. Este filtro permite el tráfico de cuentas RADIUS de los clientes RADIUS basados en Internet a NPS. Este es el puerto UDP que usa los clientes RADIUS antiguos.

### <a name="configure-output-filters-on-the-internet-interface"></a>Configurar filtros de salida en la interfaz de Internet

Configure los siguientes filtros de salida en la interfaz de Internet del firewall para permitir a los siguientes tipos de tráfico:

- Dirección IP de origen de la interfaz de red perimetral y puerto UDP de origen 1812 (0 x 714) de NPS. Este filtro permite el tráfico de autenticación RADIUS en NPS a los clientes RADIUS basados en Internet. Este es el puerto UDP predeterminado que usa NPS, tal como se define en RFC 2865. Si usa un puerto diferente, sustituya ese número de puerto por 1812.
- Dirección IP de origen de la interfaz de red perimetral y puerto UDP de origen 1813 (0 x 715) de NPS. Este filtro permite el tráfico de cuentas RADIUS en NPS a los clientes RADIUS basados en Internet. Este es el puerto UDP predeterminado que usa NPS, tal como se define en RFC 2866. Si usa un puerto diferente, sustituya ese número de puerto para 1813.
- \(Opcional\) dirección IP de origen de la interfaz de red perimetral y puerto UDP de origen de 1645 \(0x66D\) de NPS. Este filtro permite el tráfico de autenticación RADIUS en NPS a los clientes RADIUS basados en Internet. Este es el puerto UDP que usa los clientes RADIUS antiguos.
- \(Opcional\) dirección IP de origen de la interfaz de red perimetral y puerto UDP de origen de 1646 \(0x66E\) de NPS. Este filtro permite el tráfico de cuentas RADIUS en NPS a los clientes RADIUS basados en Internet. Este es el puerto UDP que usa los clientes RADIUS antiguos.

### <a name="configure-input-filters-on-the-perimeter-network-interface"></a>Configurar filtros de entrada en la interfaz de red perimetral

Configure los siguientes filtros de entrada en la interfaz de red perimetral del firewall para permitir a los siguientes tipos de tráfico:

- Dirección IP de origen de la interfaz de red perimetral y puerto UDP de origen 1812 (0 x 714) de NPS. Este filtro permite el tráfico de autenticación RADIUS en NPS a los clientes RADIUS basados en Internet. Este es el puerto UDP predeterminado que usa NPS, tal como se define en RFC 2865. Si usa un puerto diferente, sustituya ese número de puerto por 1812.
- Dirección IP de origen de la interfaz de red perimetral y puerto UDP de origen 1813 (0 x 715) de NPS. Este filtro permite el tráfico de cuentas RADIUS en NPS a los clientes RADIUS basados en Internet. Este es el puerto UDP predeterminado que usa NPS, tal como se define en RFC 2866. Si usa un puerto diferente, sustituya ese número de puerto para 1813.
- \(Opcional\) dirección IP de origen de la interfaz de red perimetral y puerto UDP de origen de 1645 \(0x66D\) de NPS. Este filtro permite el tráfico de autenticación RADIUS en NPS a los clientes RADIUS basados en Internet. Este es el puerto UDP que usa los clientes RADIUS antiguos.
- \(Opcional\) dirección IP de origen de la interfaz de red perimetral y puerto UDP de origen de 1646 \(0x66E\) de NPS. Este filtro permite el tráfico de cuentas RADIUS en NPS a los clientes RADIUS basados en Internet. Este es el puerto UDP que usa los clientes RADIUS antiguos.

### <a name="configure-output-filters-on-the-perimeter-network-interface"></a>Configurar filtros de salida en la interfaz de red perimetral

Configure los siguientes filtros de paquetes de salida en la interfaz de red perimetral del firewall para permitir a los siguientes tipos de tráfico:

- Dirección IP de destino de la interfaz de red perimetral y puerto UDP de destino 1812 (0 x 714) de NPS. Este filtro permite el tráfico de autenticación RADIUS de los clientes RADIUS basados en Internet a NPS. Este es el puerto UDP predeterminado que usa NPS, tal como se define en RFC 2865. Si usa un puerto diferente, sustituya ese número de puerto por 1812.
- Dirección IP de destino de la interfaz de red perimetral y puerto UDP de destino 1813 (0 x 715) de NPS. Este filtro permite el tráfico de cuentas RADIUS de los clientes RADIUS basados en Internet a NPS. Este es el puerto UDP predeterminado que usa NPS, tal como se define en RFC 2866. Si usa un puerto diferente, sustituya ese número de puerto para 1813.
- \(Opcional\) dirección IP de destino de la interfaz de red perimetral y puerto UDP de destino de 1645 \(0x66D\) de NPS. Este filtro permite el tráfico de autenticación RADIUS de los clientes RADIUS basados en Internet a NPS. Este es el puerto UDP que usa los clientes RADIUS antiguos.
- \(Opcional\) dirección IP de destino de la interfaz de red perimetral y puerto UDP de destino de 1646 \(0x66E\) de NPS. Este filtro permite el tráfico de cuentas RADIUS de los clientes RADIUS basados en Internet a NPS. Este es el puerto UDP que usa los clientes RADIUS antiguos.

Para mayor seguridad, puede usar las direcciones IP de cada cliente RADIUS que envía los paquetes a través del firewall para definir filtros para el tráfico entre el cliente y la dirección IP de NPS en la red perimetral.

### <a name="filters-on-the-perimeter-network-interface"></a>Filtros en la interfaz de red perimetral

Configure los siguientes filtros de paquetes entrantes en la interfaz de red perimetral del firewall para permitir a los siguientes tipos de tráfico de intranet:

- Dirección IP de origen de la interfaz de red perimetral de NPS. Este filtro permite el tráfico de NPS en la red perimetral.

Configure los siguientes filtros de salida en la interfaz de red perimetral del firewall para permitir a los siguientes tipos de tráfico de intranet:

- Dirección IP de destino de la interfaz de red perimetral de NPS. Este filtro permite el tráfico a NPS en la red perimetral.

### <a name="filters-on-the-intranet-interface"></a>Filtros en la interfaz de intranet

Configure los siguientes filtros de entrada en la interfaz de intranet del firewall para permitir a los siguientes tipos de tráfico:

- Dirección IP de destino de la interfaz de red perimetral de NPS. Este filtro permite el tráfico a NPS en la red perimetral.

Configure los siguientes filtros de paquetes de salida en la interfaz de intranet del firewall para permitir a los siguientes tipos de tráfico:

- Dirección IP de origen de la interfaz de red perimetral de NPS. Este filtro permite el tráfico de NPS en la red perimetral.


Para obtener más información sobre la administración de NPS, consulte [administrar un servidor de directivas de redes](nps-manage-top.md).

Para obtener más información acerca de NPS, consulte [servidor de directivas de redes (NPS)](nps-top.md).




