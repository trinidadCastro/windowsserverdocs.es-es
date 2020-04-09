---
title: rdpsign
description: Obtenga información sobre cómo firmar digitalmente un archivo RDP.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4a6fa8ce-3d32-49a5-b056-bcc1a23391f5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: c9df46ab09b758cbd86334d5b418c876c41f0eff
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836678"
---
# <a name="rdpsign"></a>rdpsign

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Permite firmar digitalmente un archivo Protocolo de escritorio remoto (. RDP).
para obtener ejemplos de cómo usar este comando, vea [ejemplos](#BKMK_examples).

> [!NOTE]
> En Windows Server 2008 R2, el nombre de Terminal Services ha cambiado a Servicios de Escritorio remoto. Para conocer las novedades de la versión más reciente, consulte [novedades de servicios de escritorio remoto en Windows server 2012](https://technet.microsoft.com/library/hh831527) en la biblioteca de TechNet de Windows Server.

## <a name="syntax"></a>Sintaxis
```
rdpsign /sha1 <hash> [/q | /v |] [/l] <file_name.rdp>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------|--------|
|\<hash de/SHA1 >|Especifica la huella digital, que es el hash Algoritmo hash seguro 1 (SHA1) del certificado de firma que se incluye en el almacén de certificados. Se usa en Windows Server 2012 R2 y versiones anteriores.|
|\<hash de/SHA256 >|Especifica la huella digital, que es el hash del algoritmo hash seguro 256 (SHA256) del certificado de firma que se incluye en el almacén de certificados. Reemplaza/SHA1 en Windows Server 2016 y versiones más recientes.|
|/q|Modo silencioso. No hay ninguna salida cuando el comando se ejecuta correctamente y la salida mínima si se produce un error en el comando.|
|/v|modo detallado. Muestra todas las advertencias, mensajes y estado.|
|/l|Comprueba los resultados de la firma y la salida sin reemplazar realmente ninguno de los archivos de entrada.|
|/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios
-   La huella digital del certificado SHA1 o SHA256 debe representar un editor de archivos. RDP de confianza. Para obtener la huella digital del certificado, abra el complemento certificados, haga doble clic en el certificado que desea usar (ya sea en el almacén de certificados del equipo local o en el almacén de certificados personales), haga clic en la pestaña **detalles** y, a continuación, en la lista de **campos** , haga clic en **huella digital**.

    > [!NOTE]
    > Al copiar la huella digital para usarla con la herramienta rdpsign. exe, debe quitar los espacios.

-   Debe especificar el archivo. RDP (o archivos) para firmar con el nombre de archivo completo. No se aceptan caracteres comodín.
-   Los archivos de salida firmados sobrescribirán los archivos de entrada.
-   Si alguno de los archivos. RDP no se puede leer ni escribir en él, la herramienta continuará con el siguiente archivo si se especifican varios archivos.

## <a name="examples"></a><a name="BKMK_examples"></a>Example
- Para firmar un archivo. RDP denominado archivo1. RDP, navegue hasta la carpeta donde guardó el archivo. RDP y, a continuación, escriba lo siguiente:
  ```
  rdpsign /sha1 hash file1.rdp
  ```
  > [!NOTE]
  > El valor *hash* representa la huella digital del certificado SHA1, sin espacios en blanco.
- Para comprobar si la firma digital se realizará correctamente en un archivo. RDP sin firmar realmente el archivo, escriba lo siguiente:
  ```
  rdpsign /sha1 hash /l file1.rdp
  ```
- Para firmar varios archivos. RDP, separe los nombres de archivo mediante espacios. Por ejemplo, para firmar varios archivos. RDP denominados archivo1. RDP, archivo2. RDP y Archivo3. RDP, escriba lo siguiente:
  ```
  rdpsign /sha1 hash file1.rdp file2.rdp file3.rdp
  ```
  ## <a name="see-also"></a>Consulta también
  - Referencia [de comandos de
  de la clave de sintaxis de línea de comandos](command-line-syntax-key.md) [servicios de escritorio remoto (Terminal Services)](remote-desktop-services-terminal-services-command-reference.md)
