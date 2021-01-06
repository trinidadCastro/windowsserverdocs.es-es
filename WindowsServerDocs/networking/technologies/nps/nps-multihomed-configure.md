---
title: Configuración de NPS en un equipo de host múltiple
description: En este tema se proporcionan instrucciones sobre la configuración de un servidor con varios adaptadores de red que ejecuta el servidor de directivas de redes en Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: d9d9e9ac-4859-4522-89ed-a23092c9e12a
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 5c92046aaef7f22a2f9553260b009cda6663aa2e
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97944981"
---
# <a name="configure-nps-on-a-multihomed-computer"></a>Configuración de NPS en un equipo de host múltiple

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para configurar un NPS con varios adaptadores de red.

Si usa varios adaptadores de red en un servidor que ejecuta NPS (Servidor de directivas de redes), puede configurar lo siguiente:

- Los adaptadores de red que realizan y no envían ni reciben Servicio de autenticación remota telefónica de usuario \( tráfico RADIUS \) .
- En cada adaptador de red, si NPS supervisa el tráfico RADIUS en el protocolo de Internet versión \( 4 \) , IPv4, IPv6 o ambos, tanto IPv4 como IPv6.
- Los números de puerto UDP a través del cual se envía y recibe tráfico RADIUS en función de cada protocolo \( IPv4 o IPv6 \) , por adaptador de red.

De forma predeterminada, NPS escucha el tráfico RADIUS en los puertos 1812, 1813, 1645 y 1646 para ambos protocolos IPv6 e IPv4, y para todos los adaptadores de red instalados. Dado que NPS usa automáticamente todos los adaptadores de red para el tráfico RADIUS, solo tiene que especificar los adaptadores de red que desea que NPS use para el tráfico RADIUS si desea impedir que NPS use un adaptador de red específico.

>[!NOTE]
>Si desinstala IPv4 o IPv6 en un adaptador de red, NPS no supervisará el tráfico RADIUS para el protocolo desinstalado.

En un NPS que tiene varios adaptadores de red instalados, es posible que desee configurar NPS para enviar y recibir tráfico RADIUS solo en los adaptadores que especifique.

Por ejemplo, un adaptador de red instalado en NPS puede conducir a un segmento de red que no contiene clientes RADIUS, mientras que un segundo adaptador de red proporciona a NPS una ruta de acceso de red a sus clientes RADIUS configurados. En este escenario, es importante dirigir NPS para que use el segundo adaptador de red para todo el tráfico RADIUS.

En otro ejemplo, si el NPS tiene tres adaptadores de red instalados, pero solo quiere que NPS Use dos de los adaptadores para el tráfico RADIUS, puede configurar la información de Puerto solo para los dos adaptadores. Al excluir la configuración de puertos para el tercer adaptador, evita que NPS use el adaptador para el tráfico RADIUS.

## <a name="using-a-network-adapter"></a>Uso de un adaptador de red

Para configurar NPS para que escuche y envíe tráfico RADIUS en un adaptador de red, use la siguiente sintaxis en el cuadro de diálogo Propiedades del servidor de directivas de redes en la consola NPS:

- Sintaxis del tráfico IPv4: IPAddress: Puertoudp, donde IPAddress es la dirección IPv4 que está configurada en el adaptador de red sobre el que desea enviar tráfico RADIUS y Puertoudp es el número de Puerto RADIUS que desea usar para el tráfico de autenticación o cuentas RADIUS.
- Sintaxis de tráfico de IPv6: [Direcciónipv6]: Puertoudp, donde se requieren corchetes en Direcciónipv6, Direcciónipv6 es la dirección IPv6 configurada en el adaptador de red sobre el que desea enviar tráfico RADIUS y Puertoudp es el número de Puerto RADIUS que desea usar para el tráfico de autenticación o cuentas RADIUS.

Los siguientes caracteres pueden usarse como delimitadores para configurar la información de direcciones IP y puertos UDP:

- Delimitador de dirección/puerto: dos puntos (:)
- Delimitador de puerto: coma (,)
- Delimitador de interfaz: punto y coma (;)

## <a name="configuring-network-access-servers"></a>Configuración de servidores de acceso a la red

Asegúrese de que los servidores de acceso a la red estén configurados con los mismos números de puerto UDP de RADIUS que configure en su NPSs. Los puertos UDP estándar de RADIUS definidos en las RFC 2865 y 2866 son 1812 para autenticación y 1813 para cuentas; sin embargo, algunos servidores de acceso están configurados de forma predeterminada para usar el puerto UDP 1645 para las solicitudes de autenticación y el puerto UDP 1646 para las solicitudes de cuentas.

>[!IMPORTANT]
>Si no usa los números de puerto RADIUS predeterminados, debe configurar excepciones en el firewall para el equipo local para permitir el tráfico RADIUS en los nuevos puertos. Para obtener más información, consulte [configurar firewalls para el tráfico RADIUS](nps-firewalls-configure.md).

## <a name="configure-the-multihomed-nps"></a>Configuración del NPS de host múltiple

Puede usar el procedimiento siguiente para configurar el NPS de host múltiple.

La pertenencia a **Administradores de dominio**, o equivalente, es lo mínimo necesario para completar este procedimiento.

### <a name="to-specify-the-network-adapter-and-udp-ports-that-nps-uses-for-radius-traffic"></a>Para especificar el adaptador de red y puertos UDP que usa NPS para el tráfico RADIUS

1. En Administrador del servidor, haga clic en **herramientas** y, a continuación, haga clic en **servidor de directivas de redes** para abrir la consola NPS.

2. Haga clic con el botón secundario en **servidor de directivas de redes** y, a continuación, haga clic en **propiedades**.

3. Haga clic en la pestaña **puertos** y anteponga la dirección IP del adaptador de red que desea usar para el tráfico RADIUS a los números de Puerto existentes. Por ejemplo, si desea usar la dirección IP 192.168.1.2 y los puertos RADIUS 1812 y 1645 para las solicitudes de autenticación, cambie la configuración de puerto de **1812, 1645** a **192.168.1.2:1812, 1645**. Si los puertos UDP de autenticación y cuentas RADIUS son distintos de los valores predeterminados, cambie la configuración de los puertos según corresponda.

4. Para usar varios valores de puertos para las solicitudes de autenticación y cuentas, separe los números de puerto con comas.

Para obtener más información sobre los puertos UDP de NPS, consulte Configuración de la [información del puerto UDP de NPS](nps-udp-ports-configure.md) .


Para obtener más información acerca de NPS, consulte [servidor de directivas de redes](nps-top.md) .

