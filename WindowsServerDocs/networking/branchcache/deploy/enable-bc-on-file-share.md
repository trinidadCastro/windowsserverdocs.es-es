---
title: Habilitar BranchCache en un recurso compartido de archivos (opcional)
description: Este tema forma parte de la guía de implementación de BranchCache para Windows Server 2016, que muestra cómo implementar BranchCache en los modos de caché distribuida y hospedada para optimizar el uso del ancho de banda WAN en las sucursales.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 9c465a9e-c504-44ec-9ebc-4e06ba54db30
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 501fd1b1ac3b0bc7a0767ec57502ba18a8ee76e4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861088"
---
# <a name="enable-branchcache-on-a-file-share-optional"></a>Habilitar BranchCache en un recurso compartido de archivos (opcional)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este procedimiento para habilitar BranchCache en un recurso compartido de archivos.  
  
> [!IMPORTANT]  
> No es necesario realizar este procedimiento si configura el valor de publicación de hash con el valor **permitir publicación de hash para todas las carpetas compartidas**.  
  
El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o un grupo equivalente.  
  
### <a name="to-enable-branchcache-on-a-file-share"></a>Para habilitar BranchCache en un recurso compartido de archivos  
  
1.  Abra Windows PowerShell, escriba **mmv** y presione ENTRAR. Se abrirá Microsoft Management Console (MMC).  
  
2.  En MMC, en el menú **Archivo**, haga clic en **Agregar o quitar complemento**. Se abre el cuadro de diálogo **Agregar o quitar complementos**.  
  
3.  En **Agregar o quitar complementos**, en **complementos disponibles**, haga doble clic en **carpetas compartidas**. Se abre el Asistente para carpetas compartidas con el objeto equipo local seleccionado. Configure la vista que prefiera, haga clic en **Finalizar**y, a continuación, haga clic en **Aceptar**.  
  
4.  Haga doble clic en **carpetas compartidas (local)** y, a continuación, haga clic en **recursos compartidos**.  
  
5.  En el panel de detalles, haga clic con el botón secundario en un recurso compartido y, a continuación, haga clic en **propiedades**. Se abrirá el cuadro de diálogo **propiedades** del recurso compartido.  
  
6.  En el cuadro de diálogo **propiedades** , en la ficha **General** , haga clic en **configuración sin conexión**. Se abre el cuadro de diálogo **configuración sin conexión** .  
  
7.  Asegúrese de que **solo los archivos y programas que los usuarios especifican están disponibles sin conexión** y, a continuación, haga clic en **Habilitar BranchCache**.  
  
8.  Haga clic en **Aceptar** dos veces.  
  

