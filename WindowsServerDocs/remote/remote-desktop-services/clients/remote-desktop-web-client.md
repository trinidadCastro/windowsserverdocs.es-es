---
title: Acceso al cliente web de Escritorio remoto
description: Describe cómo iniciar sesión el cliente web de escritorio remoto.
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 07/20/2018
ms.topic: article
author: Heidilohr
ms.localizationpriority: medium
ms.openlocfilehash: f4433ad592219d6ed15b28fd0514790b078525fd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849376"
---
# <a name="access-the-remote-desktop-web-client"></a>Acceso al cliente web de Escritorio remoto

El cliente web de escritorio remoto le permite usar un explorador web compatible para acceder a recursos remotos de su organización (aplicaciones y escritorios) publicados en el el administrador. Podrá interactuar con las aplicaciones remotas y escritorios como lo haría con un equipo local independientemente de dónde se encuentre, sin tener que cambiar a un PC de escritorio diferentes. Una vez que el administrador configura los recursos remotos, todo lo que necesita son su dominio, nombre de usuario, contraseña, la dirección URL de su administrador enviada y un explorador web compatible, y está listo para continuar.

>[!NOTE]
>¿Tiene curiosidad acerca de las nuevas versiones para el cliente web? ¿Consulte [Novedades de cliente web de escritorio remoto?](web-client-whatsnew.md)

## <a name="what-youll-need-to-use-the-web-client"></a>Lo que deberá usar al cliente web

* Para el cliente web, necesitará un equipo que ejecuta Windows, macOS, Linux o ChromeOS. No se admiten los dispositivos móviles en este momento.
* Un explorador moderno, como Microsoft Edge, Internet Explorer 11, Google Chrome, Safari o Mozilla Firefox (v55.0 y versiones posteriores).
* La dirección URL de su administrador le envió.

>[!NOTE]
>La versión de Internet Explorer del cliente web no tiene audio en este momento.
>Safari puede mostrar una pantalla gris si el explorador se cambia el tamaño o entra en pantalla completa varias veces.

## <a name="start-using-the-remote-desktop-client"></a>Empezar a usar al cliente de escritorio remoto

Para iniciar sesión el cliente, vaya a la dirección URL de su administrador le envió. En la página de inicio de sesión, escriba su nombre de usuario y dominio en el formato ```DOMAIN\username```, escriba la contraseña y, a continuación, seleccione **inicie sesión en**.

>[!NOTE]
>Al iniciar sesión en el cliente web, acepta que su PC cumple con la directiva de seguridad de su organización.

Después de iniciar sesión, el cliente le llevará a la **todos los recursos** ficha, que contiene todos los elementos publicados para usted en uno o varios grupos que se pueden contraer, como el grupo de "Recursos de trabajo". Verá varios iconos que representan las aplicaciones, escritorios o las carpetas que contienen más aplicaciones o escritorios que el administrador haya facilitado al grupo de trabajo. Puede volver a esta pestaña en cualquier momento para iniciar recursos adicionales.

Para empezar a usar una aplicación o el escritorio, seleccione el elemento que desea usar, escriba el mismo nombre de usuario y contraseña que usó para iniciar sesión en el cliente web si se le solicita y, a continuación, seleccione **enviar**. También puede que aparezca un cuadro de diálogo de consentimiento para tener acceso a recursos locales, como impresoras y el Portapapeles. Puede elegir no redirigir cualquiera de estos, o seleccione **permitir** para usar la configuración predeterminada. Espere a que el cliente web establecer la conexión y, a continuación, empezar a usar el recurso como lo haría normalmente.

Cuando haya terminado, puede finalizar la sesión seleccionando la **cerrar sesión** botón en la barra de herramientas en la parte superior de la pantalla o cerrar la ventana del explorador.

## <a name="printing-from-the-remote-desktop-web-client"></a>Imprimir desde el cliente web de escritorio remoto

Siga estos pasos para imprimir desde el cliente web:

1. Iniciar el proceso de impresión como lo haría normalmente para la aplicación que desea imprimir desde.
2. Cuando se le pedirá que elija una impresora, seleccione **impresora Virtual de escritorio remoto**.
3. Después de elegir sus preferencias, seleccione **impresión**.
4. El explorador generará un archivo PDF del trabajo de impresión.
5. Puede abrir el archivo PDF y su contenido en la impresora local de impresión o guardarlo en su equipo para su uso posterior.

## <a name="copy-and-paste-from-the-remote-desktop-web-client"></a>Copiar y pegar desde el cliente web de escritorio remoto

El cliente web admite actualmente copiar y pegar solo texto. Archivos no se copian o pegados hacia y desde el cliente web. Además, puede usar solo **Ctrl + C** y **CTRL+v** para copiar y pegar texto.

## <a name="get-help-with-the-web-client"></a>Obtener ayuda con el cliente web

Si se ha encontrado un problema que no puede resolverse mediante la información de este artículo, puede obtener ayuda con el cliente web por correo electrónico la dirección en la página About del cliente web.