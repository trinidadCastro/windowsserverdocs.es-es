---
title: Crear un registro de Alias (CNAME) en DNS para WEB1
description: Este tema es parte de la Guía de certificados de servidor de implementación para implementaciones de conexión inalámbrica y cableadas 802.1X
manager: brianlic
ms.topic: article
ms.assetid: bfae23f0-ae12-486b-94fe-50a137e141a5
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 65f10efe1cfd2866fb99406bf031197f6268b05b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="create-an-alias-cname-record-in-dns-for-web1"></a>Crear un registro de Alias \(CNAME\) en DNS para WEB1

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este procedimiento para agregar un registro de Alias nombre canónico \(CNAME\) recursos de tu servidor Web a una zona de DNS en el controlador de dominio. Con los registros CNAME, puedes usar más de un nombre para apuntar a un único host, lo que facilita a hacer cosas tales como host de un servidor de protocolo de transferencia de archivos \(FTP\) y un servidor Web en el mismo equipo.   
  
Por este motivo, eres libre de usar el servidor Web para hospedar la revocación de certificados \(CRL\) de lista para la entidad de certificación \(CA\), así como para realizar servicios adicionales, como FTP o un servidor Web.  
  
Al realizar este procedimiento, reemplazar **nombre Alias** y otras variables con valores que son adecuados para la implementación.  
  
Para realizar este procedimiento, debe ser miembro del **administradores de dominio**.  
  
## <a name="to-add-an-alias-cname-resource-record-to-a-zone"></a>Para agregar un registro de alias \(CNAME\) recursos a una zona  
  
>[!NOTE]  
>Para realizar este procedimiento mediante Windows PowerShell, consulta [agregar DnsServerResourceRecordCName](https://technet.microsoft.com/library/jj649894(v=wps.630).aspx).  
  
1.  En DC1, en el administrador del servidor, haz clic en **herramientas** y, a continuación, haz clic en **DNS**. Abre el Administrador de DNS de Microsoft Management Console (MMC).  
  
2.  En el árbol de consola, haz doble clic en **zonas de búsqueda directa**, haz clic en la zona de búsqueda directa donde quieres agregar el registro de Alias de recursos y, a continuación, haz clic en **nuevo Alias \(CNAME\)**. La **nuevo registro de recursos** abre el cuadro de diálogo.  
  
3.  En **nombre Alias**, escribe el nombre de alias **pki**.  
  
4.  Cuando escribes un valor para **nombre Alias**, la **\(FQDN\) de nombre de dominio completo** llena automáticamente en el cuadro de diálogo. Por ejemplo, si el nombre de alias es "pki" y el dominio es corp.contoso.com, el valor **pki.corp.contoso.com** es rellenará automáticamente para TI.  
  
5.  En **\(FQDN\) de nombre de dominio completo para el host de destino**, escriba el nombre completo del servidor Web. Por ejemplo, si tu servidor Web se denomina WEB1 y tu dominio es corp.contoso.com, escriba **WEB1.corp.contoso.com**.  
  
6.  Haz clic en **Aceptar** para agregar el nuevo registro a la zona.  
  

