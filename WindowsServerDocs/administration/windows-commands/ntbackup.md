---
title: ntbackup
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6bce6b0d-646b-46b6-b833-0ff1d6f082c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ba3aaaa192283e0e1dc1777a27fc13973949784b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723482"
---
# <a name="ntbackup"></a>ntbackup



El comando **NTBackup** no está disponible en Windows Vista o windows Server 2008. En su lugar, debe usar el comando **Wbadmin** y los subcomandos para realizar copias de seguridad y restaurar el equipo y los archivos desde un símbolo del sistema.

No se pueden recuperar las copias de seguridad creadas con **ntbackup** mediante **wbadmin**. Sin embargo, existe una versión de **NTBackup** disponible como descarga para los usuarios de windows Server 2008 y Windows Vista que desean recuperar copias de seguridad creadas con **NTBackup**. Esta versión descargable de **NTBackup** solo permite realizar recuperaciones de copias de seguridad heredadas y no se puede usar en equipos que ejecutan windows Server 2008 o Windows Vista para crear nuevas copias de seguridad. Para descargar esta versión de **NTBackup**, vea [https://go.microsoft.com/fwlink/?LinkId=82917](https://go.microsoft.com/fwlink/?LinkId=82917).

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

[Wbadmin](wbadmin.md)