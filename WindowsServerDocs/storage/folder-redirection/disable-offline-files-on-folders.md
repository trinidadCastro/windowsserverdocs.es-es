---
title: Deshabilitar archivos sin conexión en las carpetas redirigidas individuales
description: Cómo deshabilitar el almacenamiento en caché de archivos sin conexión en las carpetas individuales que se le redirigirá a recursos compartidos de red mediante el uso de la redirección de carpetas.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: b006742c9256c357d9aff3fb1b765dbed087383a
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2019
ms.locfileid: "65475882"
---
# <a name="disable-offline-files-on-individual-redirected-folders"></a>Deshabilitar archivos sin conexión en las carpetas redirigidas individuales

>Se aplica a: Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2 o Windows (canal semianual)

Este tema describe cómo deshabilitar el almacenamiento en caché de archivos sin conexión en las carpetas individuales que se le redirigirá a recursos compartidos de red mediante el uso de la redirección de carpetas. Esto proporciona la capacidad de especificar las carpetas que desea excluir del almacenamiento en caché localmente, lo que reduce la memoria caché de archivos sin conexión tamaño y el tiempo necesario para sincronizar archivos sin conexión.

>[!NOTE]
>Este tema incluye cmdlets de Windows PowerShell de ejemplo que puede usar para automatizar algunos de los procedimientos descritos. Para obtener más información, consulte [conceptos básicos de Windows PowerShell](https://docs.microsoft.com/powershell/scripting/getting-started/fundamental/windows-powershell-basics?view=powershell-6).

## <a name="prerequisites"></a>Requisitos previos

Para deshabilitar el almacenamiento en caché de archivos sin conexión de carpetas redirigidas específicas, el entorno debe cumplir los siguientes requisitos previos.

- Un dominio de Active Directory Domain Services (AD DS), con los equipos cliente Unidos al dominio. No existen requisitos de nivel funcional del bosque o dominio ni requisitos de esquema.
- Equipos cliente que ejecutan Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows (canal semianual).
- Un equipo con administración de directivas de grupo instalada.

## <a name="disabling-offline-files-on-individual-redirected-folders"></a>Deshabilitar archivos sin conexión en las carpetas redirigidas individuales

Para deshabilitar archivos sin conexión de almacenamiento en caché de carpetas redirigidas específicas, use Directiva de grupo para habilitar el **hacer no automáticamente que determinadas carpetas redirigidas estén disponibles sin conexión** configuración de directiva para el objeto de directiva de grupo (GPO) adecuado. Esta configuración de directiva para **deshabilitado** o **no configurado** hace que todas las carpetas redirigidas estén disponibles sin conexión.

>[!NOTE]
>Solo los administradores de dominio, administradores de organización y los miembros del grupo de propietarios del creador de directiva de grupo pueden crear GPO.

### <a name="to-disable-offline-files-on-specific-redirected-folders"></a>Para deshabilitar archivos sin conexión en determinadas carpetas redirigidas

1. Abra **administración de directivas de grupo**.
2. Para, opcionalmente, crear un nuevo GPO que especifica qué usuarios deben haber redirigido carpetas excluidas de estar disponibles sin conexión, haga clic en el dominio adecuado o unidad organizativa (OU) y, a continuación, seleccione **crear un GPO en este dominio y vincular TI aquí**.
3. En el árbol de consola, haga clic en el GPO para el que desea configurar las opciones de redirección de carpetas y, a continuación, seleccione **editar**. Aparece el Editor de administración de directivas de grupo.
4. En el árbol de consola, bajo **configuración de usuario**, expanda **directivas**, expanda **plantillas administrativas**, expanda **sistema**, y Expanda **redirección de carpetas**.
5. Haga clic en **hacer no automáticamente que determinadas carpetas redirigidas estén disponibles sin conexión** y, a continuación, seleccione **editar**. El **hacer no automáticamente que determinadas carpetas redirigidas estén disponibles sin conexión** aparecerá la ventana.
6. Seleccione **Habilitado**. En el **opciones** panel, seleccione las carpetas que no deben estar disponibles sin conexión seleccionando las casillas correspondientes. Seleccione **Aceptar**.

### <a name="windows-powershell-equivalent-commands"></a>Comandos equivalentes de Windows PowerShell

El siguiente cmdlet de Windows PowerShell o los cmdlets realizan la misma función, como se describe el procedimiento en [deshabilitar archivos sin conexión en las carpetas redirigidas individuales](#disabling-offline-files-on-individual-redirected-folders). Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.

Este ejemplo crea un nuevo GPO denominado *configuración de archivos sin conexión* en el *Miuo* unidad organizativa en el *contoso.com* dominio (es el nombre distintivo LDAP "ou = MyOU, dc = Contoso, dc = com "). A continuación, se deshabilita archivos sin conexión para la carpeta vídeos redirigidos.

```PowerShell
New-GPO -Name "Offline Files Settings" | New-Gplink -Target "ou=MyOu,dc=contoso,dc=com" -LinkEnabled Yes

Set-GPRegistryValue –Name "Offline Files Settings" –Key
"HKCU\Software\Policies\Microsoft\Windows\NetCache\{18989B1D-99B5-455B-841C-AB7C74E4DDFC}" -ValueName DisableFRAdminPinByFolder –Type DWORD –Value 1
```

Consulte la tabla siguiente para obtener una lista de nombres de clave del registro (carpeta de GUID) que se usará para cada carpeta redirigida.

|Carpeta redirigida|Nombre de la clave del registro (GUID de carpeta)|
|---|---|
|AppData (roaming)|{3EB685DB-65F9-4CF6-A03A-E3EF65729F3D}|
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
|Juegos guardados|{4C5C32FF-BB9D-43B0-B5B4-2D72E54EAAA4}|

## <a name="more-information"></a>Más información

- [Información general sobre la redirección de carpetas, archivos sin conexión y perfiles de usuario móviles](folder-redirection-rup-overview.md)
- [Implementar la redirección de carpetas con archivos sin conexión](deploy-folder-redirection.md)