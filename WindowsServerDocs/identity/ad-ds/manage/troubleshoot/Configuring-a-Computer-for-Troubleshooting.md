---
ms.assetid: 155abe09-6360-4913-8dd9-7392d71ea4e6
title: "Configurar un equipo para la solución de problemas"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 915b8d1133b3bee050f5eedce9e7ac445f833048
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="configuring-a-computer-for-troubleshooting"></a>Configurar un equipo para la solución de problemas

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


<developerConceptualDocument xmlns="https://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:xlink="https://www.w3.org/1999/xlink" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://ddue.schemas.microsoft.com/authoring/2003/5 http://clixdevr3.blob.core.windows.net/ddueschema/developer.xsd">
  <introduction>
    <para>Antes de usar técnicas avanzadas de solución de problemas para identificar y solucionar problemas de Active Directory, configurar los equipos para solucionar problemas. También debe tener un conocimiento básico de <token>nextref_longhorincludes > conceptos, procedimientos y herramientas de solución de problemas. </para>
    <para>Para obtener información sobre herramientas de supervisión de Windows Server 2008, consulta el Step-by-Step Guía de supervisión del rendimiento y confiabilidad en Windows Server 2008 (<externalLink><linkText>https://go.microsoft.com/fwlink/?LinkId=123737</linkText><linkUri>https://go.microsoft.com/fwlink/?LinkId=123737</linkUri></externalLink>).</para>
  </introduction>
  <section>
    <title>Tareas de configuración para la solución de problemas</title>
    <content>
      <para>Para configurar el equipo para la solución de problemas de los servicios de dominio de Active Directory (AD DS), realice las siguientes tareas:</para>
      <para>
        <link xlink:href="#BKMK_2">instalar Remote Server Administration Tools de AD DS</link>
      </para>
      <para>
        <link xlink:href="#BKMK_3">configurar Monitor de confiabilidad y rendimiento</link>
      </para>
      <para>
        <link xlink:href="#BKMK_4">establecer niveles de registro</link>
      </para>
    </content>
    <sections>
      <section address="BKMK_2">
        <title>Instalar las herramientas de administración remota del servidor de AD DS</title>
        <content>
          <para>Al instalar AD DS para crear un controlador de dominio, las herramientas administrativas que puedes usar para administrar AD DS se instalan automáticamente. Si quieres administrar controladores de dominio de forma remota desde un equipo que no es un controlador de dominio, puedes instalar las herramientas administrativas en un servidor miembro que se está ejecutando <token>nextref_longhorincludes > o en un equipo que ejecute Windows Vista con Service Pack 1 (SP1). En los servidores miembro que se ejecutan <token>nextref_longhorincludes >, usan la característica de herramientas de administración remota del servidor (RSAT) en el administrador del servidor para instalar herramientas de servicios de dominio de Active Directory. RSAT reemplaza herramientas de soporte técnico de Windows en Windows Server 2003. También puedes instalar las herramientas de servicios de dominio de Active Directory en un equipo que ejecuta Windows Vista con Service Pack 1 (SP1), descarga las herramientas para ese equipo.</para>
          <para>Para obtener información sobre cómo instalar RSAT, consulta <link xlink:href="610ba7d9-51b5-4e14-9232-0510a9091aba">instalar Remote Server Administration Tools de AD DS</link>.</para>
        </content>
      </section>
      <section address="BKMK_3">
        <title>Configurar el Monitor de confiabilidad y rendimiento</title>
        <content>
          <para>Windows Server 2008 incluye la confiabilidad de Windows y el Monitor de rendimiento, que es un complemento Microsoft Management Console (MMC) que combina la funcionalidad de herramientas independientes anteriores, incluidos los registros de rendimiento y alertas, Asesor de rendimiento de servidor y el Monitor de sistema. Este complemento proporciona una interfaz gráfica de usuario (GUI) para personalizar los conjuntos de recopiladores de datos y sesiones de seguimiento de eventos. </para>
          <para>Monitor de confiabilidad y rendimiento también incluye el Monitor de confiabilidad, un complemento de MMC que realiza un seguimiento de cambios en el sistema y compara a los cambios en la estabilidad del sistema, proporcionando una visión gráfica de la relación entre sus. </para>
        </content>
      </section>
      <section address="BKMK_4">
        <title>Establecer niveles de registro</title>
        <content>
          <para>Si la información que recibes en el registro de servicio de directorio en el Visor de eventos no es suficiente para la solución de problemas, generar los niveles de registro usando la entrada de registro adecuadas en <embeddedLabel>HKEY_LOCAL_MACHINESYSTEMCurrentControlSetServicesNTDSDiagnostics</embeddedLabel>. </para>
          <para>De manera predeterminada, los niveles de registro para todas las entradas se establecen en <embeddedLabel>0</embeddedLabel>, que proporciona la cantidad mínima de información. Es el más alto nivel de registro <embeddedLabel>5</embeddedLabel>. Aumentar el nivel de una entrada hace que los eventos adicionales iniciar sesión en el registro de eventos de servicio de directorio. </para>
          <para>Puedes usar el siguiente procedimiento para cambiar el nivel de registro para una entrada de diagnóstico. </para>
          <alert class="caution">
            <para>Te recomendamos que no modifica directamente el registro a menos que no haya ninguna otra alternativa. Modificaciones en el registro no se validan por el editor del registro o por Windows antes de que se aplican y, como resultado, se pueden almacenar valores incorrectos. Esto puede producir errores irrecuperables en el sistema. Cuando sea posible, usa la directiva de grupo o de otras herramientas de Windows, como los complementos MMC para realizar tareas, en lugar de modificar el registro directamente. Si debes editar el registro, Ten mucho cuidado. </para>
          </alert>
          <para>
            <embeddedLabel>Requisitos</embeddedLabel>
          </para>
          <list class="bullet">
            <listItem>
              <para>Pertenencia <embeddedLabel>Administradores de dominio</embeddedLabel>, o equivalente, es lo mínimo necesario para completar este procedimiento. <token>review_detailincludes></para>
            </listItem>
            <listItem>
              <para>Herramientas: Regedit.exe</para>
            </listItem>
          </list>
          <procedure>
            <title>Para cambiar el nivel de registro para una entrada de diagnóstico</title>
            <steps class="ordered">
              <step>
                <content>
                  <para>Haga clic en <ui>Inicio</ui>, haz clic en <ui>Ejecutar</ui>, tipo <userInput>regedit</userInput> y, a continuación, haz clic en <ui>Aceptar</ui>.</para>
                </content>
              </step>
              <step>
                <content>
                  <para>Navegar a la entrada para el que deseas establecer el registro en <embeddedLabel>HKEY_LOCAL_MACHINESYSTEMCurrentControlSetServicesNTDSDiagnostics</embeddedLabel>.</para>
                </content>
              </step>
              <step>
                <content>
                  <para>Haz doble clic en la entrada y en <embeddedLabel>Base</embeddedLabel>, haz clic en <embeddedLabel>Decimal</embeddedLabel>.</para>
                </content>
              </step>
              <step>
                <content>
                  <para>En <embeddedLabel>Valor</embeddedLabel>, escribe un número entero entre <embeddedLabel>0</embeddedLabel> a través de <embeddedLabel>5</embeddedLabel> y, a continuación, haz clic en <ui>Aceptar</ui>.</para>
                </content>
              </step>
            </steps>
          </procedure>
        </content>
      </section>
    </sections>
  </section>
  <relatedTopics />
</developerConceptualDocument>


