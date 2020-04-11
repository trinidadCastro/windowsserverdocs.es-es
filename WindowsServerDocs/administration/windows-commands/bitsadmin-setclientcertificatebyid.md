---
title: bitsadmin setclientcertificatebyid
description: Tema de comandos de Windows para **bitsadmin setclientcertificatebyid**, que especifica el identificador del certificado de cliente que se va a usar para la autenticación de cliente en una solicitud HTTPS (SSL)
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8585a7a1-7472-437b-b04a-a11925782a3a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 376bb850664a5ed569488634029cb7384856f158
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123048"
---
# <a name="bitsadmin-setclientcertificatebyid"></a>bitsadmin setclientcertificatebyid

Especifica el identificador del certificado de cliente que se va a usar para la autenticación de cliente en una solicitud HTTPS (SSL).

## <a name="syntax"></a>Sintaxis

```
bitsadmin /setclientcertificatebyid <job> <store_location> <store_name> <hexadecimal_cert_id>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |
| store_location | Identifica la ubicación de un almacén del sistema que se va a usar para buscar el certificado, incluidos:<ul><li>CURRENT_USER</li><li>LOCAL_MACHINE</li><li>CURRENT_SERVICE</li><li>Server</li><li>USUARIOS</li><li>CURRENT_USER_GROUP_POLICY</li><li>LOCAL_MACHINE_GROUP_POLICY</li><li>LOCAL_MACHINE_ENTERPRISE.</li></ul> |
| store_name | El nombre del almacén de certificados, incluido:<ul><li>CA (certificados de entidad de certificación)</li><li>MIS (certificados personales)</li><li>RAÍZ (certificados raíz)</li><li>SPC (certificado de editor de software).</li></ul> |
| hexadecimal_cert_id | Número hexadecimal que representa el hash del certificado. |

## <a name="examples"></a>Ejemplos

En el ejemplo siguiente se especifica el identificador del certificado de cliente que se va a utilizar para la autenticación del cliente en una solicitud HTTPS (SSL) para el trabajo denominado *myDownloadJob*.

```
C:\>bitsadmin /setclientcertificatebyid myDownloadJob BG_CERT_STORE_LOCATION_CURRENT_USER MY A106B52356D3FBCD1853A41B619358BD
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)