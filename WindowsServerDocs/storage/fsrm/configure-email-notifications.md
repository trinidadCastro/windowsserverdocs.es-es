---
title: Configurar notificaciones por correo electrónico
description: En este artículo se describe cómo configurar las notificaciones por correo electrónico
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 541ceec25e8cb0fae0b55c3de3be269982546c54
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447660"
---
# <a name="configure-e-mail-notifications"></a>Configurar notificaciones por correo electrónico

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Al crear cuotas y filtros de archivos, tienes la opción de enviar notificaciones por correo electrónico a los usuarios cuando estén llegando al límite de su cuota o cuando hayan intentado guardar archivos que han sido bloqueados. Al generar informes de almacenamiento, tienes la opción de enviar los informes a determinados destinatarios por correo electrónico. Si quieres notificar de forma rutinaria a determinados administradores los eventos de cuota y filtrado de archivos, o enviarles informes de almacenamiento, puedes configurar uno o varios destinatarios predeterminados.

Para enviar estos informes de almacenamiento y notificaciones, debes especificar el servidor SMTP que se utilizará para reenviar los mensajes de correo electrónico.

## <a name="to-configure-e-mail-options"></a>Para configurar las opciones de correo electrónico

1. En el árbol de consola, haz clic con el botón derecho en **Administrador de recursos del servidor de archivos** y luego haz clic en **Configurar opciones**. Se abre el cuadro de diálogo **Opciones del Administrador de recursos del servidor de archivos**.

2. En la pestaña **Notificaciones por correo electrónico**, en **Nombre del servidor SMTP o dirección IP**, escribe el nombre de host o la dirección IP del servidor SMTP que reenviará los informes de almacenamiento y las notificaciones por correo electrónico.

3. Si quieres notificar de forma rutinaria a determinados administradores los eventos de cuota y filtrado de archivos o enviarles por correo electrónico informes de almacenamiento, en **Administradores receptores predeterminados**, escribe cada dirección de correo electrónico.

   Usa el formato <em>account@domain</em>. Usa punto y coma para separar varias cuentas.

4. Para especificar una dirección "De" diferente para las notificaciones por correo electrónico e informes de almacenamiento enviados desde el Administrador de recursos del servidor de archivos, en **Dirección "De" de correo electrónico predeterminada**, escribe la dirección de correo electrónico que quieres que aparezca en el mensaje.

5. Para probar la configuración, haz clic en **Enviar correo electrónico de prueba**.

6. Haga clic en **Aceptar**.


## <a name="see-also"></a>Vea también

-   [Configurar las opciones del Administrador de recursos del servidor de archivos](setting-file-server-resource-manager-options.md)