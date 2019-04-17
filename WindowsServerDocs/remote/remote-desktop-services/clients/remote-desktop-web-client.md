---
title: Acceso al cliente web de Escritorio remoto
description: Describe cómo iniciar sesión en el cliente de web de escritorio remoto.
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 07/20/2018
ms.topic: article
author: Heidilohr
ms.localizationpriority: medium
ms.openlocfilehash: f4433ad592219d6ed15b28fd0514790b078525fd
ms.sourcegitcommit: d5f10c0c98a9976a86be9f4fa8866650c7fcfc1a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2019
ms.locfileid: "9065862"
---
# Acceso al cliente web de Escritorio remoto

El cliente de web de escritorio remoto te permite usar un explorador web compatible para tener acceso a recursos remoto de la organización (aplicaciones y escritorios) publicados en la el administrador. Podrás interactuar con las aplicaciones remotas y los equipos de escritorio como lo harías con un equipo local, independientemente de dónde se encuentre, sin tener que cambiar a un equipo de escritorio diferente. Una vez que el administrador configura los recursos remotos, todo lo que necesitas son tu dominio, el nombre de usuario, la contraseña, la dirección URL de tu administrador enviado y un explorador web compatible, y estás listo.

>[!NOTE]
>¿Curiosidad acerca de las nuevas versiones para el cliente web? Echa un vistazo a [Novedades de cliente de web de escritorio remoto?](web-client-whatsnew.md)

## Lo que tendrás que usar al cliente web

* Para el cliente web, necesitarás un equipo que ejecute Windows, macOS, ChromeOS o Linux. Los dispositivos móviles no se admiten en este momento.
* Un explorador moderno, como Microsoft Edge, Internet Explorer 11, Google Chrome, Safari o Mozilla Firefox (v55.0 y versiones posteriores).
* La dirección URL el administrador ha enviado.

>[!NOTE]
>La versión de Internet Explorer del cliente web carece de audio en este momento.
>Safari puede mostrar una pantalla gris si el explorador cambie de tamaño o entra en pantalla completa varias veces.

## Empezar a usar al cliente de escritorio remoto

Para iniciar sesión en el cliente, ve a la dirección URL, el administrador ha enviado. En la página de inicio de sesión, escribe tu nombre de usuario y de dominio en el formato ```DOMAIN\username```, escribe tu contraseña y, a continuación, selecciona el **Inicio de sesión**.

>[!NOTE]
>Al iniciar sesión en el cliente web, aceptas que el equipo cumple con la directiva de seguridad de la organización.

Después de iniciar sesión, el cliente te llevará a la pestaña de **Todos los recursos** , que contiene todos los elementos publicados te dentro de uno o más grupos contraíbles, como el grupo de "Recursos de trabajo". Podrás ver varios iconos que representan las aplicaciones, equipos de escritorio o carpetas que contienen más aplicaciones o equipos de escritorio que el administrador ha puesto a disposición del grupo de trabajo. Puede volver a esta ficha en cualquier momento para iniciar recursos adicionales.

Para empezar a usar una aplicación o el escritorio, selecciona el elemento que quieres usar, escriba el mismo nombre de usuario y contraseña que utiliza para iniciar sesión en el cliente de web si se te solicite y, a continuación, selecciona **Enviar**. También es posible que se muestra un cuadro de diálogo de consentimiento para tener acceso a recursos locales, como impresoras y el Portapapeles. Puedes elegir no redirigir cualquiera de estas o selecciona **Permitir** a usar la configuración predeterminada. Espera a que el cliente de web establecer la conexión y, a continuación, empezar a usar el recurso como lo harías normalmente.

Cuando hayas terminado, puede terminar la sesión activando el botón **Cerrar** en la barra de herramientas en la parte superior de la pantalla o cerrar la ventana del explorador.

## Imprimir desde el cliente de web de escritorio remoto

Sigue estos pasos para imprimir desde el cliente web:

1. Iniciar el proceso de impresión como lo harías normalmente para la aplicación que quieras imprimir desde.
2. Cuando se te solicite que elija una impresora, seleccione **Impresora Virtual de escritorio remoto**.
3. Después de elegir tus preferencias, seleccione **Imprimir**.
4. El explorador generará un archivo PDF de su trabajo de impresión.
5. Puedes elegir abrir el archivo PDF e imprimir su contenido en la impresora local o guardarlo en el equipo para su uso posterior.

## Copiar y pegar desde el cliente de web de escritorio remoto

El cliente web admite actualmente copiar y pegar texto solo. Archivos no copiar o pegar hacia y desde el cliente web. Además, solo puedes usar **CTRL+c** y **CTRL+v** para copiar y pegar texto.

## Obtén ayuda con el cliente web

Si se ha encontrado un problema que no se resuelve mediante la información de este artículo, puedes obtener ayuda con el cliente de web por correo electrónico en la dirección en la página de información acerca del cliente web.