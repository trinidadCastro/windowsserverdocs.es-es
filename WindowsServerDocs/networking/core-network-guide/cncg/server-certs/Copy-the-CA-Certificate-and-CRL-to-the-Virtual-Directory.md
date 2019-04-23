---
title: Copie el certificado de CA y la CRL en el directorio Virtual
description: Este tema forma parte de la Guía de implementación de servidores de certificados para las implementaciones inalámbricas y cableadas 802.1X
manager: dougkim
ms.topic: article
ms.assetid: a1b5fa23-9cb1-4c32-916f-2d75f48b42c7
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.date: 07/19/2018
ms.openlocfilehash: 15cc807db805e1be0349ea51119663e515c7e551
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874036"
---
# <a name="copy-the-ca-certificate-and-crl-to-the-virtual-directory"></a>Copie el certificado de CA y la CRL en el directorio Virtual

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este procedimiento para copiar el certificado de CA raíz de la lista de revocación de certificados y Enterprise de la entidad de certificación a un directorio virtual en el servidor Web y para asegurarse de que AD CS esté configurado correctamente. Antes de ejecutar los comandos siguientes, asegúrese de sustituir los nombres de directorio y el servidor con aquéllos que sean adecuados para su implementación.  
  
Para realizar este procedimiento debe ser miembro de **Admins. del dominio**.  
  
#### <a name="to-copy-the-certificate-revocation-list-from-ca1-to-web1"></a>Para copiar la lista de revocación de certificados de CA1 a WEB1  
  
1.  En CA1, ejecute Windows PowerShell como administrador y, a continuación, publicar la CRL con el siguiente comando:  
  
    - Escriba `certutil -crl` y presione ENTRAR.  

    - Para copiar las listas de revocación de certificados en el recurso compartido de archivos en el servidor Web, escriba `copy C:\Windows\system32\certsrv\certenroll\*.crl \\WEB1\pki`, y, a continuación, presione ENTRAR.  
  
2.  Para comprobar que las ubicaciones de CDP y AIA extensión están configuradas correctamente, escriba `pkiview.msc`, y, a continuación, presione ENTRAR. Pkiview se abre MMC de PKI de empresa.  
  
3.  En el panel izquierdo, haga clic en el nombre de CA.<p>Por ejemplo, si el nombre de entidad de certificación es corp-CA1-CA, haga clic en **CA de corp-CA1**. 

4. En la columna de estado del panel de resultados, compruebe que se muestran los valores de la siguiente **Aceptar**:

    - **Certificado de CA**
    - **Ubicación de AIA #1**
    - **Ubicación de CDP #1**   
  
  
> [!TIP]  
> Si **estado** para cualquier elemento no es **Aceptar**, realice lo siguiente:  
> -   Abra el recurso compartido en el servidor Web para comprobar que el certificado y los archivos de lista de revocación de certificados se han copiado correctamente en el recurso compartido. Si no se copiaron correctamente en el recurso compartido, modifique los comandos de copia con el origen de archivo correcto y compartir destino y vuelva a ejecutar los comandos.  
> -   Compruebe que ha escrito las ubicaciones de CDP y AIA correctas en la pestaña Extensiones de CA. Asegúrese de que no hay ningún espacio adicional u otros caracteres en las ubicaciones que ha proporcionado.  
> -   Compruebe que copió el certificado de CA y de CRL en la ubicación correcta en el servidor Web, y que la ubicación coincida con la ubicación que ha proporcionado para las ubicaciones de CDP y AIA en la CA.  
> -   Compruebe que ha configurado correctamente los permisos para la carpeta virtual donde se almacenan el certificado de CA y CRL.  
  


