---
title: "(Veraltet:) Binomial Distribution Suite – Azure | Microsoft Docs"
description: (Veraltet:) Binomial Distribution Suite
services: machine-learning
documentationcenter: 
author: ireiter
manager: jhubbard
editor: cgronlun
ms.assetid: 6d102d57-8f20-4ab3-be31-02fcfe4d15ed
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: ireiter
ROBOTS: NOINDEX
redirect_url: https://gallery.cortanaintelligence.com/
redirect_document_id: TRUE
ms.translationtype: Human Translation
ms.sourcegitcommit: f6ad106e769c807d1c281c8d19127eabc2048f30
ms.openlocfilehash: 4d4a343be86909acf054eaaf9cc4a1b0df5a4209
ms.contentlocale: de-de
ms.lasthandoff: 01/11/2017


---
# <a name="deprecated-binomial-distribution-suite"></a>(Veraltet:) Binomial Distribution Suite

> [!NOTE]
> Der Microsoft DataMarket wird eingestellt, und diese API gilt nun als veraltet. 
> 
> Im [Cortana Intelligence-Katalog](http://gallery.cortanaintelligence.com) finden Sie viele nützliche Beispielexperimente und -APIs. Weitere Informationen zum Katalog finden Sie unter [Teilen und Entdecken von Ressourcen im Cortana Intelligence-Katalog](machine-learning-gallery-how-to-use-contribute-publish.md).

Die Binomial Distribution Suite ist ein Satz mit Beispielwebdiensten ([Binomial Generator](https://datamarket.azure.com/dataset/aml_labs/bdg5), [Probability Calculator](https://datamarket.azure.com/dataset/aml_labs/bdp4), [Quantile Calculator](https://datamarket.azure.com/dataset/aml_labs/bdq5)), mit denen Sie Binomialverteilungen generieren und verarbeiten können. Mit den Diensten können Sie eine Binomialverteilungssequenz beliebiger Länge generieren, die Quantile aus angegebenen Wahrscheinlichkeiten und Wahrscheinlichkeiten aus einem angegebenen Quantil errechnen. Alle Dienste machen unterschiedliche Ausgaben basierend auf dem ausgewählten Dienst (siehe Beschreibung unten). Die Binomial Distribution Suite basiert auf den R-Funktionen "qbinom", "rbinom" und "pbinom", die im R-Statistikpaket enthalten sind. 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

> Diese Webdienste können von Benutzern verwendet werden – beispielsweise direkt im Marketplace, über eine mobile App, eine Website oder sogar über einen lokalen Computer. Dieser Webdienst ist jedoch auch ein gutes Beispiel dafür, wie Azure Machine Learning zum Erstellen von Webdiensten basierend auf R-Code verwendet werden kann. Mit nur wenigen Codezeilen R-Code und einigen Klicks in Azure Machine Learning Studio können Sie ein Experiment mit R-Code erstellen und als Webdienst veröffentlichen. Der Webdienst kann dann im Azure Marketplace veröffentlicht und von Benutzern und Geräten auf der ganzen Welt genutzt werden – ohne Einrichtung einer Infrastruktur durch den Autor des Webdiensts.
> 
> 

## <a name="consumption-of-web-service"></a>Nutzung des Webdiensts
Die Binomial Distribution Suite enthält die folgenden drei Dienste.

### <a name="binomial-distribution-quantile-calculator"></a>Binomial Distribution Quantile Calculator
Dieser Dienst akzeptiert vier Argumente einer Normalverteilung und berechnet die zugehörigen Quantile.
Die Eingabeargumente sind:

* p – eine einzige aggregierte Wahrscheinlichkeit mehrerer Versuche  
* size – Anzahl von Versuchen
* prob – Wahrscheinlichkeit des Erfolgs eines Versuchs
* side – L für den unteren Rand der Verteilung, U für den oberen Rand der Verteilung 

Die Ausgabe des Diensts ist das berechnete Quantil, das zur angegebenen Wahrscheinlichkeit gehört.

### <a name="binomial-distribution-probability-calculator"></a>Binomial Distribution Probability Calculator
Dieser Dienst akzeptiert vier Argumente einer Binomialverteilung und berechnet die zugehörige Wahrscheinlichkeit.
Die Eingabeargumente sind:

* Q – ein einzelnes Quantil eines Ereignisses mit Binomialverteilung 
* size – Anzahl von Versuchen
* prob – Wahrscheinlichkeit des Erfolgs eines Versuchs
* side – L für den unteren Rand der Verteilung, U für den oberen Rand der Verteilung oder E, was einer einzelnen Zahl von Erfolgen entspricht.

Die Ausgabe des Diensts ist die berechnete Wahrscheinlichkeit, die dem angegebenen Quantil zugeordnet ist.

### <a name="binomial-distribution-generator"></a>Binomial Distribution Generator
Dieser Dienst akzeptiert 3 Argumente einer Binomialverteilung und generiert eine zufällige Sequenz von Zahlen, die binomial verteilt werden. Die folgenden Argumente sollten innerhalb der Anforderung bereitgestellt werden:

* n – Anzahl von Beobachtungen 
* size – Anzahl von Versuchen
* prob – Wahrscheinlichkeit des Erfolgs

Die Ausgabe des Diensts ist eine Sequenz der Länge n mit einer Binomialverteilung anhand der Argumente size und prob.

> Dieser Dienst, der im Azure Marketplace gehostet wird, ist ein OData-Dienst. Diese Dienste können durch POST- oder GET-Methoden aufgerufen werden. 
> 
> 

Es gibt mehrere Möglichkeiten, den Dienst auf automatisierte Weise zu nutzen (Beispiel-Apps finden Sie hier: [Generator](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionGenerator.aspx), [Probability Calculator](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionProbabilityCalculator.aspx), [Quantile Calculator](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionQuantileCalculator)). 

### <a name="starting-c-code-for-web-service-consumption"></a>Starten von C#-Code für Webdienstnutzung:
### <a name="binomial-distribution-quantile-calculator"></a>Binomial Distribution Quantile Calculator
    public class Input
    {
            public string p;
            public string size;
            public string prob;
            public string side;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void main()
    {
            var input = new Input() { p = TextBox1.Text, size = TextBox2.Text, prob = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();

            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }

### <a name="binomial-distribution-probability-calculator"></a>Binomial Distribution Probability Calculator
    public class Input
    {
            public string q;
            public string size;
            public string prob;
            public string side;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { q = TextBox1.Text, size = TextBox2.Text, prob = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = " PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();

            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }


### <a name="binomial-distribution-generator"></a>Binomial Distribution Generator
    public class Input
    {
            public string n;
            public string size;
            public string p;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { n = TextBox1.Text, size = TextBox2.Text, p = TextBox3.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();

            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }





## <a name="creation-of-web-service"></a>Erstellen des Webdiensts
> Dieser Webdienst wurde mithilfe von Azure Machine Learning erstellt. Eine kostenlose Testversion sowie Einführungsvideos zum Erstellen von Experimenten und [Veröffentlichen von Webdiensten](machine-learning-publish-a-machine-learning-web-service.md) finden Sie unter [azure.com/ml](http://azure.com/ml). Im Folgenden finden Sie einen Screenshot des Experiments, mit dem der Webdienst erstellt wurde und Beispielcode für die einzelnen Module im Experiment.
> 
> 

### <a name="binomial-distribution-quantile-calculator"></a>Binomial Distribution Quantile Calculator
![Arbeitsbereich erstellen][4]

#### <a name="module-1"></a>Modul 1:
    #data schema with example data (replaced with data from web service)
    data.set=data.frame(p=0.1,size=10,prob=.5,side='L');
    maml.mapOutputPort("data.set"); #send data to output port
#### <a name="module-2"></a>Modul 2:
    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    if (param$p < 0 ) {
    print('Bad input: p must be between 0 and 1')
    param$p = 0
    } else if (param$p > 1) {
    print('Bad input: p must be between 0 and 1')
    param$p = 1
    }

    if (param$prob < 0 ) {
    print('Bad input: prob must be between 0 and 1')
    param$prob = 0
    } else if (param$prob > 1) {
    print('Bad input: prob must be between 0 and 1')
    param$prob = 1
    }

    quantile = qbinom(param$p,size=param$size,prob=param$prob)
    df = data.frame(x=1:param$size, prob=dbinom(1:param$size, param$size, prob=param$prob))
    quantile

    if (param$side == 'U'){
    quantile = qbinom(param$p,size=param$size,prob=param$prob,lower.tail = F)
    band=subset(df,x>quantile)
    } else if (param$side =='L') {
    quantile = qbinom(param$p,size=param$size,prob=param$prob,lower.tail = T)
    band=subset(df,x<=quantile)
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(quantile)

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");


### <a name="binomial-distribution-probability-calculator"></a>Binomial Distribution Probability Calculator
![Arbeitsbereich erstellen][5]

#### <a name="module-1"></a>Modul 1:
    #data schema with example data (replaced with data from web service)
    data.set=data.frame(q=5,size=10,prob=.5,side='L');
    maml.mapOutputPort("data.set"); #send data to output port


#### <a name="module-2"></a>Modul 2:
    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    prob = pbinom(param$q,size=param$size,prob=param$prob)
    prob.eq = dbinom(param$q,size=param$size,prob=param$prob)
    df = data.frame(x=1:param$size, prob=dbinom(1:param$size, param$size, prob=param$prob))
    prob

    if (param$side == 'U'){
    prob = 1 - prob
    band=subset(df,x>param$q)
    } else if (param$side =='E') {
    prob = prob.eq
    band=subset(df,x==param$q)
    } else if (param$side =='L') {
    prob = prob
    band=subset(df,x<=param$q)
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(prob)

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

### <a name="binomial-distribution-generator"></a>Binomial Distribution Generator
![Arbeitsbereich erstellen][6]

#### <a name="module-1"></a>Modul 1:
    #data schema with example data (replaced with data from web service)
    data.set=data.frame(n=50,size=10,p=.5);
    maml.mapOutputPort("data.set"); #send data to output port

#### <a name="module-2"></a>Modul 2:
    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    dist = rbinom(param$n,param$size,param$p)

    output = as.data.frame(t(dist))

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

## <a name="limitations"></a>Einschränkungen
Dies sind sehr einfache Beispiele zum Thema Binomialverteilung. Wie aus den oben stehenden Beispielcode ersichtlich ist, wird kaum Abfangen von Fehlern implementiert.

## <a name="faq"></a>Häufig gestellte Fragen
Häufig gestellte Fragen zur Nutzung des Webdiensts und zum Veröffentlichen im Azure Marketplace finden Sie [hier](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_1.png

[2]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_2.png

[3]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_3.png

[4]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_4.png

[5]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_5.png

[6]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_6.png


