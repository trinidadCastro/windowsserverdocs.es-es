---
title: Agregar un servidor de agente de conexión a Escritorio remoto para configurar la alta disponibilidad de RDS
description: Obtenga información sobre cómo agregar a un agente de conexión a Escritorio remoto a una implementación de RDS de alta disponibilidad.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 04/10/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 2f4fc63c6ff7c1254fda630a8f34188d8fedc8e5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825046"
---
# <a name="add-the-rd-connection-broker-server-to-the-deployment-and-configure-high-availability"></a>Adición del servidor de Agente de conexión a Escritorio remoto para la implementación y la configuración de alta disponibilidad

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede implementar un clúster de agente de conexión de escritorio remoto (RD Connection Broker) para mejorar la disponibilidad y escalabilidad de su infraestructura de servicios de escritorio remoto. 

## <a name="pre-requisites"></a>Requisitos previos

Configurar un servidor para que actúe como un segundo agente de conexión a Escritorio remoto: Esto puede ser un servidor físico o una máquina virtual.

Configuración de una base de datos para el agente de conexión. Puede usar [Azure SQL Database](https://azure.microsoft.com/documentation/articles/sql-database-get-started/#create-a-new-aure-sql-database) instancia o SQL Server en su entorno local. Hablamos acerca del uso de Azure SQL a continuación, pero los pasos se siguen aplicando a SQL Server. Deberá buscar la cadena de conexión de la base de datos y asegúrese de que tiene el controlador ODBC correcto.

## <a name="step-1-configure-the-database-for-the-connection-broker"></a>Paso 1: Configurar la base de datos para el agente de conexión

1. Buscar la cadena de conexión de la base de datos que creó, necesitará ambos para identificar la versión del controlador ODBC que necesita y más adelante, cuando se configura el agente de conexión propia (paso 3), por lo que guarda la cadena en lugar donde se puede hacer referencia a él fácilmente. Aquí puede saber cuál es la cadena de conexión para SQL Azure:  
    1. En el portal de Azure, haga clic en **examinar > grupos de recursos** y haga clic en el grupo de recursos para la implementación.   
    2. Seleccione la base de datos SQL que acaba de crear (por ejemplo, CB DB1).   
    3. Haga clic en **configuración > Propiedades > mostrar cadenas de conexión de base de datos**.   
    4. Copie la cadena de conexión **ODBC (incluye Node.js)**, que debería tener este aspecto:   
      
        Driver = {SQL Server Native Client 13.0}; Server = tcp:cb-sqls1.database.windows.net,1433; Database = CB-DB1; UID =sqladmin@contoso; PWD = {your_password_here}; Cifrar = yes; TrustServerCertificate = no; Tiempo de espera de conexión = 30;   
  
    5. Reemplace "your_password_here" con la contraseña real. Usará esta cadena completa, con la contraseña que se incluyen, al conectarse a la base de datos. 
2. Instalar al controlador ODBC en el nuevo agente de conexión: 
   1. Si usa una máquina virtual para el agente de conexión, cree una dirección IP pública para el primer agente de conexión a Escritorio remoto. (Sólo tiene que hacer esto si la máquina virtual RDMS no dispone de una dirección IP pública para permitir las conexiones RDP.)
       1. En el portal de Azure, haga clic en **examinar > grupos de recursos**, haga clic en el grupo de recursos para la implementación y, a continuación, haga clic en la primera máquina virtual de agente de conexión a Escritorio remoto (por ejemplo, Contoso-Cb1).
       2. Haga clic en **configuración > interfaces de red**y, a continuación, haga clic en la interfaz de red correspondiente.
       3. Haga clic en **configuración > dirección IP**.
       4. Para **dirección IP pública**, seleccione **habilitado**y, a continuación, haga clic en **dirección IP**.
       5. Si tiene una dirección IP pública existente que desea usar, selecciónelo en la lista. En caso contrario, haga clic en **crear nuevo**, escriba un nombre y, a continuación, haga clic en **Aceptar** y, a continuación, **guardar**.
   2. Conectar con el primer agente de conexión a Escritorio remoto:
       1. En el portal de Azure, haga clic en **examinar > grupos de recursos**, haga clic en el grupo de recursos para la implementación y, a continuación, haga clic en la primera máquina virtual de agente de conexión a Escritorio remoto (por ejemplo, Contoso-Cb1).
       2. Haga clic en **Connect > Abrir** para abrir el cliente de escritorio remoto.
       3. En el cliente, haga clic en **Connect**y, a continuación, haga clic en **usar otra cuenta de usuario**. Escriba el nombre de usuario y la contraseña para una cuenta de administrador de dominio.
       4. Haga clic en **Sí** cuando existe una advertencia sobre el certificado.
   3. Descargue el [controlador ODBC para SQL Server](https://www.microsoft.com/download/confirmation.aspx?id=50420) que coincida con la versión en la cadena de conexión ODBC. Para la cadena de ejemplo anterior, es necesario instalar la versión 13 del controlador de ODBC.
   4. Copie el archivo sqlincli.msi al primer servidor de agente de conexión a Escritorio remoto.   
   5. Abra el archivo sqlincli.msi e instale al cliente nativo.  
   6. Repita los pasos del 1 al 5 para cada agentes adicionales de conexión a Escritorio remoto (por ejemplo, Contoso-Cb2).
   7. Instalar al controlador ODBC en cada servidor que se ejecutará al agente de conexión.

## <a name="step-2-configure-load-balancing-on-the-rd-connection-brokers"></a>Paso 2: Configurar Equilibrio de carga en los agentes de conexión a Escritorio remoto 

Si usa la infraestructura de Azure, puede crear un [equilibrador de carga Azure](#create-a-load-balancer); si no, puede establecer hacia arriba [round robin DNS](#configure-dns-round--robin). 

### <a name="create-a-load-balancer"></a>Crear un equilibrador de carga  
1. Crear un equilibrador de carga   
      1. En el portal Azure, haga clic en **examinar > equilibradores de carga > agregar**.   
      2. Escriba un nombre para el nuevo equilibrador de carga (por ejemplo, hacb).   
      3. Seleccione **interno** para el **esquema**, **red Virtual** para la implementación (por ejemplo, Contoso-VNet) y el **subred** con todos de los recursos (por ejemplo, el valor predeterminado).   
      4. Seleccione **estático** para el **asignación de direcciones IP** y escriba un **dirección IP privada** es decir, no actualmente en uso (por ejemplo, 10.0.0.32).   
      5. Seleccione la **suscripción**, **grupo de recursos** con todos los recursos y la correspondiente **ubicación**.   
      6. Selecciona **Crear**.   
2. Crear un [sondeo](https://azure.microsoft.com/documentation/articles/load-balancer-custom-probe-overview/) para supervisar los servidores que están activos:   
      1. En Azure portal, haga clic en **examinar > equilibradores de carga**y, a continuación, haga clic en el equilibrador de carga que acaba de crear (por ejemplo, CBLB). Haga clic en **Configuración**.   
      2. Haga clic en **sondeos > agregar**.   
      3. Escriba un nombre para el sondeo (por ejemplo, **RDP**), seleccione **TCP** como el **protocolo**, escriba **3389** para el **puerto**y, a continuación, haga clic en **Aceptar**.   
3. Cree el grupo de back-end de los agentes de conexión:   
      1. En **configuración**, haga clic en **grupos de direcciones de back-end > agregar**.   
      2. Escriba un nombre (por ejemplo, CBBackendPool), a continuación, haga clic en **agregar una máquina virtual**.  
      3. Elija un conjunto de disponibilidad (por ejemplo, CbAvSet) y, a continuación, haga clic en **Aceptar**.   
      3. Haga clic en **elegir las máquinas virtuales**, seleccione cada máquina virtual y, a continuación, haga clic en **Seleccionar > Aceptar > Aceptar**.   
4. Cree la regla de equilibrio de carga RDP:   
      1. En **configuración**, haga clic en **reglas de equilibrio de carga**y, a continuación, haga clic en **agregar**.   
      2. Escriba un nombre (por ejemplo, RDP), seleccione **TCP** para el **protocolo**, escriba **3389** para ambos **puerto** y **puerto back-end** y haga clic en **Aceptar**.   
5. Agregue un registro DNS para el equilibrador de carga:   
      1. Conéctese a la máquina virtual del servidor RDMS (por ejemplo, Contoso-CB1). Consulte la [Prepare la máquina virtual de RD Connection Broker](Prepare-the-RD-Connection-Broker-VM-for-Remote-Desktop.md) artículo para obtener información sobre cómo conectarse a la máquina virtual.   
      2. En el administrador del servidor, haga clic en **Herramientas > DNS**.   
      3. En el panel izquierdo, expanda **DNS**, haga clic en la máquina DNS, haga clic en **zonas de búsqueda directa**y, a continuación, haga clic en el nombre de dominio (por ejemplo, Contoso.com). (Podría tardar unos segundos en procesar la consulta al servidor DNS para obtener la información).  
      4. Haga clic en **acción > Nuevo Host (A o AAAA)**.   
      9. Escriba el nombre (por ejemplo, hacb) y la dirección IP especificada anteriormente (por ejemplo, 10.0.0.32).   
  
### <a name="configure-dns-round-robin"></a>Configurar DNS round robin  
  
Los pasos siguientes son una alternativa a la creación de un equilibrador de carga interno de Azure.   
  
1. Conectarse al servidor RDMS en Azure portal. mediante el cliente de conexión a Escritorio remoto   
2. Creación de registros DNS:   
      1. En el administrador del servidor, haga clic en **Herramientas > DNS**.   
      2. En el panel izquierdo, expanda **DNS**, haga clic en la máquina DNS, haga clic en **zonas de búsqueda directa**y, a continuación, haga clic en el nombre de dominio (por ejemplo, Contoso.com). (Podría tardar unos segundos en procesar la consulta al servidor DNS para obtener la información).  
      3. Haga clic en **acción** y **Host nuevo (A o AAAA)**.   
      4. Escriba el **nombre DNS** para el agente de conexión a Escritorio remoto de clúster (por ejemplo, hacb) y, a continuación, escriba el **dirección IP** del primer agente de conexión a Escritorio remoto.   
      5. Repita los pasos 3 y 4 para cada agente de conexión a Escritorio remoto adicionales, que proporciona cada dirección IP única para cada registro adicional.


Por ejemplo, si las direcciones IP para las dos máquinas virtuales de RD Connection Broker 10.0.0.8 y 10.0.0.9, podría crear dos registros de host DNS:
 - Nombre de host: hacb.contoso.com, dirección IP: 10.0.0.8
 - Nombre de host: hacb.contoso.com, dirección IP: 10.0.0.9

## <a name="step-3-configure-the-connection-brokers-for-high-availability"></a>Paso 3: Configurar a los agentes de conexión para lograr alta disponibilidad

1. Agregue el nuevo servidor de agente de conexión a Escritorio remoto al administrador del servidor:
   1. En el administrador del servidor, haga clic en **administrar > Agregar servidores**.
   2. Haz clic en **Buscar ahora**.
   3. Haga clic en el servidor de agente de conexión a Escritorio remoto (por ejemplo, Contoso-Cb2) recién creado y haga clic en **Aceptar**.
2. Configurar la alta disponibilidad para el agente de conexión a Escritorio remoto:
   1. En el administrador del servidor, haga clic en **Remote Desktop Services > información general sobre**.
   2. Haga clic en **RD Connection Broker**y, a continuación, haga clic en **configurar alta disponibilidad**.
   3. Página a través del asistente hasta llegar a la sección del tipo de configuración. Seleccione **servidor de base de datos compartido**y, a continuación, haga clic en **siguiente**.
   4. Escriba el nombre DNS para el clúster de agente de conexión a Escritorio remoto.
   5. Escriba la cadena de conexión para la base de datos SQL y, a continuación, página del Asistente para establecer la alta disponibilidad.
3. Agregar al nuevo agente de conexión a Escritorio remoto a la implementación
   1. En el administrador del servidor, haga clic en **Remote Desktop Services > información general sobre**.
   2. Haga clic en el agente de conexión a Escritorio remoto y, a continuación, haga clic en **Agregar servidor de agente de conexión a Escritorio remoto**.
   3. Página a través del asistente hasta llegar a la selección de servidor, a continuación, seleccione el servidor de agente de conexión a Escritorio remoto (por ejemplo, Contoso-CB2) recién creado.
   4. Complete el asistente, acepte los valores predeterminados.
4. Configurar certificados de confianza en clientes y servidores de agente de conexión a Escritorio remoto.

