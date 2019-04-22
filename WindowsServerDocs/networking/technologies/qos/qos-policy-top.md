---
title: Directiva de calidad de servicio (QoS)
description: En este tema se proporciona información general de la directiva de calidad de servicio (QoS), que le permite usar la directiva de grupo para establecer prioridades de ancho de banda de tráfico de red de determinadas aplicaciones y servicios en Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 16918506-102c-482e-89d3-004ad8d6aabe
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a3ef81825ef6544bc96506e37bc027e0ac2a78db
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820286"
---
# <a name="quality-of-service-qos-policy"></a>Calidad de servicio \(QoS\) directiva

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puedes usar la directiva QoS como un punto central de administración de ancho de banda de red en toda la infraestructura de Active Directory mediante la creación de perfiles de QoS, cuya configuración se distribuye con la directiva de grupo.

>[!NOTE]
>  Además de este tema, está disponible la siguiente documentación de la directiva de QoS.  
>   
>  - [Introducción a directiva de QoS](qos-policy-get-started.md)
>  - [Administrar la directiva de QoS](qos-policy-manage.md)
>  - [Preguntas más frecuentes sobre la directiva de QoS](qos-policy-faq.md)

Las directivas de QoS se aplican a una sesión de inicio de sesión de usuario o un equipo como parte de un objeto de directiva de grupo \(GPO\) que se han vinculado a un contenedor de Active Directory, como un dominio, sitio o unidad organizativa \(OU\).

Administración del tráfico de QoS se produce debajo de la capa de aplicación, lo que significa que las aplicaciones existentes no debe modificarse para beneficiarse de las ventajas proporcionadas por las directivas de QoS.

## <a name="operating-systems-that-support-qos-policy"></a>Sistemas operativos que admiten la directiva de QoS

Puede usar la directiva de QoS para administrar el ancho de banda para equipos o usuarios con los siguientes sistemas operativos de Microsoft.

- Windows Server 2016
- Windows 10
- Windows Server 2012 R2
- Windows 8.1
- Windows Server 2012
- Windows 8
- Windows Server 2008 R2
- Windows 7
- Windows Server 2008
- Windows Vista

### <a name="location-of-qos-policy-in-group-policy"></a>Ubicación de la directiva de QoS en la directiva de grupo

En el Editor de administración de la directiva de grupo de 2016 con Windows Server, la ruta de acceso a la directiva de QoS para la configuración del equipo es la siguiente.

**Directiva predeterminada de dominio | Configuración del equipo | Directivas | Configuración de Windows | Directiva\-en función de QoS**

En la siguiente imagen se ilustra esta ruta de acceso.

![Ubicación de la directiva de QoS en la directiva de grupo](../../media/QoS/QoS-Gp.jpg)

En el Editor de administración de la directiva de grupo de 2016 con Windows Server, la ruta de acceso a la directiva de QoS para la configuración de usuario es la siguiente.

**Directiva predeterminada de dominio | Configuración de usuario | Directivas | Configuración de Windows | Directiva\-en función de QoS**

De forma predeterminada no se configura ninguna directiva de QoS.

## <a name="why-use-qos-policy"></a>¿Por qué usar la directiva de QoS?
  
A medida que aumenta el tráfico de la red, es importante que se va a equilibrar el rendimiento de red con el costo del servicio: pero no es normalmente más fácil de establecer prioridades y administrar el tráfico de red.

En la red, de misión\-críticos y latencia\-deben competir aplicaciones sensibles al ancho de banda de red en el tráfico de menor prioridad. Al mismo tiempo, algunos usuarios y equipos con el rendimiento de red específicas pueden requerir requisitos diferencian los niveles de servicio.

A menudo primero aparecen los retos de proporcionar niveles de rendimiento de red rentable y predecible a través de la red de área extensa \(WAN\) conexiones o con aplicaciones sensibles a la latencia, como voz sobre IP \(VoIP\) y streaming de vídeo. Sin embargo, se aplica el objetivo final de proporcionar niveles de servicio de red predecible a cualquier entorno de red \(por ejemplo, red de área de una empresa local\)y a más de las aplicaciones de VoIP, como línea personalizada de su compañía\-de\-aplicaciones empresariales.
  
QoS basada en directivas es la herramienta de administración de ancho de banda de red que le proporciona control de red - basado en las aplicaciones, usuarios y equipos. 

Cuando se usa la directiva de QoS, las aplicaciones no es necesario que se escribirá para las interfaces de programación de aplicaciones específico \(API\). Esto le permite usar QoS con las aplicaciones existentes. Además, QoS basada en directivas aprovecha la infraestructura de administración existente, dado que QoS basada en directivas se basa en la directiva de grupo.

## <a name="define-qos-priority-through-a-differentiated-services-code-point-dscp"></a>Definir la prioridad de QoS a través de un punto de código de servicios diferenciados \(DSCP\)
  
Puede crear directivas de QoS que definen la prioridad del tráfico de red con un punto de código de servicios diferenciados \(DSCP\) valor que se asigna a distintos tipos de tráfico de red. 

El valor DSCP permite aplicar un valor \(0 a 63\) dentro del tipo de servicio \(TOS\) campo del encabezado de un paquete IPv4 y en el campo de clase de tráfico de IPv6. 

El valor de DSCP proporciona clasificación del tráfico de red en el protocolo de Internet \(IP\) nivel, que los enrutadores que se usan para decidir el comportamiento del tráfico de puesta en cola. 

Por ejemplo, puede configurar los enrutadores para que sitúen los paquetes con valores de DSCP específicos en una de las tres colas: alta prioridad, más recomendable o inferior al mejor esfuerzo. 

Tráfico de red críticas, que se encuentra en la cola de prioridad alta, tiene preferencia sobre el tráfico restante.

### <a name="limit-network-bandwidth-use-per-application-with-throttle-rate"></a>Limitar el uso del ancho de banda de red por la aplicación con la velocidad del Acelerador

También puede limitar el tráfico de red saliente de la aplicación mediante la especificación de una tasa de limitación en la directiva de QoS.

Una directiva de QoS que define los valores de limitación determina la frecuencia de tráfico de red saliente. Por ejemplo, para administrar los costes de WAN, un departamento de TI podría implementar un contrato de nivel de servicio que especifica que un servidor de archivos nunca puede proporcionar descargas más allá de un índice específico.  

### <a name="use-qos-policy-to-apply-dscp-values-and-throttle-rates"></a>Valores de uso de directivas de QoS para aplicar el valor DSCP y limitar las tasas

También puede usar Directiva de QoS para aplicar valores de DSCP y limitar las tasas de tráfico de red saliente al siguiente:

- Aplicación de envío y la ruta de acceso de directorio

- Direcciones de IPv4 o IPv6 de origen y destino o prefijos de dirección

- Protocolo: protocolo de Control de transmisión \(TCP\) y protocolo de datagramas de usuario \(UDP\)

- Puertos de origen y destino y los intervalos de puertos \(TCP o UDP\)

- Grupos específicos de usuarios o equipos a través de la implementación de directiva de grupo

Mediante el uso de estos controles, puede especificar una directiva de QoS con un valor DSCP de 46 para una aplicación de VoIP, lo que permite colocar los paquetes de VoIP en una cola de baja latencia y los enrutadores, o puede usar una directiva de QoS para limitar un conjunto de tráfico de salida de los servidores en 512 kilobytes por segundo <c1/>KBps\) cuando se envían desde el puerto TCP 443.

También puede aplicar directivas de QoS a una aplicación concreta que tenga requisitos especiales de ancho de banda. Para obtener más información, consulte [escenarios de directiva de QoS](qos-policy-scenarios.md).
  
## <a name="advantages-of-qos-policy"></a>Ventajas de la directiva de QoS

Con la directiva de QoS, puede configurar y aplicar directivas de QoS que no se puede configurar en los conmutadores y enrutadores. Directiva de QoS ofrece las siguientes ventajas.
  
1. **Nivel de detalle:** Es difícil crear directivas de QoS de nivel de usuario en los enrutadores o conmutadores, especialmente si el equipo del usuario está que bien configurado mediante el uso de la asignación de direcciones IP dinámicas o si el equipo no está conectado al conmutador fijo o puertos de enrutador, como sucede con frecuencia con equipos portátiles. En cambio, directiva de QoS facilita la configuración de un usuario\-el nivel de directiva de QoS en un controlador de dominio y propagar la directiva para el equipo del usuario.
2. **Flexibilidad**. Independientemente de dónde y cómo se conecta un equipo a la red, se aplica la directiva de QoS: el equipo puede conectarse mediante Wi-Fi o Ethernet desde cualquier ubicación. Para el usuario\-nivel las directivas de QoS, la QoS se aplica la directiva en cualquier dispositivo compatible en cualquier ubicación donde el usuario inicia sesión.
3. **Seguridad:** Si el departamento de TI cifra el tráfico de los usuarios de extremo a extremo mediante la seguridad de protocolo de Internet \(IPsec\), no se puede clasificar el tráfico en los enrutadores basándose en toda la información sobre la capa IP en el paquete \(, por ejemplo, un El puerto TCP\). Sin embargo, mediante la directiva de QoS, puede clasificar los paquetes en el dispositivo final para indicar la prioridad de los paquetes en el encabezado IP antes de que las cargas IP se cifran y se envían los paquetes.
4. **Rendimiento:** Algunas funciones de calidad de servicio, como la limitación, se realizan mejor cuando están más cerca del origen. Directiva de QoS mueve dichas funciones de QoS más cercanos al origen.
5. **Facilidad de uso:** Directiva de QoS mejora la administración de red de dos maneras:

    **a**. Porque se basa en la directiva de grupo, puede usar la directiva de QoS para configurar y administrar un conjunto de directivas de QoS de usuario/equipo siempre que sea necesario y en el equipo de un controlador de dominio central.

    **b**. Directiva de QoS facilita la configuración de usuario o equipo, ya que proporciona un mecanismo para especificar las directivas por localizador uniforme de recursos \(URL\) en lugar de especificar directivas basadas en las direcciones IP de cada uno de los servidores donde se necesitan las directivas de QoS que se aplicará. Por ejemplo, suponga que la red tiene un clúster de servidores que comparten una dirección URL comunes. Mediante la directiva de QoS, puede crear una directiva según la dirección URL comunes, en lugar de crear una directiva para cada servidor del clúster, con cada directiva según la dirección IP de cada servidor.

El siguiente tema en esta guía, consulte [Introducción a directiva de QoS](qos-policy-get-started.md).

