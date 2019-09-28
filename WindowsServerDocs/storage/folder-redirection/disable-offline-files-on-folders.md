---
title: Deshabilitar Archivos sin conexión en carpetas redirigidas individuales
description: Cómo deshabilitar el almacenamiento en caché de Archivos sin conexión en carpetas individuales que se redirigen a recursos compartidos de red mediante el redireccionamiento de carpetas.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: c2614c0180b32a0215454f2d725d6a962986ef1f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394395"
---
# <a name="disable-offline-files-on-individual-redirected-folders"></a>Deshabilitar Archivos sin conexión en carpetas redirigidas individuales

>Se aplica a: Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2, Windows (canal semianual)

En este tema se describe cómo deshabilitar el almacenamiento en caché de Archivos sin conexión en carpetas individuales que se redirigen a recursos compartidos de red mediante el redireccionamiento de carpetas. Esto permite especificar las carpetas que se excluirán del almacenamiento en caché de forma local, lo que reduce el tamaño de la caché de Archivos sin conexión y el tiempo necesario para sincronizar Archivos sin conexión.

>[!NOTE]
>Este tema incluye cmdlets de Windows PowerShell de ejemplo que puede usar para automatizar algunos de los procedimientos descritos. Para obtener más información, vea [conceptos básicos de Windows PowerShell](https://docs.microsoft.com/powershell/scripting/getting-started/fundamental/windows-powershell-basics?view=powershell-6).

## <a name="prerequisites"></a>Requisitos previos

Para deshabilitar Archivos sin conexión el almacenamiento en caché de carpetas redirigidas específicas, el entorno debe cumplir los siguientes requisitos previos.

- Un dominio Active Directory Domain Services (AD DS), con equipos cliente Unidos al dominio. No hay requisitos de nivel funcional de bosque o dominio ni requisitos de esquema.
- Equipos cliente que ejecutan Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows (canal semianual).
- Un equipo con la administración de directiva de grupo instalada.

## <a name="disabling-offline-files-on-individual-redirected-folders"></a>Deshabilitar Archivos sin conexión en carpetas redirigidas individuales

Para deshabilitar Archivos sin conexión el almacenamiento en caché de carpetas redirigidas específicas, use directiva de grupo para habilitar la configuración de Directiva no **hacer que las carpetas redirigidas específicas estén disponibles sin conexión** para el objeto de directiva de grupo correspondiente (GPO). La configuración de esta directiva como **deshabilitada** o **no configurada** hace que todas las carpetas redirigidas estén disponibles sin conexión.

>[!NOTE]
>Solo los administradores de dominio, los administradores de organización y los miembros del grupo propietarios de directiva de grupo Creator pueden crear GPO.

### <a name="to-disable-offline-files-on-specific-redirected-folders"></a>Para deshabilitar Archivos sin conexión en carpetas redirigidas específicas

1. Abra **Administración de directiva de grupo**.
2. Para crear opcionalmente un nuevo GPO que especifique qué usuarios deben tener carpetas redirigidas excluidas de estar disponibles sin conexión, haga clic con el botón secundario en el dominio o unidad organizativa (OU) adecuado y, a continuación, seleccione **crear un GPO en este dominio y vincularlo aquí.** .
3. En el árbol de consola, haga clic con el botón secundario en el GPO para el que desea configurar las opciones de redirección de carpetas y, a continuación, seleccione **Editar**. Aparece el Editor de administración de directivas de grupo.
4. En el árbol de consola, **en configuración de usuario**, expanda **directivas**, expanda **plantillas administrativas**, expanda **sistema**y expanda **redirección de carpetas**.
5. Haga clic con el botón derecho en **no hacer que las carpetas redirigidas específicas estén disponibles sin conexión automáticamente** y, a continuación, seleccione **Editar**. Aparece la ventana no **hacer que las carpetas redirigidas específicas estén disponibles sin conexión automáticamente** .
6. Seleccione **Habilitado**. En el panel **Opciones** , seleccione las carpetas que no deben estar disponibles sin conexión activando las casillas adecuadas. Seleccione **Aceptar**.

### <a name="windows-powershell-equivalent-commands"></a>Comandos equivalentes de Windows PowerShell

Los siguientes cmdlets o cmdlets de Windows PowerShell realizan la misma función que el procedimiento descrito en [deshabilitar archivos sin conexión en carpetas redirigidas individuales](#disabling-offline-files-on-individual-redirected-folders). Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.

En este ejemplo se crea un nuevo GPO denominado *archivos sin conexión Settings* en la unidad organizativa *miuo* del dominio *contoso.com* (el nombre distintivo LDAP es "ou = miuo, DC = Contoso, DC = com"). A continuación, deshabilita Archivos sin conexión para la carpeta redirigida de vídeos.

```PowerShell
New-GPO -Name "Offline Files Settings" | New-Gplink -Target "ou=MyOu,dc=contoso,dc=com" -LinkEnabled Yes

Set-GPRegistryValue –Name "Offline Files Settings" –Key
"HKCU\Software\Policies\Microsoft\Windows\NetCache\{18989B1D-99B5-455B-841C-AB7C74E4DDFC}" -ValueName DisableFRAdminPinByFolder –Type DWORD –Value 1
```

Vea la tabla siguiente para obtener una lista de los nombres de clave del registro (GUID de carpeta) que se usarán para cada carpeta redirigida.

|Carpeta redirigida|Nombre de clave del registro (GUID de carpeta)|
|---|---|
|AppData (itinerancia)|{3EB685DB-65F9-4CF6-A03A-E3EF65729F3D}|
|Escritorio|B4BFCC3A-DB2C-424C-B029-7FE99A87C641|
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

- [Información general sobre el redireccionamiento de carpetas, los Archivos sin conexión y los perfiles de usuario móviles](folder-redirection-rup-overview.md)
- [Implementar el redireccionamiento de carpetas con Archivos sin conexión](deploy-folder-redirection.md)