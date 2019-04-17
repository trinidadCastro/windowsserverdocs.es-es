---
title: Habilitar BranchCache en un recurso compartido de archivos (opcional)
description: En este tema es parte de la BranchCache implementación Guía para Windows Server 2016, que se muestra cómo implementar BranchCache en modos de caché distribuida y hospedada para optimizar el uso de ancho de banda WAN en sucursales
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-bc
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 9c465a9e-c504-44ec-9ebc-4e06ba54db30
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 33ed40ef91d9389bb7940dcf928cba43f0c9dbd2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="enable-branchcache-on-a-file-share-optional"></a>Habilitar BranchCache en un recurso compartido de archivos (opcional)

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este procedimiento para habilitar BranchCache en un recurso compartido de archivos.  
  
> [!IMPORTANT]  
> No es necesario realizar este procedimiento si configuras la configuración de publicación de hash con el valor **permitir la publicación de hash para todas las carpetas compartidas**.  
  
Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para realizar este procedimiento.  
  
### <a name="to-enable-branchcache-on-a-file-share"></a>Para habilitar BranchCache en un recurso compartido de archivos  
  
1.  Abre Windows PowerShell, escribe **mmc**, y, a continuación, presione ENTRAR. Abre Microsoft Management Console (MMC).  
  
2.  En la consola de MMC, en la **archivo** menú, haz clic en **agregar o quitar complemento**. La **agregar o quitar complementos** abre el cuadro de diálogo.  
  
3.  En **agregar o quitar complementos**, en **complementos disponibles**, haz doble clic en **carpetas compartidas**. Se abrirá el Asistente de carpetas compartidas con el objeto de equipo Local seleccionado. Configurar la vista que prefieras, haz clic en **finalizar**y, a continuación, haz clic en **Aceptar**.  
  
4.  Haz doble clic en **carpetas compartidas (locales)**y, a continuación, haz clic en **recursos compartidos**.  
  
5.  En el panel de detalles, haz clic en un recurso compartido y, a continuación, haz clic en **propiedades**. La cuota **propiedades** abre el cuadro de diálogo.  
  
6.  En la **propiedades** cuadro de diálogo, en la **General**, haga clic **configuración sin conexión**. La **configuración sin conexión** abre el cuadro de diálogo.  
  
7.  Asegúrate de que **solo los archivos y programas que los usuarios especificar están disponibles sin conexión** está seleccionado y, a continuación, haz clic en **habilitar BranchCache**.  
  
8.  Haz clic en **Aceptar** dos veces.  
  

