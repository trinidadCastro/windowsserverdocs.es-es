---
title: Habilitar el modo Siempre sin conexión para obtener acceso más rápido a los archivos
description: Cómo usar el modo Siempre sin conexión de Archivos sin conexión para proporcionar un acceso más rápido a los archivos almacenados en caché y a las carpetas redirigidas.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: 389fdd26a7e1d9824f1eaf0136a544547f08eb05
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2020
ms.locfileid: "71401954"
---
# <a name="enable-always-offline-mode-for-faster-access-to-files"></a>Habilitar el modo Siempre sin conexión para obtener acceso más rápido a los archivos

>Se aplica a: Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2 y Windows (canal semianual)

En este documento se describe cómo usar el modo Siempre sin conexión de Archivos sin conexión para proporcionar un acceso más rápido a los archivos almacenados en caché y a las carpetas redirigidas. El modo Siempre sin conexión también proporciona un menor uso de ancho de banda, ya que los usuarios siempre trabajan sin conexión, incluso cuando están conectados a través de una conexión de red de alta velocidad.

## <a name="prerequisites"></a>Requisitos previos

Antes de habilitar el modo Siempre sin conexión, el entorno debe cumplir los siguientes requisitos previos.

- Un dominio de Active Directory Domain Services (AD DS) con equipos cliente unidos al dominio. No se aplican requisitos de nivel funcional de bosque o dominio ni requisitos de esquema.
- Los equipos cliente ejecutan Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012. (Los equipos cliente que ejecutan versiones anteriores de Windows pueden seguir realizando la transición al modo En línea en conexiones de red de alta velocidad).
- Un equipo que tenga instalada la herramienta Administración de directivas de grupo.

## <a name="enable-always-offline-mode"></a>Habilitar el modo Siempre sin conexión

Para habilitar el modo Siempre sin conexión, usa la directiva de grupo para habilitar la configuración de directiva **Configurar el modo de vínculo de baja velocidad** y establece la latencia en **1** (milisegundo). Al hacerlo, los equipos cliente que ejecuten Windows 8 o Windows Server 2012 usarán automáticamente el modo Siempre sin conexión.

>[!NOTE]
>Es posible que los equipos que ejecutan Windows 7, Windows Vista, Windows Server 2008 R2 o Windows Server 2008 sigan realizando la transición al modo En línea si la latencia de la conexión de red cae por debajo de un milisegundo.

1. Abre **Administración de directivas de grupo**.
2. Para crear un nuevo objeto de directiva de grupo (GPO) para la configuración de Archivos sin conexión, haz clic con el botón derecho en el dominio o la unidad organizativa que desees y, a continuación, selecciona **Create a GPO in this domain, and link it here** (Crear un GPO en este dominio y vincularlo aquí).
3. En el árbol de consola, haz clic con el botón derecho en el GPO para el que quieres configurar las opciones de Archivos sin conexión y, a continuación, selecciona **Editar**. Se muestra el **Editor de administración de directivas de grupo**.
4. En el árbol de consola, en **Configuración del equipo**, expande las opciones **Directivas**, **Plantillas administrativas**, **Red** y **Archivos sin conexión**.
5. Haz clic con el botón derecho en **Configurar el modo de vínculo de baja velocidad** y selecciona **Editar**. Aparecerá la ventana **Configurar el modo de vínculo de baja velocidad**.
6. Seleccione **Habilitado**.
7. En el cuadro **Opciones**, selecciona **Mostrar**. Aparecerá la ventana **Mostrar contenido**.
8. En el cuadro **Nombre de valor**, especifica el recurso compartido de archivos para el que quieres habilitar el modo Siempre sin conexión.
9. Para habilitar el modo Siempre sin conexión en todos los recursos compartidos de archivos, escribe **\*** .
10. En el cuadro **Valor**, escribe **Latencia=1** para establecer el umbral de latencia en un milisegundo y, a continuación, selecciona **Aceptar**.

>[!NOTE]
>De forma predeterminada, cuando está ajustado el modo Siempre sin conexión, Windows sincroniza los archivos de la memoria caché de Archivos sin conexión en segundo plano cada dos horas. Para cambiar este valor, usa la configuración de directiva **Configurar sincronización en segundo plano**.

## <a name="more-information"></a>Información adicional

* [Introducción al redireccionamiento de carpetas, archivos sin conexión y perfiles de usuario móvil](folder-redirection-rup-overview.md)
* [Implementar la redirección de carpetas con Archivos sin conexión](deploy-folder-redirection.md)