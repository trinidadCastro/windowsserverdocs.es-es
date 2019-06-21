---
title: Habilitar BranchCache en un recurso compartido de archivos (opcional)
description: En este tema forma parte de BranchCache Deployment Guide para Windows Server 2016, que demuestra cómo implementar BranchCache en los modos de caché distribuida y hospedada para optimizar el uso de ancho de banda WAN de sucursales
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-bc
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 9c465a9e-c504-44ec-9ebc-4e06ba54db30
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fd1757f6da011c2f774d8f97f628e5f0e87d3bf7
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284035"
---
# <a name="enable-branchcache-on-a-file-share-optional"></a>Habilitar BranchCache en un recurso compartido de archivos (opcional)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este procedimiento para habilitar BranchCache en un recurso compartido de archivos.  
  
> [!IMPORTANT]  
> No es necesario realizar este procedimiento si configura la configuración de publicación de hash con el valor **permitir la publicación de hash para todas las carpetas compartidas**.  
  
El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o un grupo equivalente.  
  
### <a name="to-enable-branchcache-on-a-file-share"></a>Para habilitar BranchCache en un recurso compartido de archivos  
  
1.  Abra Windows PowerShell, escriba **mmc**y presione ENTRAR. Se abrirá Microsoft Management Console (MMC).  
  
2.  En MMC, en el menú **Archivo**, haga clic en **Agregar o quitar complemento**. Se abre el cuadro de diálogo **Agregar o quitar complementos**.  
  
3.  En **agregar o quitar complementos**, en **complementos disponibles**, haga doble clic en **carpetas compartidas**. Se abre el Asistente de carpetas compartidas con el objeto de equipo Local seleccionado. Configurar la vista que prefiera, haga clic en **finalizar**y, a continuación, haga clic en **Aceptar**.  
  
4.  Haga doble clic en **(Local) para carpetas compartidas**y, a continuación, haga clic en **recursos compartidos**.  
  
5.  En el panel de detalles, haga clic en un recurso compartido y, a continuación, haga clic en **propiedades**. Del recurso compartido **propiedades** abre el cuadro de diálogo.  
  
6.  En el **propiedades** cuadro de diálogo el **General** , haga clic **configuración sin conexión**. El **configuración sin conexión** abre el cuadro de diálogo.  
  
7.  Asegúrese de que **solo los archivos y programas especificados por los usuarios estarán disponibles sin conexión** está seleccionada y, a continuación, haga clic en **habilitar BranchCache**.  
  
8.  Haga clic en **Aceptar** dos veces.  
  

