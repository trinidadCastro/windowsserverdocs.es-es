---
title: Los servidores de réplica deben configurarse para identificar servidores principales específicos autorizados para enviar el tráfico de replicación.
description: Proporciona instrucciones para resolver el problema que informa esta regla de Analizador de procedimientos recomendados.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 0aeb1f4b-2e75-430b-9557-fe64738c4992
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 3ad031c548d4c8b945e47b06bf66e710d4c3c22e
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87995763"
---
# <a name="replica-servers-should-be-configured-to-identify-specific-primary-servers-authorized-to-send-replication-traffic"></a>Los servidores de réplica deben configurarse para identificar servidores principales específicos autorizados para enviar el tráfico de replicación.

>Se aplica a: Windows Server 2016

Para más información sobre los análisis y los procedimientos recomendados, vea [Ejecución de análisis del Analizador de procedimientos recomendados y administración de los resultados de los análisis](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Advertencia|
|**Categoría**|Configuración|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>Incidencia
*Tal y como se ha configurado, este servidor de réplica acepta el tráfico de replicación de todos los servidores principales y los almacena en una sola ubicación.*

### <a name="impact"></a>Impacto
*Toda la replicación de todos los servidores principales se almacena en una ubicación, lo que puede incluir problemas de privacidad o seguridad.*

## <a name="resolution"></a>Resolución
*Use el administrador de Hyper-V para crear nuevas entradas de autorización para los servidores principales específicos y especificar ubicaciones de almacenamiento separadas para cada una de ellas. Puede usar caracteres comodín para agrupar los servidores principales en conjuntos para cada entrada de autorización.*

#### <a name="create-authorization-entries-using-hyper-v-manager"></a>Crear entradas de autorización mediante el administrador de Hyper-V

1.  Abra el administrador de Hyper-V. (En Administrador del servidor, haga clic en **herramientas**  >  **Administrador de Hyper-V**).

2.  En la lista de hosts, haga clic con el botón secundario en el que desee y, a continuación, haga clic en **configuración de Hyper-V**.

3.  En el panel de navegación, haga clic en **configuración de replicación**.

4.  En **autorización y almacenamiento**, haga clic en **permitir la replicación desde los servidores especificados**.

5.  Debajo de la lista de servidores, haga clic en **Agregar**.

6.  En **Agregar entrada de autorización**:

    -   Escriba el nombre completo del primer servidor.

    -   Especifique una ubicación dedicada para almacenar solo los archivos del servidor.

7.  Haga clic en **Aceptar**.

8.  Repita el procedimiento para cada servidor principal.

9. Vuelva a hacer clic en **Aceptar** para finalizar y cerrar la ventana.

### <a name="create-authorization-entries-using-windows-powershell"></a>Crear entradas de autorización con Windows PowerShell

1.  Abra Windows PowerShell. (En el escritorio, haga clic en Inicio y comience a escribir **Windows PowerShell**).

2.  Haga clic con el botón derecho en **Windows PowerShell** y haga clic en **Ejecutar como administrador**.

3.  Ejecute un comando similar al siguiente, reemplazando:

    -   El nombre del servidor principal de server01.domain01.contoso.com con el nombre de dominio completo del servidor.

    -   La ubicación de D:\ReplicaVMStorage con su ubicación.

    -   El grupo de confianza denominado predeterminado con el nombre de su grupo, si ha creado uno. Si no es así, utilice el valor predeterminado.

```
New-VMReplicationAuthorizationEntry server01.domain01.contoso.com D:\ReplicaVMStorage DEFAULT
```

## <a name="see-also"></a>Consulte también
[New-VMReplicationAuthorizationEntry](/powershell/module/hyper-v/new-vmreplicationauthorizationentry?view=win10-ps)