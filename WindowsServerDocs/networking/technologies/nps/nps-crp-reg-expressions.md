---
title: Usar expresiones regulares de NPS
description: En este tema se explica el uso de expresiones regulares para la coincidencia de patrones en NPS en Windows Server 2016. Puede usar esta sintaxis para especificar las condiciones de los atributos de directiva de red y dominios Kerberos de RADIUS.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: bc22d29c-678c-462d-88b3-1c737dceca75
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a985df9fea31e5ee180caef4e69899ae8468ff71
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865266"
---
# <a name="use-regular-expressions-in-nps"></a>Usar expresiones regulares de NPS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se explica el uso de expresiones regulares para la coincidencia de patrones en NPS en Windows Server 2016. Puede usar esta sintaxis para especificar las condiciones de los atributos de directiva de red y dominios Kerberos de RADIUS.

## <a name="pattern-matching-reference"></a>Referencia de la coincidencia de patrones

Puede usar la tabla siguiente como un origen de referencia al crear expresiones regulares con sintaxis de coincidencia de patrones.

|Carácter|Descripción|Ejemplo|
|---------|-----------|-------|
|`\`  |Marca el siguiente carácter como un carácter de coincidencia. |`/n/ matches the character "n". The sequence /\n/ matches a line feed or newline character.`  |
|`^`  |Coincide con el principio de la entrada o línea. | &nbsp; |
|`$`  |Coincide con el final de la entrada o línea. | &nbsp; |
|`*`  |Coincide con el carácter anterior cero o más veces. |`/zo*/ matches either "z" or "zoo."` |
|`+`  |Coincide con el carácter anterior una o varias veces. |`/zo+/ matches "zoo" but not "z."` |
|`?`  |Coincide con los tiempos de carácter cero o uno anterior. |`/a?ve?/ matches the "ve" in "never."` |
|`.`  |Coincide con cualquier carácter excepto con un carácter de nueva línea.  | &nbsp; |
|`(pattern)`  |Coincide con "pattern" y recuerda a la coincidencia.<br />Para que coincida con los caracteres literales `(` y `)` (paréntesis), utilice `\(` o `\)`.   | &nbsp;  |
|`x|y `  |Coincide con x o y.  |`/z|food?/ matches "zoo" or "food."` |
|`{n} `  |Coincide exactamente n veces \(n es un no\-entero negativo\).  |`/o{2}/ does not match the "o" in "Bob," but matches the first two instances of the letter o in "foooood."`  |
|`{n,}`  |Coincide con al menos n veces \(n es un no\-entero negativo\).  |`/o{2,}/ does not match the "o" in "Bob" but matches all of the instances of the letter o in "foooood." /o{1,}/ is equivalent to /o+/.`  |
|`{n,m}`  |Coincide con al menos n y a lo sumo veces m \(m y n son no\-enteros negativos\).  |`/o{1,3}/ matches the first three instances of the letter o in "fooooood."`  |
|`[xyz]`  |Coincide con cualquiera de los caracteres incluidos \(un juego de caracteres\).  |`/[abc]/ matches the "a" in "plain."`  |
|`[^xyz]`  |Coincide con cualquier carácter que no se entrecomillan \(un juego de caracteres negativos\).  |`/[^abc]/ matches the "p" in "plain."`  |
|`\b`  |Coincide con un límite de palabras \(por ejemplo, un espacio\).  |`/ea*r\b/ matches the "er" in "never early."`  |
|`\B`  |Coincide con un límite de palabra.  |`/ea*r\B/ matches the "ear" in "never early."`  |
|`\d`  |Coincide con un carácter de dígito \(equivalente a dígitos del 0 al 9\).  | &nbsp; |
|`\D`  |Coincide con un carácter de dígito \(equivalente a `[^0-9]` \).  | &nbsp; |
|`\f`  |Coincide con un carácter de avance.  | &nbsp; |
|`\n`  |Coincide con un salto de línea.  | &nbsp; |
|`\r`  |Coincide con un carácter de retorno de carro.  | &nbsp; |
|`\s`  |Coincide con cualquier carácter de espacio en blanco incluidos espacio, tabulación y avance de página \(equivalente a `[ \f\n\r\t\v]` \).  | &nbsp; |
|`\S`  |Coincide con cualquier carácter que no sea espacio en blanco \(equivalente a `[^ \f\n\r\t\v]` \).  | &nbsp; |
|`\t`  |Coincide con un carácter de tabulación.  | &nbsp; |
|`\v`  |Coincide con un carácter de tabulación vertical.  | &nbsp; |
|`\w`  |Coincide con cualquier carácter de palabra, incluido el subrayado \(equivalente a `[A-Za-z0-9_]` \).  | &nbsp; |
|`\W`  |Coincidencias no\-carácter de palabra, excluido el carácter de subrayado \(equivalente a `[^A-Za-z0-9_]` \).  | &nbsp; |
|`\num`  |Hace referencia a las coincidencias memorizadas \( `?num`, donde num es un entero positivo\).  Esta opción puede utilizarse únicamente en el **reemplazar** cuadro de texto al configurar la manipulación de atributos.| `\1` reemplaza lo que se almacena en la primera coincidencia memorizada.  |
|`/n/ `  |Permite la inserción de códigos ASCII en expresiones regulares \( `?n`, donde n es un valor de escape octal, hexadecimal o decimal\).  | &nbsp; |

## <a name="examples-for-network-policy-attributes"></a>Ejemplos de atributos de directiva de red

Los ejemplos siguientes describen el uso de la sintaxis de coincidencia de patrón para especificar los atributos de directiva de red:

- Para especificar todos los números de teléfono en el código de área 899, la sintaxis es:

     `899.*`

- Para especificar un intervalo de direcciones IP que empiezan por 192.168.1, la sintaxis es:

    `192\.168\.1\..+`

## <a name="examples-for-manipulation-of-the-realm-name-in-the-user-name-attribute"></a>Ejemplos de tratamiento del nombre de dominio Kerberos en el atributo de nombre de usuario

Los ejemplos siguientes describen el uso de la sintaxis de coincidencia de patrón para manipular los nombres de dominio Kerberos para el atributo de nombre de usuario, que se encuentra en la **atributo** ficha en las propiedades de una directiva de solicitud de conexión.

**Para quitar la parte del dominio Kerberos del atributo de nombre de usuario**

En un escenario de acceso telefónico externo en el que un Internet proveedor de servicios \(ISP\) enruta las solicitudes de conexión a una organización NPS, proxy RADIUS del ISP requiera un nombre de dominio Kerberos para enrutar la solicitud de autenticación. Sin embargo, el NPS no reconozca la parte del nombre de dominio Kerberos del nombre de usuario. Por lo tanto, el nombre de dominio Kerberos debe quitarse el proxy RADIUS del ISP antes de que se reenvíe a la organización de NPS.

- Buscar: @microsoft \.com

- Reemplazar:

**Para reemplazar *user@example.microsoft.com* con *example.microsoft.com\user***

- Buscar:`(.*)@(.*)`

- Reemplazar:`$2\$1`



**Para reemplazar *dominio\usuario* con *specific_domain\user***

- Buscar:`(.*)\\(.*)`

- Reemplazar: *dominio_específico*`\$2`



**Para reemplazar *usuario* con *user@specific_domain***

- Buscar:`$`

- Reemplazar: @*dominio_específico*

## <a name="example-for-radius-message-forwarding-by-a-proxy-server"></a>Ejemplo de reenvío de mensajes RADIUS por un servidor proxy

Puede crear reglas de enrutamiento que reenvíen mensajes RADIUS con un nombre de dominio Kerberos especificado a un conjunto de servidores RADIUS cuando NPS se usa como proxy RADIUS. Siguiente es la sintaxis recomendada para enrutar las solicitudes según el nombre de dominio Kerberos.

- **Nombre de NetBIOS**: `WCOAST`
- **Patrón**:      `^wcoast\\`

En el siguiente ejemplo, wcoast.microsoft.com es un sufijo del nombre principal (UPN) único para el wcoast.microsoft.com de dominio DNS o Active Directory. Mediante el patrón suministrado, el proxy NPS puede enrutar mensajes según el nombre NetBIOS del dominio o el sufijo UPN.

- **Nombre de NetBIOS**: `WCOAST`
- **Sufijo de UPN**:   `wcoast.microsoft.com`
- **Patrón**:      `^wcoast\\|@wcoast\.microsoft\.com$`


Para obtener más información sobre la administración de NPS, consulte [administrar un servidor de directivas de redes](nps-manage-top.md).

Para obtener más información acerca de NPS, consulte [servidor de directivas de redes (NPS)](nps-top.md).
