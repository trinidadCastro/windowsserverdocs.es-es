---
title: Paso 3 planear la implementación de un clúster con equilibrio de carga
description: En este tema forma parte de la Guía de implementación de acceso remoto en un clúster en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7540c17b-81de-47de-a04f-3247afa26f70
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 12485ddb9cbb70766c018e8f99caa91cfa6ee9da
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861336"
---
# <a name="step-3-plan-a-load-balanced-cluster-deployment"></a>Paso 3 planear la implementación de un clúster con equilibrio de carga

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

El siguiente paso es planear la configuración de equilibrio de carga y la implementación del clúster.  
  
|Tarea|Descripción|  
|----|--------|  
|3.1 planear el equilibrio de carga|Decida si desea usar Windows red equilibrio de carga (NLB) o un equilibrador de carga externo (ELB).|  
|3.2 plan IP-HTTPS|Si no se usa un certificado autofirmado, el servidor de acceso remoto necesita un certificado SSL en cada servidor del clúster, para autenticar conexiones IP-HTTPS.|  
|3.3 plan para las conexiones de cliente VPN|Tenga en cuenta los requisitos para las conexiones de cliente VPN.|  
|3.4 planear el servidor de ubicación de red|Si se hospeda el sitio Web servidor de ubicación de red en el servidor de acceso remoto y no se usa un certificado autofirmado, asegúrese de que cada servidor del clúster tiene un certificado de servidor para autenticar la conexión al sitio Web.|  
  
## <a name="bkmk_2_1_Plan_LB"></a>3.1 planear el equilibrio de carga  
Acceso remoto puede implementarse en un solo servidor o en un clúster de servidores de acceso remoto. El tráfico al clúster puede ser de carga equilibrada para proporcionar alta disponibilidad y escalabilidad para los clientes de DirectAccess. Hay dos opciones de equilibrio de carga:  
  
-   **Windows NLB**-NLB de Windows es una característica de Windows server. Para ello, no necesita hardware adicional porque todos los servidores del clúster son responsables de administrar la carga de tráfico. NLB de Windows admite un máximo de ocho servidores en un clúster de acceso remoto.  
  
-   **Equilibrador de carga externo**-uso de un equilibrador de carga externo requiere hardware externo para administrar la carga de tráfico entre los servidores del clúster de acceso remoto. Además, el uso de un equilibrador de carga externo admite un máximo de 32 servidores de acceso remoto en un clúster. Algunos puntos a tener en cuenta al configurar el equilibrio de carga externo son:  
  
    -   El administrador debe asegurarse de que se usan las direcciones IP Virtual configurado con el Asistente de equilibrio de carga de acceso remoto en los equilibradores de carga externo (por ejemplo, F5 Big-Ip Local Traffic Manager system). Cuando se habilita el equilibrio de carga externo, las direcciones IP de las interfaces internas y externas se promoverá a direcciones IP virtuales y tienen que estar conectadas en los equilibradores de carga. Esto se hace para que el administrador no tiene que cambiar la entrada DNS para el nombre público de la implementación del clúster. Además, los extremos del túnel IPsec se derivan de la direcciones IP del servidor. Si el administrador proporciona diferentes direcciones IP virtuales, el cliente no podrá conectarse al servidor. Vea el ejemplo para configurar DirectAccess con equilibrio de carga externo en 3.1.1 ejemplo de configuración de equilibrador de carga externo.  
  
    -   Muchos equilibradores de carga externo (incluido F5) no admiten el equilibrio de carga de 6to4 e ISATAP. Si el servidor de acceso remoto es un enrutador ISATAP, la función ISATAP debe moverse a otro equipo. Además, cuando la función ISATAP es en un equipo diferente, los servidores de DirectAccess deben tener conectividad IPv6 nativa con el enrutador ISATAP. Tenga en cuenta que esta conectividad debe estar presente antes de configurar DirectAccess.  
  
    -   Equilibrio de carga externo, si Teredo debe usarse, todos los servidores de acceso remoto deben tener dos direcciones IPv4 públicas consecutivas como direcciones IP dedicadas. Las direcciones IP virtuales del clúster también debe tener dos direcciones IPv4 públicas consecutivas. Esto no es cierto para NLB de Windows que solo las direcciones IP virtuales del clúster debe tener dos direcciones IPv4 públicas consecutivas. Si no se usa Teredo, no son necesarias dos direcciones IP consecutivas.  
  
    -   Puede cambiar el Administrador de NLB de Windows al equilibrador de carga externo y viceversa. Tenga en cuenta que el administrador no se puede cambiar desde el equilibrador de carga externo a NLB de Windows si tiene más de 8 servidores en la implementación de equilibrador de carga externo.  
  
### <a name="ELBConfigEx"></a>3.1.1 ejemplo de configuración de equilibrador de carga externo  
Esta sección describen los pasos de configuración para habilitar un equilibrador de carga externo en una nueva implementación de acceso remoto. Cuando se usa un equilibrador de carga externo, el clúster de acceso remoto puede ser similar a la ilustración siguiente, donde los servidores de acceso remoto están conectados a la red corporativa a través de un equilibrador de carga en la red interna y a Internet a través de un equilibrador de carga conectado a la red externa:  
  
![Ejemplo de configuración de equilibrador de carga externo](../../../../media/Step-3-Plan-a-Load-Balanced-Cluster-Deployment/ELBDiagram-URA_Enterprise_NLB-.png)  
  
##### <a name="planning-information"></a>Información de planeación  
  
1.  VIP externas (direcciones IP que el cliente usará para conectarse al acceso remoto) se optó por ser 131.107.0.102, 131.107.0.103  
  
2.  Equilibrador de la red externa self-IP - 131.107.0.245 (Internet), 131.107.1.245 de carga  
  
    La red perimetral (también conocida como zona desmilitarizada y DMZ) es entre el equilibrador de carga en la red externa y el servidor de acceso remoto.  
  
3.  Direcciones IP para el servidor de acceso remoto en la red perimetral - 131.107.1.102, 131.107.1.103  
  
4.  Direcciones IP para el servidor de acceso remoto en la red ELB (es decir, entre el servidor de acceso remoto y el equilibrador de carga en la red interna) - 30.11.1.101, 2006:2005:11:1::101  
  
5.  Cargar el equilibrador de la red interna self-IP - 30.11.1.245 2006:2005:11:1::245 (ELB), 30.1.1.245 2006:2005:1:1::245 (Corpnet)  
  
6.  Se optó por ser 30.1.1.10, 2006:2005:1:1::10 VIP internas (utilizadas para el sondeo de cliente web y de servidor de ubicación de red, si se instalan en los servidores de acceso remoto de direcciones IP)  
  
##### <a name="steps"></a>Pasos  
  
1.  Configurar el adaptador de red externo del servidor de acceso remoto (que está conectado a la red perimetral) con direcciones 131.107.0.102, 131.107.0.103. Este paso es necesario para la configuración de DirectAccess detectar los extremos del túnel IPsec correctos.  
  
2.  Configurar el adaptador de red interno del servidor de acceso remoto (que está conectado a la red ELB) con las direcciones IP de servidor de ubicación de red de sondeo de web (30.1.1.10, 2006:2005:1:1::10). Este paso es necesario para permitir que los clientes tener acceso a la dirección IP de sondeo de la web, por lo que el Asistente de conectividad de red correctamente indica el estado de conexión para DirectAccess. Este paso también permite el acceso al servidor de ubicación de red, si se configura en el servidor de DirectAccess.  
  
    > [!NOTE]  
    > Asegúrese de que el controlador de dominio sea accesible desde el servidor de acceso remoto con esta configuración.  
  
3.  Configurar el servidor de DirectAccess solo en el servidor de acceso remoto.  
  
4.  Habilitar en la configuración de DirectAccess de equilibrio de carga externo. Usar 131.107.1.102 como la dirección IP dedicada (DIP) externa (131.107.1.103 se seleccionará automáticamente), use 30.11.1.101, 2006:2005:11:1::101 como la DIP interna.  
  
5.  Configurar las direcciones IP virtuales (VIP) externa en el equilibrador de carga externo con direcciones 131.107.0.102 y 131.107.0.103. Además, configure las direcciones VIP internas en el equilibrador de carga externo con direcciones 30.1.1.10 y 2006:2005:1:1::10.  
  
6.  Ahora, el servidor de acceso remoto se configurarán con las direcciones IP planeadas y las direcciones IP internas y externas para el clúster se configurará según las direcciones IP planeadas.  
  
## <a name="bkmk_2_2_NLB"></a>3.2 plan IP-HTTPS  
  
1.  **Requisitos del certificado**-seleccionó usar un certificado IP-HTTPS emitido por una entidad de certificación pública o interna (CA) o un certificado autofirmado durante la implementación del servidor de acceso remoto único. Para la implementación del clúster, debe usar el tipo idéntico de certificado en cada miembro del clúster de acceso remoto. Es decir, si usa un certificado emitido por una entidad de certificación pública (recomendado), debe instalar un certificado emitido por una CA pública en cada miembro del clúster. El nombre del sujeto del certificado nuevo debe ser idéntico para el nombre del sujeto del certificado IP-HTTPS que se utiliza actualmente en su implementación. Tenga en cuenta que si está utilizando certificados autofirmados éstas se configurarán automáticamente en cada servidor durante la implementación del clúster.  
  
2.  **Requisitos del prefijo**-acceso remoto permite el equilibrio de carga de tráfico basados en SSL y tráfico de DirectAccess. Para equilibrar la carga de IPv6 basada en el tráfico de DirectAccess, acceso remoto debe examinar la tunelización de IPv4 para todas las tecnologías de transición. Porque se cifra el tráfico de IP-HTTPS, no es posible examinar el contenido del túnel IPv4. Para habilitar el tráfico de IP-HTTPS tener equilibrada de carga, debe asignar un prefijo IPv6 lo bastante ancho como para que se puede asignar un prefijo IPv6 /64 diferente a cada miembro del clúster. Puede configurar un máximo de 32 servidores en un clúster con equilibrio de carga; por lo tanto, debe especificar un /59 prefijo. Este prefijo debe enrutarse a la dirección IPv6 interna del clúster de acceso remoto y se configura en el Asistente para la instalación del servidor de acceso remoto.  
  
    > [!NOTE]  
    > Los requisitos de prefijo son relevantes sólo en una red interna IPv6 habilitado (de solo IPv6 o IPV4 + IPv6). En una red corporativa solo IPv4, se configura automáticamente el prefijo de cliente y el administrador no puede cambiarlo.  
  
## <a name="BKMK_3.3"></a>3.3 plan para las conexiones de cliente VPN  
Hay una serie de consideraciones para las conexiones de cliente VPN:  
  
-   Tráfico del cliente VPN no puede ser de carga equilibrada si las direcciones de cliente VPN se asignan mediante DHCP. Se requiere un grupo de direcciones estáticas.  
  
-   RRAS puede habilitarse en un clúster con equilibrio de carga que se ha implementado para DirectAccess únicamente con **habilitar VPN** en el panel de tareas de la consola de administración de acceso remoto.  
  
-   Los cambios de la VPN completado en la consola de enrutamiento y administración de acceso remoto (rrasmgmt.msc) tendrá que replicarse manualmente en todos los servidores de acceso remoto en el clúster.  
  
-   Para habilitar el tráfico de cliente de VPN IPv6 con equilibrio de carga, debe especificar un prefijo IPv6 de 59 bits.  
  
## <a name="BKMK_nls"></a>3.4 planear el servidor de ubicación de red  
Si está ejecutando el sitio Web servidor de ubicación de red en el servidor de acceso remoto único, durante la implementación seleccionó para usar un certificado emitido por una entidad de certificación interna (CA) o un certificado autofirmado.  Tenga en cuenta lo siguiente:  
  
1.  Cada miembro del clúster de acceso remoto debe tener un certificado para el servidor de ubicación de red que corresponde a la entrada DNS para el sitio Web servidor de ubicación de red.  
  
2.  El certificado para cada servidor de clúster debe emitirse en la misma manera que el certificado en el único acceso remoto servidor red ubicación certificado de servidor actual. Por ejemplo, si usa un certificado emitido por una CA interna, debe instalar un certificado emitido por la CA interna en cada miembro del clúster.  
  
3.  Si usa un certificado autofirmado, un certificado autofirmado se configurarán automáticamente para cada servidor durante la implementación del clúster.  
  
4.  El nombre del sujeto del certificado no debe ser idéntico al nombre de cualquiera de los servidores de la implementación de acceso remoto.  
