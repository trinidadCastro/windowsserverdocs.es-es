---
title: Configuración de NPS en un equipo host múltiple
description: Este tema proporciona instrucciones sobre cómo configurar un servidor con varios adaptadores de red que se ejecuta el servidor de directivas de red en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d9d9e9ac-4859-4522-89ed-a23092c9e12a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f80e83a4d79036729b6b442e6362d52fbda12edd
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="configure-nps-on-a-multihomed-computer"></a>Configuración de NPS en un equipo host múltiple

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para configurar un servidor NPS con varios adaptadores de red.

Cuando usas varios adaptadores de red en un servidor que ejecuta el servidor de directivas de redes (NPS), puedes configurar lo siguiente:

- Los adaptadores de red que no y enviar y recibir tráfico \(RADIUS\) servicio autenticación remota telefónica de usuario.
- Si en una base de adaptador de red, NPS supervisa el tráfico de radio en el protocolo de Internet versión 4 \(IPv4\), IPv6, o direcciones IPv4 e IPv6.
- Los números de puerto UDP sobre qué RADIUS tráfico se envía y recibe en función del protocolo \ (IPv4 o IPv6\), cada de adaptador de red.

De manera predeterminada, NPS escucha el tráfico RADIUS en puertos 1812, 1813, 1645 y 1646 IPv6 e IPv4 para todos los adaptadores de red instalados. Como NPS usa automáticamente todos los adaptadores de red para el tráfico RADIUS, solo deberás especificar los adaptadores de red que quieras que use NPS para RADIUS tráfico cuando quieras impedir que NPS usando un adaptador de red específica.

>[!NOTE]
>Si desinstala IPv4 o IPv6 en un adaptador de red, NPS no supervisa el tráfico RADIUS para el protocolo desinstalado.

En un servidor NPS que tiene varios adaptadores de red instalados, es posible que quieras configurar NPS para enviar y recibir tráfico RADIUS solo en los adaptadores que especifique.

Por ejemplo, un adaptador de red instalado en el servidor NPS podría provocar un segmento de red que no contenga clientes RADIUS, mientras que proporciona un segundo adaptador de red NPS con una ruta de acceso de red a la ha configurado los clientes RADIUS. En este escenario, es importante dirigir NPS para que use el segundo adaptador de red para todo el tráfico RADIUS.

En otro ejemplo, si el servidor NPS tiene tres adaptadores de red instalados, pero solo quieres NPS usar dos de los adaptadores para el tráfico RADIUS, puedes configurar información de puerto para los dos adaptadores. Mediante la exclusión de configuración de puerto para el adaptador de terceros, evita que NPS usando el adaptador para el tráfico RADIUS.

## <a name="using-a-network-adapter"></a>Usar un adaptador de red

Para configurar NPS para escuchar y enviar el tráfico de radio en un adaptador de red, usa la siguiente sintaxis en el cuadro de diálogo Propiedades del servidor de directivas de red en la consola NPS:

- Sintaxis de tráfico IPv4: IPAddress: UDPport, donde IPAddress es la dirección IPv4 que está configurada en el adaptador de red que quieres enviar tráfico RADIUS y UDPport es el número de puerto de radio que quieras usar para el tráfico de contabilidad o autenticación RADIUS.
- Sintaxis de tráfico IPv6: [IPv6Address]: UDPport, donde la IPv6Address entre corchetes son necesarios, IPv6Address es la dirección IPv6 que está configurada en el adaptador de red que quieres enviar tráfico RADIUS y UDPport es el número de puerto de radio que quieras usar para el tráfico de contabilidad o autenticación RADIUS.

Los caracteres siguientes pueden usarse como delimitadores para configurar la dirección IP y puerto UDP:

- Dirección y puerto delimitador: dos puntos (:)
- El delimitador de puerto: coma (,)
- Interfaz delimitador: punto y coma (;)

## <a name="configuring-network-access-servers"></a>Configuración de los servidores de acceso de red

Asegúrate de que los servidores de acceso de red están configurados con los mismos números de puerto UDP de radio que configuran en los servidores NPS. Puertos UDP estándar RADIUS definidos en RFC 2865 y 2866 son 1812 para la autenticación y 1813 para cuentas; Sin embargo, algunos servidores de acceso se configuran de forma predeterminada para usar el puerto UDP 1645 para solicitudes de autenticación y el puerto UDP 1646 para las solicitudes de contabilidad.

>[!IMPORTANT]
>Si no usas los números de puerto de radio de forma predeterminada, debes configurar excepciones en el firewall para el equipo local permitir el tráfico de radio en los puertos nuevo. Para obtener más información, consulta [configurar el firewall para el tráfico RADIUS](nps-firewalls-configure.md).

## <a name="configure-the-multihomed-nps-server"></a>Configurar el servidor NPS múltiple

Puedes usar el siguiente procedimiento para configurar el servidor NPS de múltiple.

Pertenencia a **administradores de dominio**, o equivalente, es lo mínimo necesario para completar este procedimiento.

### <a name="to-specify-the-network-adapter-and-udp-ports-that-nps-uses-for-radius-traffic"></a>Para especificar el adaptador de red y puertos UDP que usa NPS para el tráfico RADIUS

1. En el administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **el servidor de directivas de red** para abrir la consola NPS.

2. Haz clic en **el servidor de directivas de red**y, a continuación, haz clic en **propiedades**.

3. Haz clic en el **puertos** pestaña y anteponer la dirección IP para el adaptador de red que quieras usar para el tráfico RADIUS a los números de puerto existente. Por ejemplo, si quieres usar la dirección IP 192.168.1.2 puertos y RADIUS 1812 y 1645 para solicitudes de autenticación, cambiar la configuración del puerto de **1812,1645** a **192.168.1.2:1812,1645**. Si la autenticación RADIUS RADIUS puertos UDP y cuentas son diferentes de los valores predeterminados, cambiar la configuración del puerto según corresponda.

4. Para usar varias configuraciones de puerto para la autenticación o las solicitudes de cuentas, separe los números de puerto con comas.

Para obtener más información acerca de los puertos UDP NPS, consulta [configurar información del puerto UDP NPS](nps-udp-ports-configure.md)


Para obtener más información acerca de NPS, consulta [el servidor de directivas de red](nps-top.md)

