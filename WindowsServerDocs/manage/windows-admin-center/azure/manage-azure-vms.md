---
title: Administración de máquinas virtuales de IaaS de Azure
description: Administración de máquinas virtuales de IaaS de Azure con el centro de administración de Windows (Project Honolulu)
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 09/07/2018
ms.localizationpriority: medium
ms.openlocfilehash: 03ba06ca66c95825f13ff633d5b2bfac99acaa4c
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87997445"
---
# <a name="manage-azure-iaas-virtual-machines-with-windows-admin-center"></a>Administre máquinas virtuales de IaaS de Azure con Windows Admin Center

Puede usar Windows Admin Center para administrar tanto máquinas virtuales de Azure como máquinas locales. Hay varias configuraciones posibles: elija la configuración que tenga sentido para su entorno:
- [Administración de máquinas virtuales de Azure desde una puerta de enlace del centro de administración local de Windows](#manage-with-an-on-premises-windows-admin-center-gateway)
- [Administración de máquinas virtuales de Azure desde una puerta de enlace del centro de administración de Windows instalada en una máquina virtual de Azure](#use-a-windows-admin-center-gateway-deployed-in-azure)

## <a name="manage-with-an-on-premises-windows-admin-center-gateway"></a>Administrar con una puerta de enlace del centro de administración local de Windows

Si ya ha instalado el centro de administración de Windows en una puerta de enlace local (ya sea en Windows 10 o Windows Server 2016), puede usar esta misma puerta de enlace para administrar máquinas virtuales de Windows 10 o Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2 en Azure.

### <a name="connecting-to-vms-with-a-public-ip"></a>Conexión a máquinas virtuales con una dirección IP pública

Si las máquinas virtuales de destino (las máquinas virtuales que desea administrar con el centro de administración de Windows) tienen direcciones IP públicas, agréguelas a la puerta de enlace del centro de administración de Windows mediante la dirección IP o el FQDN. Hay un par de consideraciones que deben tenerse en cuenta:

- Debe habilitar el acceso de WinRM a la máquina virtual de destino mediante la ejecución de lo siguiente en PowerShell o en el símbolo del sistema en la máquina virtual de destino:`winrm quickconfig`
- Si no se ha unido a un dominio de la máquina virtual de Azure, la máquina virtual se comporta como un servidor en grupo de trabajo, por lo que debe asegurarse de que tiene en cuenta el [uso del centro de administración de Windows en un grupo de trabajo](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup).
- También debe habilitar las conexiones entrantes al puerto 5985 para WinRM a través de HTTP para que el centro de administración de Windows administre la máquina virtual de destino:
  1. Ejecute el siguiente script de PowerShell en la máquina virtual de destino para habilitar las conexiones entrantes al puerto 5985 en el SO invitado:`Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any`

  2. También debe abrir el puerto en redes de Azure:

     - Seleccione la máquina virtual de Azure, seleccione **redes**y luego **Agregar regla de puerto de entrada**.
     - Asegúrese de que la opción **básico** está seleccionada en la parte superior del panel **Agregar regla de seguridad de entrada** .
     - En el campo **intervalos de Puerto** , escriba **5985**.

     Si la puerta de enlace del centro de administración de Windows tiene una dirección IP estática, puede seleccionar permitir solo el acceso de WinRM entrante desde la puerta de enlace del centro de administración de Windows para mayor seguridad.
     Para ello, seleccione **avanzadas** en la parte superior del panel **Agregar regla de seguridad de entrada** .

     En **origen**, seleccione **direcciones IP**y, a continuación, escriba la dirección IP de origen correspondiente a la puerta de enlace del centro de administración de Windows.

     - En **Protocolo** , seleccione **TCP**.
     - El resto puede dejarse como predeterminado.

> [!NOTE]
> Debe crear una regla de Puerto personalizada. La regla de puerto de WinRM proporcionada por redes de Azure usa el puerto 5986 (a través de HTTPS) en lugar de 5985 (a través de HTTP).

### <a name="connecting-to-vms-without-a-public-ip"></a>Conexión a máquinas virtuales sin una dirección IP pública

Si las máquinas virtuales de Azure de destino no tienen direcciones IP públicas y desea administrarlas desde una puerta de enlace del centro de administración de Windows implementada en la red local, debe configurar la red local para que tenga conectividad a la red virtual en la que están conectadas las máquinas virtuales de destino. Hay tres formas de hacerlo: ExpressRoute, VPN de sitio a sitio o VPN de punto a sitio. [Conozca qué opción de conectividad tiene sentido en su entorno.](/azure/vpn-gateway/vpn-gateway-plan-design)

>[!TIP]
>Si quiere usar una VPN de punto a sitio para conectar la puerta de enlace del centro de administración de Windows a una red virtual de Azure para administrar máquinas virtuales de Azure en esa red virtual, puede usar la característica [adaptador de red de Azure](https://aka.ms/WACNetworkAdapter) en el centro de administración de Windows. Para ello, conéctese al servidor en el que está instalado el centro de administración de Windows, vaya a la herramienta de red y seleccione "agregar adaptador de red de Azure". Cuando proporcione los detalles necesarios y haga clic en "configurar", el centro de administración de Windows configurará una VPN de punto a sitio en la red virtual de Azure que especifique, después de la cual podrá conectarse a las máquinas virtuales de Azure y administrarlas desde la puerta de enlace del centro de administración local de Windows.

Asegúrese de que WinRM se está ejecutando en las máquinas virtuales de destino mediante la ejecución de lo siguiente en PowerShell o en el símbolo del sistema en la máquina virtual de destino:`winrm quickconfig`

Si no se ha unido a un dominio de la máquina virtual de Azure, la máquina virtual se comporta como un servidor en grupo de trabajo, por lo que debe asegurarse de que tiene en cuenta el [uso del centro de administración de Windows en un grupo de trabajo](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup).

Si tiene algún problema, consulte solución de [problemas del centro de administración de Windows](../support/troubleshooting.md) para ver si se requieren pasos adicionales para la configuración (por ejemplo, si se está conectando con una cuenta de administrador local o no está unido a un dominio).

## <a name="use-a-windows-admin-center-gateway-deployed-in-azure"></a>Uso de una puerta de enlace del centro de administración de Windows implementada en Azure

Puede administrar máquinas virtuales de Azure sin ninguna dependencia local mediante la implementación del centro de administración de Windows en la red virtual en la que están conectadas las máquinas virtuales de destino.

Para administrar máquinas virtuales fuera de la red virtual en la que se implementa la puerta de enlace del centro de administración de Windows, debe establecer la conectividad de red virtual a red virtual entre la red virtual de la puerta de enlace del centro de administración de Windows y la red virtual de los servidores de destino. Puede establecer esta conectividad con el emparejamiento de VNet, la conexión de red virtual a red virtual o una conexión de sitio a sitio. [Obtenga más información sobre qué opción de conectividad de red virtual a red virtual tiene sentido en su entorno.](/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal)

El centro de administración de Windows se puede instalar en una máquina virtual existente o recién implementada en el entorno. La máquina virtual que elija para la instalación del centro de administración de Windows debe tener una dirección IP pública y un nombre DNS.

[Más información sobre la implementación de una puerta de enlace del centro de administración de Windows en Azure](deploy-wac-in-azure.md)