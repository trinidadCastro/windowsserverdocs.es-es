---
title: 'Notas de la versión: Problemas importantes en Windows Server 2016'
description: Se resumen problemas críticos que requieren soluciones para evitar bloqueos, faltas de respuesta, errores de instalación o pérdida de datos.
ms.date: 11/13/2018
ms.topic: article
ms.assetid: 134aab85-664f-4d44-87ef-9e5fd389071f
author: jaimeo
ms.author: jaimeo
ms.openlocfilehash: b7e86b0841023548b1df1937bdf0820d59e12292
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87990503"
---
# <a name="release-notes-important-issues-in-windows-server-2016"></a>Notas de la versión: Problemas importantes en Windows Server 2016

>Se aplica a: Windows Server 2016

En estas notas se resumen los problemas más críticos del sistema operativo Windows Server 2016 junto con soluciones alternativas y formas de evitar los problemas, si se conocen. Para más información sobre los cambios por diseño, nuevas características y soluciones en esta versión, vea [What's New in the Windows Server 2016](whats-new-in-windows-server-2016.md) (Novedades en Windows Server 2016) y anuncios de equipos de características específicas. A menos que se especifique lo contrario, cada problema notificado se aplica a todas las ediciones y opciones de instalación de Windows Server 2016.

Este documento se actualiza continuamente. Habida cuenta de que se detectan problemas críticos que necesitan una solución alternativa, se agregan, al igual que las nuevas soluciones alternativas y correcciones, a medida que están disponibles.

## <a name="express-updates-available-starting-in-november-2018-new"></a>Actualizaciones rápidas disponibles a partir de noviembre de 2018 (NUEVO)

A partir de la actualización del martes de actualizaciones de noviembre de 2018, Windows volverá a publicar las [Actualizaciones rápidas](express-updates.md) para Windows Server 2016. Si usas WSUS y Configuration Manager, volverás a ver dos paquetes para la actualización de Windows Server 2016: una actualización completa y una rápida. Si quieres usar la actualización rápida para tus entornos de servidor, deberás confirmar que el servidor ha hecho una actualización completa desde noviembre de 2017 (KB 4048953) para garantizar que la actualización rápida se instale correctamente. Si intentas realizar una actualización rápida en un servidor que no se ha actualizado desde la actualización 11B de 2017 (KB 4048953), recibirás varios errores que consumen ancho de banda y recursos de CPU en un bucle infinito. Si te encuentras con este escenario, deja de intentar la actualización rápida y, en su lugar, intenta una actualización completa reciente para detener el bucle de errores.

## <a name="server-core-installation-option"></a>Opción de instalación Server Core

[comment]: # (ID: 370, Remitente: amason; estado: firmado)

Cuando se instala Windows Server 2016 mediante la opción de instalación Server Core, el administrador de trabajos de impresión se instala y se inicia de forma predeterminada, incluso cuando no está instalado el rol de servidor de impresión.

Para evitar esta situación, tras el primer inicio, defina el administrador de trabajos de impresión como deshabilitado.

## <a name="containers"></a>Contenedores

[comment]: # (ID: 371; Remitente: taylorb; estado: firmado)
- Antes de que uses los contenedores, instala la [Actualización de la pila de mantenimiento de Windows 10, versión 1607: 23 de agosto de 2016](https://support.microsoft.com/kb/3176936) o cualquier actualización posterior disponible. De lo contrario, puede producirse una serie de problemas, incluidos los errores de compilación, inicio o ejecución de contenedores, y errores similares a Error de CreateProcess en Win32: El servidor RPC no está disponible.

[comment]: # (ID: 373; Remitente: plang; estado: firmado)
- El proveedor de NanoServerPackage OneGet no funciona en los contenedores de Windows. Para solucionar este problema, utilice Find-NanoServerPackage y Save-NanoServerPackage en un equipo diferente (no en un contenedor) para descargar el paquete necesario. Después copie los paquetes en el contenedor e instálelos.

## <a name="device-guard"></a>Device Guard

[comment]: # (ID: 369; Remitente: nirb; estado: firmado)
Si usas la protección basada en virtualización de la integridad del código o máquinas virtuales blindadas (que utilizan protección basada en virtualización de la integridad del código), debes tener en cuenta que estas tecnologías podrían ser incompatibles con algunos dispositivos y aplicaciones. Te recomendamos que pruebes estas configuraciones en el laboratorio antes de habilitar las características en sistemas de producción. Si no lo haces, podrían producirse pérdidas de datos inesperadas o errores de detención.

## <a name="microsoft-exchange"></a>Microsoft Exchange

[comment]: # (ID: 375; Remitente: wgries; estado: firmado)
Si intentas ejecutar Microsoft Exchange 2016 CU3 en Windows Server 2016, se producirán errores en el proceso del host IIS W3WP.exe. Por el momento no hay ninguna solución alternativa al respecto. Deberías posponer la implementación de Exchange 2016 CU3 en Windows Server 2016 hasta que haya disponible una corrección compatible.

## <a name="remote-server-administration-tools-rsat"></a>Herramientas de administración remota del servidor (RSAT)

[comment]: # (ID: 374; Remitente: ryanpu; estado: firmado)
Si estás ejecutando una versión de Windows 10 anterior a la Actualización de aniversario y estás utilizando Hyper-V y máquinas virtuales con un módulo de plataforma segura virtual habilitado (incluidas las máquinas virtuales blindadas) y, a continuación, instalas la versión de RSAT proporcionada para Windows Server 2016, se producirá un error al intentar iniciar dichas máquinas virtuales.

Para evitar esto, actualice el equipo cliente a la Actualización de aniversario de Windows 10 (o posterior) antes de instalar RSAT. Si ya se ha producido, desinstale RSAT, actualize el cliente a la Actualización de aniversario de Windows 10 y luego reinstale RSAT.

## <a name="shielded-virtual-machines"></a>Máquinas virtuales blindadas

[comment]: # (ID: 369; Remitente: nirb; estado: firmado)
- Asegúrate de que has instalado todas las actualizaciones disponibles antes de implementar las máquinas virtuales blindadas en la producción.

- Si usas la protección basada en virtualización de la integridad del código o máquinas virtuales blindadas (que utilizan protección basada en virtualización de la integridad del código), debes tener en cuenta que estas tecnologías podrían ser incompatibles con algunos dispositivos y aplicaciones. Te recomendamos que pruebes estas configuraciones en el laboratorio antes de habilitar las características en sistemas de producción. Si no lo haces, podrían producirse pérdidas de datos inesperadas o errores de detención.

## <a name="start-menu"></a>Menú Inicio

[comment]: # (ID: 372; Remitente: samli; estado: firmado)
Este problema afecta a Windows Server 2016 instalado con la opción Servidor con Experiencia de escritorio.

Si instalas las aplicaciones que agregan elementos de acceso directo dentro de una carpeta en el menú **Inicio**, los accesos directos no funcionarán hasta que cierres la sesión y vuelvas a iniciarla.

Vuelva al centro principal de [Windows Server 2016](../index.yml).

## <a name="storport-performance"></a>Rendimiento de Storport

Algunos sistemas pueden presentar un rendimiento de almacenamiento reducido al ejecutar una nueva instalación de Windows Server 2016 frente a Windows Server 2012 R2.  Durante el desarrollo de Windows Server 2016 se realizaron una serie de cambios para mejorar la seguridad y confiabilidad de la plataforma. Algunos de esos cambios, como la habilitación de Windows Defender de manera predeterminada, dan como resultado rutas de acceso de E/S más largas, que pueden reducir el rendimiento de E/S en determinadas cargas de trabajo y determinados patrones. Microsoft no recomienda deshabilitar Windows Defender, ya que es una importante capa de protección para sus sistemas. 

## <a name="copyright"></a>Copyright

Este documento se proporciona tal cual. La información y las vistas expresadas en este documento, incluidas las direcciones URL y otras referencias a sitios web de Internet, pueden cambiar sin previo aviso.

Este documento no le proporciona derechos legales sobre ninguna propiedad intelectual en ningún producto de Microsoft. Puede copiar y usar este documento para su referencia interna.

&copy; 2016 Microsoft Corporation. Todos los derechos reservados.

Microsoft, Active Directory, Hyper-V, Windows y Windows Server son marcas comerciales registradas o marcas comerciales de Microsoft Corporation en los Estados Unidos u otros países.

El producto contiene software de filtros gráficos que está basado parcialmente en el trabajo de Independent JPEG Group.

1.0