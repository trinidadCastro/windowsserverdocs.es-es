---
title: bitsadmin setclientcertificatebyid
description: Tema de referencia del comando bitsadmin setclientcertificatebyid, que especifica el identificador del certificado de cliente que se va a usar para la autenticación de cliente en una solicitud HTTPS (SSL).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8585a7a1-7472-437b-b04a-a11925782a3a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 24f5d0b9cda9fecc70611d8eaa21b0c8976c4c7c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719341"
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

Para especificar el identificador del certificado de cliente que se va a usar para la autenticación de cliente en una solicitud HTTPS (SSL) para el trabajo denominado *myDownloadJob*:

```
bitsadmin /setclientcertificatebyid myDownloadJob BG_CERT_STORE_LOCATION_CURRENT_USER MY A106B52356D3FBCD1853A41B619358BD
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
