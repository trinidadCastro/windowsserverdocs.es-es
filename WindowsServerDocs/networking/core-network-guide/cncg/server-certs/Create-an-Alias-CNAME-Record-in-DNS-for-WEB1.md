---
title: Crear un registro de alias (CNAME) en DNS para WEB1
description: Este tema forma parte de la guía de implementación de certificados de servidor para las implementaciones cableadas e inalámbricas de 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: bfae23f0-ae12-486b-94fe-50a137e141a5
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: c95efd5d9f0cbe7bbdba79d0971581f2c09ac53e
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97950191"
---
# <a name="create-an-alias-cname-record-in-dns-for-web1"></a>Crear un \( registro CNAME \) de alias en DNS para web1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este procedimiento para agregar un \( \) registro de recursos CNAME de nombre canónico de alias para el servidor Web a una zona en DNS en el controlador de dominio. Con los registros CNAME, puede usar más de un nombre para apuntar a un solo host, lo que facilita hacer cosas como hospedar un servidor File Transfer Protocol \( FTP \) y un servidor Web en el mismo equipo.

Por este motivo, puede usar el servidor web para hospedar la CRL de lista de revocación \( \) de certificados para la CA de la entidad de certificación, así \( \) como para realizar servicios adicionales, como FTP o servidor Web.

Cuando realice este procedimiento, reemplace **nombre de alias** y otras variables por valores adecuados para su implementación.

Para llevar a cabo este procedimiento, debe ser miembro del **grupo Admins**. del dominio.

## <a name="to-add-an-alias-cname-resource-record-to-a-zone"></a>Para agregar un \( \) registro de recurso CNAME de alias a una zona

>[!NOTE]
>Para realizar este procedimiento con Windows PowerShell, consulte [Add-DnsServerResourceRecordCName](/powershell/module/dnsserver/add-dnsserverresourcerecordcname?view=winserver2012r2-ps).

1.  En DC1, en Administrador del servidor, haga clic en **herramientas** y, a continuación, haga clic en **DNS**. Se abre Microsoft Management Console (MMC) del administrador de DNS.

2.  En el árbol de consola, haga doble clic en **zonas de búsqueda directa**, haga clic con el botón secundario en la zona de búsqueda directa en la que desea agregar el registro de recursos de alias y, a continuación, haga clic en **nuevo alias \( CNAME \)**. Se abrirá el cuadro de diálogo **nuevo registro de recursos** .

3.  En **nombre de alias**, escriba el nombre de alias **PKI**.

4.  Cuando se escribe un valor para **nombre de alias**, el **\( FQDN \) del nombre de dominio completo** se rellena automáticamente en el cuadro de diálogo. Por ejemplo, si el nombre de alias es "PKI" y su dominio es corp.contoso.com, el valor **PKI.Corp.contoso.com** se rellena automáticamente.

5.  En **FQDN de nombre de dominio completo \( para el host de \) destino**, escriba el FQDN del servidor Web. Por ejemplo, si el servidor Web se denomina WEB1 y el dominio es corp.contoso.com, escriba **web1.Corp.contoso.com**.

6.  Haga clic en **Aceptar** para agregar el nuevo registro a la zona.