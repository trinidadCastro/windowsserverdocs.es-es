---
title: Procesamiento de solicitudes de conexión
description: En este tema se proporciona información general de solicitud de conexión de servidor de directivas de red de procesamiento en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 849d661a-42c1-4f93-b669-6009d52aad39
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6ab08dced15cfadd5a0fc773efc2ddaa2da24c08
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829116"
---
# <a name="connection-request-processing"></a>Procesamiento de solicitudes de conexión

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este tema para obtener información sobre el procesamiento en el servidor de directivas de redes en Windows Server 2016 de la solicitud de conexión.

>[!NOTE]
>Además de este tema, la solicitud de conexión siguiente procesamiento documentación está disponible.
> - [Directivas de solicitud de conexión](nps-crp-crpolicies.md)
> - [Nombres de dominio Kerberos](nps-crp-realm-names.md)
> - [Grupos de servidores RADIUS remotos](nps-crp-rrsg.md)

Puede usar el procesamiento de solicitudes de conexión para especificar dónde se realiza la autenticación de solicitudes de conexión - en el equipo local o en un servidor RADIUS remoto que sea miembro de un grupo de servidores RADIUS remotos. 

Si desea que el servidor local con el servidor de directivas de redes (NPS) para realizar la autenticación para las solicitudes de conexión, puede usar la directiva de solicitud de conexión predeterminado sin ninguna configuración adicional. Según la directiva predeterminada, NPS autentica a los usuarios y equipos que tienen una cuenta en el dominio local y en dominios de confianza.

Si desea reenviar las solicitudes de conexión a un NPS remoto u otro servidor RADIUS, cree un grupo de servidores RADIUS remotos y, a continuación, configure una directiva de solicitud de conexión que reenvía las solicitudes a ese grupo de servidores RADIUS remotos. Con esta configuración, NPS puede reenviar las solicitudes de autenticación a cualquier servidor RADIUS y se pueden autenticar usuarios con cuentas en dominios no confiables.

La ilustración siguiente muestra la ruta de acceso de un mensaje de solicitud de acceso desde un servidor de acceso de red a un proxy RADIUS y, a continuación, a un servidor RADIUS en un grupo de servidores RADIUS remotos. En el proxy RADIUS, el servidor de acceso de red se configura como un cliente RADIUS; y en cada servidor RADIUS, el proxy RADIUS está configurado como cliente RADIUS.


![Procesamiento de solicitudes de conexión de NPS](../../media/Nps-Connection-Request-Processing/Nps-Connection-Request-Processing.jpg)


>[!NOTE]
>Los servidores de acceso de red que use con NPS pueden ser dispositivos de puerta de enlace que son compatibles con el protocolo RADIUS, como 802.1X de puntos de acceso inalámbrico y conmutadores de autenticación, los servidores de acceso remoto que están configurados como VPN o telefónico servidores, o otros dispositivos compatibles con RADIUS.

Si desea que NPS procese algunas solicitudes de autenticación localmente y reenvíe otras solicitudes a un grupo de servidores RADIUS remotos, configure más de una directiva de solicitud de conexión.

Para configurar una directiva de solicitud de conexión que especifica qué grupo de servidores NPS o RADIUS procesa las solicitudes de autenticación, consulte las directivas de solicitud de conexión.

Para especificar NPS u otros servidores RADIUS para autenticación, las solicitudes se reenvían, vea los grupos de servidores RADIUS remotos.

## <a name="nps-as-a-radius-server-connection-request-processing"></a>NPS como un procesamiento de solicitudes de conexión de servidor RADIUS

Cuando se usa NPS como servidor RADIUS, los mensajes RADIUS administran la autenticación, autorización y contabilidad para conexiones de acceso de red de la manera siguiente:

1. Servidores de acceso, como servidores de acceso telefónico, servidores VPN y puntos de acceso inalámbrico, reciben las solicitudes de conexión de clientes de acceso. 

2. El servidor de acceso, configurado para usar RADIUS como el protocolo de autenticación, autorización y contabilidad, crea un mensaje de solicitud de acceso y lo envía a NPS. 

3. NPS evalúa el mensaje de solicitud de acceso. 

4. Si es necesario, NPS envía un mensaje de desafío de acceso al servidor de acceso. El servidor de acceso procesa el desafío y envía una solicitud de acceso actualizada al NPS. 

5. Se comprueban las credenciales de usuario y las propiedades de acceso telefónico de la cuenta de usuario se obtienen mediante el uso de una conexión segura con un controlador de dominio. 

6. El intento de conexión se autoriza con las propiedades de acceso telefónico de las directivas de red y cuenta de usuario. 

7. Si el intento de conexión se autentica y se autoriza, NPS envía un mensaje de aceptación de acceso al servidor de acceso. Si el intento de conexión no autenticado o no autorizado, NPS envía un mensaje de rechazo de acceso al servidor de acceso. 

8. El servidor de acceso completa el proceso de conexión con el cliente de acceso y envía un mensaje de solicitud de cuentas al NPS, donde se registra el mensaje. 

9. NPS envía un mensaje de respuesta al servidor de acceso. 

>[!NOTE]
>El servidor de acceso también envía mensajes de solicitud de cuentas durante el tiempo en que se establece la conexión, cuando se cierra la conexión de cliente de acceso, y cuando se inicia y detiene el servidor de acceso.

## <a name="nps-as-a-radius-proxy-connection-request-processing"></a>NPS como un procesamiento de solicitudes de conexión de proxy RADIUS

Cuando se usa NPS como proxy RADIUS entre un cliente RADIUS y un servidor RADIUS, los mensajes RADIUS para red tener acceso a conexión intentos se reenvían del siguiente modo:

1. Servidores de acceso, como servidores de acceso telefónico, servidores de red privada virtual (VPN) y puntos de acceso inalámbrico, reciben las solicitudes de conexión de clientes de acceso.

2. El servidor de acceso, configurado para usar RADIUS como el protocolo de autenticación, autorización y contabilidad, crea un mensaje de solicitud de acceso y lo envía al NPS que se usa como proxy RADIUS NPS.

3. El proxy RADIUS NPS recibe el mensaje de solicitud de acceso y, en función de las directivas de solicitud de conexión configuradas localmente, determina adónde reenviar el mensaje de solicitud de acceso.

4. El proxy RADIUS NPS reenvía el mensaje de solicitud de acceso al servidor RADIUS correspondiente.

5. El servidor RADIUS evalúa el mensaje de solicitud de acceso.

6. Si es necesario, el servidor RADIUS envía un mensaje de desafío de acceso al proxy RADIUS NPS, desde donde se reenvía al servidor de acceso. El servidor de acceso procesa el desafío con el cliente de acceso y envía una solicitud de acceso actualizada al proxy RADIUS NPS, desde donde se reenvía al servidor RADIUS.

7. El servidor RADIUS autentica y autoriza el intento de conexión.

8. Si el intento de conexión se autentica y se autoriza, el servidor RADIUS envía un mensaje de aceptación de acceso al proxy RADIUS NPS, desde donde se reenvía al servidor de acceso. Si el intento de conexión no autenticado o no autorizado, el servidor RADIUS envía un mensaje de rechazo de acceso al proxy RADIUS NPS, desde donde se reenvía al servidor de acceso.

9. El servidor de acceso completa el proceso de conexión con el cliente de acceso y envía un mensaje de solicitud de cuentas al proxy RADIUS NPS. El proxy RADIUS NPS registra los datos de cuentas y reenvía el mensaje al servidor RADIUS.

10. El servidor RADIUS envía un mensaje de respuesta al proxy RADIUS NPS, desde donde se reenvía al servidor de acceso.

Para obtener más información acerca de NPS, consulte [servidor de directivas de redes (NPS)](nps-top.md).
