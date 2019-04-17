---
title: Procesamiento de solicitudes de conexión
description: Este tema proporciona una visión general de la solicitud de conexión de servidor de directivas de red en Windows Server 2016 de procesamiento.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 849d661a-42c1-4f93-b669-6009d52aad39
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6756c89dadbd01998ffef6a6d785c079076f2532
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="connection-request-processing"></a>Procesamiento de solicitudes de conexión

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para obtener información sobre el procesamiento en el servidor de directivas de red en Windows Server 2016 de solicitud de conexión.

>[!NOTE]
>Además de este tema, la siguiente solicitud de conexión procesamiento documentación está disponible.
> - [Directivas de la solicitud de conexión](nps-crp-crpolicies.md)
> - [Nombres de dominio Kerberos](nps-crp-realm-names.md)
> - [Grupos de servidores RADIUS remotos](nps-crp-rrsg.md)

Puedes usar el procesamiento de solicitudes de conexión para especificar dónde se realiza la autenticación de solicitudes de conexión - en el equipo local o en un servidor RADIUS remoto que es un miembro de un grupo de servidores RADIUS remotos. 

Si quieres que el servidor local con el servidor de directivas de redes (NPS) para realizar la autenticación para las solicitudes de conexión, puedes usar la directiva de solicitud de conexión predeterminada sin ninguna configuración adicional. En función de la directiva predeterminada, NPS autentica a los usuarios y equipos que tienen una cuenta en el dominio local y de dominios de confianza.

Si quieres enviar solicitudes de conexión a un NPS remoto u otro servidor RADIUS, crear un grupo de servidores RADIUS remotos y, a continuación, configurar una directiva de solicitud de conexión que reenvíe solicitudes a ese grupo de servidores RADIUS remotos. Con esta configuración, NPS puede reenviar solicitudes de autenticación a los servidores RADIUS, y se pueden autenticar los usuarios con cuentas de dominios de confianza.

La siguiente ilustración muestra la ruta de acceso de un mensaje de solicitud de acceso desde un servidor de acceso de red a un proxy RADIUS y, a continuación, en un servidor de radio en un grupo de servidores RADIUS remotos. En el servidor proxy RADIUS, el servidor de acceso de red está configurado como un cliente RADIUS; y en cada servidor RADIUS, el proxy de radio está configurado como un cliente RADIUS.


![Procesamiento de solicitudes de conexión de NPS](../../media/Nps-Connection-Request-Processing/Nps-Connection-Request-Processing.jpg)


>[!NOTE]
>Los servidores de acceso de red que use con NPS pueden ser dispositivos de puerta de enlace que son compatibles con el protocolo RADIUS, como puntos de acceso inalámbrico 802.1X y conmutadores de autenticación, servidores que ejecutan el acceso remoto que están configurados como VPN o servidores de acceso telefónico, o de otros dispositivos compatibles con el radio.

Si quieres NPS procese algunas solicitudes de autenticación localmente y reenvíe otras solicitudes a un grupo de servidores RADIUS remotos, configurar más de una directiva de solicitud de conexión.

Para configurar una directiva de solicitud de conexión que especifica a qué grupo de servidores RADIUS o de NPS procesa las solicitudes de autenticación, consulta las directivas de la solicitud de conexión.

Para especificar NPS u otros servidores RADIUS para que la autenticación se reenvían solicitudes, verás los grupos de servidores RADIUS remotos.

## <a name="nps-as-a-radius-server-connection-request-processing"></a>NPS como una procesamiento de solicitud de conexión del servidor de radio

Cuando se usa NPS como un servidor RADIUS, mensajes de radio proporcionan autenticación, autorización y contabilidad para conexiones de acceso de red de la siguiente manera:

1. Servidores de acceso, como los servidores de acceso telefónico, servidores VPN y puntos de acceso inalámbrico, reciban las solicitudes de conexión de los clientes de acceso. 

2. El servidor de acceso configurado para usar RADIUS como el protocolo de autenticación, autorización y administración de cuentas, crea un mensaje de solicitud de acceso y envía al servidor NPS. 

3. El servidor NPS evalúa el mensaje de solicitud de acceso. 

4. Si es necesario, el servidor NPS envía un mensaje de desafío de acceso al servidor de acceso. El servidor de acceso procesa el desafío y envía una solicitud de acceso actualizada en el servidor NPS. 

5. Se comprueban las credenciales del usuario y las propiedades de marcado de la cuenta de usuario se obtienen mediante una conexión segura a un controlador de dominio. 

6. El intento de conexión se autoriza con las propiedades de marcación rápida de las directivas de red y la cuenta de usuario. 

7. Si el intento de conexión se autentica y autorización, el servidor NPS envía un mensaje de aceptación de acceso al servidor de acceso. Si el intento de conexión no autenticado o no autorizado, el servidor NPS envía un mensaje de denegación de acceso al servidor de acceso. 

8. El servidor de acceso completa el proceso de conexión con el cliente de acceso y envía un mensaje de solicitud de cuentas en el servidor NPS, donde se registra el mensaje. 

9. El servidor NPS envía una respuesta de cuenta al servidor de acceso. 

>[!NOTE]
>El servidor de acceso también envía mensajes de solicitud de contabilidad durante el tiempo en que se establece la conexión, cuando se cierra la conexión de cliente de acceso y cuando se inicia y detiene el servidor de acceso.

## <a name="nps-as-a-radius-proxy-connection-request-processing"></a>NPS como una procesamiento de solicitud de conexión proxy de radio

Cuando se usa NPS como proxy RADIUS entre un cliente RADIUS y un servidor de radio, mensajes RADIUS de red de acceso a la conexión se reenvían intentos de la siguiente manera:

1. Servidores de acceso, como los servidores de acceso telefónico, servidores de red privada virtual (VPN) y puntos de acceso inalámbrico, reciban las solicitudes de conexión de los clientes de acceso.

2. El servidor de acceso configurado para usar RADIUS como el protocolo de autenticación, autorización y cuentas, crea un mensaje de solicitud de acceso y envía al servidor NPS que se usa como el proxy NPS RADIUS.

3. El proxy NPS RADIUS recibe el mensaje de solicitud de acceso y, en función de las directivas de la solicitud de conexión configuradas localmente, determina dónde reenviar el mensaje de solicitud de acceso.

4. El proxy NPS RADIUS reenvía el mensaje de solicitud de acceso al servidor RADIUS adecuado.

5. El servidor RADIUS evalúa el mensaje de solicitud de acceso.

6. Si es necesario, el servidor RADIUS envía un mensaje de desafío de acceso para el proxy NPS RADIUS, donde se reenvía al servidor de acceso. El servidor de acceso procesa el desafío con el cliente de acceso y envía una solicitud de acceso actualizada al proxy NPS RADIUS, donde se reenvía al servidor RADIUS.

7. El servidor RADIUS autentica y autoriza el intento de conexión.

8. Si el intento de conexión se autentica y autorizado, el servidor RADIUS envía un mensaje de aceptación de acceso para el proxy NPS RADIUS, donde se reenvía al servidor de acceso. Si el intento de conexión no autenticado o no autorizado, el servidor RADIUS envía un mensaje de denegación de acceso para el proxy NPS RADIUS, donde se reenvía al servidor de acceso.

9. El servidor de acceso completa el proceso de conexión con el cliente de acceso y envía un mensaje de solicitud de cuentas para el proxy NPS RADIUS. El proxy NPS RADIUS registra los datos de cuentas y reenvía el mensaje al servidor RADIUS.

10. El servidor RADIUS envía una respuesta de la cuenta para el proxy NPS RADIUS, donde se reenvía al servidor de acceso.

Para obtener más información acerca de NPS, consulta [servidor de directivas de redes (NPS)](nps-top.md).
