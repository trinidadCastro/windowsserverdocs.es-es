---
title: rdpsign
description: Obtenga información sobre cómo firmar digitalmente un archivo RDP.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4a6fa8ce-3d32-49a5-b056-bcc1a23391f5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 9e35d3a3e85ed046fb658bbf5a97ab5fc5eec6d3
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442013"
---
# <a name="rdpsign"></a>rdpsign

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Le permite firmar digitalmente un archivo de protocolo de escritorio remoto (.rdp).
Para obtener ejemplos de cómo usar este comando, consulte [ejemplos](#BKMK_examples).

> [!NOTE]
> En Windows Server 2008 R2, el nombre de Terminal Services se cambió a Servicios de Escritorio remoto. Para descubrir las novedades de la versión más reciente, consulte [novedades nuevos servicios de escritorio remoto en Windows Server 2012](https://technet.microsoft.com/library/hh831527) en la biblioteca de TechNet de Windows Server.

## <a name="syntax"></a>Sintaxis
```
rdpsign /sha1 <hash> [/q | /v |] [/l] <file_name.rdp>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------|--------|
|/sha1 \<hash>|Especifica la huella digital, que es el hash del algoritmo Hash seguro 1 (SHA1) del certificado de firmado que se incluye en el almacén de certificados.|
|/q|Modo silencioso. Ningún resultado cuando el comando se ejecuta correctamente y la salida mínima si se produce un error en el comando.|
|/v|Modo detallado. Muestra todas las advertencias, mensajes y estado.|
|/l|Comprueba los resultados de la firmas y de salida sin realmente reemplazar cualquiera de los archivos de entrada.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios
-   La huella digital de SHA1 debe representar un publicador del archivo .rdp de confianza. Para obtener la huella digital del certificado, abra el complemento certificados, haga doble clic en el certificado que desea utilizar (en el almacén de certificados del equipo local o en el almacén de certificados personales), haga clic en el **detalles** ficha y, a continuación, en el **campo** lista, haga clic en **huella digital**.

    > [!NOTE]
    > Al copiar la huella digital para su uso con la herramienta rdpsign.exe, debe quitar los espacios en blanco.

-   Debe especificar el archivo .rdp (o archivos) para iniciar sesión con el nombre de archivo completo. No se aceptan caracteres comodín.
-   Los archivos de salida con signo sobrescribirá los archivos de entrada.
-   Si alguno de los archivos .rdp no puede leer o escribir en, la herramienta continuará con el siguiente archivo si se especifican varios archivos.

## <a name="BKMK_examples"></a>Ejemplos
- Para iniciar sesión en un archivo .rdp que se denomina File1.rdp, navegue hasta la carpeta donde guardó el archivo .rdp y, a continuación, escriba lo siguiente:
  ```
  rdpsign /sha1 hash file1.rdp
  ```
  > [!NOTE]
  > El *hash* valor representa la huella digital de SHA1, sin espacios en blanco.
- Para comprobar si la firma digital se realizará correctamente para un archivo .rdp sin firmar realmente el archivo, escriba lo siguiente:
  ```
  rdpsign /sha1 hash /l file1.rdp
  ```
- Para iniciar sesión en varios archivos .rdp, separe los nombres de archivo mediante espacios. Por ejemplo, para iniciar sesión en varios archivos .rdp que se denominan File1.rdp, File2.rdp y File3.rdp, escriba lo siguiente:
  ```
  rdpsign /sha1 hash file1.rdp file2.rdp file3.rdp
  ```
  ## <a name="see-also"></a>Vea también
  [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
  [servicios de escritorio remoto &#40;servicios de Terminal Server&#41; referencia del comando](remote-desktop-services-terminal-services-command-reference.md)
