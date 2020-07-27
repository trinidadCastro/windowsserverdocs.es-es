---
title: Agregar un servidor de Agente de conexión a Escritorio remoto para configurar la alta disponibilidad en RDS
description: Aprende a agregar un Agente de conexión a Escritorio remoto a una implementación de RDS para obtener una alta disponibilidad.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 04/10/2017
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 6e7e70b8adfcba78e50757be671d38f4d62c99db
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86958707"
---
# <a name="add-the-rd-connection-broker-server-to-the-deployment-and-configure-high-availability"></a>Adición del servidor de Agente de conexión a Escritorio remoto para la implementación y la configuración de alta disponibilidad

>Se aplica a: Windows Server (Canal semianual), Windows Server 2019 y Windows Server 2016

Puedes implementar un clúster de Agente de conexión a Escritorio remoto (Agente de conexión a RD) para mejorar la disponibilidad y la escala de la infraestructura de los servicios de Escritorio remoto. 

## <a name="pre-requisites"></a>Requisitos previos

Configura un servidor para que actúe como un segundo Agente de conexión a Escritorio remoto; puede ser un servidor físico o una VM.

Configura una base de datos para el agente de conexión. Puedes usar una instancia de [Azure SQL Database](/azure/azure-sql/database/single-database-create-quickstart#create-a-new-aure-sql-database) o SQL Server en tu entorno local. Hablaremos sobre cómo usar Azure SQL a continuación, pero estos pasos aún se aplican a SQL Server. Asimismo, necesitarás encontrar la cadena de conexión de la base de datos y asegurarte de tener el controlador ODBC correcto.

## <a name="step-1-configure-the-database-for-the-connection-broker"></a>Paso 1: Configurar la base de datos del Agente de conexión

1. Busca la cadena de conexión de la base de datos que creaste; la necesitas para identificar la versión del controlador ODBC que necesitarás más adelante, cuando configures el Agente de conexión (paso 3), así que guárdala en algún lugar donde puedas consultarla fácilmente. Así es como se busca la cadena de conexión para Azure SQL:  
    1. En Azure Portal, haz clic en **Examinar > Grupos de recursos** y haz clic en el grupo de recursos para la implementación.   
    2. Selecciona la base de datos SQL que acabas de crear (por ejemplo, CB-DB1).   
    3. Haz clic en **Configuración** > **Propiedades** > **Mostrar cadenas de conexión de la base de datos**.   
    4. Copia la cadena de conexión de **ODBC (incluye Node.js)** , que debería tener el siguiente aspecto:   
      
        ```
        Driver={SQL Server Native Client 13.0};Server=tcp:cb-sqls1.database.windows.net,1433;Database=CB-DB1;Uid=sqladmin@contoso;Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
        ```
  
    5. Reemplaza "your_password_here" con la contraseña real. Recuerda que usarás esta cadena completa, con su contraseña incluida, cuando te conectes a la base de datos. 
2. Instala el controlador ODBC en el nuevo Agente de conexión: 
   1. Si estás usando una máquina virtual para el Agente de conexión, crea una dirección IP pública para el primer Agente de conexión de Escritorio remoto. (Solo tienes que hacer esto si la máquina virtual RDMS aún no tiene una dirección IP pública para permitir conexiones RDP).
       1. En Azure Portal, haz clic en **Examinar** > **Grupos de recursos**, haz clic en el grupo de recursos de la implementación y luego en la primera máquina virtual del Agente de conexión a Escritorio remoto (por ejemplo, Contoso-Cb1).
       2. Haz clic en **Configuración > Interfaces de red** y luego en la interfaz de red correspondiente.
       3. Haz clic en **Configuración> Dirección IP**.
       4. Para la **dirección IP pública**, selecciona **Habilitado** y haz clic en **Dirección IP**.
       5. Si tienes una dirección IP pública existente que quieras usar, selecciónala de la lista. De lo contrario, haz clic en **Crear nuevo**, escribe un nombre y haz clic en **Aceptar** y **Guardar**.
   2. Conéctate al primer Agente de conexión de Escritorio remoto:
       1. En Azure Portal, haz clic en **Examinar** > **Grupos de recursos**, haz clic en el grupo de recursos de la implementación y luego en la primera máquina virtual del Agente de conexión a Escritorio remoto (por ejemplo, Contoso-Cb1).
       2. Haz clic en **Conectar > Abrir** para abrir el cliente de Escritorio remoto.
       3. En el cliente, haz clic en **Conectar** y, a continuación, en **Usar otra cuenta de usuario**. Escribe el nombre de usuario y la contraseña de una cuenta de administrador de dominio.
       4. Haz clic en **Sí** cuando se te avise sobre el certificado.
   3. Descarga el [controlador ODBC para SQL Server](https://www.microsoft.com/download/confirmation.aspx?id=50420) que coincida con la versión en la cadena de conexión ODBC. En cuanto a la cadena del ejemplo anterior, necesitas instalar la versión 13 del controlador ODBC.
   4. Copia el archivo sqlincli.msi en el primer servidor de Agente de conexión a Escritorio remoto.   
   5. Abre el archivo sqlincli.msi e instala el cliente nativo.  
   6. Repite los pasos del 1 al 5 para cada Agente de conexión a Escritorio remoto adicional (por ejemplo, Contoso-Cb2).
   7. Instala el controlador ODBC en cada servidor que ejecutará el agente de conexión.

## <a name="step-2-configure-load-balancing-on-the-rd-connection-brokers"></a>Paso 2: Configurar el equilibrio de carga en los Agentes de conexión a Escritorio remoto 

Si estás usando la infraestructura de Azure, puedes crear un [equilibrador de carga de Azure](#create-a-load-balancer); si no, puedes configurar [round robin de DNS](#configure-dns-round-robin).

### <a name="create-a-load-balancer"></a>Crear un equilibrador de carga  
1. Crear un equilibrador de carga de Azure   
      1. En Azure Portal, haz clic en **Examinar > Equilibradores de carga> Agregar**.   
      2. Escribe un nombre para el nuevo equilibrador de carga (por ejemplo, hacb).   
      3. Selecciona **Interno** en las opciones **Esquema** y **Red virtual** para la implementación (por ejemplo, Contoso-VNet) y la **Subred** con todos tus recursos (por ejemplo, el valor predeterminado).   
      4. Selecciona **Estática** en la **asignación de dirección IP** y escribe una **dirección IP privada** que no esté actualmente en uso (por ejemplo, 10.0.0.32).   
      5. Selecciona la **suscripción** apropiada, el **grupo de recursos** con todos tus recursos y la **ubicación** correspondiente.   
      6. Selecciona **Crear**.   
2. Crea un [sondeo](/azure/load-balancer/load-balancer-custom-probe-overview) para supervisar qué servidores están activos:   
      1. En Azure Portal, haz clic en  **Examinar> Equilibradores de carga** y en el equilibrador de carga que acabas de crear (por ejemplo, CBLB). Haga clic en **Configuración**.   
      2. Haga clic en **Sondeos > Agregar**.   
      3. Escribe un nombre para el sondeo (por ejemplo, **RDP**), selecciona **TCP** como **protocolo**, escribe **3389** en el **puerto** y haz clic en **Aceptar**.   
3. Crea el grupo de back-end de los agentes de conexión:   
      1. En **Configuración**, haz clic en **Grupos de direcciones de back-end > Agregar**.   
      2. Escribe un nombre (por ejemplo, CBBackendPool) y haz clic en **Agregar una máquina virtual**.  
      3. Elige un conjunto de disponibilidad (por ejemplo, CbAvSet) y haz clic en **Aceptar**.   
      3. Haz clic en **Elegir las máquinas virtuales**, selecciona cada máquina virtual y haz clic en **Seleccionar > Aceptar > Aceptar**.   
4. Crea la regla de equilibrio de carga RDP:   
      1. En **Configuración**, haz clic en **Reglas de equilibrio de carga** y en **Agregar**.   
      2. Escribe un nombre (por ejemplo, RDP), selecciona **TCP** para el **protocolo**, escribe **3389** en el **puerto** y el **puerto de back-end** y haz clic en **Aceptar**.   
5. Agrega un registro DNS para el equilibrador de carga:   
      1. Conéctate a la máquina virtual del servidor RDMS (por ejemplo, Contoso-CB1). Consulta el artículo [Prepare the RD Connection Broker VM](./rds-prepare-vms.md) (Preparación de la VM del Agente de conexión a Escritorio remoto) para conocer los pasos sobre cómo te puedes conectar a la VM.   
      2. En el Administrador del servidor, haz clic en **Herramientas > DNS**.   
      3. En el panel de la izquierda, expande **DNS**, haz clic en la máquina DNS, en **Zonas de búsqueda directa** y, a continuación, haz clic en tu nombre de dominio (por ejemplo, Contoso.com). (Te puede llevar unos segundos procesar la consulta al servidor DNS para obtener la información).  
      4. Haz clic en **Acción> Nuevo host (A o AAAA)** .   
      9. Escribe el nombre (por ejemplo, hacb) y la dirección IP especificada anteriormente (por ejemplo, 10.0.0.32).   

### <a name="configure-dns-round-robin"></a>Configurar round robin DNS  
  
En los siguientes pasos se indica un modo alternativo a la creación de un equilibrador de carga interno de Azure.   
  
1. Conéctate al servidor RDMS en Azure Portal. usa el cliente de Conexión a Escritorio remoto   
2. Crea registros de DNS:   
      1. En el Administrador del servidor, haz clic en **Herramientas > DNS**.   
      2. En el panel de la izquierda, expande **DNS**, haz clic en la máquina DNS, en **Zonas de búsqueda directa** y, a continuación, haz clic en tu nombre de dominio (por ejemplo, Contoso.com). (Te puede llevar unos segundos procesar la consulta al servidor DNS para obtener la información).  
      3. Haz clic en **Acción** y en **Nuevo host (A o AAAA)** .   
      4. Escribe el **nombre de DNS** para el clúster del Agente de conexión a Escritorio remoto (por ejemplo, hacb), y escribe la **dirección IP** del primer Agente de conexión a Escritorio remoto.   
      5. Repite los pasos 3-4 para cada Agente de conexión a Escritorio remoto adicional y proporciona la dirección IP única de cada registro adicional.


Por ejemplo, si las direcciones IP de las dos máquinas virtuales del Agente de conexión a Escritorio remoto son 10.0.0.8 y 10.0.0.9, crearás dos registros de host DNS:
 - Nombre de host: hacb.contoso.com, dirección IP: 10.0.0.8
 - Nombre de host: hacb.contoso.com, dirección IP: 10.0.0.9

## <a name="step-3-configure-the-connection-brokers-for-high-availability"></a>Paso 3: Configurar los agentes de conexión para lograr alta disponibilidad

1. Agrega el nuevo servidor del Agente de conexión a Escritorio remoto al Administrador del servidor:
   1. En el Administrador del servidor, haz clic en **Administrar > Agregar servidores**.
   2. Haz clic en **Buscar ahora**.
   3. Haz clic en el servidor del Agente de conexión a Escritorio remoto que creaste recientemente (por ejemplo, Contoso-Cb2) y haz clic en **Aceptar**.
2. Configura la alta disponibilidad del Agente de conexión a Escritorio remoto:
   1. En el Administrador del servidor, haz clic en **Servicios de Escritorio remoto > Información general**.
   2. Haz clic con el botón derecho en **Agente de conexión a Escritorio remoto** y luego en **Configurar alta disponibilidad**.
   3. Realiza los pasos del asistente hasta que llegues a la sección Tipo de configuración. Selecciona **Servidor de base de datos compartido** y haz clic en **Siguiente**.
   4. Escribe el nombre DNS del clúster del Agente de conexión a Escritorio remoto.
   5. Asimismo, debes escribir la cadena de conexión para SQL Database y, a continuación, usa el asistente para establecer una alta disponibilidad.
3. Agregar el nuevo Agente de conexión a Escritorio remoto a la implementación
   1. En el Administrador del servidor, haz clic en **Servicios de Escritorio remoto > Información general**.
   2. Haz clic con el botón derecho en el Agente de conexión a Escritorio remoto y luego en **Agregar servidor del Agente de conexión a Escritorio remoto**.
   3. Realiza los pasos del asistente hasta que llegues a la opción de selección del servidor y selecciona el servidor del Agente de conexión a Escritorio remoto que creaste recientemente (por ejemplo, Contoso-CB2).
   4. Completa el asistente, aceptando los valores predeterminados.
4. Configuración de certificados de confianza en clientes y servidores de Agente de conexión a Escritorio remoto.
