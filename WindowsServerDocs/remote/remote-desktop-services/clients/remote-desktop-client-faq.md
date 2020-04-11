---
title: Preguntas más frecuentes sobre los clientes de Escritorio remoto
description: Preguntas más frecuentes sobre los clientes de Escritorio remoto
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: 785a18cf-a5d0-4bc2-95e4-9ef53ee8f65a
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 07/16/2018
ms.localizationpriority: medium
ms.openlocfilehash: a91980477b34bb529e3e6f3c6ff66da9ea7f3c84
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856058"
---
# <a name="frequently-asked-questions-about-the-remote-desktop-clients"></a>Preguntas más frecuentes sobre los clientes de Escritorio remoto

>Se aplica a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Ahora que configuraste el cliente de Escritorio remoto en tu dispositivo (Android, Mac, iOS o Windows), puede que tengas algunas preguntas. Aquí encontrarás las respuestas a las preguntas más frecuentes sobre los clientes de Escritorio remoto. 

- [Configuración](#setting-up)
- [Conexiones, puertas de enlace y redes](#connection-gateway-and-networks)
- [Cliente web](#web-client)
- [Monitores, audio y mouse](#monitors-audio-and-mouse)
- [Hardware de Mac](#mac-client---hardware-questions)
- [Mensajes de error específicos](#specific-errors)

La mayoría de estas preguntas se aplican a todos los clientes, pero hay algunos elementos específicos para algún cliente.

Si tienes más preguntas que quieres que respondamos, déjalas como comentarios sobre este artículo.

## <a name="setting-up"></a>Configuración

### <a name="which-pcs-can-i-connect-to"></a>¿A qué equipos me puedo conectar?

Revisa el artículo sobre la [configuración admitida](remote-desktop-supported-config.md) para obtener información sobre los equipos a los que te puedes conectar.

### <a name="how-do-i-set-up-a-pc-for-remote-desktop"></a>¿Cómo configuro un equipo para Escritorio remoto?

Mi dispositivo está configurado, pero no creo que el equipo esté listo. ¿Necesitas ayuda?

En primer lugar, ¿has visto el Asistente para la instalación de Escritorio remoto? Te guía por el proceso de preparar tu equipo para el acceso remoto. Descarga y ejecuta esa herramienta en tu equipo para instalar y configurar todo. 

En caso contrario, si prefieres seguir los procesos manualmente, sigue leyendo.

En Windows 10, haz lo siguiente:

1. En el dispositivo al que quieres conectarte, abre **Configuración**.
2. Selecciona **Sistema** y, después, **Escritorio remoto**.
3. Utiliza el control deslizante para habilitar Escritorio remoto.
4. En general, lo mejor es el equipo activo y reconocible para facilitar las conexiones. Haz clic en **Mostrar configuración** para ir a la configuración de energía del equipo, donde puedes cambiar esta configuración.
   > [!NOTE]
   > No puedes conectarte a un equipo que está en suspensión o en hibernación, así que asegúrate de que la configuración de suspensión o hibernación del equipo remoto esté establecida en **Nunca**. (La hibernación no está disponible en todos los equipos).


Toma nota del nombre de este equipo en **How to connect to this PC** (Cómo conectarse a este equipo). Lo necesitarás para configurar a los clientes.

Puedes conceder permiso para que usuarios específicos accedan a este equipo; para hacerlo, haz clic en **Select users that can remotely access this PC** (Seleccionar los usuarios que pueden obtener acceso remoto a este equipo).
Los miembros del grupo de administradores tienen acceso de forma automática.

En Windows 8.1, sigue las instrucciones para permitir conexiones remotas que aparecen en [Conectarse a otro equipo mediante Conexión a Escritorio remoto](https://support.microsoft.com/help/17463/windows-7-connect-to-another-computer-remote-desktop-connection#1TC=windows-8).



## <a name="connection-gateway-and-networks"></a>Conexiones, puertas de enlace y redes

### <a name="why-cant-i-connect-using-remote-desktop"></a>¿Por qué no me puedo conectar mediante Escritorio remoto?

Estas son algunas de las soluciones posibles a los problemas comunes que podrías encontrar al intentar conectarte a un equipo remoto. Si estas soluciones no funcionan, puedes encontrar más ayuda en el [sitio web de la Comunidad Windows](https://go.microsoft.com/fwlink/p/?LinkId=242079).

- **No se encuentra el equipo remoto.** Asegúrate de tener el nombre del equipo correcto y revisa si lo escribiste correctamente. Si sigues sin poder conectarte, trata de usar la dirección IP del equipo remoto en lugar el nombre del equipo.
- **Hay un problema con la red.** Asegúrate de tener conexión a Internet. 
- **El puerto de Escritorio remoto puede estar bloqueado por un firewall.** Sigue estos pasos si usas el Firewall de Windows:

  1. Abra Firewall de Windows. 
  2. Haz clic en **Permitir una aplicación o una característica a través del Firewall de Windows**. 
  3. Haz clic en **Cambiar configuración**. Es posible que se te pida una contraseña de administrador o que confirmes la selección.
  4. En **Allowed apps and features** (Aplicaciones y características permitidas), selecciona **Escritorio remoto** y pulsa o haz clic en **Aceptar**.

     Si usas otro firewall, asegúrate de que el puerto de Escritorio remoto (por lo general, 3389) está abierto.
- **Es posible que las conexiones remotas no estén configuradas en el equipo remoto.** Para solucionar este problema, desplázate hacia atrás hasta la pregunta [¿Cómo configuro un equipo para Escritorio remoto?](#how-do-i-set-up-a-pc-for-remote-desktop) en este tema.
- **Es posible que el equipo remoto solo permita que se conecten equipos que tienen configurada la Autenticación a nivel de red.** 
- **Puede que el equipo remoto esté apagado.** No puedes conectarte a un equipo apagado, en suspensión o en hibernación, así que asegúrate de que la configuración de la suspensión o la hibernación en el equipo remoto esté establecida en **Nunca** (la hibernación no está disponible en todos los equipos).

### <a name="why-cant-i-find-or-connect-to-my-pc"></a>¿Por qué no puedo encontrar mi equipo ni conectarme a él?

Compruebe lo siguiente:

- ¿El equipo está encendido y activo?
- ¿Escribiste la dirección IP o el nombre correcto?

   > [!IMPORTANT]
   > Para usar el nombre del equipo, tu red debe resolver correctamente el nombre a través de DNS. En muchas redes domésticas, tienes que usar la dirección IP en lugar del nombre de host para conectarte.
- ¿El equipo está en otra red? ¿Configuraste el equipo para permitir las conexiones externas?  Si necesitas ayuda, revisa [Permitir el acceso a tu PC desde fuera de la red](remote-desktop-allow-outside-access.md).
- ¿Estás conectado a una versión compatible de Windows? 

   > [!NOTE]
   > Windows XP Home, Windows Media Center Edition, Windows Vista Home ni Windows 7 Home o Starter son compatibles sin software de terceros.

### <a name="why-cant-i-sign-in-to-a-remote-pc"></a>¿Por qué no puedo iniciar sesión en un equipo remoto?

Si puedes ver la pantalla de inicio de sesión del equipo remoto pero no puedes iniciar sesión, es posible que no te hayan agregado al grupo de usuarios de Escritorio remoto ni a ningún grupo con derechos de administrador del equipo remoto. Pídele al administrador del sistema que te agregue.

### <a name="which-connection-methods-are-supported-for-company-networks"></a>¿Cuáles son los métodos de conexión admitidos para las redes de la empresa?

Si quieres acceder al escritorio de la oficina desde fuera de la red de la empresa, la empresa debe brindarte un medio de acceso remoto. El cliente de Escritorio remoto actualmente admite lo siguiente:

- Puerta de enlace de Terminal Server o Puerta de enlace de Escritorio remoto.
- Acceso web de Escritorio remoto.
- VPN (a través de opciones de VPN integradas de iOS).

### <a name="vpn-doesnt-work"></a>La VPN no funciona

Los problemas de VPN pueden tener varias causas. El primer paso es comprobar que la VPN funciona en la misma red que el equipo PC o Mac. Si no puedes probar con un equipo PC o Mac, puedes intentar acceder a una página web de la intranet de la empresa con el explorador del dispositivo.

Otros aspectos que debes comprobar:
- **La red 3G bloquea o daña la VPN.** Hay varios proveedores de 3G que parecen bloquear o dañar el tráfico de 3G. Comprueba que la conectividad de la VPN funciona correctamente durante más de un minuto.
- **VPN de L2TP o PPTP.** Si usas L2TP o PPTP en la VPN, establece Enviar todo el tráfico en ON (Activado) en la configuración de la VPN.
- **La VPN está mal configurada.** Un servidor de VPN mal configurado puede ser la razón de por qué las conexiones de VPN no funcionaron nunca o dejaron de funcionar después de un tiempo. Si esto sucede, asegúrate de probar con el explorador web del dispositivo iOS o un equipo PC o Mac en la misma red.

### <a name="how-can-i-test-if-vpn-is-working-properly"></a>¿Cómo puedo probar si la VPN funciona correctamente?

Comprueba que la VPN está habilitada en tu dispositivo. Para probar la conexión VPN, ve a una página web en la red interna o usa un servicio web que solo esté disponible a través de la VPN.

### <a name="how-do-i-configure-l2tp-or-pptp-vpn-connections"></a>¿Cómo configuro las conexiones VPN de L2TP o PPTP?

Si usas L2TP o PPTP en la VPN, asegúrate de establecer **Enviar todo el tráfico** en **ON** (Activado) en la configuración de la VPN.

## <a name="web-client"></a>Cliente web

### <a name="which-browsers-can-i-use"></a>¿Qué exploradores puedo usar?

El cliente web es compatible con Microsoft Edge, Internet Explorer 11, Mozilla Firefox (versión 55.0 y posteriores), Safari y Google Chrome.

### <a name="what-pcs-can-i-use-to-access-the-web-client"></a>¿Qué equipos puedo usar para acceder al cliente web?

El cliente web admite Windows, macOS, Linux y ChromeOS. En este momento no se admiten dispositivos móviles.

### <a name="can-i-use-the-web-client-in-a-remote-desktop-deployment-without-a-gateway"></a>¿Puedo usar el cliente web en una implementación de Escritorio remoto sin una puerta de enlace?

No. El cliente requiere una puerta de enlace de Escritorio remoto para conectarse. ¿No sabes qué significa eso? Pregúntale al administrador.

### <a name="does-the-remote-desktop-web-client-replace-the-remote-desktop-web-access-page"></a>¿El cliente web de Escritorio remoto reemplaza la página de acceso web de Escritorio remoto?

No. El cliente web de Escritorio remoto está hospedado en una dirección URL distinta de la página de acceso web de Escritorio remoto. Puedes usar el cliente web o la página de acceso web para ver los recursos remotos en un explorador.

### <a name="can-i-embed-the-web-client-in-another-web-page"></a>¿Puedo insertar el cliente web en otra página web?

Por el momento, no se admite esta característica.

## <a name="monitors-audio-and-mouse"></a>Monitores, audio y mouse

### <a name="how-do-i-use-all-of-my-monitors"></a>¿Cómo uso todos mis monitores?
Para usar dos o más pantallas, haz lo siguiente:

1. Haz clic con el botón derecho en el Escritorio remoto para el que quieres habilitar varias pantallas y, luego, haz clic en **Editar**.
2. Habilita **Use all monitors** (Usar todos los monitores) y **Full screen** (Pantalla completa).

### <a name="is-bi-directional-sound-supported"></a>¿Se admite el sonido bidireccional?
El sonido bidireccional puede configurarse en el cliente de Windows para cada conexión. Se puede acceder a la configuración correspondiente en la sección **Audio remoto** de la pestaña de opciones **Recursos locales**.

### <a name="what-can-i-do-if-the-sound-wont-play"></a>¿Qué puedo hacer si no se reproduce el sonido?
Cierra la sesión (no solo te desconectes, sino que cierra totalmente la sesión) y vuelve a iniciarla.

## <a name="mac-client---hardware-questions"></a>Cliente de Mac: preguntas de hardware
### <a name="is-retina-resolution-supported"></a>¿Se admite la resolución Retina?
Sí, el cliente de Escritorio remoto admite la resolución Retina.

### <a name="how-do-i-enable-secondary-right-click"></a>¿Cómo habilito el clic con el botón derecho secundario?
Tienes tres opciones para usar el clic con el botón derecho en una sesión abierta:

- Mouse USB de PC estándar con dos botones.
- Magic Mouse de Apple: Para habilitar el clic con el botón derecho, haz clic en **Preferencias del sistema** en la base de acoplamiento, haz clic en **Mouse** y habilita **Clic secundario**.
- Magic Trackpad o MacBook Trackpad de Apple: Para habilitar el clic con el botón derecho, haz clic en **Preferencias del sistema** en la base de acoplamiento, haz clic en **Mouse** y habilita **Clic secundario**.

### <a name="is-airprint-supported"></a>¿Se admite AirPrint?
No, el cliente de Escritorio remoto no admite AirPrint. (Esto ocurre tanto en clientes Mac como iOS).

### <a name="why-do-incorrect-characters-appear-in-the-session"></a>¿Por qué aparecen caracteres incorrectos en la sesión?
Si usas un teclado internacional, es posible que tengas el problema de que los caracteres que aparecen en la sesión no coinciden con los caracteres que escribiste en el teclado Mac.

Esto puede ocurrir en los escenarios siguientes:

- Estás usando un teclado que la sesión remota no reconoce. Cuando Escritorio remoto no reconoce el teclado, se establece de manera predeterminada en el último idioma usado con el equipo remoto.
- Te estás conectando a una sesión que se desconectó previamente en un equipo remoto y ese equipo remoto usa un idioma de teclado distinto del idioma que actualmente intentas usar.

Para corregir este problema, establece de manera manual el idioma del teclado de la sesión remota. Consulta los pasos en la sección siguiente.

### <a name="how-do-language-settings-affect-keyboards-in-a-remote-session"></a>¿Cómo afecta la configuración del idioma a los teclados durante una sesión remota?
Hay muchos tipos de distribuciones del teclado de Mac. Algunas son distribuciones específicas de Mac o distribuciones personalizadas para las que es posible que no haya disponible una coincidencia exacta en la versión de Windows con la que te comunicas de manera remota. La sesión remota asigna el teclado a la mejor coincidencia de idioma de teclado disponible en el equipo remoto. 

Si la distribución del teclado Mac está establecida en la versión de PC del teclado de idioma (por ejemplo, Francés - PC), todas las teclas deberían estar correctamente asignadas y el teclado debería funcionar.

Si la distribución del teclado Mac está establecida en la versión de MAC de un teclado (por ejemplo, Francés), la sesión remota te asignará a la versión de PC del idioma francés. Algunas de las combinaciones de teclas de Mac que estás acostumbrado a usar en OSX no van a funcionar en la sesión remota de Windows.

Si la distribución del teclado está establecida en una variación de un idioma (por ejemplo, Francés canadiense) y si la sesión remota no te puede asignar a esa variación exacta, la sesión remota te asignará al idioma más cercano (por ejemplo, Francés). Algunas de las combinaciones de teclas de Mac que estás acostumbrado a usar en OSX no van a funcionar en la sesión remota de Windows.

Si la distribución del teclado está establecida en una distribución con la que la sesión remota no puede coincidir para nada, la sesión remota se establecerá en el valor predeterminado para ofrecerte el último idioma que usaste con ese PC. En este caso, o en casos donde necesitas cambiar el idioma de la sesión remota para que coincida con el teclado de Mac, puedes establecer manualmente el idioma del teclado de la sesión remota en el idioma más cercano al que quieres usar de la siguiente manera.

Usa estas instrucciones para cambiar la distribución del teclado dentro de la sesión de Escritorio remoto:

**En Windows 10 o Windows 8:**

1. Desde dentro de la sesión remota, abre Región e idioma. Haz clic en **Inicio > Configuración > Hora e idioma**. Abre **Región e idioma**.
2. Agrega el idioma que quieres usar. Luego, cierra la ventana Región e idioma.
3. Ahora, en la sesión remota, verás la capacidad de cambiar entre los distintos idiomas. (Al lado derecho de la sesión remota, cerca del reloj). Haz clic en el idioma al que quieres cambiar (como **Eng**).

Puede que tengas que cerrar y reiniciar la aplicación que usas actualmente para que se apliquen los cambios del teclado.


## <a name="specific-errors"></a>Errores específicos

### <a name="why-do-i-get-an-insufficient-privileges-error"></a>¿Por qué recibo un error sobre "Privilegios insuficientes"?
No tienes permiso para acceder a la sesión a la que te quieres conectar. La causa más probable es que te intentas conectar a una sesión de administrador. Solo los administradores pueden conectarse a la consola. Comprueba que el modificador de la consola está desactivado en la configuración avanzada del Escritorio remoto. Si este no es el origen del problema, ponte en contacto con el administrador del sistema para obtener más ayuda.

### <a name="why-does-the-client-say-that-there-is-no-cal"></a>¿Por qué el cliente dice que no hay licencia CAL?
Cuando un cliente de Escritorio remoto se conecta a un servidor de Escritorio remoto, el servidor emite una licencia de acceso de cliente para los Servicios de Escritorio remoto (RDS CAL) que el cliente almacena. Cada vez que el cliente se vuelve a conectar, usará su RDS CAL y el servidor no emitirá otra licencia. El servidor emitirá otra licencia si falta la RDS CAL del dispositivo o si está dañada. Cuando se alcanza el número máximo de dispositivos con licencia, el servidor no emitirá RDS CAL nuevas. Para obtener ayuda, ponte en contacto con el administrador de red.

### <a name="why-did-i-get-an-access-denied-error"></a>¿Por qué recibo un error de "Acceso denegado"?
El error de "Acceso denegado" lo genera la puerta de enlace de Escritorio remoto y es el resultado del uso de credenciales incorrectas durante el intento de conexión. Comprueba tu nombre de usuario y contraseña. Si la conexión funcionaba antes y el error se produjo recientemente, es posible que hayas cambiado la contraseña de tu cuenta de usuario de Windows y no la hayas actualizado todavía en la configuración de Escritorio remoto.

### <a name="what-does-rpc-error-23014-or-error-0x59e6-mean"></a>¿Qué significa "Error 23014 de RPC" o "Error 0x59e6"?
En caso de recibir un **error 23014 de RPC** o un **Error 0x59E6, vuelve a intentarlo después de unos minutos**, es porque el servidor de puerta de enlace de Escritorio remoto alcanzó el número máximo de conexiones activas. El número máximo de conexiones difiere en función de la versión de Windows que se ejecuta en la puerta de enlace de Escritorio remoto: La implementación de Windows Server 2008 R2 Standard limita el número de conexiones a 250. La implementación de Windows Server 2008 R2 Foundation limita el número de conexiones a 50. Todas las demás implementaciones de Windows permiten un número ilimitado de conexiones.

### <a name="what-does-the-failed-to-parse-ntlm-challenge-error-mean"></a>¿Qué significa el error "Failed to parse NTLM challenge" (Error al analizar el desafío NTLM)?
Este error se debe a una configuración incorrecta en el equipo remoto. Asegúrate de que la configuración de nivel de seguridad de RDP en el equipo remoto esté establecida en "Compatible con cliente". (Consulta con el administrador del sistema si necesitas ayuda para hacer esto).

### <a name="what-does-ts_rap-you-are-not-allowed-to-connect-to-the-given-host-mean"></a>¿Qué significa "TS_RAP You are not allowed to connect to the given host" (TS_RAP No estás autorizado a conectarte con el host determinado)?
Este error se produce cuando una directiva de autorización de recursos en el servidor de puerta de enlace impide que tu nombre de usuario se conecte al equipo remoto. Esto puede ocurrir en las siguientes instancias:

- El nombre del equipo remoto es el mismo de la puerta de enlace. Luego, cuando intentas conectarte al equipo remoto, la conexión va en su lugar a la puerta de enlace, a la que probablemente no tienes permiso para acceder. Si necesitas conectarte a la puerta de enlace, no uses el nombre de la puerta de enlace externa como el nombre del equipo. En su lugar, usa "localhost", la dirección IP (127.0.0.1) o el nombre del servidor interno.
- Tu cuenta de usuario no es miembro del grupo de usuarios para acceso remoto.
