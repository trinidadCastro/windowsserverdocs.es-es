---
title: Crear un registro de alias (CNAME) en DNS para WEB1
description: Este tema forma parte de la guía de implementación de certificados de servidor para las implementaciones cableadas e inalámbricas de 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: bfae23f0-ae12-486b-94fe-50a137e141a5
ms.prod: windows-server
ms.technology: networking
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 0ac6ef7da3c9e604d9fa3c029f1afa2861a56741
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318336"
---
# <a name="create-an-alias-cname-record-in-dns-for-web1"></a>Crear un alias \(registro CNAME\) en DNS para WEB1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este procedimiento para agregar un nombre canónico de alias \(registro de recurso CNAME\) para el servidor Web a una zona en DNS en el controlador de dominio. Con los registros CNAME, puede usar más de un nombre para apuntar a un solo host, lo que facilita hacer cosas como hospedar un File Transfer Protocol \(servidor\) FTP y un servidor Web en el mismo equipo.   
  
Por este motivo, puede usar el servidor web para hospedar la lista de revocación de certificados \(CRL\) para la entidad de certificación \(CA\) así como para realizar servicios adicionales, como FTP o servidor Web.  
  
Cuando realice este procedimiento, reemplace **nombre de alias** y otras variables por valores adecuados para su implementación.  
  
Para llevar a cabo este procedimiento, debe ser miembro del **grupo Admins**. del dominio.  
  
## <a name="to-add-an-alias-cname-resource-record-to-a-zone"></a>Para agregar un alias \(CNAME\) registro de recursos a una zona  
  
>[!NOTE]  
>Para realizar este procedimiento con Windows PowerShell, consulte [Add-DnsServerResourceRecordCName](https://technet.microsoft.com/library/jj649894(v=wps.630).aspx).  
  
1.  En DC1, en Administrador del servidor, haga clic en **herramientas** y, a continuación, haga clic en **DNS**. Se abre Microsoft Management Console (MMC) del administrador de DNS.  
  
2.  En el árbol de consola, haga doble clic en **zonas de búsqueda directa**, haga clic con el botón secundario en la zona de búsqueda directa en la que desea agregar el registro de recursos de alias y, a continuación, haga clic en **nuevo Alias \(CNAME\)** . Se abrirá el cuadro de diálogo **nuevo registro de recursos** .  
  
3.  En **nombre de alias**, escriba el nombre de alias **PKI**.  
  
4.  Cuando se escribe un valor para **nombre de alias**, el **nombre de dominio completo \(FQDN\)** se rellena automáticamente en el cuadro de diálogo. Por ejemplo, si el nombre de alias es "PKI" y su dominio es corp.contoso.com, el valor **PKI.Corp.contoso.com** se rellena automáticamente.  
  
5.  En **nombre de dominio completo \(FQDN\) para el host de destino**, escriba el FQDN del servidor Web. Por ejemplo, si el servidor Web se denomina WEB1 y el dominio es corp.contoso.com, escriba **web1.Corp.contoso.com**.  
  
6.  Haga clic en **Aceptar** para agregar el nuevo registro a la zona.  
  

