---
title: Definir el DNS y la configuración del firewall
description: Este tema proporciona instrucciones detalladas para la implementación de VPN siempre activada en Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: d8cf3bae-45bf-4ffa-9205-290d555c59da
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 06/11/2018
ms.openlocfilehash: f57bfa005ef1af2556e4bd1cbb90ef8d3cfd22e3
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067484"
---
# Paso 5. Establecer la configuración de DNS y el firewall

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10

& #171;  [ **Anterior:** paso 4. Instalar y configurar el servidor NPS](vpn-deploy-nps.md)<br>
& #187;  [ **Siguiente:** paso 6. Configurar al cliente de Windows 10 siempre en conexiones VPN](vpn-deploy-client-vpn-connections.md)

En este paso, configurar DNS y el Firewall de configuración para la conectividad VPN.

## Configurar la resolución de nombres DNS

Cuando se conectan los clientes VPN remotos, usan los mismos servidores DNS que usan los clientes internos, lo que les permite resolver los nombres de la misma manera que el resto de las estaciones de trabajo internas. 

Por este motivo, debe asegurarse de que el nombre del equipo que los clientes externos que se usan para conectarse al servidor VPN coincide con el nombre alternativo del sujeto definido en certificados emitidos para el servidor VPN.

Para garantizar que los clientes remotos pueden conectarse al servidor VPN, puedes crear un registro A DNS (Host) en la zona DNS externa. El registro debe usar el nombre alternativo del sujeto de certificado para el servidor VPN.


### Para agregar un host \ registro de recursos (A o AAAA\) a una zona

1. En un servidor DNS, en el Administrador de servidores, haz clic en **las herramientas**y, a continuación, haz clic en **DNS**. Abre el Administrador de DNS.
2. En el árbol de consola del Administrador de DNS, haz clic en el servidor que desea administrar.
3. En el panel de detalles, en **nombre**, **Zonas de búsqueda** del double\ clic para expandir la vista.
4. Detalles de **Zonas de búsqueda** , right\ y haga clic en la zona de búsqueda directa a la que quieres agregar un registro y, a continuación, haz clic en **Nuevo Host \(A or AAAA\)**. Abre el cuadro de diálogo **Nuevo Host** .
5. En el **Nuevo Host**, en **nombre**, escribe el nombre alternativo del sujeto de certificado para el servidor VPN.
6. En la dirección IP, escribe la dirección IP del servidor VPN. Puedes escribir la dirección IP versión 4 (IPv4) en el formato para agregar un registro de recursos \(A\) de host o IP versión 6 \(IPv6\) para agregar un registro de recursos \(AAAA\) de host.
7. Si has creado una zona de búsqueda inversa para un intervalo de direcciones IP, incluida la dirección IP que ha escrito, a continuación, selecciona la casilla de verificación **Crear registro del puntero (PTR) asociado** .  Al seleccionar esta opción, crea un registro de recursos \(PTR\) de puntero adicional en una zona inversa para este host, en función de la información que utilizaste en el **nombre** y **dirección IP**.
8. Haz clic en **Agregar host**.

## Configurar el Firewall de Edge

El Firewall de Edge separa la red perimetral externa de Internet pública. Para obtener una representación visual de esta separación, consulta la ilustración en el tema [Siempre en VPN Introducción a la tecnología](../always-on-vpn-technology-overview.md).

El Firewall de Edge debe permitir y reenviar puertos específicos para el servidor VPN. Si usas \(NAT\) traducción de direcciones de red en el firewall de edge, debes habilitar el reenvío de puerto de protocolo de datagramas de usuario \(UDP\) ports500 y 4500. Reenviar estos puertos a la dirección IP que se asigna a la interfaz externa del servidor VPN.

Si va a distribuir el tráfico entrante y llevar a cabo NAT en o detrás del servidor VPN, a continuación, debe abrir las reglas de firewall para permitir ports500 UDP y 4500 entrante a la dirección IP externa aplicado a la interfaz pública en el servidor VPN.

En cualquier caso, si el firewall admite inspección profunda del paquete y que tienen dificultades para establecer conexiones de cliente, se debe intentar relajar o deshabilitar la inspección profunda del paquete para sesiones de IKE.

Para obtener información sobre cómo hacer que estos cambios de configuración, consulta la documentación del firewall.

## Configurar el Firewall de red perimetral interno

El Firewall de red perimetral interno separa la red corporativa o de organización de la red perimetral interna. Para obtener una representación visual de esta separación, consulta la ilustración en el tema [Siempre en VPN Introducción a la tecnología](../always-on-vpn-technology-overview.md).

En esta implementación, se configura el servidor de VPN de acceso remoto en la red perimetral como un cliente de radio.  El servidor VPN envía el tráfico RADIUS al NPS en la red corporativa y también recibe el tráfico RADIUS de NPS.

Configurar el firewall para permitir el tráfico RADIUS fluyan en ambas direcciones.


>[!NOTE]
>El servidor NPS en la red de la organización o empresa funciona como un servidor RADIUS para el servidor VPN, que es un cliente de radio. Para obtener más información acerca de la infraestructura de radio, consulta el [Servidor de directivas de redes (NPS)](../../../../../networking/technologies/nps/nps-top.md).

### Puertos de tráfico RADIUS en el servidor VPN y el servidor NPS

De manera predeterminada, VPN y NPS escuchan el tráfico RADIUS en los puertos 1812, 1813, 1645 y 1646 en todos los adaptadores de red instalados. Si habilitas Firewall de Windows con seguridad avanzada al instalar NPS, excepciones de firewall para estos puertos se crean automáticamente durante el proceso de instalación para el tráfico IPv6 y IPv4.

>[!IMPORTANT]
>Si los servidores de acceso de red están configurados para enviar tráfico RADIUS a través de puertos que no sean de estos valores predeterminados, quitar las excepciones que se creó en Firewall de Windows con seguridad avanzada durante la instalación de NPS y crear excepciones para los puertos que usas para Tráfico RADIUS.

### Usar los mismos puertos de radio para la configuración de Firewall de red perimetral interno

Si usas la configuración predeterminada del puerto de radio en el servidor VPN y el servidor NPS, asegúrate de que abra los siguientes puertos en el Firewall de red perimetral interno:

- Puertos UDP1812, UDP1813, UDP1645 y UDP1646

Si no estás usando los puertos de radio de forma predeterminada en la implementación de NPS, debes configurar el firewall para permitir el tráfico RADIUS en los puertos que estás usando. Para obtener más información, consulta [Configurar servidores de seguridad para el tráfico RADIUS](../../../../../networking/technologies/nps/nps-firewalls-configure.md).

## Paso siguiente
Paso 6 de [. Configurar Windows 10 cliente siempre en conexiones VPN](vpn-deploy-client-vpn-connections.md): en este paso, configuración los equipos de cliente de Windows 10 para comunicarse con los que la infraestructura con una conexión VPN. Puedes usar varias tecnologías para configurar a los clientes de VPN de Windows 10, como Intune, System Center Configuration Manager y Windows PowerShell. Los tres requieren un perfil de VPN de XML para configurar la configuración de VPN adecuada. 

---
