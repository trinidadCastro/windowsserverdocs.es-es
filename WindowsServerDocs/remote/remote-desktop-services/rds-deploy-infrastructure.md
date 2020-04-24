---
title: Implementación del entorno de Escritorio remoto
ms.prod: windows-server
description: Pasos básicos para implementar un entorno de Escritorio remoto.
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 04/10/2017
ms.topic: article
author: lizap
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: 31bb6afaca92b36453d4565c1f79aae35a6f0900
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2020
ms.locfileid: "80855758"
---
# <a name="deploy-your-remote-desktop-environment"></a>Implementación del entorno de Escritorio remoto

>Se aplica a: Windows Server (Canal semianual), Windows Server 2019 y Windows Server 2016

Sigue estos pasos para implementar los servidores de Escritorio remoto en tu entorno. Puedes instalar los roles de servidor en equipos físicos o máquinas virtuales, en función de si estás creando un entorno local, basado en la nube o híbrido. 

Si estás usando máquinas virtuales para cualquiera de los servidores de Servicios de Escritorio Remoto, asegúrate de haber [preparado esas máquinas virtuales](rds-prepare-vms.md).
  
  
1.  Agrega todos los servidores que vayas a usar en los Servicios de Escritorio remoto al Administrador del servidor:  
    1.  En el Administrador del servidor, haz clic en **Administrar** > **Agregar servidores**.  
    2.  Haz clic en **Buscar ahora**.  
    3.  Haz clic en cada servidor de la implementación (por ejemplo, Contoso-Cb1, Contoso-WebGw1 y Contoso-Sh1) y en **Aceptar**.  
2.  Crea una implementación basada en una sesión para poder implementar los componentes de los Servicios de Escritorio Remoto:  
    1.  En el Administrador del servidor, haz clic en **Administrar** > **Agregar roles y características**.  
    2.  Haz clic en **Instalación de Servicios de Escritorio remoto**, **Implementación estándar** e **Implementación de escritorio basada en una sesión**.  
    3.  Selecciona los servidores adecuados para el servidor del Agente de conexión a Escritorio remoto, el servidor de Acceso web de Escritorio remoto y el servidor de Host de sesión de Escritorio remoto (por ejemplo, Contoso-Cb1, Contoso-WebGw1 y Contoso-SH1, respectivamente).  
    4.  Selecciona **Reiniciar el servidor de destino automáticamente si es necesario**, y luego haz clic en **Implementar**.  
    5.  Espera a que la implementación se complete con éxito  
3.  Agrega un servidor de licencias de Escritorio remoto:  
    1.  En el Administrador del servidor, haz clic en **Servicios de Escritorio remoto > Información general > + Licencias de Escritorio remoto**.  
    2.  Selecciona la máquina virtual donde se instalará el servidor de licencias de Escritorio remoto (por ejemplo, Contoso-Cb1).  
    3.  Haz clic en **Siguiente** y en **Agregar**.  
4.  Activa el servidor de licencias de Escritorio remoto y agrégalo al grupo de servidores de licencias:  
    1.  En el Administrador del servidor, haz clic en **Herramientas > Terminal Services > Administrador de licencias de Escritorio remoto**.  
    2.  En el Administrador de licencias de Escritorio remoto, selecciona el servidor y haz clic en **Acción > Activar servidor**.  
    3.  Acepta los valores predeterminados en el Asistente para activar servidor. Continúa aceptando los valores predeterminados hasta llegar a la página **Información de la empresa** A continuación, escribe la información de tu empresa.  
    4.  Acepta los valores predeterminados de las páginas restantes hasta la página final. Desactiva la opción para **iniciar el Asistente de instalación de licencias** y haz clic en **Finalizar**.  
    5.  A continuación, haz clic en **Acción > Revisar configuración > Agregar a grupo > Aceptar**. Escribe las credenciales de un usuario en el grupo de administradores de AAD DC y regístrate como SCP. Es posible que este paso no funcione si usas Azure AD Domain Services, pero puedes ignorar cualquier advertencia o error.  
5.  Agrega el servidor de Puerta de enlace de Escritorio remoto y el nombre del certificado:  
    1.  En el Administrador del servidor, haz clic en **Servicios de Escritorio remoto > Información general > + Puerta de enlace de Escritorio remoto**.  
    2.  En el asistente para agregar servidores de puerta de enlace de Escritorio remoto, selecciona la máquina virtual donde quieres instalar el servidor de puerta de enlace de Escritorio remoto (por ejemplo, Contoso-WebGw1).  
    3.  Escribe el nombre del certificado SSL para el servidor de la puerta de enlace de Escritorio remoto usando el nombre DNS completo (FQDN) externo del servidor de la puerta de enlace de Escritorio remoto. En Azure, esta es la etiqueta de **nombre de DNS** que usa el formato servicename.location.cloudapp.azure.com. Por ejemplo, contoso.westus.cloudapp.azure.com.  
    4.  Haz clic en **Siguiente** y en **Agregar**.
6.  Crea e instala certificados autofirmados para los servidores de la puerta de enlace de Escritorio remoto y el agente de conexión de Escritorio remoto.

       > [!NOTE]
       > Si vas a proporcionar e instalar certificados de una autoridad de certificados de confianza, realiza los procedimientos del paso h al paso k de cada función. Recuerda que debes tener el archivo .pfx disponible para cada uno de estos certificados.
       
    1.  En el Administrador del servidor, haz clic en **Servicios de Escritorio remoto > Información general > Tareas > Editar propiedades de implementación**.  
    2.  Expande **Certificados** y desplázate hacia abajo hasta la tabla. Haz clic en **Puerta de enlace de Escritorio remoto > Crear nuevo certificado**.  
    3.  Escribe el nombre del certificado mediante el FQDN externo del servidor de la puerta de enlace de Escritorio remoto (por ejemplo, contoso.westus.cloudapp.azure.com) y, a continuación, escribe la contraseña.  
    4.  Selecciona **Almacenar este certificado** y busca la carpeta compartida que creaste para los certificados en un paso anterior. (Por ejemplo, \Contoso Cb1\Certificates.)  
    5.  Escribe un nombre de archivo para el certificado (por ejemplo, ContosoRdGwCert) y, a continuación, haz clic en **Guardar**.  
    6.  Selecciona **Permitir que el certificado se agregue al almacén de certificados de las entidades de certificación raíz de confianza en los equipos de destino** y haz clic en **Aceptar**.  
    7.  Haz clic en **Aplicar** y espera a que el certificado se aplique correctamente al servidor de la puerta de enlace de Escritorio remoto.  
    8.  Haz clic en **Acceso web de Escritorio remoto > Seleccionar certificado existente** .  
    9.  Busca el certificado creado para el servidor de puerta de enlace de Escritorio remoto (por ejemplo, ContosoRdGwCert) y haz clic en **Abrir**.  
    10. Escribe la contraseña del certificado, selecciona **Permitir que el certificado se agregue al almacén de certificados raíz de confianza en los equipos de destino** y haz clic en **Aceptar**.  
    11. Haz clic en **Aplicar** y espera a que el certificado se aplique correctamente al servidor de acceso web de Escritorio remoto.  
    12. Repite los pasos 1 a 11 de las secciones **Agente de conexión de Escritorio remoto: habilitar el inicio de sesión único** y **Agente de conexión de Escritorio remoto: servicios de publicación**, mediante el FQDN interno del servidor del agente de conexión de Escritorio remoto para el nombre del nuevo certificado (por ejemplo, Contoso-Cb1.Contoso.com).  
7.  Exporta certificados públicos autofirmados y cópialos en un equipo cliente. Si estás usando certificados de una entidad de certificación de confianza, puedes omitir este paso.  
    1.  Inicia certlm.msc.  
    2.  Expande la opción **Personal** y haz clic en **Certificados**.  
    3.  En el panel derecho, haz clic con el botón derecho en el certificado del agente de conexión de Escritorio remoto destinado a la autenticación del cliente, por ejemplo, **Contoso-Cb1.Contoso.com**.  
    4.  Haz clic en **Todas las tareas > Exportar**.  
    5.  Acepta las opciones predeterminadas en el Asistente para exportar certificados hasta llegar a la página **Archivo para exportar**.  
    6.  Ve a la carpeta compartida que creaste para los certificados; por ejemplo, \Contoso-Cb1\Certificates.  
    7.  Escribe un nombre de archivo (por ejemplo, ContosoCbClientCert) y, a continuación, haz clic en **Guardar**.  
    8.  Haga clic en **Siguiente**y después en **Finalizar**.  
    9.  Repite los subpasos 1 a 8 de la puerta de enlace de Escritorio remoto y el certificado web (por ejemplo, contoso.westus.cloudapp.azure.com), dando al certificado exportado un nombre de archivo apropiado como, por ejemplo, **ContosoWebGwClientCert**.  
    10. En el Explorador de archivos, ve a la carpeta donde se almacenan los certificados, por ejemplo, \Contoso-Cb1\Certificates.  
    11. Selecciona los dos certificados de cliente exportados, haz clic con el botón derecho en ellos y, a continuación, haz clic en **Copiar**.  
    12. Pega los certificados en el equipo cliente local.  
8.  Configura las propiedades de implementación de la puerta de enlace de Escritorio remoto y de la Licencia de Escritorio remoto:  
    1.  En el Administrador del servidor, haz clic en **Servicios de Escritorio remoto > Información general > Tareas > Editar propiedades de implementación**.  
    2.  Expande la **puerta de enlace de Escritorio remoto** y borra la opción **Omitir el servidor de puerta de enlace de Escritorio remoto en las direcciones locales**.  
    3.  Expande la **licencia de RD** y selecciona **Por usuario**  
    4.  Haga clic en **Aceptar**.  
10. Crea una colección de sesión. Estos pasos crean una colección básica. Consulta [Crear una colección de Servicios de escritorio remoto para escritorios y aplicaciones se ejecuten](rds-create-collection.md) para obtener más información sobre las colecciones.
 
    1.  En el Administrador del servidor, haz clic en **Servicios de Escritorio remoto > Colecciones > Tareas > Crear colección de sesión** .  
    2.  Escribe un nombre de colección (por ejemplo, ContosoDesktop).  
    3.  Selecciona un servidor host de sesión de Escritorio remoto (Contoso-Sh1), acepta los grupos de usuarios predeterminados (Contoso\Usuarios de dominio) y escribe la ruta de acceso de la Convención de nomenclatura universal (UNC) en los discos de perfil de usuario creados anteriormente (\Contoso-Cb1\UserDisks).  
    4.  Establece un tamaño máximo y luego haz clic en **Crear**.  
  

De esta manera crearás una infraestructura básica de Servicios de Escritorio remoto. Si necesitas crear una implementación de alta disponibilidad, puedes agregar un [clúster de agente de conexión](rds-connection-broker-cluster.md) o un [segundo servidor de host para la sesión de Escritorio remoto](rds-scale-rdsh-farm.md).

