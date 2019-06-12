---
title: Interconexión de red virtual
description: ''
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 08/08/2018
ms.openlocfilehash: aab4ec7c69ec5b52eae926cd1065d777415b1124
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446209"
---
# <a name="virtual-network-peering"></a>Interconexión de red virtual

>Se aplica a: Windows Server

Emparejamiento de redes virtuales permite conectar dos redes virtuales sin problemas. Una vez emparejadas, efectos de conectividad, las redes virtuales aparecen como una sola. 

Las ventajas del uso del emparejamiento de red virtual son:

-   El tráfico entre máquinas virtuales de las redes virtuales emparejadas se enruta a través de la infraestructura de red troncal mediante *privada* únicamente de direcciones IP. La comunicación entre las redes virtuales no requiere pública Internet o puertas de enlace.

-   Una conexión de baja latencia y alto ancho de banda entre los recursos de redes virtuales diferentes.

-   La capacidad de recursos en una red virtual para comunicarse con los recursos de una red virtual diferente.

-   Sin tiempo de inactividad a los recursos de red virtual al crear el emparejamiento.

## <a name="requirements-and-constraints"></a>Requisitos y restricciones

Emparejamiento de red virtual tiene unos requisitos y restricciones:

- Redes virtuales emparejadas deben:

  -   Tener espacios de direcciones IP no superpuestos

  -   Administrar la misma controladora de red

- Una vez que emparejar una red virtual con otra red virtual, no se puede agregar o eliminar los intervalos de direcciones en el espacio de direcciones.

  >[!TIP]
  >Si necesita agregar intervalos de direcciones:<ol><li>Quite el emparejamiento.</li><li>Agregue el espacio de direcciones.</li><li>Agregar el emparejamiento de nuevo.</li></ol>

- Puesto que el emparejamiento de redes virtuales está entre dos redes virtuales, no hay ninguna relación transitiva derivada entre emparejamientos. Por ejemplo, si emparejar la red virtual a red virtual b y la red virtual b con la red, a continuación, red virtual a no obtener emparejada con la red.

  [image here]

## <a name="connectivity"></a>Conectividad

Después de emparejar redes virtuales, los recursos en cualquier red virtual pueden conectar directamente con los recursos de la red virtual emparejada.

-   Latencia de red entre máquinas virtuales en redes virtuales emparejadas es la misma que la latencia de una única red virtual.

-   Rendimiento de la red se basa en el ancho de banda permitido para la máquina virtual. No hay restricciones adicionales en el ancho de banda en el emparejamiento.

-   El tráfico entre máquinas virtuales en redes virtuales emparejadas se enruta directamente a través de la infraestructura de red troncal, no a través de una puerta de enlace o a través de Internet pública.

-   Las máquinas virtuales en una red virtual puede tener acceso el equilibrador de carga interno de la red virtual emparejada.

Puede aplicar listas de control de acceso (ACL) en cualquier red virtual para bloquear el acceso a otras redes o subredes virtuales si lo desea. Si abre la conectividad completa entre redes virtuales emparejadas (que es la opción predeterminada), puede aplicar las ACL para subredes específicas o máquinas virtuales para bloquear o denegar acceso específico. Para más información acerca de las ACL, consulte [usar Access Control Lists (ACL) para el flujo de tráfico de red de centro de datos de administrar](https://docs.microsoft.com/windows-server/networking/sdn/manage/use-acls-for-traffic-flow).

## <a name="service-chaining"></a>Encadenamiento de servicios

Puede configurar las rutas definidas por el usuario que señalan a las máquinas virtuales en redes virtuales emparejadas como la dirección IP de próximo salto, para habilitar el encadenamiento de servicios. Encadenamiento de servicios permite dirigir el tráfico desde una red virtual a un dispositivo virtual en una red virtual emparejada, a través de las rutas definidas por el usuario.

Puede implementar redes de concentrador y radio, donde la red virtual del concentrador puede hospedar componentes de infraestructura, como un dispositivo virtual de red. Todas las redes virtuales de radios emparejar con la red virtual de concentrador. El tráfico puede fluir a través de aplicaciones virtuales de red en la red virtual de concentrador.

Emparejamiento de redes virtuales permite que el próximo salto en una ruta definida por el usuario sea la dirección IP de una máquina virtual en la red virtual emparejada. Para obtener más información acerca de las rutas definidas por el usuario, vea [aplicaciones virtuales de red de uso en una red Virtual](https://docs.microsoft.com/windows-server/networking/sdn/manage/use-network-virtual-appliances-on-a-vn).

## <a name="gateways-and-on-premises-connectivity"></a>Conectividad de puertas de enlace y en el entorno local

Cada red virtual, independientemente de si se empareja con otra red virtual, todavía puede tener su propia puerta de enlace para conectarse a una red local. Al emparejar redes virtuales, también puede configurar la puerta de enlace de la red virtual emparejada como punto de tránsito a una red local. En este caso, la red virtual que usa una puerta de enlace remota no puede tener su propia puerta de enlace. Una red virtual puede tener solo una puerta de enlace que puede ser una puerta de enlace local o remoto (en la red virtual emparejada).

## <a name="monitor"></a>Supervisar

Al emparejar dos redes virtuales, debe configurar un emparejamiento para cada red virtual en el emparejamiento.

Puede supervisar el estado de la conexión de emparejamiento, puede estar en uno de los siguientes estados:

-   **Iniciado por:** Se muestra cuando se crea el emparejamiento de la primera red virtual a la segunda red virtual.

-   **Conectado:** Se muestra después de haber creado el emparejamiento de la segunda red virtual a la primera red virtual. Cambia el estado de emparejamiento para la primera red virtual de iniciado a conectado. Ambos homólogos de la red virtual deben tener el estado conectado antes de establecer una red virtual de emparejamiento correctamente.

-   **Se ha desconectado:** Muestra si una red virtual se desconecta de la otra red virtual.

[infografía de los Estados]

## <a name="next-steps"></a>Pasos siguientes
[Configurar el emparejamiento de red virtual](sdn-configure-vnet-peering.md): En este procedimiento, use Windows PowerShell para buscar el HNV red lógica del proveedor para crear dos redes virtuales, cada uno con una subred. También configura el emparejamiento entre las dos redes virtuales.

