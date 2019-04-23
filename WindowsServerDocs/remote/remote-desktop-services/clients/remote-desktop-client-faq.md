---
title: Clientes de escritorio remoto preguntas más frecuentes
description: Preguntas más frecuentes acerca de los clientes de escritorio remoto
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 785a18cf-a5d0-4bc2-95e4-9ef53ee8f65a
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 07/16/2018
ms.localizationpriority: medium
ms.openlocfilehash: ec1b0a17c578f2d8ac55d1704af6b267b6bb8e5c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865936"
---
# <a name="frequently-asked-questions-about-the-remote-desktop-clients"></a>Preguntas más frecuentes acerca de los clientes de escritorio remoto

>Se aplica a: Windows 10, Windows 8.1, Windows Server 2012 R2, Windows Server 2016

Ahora que ha configurado el cliente de escritorio remoto en el dispositivo (Android, Mac, iOS o Windows), es posible que tenga dudas. Aquí encontrará respuestas a las preguntas más frecuentes acerca de los clientes de escritorio remoto. 

- [Configurar](#Setting-up)
- [Conexiones de puerta de enlace y redes](#connection-gateway-and-networks)
- [cliente Web](#web-client)
- [Monitores, audio y mouse](#monitors-audio-and-mouse)
- [Hardware de Mac](#mac-client---hardware-questions)
- [Mensajes de error específicos](#specific-errors)

La mayoría de estas preguntas se aplican a todos los clientes, pero hay algunos elementos específicos de cliente.

Si tiene preguntas adicionales que le gustaría respondamos a, dejarlos como comentarios en este artículo.

## <a name="setting-up"></a>Configurar

### <a name="which-pcs-can-i-connect-to"></a>¿Qué equipos me conecto a?

Consulte la [admite configuración](remote-desktop-supported-config.md) artículo para obtener información acerca de qué equipos se puede conectar a.

### <a name="how-do-i-set-up-a-pc-for-remote-desktop"></a>¿Cómo configuro un PC de escritorio remoto?

Tengo mi dispositivo configurado, pero no creo listo del equipo. ¿Ayuda?

¿En primer lugar, ha visto al Asistente para configuración de escritorio remoto? Le guía a través de preparación de su PC para acceso remoto. Descarga y ejecución que la herramienta en su PC para que todo el conjunto. 

En caso contrario, si prefiere hacer cosas manualmente, siga leyendo.

Para Windows 10, haga lo siguiente:

1. En el dispositivo que desea conectarse, abra **configuración**.
2. Seleccione **sistema** y, a continuación, **escritorio remoto**.
3. Use el control deslizante para habilitar Escritorio remoto.
4. En general, es mejor mantener el equipo activo y reconocible para facilitar las conexiones. Haga clic en **Mostrar configuración** para ir a la configuración de energía de su PC, donde puede cambiar esta configuración.
   > [!NOTE]
   > No puede conectarse a un equipo que está en suspensión o hibernación, así que asegúrese de que la configuración de suspensión e hibernación en el equipo remoto se establecen en **nunca**. (Modo de hibernación no está disponible en todos los equipos).


Tome nota del nombre de este equipo bajo **cómo conectarse a este equipo**. Las necesitará para configurar a los clientes.

Puede conceder permiso a usuarios específicos tener acceso a este equipo: para ello, haga clic en **seleccione los usuarios que pueden obtener acceso remoto a este equipo**.
Los miembros del grupo de administradores automáticamente tienen acceso.

Para Windows 8.1, siga las instrucciones para permitir conexiones remotas en [conectarse a otro de escritorio con conexiones a Escritorio remoto](https://support.microsoft.com/en-us/help/17463/windows-7-connect-to-another-computer-remote-desktop-connection#1TC=windows-8).



## <a name="connection-gateway-and-networks"></a>Conexión de puerta de enlace y redes

### <a name="why-cant-i-connect-using-remote-desktop"></a>¿Por qué no puedo conectar utilizando Escritorio remoto?

Estas son algunas posibles soluciones a problemas comunes que puede surgir al intentar conectarse a un equipo remoto. Si estas soluciones no funcionan, puede buscar más ayuda en el [sitio Web de Microsoft Community](https://go.microsoft.com/fwlink/p/?LinkId=242079).

- **No se encuentra el equipo remoto.** Asegúrese de que tiene el nombre de equipo correcto y, a continuación, compruebe si escribió correctamente ese nombre. Si sigue sin poder conectarse, pruebe a usar la dirección IP del equipo remoto en lugar del nombre de equipo.
- **Hay un problema con la red.** Asegúrese de que tiene conexión a internet. 
- **El puerto de escritorio remoto podría estar bloqueado por un firewall.** Si usa Firewall de Windows, siga estos pasos:

   1. Abra Firewall de Windows. 
   2. Haga clic en **permitir una aplicación o característica a través de Firewall de Windows**. 
   3. Haga clic en **cambiar la configuración de**. Puede que se le pida una contraseña de administrador o para confirmar su elección.
   4. En **aplicaciones y características permitidas**, seleccione **escritorio remoto**y, a continuación, pulse o haga clic en **Aceptar**.

   Si usa otro firewall, asegúrese de que el puerto de escritorio remoto (normalmente 3389) esté abierto.
- **Las conexiones remotas no se pueden configurar en el equipo remoto.** Para solucionar este problema, desplácese hacia atrás hasta [¿Cómo configuro un PC de escritorio remoto?](#how-do-i-set-up-a-pc-for-remote-desktop) pregunta en este tema.
- **El equipo remoto podría permitir sólo conectar los equipos que tienen la configuración de autenticación a nivel de red.** 
- **El equipo remoto podría estar desactivado.** No puede conectarse a un equipo que está apagado, suspensión o hibernación, así que asegúrese de que la configuración de suspensión e hibernación en el equipo remoto se establecen en **nunca** (modo de hibernación no está disponible en todos los equipos.).

### <a name="why-cant-i-find-or-connect-to-my-pc"></a>¿Por qué no puedo encontrar o conectarse a su PC?
Compruebe lo siguiente:
- ¿Es el PC y Active?
- ¿Escriba la dirección IP o el nombre correcto?

   > [!IMPORTANT]
   > Con el nombre de equipo requiere que la red para resolver el nombre correctamente a través de DNS. En muchas redes domésticas, tendrá que usar la dirección IP en lugar del nombre de host para conectarse.
- ¿Es el PC en una red diferente? ¿Si configuró el equipo para permitir que fuera de las conexiones a través?  Desproteger [permitir el acceso a su equipo desde fuera de la red](remote-desktop-allow-outside-access.md) para obtener ayuda.
- ¿Está conectado a una versión compatible de Windows? 

   > [!NOTE]
   > Windows XP Home, Windows Media Center Edition, Windows Vista Home y Windows 7 Home o Starter no se admiten sin 3rd software de terceros.

### <a name="why-cant-i-sign-in-to-a-remote-pc"></a>¿Por qué no puedo iniciar sesión en un equipo remoto?
Si puede ver la pantalla de inicio de sesión del equipo remoto, pero no puede iniciar sesión, es posible que no se agregaron al grupo de usuarios de escritorio remoto o a cualquier grupo con derechos de administrador en el equipo remoto. Pídale al administrador del sistema para hacer esto por usted.

### <a name="which-connection-methods-are-supported-for-company-networks"></a>¿Los métodos de conexión se admiten para redes de empresa?
Si desea obtener acceso al escritorio de office desde fuera de la red de empresa, su empresa debe proporcionar un medio de acceso remoto. Actualmente, el cliente de escritorio remoto admite lo siguiente:

- Puerta de enlace de Terminal Server o la puerta de enlace de escritorio remoto
- Acceso Web a Escritorio remoto
- VPN (a través de opciones de VPN de iOS integradas)

### <a name="vpn-doesnt-work"></a>VPN no funciona

Problemas de VPN pueden tener varias causas. El primer paso es comprobar que la conexión VPN funciona en la misma red que un equipo PC o Mac. Si no se puede probar con un PC o Mac, puede intentar obtener acceso a una página de web de intranet de la compañía con el explorador del dispositivo.

Para comprobar otras cosas:
- **La red 3G se bloquea o daña la VPN.** Hay varios proveedores de 3G en el mundo que parecen dañados tráfico de 3G o bloque. Compruebe la conectividad VPN funciona correctamente para más de un minuto.
- **L2TP o PPTP VPN.** Si usa L2TP o PPTP en la VPN, establezca enviar todo el tráfico en ON en la configuración de VPN.
- **VPN está mal configurada.** Un servidor VPN mal configurado puede ser la razón por la las conexiones VPN nunca funcionaban o dejó de funcionar más tarde. Asegúrese de probar con iOS explorador web del dispositivo o un PC o Mac en la misma red, si esto ocurre.

### <a name="how-can-i-test-if-vpn-is-working-properly"></a>¿Cómo se puede probar si VPN está funcionando correctamente?
Compruebe que la VPN está habilitada en el dispositivo. Puede probar la conexión VPN a una página Web en su red interna o mediante un servicio web que solo está disponible a través de la VPN.

### <a name="how-do-i-configure-l2tp-or-pptp-vpn-connections"></a>¿Cómo se puede configurar las conexiones L2TP o PPTP VPN?
Si usa L2TP o PPTP en la VPN, asegúrese de establecer **enviar todo el tráfico** a **ON** en la configuración de VPN.

## <a name="web-client"></a>cliente Web

### <a name="which-browsers-can-i-use"></a>¿Qué exploradores puedo usar?

El cliente web es compatible con Microsoft Edge, Internet Explorer 11, Mozilla Firefox (v55.0 y versiones posteriores), Safari y Google Chrome.

### <a name="what-pcs-can-i-use-to-access-the-web-client"></a>¿Qué equipos puedo usar para tener acceso a los clientes web?

El cliente web es compatible con Windows, macOS, Linux y ChromeOS. No se admiten los dispositivos móviles en este momento.

### <a name="can-i-use-the-web-client-in-a-remote-desktop-deployment-without-a-gateway"></a>¿Puedo usar al cliente web en una implementación de escritorio remoto sin una puerta de enlace?

No. El cliente requiere una puerta de enlace de escritorio remoto para conectarse. ¿No sabe lo que significa? Consulte al administrador sobre él.

### <a name="does-the-remote-desktop-web-client-replace-the-remote-desktop-web-access-page"></a>¿El cliente web de escritorio remoto reemplaza la página acceso Web a Escritorio remoto?

No. El cliente web de escritorio remoto se hospeda en una dirección URL diferente que la página acceso Web a Escritorio remoto. Puede usar el cliente web o en la página Web Access para ver los recursos remotos en un explorador.

### <a name="can-i-embed-the-web-client-in-another-web-page"></a>¿Se puede incrustar al cliente web en otra página web?

Esta característica no se admite en este momento.

## <a name="monitors-audio-and-mouse"></a>Monitores, audio y mouse

### <a name="how-do-i-use-all-of-my-monitors"></a>Cómo usar todos mis monitores
Para usar dos o más pantallas, realice lo siguiente:

1. Haga clic en el escritorio remoto que desea habilitar varias pantallas para y, a continuación, haga clic en **editar**.
2. Habilitar **usar todos los monitores** y **pantalla completa**.

### <a name="is-bi-directional-sound-supported"></a>¿Es compatible con sonido bidireccional?
Sonido ascendente (del cliente al servidor, para micrófonos) no es compatible con el cliente de escritorio remoto.

### <a name="what-can-i-do-if-the-sound-wont-play"></a>¿Qué puedo hacer si no se reproduce el sonido?
Cierre la sesión de la sesión (no solo desconectar, cerrar sesión completamente) y, a continuación, vuelva a iniciarla.

## <a name="mac-client---hardware-questions"></a>Cliente de Mac - preguntas de hardware
### <a name="is-retina-resolution-supported"></a>¿Se admite la resolución retina?
Sí, el cliente de escritorio remoto admite resolución retina.

### <a name="how-do-i-enable-secondary-right-click"></a>¿Cómo habilito contextual secundaria?
Para asegurarse de usar el botón secundario dentro de una sesión abierta tiene tres opciones:

- Mouse de estándar PC dos botones USB
- Mouse mágica de Apple: Para habilitar el botón derecho, haga clic en **preferencias del sistema** en el dock, haga clic en **Mouse**y, a continuación, habilite **clic secundaria**.
- Apple mágica Trackpad o MacBook Trackpad: Para habilitar el botón derecho, haga clic en **preferencias del sistema** en el dock, haga clic en **Mouse**y, a continuación, habilite **clic secundaria**.

### <a name="is-airprint-supported"></a>¿Es compatible con AirPrint?
No, el cliente de escritorio remoto no admite AirPrint. (Esto es cierto para los clientes Mac e iOS).

### <a name="why-do-incorrect-characters-appear-in-the-session"></a>¿Por qué aparecen caracteres incorrectos en la sesión?
Si usa un teclado internacional, puede aparecer un problema donde los caracteres que aparecen en la sesión coincide con los caracteres que escribió en el teclado de Mac.

Esto puede ocurrir en los siguientes escenarios:

- Usa un teclado que no reconoce la sesión remota. Al escritorio remoto no reconoce el teclado, el valor predeterminado es el idioma utilizado por última vez en el equipo remoto.
- Se conecta a una sesión desconectada previamente en un equipo remoto y que el equipo remoto usa un lenguaje de teclado diferente que el lenguaje que está actualmente intentando usar.

Puede solucionar este problema estableciendo manualmente el idioma del teclado para la sesión remota. Consulte los pasos descritos en la sección siguiente.

### <a name="how-do-language-settings-affect-keyboards-in-a-remote-session"></a>¿Cómo afecta la configuración de idioma teclados en una sesión remota?
Hay muchos tipos de diseños de teclado de Mac. Algunas de ellas son diseños específicos de Mac o diseños personalizados para que una coincidencia exacta no estén disponible en la versión de Windows están en comunicación remota de. La sesión remota asigna el teclado a la mejor coincidencia de idioma de teclado disponible en el equipo remoto. 

Si la distribución del teclado de Mac está establecida en la versión de PC de su teclado y el teclado del idioma (por ejemplo, francés – PC) todas las claves se deben asignar correctamente debería funcionar.

Si la distribución del teclado de Mac está establecida en la versión de Mac de un teclado (por ejemplo, francés) la sesión remota asignará a la versión de PC de francés. Algunos de los métodos abreviados de teclado de Mac que estén acostumbrados a utilizar en OSX no funcionará en la sesión remota de Windows.

Si la distribución del teclado está establecida en una variación de un idioma (por ejemplo, francés canadiense) y la sesión remota no puede asignar a ese variación exacta, la sesión remota asignará al idioma más cercano (por ejemplo, francés). Algunos de los métodos abreviados de teclado de Mac que estén acostumbrados a utilizar en OSX no funcionará en la sesión remota de Windows.

Si la distribución del teclado está establecida en un diseño de que la sesión remota no puede hacer coincidir en absoluto, predeterminado de la sesión remota para darle el idioma que se utilizó por última vez con ese equipo. En este caso, o en casos donde es necesario cambiar el idioma de la sesión remota para que coincida con el teclado de Mac, puede establecer manualmente el idioma del teclado en la sesión remota en el idioma que es la coincidencia más cercana a la que desea usar como sigue.

Utilice las instrucciones siguientes para cambiar la distribución del teclado dentro de la sesión de escritorio remota:

**En Windows 10 o Windows 8:**

1. Desde dentro de la sesión remota, abra región e idioma. Haga clic en **Inicio > Configuración > hora e idioma**. Abra **región e idioma**.
2. Agregue el idioma que desea usar. A continuación, cierre la ventana de región e idioma.
3. Ahora, en la sesión remota, verá la posibilidad de cambiar entre lenguajes. (En el lado derecho de la sesión remota, cerca del reloj.) Haga clic en el idioma que desea cambiar a (como **Eng**).

Es posible que deba cerrar y reiniciar la aplicación que usa actualmente para que los cambios de teclado surta efecto.


## <a name="specific-errors"></a>Errores concretos

### <a name="why-do-i-get-an-insufficient-privileges-error"></a>¿Por qué obtengo un error "No tiene privilegios suficientes"?
No se permiten para tener acceso a la sesión que desea conectarse. La causa más probable es que está intentando conectarse a una sesión de administrador. Solo los administradores pueden conectarse a la consola. Compruebe que el conmutador de consola está desactivada en la configuración avanzada de escritorio remoto. Si esto no es el origen del problema, póngase en contacto con el administrador del sistema para obtener más ayuda.

### <a name="why-does-the-client-say-that-there-is-no-cal"></a>¿Por qué el cliente indica que no hay ninguna CAL?
Cuando un cliente de escritorio remoto se conecta a un servidor de escritorio remoto, el servidor emite un escritorio servicios de cliente de licencia de acceso remoto (CAL de RDS) almacenados por el cliente. Cada vez que se conecta el cliente nuevo va a usar sus CAL de RDS y el servidor no emitirá otra licencia. El servidor emitirá otra licencia si la CAL de RDS en el dispositivo que falta o está dañado. Cuando se alcanza el número máximo de dispositivos con licencia de servidor no emitirá nueva CAL de RDS. Para obtener ayuda, póngase en contacto con el Administrador de red.

### <a name="why-did-i-get-an-access-denied-error"></a>¿Por qué obtengo un error "Acceso denegado"?
El error "Acceso denegado" es una generada por la puerta de enlace de escritorio remoto y el resultado de credenciales incorrectas durante el intento de conexión. Compruebe el nombre de usuario y la contraseña. Si la conexión trabajado antes y recientemente se produjo el error, posiblemente cambiado su contraseña de cuenta de usuario de Windows y aún no ha actualizado aún en la configuración de escritorio remoto.

### <a name="what-does-rpc-error-23014-or-error-0x59e6-mean"></a>¿Qué significa "Error 0x59e6" o "Error RPC 23014"?
En caso de un **error RPC 23014** o **Error 0x59E6 inténtelo de nuevo después de esperar unos minutos**, el servidor de puerta de enlace de escritorio remoto ha alcanzado el número máximo de conexiones activas. Dependiendo de la versión de Windows que se ejecutan en la puerta de enlace de escritorio remoto el número máximo de conexiones es diferente: La implementación de Windows Server 2008 R2 Standard limita el número de conexiones a 250. La implementación de Windows Server 2008 R2 Foundation limita el número de conexiones en 50. Todas las otras implementaciones de Windows permiten a un número ilimitado de conexiones.

### <a name="what-does-the-failed-to-parse-ntlm-challenge-error-mean"></a>¿Qué significa el error "No se pudo analizar el desafío NTLM"?
Este error se debe a una configuración incorrecta en el equipo remoto. Asegúrese de que la configuración de nivel de seguridad RDP en el equipo remoto se establece en "Cliente Compatible." (Consulte con el administrador del sistema si necesita ayuda para hacerlo).

### <a name="what-does-tsrap-you-are-not-allowed-to-connect-to-the-given-host-mean"></a>¿Qué "TS_RAP no puedan conectarse al host dado" significa?
Este error se produce cuando una directiva de autorización de recursos en el servidor de puerta de enlace deja el nombre de usuario de la conexión al equipo remoto. Esto puede ocurrir en los casos siguientes:

- El nombre del equipo remoto es el mismo que el nombre de la puerta de enlace. A continuación, al intentar conectar con el equipo remoto, la conexión va a la puerta de enlace en su lugar, que probablemente no tiene permiso de acceso. Si necesita conectarse a la puerta de enlace, no use el nombre de la puerta de enlace externa como nombre de equipo. En su lugar, use "localhost" o la dirección IP (127.0.0.1) o el nombre del servidor interno.
- Su cuenta de usuario no es un miembro del grupo de usuarios para el acceso remoto.