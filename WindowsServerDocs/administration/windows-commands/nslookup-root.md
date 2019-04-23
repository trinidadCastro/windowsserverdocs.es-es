---
title: nslookup root
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9c29edc3-ec49-43f2-bc49-86bf0612d816
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bc2952bdbf709c31d720a7fb57430429edf9feb1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871916"
---
# <a name="nslookup-root"></a>nslookup root

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

cambia el servidor predeterminado en el servidor para la raíz del espacio de nombre de dominio del sistema de nombres de dominio (DNS).
## <a name="syntax"></a>Sintaxis
```
root 
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|{help &#124; ?}|Muestra un resumen breve de **nslookup** subcomandos.|
## <a name="remarks"></a>Comentarios
-   Actualmente, se usa el servidor de nombres ns.nic.ddn.mil. Este comando es un sinónimo de lserver ns.nic.ddn.mil. Puede cambiar el nombre del servidor de raíz con el **conjunto raíz** comando.
## <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[nslookup establece raíz](nslookup-set-root.md)
