---
title: Habilitar el modo de siempre sin conexión para acelerar el acceso a archivos
description: Cómo usar el modo siempre sin conexión de archivos sin conexión para proporcionar acceso más rápido a los archivos almacenados en caché y las carpetas redirigidas.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: bc54b1e33d09e7f2b9eea01e4f09fb83f13dc1af
ms.sourcegitcommit: 505505b7ec99506e7ff50eddbdd6aa94a602abe6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2018
ms.locfileid: "3831395"
---
# Habilitar el modo de siempre sin conexión para acelerar el acceso a archivos

>Se aplica a: Windows 10, Windows 8, Windows 8.1, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016

En este documento se describe cómo usar el modo siempre sin conexión de archivos sin conexión para proporcionar acceso más rápido a los archivos almacenados en caché y las carpetas redirigidas. Siempre sin conexión también proporciona menor uso de ancho de banda debido a los usuarios siempre están trabajando sin conexión, incluso cuando estén conectados a través de una conexión de red de alta velocidad.

## Requisitos previos

Para habilitar el modo siempre sin conexión, el entorno debe cumplir los siguientes requisitos previos.

- Un dominio de Active Directory Domain Services (AD DS) con los equipos cliente unido al dominio. No existen requisitos de nivel funcional de dominio o bosque o los requisitos de esquema.
- Equipos de cliente que ejecutan Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012. (Equipos cliente que ejecutan versiones anteriores de Windows pueden seguir transición al modo en línea en conexiones de red de alta velocidad).
- Un equipo con administración de directivas de grupo instalado.

## Habilitar el modo de siempre sin conexión

Para habilitar el modo siempre sin conexión, usa la directiva de grupo para habilitar a la configuración de directiva del **modo de vínculo de configurar** y establecer la latencia en **1** (milisegundo). Esto hace que los equipos cliente que ejecutan Windows 8 o Windows Server 2012 a utilizan automáticamente el modo siempre sin conexión.

>[!NOTE]
>Equipos que ejecutan Windows 7, Windows Vista, Windows Server 2008 R2 o Windows Server 2008 puede seguir para realizar la transición al modo en línea si la latencia de la conexión de red desciende por debajo de 1 milisegundo.

1. Abre **administración de directivas de grupo**.
2. Para crear, opcionalmente, un objeto de directiva de grupo (GPO) nuevo para la configuración de archivos sin conexión, haz clic en el dominio adecuado o unidad organizativa (OU) y, a continuación, selecciona **crear un GPO en este dominio y vincularlo aquí**.
3. En el árbol de consola, haz clic en el GPO para la que quieres configurar las opciones de configuración de archivos sin conexión y, a continuación, selecciona **Editar**. Aparece el **Editor de administración de directivas de grupo** .
4. En el árbol de consola, en **Configuración del equipo**, expanda **directivas**, expanda **Plantillas administrativas**, expande la **red**y expande **Archivos sin conexión**.
5. Haz clic en **el modo de vínculo de configurar**y, a continuación, selecciona **Editar**. Aparecerá la ventana de **Configurar el modo de vínculo de baja velocidad** .
6. Selecciona **Habilitado**.
7. En el cuadro **Opciones** , seleccione **Mostrar**. Aparecerá la **ventana de mostrar contenido** .
8. En el cuadro de **nombre de valor** , especifica el recurso compartido de archivos para el que quieres habilitar el modo de siempre sin conexión.
9. Para habilitar el modo de siempre sin conexión en todos los recursos compartidos de archivos, escribe **\***.
10. En el cuadro de **valor** , escribe **latencia = 1** para establecer el umbral de latencia a 1 milisegundo y, a continuación, selecciona **Aceptar**.

>[!NOTE]
>De manera predeterminada, cuando está en modo siempre sin conexión, Windows sincroniza archivos en la memoria caché de archivos sin conexión en segundo plano cada dos horas. Para cambiar este valor, usa la configuración de directiva de **Configurar la sincronización en segundo plano** .

## Más información

* [Información general de la redirección de carpetas, archivos sin conexión y perfiles de usuario móvil](folder-redirection-rup-overview.md)
* [Implementar la redirección de carpetas con archivos sin conexión](deploy-folder-redirection.md)