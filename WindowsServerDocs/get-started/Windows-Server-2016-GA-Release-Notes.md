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
ms.localizationpriority: medium
ms.openlocfilehash: 5a25a9152298a38ad77a377a87b71917ee586947
ms.sourcegitcommit: 23e0a68e21985d709e029e7771d3c52d6815bcb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2018
ms.locfileid: "6507718"
---
# Notas de la versión: Problemas importantes en Windows Server 2016

>Se aplica a: Windows Server 2016

En estas notas se resumen los problemas más críticos del sistema operativo Windows Server&reg; 2016 junto con soluciones alternativas y formas de evitar los problemas, si se conocen. Para más información sobre los cambios por diseño, nuevas características y soluciones en esta versión, vea [What's New in the Windows Server 2016](what-s-new-in-windows-server-2016.md) (Novedades en Windows Server 2016) y anuncios de equipos de características específicas. A menos que se especifique lo contrario, cada problema notificado se aplica a todas las ediciones y opciones de instalación de Windows Server 2016.  

Este documento se actualiza continuamente. Habida cuenta de que se detectan problemas críticos que necesitan una solución alternativa, se agregan, al igual que las nuevas soluciones alternativas y correcciones, a medida que están disponibles.  

## Express las actualizaciones disponibles a partir de noviembre de 2018 (nuevo)

A partir de la noviembre de 2018 "Martes de actualización de" actualización, Windows se vuelve a publicar [Express actualizaciones](express-updates.md) para Windows Server 2016. Si usas WSUS y System Center Configuration Manager (SCCM) una vez más verás dos paquetes de la actualización de Windows Server 2016: una actualización completa y una actualización de Express. Si quieres usar "Express" para los entornos de servidor, debes confirmar que el servidor ha realizado una actualización completa desde noviembre de 2017 (KB # 4048953) para garantizar que la actualización de Express se instala correctamente. Si se trata de una actualización de Express en un servidor que no se ha actualizado desde la actualización de 11 b 2017 (KB # 4048953), podrás ver errores repetidos que consumen recursos de CPU en un bucle infinito y ancho de banda. Si obtienes en este escenario, detener anima a la actualización "Express" y de inserción en su lugar una actualización completa recientes para detener el bucle de error.  

## Opción de instalación Server Core
[comment]: # (ID: 370; Remitente: amason; estado: firmado)  
Cuando se instala Windows Server 2016 mediante la opción de instalación Server Core, el administrador de trabajos de impresión se instala y se inicia de forma predeterminada, incluso cuando no está instalado el rol de servidor de impresión.

Para evitar esta situación, tras el primer inicio, defina el administrador de trabajos de impresión como deshabilitado.


## Contenedores  

[comment]: # (ID: 371; Remitente: taylorb; estado: firmado)  
- Antes de utilizar contenedores, instala la [actualización de la pila de mantenimiento para Windows 10 versión 1607: 23 de agosto de 2016](https://support.microsoft.com/en-us/kb/3176936) o cualquier actualización posterior que esté disponible. De lo contrario, puede producirse una serie de problemas, incluidos los errores de compilación, inicio o ejecución de contenedores, y errores similares a "Error de CreateProcess en Win32: El servidor RPC no está disponible."

[comment]: # (ID: 373; Remitente: plang; estado: firmado)  
- El proveedor de NanoServerPackage OneGet no funciona en los contenedores de Windows. Para solucionar este problema, utilice Find-NanoServerPackage y Save-NanoServerPackage en un equipo diferente (no en un contenedor) para descargar el paquete necesario. A continuación, copia los paquetes en el contenedor e instálalos.

## Device Guard
[comment]: # (ID: 369; Remitente: nirb; estado: firmado)
Si usas la protección basada en virtualización de la integridad del código o de máquinas virtuales blindadas (que utilizan protección basada en virtualización de la integridad del código), debes tener en cuenta que estas tecnologías podrían ser incompatibles con algunos dispositivos y aplicaciones. Te recomendamos que pruebes estas configuraciones en el laboratorio antes de habilitar las características en sistemas de producción. Si no lo haces, podrían producirse pérdidas de datos inesperadas o errores de detención.

## Microsoft Exchange
[comment]: # (ID: 375; Remitente: wgries; estado: firmado)
Si intentas ejecutar Microsoft Exchange 2016 CU3 en Windows Server2016, se producirán errores en el proceso del host IIS W3WP.exe. Por el momento no hay ninguna solución alternativa al respecto. Deberías posponer la implementación de Exchange 2016 CU3 en Windows Server 2016 hasta que esté disponible una corrección compatible.

## Herramientas de administración remota del servidor (RSAT)
[comment]: # (ID: 374; Remitente: ryanpu; estado: firmado)
Si ejecutas una versión de Windows10 anterior a la Actualización de aniversario y usas Hyper-V y máquinas virtuales con un módulo de plataforma segura virtual habilitado (incluidas las máquinas virtuales blindadas) y, a continuación, instalas la versión de RSAT proporcionada para Windows Server 2016, se producirá un error al intentar iniciar dichas máquinas virtuales.

Para evitar esto, actualice el equipo cliente a la Actualización de aniversario de Windows 10 (o posterior) antes de instalar RSAT. Si ya se ha producido, desinstala RSAT, actualiza el cliente a la Actualización de aniversario de Windows10 y luego reinstala RSAT.


## Máquinas virtuales blindadas
[comment]: # (ID: 369; Remitente: nirb; estado: firmado)  
- Asegúrate de que has instalado todas las actualizaciones disponibles antes de implementar máquinas virtuales blindadas en la producción.

- Si usas la protección basada en virtualización de la integridad del código o de máquinas virtuales blindadas (que utilizan protección basada en virtualización de la integridad del código), debes tener en cuenta que estas tecnologías podrían ser incompatibles con algunos dispositivos y aplicaciones. Te recomendamos que pruebes estas configuraciones en el laboratorio antes de habilitar las características en sistemas de producción. Si no lo haces, podrían producirse pérdidas de datos inesperadas o errores de detención.


## Menú Inicio
[comment]: # (ID: 372; Remitente: samli; estado: firmado)
Este problema afecta a Windows Server2016 instalado con la opción Servidor con Experiencia de escritorio.

Si instala las aplicaciones que agregan elementos de acceso directo dentro de una carpeta en el menú Inicio, los accesos directos no funcionarán hasta que cierre la sesión y vuelva a iniciarla.



Vuelva al centro principal de [Windows Server 2016](Windows-Server-2016.md).

## Rendimiento de Storport
Algunos sistemas pueden presentar un rendimiento de almacenamiento reducido al ejecutar una nueva instalación de Windows Server 2016 frente a Windows Server 2012 R2.Durante el desarrollo de Windows Server 2016 se realizaron una serie de cambios para mejorar la seguridad y confiabilidad de la plataforma. Algunos de esos cambios, como la habilitación de Windows Defender de manera predeterminada, dan como resultado rutas de acceso de E/S más largas, que pueden reducir el rendimiento de E/S en determinadas cargas de trabajo y determinados patrones. Microsoft no recomienda deshabilitar Windows Defender, ya que es una importante capa de protección para tus sistemas.  

## Copyright  
Este documento se proporciona “tal cual”. La información y las vistas expresadas en este documento, incluidas las direcciones URL y otras referencias a sitios web de Internet, pueden cambiar sin previo aviso.  

Este documento no le proporciona derechos legales sobre ninguna propiedad intelectual en ningún producto de Microsoft. Puede copiar y usar este documento para su referencia interna.  

&copy;2016 Microsoft Corporation. Todos los derechos reservados.  

Microsoft, Active Directory, Hyper-V, Windows y Windows Server son marcas comerciales registradas o marcas comerciales de Microsoft Corporation en los Estados Unidos u otros países.  

El producto contiene software de filtros gráficos que está basado parcialmente en el trabajo de Independent JPEG Group.  


1.0  
