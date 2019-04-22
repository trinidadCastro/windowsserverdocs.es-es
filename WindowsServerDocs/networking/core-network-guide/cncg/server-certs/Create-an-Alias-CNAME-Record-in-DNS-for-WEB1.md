---
title: Crear un registro de alias (CNAME) en DNS para WEB1
description: Este tema forma parte de la Guía de implementación de servidores de certificados para las implementaciones inalámbricas y cableadas 802.1X
manager: brianlic
ms.topic: article
ms.assetid: bfae23f0-ae12-486b-94fe-50a137e141a5
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 442ee8b70311eaad3f0b3f263003786b6beab8bc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813166"
---
# <a name="create-an-alias-cname-record-in-dns-for-web1"></a>Crear un Alias \(CNAME\) registros de DNS para WEB1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este procedimiento para agregar un nombre de Alias canónico \(CNAME\) registro de recursos de su servidor Web para una zona de DNS en el controlador de dominio. Con los registros CNAME, puede usar más de un nombre para que apunte a un solo host, lo que facilita realizar operaciones tales como host tanto un protocolo de transferencia de archivos \(FTP\) servidor y un servidor Web en el mismo equipo.   
  
Por este motivo, es libre de usar el servidor Web para hospedar la lista de revocación de certificados \(CRL\) para la entidad de certificación \(CA\) así como para realizar servicios adicionales, como el servidor FTP o Web.  
  
Al llevar a cabo este procedimiento, reemplace **nombre de Alias** y otras variables con los valores adecuados para su implementación.  
  
Para llevar a cabo este procedimiento, debe ser miembro de **Admins. del dominio**.  
  
## <a name="to-add-an-alias-cname-resource-record-to-a-zone"></a>Para agregar un alias \(CNAME\) registro de recursos a una zona  
  
>[!NOTE]  
>Para llevar a cabo este procedimiento con Windows PowerShell, consulte [agregar DnsServerResourceRecordCName](https://technet.microsoft.com/library/jj649894(v=wps.630).aspx).  
  
1.  En DC1, en el administrador del servidor, haga clic en **herramientas** y, a continuación, haga clic en **DNS**. Se abre el Administrador de DNS de Microsoft Management Console (MMC).  
  
2.  En el árbol de consola, haga doble clic en **zonas de búsqueda directa**, haga clic en la zona de búsqueda directa donde desea agregar el registro de recursos de Alias y, a continuación, haga clic en **nuevo Alias \(CNAME\)** . El **nuevo registro de recursos** abre el cuadro de diálogo.  
  
3.  En **nombre de Alias**, escriba el nombre de alias **pki**.  
  
4.  Al escribir un valor para **nombre de Alias**, **nombre de dominio completo \(FQDN\)**  llena automáticamente en el cuadro de diálogo. Por ejemplo, si el nombre del alias es "pki" y el dominio es corp.contoso.com, el valor **pki.corp.contoso.com** rellena automáticamente para usted.  
  
5.  En **nombre de dominio completo \(FQDN\) para el host de destino**, escriba el FQDN del servidor Web. Por ejemplo, si el servidor Web se denomina WEB1 y el dominio es corp.contoso.com, escriba **WEB1.corp.contoso.com**.  
  
6.  Haga clic en **Aceptar** para agregar el nuevo registro a la zona.  
  

