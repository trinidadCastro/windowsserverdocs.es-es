---
title: Usa las expresiones regulares de NPS
description: Este tema explica el uso de las expresiones regulares de coincidencia en NPS en Windows Server 2016. Puedes usar esta sintaxis para especificar las condiciones de los atributos de directiva de red y territorios RADIUS.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: bc22d29c-678c-462d-88b3-1c737dceca75
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f84173f1f51be9fd44995dc41f759bbea4fb3539
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="use-regular-expressions-in-nps"></a>Usa las expresiones regulares de NPS

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Este tema explica el uso de las expresiones regulares de coincidencia en NPS en Windows Server 2016. Puedes usar esta sintaxis para especificar las condiciones de los atributos de directiva de red y territorios RADIUS.

## <a name="pattern-matching-reference"></a>Coincidencia de referencia

Puedes usar la siguiente tabla como un origen de la referencia al crear expresiones regulares con sintaxis de coincidencia.

|Carácter|Descripción|Ejemplo|
|---------|-----------|-------|
|`\`  |Marca el carácter siguiente como un carácter de coincidencia. |`/n/ matches the character "n". The sequence /\n/ matches a line feed or newline character.`  |
|`^`  |Coincide con el principio de la entrada o una línea. | &nbsp; |
|`$`  |Coincide con el final de la entrada o una línea. | &nbsp; |
|`*`  |Coincide con el carácter anterior cero o más veces. |`/zo*/ matches either "z" or "zoo."` |
|`+`  |Coincide con el carácter anterior una o varias veces. |`/zo+/ matches "zoo" but not "z."` |
|`?`  |Coincide con los tiempos de carácter cero o uno anterior. |`/a?ve?/ matches the "ve" in "never."` |
|`.`  |Coincide con cualquier carácter excepto un carácter.  | &nbsp; |
|`( pattern )`  |Coincide con "patrón" y recuerda a la coincidencia.   |`To match ( ) (parentheses), use "\(" or "\)".`  |
|' x | Y '  |Coincide con x e y.  |' / z|¿alimentos? / coincide con "zoo" o "comida". ` |
|`{ n } `  |Coincide exactamente n veces \ (n es un integer\ non\ negativo).  |`/o{2}/ does not match the "o" in "Bob," but matches the first two instances of the letter o in "foooood."`  |
|`{ n ,}`  |Coincide con al menos n veces \ (n es un integer\ non\ negativo).  |`/o{2,}/ does not match the "o" in "Bob" but matches all of the instances of the letter o in "foooood." /o{1,}/ is equivalent to /o+/.`  |
|`{ n , m }`  |Coincide con al menos n y como máximo m veces \ (m y n son integers\ non\ negativo).  |`/o{1,3}/ matches the first three instances of the letter o in "fooooood."`  |
|`[ xyz ]`  |Coincide con cualquiera de los caracteres incluidos \(a character set\).  |`/[abc]/ matches the "a" in "plain."`  |
|`[^ xyz ]`  |Coincide con los caracteres que no se enmarcan \ (Set un carácter negativo).  |`/[^abc]/ matches the "p" in "plain."`  |
|`\b`  |Coincide con un límite de word \ (por ejemplo, un space\).  |`/ea*r\b/ matches the "er" in "never early."`  |
|`\B`  |Coincide con un límite de palabra.  |`/ea*r\B/ matches the "ear" in "never early."`  |
|`\d`  |Coincide con un dígito \ (equivalente a dígitos de 0a 9\).  | &nbsp; |
|`\D`  |Coincide con un carácter de dígito \ (equivalente a `[^0-9]`\).  | &nbsp; |
|`\f`  |Coincide con un carácter de avance.  | &nbsp; |
|`\n`  |Coincidencias un salto de línea.  | &nbsp; |
|`\r`  |Coincide con un carácter de retorno de carro.  | &nbsp; |
|`\s`  |Coincide con cualquier carácter de espacio en blanco incluidos espacio, pestaña y avance \ (equivalente a `[ \f\n\r\t\v]`\).  | &nbsp; |
|`\S`  |Coincide con cualquier carácter sin espacios en blanco \ (equivalente a `[^ \f\n\r\t\v]`\).  | &nbsp; |
|`\t`  |Coincide con un carácter de tabulación.  | &nbsp; |
|`\v`  |Coincide con un carácter de tabulación vertical.  | &nbsp; |
|`\w`  |Coincide con cualquier carácter de word, incluido el subrayado \ (equivalente a `[A-Za-z0-9_]`\).  | &nbsp; |
|`\W`  |Coincide con cualquier carácter non\ word, excepto el subrayado \ (equivalente a `[^A-Za-z0-9_]`\).  | &nbsp; |
|`\ num`  |Hace referencia a las coincidencias recordadas \ (`?num`, donde num es un integer\ positiva).  Esta opción puede usarse sólo en el **reemplazar** cuadro de texto al configurar la manipulación.| `\1` Reemplaza lo que está almacenado en la primera coincidencia de recordar.  |
|`/ n / `  |Permite la inserción de códigos ASCII en expresiones regulares \ (`?n`, donde n es un value\ escape octales, decimal o hexadecimal).  | &nbsp; |

## <a name="examples-for-network-policy-attributes"></a>Ejemplos de atributos de la directiva de red

Los siguientes ejemplos describen el uso de la sintaxis de coincidencia para especificar los atributos de la directiva de red:

- Para especificar todos los números de teléfono en el código de área 899, la sintaxis es:

     `899.*`

- Para especificar un intervalo de direcciones IP que comienzan con 192.168.1, la sintaxis es:

    `192\.168\.1\..+`

## <a name="examples-for-manipulation-of-the-realm-name-in-the-user-name-attribute"></a>Ejemplos de manipulación del nombre de dominio en el atributo de nombre de usuario

Los siguientes ejemplos describen el uso de la sintaxis de coincidencia para manipular los nombres de dominio para el atributo de nombre de usuario, que se encuentra en la **atributo** ficha en las propiedades de una directiva de solicitud de conexión.

**Para quitar la parte de territorio del atributo de nombre de usuario**

En un escenario de marcado externo en el cual una conexión a Internet servicio proveedor \(ISP\) rutas solicita a un servidor NPS de organización, el proxy RADIUS ISP puede requerir un nombre de dominio Kerberos para enrutar la solicitud de autenticación. Sin embargo, es posible que el servidor NPS no reconoce la parte del nombre de dominio del nombre de usuario. Por lo tanto, el nombre de dominio Kerberos debe quitarse mediante el proxy de RADIUS ISP antes de que se reenvía al servidor NPS de la organización.

- Buscar:@microsoft\.com

- Reemplaza:

**Para reemplazar *user@example.microsoft.com*con *example.microsoft.com\user***

- Buscar:`(.*)@(.*)`

- Reemplaza:`$2\$1`



**Para reemplazar *dominio\nombre de usuario* con *specific_domain\user***

- Buscar:`(.*)\\(.*)`

- Reemplazar: *dominio_específico*`\$2`



**Para reemplazar *usuario* con*user@specific_domain***

- Buscar:`$`

- Reemplazar: @*dominio_específico*

## <a name="example-for-radius-message-forwarding-by-a-proxy-server"></a>Ejemplo de reenvío de mensajes RADIUS mediante un servidor proxy

Puedes crear reglas de enrutamiento que reenvíen mensajes RADIUS con un nombre de dominio Kerberos especificado a un conjunto de servidores RADIUS cuando NPS se usa como un proxy RADIUS. Siguiente es una sintaxis recomendada para las solicitudes de enrutamiento basado en el nombre de dominio Kerberos.

- **Nombre NetBIOS **: `WCOAST`
- **Patrón **:      `^wcoast\\`

En el ejemplo siguiente, wcoast.microsoft.com es un sufijo del nombre principal (UPN) de usuario único para el dominio de Active Directory o DNS wcoast.microsoft.com. Mediante el patrón suministrado, el proxy NPS puede enrutar mensajes en función de nombre NetBIOS del dominio o el sufijo UPN.

- **Nombre NetBIOS **: `WCOAST`
- **Sufijo UPN **:   `wcoast.microsoft.com`
- **Patrón **:      `^wcoast\\|@wcoast\.microsoft\.com$`


Para obtener más información acerca de cómo administrar NPS, consulta [administrar el servidor de directivas de red](nps-manage-top.md).

Para obtener más información acerca de NPS, consulta [servidor de directivas de redes (NPS)](nps-top.md).
