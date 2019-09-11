---
title: Directiva de calidad de servicio (QoS)
description: En este tema se proporciona información general sobre la Directiva de calidad de servicio (QoS), que permite usar directiva de grupo para priorizar el ancho de banda del tráfico de red de aplicaciones y servicios específicos en Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 16918506-102c-482e-89d3-004ad8d6aabe
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8f139f047218c4c8da3a462206d4541c1fc255ec
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871860"
---
# <a name="quality-of-service-qos-policy"></a>Directiva QoS \(\) de calidad de servicio

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puedes usar la directiva QoS como un punto central de administración de ancho de banda de red en toda la infraestructura de Active Directory mediante la creación de perfiles de QoS, cuya configuración se distribuye con la directiva de grupo.

>[!NOTE]
>  Además de este tema, está disponible la siguiente documentación de la Directiva de QoS.  
>   
>  - [Introducción con la Directiva de QoS](qos-policy-get-started.md)
>  - [Administrar la directiva QoS](qos-policy-manage.md)
>  - [Preguntas más frecuentes sobre la directiva QoS](qos-policy-faq.md)

Las directivas de QoS se aplican a una sesión de inicio de sesión de usuario o a \(un\) equipo como parte de un GPO de directiva de grupo objeto que se ha vinculado a un contenedor Active Directory, \(como\)un dominio, un sitio o una unidad organizativa.

La administración del tráfico de QoS se produce debajo del nivel de aplicación, lo que significa que no es necesario modificar las aplicaciones existentes para beneficiarse de las ventajas que proporcionan las directivas de QoS.

## <a name="operating-systems-that-support-qos-policy"></a>Sistemas operativos compatibles con la Directiva de QoS

Puede usar la directiva QoS para administrar el ancho de banda de los equipos o usuarios con los siguientes sistemas operativos de Microsoft.

- Windows Server 2016
- Windows 10
- Windows Server 2012 R2
- Windows 8.1
- Windows Server 2012
- Windows 8
- Windows Server 2008 R2
- Windows 7
- Windows Server 2008
- Windows Vista

### <a name="location-of-qos-policy-in-group-policy"></a>Ubicación de la Directiva de QoS en directiva de grupo

En Windows Server 2016 Editor de administración de directivas de grupo, la ruta de acceso a la Directiva de QoS para la configuración del equipo es la siguiente.

**Directiva predeterminada de dominio | Configuración del equipo | Directivas | Configuración de Windows | QoS\-basada en directivas**

Esta ruta de acceso se muestra en la siguiente imagen.

![Ubicación de la Directiva de QoS en directiva de grupo](../../media/QoS/QoS-Gp.jpg)

En Windows Server 2016 Editor de administración de directivas de grupo, la ruta de acceso a la Directiva de QoS para la configuración de usuario es la siguiente.

**Directiva predeterminada de dominio | Configuración de usuario | Directivas | Configuración de Windows | QoS\-basada en directivas**

De forma predeterminada, no se configuran directivas QoS.

## <a name="why-use-qos-policy"></a>¿Por qué usar la Directiva de QoS?
  
A medida que aumenta el tráfico en la red, es cada vez más importante equilibrar el rendimiento de la red con el costo del servicio, pero el tráfico de red no suele ser fácil de priorizar y administrar.

En la red, las\-aplicaciones críticas de\-misión crítica y de latencia deben competir por el ancho de banda de red frente al tráfico de menor prioridad. Al mismo tiempo, algunos usuarios y equipos con requisitos de rendimiento de red específicos pueden requerir niveles de servicio diferenciados.

Los desafíos de proporcionar niveles de rendimiento de red rentables y predecibles suelen aparecer en conexiones WAN \(\) de red de área extensa o con aplicaciones sensibles a la latencia, \(como voz sobre IP VoIP\) y streaming de vídeo. Sin embargo, el objetivo final de proporcionar niveles de servicio de red predecibles se aplica \(a cualquier entorno de red, por ejemplo,\)una red de área local de las empresas y a más que las aplicaciones de VoIP, como la líneapersonalizadadelaempresa.\-de\-aplicaciones empresariales.
  
QoS basada en Directiva es la herramienta de administración del ancho de banda de red que proporciona control de red basado en aplicaciones, usuarios y equipos. 

Cuando se usa la Directiva de QoS, no es necesario escribir las aplicaciones para las API \(\)específicas de interfaces de programación de aplicaciones. Esto le ofrece la posibilidad de usar QoS con las aplicaciones existentes. Además, la QoS basada en directivas aprovecha la infraestructura de administración existente, ya que la QoS basada en Directiva está integrada en directiva de grupo.

## <a name="define-qos-priority-through-a-differentiated-services-code-point-dscp"></a>Definir la prioridad de QoS a través de \(un punto de código de servicios diferenciados DSCP\)
  
Puede crear directivas de QoS que definan la prioridad del tráfico de red \(con\) un servicios diferenciados valor de DSCP de punto de código que asigne a distintos tipos de tráfico de red. 

El DSCP le permite aplicar un \(valor de 0 a 63\) en\) el campo tipo de \(servicio en el encabezado de un paquete IPv4 y dentro del campo clase de tráfico en IPv6. 

El valor de DSCP proporciona la clasificación del tráfico de red \(en\) el nivel IP del Protocolo de Internet, que los enrutadores usan para decidir el comportamiento de Traffic Queue Server. 

Por ejemplo, puede configurar los enrutadores para colocar los paquetes con valores de DSCP específicos en una de las tres colas: alta prioridad, mejor esfuerzo o inferior al mejor esfuerzo. 

El tráfico de red crítico, que se encuentra en la cola de prioridad alta, tiene preferencia sobre otro tráfico.

### <a name="limit-network-bandwidth-use-per-application-with-throttle-rate"></a>Limitar el uso de ancho de banda de red por aplicación con velocidad de acelerador

También puede limitar el tráfico de red saliente de una aplicación especificando una velocidad de limitación en la directiva QoS.

Una directiva de QoS que define los límites de limitación determina la velocidad del tráfico de red saliente. Por ejemplo, para administrar los costos de la WAN, un departamento de ti podría implementar un acuerdo de nivel de servicio que especifique que un servidor de archivos nunca puede proporcionar descargas más allá de una tarifa específica.  

### <a name="use-qos-policy-to-apply-dscp-values-and-throttle-rates"></a>Usar la directiva QoS para aplicar valores de DSCP y tasas de limitación

También puede usar la directiva QoS para aplicar valores de DSCP y tasas de limitación para el tráfico de red saliente a lo siguiente:

- Envío de la aplicación y la ruta de acceso del directorio

- Direcciones IPv4 o IPv6 de origen y de destino o prefijos de dirección

- Protocolo: Protocolo de control \(de\) transmisión TCP y protocolo \(de datagramas de usuario UDP\)

- Puertos de origen y de destino e \(intervalos de puertos TCP o UDP\)

- Grupos específicos de usuarios o equipos a través de la implementación en directiva de grupo

Mediante el uso de estos controles, puede especificar una directiva de QoS con un valor de DSCP de 46 para una aplicación de VoIP, lo que permite a los enrutadores colocar paquetes de VoIP en una cola de baja latencia, o puede usar una directiva de QoS para limitar el tráfico de salida de un conjunto de servidores a 512 kilobytes por segundo <C1/>Kbps\) al enviar desde el puerto TCP 443.

También puede aplicar la Directiva de QoS a una aplicación determinada que tenga requisitos de ancho de banda especiales. Para obtener más información, vea [escenarios de directivas de QoS](qos-policy-scenarios.md).
  
## <a name="advantages-of-qos-policy"></a>Ventajas de la Directiva de QoS

Con la Directiva de QoS, puede configurar y aplicar directivas de QoS que no se pueden configurar en enrutadores y conmutadores. La directiva QoS ofrece las siguientes ventajas.
  
1. **Nivel de detalle:** Es difícil crear directivas QoS de nivel de usuario en enrutadores o conmutadores, especialmente si el equipo del usuario se configura mediante la asignación de direcciones IP dinámicas o si el equipo no está conectado a puertos fijos de conmutadores o enrutadores, como suele ser el caso con equipos portátiles. Por el contrario, la Directiva de QoS facilita la configuración de\-una directiva de QoS de nivel de usuario en un controlador de dominio y propaga la Directiva al equipo del usuario.
2. **Flexibilidad**. Independientemente de dónde o cómo se conecte un equipo a la red, se aplicará la Directiva de QoS: el equipo puede conectarse mediante Wi-Fi o Ethernet desde cualquier ubicación. En el\-caso de las directivas QoS de nivel de usuario, la directiva QoS se aplica a cualquier dispositivo compatible en cualquier ubicación donde el usuario inicie sesión.
3. **Bursátil** Si el Departamento de ti cifra el tráfico de los usuarios de un extremo a otro mediante el \(protocolo\)de seguridad de Internet IPSec, no se puede clasificar el tráfico en enrutadores en función de \(la información sobre la capa de IP en el paquete, por ejemplo, una Puerto\)TCP. Sin embargo, mediante el uso de la Directiva de QoS, puede clasificar paquetes en el dispositivo final para indicar la prioridad de los paquetes en el encabezado IP antes de que se cifren las cargas de IP y se envíen los paquetes.
4. **Rendimiento** Algunas funciones de QoS, como la limitación, se realizan mejor cuando están más cerca del origen. La directiva QoS mueve las funciones QoS más cercanas al origen.
5. **Capacidad** La directiva QoS mejora la capacidad de administración de la red de dos maneras:

    **un**. Dado que se basa en directiva de grupo, puede usar la directiva QoS para configurar y administrar un conjunto de directivas QoS de usuario/equipo siempre que sea necesario, y en un equipo central de controlador de dominio.

    **b**. La directiva QoS facilita la configuración de usuarios y equipos al proporcionar un mecanismo para especificar directivas mediante la \(dirección\) URL de localizador uniforme de recursos en lugar de especificar directivas basadas en las direcciones IP de cada uno de los servidores donde se necesitan las directivas de QoS que se va a aplicar. Por ejemplo, suponga que la red tiene un clúster de servidores que comparten una dirección URL común. Mediante el uso de la Directiva de QoS, puede crear una directiva basada en la dirección URL común, en lugar de crear una directiva para cada servidor del clúster, con cada directiva basada en la dirección IP de cada servidor.

Para el siguiente tema de esta guía, consulte [Introducción con la Directiva de QoS](qos-policy-get-started.md).

