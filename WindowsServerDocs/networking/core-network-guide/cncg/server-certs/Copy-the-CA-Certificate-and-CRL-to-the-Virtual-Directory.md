---
title: Copia el certificado de CA y CRL en el directorio Virtual
description: Este tema es parte de la Guía de certificados de servidor de implementación para implementaciones de conexión inalámbrica y cableadas 802.1X
manager: brianlic
ms.topic: article
ms.assetid: a1b5fa23-9cb1-4c32-916f-2d75f48b42c7
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e37bfce7f8cf33fd7fcb5e6227d783c28bd29d35
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="copy-the-ca-certificate-and-crl-to-the-virtual-directory"></a>Copia el certificado de CA y CRL en el directorio Virtual

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este procedimiento para copiar el certificado de CA de empresa y la lista de revocación de certificados raíz de la entidad de certificación a un directorio virtual en el servidor Web y para garantizar que los AD CS está correctamente configurado. Antes de ejecutar los comandos a continuación, asegúrate de que reemplaza los nombres de directorio y server con los que son adecuados para la implementación.  
  
Para realizar este procedimiento debe ser miembro del **administradores de dominio**.  
  
#### <a name="to-copy-the-certificate-revocation-list-from-ca1-to-web1"></a>Para copiar la lista de revocación de certificados de CA1a WEB1  
  
1.  En CA1, ejecutar Windows PowerShell como administrador y, a continuación, publicar la CRL con el siguiente comando:  
  
    - Tipo `certutil -crl`, y, a continuación, presione ENTRAR.  
  
    - Para copiar el certificado de CA en el recurso compartido de archivos en el servidor Web, escriba `copy C:\Windows\system32\certsrv\certenroll\*.crt \\WEB1\pki`, y, a continuación, presione ENTRAR.  
    - Para copiar las listas de revocación de certificados en el recurso compartido de archivos en el servidor Web, escriba `copy C:\Windows\system32\certsrv\certenroll\*.crl \\WEB1\pki`, y, a continuación, presione ENTRAR.  
  
2. Para reiniciar los AD CS, escriba `Restart-Service certsvc`, y, a continuación, presione ENTRAR.  
  
2.  Para comprobar que las ubicaciones de extensión CDP y AIA están configuradas correctamente, escriba `pkiview.msc`, y, a continuación, presione ENTRAR. Pkiview se abre MMC de PKI de empresa.  
  
3.  Haz clic en el nombre de entidad emisora de certificados. Por ejemplo, si el nombre de entidad emisora de certificados es corp-CA1-CA, haz clic en **corp-CA1-CA**. En el panel de detalles, compruebe que la **estado** valor para la **certificado de CA**, **AIA ubicación #1**, y **CDP ubicación #1** son todas **Aceptar **.  
  
La siguiente ilustración muestra el panel de resultados de pkiview con un estado correcto para todos los elementos.  
  
! [adcs_pkiviewmedia/adcs_pkiview.png)  
  
> [!IMPORTANT]  
> Si **estado** para cualquier elemento no es **Aceptar**, haz lo siguiente:  
> -   Abre el recurso compartido en tu servidor Web para comprobar que se copiaron correctamente el certificado y los archivos de lista de revocación de certificados en el recurso compartido. Si no se copian correctamente en el recurso compartido, modificar los comandos de copiar con el origen de archivo correcta y compartir destino y volver a ejecutar los comandos.  
> -   Comprueba que has introducido las ubicaciones correctas para CDP y AIA en la ficha Extensiones de CA Asegúrate de que hay ningún espacio adicional u otros caracteres en las ubicaciones que ha proporcionado.  
> -   Comprueba que has copiado el certificado de CA y de CRL a la ubicación correcta en el servidor Web, y que la ubicación coincide con la ubicación que proporcionaste para las ubicaciones de CDP y AIA de la CA.  
> -   Comprueba que has configurado correctamente permisos para la carpeta virtual donde se almacenan el certificado de CA y CRL.  
  


