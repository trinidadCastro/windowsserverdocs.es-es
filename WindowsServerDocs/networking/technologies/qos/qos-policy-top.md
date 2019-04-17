---
title: Calidad de la directiva de servicio (QoS)
description: Este tema proporciona una visión general de la directiva de calidad de servicio (QoS), que permite usar la directiva de grupo para establecer prioridades de ancho de banda de tráfico de red de determinadas aplicaciones y servicios en Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 16918506-102c-482e-89d3-004ad8d6aabe
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 76405c677fe7eb4d51f9c190499b909ac2fe4a0c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="quality-of-service-qos-policy"></a>Calidad de servicio \(QoS\) Directiva

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar la directiva de QoS como un punto central de administración de ancho de banda de red en toda la infraestructura de Active Directory mediante la creación de perfiles de QoS, cuya configuración se distribuye con la directiva de grupo.

>[!NOTE]
>  Además de este tema, la siguiente documentación de la directiva de QoS está disponible.  
>   
>  - [Introducción a directiva QoS](qos-policy-get-started.md)
>  - [Administrar la directiva de QoS](qos-policy-manage.md)
>  - [Preguntas más frecuentes sobre la directiva de QoS](qos-policy-faq.md)

Las directivas de QoS se aplican a un equipo o una sesión de inicio de sesión de usuario como parte de un objeto de directiva de grupo \(GPO\) que ha vinculado a un contenedor de Active Directory, como un dominio, un sitio o una unidad organizativa \(OU\).

Administración del tráfico QoS se produce por debajo del nivel de aplicación, lo que significa que no es necesario que las aplicaciones existentes modificarse para aprovechar las ventajas de las ventajas que proporcionan las directivas de QoS.

## <a name="operating-systems-that-support-qos-policy"></a>Sistemas operativos que admiten la directiva de QoS

Puedes usar la directiva de QoS para administrar el ancho de banda para los equipos o los usuarios con los siguientes sistemas operativos de Microsoft.

- Windows Server 2016
- Windows 10
- Windows Server 2012 R2
- Windows 8.1
- Windows Server 2012
- Windows 8
- Windows Server 2008 R2
- Windows 7
- Windows Server 2008
- Windows Vista

### <a name="location-of-qos-policy-in-group-policy"></a>Ubicación de la directiva de QoS en la directiva de grupo

En el Editor de administración de la directiva de grupo de 2016 con Windows Server, la ruta de acceso a la directiva de QoS para la configuración del equipo es la siguiente.

**Directiva de dominio predeterminada | Configuración del equipo | Directivas | Configuración de Windows | QoS basada en contraseñas\**

En la siguiente imagen se muestra esta ruta de acceso.

![Ubicación de la directiva de QoS en la directiva de grupo](../../media/QoS/QoS-Gp.jpg)

En el Editor de administración de la directiva de grupo de 2016 con Windows Server, la ruta de acceso a la directiva de QoS para la configuración de usuario es la siguiente.

**Directiva de dominio predeterminada | Configuración de usuario | Directivas | Configuración de Windows | QoS basada en contraseñas\**

De manera predeterminada, no se configura ninguna directiva QoS.

## <a name="why-use-qos-policy"></a>¿Por qué usar la directiva de QoS?
  
A medida que aumenta el tráfico de la red, es cada vez más importante para TI equilibrio entre rendimiento de red y el costo de servicio: pero el tráfico de red no es fácil normalmente priorizar y administrar.

En la red, mission\ latency\ sensibles aplicaciones deben competir por el ancho de banda de red contra el tráfico de prioridad más bajo. Al mismo tiempo, algunos usuarios y equipos con el rendimiento de red específica requisitos pueden requerir diferencian niveles de servicio.

Los desafíos de la prestación de niveles de rendimiento de red predecible y rentable aparecen a menudo primero las conexiones \(WAN\) de red de área extensa o con las aplicaciones sensibles a la latencia, como voz sobre IP \(VoIP\) y streaming de vídeo. Sin embargo, el objetivo final de la prestación de niveles de servicio de red predecible se aplica a cualquier entorno de red \ (por ejemplo, una empresa área local red\) y a más de las aplicaciones de VoIP, como aplicaciones de line\ descripción\-negocio personalizadas de tu empresa.
  
QoS basada en directivas es la herramienta de administración de ancho de banda de red que proporciona el control de la red - basada en aplicaciones, los usuarios y equipos. 

Al usar la directiva de QoS, las aplicaciones no es necesario para la aplicación específica programación interfaces \(APIs\). Esto te ofrece la posibilidad de usar QoS con las aplicaciones existentes. Además, basada en directivas QoS aprovecha las ventajas de la infraestructura de administración existentes, porque QoS basada en directivas está integrado en la directiva de grupo.

## <a name="define-qos-priority-through-a-differentiated-services-code-point-dscp"></a>Definir la prioridad de QoS a través de un punto de código de servicios diferenciados \(DSCP\)
  
Puedes crear directivas de QoS que definen la prioridad de tráfico de red con un valor de punto de código de servicios diferenciados \(DSCP\) que se asigna a diferentes tipos de tráfico de red. 

El valor DSCP permite aplicar un valor \(0–63\) dentro del campo de tipo de servicio \(TOS\) en el encabezado de un paquete IPv4 y dentro del campo de la clase de tráfico en IPv6. 

El valor DSCP proporciona la clasificación del tráfico de red en el nivel de protocolo de Internet \(IP\), enrutadores usan para decidir el tráfico de comportamiento de cola. 

Por ejemplo, puedes configurar enrutadores para colocar los paquetes con valores DSCP específicos en una de las tres colas: mínimo esfuerzo de prioridad alta, o inferiores a mejor esfuerzo. 

El tráfico de red esenciales, que está en la cola de prioridad alta, tiene preferencia sobre otro tráfico.

### <a name="limit-network-bandwidth-use-per-application-with-throttle-rate"></a>Limitar el uso de ancho de banda de red por la aplicación con la velocidad de aceleración

También puede limitar el tráfico de red saliente de la aplicación especificando una velocidad de aceleración en directiva QoS.

Una directiva de QoS que define el límites límites determina la velocidad de tráfico de red saliente. Por ejemplo, para administrar los costos de WAN, un departamento de TI puede implementar un contrato de nivel de servicio que especifica que un servidor de archivos no puede proporcionar nunca descargas más allá de un tipo específico.  

### <a name="use-qos-policy-to-apply-dscp-values-and-throttle-rates"></a>Usar la directiva de QoS para aplicar DSCP valores y limitar los tipos de

También puedes usar la directiva de QoS para aplicar valores DSCP y limitar los tipos de tráfico de red saliente a la siguiente:

- Aplicación de envío y la ruta de acceso de directorio

- Direcciones de origen y destino IPv4 o IPv6 o prefijos de direcciones

- Protocolo - Control de transporte de protocolo \(TCP\) y usuario Protocolo de datagramas \(UDP\)

- Puertos de origen y destino e intervalos de puertos \(TCP or UDP\)

- Determinados grupos de usuarios o equipos a través de la implementación de directiva de grupo

Al usar estos controles, puedes especificar una directiva de QoS con un valor DSCP 46 para una aplicación VoIP, habilitar los enrutadores colocar VoIP paquetes en una cola de latencia baja, o puedes usar una directiva de QoS limitar un conjunto del tráfico saliente los servidores 512 kilobytes por segundo \(KBps\) al enviar desde el puerto TCP 443.

También puedes aplicar directiva QoS a una aplicación particular que tiene requisitos de ancho de banda especial. Para obtener más información, consulta [escenarios de la directiva de QoS](qos-policy-scenarios.md).
  
## <a name="advantages-of-qos-policy"></a>Ventajas de directiva de QoS

Con la directiva de QoS, puedes configurar y aplicar directivas de QoS que no se puede configurar en los enrutadores y conmutadores. Directiva de QoS proporciona las siguientes ventajas.
  
1. **Nivel de detalle:** es difícil crear directivas de QoS de nivel de usuario en enrutadores o modificadores, especialmente si el equipo del usuario está configurado mediante el uso de la dirección IP dinámica asignación de direcciones, o bien, si el equipo no está conectado a conmutador fijo o puertos del enrutador, como sucede con frecuencia en los equipos portátiles. En cambio, directiva de QoS facilita configurar una directiva de QoS user\ en un controlador de dominio y propagar la directiva para el equipo del usuario.
2. **Flexibilidad**. Independientemente de dónde y cómo un equipo se conecte a la red, se aplica la directiva de QoS: el equipo puede conectarse con Wi-Fi o Ethernet desde cualquier ubicación. Para las directivas de QoS user\ nivel, se aplica la directiva de QoS en cualquier dispositivo compatible en cualquier ubicación donde el usuario inicie sesión.
3. **Seguridad:** si el departamento de TI cifra el tráfico de los usuarios de principio a fin usando el protocolo de Internet seguridad \(IPsec\), no se puede clasificar el tráfico en enrutadores en toda la información sobre la capa IP en el paquete \ (por ejemplo, un port\ TCP). Sin embargo, mediante la directiva de QoS, puede clasificar paquetes en el dispositivo final para indicar la prioridad de los paquetes en el encabezado IP antes de las cargas IP se cifran y se envían los paquetes.
4. **Rendimiento:** algunas funciones de QoS, como "limitación", se realizan mejor cuando están más cercanos del origen. Directiva de QoS mueve dichas funciones de QoS más cercanos al origen.
5. **Facilidad de uso:** directiva QoS mejora la capacidad de administración de red de dos maneras:

    **a**. Dado que se basa en la directiva de grupo, puedes usar directiva QoS para configurar y administrar un conjunto de directivas de equipo o usuario QoS siempre que sea necesario y en el equipo de un controlador de dominio central.

    **b**. Directiva de QoS facilita la configuración de usuario o equipo al proporcionar un mecanismo para especificar las directivas de \(URL\) localizador uniforme de recursos en lugar de especificar directivas basadas en las direcciones IP de cada uno de los servidores de donde deben aplicar directivas de QoS. Por ejemplo, supongamos que la red tiene un clúster de servidores que comparten una dirección URL comunes. Mediante la directiva de QoS, puedes crear una directiva según la dirección URL comunes, en lugar de crear una directiva para cada servidor del clúster, con cada directiva basada en la dirección IP de cada servidor.

El siguiente tema en esta guía, encontrarás [Getting Started with directiva QoS](qos-policy-get-started.md).

