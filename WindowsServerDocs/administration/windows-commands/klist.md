---
title: klist
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4689b4a9-1740-47dd-9240-02105efca428
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b693e4496f4fc1275e1f2b364900564ce86e97cb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841968"
---
# <a name="klist"></a>klist



Muestra una lista de vales de Kerberos en caché actualmente. Esta información se aplica a Windows Server 2012. Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
klist [-lh <LogonId.HighPart>] [-li <LogonId.LowPart>] tickets | tgt | purge | sessions | kcd_cache | get | add_bind | query_bind | purge_bind
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|-LH|Denota la parte alta del identificador local único (LUID) del usuario, expresada en formato hexadecimal. Si no hay ninguna – LH o – Li, el comando toma como valor predeterminado el LUID del usuario que ha iniciado sesión actualmente.|
|-Li|Denota la parte baja del identificador local único (LUID) del usuario, expresada en formato hexadecimal. Si no hay ninguna – LH o – Li, el comando toma como valor predeterminado el LUID del usuario que ha iniciado sesión actualmente.|
|vales|Enumera los vales de concesión de vales (TGT) almacenados en caché y los vales de servicio de la sesión de inicio de sesión especificada. Esta es la opción predeterminada.|
|TGT|Muestra el TGT inicial de Kerberos.|
|purga|Permite eliminar todos los vales de la sesión de inicio de sesión especificada.|
|sesiones|Muestra una lista de sesiones de inicio de sesión en este equipo.|
|kcd_cache|Muestra la información de la caché de delegación restringida de Kerberos.|
|obtener|Permite solicitar un vale al equipo de destino especificado por el nombre de entidad de seguridad de servicio (SPN).|
|add_bind|Permite especificar un controlador de dominio preferido para la autenticación Kerberos.|
|query_bind|Muestra una lista de controladores de dominio preferidos almacenados en caché para cada dominio en el que se ha establecido contacto con Kerberos.|
|purge_bind|Quita los controladores de dominio preferidos almacenados en caché para los dominios especificados.|
|kdcoptions|Muestra las opciones de Centro de distribución de claves (KDC) especificadas en RFC 4120.|
|/?|Muestra ayuda para este comando.|

## <a name="remarks"></a>Comentarios

La pertenencia al grupo **Admins**. del dominio, o equivalente, es lo mínimo necesario para ejecutar todos los parámetros de este comando.

Si no se proporciona ningún parámetro, klist recuperará todos los vales del usuario que ha iniciado sesión actualmente.

Los parámetros muestran la siguiente información:
-   **incidencia**

    Enumera los vales almacenados actualmente en caché de los servicios a los que se ha autenticado desde el inicio de sesión. Muestra los siguientes atributos de todos los vales almacenados en caché:  
    -   LogonID: LUID
    -   Cliente: la concatenación del nombre de cliente y el nombre de dominio del cliente
    -   Servidor: la concatenación del nombre de servicio y el nombre de dominio del servicio
    -   KerbTicket Encryption Type: el tipo de cifrado que se usa para cifrar el vale de Kerberos.
    -   Marcas de vale: marcas de vale de Kerberos
    -   Hora de Inicio: la hora a partir de la cual será válida la incidencia.
    -   Hora de finalización: la hora en que el vale deja de ser válido. Cuando un vale supera este tiempo, ya no se puede usar para autenticarse en un servicio o usarse para la renovación.
    -   Tiempo de renovación: el tiempo que se requiere una nueva autenticación inicial
    -   Tipo de clave de sesión: el algoritmo de cifrado que se usa para la clave de sesión
-   **TGT**

    Muestra el TGT inicial de Kerberos y los siguientes atributos del vale actualmente en caché:  
    -   LogonID: identificado en hexadecimal
    -   ServiceName: krbtgt
    -   \<SPN del destino >: krbtgt
    -   DomainName: nombre del dominio que emite el TGT
    -   TargetDomainName: dominio al que se emite el TGT
    -   AltTargetDomainName: dominio al que se emite el TGT
    -   Marcas de vale: acciones de dirección y destino y tipo
    -   Clave de sesión: longitud de clave y algoritmo de cifrado
    -   StartTime: hora del equipo local en la que se solicitó el vale
    -   EndTime: hora en la que el vale deja de ser válido. Cuando un vale supera este tiempo, ya no se puede usar para autenticarse en un servicio.
    -   RenewUntil: fecha límite para la renovación de vales
    -   TimeSkew: diferencia de hora con el Centro de distribución de claves (KDC)
    -   EncodedTicket: vale codificado
-   **deuda**

    Permite eliminar un vale específico. La purga de vales destruye todos los vales que se han almacenado en caché, por lo que debe usar este atributo con precaución. Puede impedir que pueda autenticarse en los recursos. Si esto sucede, tendrá que cerrar la sesión y volver a iniciarla.  
    -   LogonID: identificado en hexadecimal
-   **conexiones**

    Permite enumerar y mostrar la información de todas las sesiones de inicio de sesión en este equipo.  
    -   LogonID: si se especifica, solo se muestra la sesión de inicio de sesión por el valor especificado. Si no se especifica, se muestran todas las sesiones de inicio de sesión en este equipo.
-   **kcd_cache**

    Permite mostrar la información de la memoria caché de delegación restringida de Kerberos.  
    -   LogonID: si se especifica, muestra la información de la memoria caché de la sesión de inicio de sesión por el valor especificado. Si no se especifica, muestra la información de la memoria caché de la sesión de inicio de sesión del usuario actual.
-   **Obtener**

    Permite solicitar un vale al destino especificado por el SPN.  
    -   LogonID: si se especifica, solicita un vale mediante la sesión de inicio de sesión por el valor especificado. Si no se especifica, solicita un vale mediante la sesión de inicio de sesión del usuario actual.
    -   kdcoptions: solicita un vale con las opciones de KDC proporcionadas.
-   **add_bind**

    Permite especificar un controlador de dominio preferido para la autenticación Kerberos.
-   **query_bind**

    Permite mostrar controladores de dominio preferidos en caché para los dominios.
-   **purge_bind**

    Permite quitar controladores de dominio preferidos en caché para los dominios.
-   **kdcoptions**

    Para obtener la lista actual de opciones y sus explicaciones, consulte [RFC 4120](http://www.ietf.org/rfc/rfc4120.txt).

**Otras consideraciones**
-   Klist. exe está disponible en Windows Server 2012 y Windows 8, y no requiere ninguna instalación especial.

## <a name="examples"></a><a name=BKMK_Examples></a>Example

1. Al diagnosticar un identificador de evento 27 mientras se procesa una solicitud de servicio de concesión de vales (TGS) para el servidor de destino, la cuenta no tenía una clave adecuada para generar un vale de Kerberos. Puede usar klist para consultar la memoria caché de vales de Kerberos para determinar si faltan vales, si el servidor o la cuenta de destino tienen errores, o si no se admite el tipo de cifrado.  
   ```
   klist 
   ```  
   ```
   klist –li 0x3e7
   ```  
2. Al diagnosticar errores y desea conocer los detalles de cada vale de concesión de vales que se almacena en la memoria caché del equipo para una sesión de inicio de sesión, puede usar klist para mostrar la información del TGT.  
   ```
   klist tgt
   ```  
3. Si no puede establecer una conexión y el diagnóstico puede tardar demasiado tiempo, puede purgar la caché de vales de Kerberos, cerrar la sesión y volver a iniciarla.  
   ```
   klist purge
   ```  
   ```
   klist purge –li 0x3e7
   ```  
4. Si desea diagnosticar una sesión de inicio de sesión para un usuario o un servicio, puede usar el siguiente comando para buscar el LogonID que se usa en otros comandos klist.  
   ```
   klist sessions
   ```  
5. Si desea diagnosticar un error de delegación restringida de Kerberos, puede utilizar el siguiente comando para buscar el último error que se encontró.  
   ```
   klist kcd_cache
   ```  
6. Si desea realizar un diagnóstico si un usuario o un servicio pueden obtener un vale para un servidor, puede usar este comando para solicitar un vale para un SPN específico.  
   ```
   klist get host/%computername%
   ```  
7. Al diagnosticar problemas de replicación entre controladores de dominio, normalmente necesitará que el equipo cliente tenga como destino un controlador de dominio específico. En estos casos, puede usar el siguiente comando para destinar el equipo cliente a ese controlador de dominio específico.  
   ```
   klist add_bind CONTOSO KDC.CONTOSO.COM
   ```  
   ```
   klist add_bind CONTOSO.COM KDC.CONTOSO.COM
   ```  
8. Para consultar en qué controladores de dominio se ha puesto en contacto este equipo recientemente, puede usar el siguiente comando.  
   ```
   klist query_bind
   ```  
9. Si desea que Kerberos vuelva a detectar controladores de dominio, puede usar el siguiente comando. Este comando también se puede usar para vaciar la memoria caché antes de crear nuevos enlaces de controlador de dominio con klist add_bind.  
   ```
   klist purge_bind
   ```

## <a name="additional-references"></a>Referencias adicionales

-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)