---
title: Crear un registro de alias (CNAME) en DNS para WEB1
description: Este tema forma parte de la guía de implementación de certificados de servidor para las implementaciones cableadas e inalámbricas de 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: bfae23f0-ae12-486b-94fe-50a137e141a5
ms.prod: windows-server
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 77b8e464d2d8fab8803477e59826c3e715c0a6d2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406298"
---
# <a name="create-an-alias-cname-record-in-dns-for-web1"></a>Creación de un registro de alias \(CNAME @ no__t-1 en DNS para WEB1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este procedimiento para agregar un nombre canónico de alias \(CNAME @ no__t-1 registro de recursos para el servidor Web a una zona en DNS en el controlador de dominio. Con los registros CNAME, puede usar más de un nombre para señalar a un solo host, lo que facilita hacer cosas como hospedar un servidor File Transfer Protocol \(FTP @ no__t-1 y un servidor Web en el mismo equipo.   
  
Por este motivo, es libre de usar el servidor web para hospedar la lista de revocación de certificados \(CRL @ no__t-1 para la entidad de certificación \(CA @ no__t-3, así como para realizar servicios adicionales, como FTP o servidor Web.  
  
Cuando realice este procedimiento, reemplace **nombre de alias** y otras variables por valores adecuados para su implementación.  
  
Para llevar a cabo este procedimiento, debe ser miembro del **grupo Admins**. del dominio.  
  
## <a name="to-add-an-alias-cname-resource-record-to-a-zone"></a>Para agregar un alias \(CNAME @ no__t-1 registro de recursos a una zona  
  
>[!NOTE]  
>Para realizar este procedimiento con Windows PowerShell, consulte [Add-DnsServerResourceRecordCName](https://technet.microsoft.com/library/jj649894(v=wps.630).aspx).  
  
1.  En DC1, en Administrador del servidor, haga clic en **herramientas** y, a continuación, haga clic en **DNS**. Se abre Microsoft Management Console (MMC) del administrador de DNS.  
  
2.  En el árbol de consola, haga doble clic en **zonas de búsqueda directa**, haga clic con el botón secundario en la zona de búsqueda directa en la que desea agregar el registro de recursos de alias y, a continuación, haga clic en **nuevo alias \(CNAME @ no__t-3**. Se abrirá el cuadro de diálogo **nuevo registro de recursos** .  
  
3.  En **nombre de alias**, escriba el nombre de alias **PKI**.  
  
4.  Cuando se escribe un valor para **nombre de alias**, el **nombre de dominio completo \(FQDN @ no__t-3** se rellena automáticamente en el cuadro de diálogo. Por ejemplo, si el nombre de alias es "PKI" y su dominio es corp.contoso.com, el valor **PKI.Corp.contoso.com** se rellena automáticamente.  
  
5.  En **nombre de dominio completo \(FQDN @ no__t-2 para el host de destino**, escriba el FQDN del servidor Web. Por ejemplo, si el servidor Web se denomina WEB1 y el dominio es corp.contoso.com, escriba **web1.Corp.contoso.com**.  
  
6.  Haga clic en **Aceptar** para agregar el nuevo registro a la zona.  
  

