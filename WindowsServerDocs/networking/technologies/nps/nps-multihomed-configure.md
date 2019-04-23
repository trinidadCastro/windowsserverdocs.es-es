---
title: Configurar NPS en un equipo de host múltiple
description: Este tema proporciona instrucciones sobre cómo configurar un servidor con varios adaptadores de red que se ejecuta el servidor de directivas de red en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d9d9e9ac-4859-4522-89ed-a23092c9e12a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 55eccf3afc649e84c5b6f5ce7932ed97617ddca9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856806"
---
# <a name="configure-nps-on-a-multihomed-computer"></a>Configurar NPS en un equipo de host múltiple

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este tema para configurar NPS con varios adaptadores de red.

Cuando se usa varios adaptadores de red en un servidor que ejecuta el servidor de directivas de redes (NPS), puede configurar lo siguiente:

- Los adaptadores de red que no y enviar y recibir Remote Authentication Dial-in User Service \(RADIUS\) tráfico.
- En una base de adaptador de red, si NPS supervisa el tráfico RADIUS en el protocolo de Internet versión 4 \(IPv4\), IPv6 o IPv4 e IPv6.
- Los números de puerto UDP a través de qué RADIUS tráfico enviado y recibido en un protocolo por \(IPv4 o IPv6\), cada adaptador de red.

De forma predeterminada, NPS escucha el tráfico RADIUS en los puertos 1812, 1813, 1645 y 1646 para IPv6 e IPv4 para todos los adaptadores de red instalados. Dado que NPS usa automáticamente todos los adaptadores de red para el tráfico RADIUS, solo deberá especificar los adaptadores de red que desea que NPS use para RADIUS cuando desee impedir que NPS use un adaptador de red específico de tráfico.

>[!NOTE]
>Si desinstala IPv4 o IPv6 en un adaptador de red, NPS no supervisará el tráfico RADIUS para el protocolo desinstalado.

En NPS que tiene varios adaptadores de red instalados, puede configurar NPS para enviar y recibir tráfico RADIUS sólo en los adaptadores que especifique.

Por ejemplo, un adaptador de red instalado en el NPS podría conducir a un segmento de red que no contiene a clientes RADIUS, mientras que un segundo adaptador de red proporciona a NPS una ruta de acceso de red a sus clientes RADIUS configurados. En este escenario, es importante dirigir NPS para usar el segundo adaptador de red para todo el tráfico RADIUS.

En otro ejemplo, si el NPS tiene tres adaptadores de red instalados, pero sólo desea que NPS use dos de los adaptadores para el tráfico RADIUS, puede configurar información de puerto para los dos adaptadores. Si se excluye la configuración de puerto para el tercer adaptador, evita que NPS mediante el adaptador para el tráfico RADIUS.

## <a name="using-a-network-adapter"></a>Uso de un adaptador de red

Para configurar NPS para escuchar y enviar tráfico RADIUS en un adaptador de red, use la sintaxis siguiente en el cuadro de diálogo Propiedades del servidor de directivas de red en la consola de NPS:

- Sintaxis de tráfico de IPv4: DirecciónIP: puertoUDP, donde direcciónIP es la dirección IPv4 que está configurada en el adaptador de red en la que desea enviar tráfico RADIUS y puertoUDP es el número de puerto RADIUS que desea usar para el tráfico de administración de cuentas y autenticación RADIUS.
- Sintaxis de tráfico de IPv6: [IPv6Address]: PuertoUDP, donde son necesarios los corchetes que encierran IPv6Address, IPv6Address es la dirección IPv6 que está configurada en el adaptador de red en la que desea enviar tráfico RADIUS y puertoUDP es el número de puerto RADIUS que desea usar para la autenticación de RADIUS o tráfico de cuentas.

Los caracteres siguientes pueden usarse como delimitadores para configurar la dirección IP y la información del puerto UDP:

- Delimitador de dirección/puerto: dos puntos (:)
- Delimitador de puertos: coma (,)
- Delimitador de interfaces: punto y coma (;)

## <a name="configuring-network-access-servers"></a>Configuración de los servidores de acceso de red

Asegúrese de que los servidores de acceso de red están configurados con los mismos números de puerto UDP de RADIUS que configure en sus NPSs. Los puertos UDP estándar de RADIUS definidos en las RFC 2865 y 2866 son 1812 para autenticación y 1813 para cuentas; Sin embargo, algunos servidores de acceso se configuran de forma predeterminada para usar el puerto UDP 1645 para las solicitudes de autenticación y el puerto UDP 1646 para las solicitudes de cuentas.

>[!IMPORTANT]
>Si no usa los números de puerto RADIUS predeterminados, debe configurar las excepciones del firewall para el equipo local permitir el tráfico RADIUS en los nuevos puertos. Para obtener más información, consulte [configurar Firewalls para el tráfico RADIUS](nps-firewalls-configure.md).

## <a name="configure-the-multihomed-nps"></a>Configurar el NPS de host múltiple

Puede usar el procedimiento siguiente para configurar el NPS de host múltiple.

Para completar este procedimiento, se requiere como mínimo la pertenencia a **Admins. del dominio** o equivalente.

### <a name="to-specify-the-network-adapter-and-udp-ports-that-nps-uses-for-radius-traffic"></a>Para especificar el adaptador de red y puertos UDP que usa NPS para el tráfico RADIUS

1. En el administrador del servidor, haga clic en **herramientas**y, a continuación, haga clic en **servidor de directivas de red** para abrir la consola de NPS.

2. Haga clic en **servidor de directivas de red**y, a continuación, haga clic en **propiedades**.

3. Haga clic en el **puertos** pestaña y anteponga la dirección IP del adaptador de red que desea usar para el tráfico RADIUS a los números de puerto existente. Por ejemplo, si desea usar la dirección IP 192.168.1.2 y los puertos RADIUS 1812 y 1645 para las solicitudes de autenticación, cambie la configuración de puerto de **1812,1645** a **192.168.1.2:1812,1645**. Si la autenticación RADIUS y los puertos UDP de cuentas RADIUS son distintos de los valores predeterminados, cambie la configuración de puerto según corresponda.

4. Para utilizar varias configuraciones de puerto para la autenticación o las solicitudes de cuentas, separe los números de puerto con comas.

Para obtener más información acerca de los puertos UDP de NPS, consulte [configurar información de puertos UDP de NPS](nps-udp-ports-configure.md)


Para obtener más información acerca de NPS, consulte [servidor de directivas de red](nps-top.md)

