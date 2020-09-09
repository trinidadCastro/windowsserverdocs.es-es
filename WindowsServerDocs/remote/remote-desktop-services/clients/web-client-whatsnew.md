---
title: Novedades del cliente web
description: Obtén información sobre los cambios recientes en el cliente web de Escritorio remoto.
ms.topic: article
author: heidilohr
manager: lizross
ms.author: helohr
ms.date: 09/02/2020
ms.localizationpriority: medium
ms.openlocfilehash: 18142988108e1eafe59ca7fd83a29dd4dfb87720
ms.sourcegitcommit: 664ed9bb0bbac2c9c0727fc2416d8c437f2d5cbe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2020
ms.locfileid: "89472035"
---
# <a name="whats-new-in-the-web-client"></a>Novedades del cliente web

El [cliente web de Escritorio remoto](remote-desktop-web-client.md) se actualiza periódicamente, con lo que se agregan nuevas características y se corrigen problemas. Aquí puedes encontrar las actualizaciones más recientes.

> [!NOTE]
> Cambiamos el sistema de control de versiones del cliente web. A partir de la versión 1.0.18.0, todas las versiones del cliente web incluirán números (con el formato "W.X.Y.Z"). Los números de versión del cliente web de Escritorio remoto siempre terminarán en 0 (por ejemplo, W.X.Y.0). Cada versión del cliente web de Windows Virtual Desktop cambiará el último dígito hasta la próxima versión del cliente web de Escritorio remoto (por ejemplo, 1.0.18.1).

## <a name="updates-for-10220"></a>Actualizaciones de la versión 1.0.22.0
*Fecha de publicación: 2/9/2020*

- Los usuarios ahora pueden mover el menú minimizado.
- Se ha mejorado la compatibilidad con monitores 4K y ultraanchos, y se ha corregido un problema en que la copia de grandes cantidades de datos provocaba el bloqueo de las sesiones.
- Se ha mejorado la compatibilidad para usar un editor de métodos de entrada (IME) en la sesión remota. Para obtener más información sobre el uso de un editor de métodos de entrada con el cliente web, consulte [Conexión a Windows Virtual Desktop con el cliente web](/azure-docs/articles/virtual-desktop/connect-web.md).
- Se ha cambiado la interfaz de usuario de la página **Todos los recursos**.
- Se han corregido varios errores de secuencia de conexión en los que el cliente web devolvía un *error de protocolo general*.
- Se han corregido problemas de entrada de teclado en los que secuencias de teclas específicas no se controlaban correctamente.
- Mejoras de accesibilidad.

## <a name="updates-for-version-10210"></a>Actualizaciones de la versión 1.0.21.0
*Fecha de publicación: 15/11/2019*

- Se ha agregado compatibilidad para usar un editor de métodos de entrada (IME) en la sesión remota para escribir caracteres complejos.
- Se ha corregido una regresión en la que los usuarios no podían copiar ni pegar elementos en la sesión remota de dispositivos macOS.
- Se ha corregido una regresión en la que la clave de Windows local se envió a la sesión remota en Firefox.
- Se ha agregado un vínculo a un cambio de contraseña de RDWeb cuando lo habilite el administrador.

## <a name="updates-for-version-10200"></a>Actualizaciones de la versión 1.0.20.0
*Fecha de publicación: 18/10/2019*

- Se agregó compatibilidad con conexiones a hosts de Windows 7 y Windows Server 2008 R2.
- Se corrigió un problema por el que algunos iconos de aplicaciones se mostraban como mosaicos transparentes.
- Se corrigieron problemas de conexión para el explorador Internet Explorer en Windows 7.
- Se corrigieron desconexiones inesperadas que se producían cuando se cambiaba el tamaño del explorador.
- Mejoras de accesibilidad.
- Se actualizaron bibliotecas de terceros.

## <a name="updates-for-version-10180"></a>Actualizaciones de la versión 1.0.18.0
*Fecha de publicación: 14/05/2019*

- Se agregó la configuración del método de inicio del recurso en la pestaña Configuración, lo que permite que los usuarios abran recursos en el explorador o descarguen un archivo .rdp para administrarlo con otro cliente. Esta opción la puede configurar el administrador. Puedes encontrar detalles sobre las configuraciones del administrador para esta característica en la [documentación de instalación del cliente web](remote-desktop-web-client-admin.md).
- Se corrigieron problemas de representación del color, lo que permite usar colores más vívidos en la sesión remota.
- Se revisaron los mensajes de error relacionados con errores de fuente de los recursos remotos.
- Se agregó compatibilidad con más combinaciones de teclas de Office, como el pegado especial (Ctrl + Alt + V).
- Se agregó una combinación de teclas para que los usuarios invoquen la tecla Windows en la sesión remota (Alt + F3).
- Se actualizó el mensaje de error para los usuarios que intentan autenticarse con una contraseña expirada.
- Se actualizó la interfaz de usuario de fuente en la página Todos los recursos.
- Se resolvieron los cuadros de diálogo superpuestos que se producían durante la reconexión de la sesión.
- Se corrigió el cambio de tamaño de los iconos de los recursos remotos en la barra de tareas de recursos.

## <a name="updates-for-version-1011"></a>Actualizaciones de la versión 1.0.11
*Fecha de publicación: 22/02/2019*

- Se habilitó una conexión con un agente de Escritorio remoto sin una puerta de enlace de Escritorio remoto en Windows Server 2019.
- Las fuentes se ordenaron alfabéticamente (es decir, primero las aplicaciones remotas y luego los escritorios).
- Se corrigieron varios problemas de accesibilidad, con lo que se mejora la compatibilidad del lector de pantalla.
- Se actualizaron las herramientas de compilación.
- Se corrigieron varios errores.

## <a name="updates-for-version-107"></a>Actualizaciones de la versión 1.0.7
*Fecha de publicación: 24/01/2019*

- Ahora se admite el uso sin conexión en las redes internas.
- Se mejoró la representación en exploradores distintos de Microsoft Edge.
- Se implementó un límite para los reintentos de recuperación de fuentes para evitar la denegación de servicio.
- Se corrigieron errores de accesibilidad, lo que permite que los usuarios con discapacidades visuales usen el cliente web.
- Se mejoraron los mensajes de error que se muestran al usuario en caso de errores de fuentes.
- Se agregaron las combinaciones de teclas Ctrl + Alt + Fin (Windows) y fn + control + opción + suprimir (Mac) para invocar Ctrl + Alt + Supr en la máquina remota.
- Se mejoró la telemetría para los eventos de bloqueo.
- Se mejoró la canalización de compilación y las herramientas de compilación.
- Se corrigieron varios errores.

## <a name="updates-for-version-101"></a>Actualizaciones de la versión 1.0.1
*Fecha de publicación: 29/10/2018*

- Se agregó una opción para **capturar información de soporte técnico** en la página Acerca de para diagnosticar problemas.
- Ahora se admite el modo InPrivate.
- Se mejoró la compatibilidad con teclados en idiomas distintos del inglés.
- Se corrigió un problema en el que la información sobre herramientas con caracteres que no pertenecen al inglés no se mostraba correctamente.
- Se corrigió un problema de representación de gráficos que afectaba a los usuarios de Chrome.
- Se actualizó el redireccionamiento de las zonas horarias con compatibilidad total con el horario de verano.
- Se mejoró el mensaje de error en caso de memoria insuficiente.
- Se corrigieron varios errores.

## <a name="updates-for-version-100"></a>Actualizaciones de la versión 1.0.0
*Fecha de publicación: 16/07/2018*

- El cliente web de Escritorio remoto ya está disponible con carácter general.
- Los administradores pueden desactivar de manera global la telemetría para el cliente web.
- Se corrigieron varios errores.

## <a name="updates-for-version-090"></a>Actualizaciones de la versión 0.9.0
*Fecha de publicación: 05/07/2018*

- Nueva experiencia de inicio de sesión dentro del cliente web.
- Ya no se solicitan credenciales al iniciar una conexión de escritorio o aplicación (inicio de sesión único).
- El cliente web se movió a una dirección URL nueva: <https://server_FQDN/RDWeb/webclient/index.html>
- Se agregó el redireccionamiento de las zonas horarias.
- Se corrigieron varios errores.

## <a name="updates-for-version-081"></a>Actualizaciones de la versión 0.8.1
*Fecha de publicación: 17/05/2018*

- Actualizaciones para abordar la corrección de oráculo de cifrado de CredSSP que se describe en CVE-2018-0886.
- Se corrigieron errores de conexión en algunos idiomas cuando está habilitada la impresión.
- Se mejoró el mensaje de error para cuando una puerta de enlace no forma parte de la implementación.
- Se agregaron las opciones **Ayuda** y **Comentarios**.

## <a name="updates-for-version-080"></a>Actualizaciones de la versión 0.8.0
*Fecha de publicación: 28/03/2018*

- Versión preliminar pública inicial del cliente web.
- Copia y pega texto mediante el Portapapeles con **CTRL + C** y **CTRL + V**.
- Imprime en un archivo PDF.
- Localizado en 18 idiomas.
