---
title: Introducción al cliente web
description: Describe cómo iniciar sesión en el cliente web de Escritorio remoto.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 08/27/2019
ms.topic: article
author: Heidilohr
ms.localizationpriority: medium
ms.openlocfilehash: 6c37c28fded08ba68e46a10a534ee0269c714938
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2020
ms.locfileid: "74189473"
---
# <a name="get-started-with-the-web-client"></a>Introducción al cliente web

El cliente web de Escritorio remoto te permite usar un explorador web compatible para acceder a los recursos remotos de tu organización (aplicaciones y escritorios) publicados para ti por el administrador. Podrás interactuar con las aplicaciones y escritorios remotos como lo harías con un equipo local, independientemente de dónde te encuentres, sin tener que cambiar a otro PC de escritorio. Una vez que el administrador configura los recursos remotos, todo lo que necesitas es tu dominio, nombre de usuario, contraseña, la dirección URL que te ha enviado el administrador y un explorador web compatible, y estás listo para continuar.

>[!NOTE]
>¿Tienes curiosidad acerca de las nuevas versiones para el cliente web? Consulte [¿Novedades para el cliente web de escritorio remoto?](web-client-whatsnew.md)

## <a name="what-youll-need-to-use-the-web-client"></a>Qué necesitarás para usar el cliente web

* Para el cliente web, necesitará un equipo con Windows, macOS, Linux o ChromeOS. En este momento no se admiten dispositivos móviles.
* Un explorador moderno, como Microsoft Edge, Internet Explorer 11, Google Chrome, Safari o Mozilla Firefox (v55.0 y versiones posteriores).
* La dirección URL que te ha enviado tu administrador.

>[!NOTE]
>La versión de Internet Explorer del cliente web no tiene audio en este momento.
>Safari puede mostrar una pantalla gris si se cambia el tamaño del explorador o entra en pantalla completa varias veces.

## <a name="start-using-the-remote-desktop-client"></a>Comenzar a usar el cliente de Escritorio remoto

Para iniciar sesión el cliente, ve a la dirección URL que te ha enviado el administrador. En la página de inicio de sesión, escribe  tu dominio y nombre de usuario en el formato ```DOMAIN\username```, escribe la contraseña y, luego, selecciona **Iniciar sesión**.

>[!NOTE]
>Al iniciar sesión en el cliente web, aceptas que tu PC cumple con la directiva de seguridad de tu organización.

Después de iniciar sesión, el cliente te llevará a la pestaña **Todos los recursos**, que contiene todos los elementos publicados para ti en uno o varios grupos que se pueden contraer, como el grupo de "Recursos de trabajo". Verás varios iconos que representan las aplicaciones, escritorios o carpetas que contienen más aplicaciones o escritorios que el administrador ha facilitado al grupo de trabajo. Puedes volver a esta pestaña en cualquier momento para iniciar recursos adicionales.

Para empezar a usar una aplicación o escritorio, selecciona el elemento que quieres usar, escribe el mismo nombre de usuario y contraseña que usaste para iniciar sesión en el cliente web si se te solicitan y, luego, seleccione **Enviar**. También puede que aparezca un cuadro de diálogo de consentimiento para acceder a recursos locales, como impresoras y el Portapapeles. Puede elegir no redirigir a ninguno de estos, o seleccionar **Permitir** para usar la configuración predeterminada. Espere a que el cliente web establezca la conexión y, luego, empieza a usar el recurso como lo harías normalmente.

Cuando hayas terminado, puedes finalizar la sesión con el botón **Cerrar sesión** en la barra de herramientas en la parte superior de la pantalla o cerrar la ventana del explorador.

## <a name="printing-from-the-remote-desktop-web-client"></a>Impresión desde el cliente web de Escritorio remoto

Sigue estos pasos para imprimir desde el cliente web:

1. Inicia el proceso de impresión como lo harías normalmente para la aplicación desde la que quieres imprimir.
2. Cuando se te pida que elijas una impresora, selecciona **Remote Desktop Virtual Printer** (Impresora virtual de Escritorio remoto).
3. Después de elegir las preferencias, selecciona **Imprimir**.
4. El explorador generará un archivo PDF del trabajo de impresión.
5. Puedes elegir abrir el archivo PDF e imprimir su contenido en la impresora local o guardarlo en tu equipo para usarlo más adelante.

## <a name="copy-and-paste-from-the-remote-desktop-web-client"></a>Copiar y pegar desde el cliente web de Escritorio remoto

El cliente web admite actualmente copiar y pegar solo texto. No pueden copiarse ni pegarse archivos desde y hacia el cliente web. Además, puedes usar solo **Ctrl+C** y **Ctrl+V** para copiar y pegar texto.

## <a name="use-an-input-method-editor-ime-in-the-remote-session"></a>Usar un editor de métodos de entrada (IME) en la sesión remota

Para usar un editor de métodos de entrada para escribir caracteres complejos en la sesión remota, selecciona el icono con forma de engranaje en la barra de navegación para abrir el panel lateral de la **configuración**, y establece la opción para **habilitar el editor de métodos de entrada** como **Activada**. Recuerda que debes tener un editor de métodos de entrada instalado y habilitado en la sesión remota. 

## <a name="get-help-with-the-web-client"></a>Obtención de ayuda con el cliente web

Si tienes un problema que no puede resolverse con la información de este artículo, puedes obtener ayuda para el cliente web enviando un correo electrónico a la dirección que aparece en la página Acerca de del cliente web.
