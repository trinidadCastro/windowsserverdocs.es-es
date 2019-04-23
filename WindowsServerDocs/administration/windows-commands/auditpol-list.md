---
title: lista Auditpol
description: 'Tema de los comandos de Windows para **auditpol lista** : categorías de directiva o las subcategorías de auditoría de listas o enumera los usuarios para los que la directiva de auditoría por usuario se define.'
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 45502abe-3d6e-4e13-94f0-8e6fcb6db860
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 08f524ef0aacd731f709ce7a2e17b3d831da1e5b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858586"
---
# <a name="auditpol-list"></a>lista Auditpol

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Enumera los usuarios para los que se define una directiva de auditoría por usuario o subcategorías o categorías de directiva de auditoría listas.

## <a name="syntax"></a>Sintaxis
```
auditpol /list
[/user|/category|subcategory[:<categoryname>|<{guid}>|*]]
[/v] [/r]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/user|Recupera todos los usuarios para los que se ha definido la directiva de auditoría por usuario. Si se usa con el parámetro /v, también se muestra el identificador de seguridad (SID) del usuario.|
|/Category|Muestra los nombres de las categorías que comprenda el sistema. Si se usa con el parámetro /v, también se muestra el identificador único global (GUID) de categoría.|
|/subcategory|Muestra los nombres de las subcategorías y sus GUID asociados.|
|/v|Muestra el GUID con la categoría o subcategoría, o cuando se usa con/User, el SID de cada usuario.|
|/r|Muestra el resultado como un informe en formato de valores separados por comas (CSV).|
|/?|Muestra la ayuda en el símbolo del sistema.|
## <a name="remarks"></a>Comentarios
todas las operaciones de lista para la directiva por usuario, debe tener permiso lectura en ese objeto establecido en el descriptor de seguridad. También se pueden realizar operaciones de lista que poseen el **Administrar registro de auditoría y seguridad** derecho de usuario (SeSecurityPrivilege). Sin embargo, este derecho permite acceso adicional que no es necesario realizar la operación de lista.
## <a name="BKMK_examples"></a>Ejemplos
Para obtener una lista de todos los usuarios que tienen una directiva de auditoría definido, escriba:
```
auditpol /list /user
```
Para obtener una lista de todos los usuarios que tienen una directiva de auditoría definidos y su SID asociado, escriba:
```
auditpol /list /user /v
```
Para obtener una lista de todas las categorías y subcategorías en formato de informe, escriba:
```
auditpol /list /subcategory:* /r
```
Para enumerar las subcategorías de las categorías de seguimiento y acceso DS detalladas, escriba:
```
auditpol /list /subcategory:"detailed Tracking","DS Access"
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
