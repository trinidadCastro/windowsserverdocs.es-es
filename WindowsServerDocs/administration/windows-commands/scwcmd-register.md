---
title: Scwcmd register
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fe4d126a-9f27-4076-b7b1-fbefa45f378a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cc8b4a06af519b0da01dfcab8de0139b12cc68f2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834976"
---
# <a name="scwcmd-register"></a>Scwcmd: register

> Se aplica a: Windows Server 2012 R2, Windows Server 2012

Amplía o personaliza la base de datos de configuración de seguridad de Asistente de configuración de seguridad (SCW) mediante el registro de un archivo de base de datos de configuración de seguridad que contiene la función, tarea, servicio o las definiciones de puerto.

## <a name="syntax"></a>Sintaxis

```
scwcmd register /kbname:<MyApp> [/kbfile:<kb.xml>] [/kb:<path>] [/d]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/ KB:\<MyApp >|Especifica el nombre en la que se registrará la extensión de la base de datos de configuración de seguridad. Este parámetro debe especificarse.|
|/kbfile:\<Kb.xml>|Especifica la ruta de acceso y el nombre del archivo de base de datos de configuración de seguridad que se usará para ampliar o personalizar la base de datos de configuración de seguridad base. Para validar que el archivo de base de datos de configuración de seguridad es compatible con el esquema SCW, use el archivo de definición de esquema %windir%\security\KBRegistrationInfo.xsd. Esta opción debe proporcionarse a menos que el **/d** se especifica el parámetro.|
|/KB:\<ruta de acceso >|Especifica la ruta de acceso al directorio que contiene los archivos de base de datos de configuración de seguridad de SCW para actualizarse. Si no se especifica esta opción, se utiliza %windir%\security\msscw\kbs.|
|/d|Anula el registro de una extensión de la base de datos de configuración de seguridad de la base de datos de configuración de seguridad. La extensión para anular el registro especificada por el parámetro/KB. (El **/kbfile** no debe especificarse el parámetro.) Especifica la base de datos de configuración de seguridad para anular el registro de la extensión de la **/kb** parámetro.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

Scwcmd.exe solo está disponible en equipos que ejecutan Windows Server 2008 R2, Windows Server 2008 o Windows Server 2003.

## <a name="BKMK_Examples"></a>Ejemplos

Para registrar el archivo de base de datos de configuración de seguridad denominado SCWKBForMyApp.xml bajo el nombre de MyApp en la ubicación \\ \\kbserver\kb, tipo:
```
scwcmd register /kbfile:d:\SCWKBForMyApp.xml /kbname:MyApp /kb:\\kbserver\kb
```
Para anular el registro el MyApp de base de datos de configuración de seguridad ubicado en \\ \\kbserver\kb, tipo:
```
scwcmd register /d /kbname:MyApp /kb:\\kbserver\kb
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)