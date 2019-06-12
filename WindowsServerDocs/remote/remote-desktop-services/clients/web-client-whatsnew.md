---
title: ¿Novedades de cliente web de escritorio remoto?
description: Obtenga información sobre los cambios recientes en el cliente web de escritorio remoto
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 05/20/2019
ms.localizationpriority: medium
ms.openlocfilehash: 5be9b05da1e78cc54e12254f43d0f44f7ff65c5d
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66804884"
---
# <a name="whats-new-for-the-remote-desktop-web-client"></a>¿Novedades para el cliente web de escritorio remoto?

Se actualiza con regularidad el [cliente web de escritorio remoto](remote-desktop-web-client.md), agregar nuevas características y solución de problemas. Consulte las actualizaciones más recientes a continuación.

> [!NOTE]
> Hemos cambiado el sistema de control de versiones para el cliente web. A partir de la versión 1.0.18.0, todas las versiones de lanzamiento de cliente web contiene números (en el formato "W.x.y.z"). Números de versión para el cliente web de escritorio remoto siempre finalizará con un 0 (por ejemplo, W.X.Y.0). Cada versión de cliente web de Escritorio Virtual de Windows cambiará el último dígito hasta la próxima versión de cliente de web de escritorio remoto (por ejemplo, 1.0.18.1).

## <a name="updates-for-version-10180"></a>Actualizaciones de versión 1.0.18.0
*Fecha de publicación: 5/14/2019*

- Configuración de recurso el método de inicio se ha agregado en la pestaña Configuración, permitiendo a los usuarios abrir recursos en el explorador o descargar un archivo .rdp para controlar con otro cliente. Esta opción se puede configurar el administrador. Detalles acerca de las configuraciones de administrador para esta característica se encuentra en la [web de documentación de instalación de cliente](remote-desktop-web-client-admin.md).
- Color fijo presentar problemas, lo que permite más vivos son los colores de la sesión remota.
- Mensajes de error revisada relacionados con errores de recursos remotos de fuente. 
- Se agregó compatibilidad para varios accesos directos de office, como Pegado especial (Ctrl + Alt + V).
- Método abreviado de teclado se ha agregado a los usuarios invocar la clave de Windows en la sesión remota (ALT+F3)
- Mensaje de error actualizada para los usuarios que intentan autenticarse mediante una contraseña expirada.
- Fuente actualizada la interfaz de usuario en la página de todos los recursos.
- Vuelva a conectar resueltos diálogos superpuestos que se produjeron durante la sesión.
- Se ha corregido el ajuste de tamaño de icono de recurso remoto en la barra de tareas de recursos.

## <a name="updates-for-version-1011"></a>Actualizaciones de versión 1.0.11
*Fecha de publicación: 2/22/2019*

- Habilitar conexión a Escritorio remoto Broker sin una puerta de enlace de escritorio remoto en Windows Server 2019.
- Ordena alfabéticamente las fuentes de distribución (es decir, RemoteApps en primer lugar, escritorios segundo).
- Se han corregido varios errores de accesibilidad mejora de la compatibilidad de lector de pantalla.
- Actualiza nuestras herramientas de compilación.
- Varias correcciones de errores.

## <a name="updates-for-version-107"></a>Actualizaciones de versión 1.0.7
*Fecha de publicación: 1/24/2019*

- Ahora se admite el uso sin conexión en redes internas.
- Representación mejorada en los exploradores que no sea Microsoft Edge.
- Límite implementado para reintento de fuente recuperación intenta evitar la denegación de servicio.
- Errores de accesibilidad fijo, permitiendo a los usuarios con discapacidades visuales usar al cliente web.
- Mejorado los mensajes de error que se muestra al usuario para los errores de fuente.
- Se ha agregado Ctrl + Alt + final (Windows) y fn + control + opción + accesos directos de eliminación (Mac) para invocar Ctrl + Alt + Supr en el equipo remoto.
- Telemetría mejorada para eventos de bloqueo.
- Mejorado nuestra canalización de compilación y las herramientas de compilación.
- Varias correcciones de errores.

## <a name="updates-for-version-101"></a>Actualizaciones para la versión 1.0.1
*Fecha de publicación: 10/29/2018*

- Se agregó una opción a **captura información de soporte técnico** en la página About para diagnosticar problemas.
- Ahora se admite el modo inPrivate.
- Compatibilidad mejorada para teclados no ingleses.
- Se ha corregido un problema donde se mostraba incorrectamente información sobre herramientas con caracteres no válidos.
- Problema de representación de gráficos fijo que Chrome usuarios afectados.
- Actualiza la redirección de zona horaria con compatibilidad total con el horario de verano.
- Ha mejorado el mensaje de error para errores de memoria insuficiente.
- Varias correcciones de errores.

## <a name="updates-for-version-100"></a>Actualizaciones de versión 1.0.0
*Fecha de publicación: 07/16/2018*

- Cliente web de escritorio remoto está disponible con carácter general.
- Los administradores pueden desactivar globalmente telemetría para el cliente web.
- Varias correcciones de errores.

## <a name="updates-for-version-090"></a>Actualizaciones de versión 0.9.0
*Fecha de publicación: 07/05/2018*

- Inicio de sesión nueva experiencia en el cliente web.
- Ya no se le solicite las credenciales al iniciar una conexión de escritorio o aplicación (inicio de sesión único).
- Mover al cliente web a una nueva dirección URL: <https://server_FQDN/RDWeb/webclient/index.html>
- Redirección de zona horaria se ha agregado.
- Varias correcciones de errores.

## <a name="updates-for-version-081"></a>Actualizaciones de versión 0.8.1
*Fecha de publicación: 05/17/2018*

- Actualizaciones para tratar la corrección de oracle cifrado CredSSP que se describe en CVE-2018-0886.
- Se ha corregido los errores de conexión para algunos idiomas cuando está habilitada la impresión.
- Mensaje de error mejorado cuando una puerta de enlace no forma parte de la implementación.
- **Ayudar a** y **comentarios** se agregaron opciones.

## <a name="updates-for-version-080"></a>Actualizaciones de versión 0.8.0
*Fecha de publicación: 03/28/2018*

- Versión preliminar pública inicial del cliente web.
- Copiar y pegar texto mediante el Portapapeles con **CTRL + C** y **CTRL+V**.
- Imprimir en un archivo PDF.
- En 18 idiomas.
 
