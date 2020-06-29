---
title: Configurar notificaciones por correo electrónico
description: En este artículo se describe cómo configurar notificaciones por correo electrónico.
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: b80aef85d6f4a1d6bd8c05b56d7c1a12b456d1ed
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85474192"
---
# <a name="configure-e-mail-notifications"></a>Configurar notificaciones por correo electrónico

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Cuando cree cuotas y filtros de archivos, tiene la opción de enviar notificaciones de correo electrónico a usuarios cuando se acerquen a su límite de cuota o si intentan guardar archivos bloqueados. Cuando se generan informes de almacenamiento, se tiene la opción de enviarlos por correo electrónico a destinatarios concretos. Si desea informar de los eventos de filtrado y cuotas a determinados administradores o enviar informes de almacenamiento de forma habitual, puede configurar uno o más destinatarios predeterminados.

Para enviar estas notificaciones e informes de almacenamiento, debe especificar el servidor SMTP que se va a usar para reenviar los mensajes de correo electrónico.

## <a name="to-configure-e-mail-options"></a>Para configurar opciones de correo electrónico

1. En el árbol de consola, haga clic con el botón secundario en **servidor de archivos administrador de recursos**y, a continuación, haga clic en **configurar opciones**. Se abre el cuadro de diálogo **Opciones del Administrador de recursos del servidor de archivos**.

2. En la pestaña **notificaciones de correo electrónico** , en **nombre del servidor SMTP o dirección IP**, escriba el nombre de host o la dirección IP del servidor SMTP que reenviará las notificaciones de correo electrónico y los informes de almacenamiento.

3. Si desea notificar de forma rutinaria a determinados administradores acerca de los eventos de cuota o de filtrado de archivos o los informes de almacenamiento de correo electrónico, en **destinatarios de administrador predeterminados**, escriba cada dirección de correo electrónico.

   Use el formato <em>account@domain</em> . Separe las distintas cuentas con punto y coma.

4. Para especificar una dirección "de" diferente para las notificaciones de correo electrónico y los informes de almacenamiento enviados desde el servidor de archivos Administrador de recursos, en la **dirección de correo electrónico predeterminada "de"**, escriba la dirección de correo electrónico que desea que aparezca en el mensaje.

5. Para probar la configuración, haga clic en **Enviar correo electrónico de prueba**.

6. Haga clic en **OK**.


## <a name="additional-references"></a>Referencias adicionales

-   [Configurar las opciones del Administrador de recursos del servidor de archivos](setting-file-server-resource-manager-options.md)