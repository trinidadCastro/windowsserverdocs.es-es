---
title: jetpack
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 82a2b7ef-0db5-4575-a028-8acb0bf6c7ba
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a3bffc29519df139921bdb1de53e67acd558b306
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858016"
---
# <a name="jetpack"></a>jetpack

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

compacta una base de datos de servicio de nombres Internet de Windows (WINS) o protocolo de configuración dinámica de Host (DHCP). Microsoft recomienda compactar la base de datos cada vez que se aproxima a 30 MB. 

## <a name="syntax"></a>Sintaxis
```
jetpack.EXE <database name> <temp database name>
```

### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|<database name>|Especifica el archivo de base de datos original.|
|<temp database name>|Especifica el archivo de base de datos temporal.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="BKMK_Examples"></a>Ejemplos
Para compactar la base de datos WINS:
```
cd %SYSTEMROOT%\SYSTEM32\WINS
NET STOP WINS
jetpack WINS.MDB TMP.MDB
NET start WINS
```
Para compactar la base de datos DHCP:
```
cd %SYSTEMROOT%\SYSTEM32\DHCP
NET STOP DHCPSERver
jetpack DHCP.MDB TMP.MDB
NET start DHCPSERver
```
En los ejemplos anteriores, **Tmp.mdb** es una base de datos temporal jetpack.exe usa. **WINS.mdb** es la base de datos de WINS. **DHCP.mdb** es la base de datos DHCP.
Jetpack.exe compacta el WINS o base de datos DHCP haciendo lo siguiente:
1.  Copias de base de datos información a un archivo de base de datos temporal denominado **Tmp.mdb**.
2.  Elimina el archivo de base de datos original, **Wins.mdb** o **Dhcp.mdb**.
3.  cambia el nombre de los archivos de base de datos temporal para el nombre de archivo original.

> [!NOTE]
> Durante el proceso de compact jetpack.exe crea un archivo temporal con el nombre especificado por el *nombre de base de datos temporal* parámetro. El archivo temporal se quita cuando se completa el proceso de compact. Asegúrese de que no tiene un archivo ya existente en WINS o DHCP carpeta con el mismo nombre que el especificado en el *nombre de base de datos temporal* parámetro.

## <a name="additional-references"></a>Referencias adicionales
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
