---
title: Habilitar el modo siempre sin conexión para acelerar el acceso a archivos
description: Cómo usar el modo siempre sin conexión de archivos sin conexión para proporcionar un acceso más rápido a los archivos almacenados en caché y las carpetas redirigidas.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: 8684926beb0f0c911ac384970d15ba7d25f84079
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2019
ms.locfileid: "65475929"
---
# <a name="enable-always-offline-mode-for-faster-access-to-files"></a>Habilitar el modo siempre sin conexión para acelerar el acceso a archivos

>Se aplica a: Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2 y Windows (canal semianual)

Este documento describe cómo usar el modo siempre sin conexión de archivos sin conexión para proporcionar un acceso más rápido a los archivos almacenados en caché y las carpetas redirigidas. Siempre sin conexión también proporciona el uso de ancho de banda inferior porque los usuarios siempre están trabajando sin conexión, incluso cuando están conectados a través de una conexión de red de alta velocidad.

## <a name="prerequisites"></a>Requisitos previos

Para habilitar el modo siempre sin conexión, el entorno debe cumplir los siguientes requisitos previos.

- Un dominio de Active Directory Domain Services (AD DS) con los equipos cliente Unidos al dominio. No existen requisitos de nivel funcional del bosque o dominio ni requisitos de esquema.
- Equipos cliente que ejecutan Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012. (Los equipos cliente que ejecutan versiones anteriores de Windows podrían seguir para realizar la transición al modo en línea en las conexiones de red de alta velocidad).
- Un equipo con administración de directivas de grupo instalada.

## <a name="enable-always-offline-mode"></a>Habilitar el modo siempre sin conexión

Para habilitar el modo siempre sin conexión, use Directiva de grupo para habilitar el **configurar el modo de vínculo de baja velocidad** directiva de configuración y establece la latencia en **1** (en milisegundos). Si lo hace, los equipos cliente que ejecutan Windows 8 o Windows Server 2012 para que use automáticamente el modo siempre sin conexión.

>[!NOTE]
>Los equipos que ejecutan Windows 7, Windows Vista, Windows Server 2008 R2 o Windows Server 2008 puede seguir para realizar la transición al modo en línea si la latencia de la conexión de red se sitúa por debajo de un milisegundo.

1. Abra **administración de directivas de grupo**.
2. Para, opcionalmente, crear un objeto de directiva de grupo (GPO) nuevos para la configuración de archivos sin conexión, haga clic en el dominio adecuado o unidad organizativa (OU) y, a continuación, seleccione **crear un GPO en este dominio y vincularlo aquí**.
3. En el árbol de consola, haga clic en el GPO para el que desea configurar las opciones de archivos sin conexión y, a continuación, seleccione **editar**. El **Editor de administración de directivas de grupo** aparece.
4. En el árbol de consola, bajo **configuración del equipo**, expanda **directivas**, expanda **plantillas administrativas**, expanda **red**, y expanda **archivos sin conexión**.
5. Haga clic en **configurar el modo de vínculo de baja velocidad**y, a continuación, seleccione **editar**. El **configurar el modo de vínculo de baja velocidad** aparecerá la ventana.
6. Seleccione **Habilitado**.
7. En el **opciones** cuadro, seleccione **mostrar**. El **ventana Mostrar contenido** aparecerá.
8. En el **nombre del valor** , especifique el recurso compartido de archivos para el que desea habilitar el modo siempre sin conexión.
9. Para habilitar el modo siempre sin conexión en todos los recursos compartidos de archivos, escriba **\***.
10. En el **valor** , escriba **latencia = 1** para establecer el umbral de latencia a un milisegundo y, a continuación, seleccione **Aceptar**.

>[!NOTE]
>De forma predeterminada, cuando está en modo siempre sin conexión, Windows sincronización los archivos de la memoria caché de archivos sin conexión en segundo plano cada dos horas. Para cambiar este valor, use la **Configurar sincronización en segundo plano** configuración de directiva.

## <a name="more-information"></a>Más información

* [Información general sobre la redirección de carpetas, archivos sin conexión y perfiles de usuario móviles](folder-redirection-rup-overview.md)
* [Implementar la redirección de carpetas con archivos sin conexión](deploy-folder-redirection.md)