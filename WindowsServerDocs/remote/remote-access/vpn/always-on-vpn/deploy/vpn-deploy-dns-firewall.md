---
title: Definir el DNS y la configuración del firewall
description: Este tema proporciona instrucciones detalladas para la implementación de VPN de Always On en Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: d8cf3bae-45bf-4ffa-9205-290d555c59da
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 06/11/2018
ms.openlocfilehash: f57bfa005ef1af2556e4bd1cbb90ef8d3cfd22e3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886256"
---
# <a name="step-5-configure-dns-and-firewall-settings"></a>Paso 5. Configuración DNS y del firewall

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10

&#171;  [**Anterior:** Paso 4. Instalar y configurar el servidor NPS](vpn-deploy-nps.md)<br>
&#187;  [**próximo:** Paso 6. Configuración de cliente Windows 10 siempre en las conexiones VPN](vpn-deploy-client-vpn-connections.md)

En este paso, configurará DNS y servidor de seguridad de configuración para la conectividad VPN.

## <a name="configure-dns-name-resolution"></a>Configurar la resolución de nombres DNS

Cuando se conectan los clientes VPN remotos, usan los mismos servidores DNS que usan los clientes internos, lo que les permite resolver nombres en la misma manera que el resto de las estaciones de trabajo internos. 

Por este motivo, debe asegurarse de que el nombre del equipo que los clientes externos se usan para conectarse al servidor VPN coincide con el nombre alternativo del firmante definido en los certificados emitidos para el servidor VPN.

Para asegurarse de que los clientes remotos pueden conectarse al servidor VPN, puede crear un registro DNS A (Host) en la zona DNS externa. El registro debe usar el nombre alternativo de sujeto de certificado para el servidor VPN.


### <a name="to-add-a-host-a-or-aaaa-resource-record-to-a-zone"></a>Para agregar un host \(A o AAAA\) registro de recursos a una zona

1. En un servidor DNS, en el administrador del servidor, haga clic en **herramientas**y, a continuación, haga clic en **DNS**. Se abre el Administrador de DNS.
2. En el árbol de consola de administrador de DNS, haga clic en el servidor que desea administrar.
3. En el panel de detalles, en **nombre**, double\-haga clic en **zonas de búsqueda directa** para expandir la vista.
4. En **zonas de búsqueda directa** detalla derecha\-haga clic en la zona de búsqueda directa a la que desea agregar un registro y, a continuación, haga clic en **nuevo Host \(A o AAAA\)**. El **nuevo Host** abre el cuadro de diálogo.
5. En **nuevo Host**, en **nombre**, escriba el nombre alternativo de sujeto de certificado para el servidor VPN.
6. En la dirección IP, escriba la dirección IP del servidor VPN. Puede escribir la dirección IP versión 4 (IPv4) en el formato para agregar un host \(A\) registro de recursos o IP versión 6 \(IPv6\) formato para agregar un host \(AAAA\) registro de recursos.
7. Si ha creado una zona de búsqueda inversa para un intervalo de direcciones IP, incluida la dirección IP que ha escrito, a continuación, seleccione el **crear registro de puntero (PTR) asociado** casilla de verificación.  Al seleccionar esta opción crea un puntero adicional \(PTR\) registro de recursos en un orden inverso de zona para este host, según la información que ha escrito en **nombre** y **dirección IP**.
8. Haga clic en **Agregar host**.

## <a name="configure-the-edge-firewall"></a>Configurar el Firewall perimetral

El Firewall perimetral separa la red de perímetro externo de la red Internet pública. Para obtener una representación visual de esta separación, vea la ilustración del tema [siempre en tecnología de información general sobre VPN](../always-on-vpn-technology-overview.md).

El Firewall perimetral debe permitir y reenviar puertos específicos para el servidor VPN. Si usas la traducción de direcciones de red \(NAT\) en el firewall perimetral, es posible que deba habilitar el reenvío de puerto para el protocolo de datagramas de usuario \(UDP\) puertos 500 y 4500. Reenviar estos puertos a la dirección IP que se asigna a la interfaz externa del servidor VPN.

Si va a enrutar el tráfico entrante y llevar a cabo NAT en o detrás del servidor VPN, a continuación, debe abrir las reglas de firewall para permitir UDP puertos 500 y 4500 entrante a la dirección IP externa que se aplica a la interfaz pública en el servidor VPN.

En cualquier caso, si el firewall admite inspección profunda de paquetes y tiene dificultades para establecer conexiones de cliente, debe intentar amplíe o deshabilitar la inspección profunda de paquetes para las sesiones de IKE.

Para obtener información sobre cómo realizar estos cambios de configuración, consulte la documentación del firewall.

## <a name="configure-the-internal-perimeter-network-firewall"></a>Configurar el Firewall de red de perímetro interno

El Firewall de red de perímetro interno separa la red corporativa o de organización de la red de perímetro interno. Para obtener una representación visual de esta separación, vea la ilustración del tema [siempre en tecnología de información general sobre VPN](../always-on-vpn-technology-overview.md).

En esta implementación, el servidor de acceso remoto VPN en la red perimetral está configurado como cliente RADIUS.  El servidor VPN envía tráfico RADIUS a NPS en la red corporativa y también recibe el tráfico RADIUS en NPS.

Configurar el firewall para permitir el tráfico RADIUS a fluir en ambas direcciones.


>[!NOTE]
>El servidor NPS de la red corporativa o de organización funciona como un servidor RADIUS para el servidor VPN, que es un cliente RADIUS. Para obtener más información acerca de la infraestructura de RADIUS, consulte [servidor de directivas de redes (NPS)](../../../../../networking/technologies/nps/nps-top.md).

### <a name="radius-traffic-ports-on-the-vpn-server-and-nps-server"></a>Puertos del tráfico RADIUS en el servidor VPN y NPS

De forma predeterminada, NPS y VPN escuchan el tráfico RADIUS en los puertos 1812, 1813, 1645 y 1646 en todos los adaptadores de red instalados. Si habilita el Firewall de Windows con seguridad avanzada al instalar NPS, excepciones de firewall para estos puertos se crean automáticamente durante el proceso de instalación para el tráfico de IPv6 e IPv4.

>[!IMPORTANT]
>Si los servidores de acceso de red están configurados para enviar tráfico RADIUS a través de puertos distintos de los predeterminados, quite las excepciones creadas en el Firewall de Windows con seguridad avanzada durante la instalación de NPS y cree excepciones para los puertos que usa para Tráfico RADIUS.

### <a name="use-the-same-radius-ports-for-the-internal-perimeter-network-firewall-configuration"></a>Utilice los mismos puertos RADIUS para la configuración de Firewall de red de perímetro interno

Si usa la configuración de puerto RADIUS predeterminados en el servidor VPN y el servidor NPS, asegúrese de que abra los puertos siguientes en el Firewall de red de perímetro interno:

- Puertos UDP1812, UDP1813, UDP1645 y UDP1646

Si no utiliza los puertos predeterminados de RADIUS en la implementación de NPS, debe configurar el firewall para permitir el tráfico RADIUS en los puertos que está usando. Para obtener más información, consulte [configurar Firewalls para el tráfico RADIUS](../../../../../networking/technologies/nps/nps-firewalls-configure.md).

## <a name="next-step"></a>Paso siguiente
[Paso 6. Configuración de cliente Windows 10 siempre en las conexiones VPN](vpn-deploy-client-vpn-connections.md): En este paso, configure los equipos cliente de Windows 10 para comunicarse con esa infraestructura con una conexión VPN. Puede usar varias tecnologías para configurar a los clientes de VPN de Windows 10, incluidos Windows PowerShell, System Center Configuration Manager e Intune. Las tres requieren un perfil de VPN de XML para la configuración de VPN adecuado. 

---
