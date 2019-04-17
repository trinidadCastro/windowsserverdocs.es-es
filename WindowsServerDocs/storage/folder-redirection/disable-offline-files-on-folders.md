---
title: Deshabilitar archivos sin conexión en las carpetas redirigidas individuales
description: Cómo deshabilitar el almacenamiento en caché de archivos sin conexión en carpetas individuales que se redirigen a recursos compartidos de red mediante el uso de redirección de carpetas.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: adc93906cb7ff958fc1db7b00abdc557623e764e
ms.sourcegitcommit: 9ed4c9fe04ebf3ef488170503c9a354c992b6fde
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/03/2018
ms.locfileid: "4339313"
---
# Deshabilitar archivos sin conexión en las carpetas redirigidas individuales

>Se aplica a: Windows 10, Windows 8, Windows 8.1, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016

En este tema se describe cómo deshabilitar el almacenamiento en caché de archivos sin conexión en carpetas individuales que se redirigen a recursos compartidos de red mediante el uso de redirección de carpetas. Esto proporciona la capacidad de especificar qué carpetas para excluir de almacenamiento en caché de forma local, lo que reduce la memoria caché de archivos sin conexión tamaño y el tiempo necesario para sincronizar archivos sin conexión.

>[!NOTE]
>Este tema incluye cmdlets de Windows PowerShell de muestra que puedes usar para automatizar algunos de los procedimientos descritos. Para obtener más información, consulta los [Conceptos básicos de PowerShell de Windows](https://docs.microsoft.com/powershell/scripting/getting-started/fundamental/windows-powershell-basics?view=powershell-6).

## Requisitos previos

Para deshabilitar el almacenamiento en caché de archivos sin conexión de las carpetas redirigidas específicas, el entorno debe cumplir los siguientes requisitos previos.

- Un dominio de Active Directory Domain Services (AD DS), con los equipos cliente unido al dominio. No existen requisitos de nivel funcional de dominio o bosque o los requisitos de esquema.
- Equipos de cliente que ejecutan Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.
- Un equipo con administración de directivas de grupo instalado.

## Al deshabilitar archivos sin conexión en las carpetas redirigidas individuales

Para deshabilitar el almacenamiento en caché de archivos sin conexión de las carpetas redirigidas específicas, usar la directiva de grupo para habilitar a la configuración de directiva **no automáticamente que las carpetas redirigidas específicas disponibles sin conexión** para el objeto de directiva de grupo (GPO) apropiado. Esta configuración de directiva en **deshabilitado** o **No configurado** hace que todas las carpetas redirigidas disponibles sin conexión.

>[!NOTE]
>Solo los administradores de dominio, los administradores de empresa y los miembros del grupo de los propietarios de creador de la directiva de grupo pueden crear GPO.

### Para deshabilitar archivos sin conexión en carpetas redirigidas específicas

1. Abre **administración de directivas de grupo**.
2. Para crear, opcionalmente, un nuevo GPO que especifica que los usuarios deben ha redirigido excluidas de que se hagan disponibles sin conexión de carpetas, haz clic en el dominio adecuado o la unidad organizativa (OU) y, a continuación, selecciona **crear un GPO en este dominio y vincularlo aquí **.
3. En el árbol de consola, haz clic en el GPO para la que quieres configurar las opciones de redirección de carpetas y, a continuación, selecciona **Editar**. Aparece el Editor de administración de directivas de grupo.
4. En el árbol de consola, en **Configuración de usuario**, expanda **directivas**, expanda **Plantillas administrativas**, expanda **sistema**y expande la **Redirección de carpetas**.
5. Haz clic en **no automáticamente que las carpetas redirigidas específicas disponibles sin conexión** y, a continuación, selecciona **Editar**. Aparecerá el cuadro de diálogo **automáticamente que las carpetas redirigidas específicas disponibles sin conexión** .
6. Selecciona **Habilitado**. En el panel de **Opciones de** seleccionar las carpetas que no deben estar disponibles sin conexión, selecciona las casillas correspondientes. Selecciona **Aceptar**.

### Comandos equivalentes de Windows PowerShell

El siguiente cmdlet de Windows PowerShell o los cmdlets de realizar la misma función que el procedimiento descrito en [Deshabilitar archivos sin conexión en las carpetas redirigidas individuales](#disabling-offline-files-on-individual-redirected-folders). Escribe cada cmdlet en una sola línea, aunque pueden aparecer con ajuste en varias líneas aquí debido a restricciones de formato.

Este ejemplo crea un nuevo GPO con el nombre de *Configuración de archivos sin conexión* en la unidad organizativa de *MyOu* en el dominio *contoso.com* (es el nombre completo LDAP "ou = MyOU, dc = contoso, dc = com"). A continuación, deshabilita archivos sin conexión para la carpeta de vídeos redirigidos.

```PowerShell
New-GPO -Name "Offline Files Settings" | New-Gplink -Target "ou=MyOu,dc=contoso,dc=com" -LinkEnabled Yes

Set-GPRegistryValue –Name "Offline Files Settings" –Key
"HKCU\Software\Policies\Microsoft\Windows\NetCache\{18989B1D-99B5-455B-841C-AB7C74E4DDFC}" -ValueName DisableFRAdminPinByFolder –Type DWORD –Value 1
```

Consulta la tabla siguiente para obtener una lista de nombres de clave del registro (carpeta GUID) que se usará para todas las carpetas redirigidas.

|Carpeta redirigida|Nombre de la clave del registro (carpeta GUID)|
|---|---|
|AppData|{3EB685DB-65F9-4CF6-A03A-E3EF65729F3D}|
|Escritorio|{B4BFCC3A-DB2C-424C-B029-7FE99A87C641}|
|Menú Inicio|{625B53C3-AB48-4EC1-BA1F-A1EF4146FC19}|
|Documentos|{FDD39AD0-238F-46AF-ADB4-6C85480369C7}|
|Imágenes|{33E28130-4E1E-4676-835A-98395C3BC3BB}|
|Música|{4BD8D571-6D19-48D3-BE97-422220080E43}|
|Vídeos|{18989B1D-99B5-455B-841C-AB7C74E4DDFC}|
|Favoritos|{1777F761-68AD-4D8A-87BD-30B759FA33DD}|
|Contactos|{56784854-C6CB-462b-8169-88E350ACB882}|
|Descargas|{374DE290-123F-4565-9164-39C4925E467B}|
|Vínculos|{BFB9D5E0-C6A9-404C-B2B2-AE6DB6AF4968}|
|Búsquedas|{7D1D3A04-DEBB-4115-95CF-2F29DA2920DA}|
|Partidas guardadas|{4C5C32FF-BB9D-43B0-B5B4-2D72E54EAAA4}|

## Más información

- [Información general de redirección de carpetas, archivos sin conexión y perfiles de usuario móvil](folder-redirection-rup-overview.md)
- [Implementar la redirección de carpetas con archivos sin conexión](deploy-folder-redirection.md)