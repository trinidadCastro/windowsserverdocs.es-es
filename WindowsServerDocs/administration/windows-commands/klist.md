---
title: klist
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4689b4a9-1740-47dd-9240-02105efca428
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3b3d0591f9feb12782d0c77b6c786cfe17656ab2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831166"
---
# <a name="klist"></a>klist



Muestra una lista de los vales de Kerberos en caché actualmente. Esta información se aplica a Windows Server 2012. Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
klist [-lh <LogonId.HighPart>] [-li <LogonId.LowPart>] tickets | tgt | purge | sessions | kcd_cache | get | add_bind | query_bind | purge_bind
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|-lh|Indica la parte alta de que el identificador del usuario local único (LUID), expresada en formato hexadecimal. Si ni – lh ni – li están presentes, el comando predeterminado es el LUID del usuario que actualmente ha iniciado sesión.|
|-li|Indica la parte baja del identificador del usuario local único (LUID), expresada en formato hexadecimal. Si ni – lh ni – li están presentes, el comando predeterminado es el LUID del usuario que actualmente ha iniciado sesión.|
|vales|Enumera los almacenados en caché actualmente-concesión-de vales (TGT) y vales de servicio de la sesión de inicio de sesión especificado. Esta es la opción predeterminada.|
|tgt|Muestra el TGT. Kerberos inicial|
|purga|Permite eliminar todos los vales de la sesión de inicio de sesión especificado.|
|Sesiones|Muestra una lista de sesiones de inicio de sesión en este equipo.|
|kcd_cache|Muestra el Kerberos restringida información de la caché de delegación.|
|get|Permite solicitar un vale para el equipo de destino especificado por el nombre principal de servicio (SPN).|
|add_bind|Le permite especificar un controlador de dominio preferido para la autenticación Kerberos.|
|query_bind|Muestra una lista de controladores de dominio preferido en caché para cada dominio que se ha establecido contacto con Kerberos.|
|purge_bind|Quita la caché preferida controladores de dominio para los dominios especificados.|
|kdcoptions|Muestra las opciones de centro de distribución de claves (KDC) especificadas en RFC 4120.|
|/?|Muestra la ayuda para este comando.|

## <a name="remarks"></a>Comentarios

Pertenencia a **Admins. del dominio**, o equivalente, es lo mínimo necesario para ejecutar todos los parámetros de este comando.

Si no se proporcionan parámetros, Klist recuperará todos los vales para el usuario ha iniciado sesión actualmente.

Los parámetros muestran la siguiente información:
-   **tickets**

    Enumera los vales actualmente en la caché de los servicios que se han autenticado a desde el inicio de sesión. Muestra los siguientes atributos de todos los vales almacenados en caché:  
    -   LogonID: El LUID
    -   Cliente: La concatenación del nombre del cliente y el nombre de dominio del cliente
    -   Servidor: La concatenación del nombre del servicio y el nombre de dominio del servicio
    -   Tipo de cifrado KerbTicket: El tipo de cifrado que se utiliza para cifrar el vale de Kerberos
    -   Marcas de vale: Las marcas de vale de Kerberos
    -   Hora de inicio: El tiempo desde el que será válido el vale
    -   Hora de finalización: Hora que ya no es válido el vale. Cuando un vale está más allá de este momento, ya no se puede usar para autenticar a un servicio o se usa para la renovación
    -   Renovar a tiempo: El tiempo que se necesita una nueva autenticación inicial
    -   Tipo de clave de sesión: El algoritmo de cifrado que se usa para la clave de sesión
-   **tgt**

    Muestra el TGT de Kerberos inicial y los atributos siguientes del vale actualmente en caché:  
    -   LogonID: Identificado en formato hexadecimal
    -   ServiceName: krbtgt
    -   TargetName \<SPN >: krbtgt
    -   DomainName: Nombre del dominio que emite el TGT
    -   TargetDomainName: Dominio que se emitió el TGT para
    -   AltTargetDomainName: Dominio que se emitió el TGT para
    -   Marcas de vale: Tipo y las acciones de dirección y de destino
    -   Clave de sesión: Algoritmo de cifrado y la longitud de clave
    -   StartTime: Hora del equipo local que se solicitó el vale
    -   Hora de finalización: Hora que ya no es válido el vale. Cuando un vale está más allá de este momento, ya no puede usarse para autenticar a un servicio.
    -   RenewUntil: Fecha límite para la renovación de vales
    -   TimeSkew: Diferencia horaria con el centro de distribución de claves (KDC)
    -   EncodedTicket: Vale codificado
-   **purge**

    Permite eliminar un vale específico. Purga los vales destruye todos los vales que se hayan almacenado en caché, así que use este atributo con precaución. Podría dejar de poder autenticar a los recursos. Si esto ocurre, tendrá que cerrar la sesión y vuelva a iniciarla.  
    -   LogonID: Identificado en formato hexadecimal
-   **Sesiones**

    Permite enumerar y mostrar la información de todas las sesiones de inicio de sesión en este equipo.  
    -   LogonID: Si se especifica, muestra el inicio de sesión solo por el valor especificado. Si no se especifica, muestra todas las sesiones de inicio de sesión en este equipo.
-   **kcd_cache**

    Permite mostrar la información de caché de la delegación restringida de Kerberos.  
    -   LogonID: Si se especifica, muestra la información de la memoria caché para el inicio de sesión por el valor especificado. Si no se especifica, muestra la información de la memoria caché de sesión de inicio de sesión del usuario actual.
-   **get**

    Permite solicitar un vale para el destino especificado por el SPN.  
    -   LogonID: Si se especifica, se solicita un vale mediante el inicio de sesión si el valor especificado. Si no se especifica, se solicita un vale mediante el uso de la sesión de inicio de sesión del usuario actual.
    -   kdcoptions: Solicita un vale con las opciones dadas de KDC
-   **add_bind**

    Le permite especificar un controlador de dominio preferido para la autenticación Kerberos.
-   **query_bind**

    Permite mostrar controladores de dominio preferido, almacenado en caché para los dominios.
-   **purge_bind**

    Permite quitar los controladores de dominio preferido, almacenado en caché para los dominios.
-   **kdcoptions**

    Para obtener la lista actual de las opciones y sus explicaciones, consulte [4120 RFC](http://www.ietf.org/rfc/rfc4120.txt).

**Otras consideraciones**
-   Klist.exe está disponible en Windows Server 2012 y Windows 8 y requiere ninguna instalación especial.

## <a name="BKMK_Examples"></a>Ejemplos

1.  Al diagnosticar un 27 de Id. de evento al procesar un servicio de concesión de vales (TGS) de solicitud para el servidor de destino, la cuenta no tiene una clave adecuada para generar un vale de Kerberos. Puede usar Klist para consultar la caché del vale Kerberos para determinar si los vales no se encuentran, si la cuenta o el servidor de destino es un error, o si no se admite el tipo de cifrado.  
    ```
    klist 
    ```  
    ```
    klist –li 0x3e7
    ```  
2.  Cuando diagnostique errores y desea conocer los detalles de cada incidencia vale de concesión que se almacena en caché en el equipo para una sesión de inicio de sesión, puede usar Klist para mostrar la información de TGT.  
    ```
    klist tgt
    ```  
3.  Si no se puede establecer una conexión y un diagnóstico más detallado puede tardar demasiado tiempo, puede purgar la caché de vales de Kerberos, cierre la sesión y, a continuación, volver a iniciar sesión.  
    ```
    klist purge
    ```  
    ```
    klist purge –li 0x3e7
    ```  
4.  Cuando desea diagnosticar una sesión de inicio de sesión para un usuario o un servicio, puede usar el comando siguiente para encontrar el ID de registro que se usa en otros comandos Klist.  
    ```
    klist sessions
    ```  
5.  Cuando desea diagnosticar errores de delegación restringida de Kerberos, puede usar el siguiente comando para buscar el último error que se encontró.  
    ```
    klist kcd_cache
    ```  
6.  Cuando desea diagnosticar si un usuario o un servicio puede obtener un vale para un servidor, puede usar este comando para solicitar un vale para un SPN específico.  
    ```
    klist get host/%computername%
    ```  
7.  Al diagnosticar problemas de replicación entre controladores de dominio, normalmente se necesita el equipo cliente para tener como destino un controlador de dominio específico. En estos casos, puede usar el siguiente comando para establecer como destino el equipo cliente para ese controlador de dominio específico.  
    ```
    klist add_bind CONTOSO KDC.CONTOSO.COM
    
    ```  
    ```
    klist add_bind CONTOSO.COM KDC.CONTOSO.COM
    ```  
8.  Para consultar qué controladores de dominio de este equipo puede establecer contacto conectado recientemente, puede usar el siguiente comando.  
    ```
    klist query_bind
    ```  
9.  Cuando desee Kerberos para volver a detectar los controladores de dominio, puede usar el siguiente comando. Este comando también puede utilizarse para vaciar la memoria caché antes de crear nuevos enlaces de controlador de dominio con klist add_bind.  
    ```
    klist purge_bind
    ```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)