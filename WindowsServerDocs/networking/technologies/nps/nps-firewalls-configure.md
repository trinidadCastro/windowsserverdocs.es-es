---
title: Configuración del firewall para el tráfico RADIUS
description: En este tema se proporciona información general sobre cómo configurar firewalls para permitir el tráfico RADIUS para el servidor de directivas de redes en Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: 58cca2b2-4ef3-4a09-a614-8bdc08d24f15
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: e9c89312c267d2344ecfc3b48a5f6e050c353093
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97946821"
---
# <a name="configure-firewalls-for-radius-traffic"></a>Configuración del firewall para el tráfico RADIUS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Los firewalls se pueden configurar para permitir o bloquear tipos de tráfico IP en el equipo o dispositivo en el que se ejecuta el firewall. Si los firewall no se configuran correctamente para permitir el tráfico RADIUS entre clientes RADIUS, proxy RADIUS y servidores RADIUS, pueden producirse errores en la autenticación del acceso a la red e impedir a los usuarios el acceso a los recursos de red.

Es posible que tenga que configurar dos tipos de firewalls para permitir el tráfico RADIUS:

- Firewall de Windows Defender con seguridad avanzada en el servidor local que ejecuta el servidor de directivas de redes (NPS).
- Los firewalls que se ejecuten en otros equipos o dispositivos de hardware

## <a name="windows-firewall-on-the-local-nps"></a>Firewall de Windows en el NPS local

De forma predeterminada, NPS envía y recibe tráfico RADIUS mediante los \( \) puertos UDP 1812, 1813, 1645 y 1646 del Protocolo de datagramas de usuario. El Firewall de Windows Defender en el NPS se configura automáticamente con excepciones, durante la instalación de NPS, para permitir el envío y la recepción de este tráfico RADIUS.

Por lo tanto, si usa los puertos UDP predeterminados, no es necesario cambiar la configuración del firewall de Windows Defender para permitir el tráfico RADIUS hacia y desde NPSs.

En algunos casos, puede que desee cambiar los puertos que NPS usa para el tráfico RADIUS. Si configura NPS y los servidores de acceso a la red para enviar y recibir tráfico RADIUS en puertos distintos de los predeterminados, debe realizar las siguientes acciones:

- Quite las excepciones que permiten el tráfico RADIUS en los puertos predeterminados.
- Cree nuevas excepciones que permitan el tráfico RADIUS en los nuevos puertos.

Para obtener más información, consulte [configurar la información del puerto UDP de NPS](nps-udp-ports-configure.md).

## <a name="other-firewalls"></a>Otros firewalls

En la configuración más común, el Firewall está conectado a Internet y NPS es un recurso de intranet que está conectado a la red perimetral.

Para llegar al controlador de dominio dentro de la intranet, el NPS podría tener:

- Una interfaz en la red perimetral y una interfaz en la intranet (el enrutamiento IP no está habilitado).
- Una sola interfaz en la red perimetral. En esta configuración, NPS se comunica con los controladores de dominio a través de otro firewall que conecta la red perimetral a la intranet.

## <a name="configuring-the-internet-firewall"></a>Configuración del firewall de Internet

El firewall que está conectado a Internet debe configurarse con filtros de entrada y salida en su interfaz de Internet \( y, opcionalmente, su interfaz de perímetro \) de red, para permitir el reenvío de mensajes RADIUS entre los clientes NPS y RADIUS o los servidores proxy de Internet. Se pueden usar otros filtros para permitir el paso de tráfico a los servidores web, servidores VPN y otros tipos de servidores de la red perimetral.

Se pueden configurar filtros de paquetes de entrada y salida independientes en la interfaz de Internet y la interfaz de la red perimetral.

### <a name="configure-input-filters-on-the-internet-interface"></a>Configurar filtros de entrada en la interfaz de Internet

Configure los siguientes filtros de paquetes de entrada en la interfaz de Internet del firewall para permitir los siguientes tipos de tráfico:

- Dirección IP de destino de la interfaz de red perimetral y puerto UDP de destino 1812 (0x714) del NPS.  Este filtro permite el tráfico de autenticación RADIUS de los clientes RADIUS basados en Internet al NPS. Este es el puerto UDP predeterminado que usa NPS, tal como se define en RFC 2865. Si usa un puerto diferente, sustituya el número de puerto por 1812.
- Dirección IP de destino de la interfaz de red perimetral y puerto UDP de destino 1813 (0x715) del NPS. Este filtro permite el tráfico de cuentas RADIUS desde clientes RADIUS basados en Internet al NPS. Este es el puerto UDP predeterminado que usa NPS, tal como se define en RFC 2866. Si usa un puerto diferente, sustituya el número de puerto por 1813.
- \(\)Dirección IP de destino opcional de la interfaz de red perimetral y puerto UDP de destino 1645 \( 0X66D \) del NPS. Este filtro permite el tráfico de autenticación RADIUS de los clientes RADIUS basados en Internet al NPS. Éste es el puerto UDP que usan los clientes RADIUS antiguos.
- \(\)Dirección IP de destino opcional de la interfaz de red perimetral y puerto UDP de destino 1646 \( 0X66E \) del NPS. Este filtro permite el tráfico de cuentas RADIUS desde clientes RADIUS basados en Internet al NPS. Éste es el puerto UDP que usan los clientes RADIUS antiguos.

### <a name="configure-output-filters-on-the-internet-interface"></a>Configurar filtros de salida en la interfaz de Internet

Configure los siguientes filtros de salida en la interfaz de Internet del firewall para permitir los siguientes tipos de tráfico:

- Dirección IP de origen de la interfaz de red perimetral y puerto UDP de origen 1812 (0x714) del NPS. Este filtro permite el tráfico de autenticación RADIUS del NPS a los clientes RADIUS basados en Internet. Este es el puerto UDP predeterminado que usa NPS, tal como se define en RFC 2865. Si usa un puerto diferente, sustituya el número de puerto por 1812.
- Dirección IP de origen de la interfaz de red perimetral y puerto UDP de origen 1813 (0x715) del NPS. Este filtro permite el tráfico de cuentas RADIUS desde el NPS a los clientes RADIUS basados en Internet. Este es el puerto UDP predeterminado que usa NPS, tal como se define en RFC 2866. Si usa un puerto diferente, sustituya el número de puerto por 1813.
- \(\)Dirección IP de origen opcional de la interfaz de red perimetral y puerto UDP de origen 1645 \( 0X66D \) del NPS. Este filtro permite el tráfico de autenticación RADIUS del NPS a los clientes RADIUS basados en Internet. Éste es el puerto UDP que usan los clientes RADIUS antiguos.
- \(\)Dirección IP de origen opcional de la interfaz de red perimetral y puerto UDP de origen 1646 \( 0X66E \) del NPS. Este filtro permite el tráfico de cuentas RADIUS desde el NPS a los clientes RADIUS basados en Internet. Éste es el puerto UDP que usan los clientes RADIUS antiguos.

### <a name="configure-input-filters-on-the-perimeter-network-interface"></a>Configurar filtros de entrada en la interfaz de red perimetral

Configure los siguientes filtros de entrada en la interfaz de Internet del firewall para permitir los siguientes tipos de tráfico:

- Dirección IP de origen de la interfaz de red perimetral y puerto UDP de origen 1812 (0x714) del NPS. Este filtro permite el tráfico de autenticación RADIUS del NPS a los clientes RADIUS basados en Internet. Este es el puerto UDP predeterminado que usa NPS, tal como se define en RFC 2865. Si usa un puerto diferente, sustituya el número de puerto por 1812.
- Dirección IP de origen de la interfaz de red perimetral y puerto UDP de origen 1813 (0x715) del NPS. Este filtro permite el tráfico de cuentas RADIUS desde el NPS a los clientes RADIUS basados en Internet. Este es el puerto UDP predeterminado que usa NPS, tal como se define en RFC 2866. Si usa un puerto diferente, sustituya el número de puerto por 1813.
- \(\)Dirección IP de origen opcional de la interfaz de red perimetral y puerto UDP de origen 1645 \( 0X66D \) del NPS. Este filtro permite el tráfico de autenticación RADIUS del NPS a los clientes RADIUS basados en Internet. Éste es el puerto UDP que usan los clientes RADIUS antiguos.
- \(\)Dirección IP de origen opcional de la interfaz de red perimetral y puerto UDP de origen 1646 \( 0X66E \) del NPS. Este filtro permite el tráfico de cuentas RADIUS desde el NPS a los clientes RADIUS basados en Internet. Éste es el puerto UDP que usan los clientes RADIUS antiguos.

### <a name="configure-output-filters-on-the-perimeter-network-interface"></a>Configurar filtros de salida en la interfaz de red perimetral

Configure los siguientes filtros de paquetes de salida en la interfaz de red perimetral del firewall para permitir los siguientes tipos de tráfico:

- Dirección IP de destino de la interfaz de red perimetral y puerto UDP de destino 1812 (0x714) del NPS. Este filtro permite el tráfico de autenticación RADIUS de los clientes RADIUS basados en Internet al NPS. Este es el puerto UDP predeterminado que usa NPS, tal como se define en RFC 2865. Si usa un puerto diferente, sustituya el número de puerto por 1812.
- Dirección IP de destino de la interfaz de red perimetral y puerto UDP de destino 1813 (0x715) del NPS. Este filtro permite el tráfico de cuentas RADIUS desde clientes RADIUS basados en Internet al NPS. Este es el puerto UDP predeterminado que usa NPS, tal como se define en RFC 2866. Si usa un puerto diferente, sustituya el número de puerto por 1813.
- \(\)Dirección IP de destino opcional de la interfaz de red perimetral y puerto UDP de destino 1645 \( 0X66D \) del NPS. Este filtro permite el tráfico de autenticación RADIUS de los clientes RADIUS basados en Internet al NPS. Éste es el puerto UDP que usan los clientes RADIUS antiguos.
- \(\)Dirección IP de destino opcional de la interfaz de red perimetral y puerto UDP de destino 1646 \( 0X66E \) del NPS. Este filtro permite el tráfico de cuentas RADIUS desde clientes RADIUS basados en Internet al NPS. Éste es el puerto UDP que usan los clientes RADIUS antiguos.

Para mayor seguridad, puede usar las direcciones IP de cada cliente RADIUS que envía los paquetes a través del firewall para definir filtros para el tráfico entre el cliente y la dirección IP del NPS en la red perimetral.

### <a name="filters-on-the-perimeter-network-interface"></a>Filtros en la interfaz de red perimetral

Configure los siguientes filtros de paquetes de entrada en la interfaz de red perimetral del firewall de la intranet para permitir los siguientes tipos de tráfico:

- Dirección IP de origen de la interfaz de red perimetral del NPS. Este filtro permite el tráfico del NPS en la red perimetral.

Configure los siguientes filtros de salida en la interfaz de la red perimetral del firewall de la intranet para permitir los siguientes tipos de tráfico:

- Dirección IP de destino de la interfaz de red perimetral del NPS. Este filtro permite el tráfico al NPS en la red perimetral.

### <a name="filters-on-the-intranet-interface"></a>Filtros en la interfaz de la intranet

Configure los siguientes filtros de entrada en la interfaz de intranet del firewall para permitir los siguientes tipos de tráfico:

- Dirección IP de destino de la interfaz de red perimetral del NPS. Este filtro permite el tráfico al NPS en la red perimetral.

Configure los siguientes filtros de paquetes de salida en la interfaz de intranet del firewall para permitir los siguientes tipos de tráfico:

- Dirección IP de origen de la interfaz de red perimetral del NPS. Este filtro permite el tráfico del NPS en la red perimetral.


Para obtener más información sobre la administración de NPS, consulte [administrar el servidor de directivas de redes](nps-manage-top.md).

Para obtener más información acerca de NPS, consulte [servidor de directivas de redes (NPS)](nps-top.md).




