---
description: 'Más información acerca de: emparejamiento de redes virtuales'
title: Emparejamiento de redes virtuales
manager: grcusanz
ms.topic: how-to
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/08/2018
ms.openlocfilehash: 325dd4468281e1b6b5ab1fb577908d9a29ba2a28
ms.sourcegitcommit: fb2ae5e6040cbe6dde3a87aee4a78b08f9a9ea7c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2021
ms.locfileid: "98716581"
---
# <a name="virtual-network-peering"></a>Emparejamiento de redes virtuales

>Se aplica a: Windows Server 2019, Windows Server 2016

El emparejamiento de redes virtuales permite conectar dos redes virtuales sin problemas. Una vez emparejadas, para fines de conectividad, las redes virtuales aparecen como una sola.

Las ventajas del uso del emparejamiento de redes virtuales son las siguientes:

-   El tráfico entre las máquinas virtuales de las redes virtuales emparejadas se enruta a través de la infraestructura de red troncal solo a través de direcciones IP *privadas* . La comunicación entre las redes virtuales no requiere Internet o puertas de enlace públicas.

-   Baja latencia, conexión de gran ancho de banda entre los recursos de redes virtuales diferentes.

-   La capacidad de los recursos de una red virtual para comunicarse con los de otra red virtual.

-   No hay tiempo de inactividad en los recursos de una red virtual al crear el emparejamiento.

## <a name="requirements-and-constraints"></a>Requisitos y restricciones

El emparejamiento de redes virtuales tiene algunos requisitos y restricciones:

- Las redes virtuales emparejadas deben:

  -   Tienen espacios de direcciones IP no superpuestos

  -   Ser administrado por la misma controladora de red

- Una vez que se empareja una red virtual con otra red virtual, no se pueden agregar ni eliminar intervalos de direcciones en el espacio de direcciones.

  >[!TIP]
  >Si necesita agregar intervalos de direcciones:<ol><li>Quite el emparejamiento.</li><li>Agregue el espacio de direcciones.</li><li>Vuelva a agregar el emparejamiento.</li></ol>

- Puesto que el emparejamiento de redes virtuales se encuentra entre dos redes virtuales, no hay ninguna relación transitiva derivada entre los emparejamientos. Por ejemplo, si empareja red virtual a con red virtual b y red virtual b con red virtual c, red virtual a no se empareja con red virtual c.

## <a name="connectivity"></a>Conectividad

Después de emparejar las redes virtuales, los recursos de cualquiera de las redes virtuales pueden conectarse directamente con los recursos de la red virtual emparejada.

-   La latencia de red entre máquinas virtuales en redes virtuales emparejadas es la misma que la latencia dentro de una única red virtual.

-   El rendimiento de la red se basa en el ancho de banda que se permite para la máquina virtual. No hay restricciones adicionales con respecto al ancho de banda en el emparejamiento.

-   El tráfico entre las máquinas virtuales de las redes virtuales emparejadas se enruta directamente a través de la infraestructura de red troncal, no a través de una puerta de enlace o a través de la red pública de Internet.

-   Las máquinas virtuales de una red virtual pueden acceder al equilibrador de carga interno de la red virtual emparejada.

Si lo desea, puede aplicar listas de control de acceso (ACL) en cualquier red virtual para bloquear el acceso a otras redes virtuales o subredes. Si abre la conectividad completa entre las redes virtuales emparejadas (que es la opción predeterminada), puede aplicar ACL a subredes o máquinas virtuales concretas para bloquear o denegar el acceso específico. Para obtener más información acerca de las ACL, consulte [uso de listas de Access Control (ACL) para administrar el flujo de tráfico de red del centro de](../manage/use-acls-for-traffic-flow.md)información.

## <a name="service-chaining"></a>Encadenamiento de servicios

Puede configurar rutas definidas por el usuario que apunten a máquinas virtuales de redes virtuales emparejadas como la dirección IP del próximo salto para habilitar el encadenamiento de servicio. El encadenamiento de servicios permite dirigir el tráfico de una red virtual a una aplicación virtual, en una red virtual emparejada, a través de rutas definidas por el usuario.

Puede implementar redes de concentrador y radio, donde la red virtual de concentrador puede hospedar componentes de infraestructura, como una aplicación virtual de red. Todas las redes virtuales de radios del mismo nivel que la red virtual del concentrador. El tráfico puede fluir a través de aplicaciones virtuales de red en la red virtual del concentrador.

El emparejamiento de red virtual permite que el próximo salto de una ruta definida por el usuario sea la dirección IP de una máquina virtual de la red virtual emparejada. Para más información acerca de las rutas definidas por el usuario, consulte [Uso de aplicaciones virtuales de red en una red virtual](../manage/use-network-virtual-appliances-on-a-vn.md).

## <a name="gateways-and-on-premises-connectivity"></a>Puertas de enlace y conectividad local

Cada red virtual, independientemente de si está emparejada con otra red virtual, puede tener su propia puerta de enlace para conectarse a una red local. Cuando se emparejan redes virtuales, también puede configurar la puerta de enlace en la red virtual emparejada como punto de tránsito a una red local. En este caso, la red virtual que usa una puerta de enlace remota no puede tener su propia puerta de enlace. Una red virtual solo puede tener una puerta de enlace que puede ser una puerta de enlace local o remota (en la red virtual emparejada).

## <a name="monitor"></a>Supervisión

Cuando se emparejan dos redes virtuales, se debe configurar un emparejamiento para cada red virtual en el emparejamiento.

Puede supervisar el estado de la conexión de emparejamiento, que puede estar en uno de los siguientes Estados:

-   **Iniciado:** Se muestra cuando se crea el emparejamiento desde la primera red virtual a la segunda red virtual.

-   **Conectado:** Se muestra después de haber creado el emparejamiento desde la segunda red virtual a la primera red virtual. El estado de emparejamiento para la primera red virtual cambia de Iniciado a Conectado. Ambos equipos del mismo nivel de red virtual deben tener el estado conectado antes de establecer un emparejamiento de red virtual correctamente.

-   **Desconectado:** Se muestra si una red virtual se desconecta de otra red virtual.

[infografía de los Estados]

## <a name="next-steps"></a>Pasos siguientes
[Configurar el emparejamiento de redes virtuales](sdn-configure-vnet-peering.md): en este procedimiento, se usa Windows PowerShell para buscar la red lógica del proveedor de HNV para crear dos redes virtuales, cada una con una subred. También se configura el emparejamiento entre las dos redes virtuales.
