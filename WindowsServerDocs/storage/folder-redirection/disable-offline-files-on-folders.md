---
title: Deshabilitación de Archivos sin conexión en carpetas redirigidas individuales
description: Cómo deshabilitar el almacenamiento en caché de Archivos sin conexión en carpetas individuales que se redirigen a recursos compartidos de red mediante Redirección de carpetas.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: c2614c0180b32a0215454f2d725d6a962986ef1f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394395"
---
# <a name="disable-offline-files-on-individual-redirected-folders"></a>Deshabilitación de Archivos sin conexión en carpetas redirigidas individuales

>Se aplica a: Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2, Windows (canal semianual)

En este tema se describe cómo deshabilitar el almacenamiento en caché de Archivos sin conexión en carpetas individuales que se redirigen a recursos compartidos de red mediante Redirección de carpetas. Este proceso permite especificar las carpetas que se excluirán del almacenamiento en caché de forma local, lo que reduce el tamaño de la caché de Archivos sin conexión y el tiempo necesario para sincronizar Archivos sin conexión.

>[!NOTE]
>Este tema incluye cmdlets de Windows PowerShell de ejemplo que puede usar para automatizar algunos de los procedimientos descritos. Para obtener más información, consulta [Conceptos básicos de Windows PowerShell](https://docs.microsoft.com/powershell/scripting/getting-started/fundamental/windows-powershell-basics?view=powershell-6).

## <a name="prerequisites"></a>Requisitos previos

Para deshabilitar el almacenamiento en caché de Archivos sin conexión de carpetas redirigidas específicas, el entorno debe cumplir los siguientes requisitos previos.

- Un dominio de Active Directory Domain Services (AD DS) con equipos cliente unidos al dominio. No se aplican requisitos de nivel funcional de bosque o dominio ni requisitos de esquema.
- Equipos cliente que ejecuten Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows (canal semianual).
- Un equipo que tenga instalada la herramienta Administración de directivas de grupo.

## <a name="disabling-offline-files-on-individual-redirected-folders"></a>Deshabilitación de Archivos sin conexión en carpetas redirigidas individuales

Para deshabilitar el almacenamiento en caché de Archivos sin conexión de carpetas redirigidas específicas, usa la directiva de grupo para habilitar la configuración de directiva **No hacer que las carpetas específicas redirigidas estén disponibles sin conexión automáticamente** para el objeto directiva de grupo (GPO) adecuado. Al configurar esta opción de directiva en **Deshabilitado** o **Sin configurar**, todas las carpetas redirigidas estarán disponibles sin conexión.

>[!NOTE]
>Solo los administradores de dominio, los administradores de empresas y los miembros del grupo de propietarios del creador de directivas de grupo pueden crear objetos de directiva de grupo.

### <a name="to-disable-offline-files-on-specific-redirected-folders"></a>Para deshabilitar Archivos sin conexión en carpetas redirigidas específicas

1. Abre **Administración de directivas de grupo**.
2. Para tener la opción de crear un nuevo GPO que especifique qué usuarios no deben tener las carpetas redirigidas disponibles sin conexión, haz clic con el botón derecho en el dominio o la unidad organizativa pertinente y, a continuación, selecciona **Crear un GPO en este dominio y vincularlo aquí**.
3. En el árbol de consola, haz clic con el botón derecho en el GPO para el que quieres configurar las opciones de Redirección de carpetas y, a continuación, selecciona **Editar**. Se muestra el Editor de administración de directivas de grupo.
4. En el árbol de consola, en **Configuración de usuario**, expande las opciones **Directivas**, **Plantillas administrativas**, **Sistema** y **Redirección de carpetas**.
5. Haz clic con el botón derecho en **No hacer que las carpetas específicas redirigidas estén disponibles sin conexión automáticamente** y, después, selecciona **Editar**. Se muestra la ventana **No hacer que las carpetas específicas redirigidas estén disponibles sin conexión automáticamente**.
6. Seleccione **Habilitado**. En el panel **Opciones**, activa las casillas de las carpetas que no deben estar disponibles sin conexión. Selecciona **Aceptar**.

### <a name="windows-powershell-equivalent-commands"></a>Comandos equivalentes de Windows PowerShell

Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento descrito en [Deshabilitación de Archivos sin conexión en carpetas redirigidas individuales](#disabling-offline-files-on-individual-redirected-folders). Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.

En este ejemplo se crea un nuevo GPO denominado *Configuración de Archivos sin conexión* en la unidad organizativa *MiUO* del dominio *contoso.com* (el nombre distintivo LDAP es "ou=MiUO,dc=contoso,dc=com"). A continuación, deshabilita Archivos sin conexión para la carpeta redirigida Vídeos.

```PowerShell
New-GPO -Name "Offline Files Settings" | New-Gplink -Target "ou=MyOu,dc=contoso,dc=com" -LinkEnabled Yes

Set-GPRegistryValue –Name "Offline Files Settings" –Key
"HKCU\Software\Policies\Microsoft\Windows\NetCache\{18989B1D-99B5-455B-841C-AB7C74E4DDFC}" -ValueName DisableFRAdminPinByFolder –Type DWORD –Value 1
```

Consulta la tabla siguiente para obtener una lista de los nombres de claves del Registro (GUID de carpeta) que se usarán para cada carpeta redirigida.

|Carpeta redirigida|Nombre de clave del Registro (GUID de carpeta)|
|---|---|
|AppData(Roaming)|{3EB685DB-65F9-4CF6-A03A-E3EF65729F3D}|
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

## <a name="more-information"></a>Información adicional

- [Introducción al redireccionamiento de carpetas, archivos sin conexión y perfiles de usuario móvil](folder-redirection-rup-overview.md)
- [Implementar la redirección de carpetas con Archivos sin conexión](deploy-folder-redirection.md)