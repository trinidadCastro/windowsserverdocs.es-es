---
title: bitsadmin setclientcertificatebyid
description: Tema de comandos de Windows para bitsadmin setclientcertificatebyid, que especifica el identificador del certificado de cliente que se va a usar para la autenticación de cliente en una solicitud HTTPS (SSL)
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8585a7a1-7472-437b-b04a-a11925782a3a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 80c97b21194c773d1b21aab2ee31794624da671c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849668"
---
# <a name="bitsadmin-setclientcertificatebyid"></a>bitsadmin setclientcertificatebyid

Especifica el identificador del certificado de cliente que se va a usar para la autenticación de cliente en una solicitud HTTPS (SSL).

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetClientCertificateByID <Job> <store_location> <store_name> hexa-decimal_cert_id>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|
|Store_location|Identifica la ubicación de un almacén del sistema que se va a usar para buscar el certificado. Los valores posibles son:</br>1 (CURRENT_USER)</br>2 (LOCAL_MACHINE)</br>3 (CURRENT_SERVICE)</br>4 (SERVICIOS)</br>5 (USUARIOS)</br>6 (CURRENT_USER_GROUP_POLICY)</br>7 (LOCAL_MACHINE_GROUP_POLICY)</br>8 (LOCAL_MACHINE_ENTERPRISE)|
|Store_name|Nombre del almacén de certificados. Los valores posibles son:</br>CA (certificados de entidad de certificación)</br>MIS (certificados personales)</br>RAÍZ (certificados raíz)</br>SPC (certificado de editor de software)|
|Hexadecimal_cert_id|Un número hexadecimal que representa el hash del certificado.|

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se especifica el identificador del certificado de cliente que se va a utilizar para la autenticación del cliente en una solicitud HTTPS (SSL) para el trabajo denominado *myJob*.
```
C:\>bitsadmin Bitsadmin /SetClientCertificateByID myJob BG_CERT_STORE_LOCATION_CURRENT_USER MY A106B52356D3FBCD1853A41B619358BD 
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)