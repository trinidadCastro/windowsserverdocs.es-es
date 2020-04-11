---
title: bitsadmin setclientcertificatebyname
description: Windows Commands topic for **bitsadmin setclientcertificatebyname**, que especifica el nombre de sujeto del certificado de cliente que se va a usar para la autenticación de cliente en una solicitud HTTPS (SSL).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f308a6d9-d0da-48be-ae41-eced14b3cccb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f2308bb5331f1555965b278a64bb7ab95e03779b
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123058"
---
# <a name="bitsadmin-setclientcertificatebyname"></a>bitsadmin setclientcertificatebyname

Especifica el nombre de sujeto del certificado de cliente que se va a usar para la autenticación de cliente en una solicitud HTTPS (SSL).

## <a name="syntax"></a>Sintaxis

```
bitsadmin /setclientcertificatebyname <job> <store_location> <store_name> <subject_name>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |
| store_location | Identifica la ubicación de un almacén del sistema que se va a usar para buscar el certificado. Los valores posibles son:<ul><li>1 (CURRENT_USER)</li><li>2 (LOCAL_MACHINE)</li><li>3 (CURRENT_SERVICE)</li><li>4 (SERVICIOS)</li><li>5 (USUARIOS)</li><li>6 (CURRENT_USER_GROUP_POLICY)</li><li>7 (LOCAL_MACHINE_GROUP_POLICY)</li><li>8 (LOCAL_MACHINE_ENTERPRISE)</li></ul> |
| store_name | Nombre del almacén de certificados. Los valores posibles son:<ul><li>CA (certificados de entidad de certificación)</li><li>MIS (certificados personales)</li><li>RAÍZ (certificados raíz)</li><li>SPC (certificado de editor de software)</li></ul> |
| subject_name | Nombre del certificado. |

## <a name="examples"></a>Ejemplos

En el ejemplo siguiente se especifica el nombre *del certificado de* cliente que se va a utilizar para la autenticación del cliente en una solicitud HTTPS (SSL) para el trabajo denominado *myDownloadJob*.

```
C:\>bitsadmin /setclientcertificatebyname myDownloadJob 1 MY myCertificate
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)