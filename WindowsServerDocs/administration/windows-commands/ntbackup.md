---
title: ntbackup
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 783f73eba2aeaf9f30c5c1e12a623f1f87f24ede
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846346"
---
# <a name="ntbackup"></a>ntbackup



El **ntbackup** comando no está disponible en Windows Vista o Windows Server 2008. En su lugar, debe usar el **wbadmin** comando y subcomandos para realizar copias de seguridad y restaurar el equipo y archivos desde un símbolo del sistema.

No se pueden recuperar las copias de seguridad creadas con **ntbackup** mediante **wbadmin**. Sin embargo, una versión de **ntbackup** está disponible como descarga para usuarios de Windows Server 2008 y Windows Vista que desean recuperar copias de seguridad que crearon mediante **ntbackup**. Esta versión descargable de **ntbackup** permite realizar recuperaciones solo de las copias de seguridad heredadas y no se puede usar en equipos que ejecutan Windows Server 2008 o Windows Vista para crear nuevas copias de seguridad. Para descargar esta versión de **ntbackup**, consulte [ https://go.microsoft.com/fwlink/?LinkId=82917 ](https://go.microsoft.com/fwlink/?LinkId=82917).

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

[Wbadmin](wbadmin.md)