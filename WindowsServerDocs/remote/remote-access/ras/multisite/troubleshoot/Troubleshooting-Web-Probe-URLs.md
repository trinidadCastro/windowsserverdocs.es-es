---
title: Solucionar problemas relacionados con las direcciones URL de sondeo web
description: Este tema forma parte de la Guía de implementación de varios servidores de acceso remoto en una implementación multisitio en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6dfffd1e-f4f4-43b6-9e3c-49015ce34338
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 44bc409af33ce217e21c5d6e252ac4804718d7a8
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282467"
---
# <a name="troubleshooting-web-probe-urls"></a>Solucionar problemas relacionados con las direcciones URL de sondeo web

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema encontrará información para solucionar problemas relacionados con el comando `Set-DAEntryPointDC`. Para confirmar que el error recibido está relacionado con el establecimiento del controlador de dominio del punto de entrada, consulte el identificador de evento 10065 en el Registro de eventos de Windows.  
  
## <a name="SaveGPOSettings"></a>Guardando la configuración de GPO de servidor  
**Recibidas el error**. Se produjo un error al guardar la configuración de acceso remoto en el GPO < nombre_gpo >.  
  
Para solucionar este error, consulte guardar la configuración de GPO de servidor.  
  
## <a name="remote-access-is-not-configured"></a>Acceso remoto no está configurado  
**Recibidas el error**. Acceso remoto no está configurado en < nombreDeServidor >. Especifique el nombre de un servidor que forme parte de una implementación multisitio.  
  
O bien  
  
Acceso remoto no está configurado en el servidor < nombre_servidor >. Especifique un equipo con DirectAccess habilitado.  
  
**Causa**  
  
Acceso remoto no está configurado en el equipo especificado en el parámetro *ComputerName*.  
  
El cmdlet `Set-DaEntryPointDC` está disponible únicamente en los servidores que formen parte de una implementación multisitio configurada.  
  
**Solución**  
  
Ejecute el comando y cuide de especificar el parámetro *ComputerName* con el nombre del servidor que ya está configurado como parte de la implementación multisitio.  
  
## <a name="multisite-is-not-enabled"></a>La funcionalidad de multisitio no está habilitada  
**Recibidas el error**. Debe habilitar una implementación multisitio antes de realizar esta operación. Para ello, use el cmdlet `Enable-DAMultiSite`.  
  
**Causa**  
  
La funcionalidad de multisitio no está habilitada en el servidor especificado en el parámetro *ComputerName*.  
  
El cmdlet `Set-DaEntryPointDC` está disponible únicamente en los servidores que formen parte de una implementación multisitio configurada.  
  
**Solución**  
  
Ejecute el comando y cuide de especificar el parámetro *ComputerName* con el nombre del servidor que ya está configurado como parte de la implementación multisitio.  
  
## <a name="entry-point-and-domain-controller-not-provided-in-cmdlet"></a>No se ha especificado el punto de entrada ni el controlador de dominio en el cmdlet  
El cmdlet `Set-DaEntryPointDC` permite cambiar el controlador de dominio asociado a diferentes puntos de entrada (por ejemplo, porque un controlador de dominio concreto ya no esté disponible). Así, se puede actualizar un punto de entrada específico para que use otro controlador de dominio, o bien actualizar todos los puntos de entrada que usan un controlador de dominio en particular de modo que usen uno nuevo. En el primer caso, se debe usar el parámetro *EntryPointName* para especificar el punto de entrada que se debe actualizar, mientras que, en el segundo, se debe usar el parámetro *ExistingDC* para especificar el controlador de dominio que hay que reemplazar. Solo se puede establecer uno de estos dos parámetros.  
  
**Recibidas el error**. No se especificaron parámetros obligatorios. Proporcione el nombre de un punto de entrada o de un controlador de dominio existente.  
  
O bien  
  
Faltan todos los parámetros obligatorios en el cmdlet `Set-DaEntryPointDC`.  
  
**Causa**  
  
No se especificó el parámetro *EntryPointName* o el parámetro *ExistingDC* (o ninguno de los dos) correspondientes al cmdlet `Set-DaEntryPointDC`.  
  
**Solución**  
  
Ejecute el comando y cuide de especificar el parámetro *EntryPointName* o el parámetro *ExistingDC*.  
  
## <a name="could-not-locate-domain-controller"></a>No se puede encontrar el controlador de dominio  
**Recibidas el error**. No se puede localizar automáticamente un nuevo controlador de dominio. Vuelva a intentarlo más tarde o compruebe la configuración del controlador de dominio.  
  
**Causa**  
  
El equipo especificado con el parámetro *ComputerName* no está accesible a través de RPC o el dominio carece de un controlador de dominio de escritura.  
  
**Solución**  
  
Asegúrese de que se puede acceder al equipo remoto mediante RPC y de que hay un controlador de dominio de escritura disponible para el dominio. Si lo hay, también puede indicar su nombre expresamente mediante el parámetro *NewDC*.  
  
## <a name="could-not-connect-to-domain-controller"></a>No se puede conectar con el controlador de dominio  
  
-   **Problema 1**  
  
    **Recibidas el error**. No se puede alcanzar el controlador de dominio < controlador_dominio >. Compruebe la conectividad de red y la disponibilidad del servidor.  
  
    **Causa**  
  
    No se puede acceder al controlador de dominio. Esto sucede únicamente cuando el administrador especifica un controlador de dominio en los parámetros *NewDC* o *ExistingDC*.  
  
    **Solución**  
  
    Asegúrese de que el nombre del controlador de dominio está bien escrito. Si ha usado un nombre corto, use el FQDN e inténtelo de nuevo.  
  
-   **Issue 2**  
  
    **Recibidas el error**. No se puede establecer contacto con el controlador de dominio < controlador_dominio >.  
  
    **Causa**  
  
    Puede que haya un problema de red que impide el acceso al controlador de dominio especificado en el parámetro *NewDC* o a cualquier otro controlador de dominio existente en la configuración.  
  
    **Solución**  
  
    Asegúrese de que el nombre del controlador de dominio está bien escrito, confirme que existe, que está en funcionamiento y que se puede escribir en él y, asimismo, que existe una relación de confianza entre el controlador de dominio y el dominio.  
  
-   **Problema 3**  
  
    **Recibidas el error**. No se puede alcanzar el controlador de dominio < controlador_dominio > para %2!s!).  
  
    **Causa**  
  
    Para lograr que la configuración sea coherente en unas implementación multisitio, es importante asegurarse de que cada GPO se administra mediante un único controlador de dominio. Cuando el controlador de dominio que administra el GPO de servidor de un punto de entrada no está disponible, no se lean o modifiquen los valores de configuración de acceso remoto.  
  
    **Solución**  
  
    Siga el procedimiento "para cambiar el controlador de dominio que administra los GPO de servidor" se describe en [2.4. Configurar GPO](assetId:///b1960686-a81e-4f48-83f1-cc4ea484df43#ConfigGPOs).  
  
-   **Problema 4**  
  
    **Recibidas el error**. No se puede alcanzar el controlador de dominio principal en el dominio < nombreDeDominio >.  
  
    **Causa**  
  
    Para lograr que la configuración sea coherente en unas implementación multisitio, es importante asegurarse de que cada GPO se administra mediante un único controlador de dominio. Los GPO de cliente se administran en el controlador de dominio principal. En caso de que el controlador de dominio principal no esté disponible, la configuración de Acceso remoto no se puede leer ni alterar.  
  
    **Solución**  
  
    Siga el procedimiento "para transferir el rol de emulador PDC" descrito en [2.4. Configurar GPO](assetId:///b1960686-a81e-4f48-83f1-cc4ea484df43#ConfigGPOs).  
  
## <a name="read-only-domain-controller"></a>Controlador de dominio de solo lectura  
**Recibidas el error**. El controlador de dominio < controlador_dominio > es de solo lectura. Especifique uno que no lo sea.  
  
**Causa**  
  
El controlador de dominio especificado con el parámetro *NewDC* es de solo lectura.  
  
**Solución**  
  
Al usar `Set-DAEntryPointDC`, el parámetro *NewDC* se utiliza para actualizar el controlador de dominio asociado a un punto de entrada en concreto o para actualizar todos los puntos de entrada asociados a un controlador de dominio. Por lo tanto, el nuevo controlador de dominio debe ser de escritura. Establezca el parámetro *NewDC* en un controlador de dominio de escritura e inténtelo de nuevo.  
  
## <a name="cannot-retrieve-gpo"></a>No se puede recuperar el GPO  
  
-   **Problema 1**  
  
    **Recibidas el error**. No se puede recuperar el GPO < nombre_gpo > en < controlador_dominio_anterior > del controlador de dominio desde el controlador de dominio < controlador_dominio_sustitución > porque no están en el mismo dominio.  
  
    **Causa**  
  
    El servidor de acceso remoto y el controlador de dominio no están en el mismo dominio, de modo que el GPO no se puede recuperar.  
  
    **Solución**  
  
    Si ha tratado de actualizar un punto de entrada concreto, asegúrese de que el nuevo controlador de dominio se encuentra en el mismo dominio que el servidor de punto de entrada. Si ha tratado de actualizar un controlador de dominio concreto, asegúrese de que el nuevo controlador de dominio se encuentra en el mismo dominio que el que está intentando sustituir.  
  
-   **Issue 2**  
  
    **Recibidas el error**. No se puede recuperar el GPO < nombre_gpo > en < controlador_dominio_anterior > del controlador de dominio desde el controlador de dominio < controlador_dominio_sustitución >. Espere a que se complete la replicación del dominio y, a continuación, vuelva a intentarlo.  
  
    **Causa**  
  
    Al tratar de actualizar un controlador de dominio de punto de entrada, el cmdlet intenta leer el GPO de servidor desde el nuevo controlador de dominio, pero el GPO no se puede encontrar en dicho controlador de dominio porque aún no se ha replicado.  
  
    **Solución**  
  
    El GPO de servidor no existe en el nuevo controlador de dominio. Asegúrese de que los GPO se han replicado correctamente en el nuevo controlador de dominio e inténtelo de nuevo.  
  
-   **Problema 3**  
  
    **Recibidas el error**. No tiene permisos para acceder a GPO < nombre_gpo >.  
  
    **Causa**  
  
    Al tratar de actualizar un controlador de dominio de punto de entrada, el cmdlet intenta leer el GPO de servidor desde el nuevo controlador de dominio, pero el GPO no se puede leer en dicho controlador de dominio porque carece de los permisos adecuados.  
  
    **Solución**  
  
    El GPO existe en el controlador de dominio, pero no se puede leer. Asegúrese de que dispone de los permisos necesarios y vuelva a intentarlo.  
  
## <a name="entry-point-not-part-of-multisite-deployment"></a>El punto de entrada no forma parte de la implementación multisitio  
**Recibidas el error**. Punto de entrada < nombre_punto_entrada > no forma parte de la implementación multisitio. Especifique un valor alternativo.  
  
**Causa**  
  
No se encontró el nombre de punto de entrada indicado.  
  
**Solución**  
  
Asegúrese de que el nombre de punto de entrada está bien escrito y de que los GPO se han replicado en los controladores de dominio necesarios y, tras ello, inténtelo de nuevo. Para ver el controlador de dominio asignado para cada punto de entrada, use `Get-DAEntryPointDC`.  
  
## <a name="remote-access-server-settings"></a>Configuración del servidor de acceso remoto  
  
-   **Problema 1**  
  
    **Recibidas el error**. No se puede tener acceso a servidor < nombre_servidor > en < nombre_punto_entrada > del punto de entrada.  
  
    **Causa**  
  
    Al tratar de actualizar un controlador de dominio de punto de entrada, el cmdlet intenta leer y escribir dicho controlador desde todos los servidores de acceso remoto relevantes. El cmdlet no pudo leer los datos de uno o más servidores de acceso remoto.  
  
    **Solución**  
  
    Asegúrese de que todos los servidores de acceso remoto se están ejecutando y de que dispone de permisos de administrador en todos ellos y, después, inténtelo de nuevo.  
  
-   **Issue 2**  
  
    **Recibidas el error**. Configuración no se puede guardar en el registro en el servidor < nombreDeServidor > en < nombre_punto_entrada > del punto de entrada.  
  
    **Causa**  
  
    Al tratar de actualizar un controlador de dominio de punto de entrada, el cmdlet intenta leer y escribir dicho controlador desde todos los servidores de acceso remoto relevantes. El cmdlet no pudo escribir los datos en uno o más servidores de acceso remoto.  
  
    **Solución**  
  
    Asegúrese de que todos los servidores de acceso remoto se están ejecutando y de que dispone de permisos de administrador en todos ellos y, después, inténtelo de nuevo.  
  
-   **Problema 3**  
  
    **Recibidas el error**. No se puede aplicar actualizaciones de GPO en < nombreDeServidor >. Los cambios no surtirán efecto hasta la siguiente actualización de directiva.  
  
    **Causa**  
  
    Al usar el cmdlet `Set-DAEntryPointDC`, el parámetro *ComputerName* especificado es un servidor de acceso remoto en un punto de entrada distinto del último que se agregó a la implementación multisitio.  
  
    **Solución**  
  
    Todos aquellos servidores que no se han actualizado se pueden consultar mediante la opción **Estado de configuración** en **PANEL**, en la Consola de administración de acceso remoto. Esto no acarrea problemas de funcionamiento; sin embargo, se puede ejecutar `gpupdate /force` en cualquier servidor que no se haya actualizado para actualizar su estado de configuración inmediatamente.  
  
## <a name="problem-resolving-fqdn"></a>Problema al resolver FQDN  
**Recibidas el error**. No se puede tener acceso a servidor < nombre_servidor > en < nombre_punto_entrada > del punto de entrada.  
  
**Causa**  
  
Al obtener la lista de servidores de DirectAccess que se van a modificar, el cmdlet no pudo resolver el nombre de dominio completo (FQDN) de uno de los servidores a partir del SID de equipo correspondiente.  
  
**Solución**  
  
El punto de entrada mencionado en el mensaje de error está asociado a un controlador de dominio; asegúrese de que dicho controlador está disponible para el punto de entrada. Si el equipo al que el SID especificado pertenece se quitó del dominio, pase por alto este mensaje y quite el servidor de la implementación multisitio.  
  
## <a name="no-entry-points-to-update"></a>No hay puntos de entrada que actualizar  
**Advertencia**. No se han modificado la configuración del controlador de dominio. Si cree que es necesario hacer cambios, asegúrese de que los parámetros del cmdlet estén configurados correctamente y de que los GPO se repliquen en los controladores de dominio requeridos.  
  
**Causa**  
  
Al llamar al cmdlet `Set-DaEntryPointDC` con el parámetro *ExistingDC*, DirectAccess comprueba todos los puntos de entrada y actualiza los que estén asociados al controlador de dominio especificado. Sin embargo, no hay ninguno que use el parámetro *ExistingDC* especificado.  
  
**Solución**  
  
Para ver la lista de los puntos de entrada y sus controladores de dominio asociados, use el cmdlet `Get-DAEntryPointDC`. Si se deberían haber realizado cambios, asegúrese de que los parámetros del cmdlet están bien escritos y de que los GPO se han replicado en los controladores de dominio necesarios y, tras ello, inténtelo de nuevo.  
  


