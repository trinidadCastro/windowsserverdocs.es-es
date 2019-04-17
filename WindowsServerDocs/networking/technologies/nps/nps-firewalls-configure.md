---
title: Configurar el firewall para el tráfico RADIUS
description: Este tema proporciona una visión general de cómo configurar el firewall para permitir el tráfico de radio en el servidor de directivas de red en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 58cca2b2-4ef3-4a09-a614-8bdc08d24f15
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 140111e10eabbece098ae9b7c36746cc663c9cce
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="configure-firewalls-for-radius-traffic"></a>Configurar el firewall para el tráfico RADIUS

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Los servidores de seguridad pueden configurarse para permitir o bloquear tipos de tráfico IP hacia y desde el equipo o dispositivo en el que se ejecuta el firewall. Si los firewalls no están configurados correctamente para permitir el tráfico RADIUS entre clientes RADIUS, proxy RADIUS y servidores RADIUS, autenticación de acceso de red puede producir un error, impide que los usuarios acceso a recursos de red. 

Es posible que debas configurar dos tipos de Firewall para permitir el tráfico de radio:

- Firewall de Windows Defender con seguridad avanzada en el servidor local que se ejecuta el servidor de directivas de redes (NPS).
- Servidores de seguridad ejecutando en otros equipos o dispositivos de hardware.

## <a name="windows-firewall-on-the-local-nps-server"></a>Firewall de Windows en el servidor NPS local

De manera predeterminada, NPS envía y recibe el tráfico de RADIUS mediante el protocolo de datagramas de usuario \(UDP\) puertos 1812, 1813, 1645 y 1646. Firewall de Windows Defender en el servidor NPS se configura automáticamente con excepciones, durante la instalación de NPS para permitir este tráfico RADIUS enviarse y recibirse.

Por lo tanto, si estás usando los puertos UDP predeterminados, no es necesario cambiar la configuración de Firewall de Windows Defender para permitir el tráfico RADIUS hacia y desde servidores NPS.

En algunos casos, es posible que desee cambiar los puertos que NPS usa para el tráfico RADIUS. Si configuras NPS y los servidores de acceso de red para enviar y recibir tráfico RADIUS en puertos que no sean los valores predeterminados, debes hacer lo siguiente:

- Quitar las excepciones que permiten el tráfico RADIUS en los puertos de forma predeterminada.
- Crea excepciones nuevas que permitan el tráfico de radio en los puertos nuevo.

Para obtener más información, consulta [configurar información del puerto UDP NPS](nps-udp-ports-configure.md).

## <a name="other-firewalls"></a>Otros servidores de seguridad

En la configuración más común, el firewall está conectado a Internet y el servidor NPS es un recurso de intranet que está conectado a la red de perímetro.

Para llegar al controlador de dominio dentro de la intranet, podría tener el servidor NPS:

- Una interfaz en la red perimetral y una interfaz de la intranet (no está habilitado el enrutamiento de IP). 
- Una única interfaz en la red de perímetro. En esta configuración, NPS se comunica con controladores de dominio mediante otro firewall que se conecta a la red perimetral a la intranet.

## <a name="configuring-the-internet-firewall"></a>Configurar el firewall de Internet

El firewall que está conectado a Internet debe estar configurado con los filtros de entrada y salidos en la interfaz de Internet \ (y, opcionalmente, su interface\ perímetro de red) para permitir el reenvío de mensajes RADIUS entre el servidor NPS y clientes de RADIUS o servidores proxy de Internet. Filtros adicionales pueden usarse para permitir que el paso de tráfico a servidores Web, servidores VPN y otros tipos de servidores en la red de perímetro.

Paquetes de entrada y salida filtros pueden configurarse en la interfaz de Internet y la interfaz de red perimetral diferentes.

### <a name="configure-input-filters-on-the-internet-interface"></a>Configurar los filtros de entrada en la interfaz de Internet

Configurar los siguientes filtros de paquetes de entrada en la interfaz de Internet del firewall para permitir a los siguientes tipos de tráfico:

- Dirección IP de destino de la interfaz de red perimetral y puerto UDP de destino 1812 (0 x 714) del servidor NPS.  Este filtro permite el tráfico de autenticación RADIUS de los clientes RADIUS basados en Internet en el servidor NPS. Este es el puerto UDP predeterminado que se usa NPS, definido en RFC 2865. Si estás usando un puerto diferente, se reemplaza por ese número de puerto para 1812.
- Dirección IP de destino de la interfaz de red perimetral y puerto UDP de destino 1813 (0 x 715) del servidor NPS. Este filtro permite el tráfico de contabilidad de RADIUS de los clientes RADIUS basados en Internet en el servidor NPS. Este es el puerto UDP predeterminado que se usa NPS, definido en RFC 2866. Si estás usando un puerto diferente, se reemplaza por ese número de puerto para 1813.
- \(Optional\) dirección IP de destino de la interfaz de red perimetral y puerto UDP de destino de \(0x66D\) 1645 del servidor NPS. Este filtro permite el tráfico de autenticación RADIUS de los clientes RADIUS basados en Internet en el servidor NPS. Este es el puerto UDP que usa los clientes RADIUS más antiguos.
- \(Optional\) dirección IP de destino de la interfaz de red perimetral y puerto UDP de destino de \(0x66E\) 1646 del servidor NPS. Este filtro permite el tráfico de contabilidad de RADIUS de los clientes RADIUS basados en Internet en el servidor NPS. Este es el puerto UDP que usa los clientes RADIUS más antiguos.

### <a name="configure-output-filters-on-the-internet-interface"></a>Configurar los filtros de salida en la interfaz de Internet

Configurar los siguientes filtros de salida en la interfaz de Internet del firewall para permitir a los siguientes tipos de tráfico:

- Dirección IP de origen de la interfaz de red perimetral y puerto UDP origen 1812 (0 x 714) del servidor NPS. Este filtro permite el tráfico de autenticación de RADIUS desde el servidor NPS a los clientes RADIUS basados en Internet. Este es el puerto UDP predeterminado que se usa NPS, definido en RFC 2865. Si estás usando un puerto diferente, se reemplaza por ese número de puerto para 1812.
- Dirección IP de origen de la interfaz de red perimetral y puerto UDP origen 1813 (0 x 715) del servidor NPS. Este filtro permite el tráfico de contabilidad de RADIUS desde el servidor NPS a los clientes RADIUS basados en Internet. Este es el puerto UDP predeterminado que se usa NPS, definido en RFC 2866. Si estás usando un puerto diferente, se reemplaza por ese número de puerto para 1813.
- \(Optional\) dirección IP de origen de la interfaz de red perimetral y puerto UDP de origen de \(0x66D\) 1645 del servidor NPS. Este filtro permite el tráfico de autenticación de RADIUS desde el servidor NPS a los clientes RADIUS basados en Internet. Este es el puerto UDP que usa los clientes RADIUS más antiguos.
- \(Optional\) dirección IP de origen de la interfaz de red perimetral y puerto UDP origen 1646 \(0x66E\) del servidor NPS. Este filtro permite el tráfico de contabilidad de RADIUS desde el servidor NPS a los clientes RADIUS basados en Internet. Este es el puerto UDP que usa los clientes RADIUS más antiguos.

### <a name="configure-input-filters-on-the-perimeter-network-interface"></a>Configurar los filtros de entrada en la interfaz de red perimetral

Configurar los siguientes filtros de entrada en la interfaz de red perimetral del firewall para permitir a los siguientes tipos de tráfico:

- Dirección IP de origen de la interfaz de red perimetral y puerto UDP origen 1812 (0 x 714) del servidor NPS. Este filtro permite el tráfico de autenticación de RADIUS desde el servidor NPS a los clientes RADIUS basados en Internet. Este es el puerto UDP predeterminado que se usa NPS, definido en RFC 2865. Si estás usando un puerto diferente, se reemplaza por ese número de puerto para 1812.
- Dirección IP de origen de la interfaz de red perimetral y puerto UDP origen 1813 (0 x 715) del servidor NPS. Este filtro permite el tráfico de contabilidad de RADIUS desde el servidor NPS a los clientes RADIUS basados en Internet. Este es el puerto UDP predeterminado que se usa NPS, definido en RFC 2866. Si estás usando un puerto diferente, se reemplaza por ese número de puerto para 1813.
- \(Optional\) dirección IP de origen de la interfaz de red perimetral y puerto UDP de origen de \(0x66D\) 1645 del servidor NPS. Este filtro permite el tráfico de autenticación de RADIUS desde el servidor NPS a los clientes RADIUS basados en Internet. Este es el puerto UDP que usa los clientes RADIUS más antiguos.
- \(Optional\) dirección IP de origen de la interfaz de red perimetral y puerto UDP origen 1646 \(0x66E\) del servidor NPS. Este filtro permite el tráfico de contabilidad de RADIUS desde el servidor NPS a los clientes RADIUS basados en Internet. Este es el puerto UDP que usa los clientes RADIUS más antiguos.

### <a name="configure-output-filters-on-the-perimeter-network-interface"></a>Configurar los filtros de salida en la interfaz de red perimetral

Configurar los siguientes filtros de paquete de salida en la interfaz de red perimetral del firewall para permitir a los siguientes tipos de tráfico:

- Dirección IP de destino de la interfaz de red perimetral y puerto UDP de destino 1812 (0 x 714) del servidor NPS. Este filtro permite el tráfico de autenticación RADIUS de los clientes RADIUS basados en Internet en el servidor NPS. Este es el puerto UDP predeterminado que se usa NPS, definido en RFC 2865. Si estás usando un puerto diferente, se reemplaza por ese número de puerto para 1812.
- Dirección IP de destino de la interfaz de red perimetral y puerto UDP de destino 1813 (0 x 715) del servidor NPS. Este filtro permite el tráfico de contabilidad de RADIUS de los clientes RADIUS basados en Internet en el servidor NPS. Este es el puerto UDP predeterminado que se usa NPS, definido en RFC 2866. Si estás usando un puerto diferente, se reemplaza por ese número de puerto para 1813.
- \(Optional\) dirección IP de destino de la interfaz de red perimetral y puerto UDP de destino de \(0x66D\) 1645 del servidor NPS. Este filtro permite el tráfico de autenticación RADIUS de los clientes RADIUS basados en Internet en el servidor NPS. Este es el puerto UDP que usa los clientes RADIUS más antiguos.
- \(Optional\) dirección IP de destino de la interfaz de red perimetral y puerto UDP de destino de \(0x66E\) 1646 del servidor NPS. Este filtro permite el tráfico de contabilidad de RADIUS de los clientes RADIUS basados en Internet en el servidor NPS. Este es el puerto UDP que usa los clientes RADIUS más antiguos.

Para mayor seguridad, puedes usar las direcciones IP de cada cliente RADIUS que envía los paquetes a través del firewall para definir los filtros para el tráfico entre el cliente y la dirección IP del servidor NPS en la red de perímetro.

### <a name="filters-on-the-perimeter-network-interface"></a>Filtros en la interfaz de red perimetral

Configurar los siguientes filtros de paquetes de entrada en la interfaz de red perimetral del firewall de intranet para permitir a los siguientes tipos de tráfico:

- Dirección IP de origen de la interfaz de red perimetral del servidor NPS. Este filtro permite el tráfico del servidor NPS en la red de perímetro.

Configurar los siguientes filtros de salida en la interfaz de red perimetral del firewall de intranet para permitir a los siguientes tipos de tráfico:

- Dirección IP de destino de la interfaz de red perimetral del servidor NPS. Este filtro permite el tráfico en el servidor NPS en la red de perímetro.

### <a name="filters-on-the-intranet-interface"></a>Filtros en la interfaz de intranet

Configurar los siguientes filtros de entrada en la interfaz de intranet del firewall para permitir a los siguientes tipos de tráfico:

- Dirección IP de destino de la interfaz de red perimetral del servidor NPS. Este filtro permite el tráfico en el servidor NPS en la red de perímetro.

Configurar los siguientes filtros de paquete de salida en la interfaz de intranet del firewall para permitir a los siguientes tipos de tráfico:

- Dirección IP de origen de la interfaz de red perimetral del servidor NPS. Este filtro permite el tráfico del servidor NPS en la red de perímetro.


Para obtener más información acerca de cómo administrar NPS, consulta [administrar el servidor de directivas de red](nps-manage-top.md).

Para obtener más información acerca de NPS, consulta [servidor de directivas de redes (NPS)](nps-top.md).




