---
title: Novedades del cliente de Microsoft Store
description: Obtén información sobre los cambios recientes en el cliente de Escritorio remoto para Microsoft Store
ms.topic: article
author: heidilohr
manager: lizross
ms.author: helohr
ms.date: 10/14/2020
ms.localizationpriority: medium
ms.openlocfilehash: 63f3ccb3b105bf59033214d426650dd727509095
ms.sourcegitcommit: 45099dfe3682df1e2bc0bd5998594a79cfff16fe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2020
ms.locfileid: "92079834"
---
# <a name="whats-new-in-the-microsoft-store-client"></a>Novedades del cliente de Microsoft Store

El [cliente de Microsoft Store](windows.md) se actualiza periódicamente, con lo que se agregan nuevas características y se corrigen problemas. Aquí puedes encontrar las actualizaciones más recientes.

## <a name="updates-for-version-1021534"></a>Actualizaciones para la versión 10.2.1534

*Fecha de publicación: 26/08/2020*

- Se reescribió el cliente para usar el mismo motor principal de RDP subyacente que los clientes de iOS, macOS y Android.
- Se agregó compatibilidad con la versión integrada en Azure Resource Manager de Windows Virtual Desktop.
- Se agregó compatibilidad para x64 y ARM64.
- Se actualizó el diseño del panel lateral a pantalla completa.
- Se agregó compatibilidad para los modos claro y oscuro.
- Se agregó una funcionalidad para suscribirse y conectarse a implementaciones de la nube soberana.
- Se agregó una funcionalidad para habilitar la copia de seguridad y la restauración de áreas de trabajo (marcadores) en el lanzamiento a producción (RTM).
- Se actualizó la funcionalidad para usar los tokens de Azure Active Directory (Azure AD) existentes durante el proceso de suscripción a fin de reducir el número de veces que los usuarios deben iniciar sesión.
- La suscripción actualizada ahora puede detectar si usa Windows Virtual Desktop o la versión clásica de Windows Virtual Desktop.
- Se corrigió un problema de copia de archivos a equipos remotos.
- Se corrigieron problemas de accesibilidad comunes de los botones.
- Se permite un máximo de 20 credenciales por aplicación.

## <a name="updates-for-version-1011215"></a>Actualizaciones para la versión 10.1.1215

*Fecha de publicación: 20/04/2020*

- Se actualizó la cadena de agente de usuario de Windows Virtual Desktop.

## <a name="updates-for-version-1011195"></a>Actualizaciones para la versión 10.1.1195

*Fecha de publicación: 06/03/2020*

- Ahora, el audio de la sesión continúa reproduciéndose aunque la aplicación esté minimizada o en segundo plano.
- Se corrigió un problema que hacía que las teclas de alternancia (Bloq Mayús, Bloq Num, etc.) no estuvieran sincronizadas entre los equipos locales y remotos.
- Mejoras en el rendimiento en los dispositivos de 64 bits.
- Se corrigió un bloqueo que se producía al suspender la aplicación.

## <a name="updates-for-version-1011107"></a>Actualizaciones de la versión 10.1.1107

*Fecha de publicación: 09/04/2019*

- Ahora puedes copiar archivos entre equipos locales y remotos.
- Ahora puedes usar tu dirección de correo electrónico para acceder a los recursos remotos (si el administrador ha habilitado esta opción).
- Ahora puedes cambiar las asignaciones de cuentas de usuario de las fuentes de recursos remotos.
- La aplicación ahora muestra el icono adecuado para los archivos RDP asignados a esta aplicación en el Explorador de archivos, en lugar de un icono predeterminado en blanco.

## <a name="updates-for-version-1011098"></a>Actualizaciones de la versión 10.1.1098

*Fecha de publicación: 15/03/2019*

- Ahora puedes establecer un nombre para mostrar para las cuentas de usuario, de modo que puedes guardar el mismo nombre de usuario con contraseñas diferentes.
- Ahora es posible seleccionar una cuenta de usuario existente al agregar recursos remotos.
- Se ha corregido un problema que provocaba que el cliente no finalizase correctamente.
- El cliente ahora controla correctamente la suspensión cuando se abren ventanas secundarias.
- Corrección de errores adicionales.

## <a name="updates-for-version-1011088"></a>Actualizaciones de la versión 10.1.1088

*Fecha de publicación: 06/11/2018*

- Ahora el nombre para mostrar de la conexión es más reconocible.
- Se ha corregido un bloqueo al cerrar la ventana del cliente mientras había aún una conexión activa.
- Se ha corregido un bloqueo al volver a conectarse después de minimizar el cliente.
- Se permite arrastrar los escritorios a cualquier parte de un grupo.
- Compruebe que al iniciar una conexión desde la lista de accesos directos se abre una ventana independiente cuando es necesario.
- Corrección de errores adicionales.

## <a name="updates-for-version-1011060"></a>Actualizaciones de la versión 10.1.1060

*Fecha de publicación: 14/09/2018*

- Se ha resuelto un problema por el que, al hacer doble clic en una conexión al escritorio, provocaba que se iniciaran dos sesiones.
- Se ha corregido un bloqueo al cambiar entre escritorios virtuales localmente.
- Mover una sesión a otro monitor ahora también actualiza el factor de escala de la sesión.
- Se permiten teclas adicionales del sistema, como AltGr.
- Corrección de errores adicionales.

## <a name="updates-for-version-1011046"></a>Actualizaciones de la versión 10.1.1046

*Fecha de publicación: 20/06/2018*

- Correcciones de errores.

## <a name="updates-for-version-1011042"></a>Actualizaciones de la versión 10.1.1042

*Fecha de publicación: 02/04/2018*

- Actualizaciones para abordar la corrección de oráculo de cifrado de CredSSP que se describe en CVE-2018-0886.
- Corrección de errores adicionales.
