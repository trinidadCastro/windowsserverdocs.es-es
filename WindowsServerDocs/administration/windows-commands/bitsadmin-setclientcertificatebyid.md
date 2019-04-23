---
title: Bitsadmin setclientcertificatebyid
description: Tema de los comandos de Windows para **setclientcertificatebyid bitsadmin** especifica el identificador del certificado de cliente que se usará para la autenticación del cliente en una solicitud de HTTPS (SSL)
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8585a7a1-7472-437b-b04a-a11925782a3a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2424de18ee8aaec73b086207e8ef56d85df862fa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863936"
---
# <a name="bitsadmin-setclientcertificatebyid"></a>Bitsadmin setclientcertificatebyid



Especifica el identificador del certificado de cliente que se usará para la autenticación del cliente en una solicitud de HTTPS (SSL).

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetClientCertificateByID <Job> <store_location> <store_name> hexa-decimal_cert_id>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|Nombre para mostrar o el GUID del trabajo|
|Store_location|Identifica la ubicación de un almacén del sistema que se utilizará para buscar el certificado. Los valores posibles son:</br>1 (CURRENT_USER)</br>2 (LOCAL_MACHINE)</br>3 (CURRENT_SERVICE)</br>4 (SERVICIOS)</br>5 (USUARIOS)</br>6 (CURRENT_USER_GROUP_POLICY)</br>7 (LOCAL_MACHINE_GROUP_POLICY)</br>8 (LOCAL_MACHINE_ENTERPRISE)|
|Store_name|El nombre del almacén de certificados. Los valores posibles son:</br>Entidad emisora de certificados (certificados de entidad de certificación)</br>(Los certificados personales)</br>RAÍZ (certificados raíz)</br>SPC (certificado de publicador de Software)|
|Hexadecimal_cert_id|Un número hexadecimal que representa el hash del certificado|

## <a name="BKMK_examples"></a>Ejemplos

El ejemplo siguiente especifica el identificador del certificado de cliente que se usará para la autenticación del cliente en una solicitud de HTTPS (SSL) para el trabajo denominado *myJob*.
```
C:\>bitsadmin Bitsadmin /SetClientCertificateByID myJob BG_CERT_STORE_LOCATION_CURRENT_USER MY A106B52356D3FBCD1853A41B619358BD 
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)