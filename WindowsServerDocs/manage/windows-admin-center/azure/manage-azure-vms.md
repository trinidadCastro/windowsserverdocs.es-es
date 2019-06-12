---
title: Administrar máquinas virtuales de IaaS de Azure
description: Administración de máquinas virtuales de IaaS de Azure con Windows Admin Center (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 09/07/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: ac98f42c4ad5606cc8d2b142f209f9bdb2b9611c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445903"
---
# <a name="manage-azure-iaas-virtual-machines-with-windows-admin-center"></a>Administración de máquinas virtuales de IaaS de Azure con Windows Admin Center

Puede usar Windows Admin Center para administrar sus máquinas virtuales de Azure, así como en las máquinas locales. Hay varias configuraciones diferentes posibles: elija la configuración que tenga sentido para su entorno:
- [Administrar máquinas virtuales de Azure desde una puerta de enlace de Windows Admin Center local](#manage-with-an-on-premises-windows-admin-center-gateway)
- [Administrar máquinas virtuales de Azure desde una puerta de enlace de Windows Admin Center en una máquina virtual de Azure](#use-a-windows-admin-center-gateway-deployed-in-azure)

## <a name="manage-with-an-on-premises-windows-admin-center-gateway"></a>Administrar con una puerta de enlace de Windows Admin Center local

Si ya ha instalado Windows Admin Center en una puerta de enlace local (ya sea en Windows 10 o Windows Server 2016), puede usar esta misma puerta de enlace para administrar Windows 10 o Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2 Máquinas virtuales en Azure. 

### <a name="connecting-to-vms-with-a-public-ip"></a>Conectarse a máquinas virtuales con una dirección IP pública

Si el destino de las máquinas virtuales (las máquinas virtuales que desea administrar con Windows Admin Center) dispone de las direcciones IP públicas, puede agregarlos a la puerta de enlace de Windows Admin Center mediante la dirección IP o FQDN. Hay algunas consideraciones a tener en cuenta:

- Debe habilitar el acceso a WinRM para la máquina virtual de destino ejecutando lo siguiente en PowerShell o la línea de comandos en la máquina virtual de destino: `winrm quickconfig`
- Si se aún no lo ha unido al dominio la máquina virtual de Azure, la máquina virtual se comporta como un servidor en el grupo de trabajo, por lo que necesitará para asegurarse de que se tiene en cuenta [mediante Windows Admin Center en un grupo de trabajo](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup).
- También debe habilitar las conexiones entrantes al puerto 5985 para WinRM a través de HTTP en orden para Windows Admin Center administrar la máquina virtual de destino:
  1. Ejecute el siguiente script de PowerShell en la máquina virtual para habilitar las conexiones entrantes al puerto 5985 en el sistema operativo invitado de destino:   
     `Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any`

  2. También debe abrir el puerto en redes de Azure:

     - Seleccione la máquina virtual de Azure, seleccione **redes**, a continuación, **Agregar regla de puerto de entrada**. 
     - Asegúrese de **básica** está seleccionado en la parte superior de la **Agregar regla de seguridad de entrada** panel.
     - En el **intervalos de puertos** , introduzca **5985**.
    
     Si la puerta de enlace de Windows Admin Center tiene una dirección IP estática, puede seleccionar para permitir WinRM acceso de entrada de la puerta de enlace de Windows Admin Center para mayor seguridad.
     Para ello, seleccione **avanzadas** en la parte superior de la **Agregar regla de seguridad de entrada** panel.

     Para **origen**, seleccione **direcciones IP**, a continuación, escriba la dirección IP de origen correspondiente a la puerta de enlace de Windows Admin Center.

     - Para **protocolo** seleccione **TCP**.
     - El resto puede dejarse como valor predeterminado.

> [!NOTE]
> Debe crear una regla de puerto personalizado. La regla de puerto de WinRM proporcionada por las redes de Azure usa el puerto 5986 (a través de HTTPS) en lugar de 5985 (a través de HTTP). 

### <a name="connecting-to-vms-without-a-public-ip"></a>Conectarse a máquinas virtuales sin una dirección IP pública

Si el destino de máquinas virtuales de Azure no tiene direcciones IP públicas, y desea administrar estas máquinas virtuales de una puerta de enlace de Windows Admin Center implementado en su red local, deberá configurar la red local para que tenga conectividad a la red virtual en el que son el destino de las máquinas virtuales conectado. Existen 3 formas para hacer esto: ExpressRoute, VPN de sitio a sitio o VPN de punto a sitio. [Obtenga información sobre qué opción de conectividad tiene sentido en su entorno.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design) 

>[!TIP]
>Si desea usar una VPN de punto a sitio para conectar la puerta de enlace de Windows Admin Center a una red virtual de Azure para administrar máquinas virtuales de Azure en que la red virtual, puede usar el [adaptador de red de Azure](https://aka.ms/WACNetworkAdapter) característica en Windows Admin Center. Para ello, conéctese al servidor donde está instalado Windows Admin Center, vaya a la herramienta de red y seleccione "Agregar adaptador de red de Azure". Cuando proporcione la información necesaria y haga clic en "Configurar", Windows Admin Center configurará ninguna VPN de punto a sitio a la red virtual de Azure que especifique, después, puede conectarse y administrar máquinas virtuales de Azure desde la puerta de enlace de Windows Admin Center de forma local.

Asegúrese de que se está ejecutando en el destino de las máquinas virtuales WinRM ejecutando lo siguiente en PowerShell o la línea de comandos en la máquina virtual de destino: `winrm quickconfig`

Si se aún no lo ha unido al dominio la máquina virtual de Azure, la máquina virtual se comporta como un servidor en el grupo de trabajo, por lo que necesitará para asegurarse de que se tiene en cuenta [mediante Windows Admin Center en un grupo de trabajo](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup).

Si experimenta algún problema, consulte [solucionar Windows Admin Center](../support/troubleshooting.md) para comprobar si hay pasos adicionales necesarios para la configuración (por ejemplo, si se conecta mediante una cuenta de administrador local o no están unidos a un dominio).

## <a name="use-a-windows-admin-center-gateway-deployed-in-azure"></a>Usar una puerta de enlace de Windows Admin Center implementado en Azure

Puede administrar máquinas virtuales de Azure sin dependencias en el entorno local mediante la implementación de Windows Admin Center en la red virtual donde están conectadas las máquinas virtuales de destino. 

Para administrar las máquinas virtuales fuera de la red virtual en el que se implementa la puerta de enlace de Windows Admin Center, debe establecer la conectividad de VNet a VNet entre la red virtual de la puerta de enlace de Windows Admin Center y la red virtual de los servidores de destino. Puede establecer esta conectividad con emparejamiento de VNet, conexiones de red virtual a red virtual o una conexión de sitio a sitio. [Obtenga más información acerca de la conectividad de VNet a qué opción tiene sentido en su entorno.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal)

Windows Admin Center puede instalarse en una VM existente o recién implementada en su entorno. La máquina virtual que elija para la instalación de Windows Admin Center debe tener un nombre IP y DNS público.

[Más información sobre cómo implementar una puerta de enlace de Windows Admin Center en Azure](deploy-wac-in-azure.md)