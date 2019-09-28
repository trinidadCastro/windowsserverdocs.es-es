---
title: Habilitar el modo siempre sin conexión para obtener un acceso más rápido a los archivos
description: Cómo usar el modo Always offline de Archivos sin conexión para proporcionar un acceso más rápido a los archivos almacenados en caché y a las carpetas redirigidas.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: 389fdd26a7e1d9824f1eaf0136a544547f08eb05
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71401954"
---
# <a name="enable-always-offline-mode-for-faster-access-to-files"></a>Habilitar el modo siempre sin conexión para obtener un acceso más rápido a los archivos

>Se aplica a: Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2 y Windows (canal semianual)

En este documento se describe cómo usar el modo Always offline de Archivos sin conexión para proporcionar un acceso más rápido a los archivos almacenados en caché y a las carpetas redirigidas. Siempre sin conexión también proporciona menor uso de ancho de banda porque los usuarios siempre están trabajando sin conexión, incluso cuando están conectados a través de una conexión de red de alta velocidad.

## <a name="prerequisites"></a>Requisitos previos

Para habilitar el modo siempre sin conexión, el entorno debe cumplir los siguientes requisitos previos.

- Un dominio de Active Directory Domain Services (AD DS) con equipos cliente Unidos al dominio. No hay requisitos de nivel funcional de bosque o dominio ni requisitos de esquema.
- Equipos cliente que ejecutan Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012. (Los equipos cliente que ejecutan versiones anteriores de Windows pueden seguir realizando la transición al modo en línea en conexiones de red de alta velocidad).
- Un equipo con la administración de directiva de grupo instalada.

## <a name="enable-always-offline-mode"></a>Habilitar el modo siempre sin conexión

Para habilitar el modo siempre sin conexión, use directiva de grupo para habilitar la configuración de la directiva **configurar el modo de vínculo de baja velocidad** y establecer la latencia en **1** (milisegundo). Si lo hace, los equipos cliente que ejecuten Windows 8 o Windows Server 2012 usarán automáticamente el modo siempre sin conexión.

>[!NOTE]
>Es posible que los equipos que ejecutan Windows 7, Windows Vista, Windows Server 2008 R2 o Windows Server 2008 sigan cambiando al modo en línea si la latencia de la conexión de red cae por debajo de un milisegundo.

1. Abra **Administración de directiva de grupo**.
2. Para crear un nuevo objeto de directiva de grupo (GPO) para la configuración de Archivos sin conexión, haga clic con el botón secundario en el dominio o unidad organizativa (OU) adecuado y, a continuación, seleccione **crear un GPO en este dominio y vincularlo aquí**.
3. En el árbol de consola, haga clic con el botón secundario en el GPO para el que desea configurar las opciones de Archivos sin conexión y, a continuación, seleccione **Editar**. Aparece el **Editor de administración de directivas de grupo** .
4. En el árbol de consola, **en configuración del equipo**, expanda **directivas**, expanda **plantillas administrativas**, expanda **red**y expanda **archivos sin conexión**.
5. Haga clic con el botón derecho en **configurar el modo de vínculo de baja velocidad**y seleccione **Editar**. Aparecerá la ventana **configurar el modo de vínculo de baja velocidad** .
6. Seleccione **Habilitado**.
7. En el cuadro **Opciones** , seleccione **Mostrar**. Aparecerá la **ventana Mostrar contenido** .
8. En el cuadro **nombre de valor** , especifique el recurso compartido de archivos para el que desea habilitar el modo siempre sin conexión.
9. Para habilitar el modo siempre sin conexión en todos los recursos compartidos de archivos, escriba **\*** .
10. En el cuadro **valor** , escriba **latencia = 1** para establecer el umbral de latencia en un milisegundo y luego seleccione **Aceptar**.

>[!NOTE]
>De forma predeterminada, cuando está en modo siempre sin conexión, Windows sincroniza los archivos de la memoria caché del Archivos sin conexión en segundo plano cada dos horas. Para cambiar este valor, use la configuración de directiva **configurar sincronización en segundo plano** .

## <a name="more-information"></a>Más información

* [Información general sobre el redireccionamiento de carpetas, los Archivos sin conexión y los perfiles de usuario móviles](folder-redirection-rup-overview.md)
* [Implementar el redireccionamiento de carpetas con Archivos sin conexión](deploy-folder-redirection.md)