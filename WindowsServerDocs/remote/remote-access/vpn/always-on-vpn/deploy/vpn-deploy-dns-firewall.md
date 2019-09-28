---
title: Definir el DNS y la configuración del firewall
description: En este tema se proporcionan instrucciones detalladas para implementar Always On VPN en Windows Server 2016.
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: d8cf3bae-45bf-4ffa-9205-290d555c59da
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 06/11/2018
ms.openlocfilehash: 326f1e8d52dc34ad433e8cc3bd4c4e84508026b7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388078"
---
# <a name="step-5-configure-dns-and-firewall-settings"></a>Paso 5. Configuración de DNS y firewall

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Previo** Paso 4. Instalar y configurar el servidor NPS](vpn-deploy-nps.md)
- [**Nueva** Paso 6. Configurar las conexiones VPN de Always On del cliente de Windows 10](vpn-deploy-client-vpn-connections.md)

En este paso, va a configurar DNS y la configuración de Firewall para la conectividad VPN.

## <a name="configure-dns-name-resolution"></a>Configuración de la resolución de nombres DNS

Cuando los clientes de VPN remotos se conectan, usan los mismos servidores DNS que usan los clientes internos, lo que les permite resolver nombres de la misma manera que el resto de las estaciones de trabajo internas.

Por este motivo, debe asegurarse de que el nombre de equipo que usan los clientes externos para conectarse al servidor VPN coincide con el nombre alternativo del firmante definido en los certificados emitidos para el servidor VPN.

Para asegurarse de que los clientes remotos pueden conectarse al servidor VPN, puede crear un registro DNS A (host) en la zona DNS externa. El registro A debe usar el nombre alternativo del sujeto del certificado para el servidor VPN.

### <a name="to-add-a-host-a-or-aaaa-resource-record-to-a-zone"></a>Para agregar un registro de recursos de host (A o AAAA) a una zona

1. En un servidor DNS, en Administrador del servidor, seleccione **herramientas**y, a continuación, seleccione **DNS**. Se abre el administrador de DNS.
2. En el árbol de la consola del administrador de DNS, seleccione el servidor que desea administrar.
3. En el panel de detalles, en **nombre**, haga doble clic en **zonas de búsqueda directa** para expandir la vista.
4. En detalles de **zonas de búsqueda directa** , haga clic con el botón secundario en la zona de búsqueda directa a la que desea agregar un registro y, a continuación, seleccione **host nuevo (a o aaaa)** . Se abre el cuadro de diálogo **nuevo host** .
5. En **nuevo host**, en **nombre**, escriba el nombre alternativo del sujeto del certificado para el servidor VPN.
6. En dirección IP, escriba la dirección IP del servidor VPN. Puede escribir la dirección en formato IP versión 4 (IPv4) para agregar un registro de recursos de host (A) o el formato de IP versión 6 (IPv6) para agregar un registro de recursos de host (AAAA).
7. Si ha creado una zona de búsqueda inversa para un intervalo de direcciones IP, incluida la dirección IP que especificó, active la casilla **crear registro de puntero (PTR) asociado** .  Al seleccionar esta opción, se crea un registro de recursos de puntero (PTR) adicional en una zona inversa para este host, en función de la información que escribió en **nombre** y **dirección IP**.
8. Seleccione **Agregar host**.

## <a name="configure-the-edge-firewall"></a>Configuración del firewall perimetral

El firewall perimetral separa la red perimetral externa de la red pública de Internet. Para obtener una representación visual de esta separación, vea la ilustración del tema [Always on información general sobre la tecnología VPN](../always-on-vpn-technology-overview.md).

El firewall perimetral debe permitir y reenviar puertos específicos al servidor VPN. Si usa la traducción de direcciones de red (NAT) en el firewall perimetral, es posible que necesite habilitar el reenvío de puerto para los puertos 500 y 4500 del Protocolo de datagramas de usuario (UDP). Reenvíe estos puertos a la dirección IP asignada a la interfaz externa del servidor VPN.

Si enruta el tráfico entrante y realiza NAT en o detrás del servidor VPN, debe abrir las reglas de Firewall para permitir los puertos UDP 500 y 4500 de entrada a la dirección IP externa aplicada a la interfaz pública en el servidor VPN.

En cualquier caso, si el Firewall admite la inspección profunda de paquetes y tiene dificultades para establecer conexiones de cliente, debe intentar relajar o deshabilitar la inspección profunda de paquetes para las sesiones IKE.

Para obtener información sobre cómo realizar estos cambios de configuración, consulte la documentación del firewall.

## <a name="configure-the-internal-perimeter-network-firewall"></a>Configurar el Firewall de red perimetral interno

El Firewall de red perimetral interno separa la organización o la red corporativa de la red perimetral interna. Para obtener una representación visual de esta separación, vea la ilustración del tema [Always on información general sobre la tecnología VPN](../always-on-vpn-technology-overview.md).

En esta implementación, el servidor VPN de acceso remoto de la red perimetral se configura como un cliente RADIUS.  El servidor VPN envía el tráfico RADIUS al NPS en la red corporativa y también recibe tráfico RADIUS del NPS.

Configure el firewall para permitir que el tráfico RADIUS fluya en ambas direcciones.

>[!NOTE]
>El servidor NPS de la organización o la red corporativa funciona como un servidor RADIUS para el servidor VPN, que es un cliente RADIUS. Para obtener más información acerca de la infraestructura de RADIUS, consulte [servidor de directivas de redes (NPS)](../../../../../networking/technologies/nps/nps-top.md).

### <a name="radius-traffic-ports-on-the-vpn-server-and-nps-server"></a>Puertos de tráfico RADIUS en el servidor VPN y el servidor NPS

De forma predeterminada, NPS y VPN escuchan el tráfico RADIUS en los puertos 1812, 1813, 1645 y 1646 en todos los adaptadores de red instalados. Si habilita Firewall de Windows con seguridad avanzada al instalar NPS, se crearán automáticamente las excepciones de Firewall para estos puertos durante el proceso de instalación del tráfico IPv6 e IPv4.

>[!IMPORTANT]
>Si los servidores de acceso a la red están configurados para enviar tráfico RADIUS a través de puertos distintos de los predeterminados, quite las excepciones creadas en firewall de Windows con seguridad avanzada durante la instalación de NPS y cree excepciones para los puertos que se usan para Tráfico RADIUS.

### <a name="use-the-same-radius-ports-for-the-internal-perimeter-network-firewall-configuration"></a>Usar los mismos puertos RADIUS para la configuración del firewall de red perimetral interno

Si usa la configuración de Puerto RADIUS predeterminada en el servidor VPN y el servidor NPS, asegúrese de que abre los siguientes puertos en el Firewall de red perimetral interno:

- Puertos UDP1812, UDP1813, UDP1645 y UDP1646

Si no usa los puertos RADIUS predeterminados en la implementación de NPS, debe configurar el firewall para permitir el tráfico RADIUS en los puertos que usa. Para obtener más información, consulte [configurar firewalls para el tráfico RADIUS](../../../../../networking/technologies/nps/nps-firewalls-configure.md).

## <a name="next-steps"></a>Pasos siguientes

[Paso 6. Configurar conexiones](vpn-deploy-client-vpn-connections.md)VPN de Always on cliente de Windows 10: En este paso, se configuran los equipos cliente de Windows 10 para que se comuniquen con esa infraestructura con una conexión VPN. Puede usar varias tecnologías para configurar los clientes de VPN de Windows 10, como Windows PowerShell, System Center Configuration Manager e Intune. Los tres requieren un perfil de VPN de XML para establecer la configuración de VPN adecuada.