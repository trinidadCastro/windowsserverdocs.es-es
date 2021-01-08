---
title: Copiar el certificado de entidad de certificación y la CRL en el directorio virtual
description: Obtenga información acerca de cómo copiar la lista de revocación de certificados y el certificado de CA raíz de empresa de la entidad de certificación a un directorio virtual del servidor Web y para asegurarse de que AD CS está configurado correctamente.
manager: dougkim
ms.topic: article
ms.assetid: a1b5fa23-9cb1-4c32-916f-2d75f48b42c7
ms.author: lizross
author: eross-msft
ms.date: 07/19/2018
ms.openlocfilehash: 47d0e72b06d60b8865356d74c041a26f62635046
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2021
ms.locfileid: "98038565"
---
# <a name="copy-the-ca-certificate-and-crl-to-the-virtual-directory"></a>Copiar el certificado de entidad de certificación y la CRL en el directorio virtual

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este procedimiento para copiar la lista de revocación de certificados y el certificado de CA raíz de empresa de la entidad de certificación a un directorio virtual del servidor Web y para asegurarse de que AD CS esté configurado correctamente. Antes de ejecutar los siguientes comandos, asegúrese de reemplazar los nombres de los directorios y servidores por los que sean adecuados para su implementación.

Para llevar a cabo este procedimiento, debe ser miembro del **grupo Admins**. del dominio.

#### <a name="to-copy-the-certificate-revocation-list-from-ca1-to-web1"></a>Para copiar la lista de revocación de certificados de CA1 a WEB1

1.  En CA1, ejecute Windows PowerShell como administrador y, después, publique la CRL con el siguiente comando:

    - Escriba `certutil -crl` y presione ENTRAR.

    - Para copiar el certificado CA1 en el recurso compartido de archivos del servidor Web, escriba `copy C:\Windows\system32\certsrv\certenroll\*.crt \\WEB1\pki` y, a continuación, presione Entrar.

    - Para copiar las listas de revocación de certificados en el recurso compartido de archivos del servidor Web, escriba `copy C:\Windows\system32\certsrv\certenroll\*.crl \\WEB1\pki` y, a continuación, presione Entrar.

2.  Para comprobar que las ubicaciones de las extensiones de CDP y AIA están configuradas correctamente, escriba `pkiview.msc` y, a continuación, presione Entrar. Se abre el MMC de pkiview Enterprise PKI.

3.  En el panel izquierdo, haga clic en el nombre de la entidad de certificación.<p>Por ejemplo, si el nombre de la CA es Corp-CA1-CA, haga clic en **Corp-CA1-CA**.

4. En la columna Estado del panel de resultados, compruebe que los valores de lo siguiente muestran **correcto**:

    - **Certificado de CA**
    - **Ubicación de AIA #1**
    - **#1 de ubicación de CDP**


> [!TIP]
> Si el **Estado** de algún elemento no es **correcto**, haga lo siguiente:
> -   Abra el recurso compartido en el servidor web para comprobar que los archivos de lista de revocación de certificados y certificados se copiaron correctamente en el recurso compartido. Si no se copiaron correctamente en el recurso compartido, modifique los comandos de copia con el origen de archivo y el destino de recurso compartido correctos y vuelva a ejecutar los comandos.
> -   Compruebe que ha especificado las ubicaciones correctas para el CDP y AIA en la pestaña extensiones de CA. Asegúrese de que no hay espacios adicionales u otros caracteres en las ubicaciones que ha proporcionado.
> -   Compruebe que ha copiado la CRL y el certificado de CA en la ubicación correcta del servidor Web y que la ubicación coincide con la ubicación que proporcionó para las ubicaciones de CDP y AIA en la CA.
> -   Compruebe que ha configurado correctamente los permisos para la carpeta virtual donde se almacenan el certificado de CA y la CRL.



