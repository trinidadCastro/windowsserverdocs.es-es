---
title: Clientes de escritorio remotos preguntas más frecuentes
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
ms.sourcegitcommit: d5f10c0c98a9976a86be9f4fa8866650c7fcfc1a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2019
ms.locfileid: "9065952"
---
# Preguntas más frecuentes acerca de los clientes de escritorio remoto

>Se aplica a: Windows 10, Windows 8.1, Windows Server 2012 R2 y Windows Server 2016

Ahora que has configurado el cliente de escritorio remoto en el dispositivo (Android, Mac, iOS o Windows), tienes preguntas. Aquí encontrarás respuestas a las preguntas más frecuentes acerca de los clientes de escritorio remoto. 

- [Configurar](#Setting-up)
- [Las conexiones, puerta de enlace y redes](#connection-gateway-and-networks)
- [Cliente Web](#web-client)
- [Monitores, audio y mouse](#monitors-audio-and-mouse)
- [Hardware de Mac](#mac-client---hardware-questions)
- [Mensajes de error específicos](#specific-errors)

La mayoría de estas preguntas se aplica a todos los clientes, pero hay algunos elementos específicos de cliente.

Si tienes más preguntas que te gustaría nos responder, deja como comentarios en este artículo.

## Configurar

### ¿Qué equipos puedo conectar con?

Consulta el artículo [configuración admitida](remote-desktop-supported-config.md) para obtener información sobre qué equipos puede conectarse a.

### ¿Cómo configurar un equipo de escritorio remoto?

Tengo mi dispositivo configurado, pero no creo listas del equipo. ¿Ayuda?

¿En primer lugar, ha visto al Asistente para instalación de escritorio remoto? Te enseña preparación tu PC para el acceso remoto. Descarga y ejecución que herramienta en tu PC todo establecida. 

De lo contrario, si prefieres hacer cosas manualmente, sigue leyendo.

Para Windows 10, haz lo siguiente:

1. En el dispositivo al que desea conectarse para abrir la **configuración**.
2. Selecciona el **sistema** y, a continuación, **escritorio remoto**.
3. Usar el control deslizante para habilitar Escritorio remoto.
4. En general, es mejor mantener el equipo activo e intuitivo para facilitar las conexiones. Haz clic en **Mostrar configuración** para ir a la configuración de tu equipo, donde puedes cambiar esta configuración de energía.
   > [!NOTE]
   > No puede conectarse a un equipo que está suspendido o hibernación, por lo tanto, asegúrate de que la configuración de suspensión y la hibernación en el equipo remoto se establece como **nunca**. (Modo de hibernación no está disponible en todos los equipos).


Anota el nombre del equipo en **cómo conectarse a este equipo**. Tendrás que esta opción para configurar a los clientes.

Puede conceder permiso para usuarios específicos tener acceso a este equipo: para ello, haz clic en **Seleccionar usuarios que pueden tener acceso remoto a este equipo**.
Los miembros del grupo de administradores automáticamente tienen acceso.

Para Windows 8.1, sigue las instrucciones para permitir conexiones remotas en [conectarse a otro escritorio, usando las conexiones a Escritorio remoto](https://support.microsoft.com/en-us/help/17463/windows-7-connect-to-another-computer-remote-desktop-connection#1TC=windows-8).



## Conexión, puerta de enlace y redes

### ¿Por qué no puedo conectarme mediante el escritorio remoto?

Estas son algunas posibles soluciones a problemas comunes que podrían surgir al intentar conectarse a un equipo remoto. Si no funcionan estas soluciones, puedes encontrar más ayuda en el [sitio Web de la Comunidad de Microsoft](https://go.microsoft.com/fwlink/p/?LinkId=242079).

- **No se encuentra el equipo remoto.** Asegúrate de que tiene el nombre de equipo correcto y, a continuación, comprueba si se han escrito ese nombre correctamente. Si aún no se puede conectar, intenta usar la dirección IP del equipo remoto en lugar del nombre de equipo.
- **Hay un problema con la red.** Asegúrate de que tener conexión a internet. 
- **El puerto de escritorio remoto podría bloquearse por un firewall.** Si estás usando el Firewall de Windows, sigue estos pasos:

   1. Abrir Firewall de Windows. 
   2. Haz clic en **Permitir una aplicación o una característica a través del Firewall de Windows**. 
   3. Haz clic en **cambiar la configuración**. Es posible que se te pida una contraseña de administrador o para confirmar tu elección.
   4. En **las aplicaciones permitidas y características**, seleccione **Escritorio remoto**y, a continuación, pulsa o haz clic en **Aceptar**.

   Si estás usando un firewall diferente, asegúrate de que el puerto de escritorio remoto (normalmente 3389) esté abierto.
- **Las conexiones remotas no se pueden configurar en el equipo remoto.** Para corregir esto, desplázate copia de seguridad en [¿cómo configurar un equipo de escritorio remoto?](#how-do-i-set-up-a-pc-for-remote-desktop) pregunta en este tema.
- **El equipo remoto solo podría permitir que los equipos conectar con autenticación a nivel de red configurar.** 
- **Puede que esté desactivado el equipo remoto.** No se puede conectar a un equipo que está desactivado, dormido o hibernación, por lo tanto, asegúrate de que la configuración de suspensión y la hibernación en el equipo remoto se configuran en **nunca** (modo de hibernación no está disponible en todos los equipos.).

### ¿Por qué no puedo encontrar o conectarte a mi equipo?
Comprueba lo siguiente:
- ¿Es el equipo y Active?
- ¿Desea escribir la dirección IP o el nombre correcto?

   > [!IMPORTANT]
   > Con el nombre de equipo, requiere la red para resolver el nombre correctamente a través de DNS. En muchas redes domésticas, tienes que usar la dirección IP en lugar del nombre de host para conectarse.
- ¿Es el equipo en una red diferente? ¿Ha configurado el equipo para permitir que fuera de las conexiones a través de?  Echa un vistazo a [Permitir el acceso al PC desde fuera de la red](remote-desktop-allow-outside-access.md) para obtener ayuda.
- ¿Estás conectando a una versión compatible de Windows? 

   > [!NOTE]
   > No se admiten Windows XP Home, Windows Media Center Edition, Windows Vista Home y Windows 7 Home o Starter sin 3 de software de terceros.

### ¿Por qué no se puede iniciar sesión en un equipo remoto?
Si puedes ver la pantalla de inicio de sesión del equipo remoto, pero no se puede iniciar sesión, puede que no haya se ha agregado al grupo de usuarios de escritorio remoto o a cualquier grupo con derechos de administrador en el equipo remoto. Solicitar el administrador del sistema para hacer esto para TI.

### ¿Los métodos de conexión se admiten para las redes de empresa?
Si quieres obtener acceso al escritorio de office desde fuera de la red de empresa, tu empresa debe proporcionar un medio de acceso remoto. El cliente de escritorio remoto se admite actualmente los siguientes:

- Puerta de enlace de Terminal Server o la puerta de enlace de escritorio remoto
- Acceso Web de escritorio remoto
- VPN (a través de opciones de VPN integradas de iOS)

### VPN no funciona

Problemas VPN pueden tener varias causas. El primer paso es comprobar que la VPN funciona en la misma red que un equipo PC o Mac. Si no se pueden probar con un equipo o Mac, puedes intentar acceder a una página de web de intranet de empresa con el Explorador de tu dispositivo.

Otras cosas que debes comprobar:
- **La red de 3G se bloquea o daña VPN.** Hay varios proveedores de 3G en el mundo que parecen bloque o en el tráfico de 3G dañado. Comprobar la conectividad VPN funciona correctamente para más de un minuto.
- **L2TP o VPN PPTP.** Si estás usando L2TP o PPTP en la VPN, establezca enviar todo el tráfico en ON en la configuración de VPN.
- **VPN está mal configurada.** Un servidor VPN mal configurado puede ser el motivo las conexiones VPN nunca trabajan o dejó de funcionar después de algún tiempo. Asegúrate de que pruebas con iOS el explorador web de dispositivo o un equipo o Mac en la misma red si esto ocurre.

### ¿Cómo se puede probar si funciona correctamente VPN?
Comprueba que la VPN esté habilitada en el dispositivo. Puedes probar la conexión VPN si va a una página Web en la red interna o mediante un servicio web que solo está disponible a través de la VPN.

### ¿Cómo se configura las conexiones VPN de PPTP o L2TP?
Si estás usando L2TP o PPTP en la VPN, asegúrate de que establecer **Enviar todo el tráfico** **on** en la configuración de VPN.

## Cliente Web

### ¿Qué exploradores se debe usar?

El cliente web es compatible con Microsoft Edge, Internet Explorer 11, Mozilla Firefox (v55.0 y versiones posteriores), Safari y Google Chrome.

### ¿Qué equipos puedo usar para acceder al cliente web?

El cliente web es compatible con Windows, macOS, Linux y ChromeOS. Los dispositivos móviles no se admiten en este momento.

### ¿Puedo usar al cliente web en una implementación de escritorio remoto sin una puerta de enlace?

No. El cliente requiere una puerta de enlace de escritorio remoto para conectarse. ¿No sabes lo que significa? Preguntar por el administrador se.

### ¿El cliente de web de escritorio remoto reemplaza la página de acceso Web de escritorio remoto?

No. El cliente de web de escritorio remoto está alojado en una dirección URL de la página de acceso Web de escritorio remoto. Puedes usar el cliente web o la página Web Access para ver los recursos remotos en un explorador.

### ¿Puedo incrustar al cliente web en otra página web?

Esta característica no se admite en ese momento.

## Monitores, audio y mouse

### Cómo usar todos los monitores
Para usar las pantallas de dos o más, hacer lo siguiente:

1. Haz clic en el escritorio remoto que deseas habilitar varias pantallas para y, a continuación, haz clic en **Editar**.
2. Habilitar **pantalla completa**y **usar a todos los monitores** .

### ¿Se admite el sonido bidireccional?
Sonido corriente (desde el cliente al servidor, para micrófonos) no es compatible con el cliente de escritorio remoto.

### ¿Qué puedo hacer si no reproduce el sonido?
Cerrar la sesión (no solo desconectar, cerrar todo el recorrido) y, a continuación, vuelve a iniciar sesión.

## Cliente de Mac - preguntas de hardware
### ¿Se admite la resolución de retina?
Sí, el cliente de escritorio remoto admite resolución retina.

### ¿Cómo puedo habilitar secundario con el botón secundario?
Para hacer uso de la con el botón derecho dentro de una sesión abierta tiene tres opciones:

- Ratón estándar de PC dos botones USB
- Apple mágico Mouse: Para habilitar con el botón derecho, haz clic en **Las preferencias del sistema** en la base dock, haz clic en el **Mouse**y, a continuación, habilitar **secundaria, haz clic en**.
- Panel táctil mágica de Apple o MacBook Trackpad: para habilitar con el botón secundario, haz clic en **Las preferencias del sistema** en la base dock, haz clic en el **Mouse**y, a continuación, habilitar **secundaria, haz clic en**.

### ¿Se admite AirPrint?
No, el cliente de escritorio remoto no admite AirPrint. (Esto es true para los clientes de iOS y Mac).

### ¿Por qué aparecen caracteres incorrectos en la sesión?
Si estás usando un teclado internacional, es posible que veas un problema donde los caracteres que aparecen en la sesión de coincidir con el que ha escrito en el teclado de Mac.

Esto puede ocurrir en los siguientes escenarios:

- Estás usando un teclado que no reconoce la sesión remota. Al escritorio remoto no reconoce el teclado, el valor predeterminado es el idioma que se usó por última vez con el equipo remoto.
- Se conecta a una sesión previamente desconectada en un equipo remoto y que el equipo remoto usa un idioma del teclado diferente actualmente está intentando usar.

Para corregir este problema, establecer manualmente el idioma del teclado para la sesión remota. Consulta los pasos descritos en la siguiente sección.

### ¿Cómo afecta configuración de idioma teclados en una sesión remota?
Hay muchos tipos de distribuciones de teclado de Mac. Algunas de ellas son diseños específicos de Mac o diseños personalizados para la que no puede estar disponible en la versión de Windows de comunicación remota en una coincidencia exacta. La sesión remota asigna el teclado para lograr la mejor coincidencia de idioma del teclado disponible en el equipo remoto. 

Si se establece el diseño de teclado de Mac en la versión del equipo del teclado de idioma (por ejemplo, francés: PC) todas las claves deben asignarse correctamente y el teclado debería funcionar.

Si se establece el diseño de teclado de Mac en la versión de Mac de un teclado (por ejemplo, francés) la sesión remota asignará a la versión de PC de francés. Algunos de los métodos abreviados de teclado de Mac que se usa para usar en OSX no funcionarán en la sesión remota de Windows.

Si el diseño de teclado se establece en una variación de un idioma (por ejemplo, francés canadiense) y la sesión remota no puede asignar a esa variación exacta, la sesión remota asignará al lenguaje más cercano (por ejemplo, francés). Algunos de los métodos abreviados de teclado de Mac que se usa para usar en OSX no funcionarán en la sesión remota de Windows.

Si se establece el diseño de teclado en un diseño de que la sesión remota no coincide en modo alguno, predeterminado de la sesión remota para dar el idioma que se usó por última vez con ese equipo. En este caso, o en casos donde necesites cambiar el idioma de la sesión remota para que coincida con el teclado de Mac, puedes establecer manualmente el idioma del teclado en la sesión remota para el idioma que se parezca a la que quieres usar como sigue.

Usa las siguientes instrucciones para cambiar el diseño de teclado dentro de la sesión de escritorio remoto:

**En Windows 10 o Windows 8:**

1. Desde dentro de la sesión remota, abre el idioma y región. Haga clic en **Inicio > configuración > hora e idioma**. Abre el **idioma y región**.
2. Agregue el idioma que quieras usar. A continuación, cierra la ventana de idioma y región.
3. Ahora, en la sesión remota, verás la posibilidad de cambiar entre idiomas. (En el lado derecho de la sesión remota, cerca del reloj.) Haz clic en el idioma que quieras cambiar a (por ejemplo, **inglés**).

Es posible que debes cierra y reinicia la aplicación que esté usando actualmente para que los cambios de teclado surta efecto.


## Errores específicos

### ¿Por qué se puede obtener un error de "Privilegios insuficientes"?
No se permiten tener acceso a la sesión que se va a conectar a. Lo más probable es que está intentando conectarse a una sesión de administrador. Solo los administradores pueden conectarse a la consola. Comprueba que el conmutador de consola está desactivado en la configuración avanzada del escritorio remoto. Si no es el origen del problema, ponte en contacto con el administrador del sistema para obtener más ayuda.

### ¿Por qué el cliente decir que no hay ninguna CAL?
Cuando un cliente de escritorio remoto se conecta a un servidor de escritorio remoto, el servidor emite un escritorio servicios de cliente de licencia de acceso remoto (CAL de RDS) almacenados por el cliente. Siempre que se conecta el cliente nuevo usará su CAL de RDS y el servidor no emitirá otra licencia. El servidor emitirá otra licencia si la CAL de RDS en el dispositivo está dañado o no. Cuando se alcanza el número máximo de dispositivos con licencia el servidor no emitirá CAL de RDS de nuevo. Para obtener ayuda, ponte en contacto con el Administrador de red.

### ¿Por qué recibí un error de "Acceso denegado"?
El error "Acceso denegado" es un generado por la puerta de enlace de escritorio remoto y el resultado de credenciales incorrectas durante el intento de conexión. Comprueba el nombre de usuario y la contraseña. Si la conexión trabajado antes y se produjo el error recientemente, posiblemente cambiar la contraseña de su cuenta de usuario de Windows y no se hayan actualizado aún en la configuración del escritorio remota.

### ¿Qué "Error RPC 23014" o Media "Error 0x59e6"?
En el caso de un **error RPC 23014** o **Error 0x59E6 a intentarlo después de esperar unos minutos**, el servidor de puerta de enlace de escritorio remoto ha alcanzado el número máximo de conexiones activas. Dependiendo de la versión de Windows que se ejecutan en la puerta de enlace de escritorio remoto el número máximo de conexiones difiere: implementación The Windows Server 2008 R2 Standard limita el número de conexiones a 250. La implementación de Windows Server 2008 R2 Foundation limita el número de conexiones a 50. Todas las otras implementaciones de Windows permiten a una cantidad ilimitada de conexiones.

### ¿Qué significa el error "No se pudo analizar el desafío NTLM"?
Este error se debe a un error de configuración en el equipo remoto. Asegúrate de que la configuración de nivel de seguridad RDP en el equipo remoto se establece en "Compatible con el cliente". (Hablar con el administrador del sistema si necesitas ayuda hacerlo).

### ¿Qué "TS_RAP no puede conectarse a la host determinado" significa?
Este error se produce cuando una directiva de autorización de recursos en el servidor de puerta de enlace se detiene el nombre de usuario puedan conectarse al equipo remoto. Esto puede suceder en los siguientes casos:

- El nombre de equipo remoto es el mismo que el nombre de la puerta de enlace. A continuación, cuando se intenta conectar al equipo remoto, la conexión va a la puerta de enlace en su lugar, que probablemente no tienen permiso para acceder a. Si es necesario para conectarse a la puerta de enlace, no usa el nombre de la puerta de enlace externa como nombre de equipo. En su lugar, usa "localhost" o la dirección IP (127.0.0.1) o el nombre de servidor interno.
- Tu cuenta de usuario no es un miembro del grupo de usuarios para el acceso remoto.