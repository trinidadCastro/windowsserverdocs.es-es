---
title: Combinar vdisk
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5865bb08-89a3-406c-8328-0ef8868d03e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bc20fcaf6e511bb25156996bddc3357f99195875
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437411"
---
# <a name="merge-vdisk"></a>Combinar vdisk

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Combina un diferenciación disco duro virtual (VHD) con su elemento primario correspondiente VHD. Se modificará el VHD primario para incluir las modificaciones en el disco duro virtual de diferenciación.
> [!NOTE]
> Este comando sólo es aplicable a Windows 7 y Windows Server 2008 R2.
> ## <a name="syntax"></a>Sintaxis
> ```
> merge vdisk depth=<n>
> ```
> ### <a name="parameters"></a>Parámetros
> 
> | Parámetro |                                                                                    Descripción                                                                                    |
> |-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> | profundidad =<n> | Indica el número de archivos de disco duro virtual principal para combinar. Por ejemplo, **profundidad = 1** indica que el VHD de diferenciación se combinarán con un nivel de la cadena de diferenciación. |
> 
> ## <a name="remarks"></a>Comentarios
> - Un disco duro virtual debe estar seleccionado y desconectado para que esta operación se realice correctamente. Use la **seleccione vdisk** comando para seleccionar un disco duro virtual y desplace el foco a ella.
> - Este parámetro modifica al VHD primario. Como resultado, otros VHD de diferenciación que dependen del elemento primario ya no serán válidos.
>   ## <a name="BKMK_Examples"></a>Ejemplos
>   Para combinar un VHD de diferenciación con su elemento primario de disco duro virtual, escriba:
>   ```
>   merge vdisk depth=1
>   ```
>   ## <a name="additional-references"></a>Referencias adicionales
> - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
> - [attach vdisk](attach-vdisk.md)
> - [compact vdisk](compact-vdisk.md)

-   [vdisk detallado](detail-vdisk.md)
-   [Desasociar vdisk](detach-vdisk.md)
-   [Expandir vdisk](expand-vdisk.md)
-   [select vdisk](select-vdisk.md)
-   [list_1](list_1.md)
