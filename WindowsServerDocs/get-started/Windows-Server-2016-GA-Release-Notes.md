---
title: 'Notas de la versión: Problemas importantes en Windows Server 2016'
description: Se resumen problemas críticos que requieren soluciones para evitar bloqueos, faltas de respuesta, errores de instalación o pérdida de datos.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 11/13/2018
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 134aab85-664f-4d44-87ef-9e5fd389071f
author: jaimeo
ms.author: jaimeo
ms.openlocfilehash: dec1ec184a147ef4fae64e9cc4384c0b6e4510b6
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749538"
---
# <a name="release-notes-important-issues-in-windows-server-2016"></a>Notas de la versión: Problemas importantes en Windows Server 2016

>Se aplica a: Windows Server 2016

Estas notas resumen los problemas más críticos en el sistema operativo de Windows Server 2016, como las maneras de evitar o solucionar los problemas, si se conoce. Para más información sobre los cambios por diseño, nuevas características y soluciones en esta versión, vea [What's New in the Windows Server 2016](whats-new-in-windows-server-2016.md) (Novedades en Windows Server 2016) y anuncios de equipos de características específicas. A menos que se especifique lo contrario, cada problema notificado se aplica a todas las ediciones y opciones de instalación de Windows Server 2016.

Este documento se actualiza continuamente. Habida cuenta de que se detectan problemas críticos que necesitan una solución alternativa, se agregan, al igual que las nuevas soluciones alternativas y correcciones, a medida que están disponibles.

## <a name="express-updates-available-starting-in-november-2018-new"></a>Express actualizaciones disponibles a partir de noviembre de 2018 (nuevo)

A partir de la de noviembre de 2018 actualización "Actualizar martes", Windows nuevo publicará [las actualizaciones rápidas](express-updates.md) para Windows Server 2016. Si usa WSUS y System Center Configuration Manager (SCCM) una vez más, verá dos paquetes de la actualización de Windows Server 2016: una actualización completa y una actualización de Express. Si desea usar Express para los entornos de servidor, deberá confirmar que el servidor ha realizado una actualización completa desde noviembre de 2017 (KB # 4048953) para garantizar que la actualización de Express se instala correctamente. Si se intenta una actualización de Express en un servidor que no se ha actualizado desde la actualización de 11B 2017 (KB # 4048953), verá errores repetidos que consumen ancho de banda y los recursos de CPU en un bucle infinito. Si se produce en este escenario, detenga la inserción de la actualización de Express y en su lugar, inserte una actualización reciente completa para detener el bucle de error.

## <a name="server-core-installation-option"></a>Opción de instalación Server Core

[comment]: # (ID: 370; Remitente: amason; estado: firmado)

Cuando se instala Windows Server 2016 mediante la opción de instalación Server Core, el administrador de trabajos de impresión se instala y se inicia de forma predeterminada, incluso cuando no está instalado el rol de servidor de impresión.

Para evitar esta situación, tras el primer inicio, defina el administrador de trabajos de impresión como deshabilitado.

## <a name="containers"></a>Contenedores

[comment]: # (ID: 371; Remitente: taylorb; estado: firmado)
- Antes de utilizar contenedores, instale [actualización de la pila de mantenimiento para Windows 10 versión 1607: 23 de agosto de 2016](https://support.microsoft.com/en-us/kb/3176936) o las actualizaciones posteriores que están disponibles. En caso contrario, puede producirse una serie de problemas, incluidos los errores de compilación, inicio, o ejecutar contenedores y errores similares a "Error de CreateProcess en Win32: El servidor RPC no está disponible."

[comment]: # (ID: 373; Remitente: plang; estado: firmado)
- El proveedor de NanoServerPackage OneGet no funciona en los contenedores de Windows. Para solucionar este problema, utilice Find-NanoServerPackage y Save-NanoServerPackage en un equipo diferente (no en un contenedor) para descargar el paquete necesario. Después copie los paquetes en el contenedor e instálelos.

## <a name="device-guard"></a>Device Guard

[comment]: # (ID: 369; Remitente: nirb; estado: firmado)
Si usas la protección basada en virtualización de la integridad del código o de máquinas virtuales blindadas (que utilizan protección basada en virtualización de la integridad del código), debes tener en cuenta que estas tecnologías podrían ser incompatibles con algunos dispositivos y aplicaciones. Te recomendamos que pruebes estas configuraciones en el laboratorio antes de habilitar las características en sistemas de producción. Si no lo haces, podrían producirse pérdidas de datos inesperadas o errores de detención.

## <a name="microsoft-exchange"></a>Microsoft Exchange

[comment]: # (ID: 375; Remitente: wgries; estado: firmado)
Si intentas ejecutar Microsoft Exchange 2016 CU3 en Windows Server 2016, se producirán errores en el proceso del host IIS W3WP.exe. Por el momento no hay ninguna solución alternativa al respecto. Deberías posponer la implementación de Exchange 2016 CU3 en Windows Server 2016 hasta que esté disponible una corrección compatible.

## <a name="remote-server-administration-tools-rsat"></a>Herramientas de administración remota del servidor (RSAT)

[comment]: # (ID: 374; Remitente: ryanpu; estado: firmado)
Si está ejecutando una versión de Windows 10 anterior a la actualización de aniversario y está utilizando Hyper-V y máquinas virtuales con un habilitado virtual módulo de plataforma segura (incluidas las máquinas virtuales blindadas) y, a continuación, instale la versión de RSAT proporcionada para Windows Server 2016, intentos de iniciar las máquinas virtuales se producirá un error.

Para evitar esto, actualice el equipo cliente a la Actualización de aniversario de Windows 10 (o posterior) antes de instalar RSAT. Si ya se ha producido, desinstale RSAT, actualize el cliente a la Actualización de aniversario de Windows 10 y luego reinstale RSAT.

## <a name="shielded-virtual-machines"></a>Máquinas virtuales blindadas

[comment]: # (ID: 369; Remitente: nirb; estado: firmado)  
- Asegúrate de que has instalado todas las actualizaciones disponibles antes de implementar las máquinas virtuales blindadas en la producción.

- Si usas la protección basada en virtualización de la integridad del código o de máquinas virtuales blindadas (que utilizan protección basada en virtualización de la integridad del código), debes tener en cuenta que estas tecnologías podrían ser incompatibles con algunos dispositivos y aplicaciones. Te recomendamos que pruebes estas configuraciones en el laboratorio antes de habilitar las características en sistemas de producción. Si no lo haces, podrían producirse pérdidas de datos inesperadas o errores de detención.

## <a name="start-menu"></a>Menú Inicio

[comment]: # (ID: 372; Remitente: samli; estado: firmado)
Este problema afecta a Windows Server 2016 instalado con la opción Servidor con Experiencia de escritorio.

Si instala las aplicaciones que agregan elementos contextual dentro de una carpeta en el **iniciar** menú, los accesos directos no funcionarán hasta que se cierra la sesión y se vuelva a intentarlo.

Vuelva al centro principal de [Windows Server 2016](Windows-Server-2016.md).

## <a name="storport-performance"></a>Rendimiento de Storport

Algunos sistemas pueden presentar un rendimiento de almacenamiento reducido al ejecutar una nueva instalación de Windows Server 2016 frente a Windows Server 2012 R2.  Durante el desarrollo de Windows Server 2016 se realizaron una serie de cambios para mejorar la seguridad y confiabilidad de la plataforma. Algunos de esos cambios, como la habilitación de Windows Defender de manera predeterminada, dan como resultado rutas de acceso de E/S más largas, que pueden reducir el rendimiento de E/S en determinadas cargas de trabajo y determinados patrones. Microsoft no recomienda deshabilitar Windows Defender, ya que es una importante capa de protección para tus sistemas.  

## <a name="copyright"></a>Copyright

Este documento se proporciona “tal cual”. La información y las vistas expresadas en este documento, incluidas las direcciones URL y otras referencias a sitios web de Internet, pueden cambiar sin previo aviso.  

Este documento no le proporciona derechos legales sobre ninguna propiedad intelectual en ningún producto de Microsoft. Puedes copiarlo y usarlo como referencia interna.  

&copy;2016 Microsoft Corporation. Todos los derechos reservados.  

Microsoft, Active Directory, Hyper-V, Windows y Windows Server son marcas comerciales registradas o marcas comerciales de Microsoft Corporation en los Estados Unidos u otros países.  

El producto contiene software de filtros gráficos que está basado parcialmente en el trabajo de Independent JPEG Group.  

1.0
