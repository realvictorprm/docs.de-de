---
title: "Ausw&#228;hlen eines Nachrichtenencoders | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 2204d82d-d962-4922-a79e-c9a231604f19
caps.latest.revision: 19
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 19
---
# Ausw&#228;hlen eines Nachrichtenencoders
In diesem Thema werden die Kriterien für die Auswahl eines in [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] enthaltenen Encoders beschrieben: Binärdaten, Textdaten und MTOM \(Message Transmission Optimization Mechanism\).  
  
 In [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] geben Sie an, wie Daten in einem Netzwerk über eine *Bindung* zwischen Endpunkten übertragen werden. Eine Bindung besteht aus einer Folge von *Bindungselementen*.Ein Nachrichtenencoder wird von einem Nachrichtencodierungs\-Bindungselement im Bindungsstapel dargestellt.Eine Bindung enthält optionale Protokollbindungselemente, wie ein Sicherheitsbindungselement oder ein zuverlässiges Nachrichtenbindungselement, ein erforderliches Nachrichtencodierungs\-Bindungselement und ein erforderliches Transportbindungselement.  
  
 Das Nachrichtencodierungs\-Bindungselement befindet sich unter den optionalen Protokollbindungselementen und über dem erforderlichen Transportbindungselement.Auf der ausgehenden Seite serialisiert ein Nachrichtenencoder die ausgehende <xref:System.ServiceModel.Channels.Message> und übergibt sie an den Transport.Auf der eingehenden Seite ruft ein Nachrichtenencoder die serialisierte Form einer <xref:System.ServiceModel.Channels.Message> aus dem Transport ab und übergibt sie an die höhere Protokollschicht, falls vorhanden, oder andernfalls an die Anwendung.  
  
 Wenn Sie eine Verbindung mit einem zuvor vorhandenen Client oder Server herstellen, haben Sie eventuell keine Wahl, ob Sie eine bestimmte Nachrichtencodierung verwenden möchten, da Sie Ihre Nachrichten auf eine Weise codieren müssen, die von der anderen Seite erwartet wird.Wenn Sie jedoch einen [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)]\-Dienst schreiben, können Sie Ihren Dienst über mehrere Endpunkte verfügbar machen, von denen jeder eine andere Nachrichtencodierung verwendet.So können Clients die beste Codierung für die Kommunikation mit Ihrem Dienst über den am besten geeigneten Endpunkt auswählen. Außerdem verfügen Sie dann über die Flexibilität, die für die Clients am besten geeignete Codierung auszuwählen.Die Verwendung von mehreren Endpunkten ermöglicht Ihnen, die Vorteile anderer Nachrichtencodierungen mit anderen Bindungselementen zu kombinieren.  
  
## Vom System bereitgestellte Encoder  
 [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] schließt drei Nachrichtenencoder ein, die durch die folgenden drei Klassen dargestellt werden:  
  
-   <xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement>, der Textnachrichtenencoder, unterstützt sowohl reine XML\-Codierung als auch SOAP\-Codierung.Der reine XML\-Codierungsmodus des Textnachrichtenencoders wird als "Plain Old XML" \(POX\) bezeichnet, um ihn von der textbasierten SOAP\-Codierung zu unterscheiden.Um POX zu aktivieren, legen Sie die <xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement.MessageVersion%2A>\-Eigenschaft auf <xref:System.ServiceModel.Channels.MessageVersion.None%2A> fest.Verwenden Sie den Textnachrichtenencoder, um mit anderen Endpunkten zusammenzuarbeiten als mit [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)]\-Endpunkten.  
  
-   <xref:System.ServiceModel.Channels.BinaryMessageEncodingBindingElement>, der binäre Nachrichtenencoder, verwendet ein kompaktes binäres Format und ist für die Kommunikation zwischen [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] und [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] optimiert und daher nicht interoperabel.Dies ist auch der leistungsstärkste Encoder, der von [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] bereitgestellt wird.  
  
-   Das <xref:System.ServiceModel.Channels.MTOMMessageEncodingBindingElement>\-Bindungselement gibt die Zeichencodierung und die für MTOM verwendete Nachrichtenversionsverwaltung an.MTOM ist eine effiziente Technologie zum Senden von Binärdaten in [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)]\-Nachrichten.Der MTOM\-Encoder versucht, einen Ausgleich zwischen Effizienz und Interoperabilität zu schaffen.Die MTOM\-Codierung überträgt die meisten XML\-Daten in Textform, optimiert aber große Binärdatenblöcke durch Übertragung ohne Textkonvertierung.Hinsichtlich Effizienz steht MTOM unter den von [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] bereitgestellten Encodern zwischen Text \(dem langsamsten Encoder\) und Binärdaten \(dem schnellsten Encoder\).  
  
## So wählen Sie einen Nachrichtenencoder aus  
 In der folgenden Tabelle werden allgemeine Faktoren beschrieben, die bei der Auswahl eines Nachrichtenencoders berücksichtigt werden.Priorisieren Sie die Faktoren, die für Ihre Anwendung wichtig sind, und wählen Sie dann die Nachrichtenencoder aus, die mit diesen Faktoren am besten funktionieren.Berücksichtigen Sie alle weiteren, in dieser Tabelle nicht aufgelisteten Faktoren sowie alle benutzerdefinierten Encoder, die eventuell für Ihre Anwendung erforderlich sind.  
  
|Faktor|Beschreibung|Encoder, die diesen Faktor unterstützen|  
|------------|------------------|---------------------------------------------|  
|Unterstützte Zeichensätze|<xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement> und <xref:System.ServiceModel.Channels.MtomMessageEncodingBindingElement> unterstützen nur die UTF8\- und UTF16\-Unicode\-Codierungen \(*Big\-Endian* und *Little\-Endian*\).Sind andere Codierungen erforderlich \(beispielsweise UTF7 oder ASCII\), muss ein benutzerdefinierter Encoder verwendet werden.Ein Beispiel für einen benutzerdefinierten Encoder finden Sie unter [Custom Message Encoder](http://go.microsoft.com/fwlink/?LinkId=119857).|Text|  
|Inspektion|Inspektion ist die Fähigkeit, Nachrichten während der Übertragung zu untersuchen.Textcodierungem, entweder mit oder ohne die Verwendung von SOAP, ermöglichen die Inspektion und Analyse von Nachrichten mit vielen Anwendungen, ohne spezielle Tools zu verwenden.Beachten Sie, dass sich die Verwendung der Übertragungssicherheit, entweder auf Nachrichten\- oder auf Transportebene, auf die Inspektionsfähigkeit der Nachrichten auswirkt.Vertraulichkeit schützt eine Nachricht davor, untersucht zu werden, und Integrität schützt eine Nachricht davor, geändert zu werden.|Text|  
|Zuverlässigkeit|Zuverlässigkeit ist die Widerstandsfähigkeit eines Encoders gegen Übertragungsfehler.Zuverlässigkeit kann auch auf Nachrichten\-, Transport\- oder Anwendungsebene bereitgestellt werden.Bei allen [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)]\-Standardencodern wird davon ausgegangen, dass eine andere Ebene die Zuverlässigkeit bietet.Der Encoder hat wenige Möglichkeiten, nach einem Übertragungsfehler eine Wiederherstellung durchzuführen.|Keine|  
|Einfachheit|Einfachheit stellt die Leichtigkeit dar, mit der Sie Encoder und Decoder für eine Codierungsspezifikation erstellen können.Textcodierungen sind besonders vorteilhaft für die Einfachheit. Die POX\-Textcodierung hat den zusätzlichen Vorteil, dass keine Unterstützung der SOAP\-Verarbeitung erforderlich ist.|Text \(POX\)|  
|Größe|Die Codierung bestimmt den Mehraufwand für den Inhalt.Die Größe der codierten Nachrichten bezieht sich direkt auf den maximalen Durchsatz von Dienstvorgängen.Binäre Codierungen sind im Allgemeinen kompakter als Textcodierungen.Wenn die Nachrichtengröße ein Maximum erreicht hat, sollten Sie auch eine Komprimierung des Nachrichteninhalts während der Codierung in Erwägung ziehen.Durch die Komprimierung entstehen jedoch höhere Verarbeitungskosten für den Absender und den Empfänger der Nachricht.|Binär|  
|Streaming|Streaming ermöglicht Anwendungen, mit der Verarbeitung einer Nachricht zu beginnen, bevor die ganze Nachricht angekommen ist.Wenn Sie das Streaming effektiv anwenden möchten, ist es erforderlich, dass die wichtigen Daten einer Nachricht am Anfang der Nachricht zur Verfügung stehen, sodass die empfangende Anwendung nicht warten muss, bis die Nachricht ankommt.Außerdem müssen Anwendungen, die eine Übertragung per Stream nutzen, die Daten in der Nachricht inkrementell organisieren, damit der Inhalt keine Weiterleitungsabhängigkeiten aufweist.In vielen Fällen müssen Sie einen Kompromiss schließen zwischen dem Streaminginhalt und der kleinstmöglichen Übertragungsgröße dieses Inhalts.|Keine \(None\)|  
|Unterstützung von Drittanbietertools|Unterstützungsbereiche für eine Codierung umfassen Entwicklung und Diagnose.Drittanbieter haben viel investiert in Bibliotheken und Toolkits zur Verarbeitung von Nachrichten, die im POX\-Format codiert sind.|Text \(POX\)|  
|Interoperabilität|Dieser Faktor bezieht sich auf die Fähigkeit eines [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)]\-Encoders, mit [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)]\-Diensten zu interoperieren.|Text<br /><br /> MTOM \(teilweise\)|  
  
 Hinweis: Bei Verwendung des Binärencoders hat die IgnoreWhitespace\-Einstellung keine Aufwirkung, wenn ein XMLReader erstellt wird.Wenn Sie beispielsweise Folgendes innerhalb eines Dienstvorgangs ausführen:  
  
```csharp  
public void OperationContract(XElement input)  
        {  
            Console.WriteLine("{0}", input.Value);  
            int counter = 0;  
            var xreader = input.CreateReader();  
            var reader = XmlReader.Create(xreader, new XmlReaderSettings() { IgnoreWhitespace = true });  
            while (reader.Read())  
            {  
                counter++;  
            }  
  
            Console.WriteLine("Read {0} lines with reader", counter);  
        }  
  
```  
  
 Die IgnoreWhitespace\-Einstellung wird ignoriert.  
  
## Komprimierung und Binärencoder  
 Ab WCF 4.5 bietet der WCF\-Binärencoder Komprimierungsunterstützung.Dies ermöglicht Ihnen, den gzip\/deflate\-Algorithmus zum Senden komprimierter Nachrichten von einem WCF\-Client zu senden und mit komprimierten Nachrichten von einem selbstgehosteten WCF\-Dienst zu antworten.Diese Funktion ermöglicht die Komprimierung sowohl bei HTTP\- als auch bei TCP\-Transporten.Ein IIS\-gehosteter WCF\-Dienst kann zum Senden von komprimierten Antworten befähigt werden, indem der IIS\-Hostserver konfiguriert wird.Der Komprimierungstyp wird mit der <xref:System.ServiceModel.Channels.BinaryMessageEncodingElement.CompressionFormat%2A>\-Eigenschaft konfiguriert.Diese Eigenschaft wird auf einen der <xref:System.ServiceModel.Channels.CompressionFormat>\-Enumerationswerte festgelegt:  
  
|CompressionFormat\-Wert|Beschreibung|  
|-----------------------------|------------------|  
|F:System.ServiceModel.Channels.CompressionFormat.Deflate|Verwendet die Deflate\-Komprimierung.|  
|F:System.ServiceModel.Channels.CompressionFormat.GZip|Verwendet die GZip\-Komprimierung.|  
|F:System.ServiceModel.Channels.CompressionFormat.None|Es wird keine Komprimierung verwendet.|  
  
 Da diese Eigenschaft nur auf dem binaryMessageEncodingBindingElement verfügbar gemacht wird, müssen Sie eine benutzerdefinierte Bindung wie die folgende erstellen, um diese Funktion zu verwenden:\<customBinding\>        \<binding name\="BinaryCompressionBinding"\>          \<binaryMessageEncoding compressionFormat \="GZip"\/\>          \<httpTransport \/\>        \<\/binding\>      \<\/customBinding\>Sowohl der Client als auch der Dienst müssen einwilligen komprimierte Meldungen zu senden und zu empfangen. Daher muss die compressionFormat\-Eigenschaft für das binaryMessageEncoding\-Element sowohl auf dem Client als auch auf dem Dienst konfiguriert werden.Eine ProtocolException wird ausgelöst, wenn der Dienst oder der Client nicht für die Komprimierung konfiguriert wurde, die andere Seite allerdings schon. Das Aktivieren der Komprimierung sollte sorgfältig überlegt werden.Die Komprimierung ist vor allem dann nützlich, wenn Netzwerkbandbreite beschränkt ist.Wenn die CPU der Engpass ist, mindert die Komprimierung den Durchsatz.Entsprechende Tests müssen in einer simulierten Umgebung ausgeführt werden, um herauszufinden, ob dies Vorteile für die Anwendung bedeutet.  
  
## Siehe auch  
 [Bindungen](../../../../docs/framework/wcf/feature-details/bindings.md)