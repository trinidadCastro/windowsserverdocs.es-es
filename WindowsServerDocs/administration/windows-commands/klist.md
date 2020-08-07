---
title: klist
description: Artículo de referencia para el comando klist, que muestra una lista de vales de Kerberos en caché actualmente.
ms.topic: article
ms.assetid: 4689b4a9-1740-47dd-9240-02105efca428
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2e37946106d7c47f058fd42b9926e388ab830e47
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87888181"
---
# <a name="klist"></a>klist

Muestra una lista de vales de Kerberos en caché actualmente.

> [!IMPORTANT]
> Debe ser al menos un **Administrador de dominio**, o equivalente, para ejecutar todos los parámetros de este comando.

## <a name="syntax"></a>Sintaxis

```
klist [-lh <logonID.highpart>] [-li <logonID.lowpart>] tickets | tgt | purge | sessions | kcd_cache | get | add_bind | query_bind | purge_bind
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| -LH | Denota la parte alta del identificador local único (LUID) del usuario, expresada en formato hexadecimal. Si no hay ninguna **– LH** ni **– Li** , el comando toma como valor predeterminado el LUID del usuario que ha iniciado sesión actualmente. |
| -Li | Denota la parte baja del identificador local único (LUID) del usuario, expresada en formato hexadecimal. Si no hay ninguna **– LH** ni **– Li** , el comando toma como valor predeterminado el LUID del usuario que ha iniciado sesión actualmente. |
| vales | Enumera los vales de concesión de vales (TGT) almacenados en caché y los vales de servicio de la sesión de inicio de sesión especificada. Ésta es la opción predeterminada. |
| TGT | Muestra el TGT inicial de Kerberos. |
| purga | Permite eliminar todos los vales de la sesión de inicio de sesión especificada. |
| sesiones | Muestra una lista de sesiones de inicio de sesión en este equipo. |
| kcd_cache | Muestra la información de la caché de delegación restringida de Kerberos. |
| get | Permite solicitar un vale al equipo de destino especificado por el nombre de entidad de seguridad de servicio (SPN). |
| add_bind | Permite especificar un controlador de dominio preferido para la autenticación Kerberos. |
| query_bind | Muestra una lista de controladores de dominio preferidos almacenados en caché para cada dominio en el que se ha establecido contacto con Kerberos. |
| purge_bind | Quita los controladores de dominio preferidos almacenados en caché para los dominios especificados. |
| kdcoptions | Muestra las opciones de Centro de distribución de claves (KDC) especificadas en RFC 4120. |
| /? | Muestra ayuda para este comando. |

#### <a name="remarks"></a>Observaciones

- Si no se proporciona ningún parámetro, **klist** recupera todos los vales del usuario que ha iniciado la sesión actual.

- Los parámetros muestran la siguiente información:

  - **tickets** : enumera los vales en caché de servicios que ha autenticado desde el inicio de sesión. Muestra los siguientes atributos de todos los vales almacenados en caché:

    - **LogonID:** LUID.

    - **Cliente de:** Concatenación del nombre de cliente y el nombre de dominio del cliente.

    - **Servidor:** Concatenación del nombre de servicio y el nombre de dominio del servicio.

    - **Tipo de cifrado de KerbTicket:** El tipo de cifrado que se usa para cifrar el vale de Kerberos.

    - **Marcas de vale:** Marcas de vale de Kerberos.

    - **Hora de Inicio:** Hora a partir de la cual el vale es válido.

    - **Hora de finalización:** La hora en que el vale deja de ser válido. Cuando un vale supera este tiempo, ya no se puede usar para autenticarse en un servicio o usarse para la renovación.

    - **Tiempo de renovación:** La hora a la que se requiere una nueva autenticación inicial.

    - **Tipo de clave de sesión:** Algoritmo de cifrado que se usa para la clave de sesión.

  - **TGT** : muestra el TGT inicial de Kerberos y los siguientes atributos del vale actualmente en caché:

    - **LogonID:** Identificado en hexadecimal.

    - **ServiceName:** krbtgt

    - **NombreDeDestino `<SPN>` :** krbtgt

    - **NombreDeDominio:** Nombre del dominio que emite el TGT.

    - **TargetDomainName:** Dominio al que se emite el TGT.

    - **AltTargetDomainName:** Dominio al que se emite el TGT.

    - **Marcas de vale:** Direcciones y acciones de destino y tipo.

    - **Clave de sesión:** Longitud de clave y algoritmo de cifrado.

    - **StartTime:** Hora del equipo local en la que se solicitó el vale.

    - **EndTime:** Hora en que el vale deja de ser válido. Cuando un vale supera este tiempo, ya no se puede usar para autenticarse en un servicio.

    - **RenewUntil:** Fecha límite para la renovación del vale.

    - **TimeSkew:** Diferencia horaria con el Centro de distribución de claves (KDC).

    - **EncodedTicket:** Vale codificado.

  - **Purge** : permite eliminar un vale específico. La purga de vales destruye todos los vales que se han almacenado en caché, por lo que debe usar este atributo con precaución. Puede impedir que pueda autenticarse en los recursos. Si esto sucede, tendrá que cerrar la sesión y volver a iniciarla.

    - **LogonID:** Identificado en hexadecimal.

  - **sesiones** : permite enumerar y mostrar la información de todas las sesiones de inicio de sesión en este equipo.

    - **LogonID:** Si se especifica, solo se muestra la sesión de inicio de sesión por el valor especificado. Si no se especifica, se muestran todas las sesiones de inicio de sesión en este equipo.

  - **kcd_cache** : permite mostrar la información de la memoria caché de delegación restringida de Kerberos.

    - **LogonID:** Si se especifica, muestra la información de la memoria caché de la sesión de inicio de sesión por el valor especificado. Si no se especifica, muestra la información de la memoria caché de la sesión de inicio de sesión del usuario actual.

  - **Get** : permite solicitar un vale para el destino especificado por el SPN.

    - **LogonID:** Si se especifica, solicita un vale mediante la sesión de inicio de sesión por el valor especificado. Si no se especifica, solicita un vale mediante la sesión de inicio de sesión del usuario actual.

    - **kdcoptions:** Solicita un vale con las opciones de KDC especificadas

  - **add_bind** : permite especificar un controlador de dominio preferido para la autenticación Kerberos.

  - **query_bind** : permite mostrar controladores de dominio preferidos en caché para los dominios.

  - **purge_bind** : permite quitar controladores de dominio preferidos en caché para los dominios.

  - **kdcoptions** : para obtener la lista actual de opciones y sus explicaciones, consulte [RFC 4120](http://www.ietf.org/rfc/rfc4120.txt).

### <a name="examples"></a>Ejemplos

Para consultar la memoria caché de vales de Kerberos para determinar si faltan vales, si el servidor o la cuenta de destino tienen errores, o si no se admite el tipo de cifrado debido a un error con el identificador de evento 27, escriba:

```
klist
```

```
klist –li 0x3e7
```

Para obtener información sobre los detalles de cada vale de concesión de vales que se almacena en la memoria caché del equipo para una sesión de inicio de sesión, escriba:

```
klist tgt
```

Para purgar la caché de vales de Kerberos, cierre la sesión y vuelva a iniciarla, escriba:

```
klist purge
```

```
klist purge –li 0x3e7
```

Para diagnosticar una sesión de inicio de sesión y encontrar un logonID para un usuario o un servicio, escriba:

```
klist sessions
```

Para diagnosticar un error de delegación restringida de Kerberos y encontrar el último error encontrado, escriba:

```
klist kcd_cache
```

Para diagnosticar si un usuario o un servicio puede obtener un vale para un servidor, o para solicitar un vale para un SPN específico, escriba:

```
klist get host/%computername%
```

Para diagnosticar problemas de replicación entre controladores de dominio, normalmente necesita el equipo cliente para un controlador de dominio específico. Para dirigir el equipo cliente al controlador de dominio específico, escriba:

```
klist add_bind CONTOSO KDC.CONTOSO.COM
```

```
klist add_bind CONTOSO.COM KDC.CONTOSO.COM
```

Para consultar los controladores de dominio a los que se ha puesto en contacto recientemente este equipo, escriba:

```
klist query_bind
```

Para volver a detectar controladores de dominio o vaciar la memoria caché antes de crear nuevos enlaces de controlador de dominio con `klist add_bind` , escriba:

```
klist purge_bind
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)