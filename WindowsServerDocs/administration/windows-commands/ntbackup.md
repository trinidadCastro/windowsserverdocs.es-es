---
title: ntbackup
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6bce6b0d-646b-46b6-b833-0ff1d6f082c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ebbe71fd5547311beb36747d32d695823e0f0059
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372682"
---
# <a name="ntbackup"></a>ntbackup



El comando **NTBackup** no está disponible en Windows Vista o windows Server 2008. En su lugar, debe usar el comando **Wbadmin** y los subcomandos para realizar copias de seguridad y restaurar el equipo y los archivos desde un símbolo del sistema.

No se pueden recuperar las copias de seguridad creadas con **ntbackup** mediante **wbadmin**. Sin embargo, existe una versión de **NTBackup** disponible como descarga para los usuarios de windows Server 2008 y Windows Vista que desean recuperar copias de seguridad creadas con **NTBackup**. Esta versión descargable de **NTBackup** solo permite realizar recuperaciones de copias de seguridad heredadas y no se puede usar en equipos que ejecutan windows Server 2008 o Windows Vista para crear nuevas copias de seguridad. Para descargar esta versión de **NTBackup**, consulte [@no__t 2](https://go.microsoft.com/fwlink/?LinkId=82917).

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

[Wbadmin](wbadmin.md)