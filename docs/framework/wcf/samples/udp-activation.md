---
title: "UDP-Aktivierung | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 4b0ccd10-0dfb-4603-93f9-f0857c581cb7
caps.latest.revision: 15
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 15
---
# UDP-Aktivierung
Dieses Beispiel basiert auf dem Beispiel [Transport: UDP](../../../../docs/framework/wcf/samples/transport-udp.md).Es erweitert das Beispiel [Transport: UDP](../../../../docs/framework/wcf/samples/transport-udp.md) um Unterstützung von Prozessaktivierung mittels WAS \(Windows Process Activation Service, Windows\-Prozessaktivierungsdienst\).  
  
 Das Beispiel besteht im Wesentlichen aus drei Teilen:  
  
-   Einem UDP\-Protokoll\-Aktivierer, welcher ein eigenständiger Prozess ist, der für zu aktivierende Anwendungen UDP\-Nachrichten empfängt.  
  
-   Einem Client, der mit dem benutzerdefinierten UDP\-Transport Nachrichten sendet.  
  
-   Einem Dienst, der in einem von WAS aktivierten Workerprozess gehostet wird und Nachrichten über den benutzerdefinierten UDP\-Transport empfängt.  
  
## UDP\-Protokoll\-Aktivierer  
 Der UDP\-Protokoll\-Aktivierer ist eine Brücke zwischen dem [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)]\-Client und dem [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)]\-Dienst.Er bietet Datenkommunikation über das UDP\-Protokoll auf der Transportebene.Seine zwei Hauptfunktionen sind:  
  
-   WAS\-Listeneradapter \(LA\), der mit WAS zusammenarbeitet, um als Antwort auf eingehende Nachrichten Prozesse zu aktivieren.  
  
-   UDP\-Protokolllistener, der für zu aktivierende Anwendungen UDP\-Nachrichten entgegennimmt.  
  
 Der Aktivierer muss auf dem Server als eigenständiges Programm ausgeführt werden.WAS\-Listeneradapter \(wie der NetTcpActivator und der NetPipeActivator\) werden meist in Windows\-Diensten mit langer Laufzeit implementiert.Im vorliegenden Beispiel wird der Protokollaktivierer aber aus Gründen der Einfachheit und Übersichtlichkeit als eigenständige Anwendung implementiert.  
  
### WAS\-Listeneradapter  
 Der WAS\-Listeneradapter für UDP ist in der `UdpListenerAdapter`\-Klasse implementiert.Das ist das Modul, das mit WAS interagiert, um Anwendungen für das UDP\-Protokoll zu aktivieren.Dies erfolgt durch Aufrufen der folgenden Webhost\-APIs:  
  
-   `WebhostRegisterProtocol`  
  
-   `WebhostUnregisterProtocol`  
  
-   `WebhostOpenListenerChannelInstance`  
  
-   `WebhostCloseAllListenerChannelInstances`  
  
 Nach dem ersten Aufrufen von `WebhostRegisterProtocol` empfängt der Listeneradapter den `ApplicationCreated`\-Rückruf von WAS für alle in applicationHost.config \(die sich unter %windir%\\system32\\inetsrv befindet\) registrierten Anwendungen.In diesem Beispiel werden nur die Anwendungen mit aktiviertem UDP\-Protokoll verarbeitet \(mit der Protokoll\-ID als "net.udp"\).In anderen Implementierungen kann dies anders gehalten werden, wenn diese Implementierungen auf dynamische Konfigurationsänderungen bei der Anwendung reagieren \(wie zum Beispiel der Übergang einer Anwendung von aktiviert zu deaktiviert\).  
  
 Wenn der `ConfigManagerInitializationCompleted`\-Rückruf empfangen wird, zeigt dies an, dass WAS sämtliche Benachrichtigungen für die Initialisierung des Protokolls abgeschlossen hat.Ab diesem Zeitpunkt ist der Listeneradapter bereit, Aktivierungsanforderungen zu verarbeiten.  
  
 Wenn eine neue Anforderung für eine Anwendung zum ersten Mal eingeht, ruft der Listeneradapter `WebhostOpenListenerChannelInstance` in WAS auf, wodurch der Workerprozess gestartet wird \(wenn er nicht bereits gestartet ist\).Anschließend werden die Protokollhandler geladen, und die Kommunikation zwischen dem Listeneradapter und der virtuellen Anwendung kann beginnen.  
  
 Der Listeneradapter wird in der ApplicationHost.config \(unter %SystemRoot%\\System32\\inetsrv\\\) im Abschnitt \<`listenerAdapters`\> wie folgt registriert:  
  
```  
<add name="net.udp" identity="S-1-5-21-2127521184-1604012920-1887927527-387045" />  
```  
  
### Protokolllistener  
 Der UDP\-Protokolllistener ist ein Modul innerhalb des Protokollaktivierers, der für die virtuelle Anwendung einen UDP\-Endpunkt überwacht.Er ist in der `UdpSocketListener`\-Klasse implementiert.Der Endpunkt liegt als `IPEndpoint` vor, bei dem die Portnummer aus der Bindung des Protokolls für die Site extrahiert wird.  
  
### Steuerungsdienst  
 Im vorliegenden Beispiel wird [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] für die Kommunikation zwischen dem Aktivierer und dem WAS\-Workerprozess verwendet.Der Dienst, der sich im Aktivierer befindet, wird als "Steuerungsdienst" bezeichnet.  
  
## Protokollhandler  
 Nachdem der Listeneradapter `WebhostOpenListenerChannelInstance` aufgerufen hat, startet der WAS\-Prozess\-Manager den Workerprozess \(falls dieser noch nicht gestartet ist\).Anschließend lädt der im Workerprozess befindliche Anwendungs\-Manager den UDP\-Prozessprotokollhandler \(PPH\) mit der Anforderung nach dieser `ListenerChannelId`.Der PPH wiederum ruft `IAdphManager`.`StartAppDomainProtocolListenerChannel` auf, um den UDP\-AppDomain\-Protokollhandler \(ADPH\) zu starten.  
  
## HostedUDPTransportConfiguration  
 Diese Informationen werden wie folgt in der Web.config registriert:  
  
```  
<serviceHostingEnvironment>  
<add name="net.udp" transportConfigurationType="Microsoft.ServiceModel.Samples.Hosting.HostedUdpTransportConfiguration, UdpActivation, Version=1.0.0.0, Culture=neutral, PublicKeyToken=6fa904d2da1848d6" />  
</serviceHostingEnvironment>  
```  
  
## Spezielle Einrichtung für dieses Beispiel  
 Dieses Beispiel kann nur unter Windows Vista, Windows Server 2008 oder Windows 7 erstellt und ausgeführt werden.Zum Ausführen des Beispiels müssen Sie zuerst sämtliche Komponenten ordnungsgemäß einrichten.Gehen Sie folgendermaßen vor, um das Beispiel zu installieren.  
  
#### So richten Sie dieses Beispiel ein  
  
1.  Installieren Sie [!INCLUDE[vstecasp](../../../../includes/vstecasp-md.md)] 4.0 mithilfe des folgenden Befehls.  
  
    ```  
    %windir%\Microsoft.NET\Framework\v4.0.XXXXX\aspnet_regiis.exe /i /enable  
  
    ```  
  
2.  Erstellen Sie das Projekt unter Windows Vista.Nach dem Kompilieren führt es in der Postbuildphase auch die folgenden Vorgänge aus:  
  
    -   Es installiert die UDP\-Bindung für die Site "Default Web Site".  
  
    -   Es erstellt den "ServiceModelSamples" der virtuellen Anwendung so, dass er auf den physischen Pfad zeigt: "%SystemDrive%\\inetpub\\wwwroot\\servicemodelsamples".  
  
    -   Außerdem aktiviert es für diese virtuelle Anwendung das "net.udp"\-Protokoll.  
  
3.  Starten Sie die Benutzeroberflächenanwendung "WasNetActivator.exe".Klicken Sie auf die Registerkarte **Einrichtung**, aktivieren Sie die folgenden Kontrollkästchen, und klicken Sie dann auf **Installieren**, um sie zu installieren:  
  
    -   UDP\-Listeneradapter  
  
    -   UDP\-Protokollhandler  
  
4.  Klicken Sie auf die Registerkarte **Aktivierung** der Benutzeroberflächenanwendung "WasNetActivator.exe".Klicken Sie auf die Schaltfläche **Starten**, um den Listeneradapter zu starten.Nun können Sie das Programm ausführen.  
  
    > [!NOTE]
    >  Wenn Sie die Arbeit mit diesem Beispiel abgeschlossen haben, müssen Sie Cleanup.bat ausführen, um die net.udp\-Bindung aus der "Standardwebsite" zu entfernen.  
  
## Verwendungsbeispiel  
 Nach dem Kompilieren liegen vier verschiedene Binärdateien vor:  
  
-   Client.exe: Der Clientcode.Die App.config ist in die Client.exe\-Clientkonfigurationsdatei hinein kompiliert.  
  
-   UDPActivation.dll: Die Bibliothek, die alle wichtigen UDP\-Implementierungen enthält.  
  
-   Service.dll: Der Dienstcode.Diese wird in das Verzeichnis \\bin der virtuellen Anwendung ServiceModelSamples kopiert.Die Dienstdatei ist Service.svc, und die Konfigurationsdatei ist Web.config.Nach dem Kompilieren werden sie an den folgenden Speicherort kopiert: %SystemDrive%\\Inetpub\\wwwroot\\ServiceModelSamples.  
  
-   WasNetActivator: Das UDP\-Aktiviererprogramm.  
  
-   Stellen Sie sicher, dass alle erforderlichen Bestandteile ordnungsgemäß installiert sind.Die folgenden Schritte zeigen, wie das Beispiel ausgeführt wird:  
  
1.  Stellen Sie sicher, dass die folgenden Windows\-Dienste gestartet sind:  
  
    -   WAS \(Windows Process Activation Service\)  
  
    -   Internetinformationsdienste \(IIS\): W3SVC.  
  
2.  Starten Sie anschließend den Aktivierer \(WasNetActivator.exe\).Auf der Registerkarte **Aktivierung** ist in der Dropdownliste als einziges Protokoll **UDP** ausgewählt.Klicken Sie auf die Schaltfläche **Starten**, um den Aktivierer zu starten.  
  
3.  Wenn der Aktivierer gestartet ist, können Sie den Clientcode ausführen, indem Sie in einem Befehlsfenster die Client.exe ausführen.Nachfolgend ist die Ausgabe des Beispiels aufgeführt:  
  
    ```  
    Testing Udp Activation.  
    Start the status service.  
    Sending UDP datagrams.  
    Type a word that you want to say to the server: Hello, world!  
        Sending datagram: Hello, world![0]  
        Sending datagram: Hello, world![1]  
        Sending datagram: Hello, world![2]  
        Sending datagram: Hello, world![3]  
        Sending datagram: Hello, world![4]  
    Calling UDP duplex contract (ICalculatorContract).  
        0 + 0 = 0  
        1 + 2 = 3  
        2 + 4 = 6  
        3 + 6 = 9  
        4 + 8 = 12  
    Getting status and dump server traces:  
        Operation 'Hello' is called: Hello, world![0]  
        Operation 'Hello' is called: Hello, world![1]  
        Operation 'Hello' is called: Hello, world![2]  
        Operation 'Hello' is called: Hello, world![3]  
        Operation 'Hello' is called: Hello, world![4]  
        Operation 'Add' is called: 0 + 0  
        Operation 'Add' is called: 1 + 2  
        Operation 'Add' is called: 2 + 4  
        Operation 'Add' is called: 3 + 6  
        Operation 'Add' is called: 4 + 8  
    Press <ENTER> to complete test.  
    ```  
  
> [!IMPORTANT]
>  Die Beispiele sind möglicherweise bereits auf dem Computer installiert.Überprüfen Sie das folgende \(standardmäßige\) Verzeichnis, bevor Sie fortfahren.  
>   
>  `<Installationslaufwerk>:\WF_WCF_Samples`  
>   
>  Wenn dieses Verzeichnis nicht vorhanden ist, rufen Sie [Windows Communication Foundation \(WCF\) and Windows Workflow Foundation \(WF\) Samples for .NET Framework 4](http://go.microsoft.com/fwlink/?LinkId=150780) auf, um alle [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)]\- und [!INCLUDE[wf1](../../../../includes/wf1-md.md)]\-Beispiele herunterzuladen.Dieses Beispiel befindet sich im folgenden Verzeichnis.  
>   
>  `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Transport\UdpActivation`  
  
## Siehe auch