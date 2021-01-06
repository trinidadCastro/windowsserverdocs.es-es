---
title: Procesamiento de solicitudes de conexión
description: En este tema se proporciona información general sobre el procesamiento de solicitudes de conexión del servidor de directivas de redes en Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: 849d661a-42c1-4f93-b669-6009d52aad39
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 228eebc3ad3d81868e13137ab9803d04b15df238
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949901"
---
# <a name="connection-request-processing"></a>Procesamiento de solicitudes de conexión

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información sobre el procesamiento de solicitudes de conexión en el servidor de directivas de redes en Windows Server 2016.

>[!NOTE]
>Además de este tema, está disponible la siguiente documentación sobre el procesamiento de solicitudes de conexión.
> - [Directivas de solicitud de conexión](nps-crp-crpolicies.md)
> - [Nombres de dominio Kerberos](nps-crp-realm-names.md)
> - [Grupos de servidores RADIUS remotos](nps-crp-rrsg.md)

Puede usar el procesamiento de solicitudes de conexión para especificar dónde se realiza la autenticación de las solicitudes de conexión: en el equipo local o en un servidor RADIUS remoto que sea miembro de un grupo de servidores RADIUS remotos.

Si desea que el servidor local que ejecuta el servidor de directivas de redes (NPS) realice la autenticación para las solicitudes de conexión, puede usar la Directiva de solicitud de conexión predeterminada sin configuración adicional. Basándose en la directiva predeterminada, NPS autentica a los usuarios y equipos que tienen una cuenta en el dominio local y en dominios de confianza.

Si desea reenviar las solicitudes de conexión a un NPS remoto u otro servidor RADIUS, cree un grupo de servidores RADIUS remoto y, a continuación, configure una directiva de solicitud de conexión que reenvíe las solicitudes a ese grupo de servidores RADIUS remotos. Con esta configuración, NPS puede reenviar solicitudes de autenticación a cualquier servidor RADIUS, y los usuarios con cuentas en dominios que no son de confianza pueden ser autenticados.

La ilustración siguiente muestra la ruta de un mensaje de solicitud de acceso desde un servidor de acceso a la red a un proxy RADIUS y, a continuación, a un servidor RADIUS de un grupo del servidor RADIUS remoto. En el proxy RADIUS, el servidor de acceso a la red se configura como un cliente RADIUS; y en cada servidor RADIUS, el proxy RADIUS se configura como un cliente RADIUS.


![Procesamiento de solicitudes de conexión NPS](../../media/Nps-Connection-Request-Processing/Nps-Connection-Request-Processing.jpg)


>[!NOTE]
>Los servidores de acceso a la red que se usan con NPS pueden ser dispositivos de puerta de enlace compatibles con el protocolo RADIUS, como puntos de acceso inalámbricos de 802.1 X y conmutadores de autenticación, servidores que ejecutan acceso remoto y que están configurados como servidores VPN o de acceso telefónico, u otros dispositivos compatibles con RADIUS.

Si desea que NPS procese algunas solicitudes de autenticación localmente y reenvíe otras solicitudes a un grupo del servidor RADIUS remoto, configure más de una directiva de solicitud de conexión.

Para configurar una directiva de solicitud de conexión que especifique qué grupo de servidores NPS o RADIUS procesa las solicitudes de autenticación, consulte directivas de solicitud de conexión.

Para especificar NPS u otros servidores RADIUS a los que se reenvían las solicitudes de autenticación, consulte grupos de servidores RADIUS remotos.

## <a name="nps-as-a-radius-server-connection-request-processing"></a>Procesamiento de solicitudes de conexión de NPS como servidor RADIUS

Cuando se usa NPS como un servidor RADIUS, los mensajes RADIUS proporcionan autenticación, autorización y cuentas para las conexiones de acceso a la red de la siguiente manera:

1. Los servidores de acceso, como los servidores de acceso telefónico a la red, los servidores VPN y los puntos de acceso inalámbrico reciben solicitudes de conexión de clientes de acceso.

2. El servidor de acceso, configurado para usar RADIUS como protocolo de autenticación, autorización y cuentas, crea un mensaje de Access-Request y lo envía al NPS.

3. El NPS evalúa el mensaje de Access-Request.

4. Si es necesario, el NPS envía un mensaje de Access-Challenge al servidor de acceso. El servidor de acceso procesa el desafío y envía una Access-Request actualizada al NPS.

5. Las credenciales de usuario se comprueban y las propiedades de acceso telefónico de la cuenta de usuario se obtienen por medio de una conexión segura con un controlador de dominio.

6. El intento de conexión se autoriza con las propiedades de acceso telefónico de la cuenta de usuario y las directivas de red.

7. Si el intento de conexión se autentica y autoriza, el NPS envía un mensaje de Access-Accept al servidor de acceso. Si el intento de conexión no se ha autenticado o no está autorizado, el NPS envía un mensaje de Access-Reject al servidor de acceso.

8. El servidor de acceso completa el proceso de conexión con el cliente de acceso y envía un mensaje de Accounting-Request al NPS, donde se registra el mensaje.

9. NPS envía una Accounting-Response al servidor de acceso.

>[!NOTE]
>Asimismo, el servidor de acceso envía mensajes de solicitud de registro de actividad cuando la conexión se establece, cuando la conexión del cliente de acceso se cierra y cuando el servidor de acceso se inicia y detiene.

## <a name="nps-as-a-radius-proxy-connection-request-processing"></a>NPS como procesamiento de solicitudes de conexión de proxy RADIUS

Cuando NPS se usa como proxy RADIUS entre un cliente RADIUS y un servidor RADIUS, los mensajes RADIUS para los intentos de conexión de acceso a la red se reenvían del siguiente modo:

1. Los servidores de acceso, tales como los servidores de acceso telefónico a la red, los servidores de red privada virtual (VPN) y los puntos de acceso inalámbrico, reciben las solicitudes de conexión de los clientes de acceso.

2. El servidor de acceso, configurado para usar RADIUS como protocolo de autenticación, autorización y cuentas, crea un mensaje de Access-Request y lo envía al NPS que se usa como el proxy RADIUS NPS.

3. El proxy RADIUS NPS recibe el mensaje de solicitud de acceso y, basándose en las directivas de solicitud de conexión configuradas localmente, determina adónde reenviar el mensaje de solicitud de acceso.

4. El proxy RADIUS NPS reenvía el mensaje de solicitud de acceso al servidor RADIUS correspondiente.

5. El servidor RADIUS evalúa el mensaje de solicitud de acceso.

6. Si es necesario, el servidor RADIUS envía un mensaje de desafío de acceso al proxy RADIUS NPS, desde donde se reenvía al servidor de acceso. El servidor de acceso procesa el desafío con el cliente de acceso y envía una solicitud de acceso actualizada al proxy RADIUS NPS, desde donde se reenvía al servidor RADIUS.

7. El servidor RADIUS autentica y autoriza el intento de conexión.

8. Si se autentica y autoriza el intento de conexión, el servidor RADIUS envía un mensaje de aceptación de acceso al proxy RADIUS NPS, desde donde se reenvía al servidor de acceso. Si no se autentica o autoriza el intento de conexión, el servidor RADIUS envía un mensaje de rechazo de acceso al proxy RADIUS NPS, desde donde se reenvía al servidor de acceso.

9. El servidor de acceso completa el proceso de conexión con el cliente de acceso y envía un mensaje de solicitud de registro de actividad al proxy RADIUS NPS. El proxy RADIUS NPS registra los datos de cuentas y reenvía el mensaje al servidor RADIUS.

10. El servidor RADIUS envía un mensaje de respuesta de cuenta al proxy RADIUS NPS, desde donde se reenvía al servidor de acceso.

Para obtener más información acerca de NPS, consulte [servidor de directivas de redes (NPS)](nps-top.md).
