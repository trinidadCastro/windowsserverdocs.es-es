---
title: prompt_1 de FTP
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 930df39b-45c4-4e0b-bfe2-1d1963be817a vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 65f7505dfcb3677fcaace9bd645cca7e7ba70b7d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438594"
---
# <a name="ftp-prompt1"></a>ftp: prompt_1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Alterna entre **símbolo del sistema** activar y desactivar el modo.   
## <a name="syntax"></a>Sintaxis  
```  
prompt  
```  
### <a name="parameters"></a>Parámetros  
ninguno  
## <a name="remarks"></a>Comentarios  
- De forma predeterminada, **símbolo del sistema** está activado.  
- **FTP** solicita durante la transferencia de múltiples archivos para que pueda recuperar o almacenar archivos de forma selectiva.  **Mget** y **mput** transferir todos los archivos si **símbolo del sistema** está desactivada.  
  ## <a name="BKMK_Examples"></a>Ejemplos  
  Alternar modo de mensaje y desactivar.  
  ```  
  prompt  
  ```  
  ## <a name="additional-references"></a>Referencias adicionales  
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  