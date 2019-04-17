---
title: Archivo por lotes de red (Netsh) Shell ejemplo
description: Puedes usar este tema para obtener información sobre cómo crear un archivo por lotes que realiza varias tareas con Netsh en Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: c94e37a4-3637-4613-9eb5-ed604e831eca
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b0528cfaef201ba30e00e30f56a763be39a6b828
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="network-shell-netsh-example-batch-file"></a>Archivo por lotes de red Shell \(Netsh\) ejemplo

Se aplica a: Windows Server 2016

Puedes usar este tema para obtener información sobre cómo crear un archivo por lotes que realiza varias tareas mediante Netsh en Windows Server 2016. En este archivo por lotes de ejemplo, el **netsh wins** se usa el contexto.

## <a name="example-batch-file-overview"></a>Introducción al archivo por lotes de ejemplo

Puedes usar los comandos Netsh para el servicio de nombres de Windows Internet \(WINS\) en archivos por lotes y otros scripts para automatizar tareas. El siguiente ejemplo de archivo por lotes muestra cómo usar los comandos Netsh para WINS para realizar diversas tareas relacionadas.

En este archivo por lotes de ejemplo, WINS\-A es un servidor en WINS con la dirección IP 192.168.125.30 y WINS\ B es un servidor de WINS con la dirección IP 192.168.0.189.

El archivo por lotes de ejemplo realice las siguientes tareas.

- Agrega un registro de nombre dinámico con la dirección IP 192.168.0.205, MY\_RECORD \[04h\], a WINS\-A
- Establece WINS\ B como un duplicador de extracción/de WINS\ A
- Se conecta a WINS\ B y, a continuación, Establece WINS\-A, como un duplicador de extracción/de WINS\ B
- Inicia una replicación de inserción de WINS\ A WINS\ b
- Se conecta a WINS\-B para comprobar que el nuevo registro, MY\_RECORD, duplicó correctamente

## <a name="netsh-example-batch-file"></a>Archivo por lotes de ejemplo de Netsh

En el siguiente archivo por lotes de ejemplo, las líneas que contienen los comentarios están precedidas por "rem" para comentario. Netsh omite los comentarios.

    rem: Begin example batch file.
    
    rem two WINS servers:
    
    rem (WINS-A) 192.168.125.30
    
    rem (WINS-B) 192.168.0.189
    
    rem 1. Connect to (WINS-A), and add the dynamic name MY\_RECORD \[04h\] to the (WINS-A) database.
    
    netsh wins server 192.168.125.30 add name Name=MY\_RECORD EndChar=04 IP={192.168.0.205}
    
    rem 2. Connect to (WINS-A), and set (WINS-B) as a push/pull replication partner of (WINS-A).
    
    netsh wins server 192.168.125.30 add partner Server=192.168.0.189 Type=2
    
    rem 3. Connect to (WINS-B), and set (WINS-A) as a push/pull replication partner of (WINS-B).
    
    netsh wins server 192.168.0.189 add partner Server=192.168.125.30 Type=2
    
    rem 4. Connect back to (WINS-A), and initiate a push replication to (WINS-B).
    
    netsh wins server 192.168.125.30 init push Server=192.168.0.189 PropReq=0
    
    rem 5. Connect to (WINS-B), and check that the record MY\_RECORD \[04h\] was replicated successfully.
    
    netsh wins server 192.168.0.189 show name Name=MY\_RECORD EndChar=04
    
    rem 6. End example batch file.

## <a name="netsh-wins-commands-used-in-the-example-batch-file"></a>Comandos Netsh WINS usados en el archivo por lotes de ejemplo

La siguiente sección listas la **netsh wins** comandos que se usan en este ejemplo de procedimiento.

- **servidor**. Cambia el contexto actual de línea de Web\ WINS al servidor especificado por su nombre o dirección IP.
- **Agregar nombre**. Registra un nombre en el servidor en WINS.
- **Agregar asociado**. Agrega a un duplicador en el servidor en WINS.
- **inserción de init**. Inicia y envía un desencadenador de inserción a un servidor WINS.
- **Mostrar nombre**. Muestra información detallada para un registro concreto en la base de datos WINS.  

Para obtener más información, consulta [Shell de red (Netsh)](netsh.md).
