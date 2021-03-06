---
title: Erweitern von U-SQL-Skripts um R in Azure Data Lake Analytics | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie R-Code in U-SQL-Skripts ausführen."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: sukvg
editor: cgronlun
ms.assetid: c1c74e5e-3e4a-41ab-9e3f-e9085da1d315
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/01/2017
ms.author: saveenr
ms.translationtype: Human Translation
ms.sourcegitcommit: e72275ffc91559a30720a2b125fbd3d7703484f0
ms.openlocfilehash: 3728d81243a1ac9f501bd8d7d538c1f73b954405
ms.contentlocale: de-de
ms.lasthandoff: 05/05/2017


---

# <a name="tutorial-get-started-with-extending-u-sql-with-r"></a>Tutorial: Erste Schritte beim Erweitern von U-SQL um R

Im folgenden Beispiel werden die grundlegenden Schritte für eine Bereitstellung von R-Code veranschaulicht:
* Verwenden Sie die REFERENCE ASSEMBLY-Anweisung, um R-Erweiterungen für das U-SQL-Skript zu aktivieren.
* Verwenden Sie den REDUCE-Vorgang, um die Eingabedaten für einen Schlüssel zu partitionieren.
* Die R-Erweiterungen für U-SQL enthalten einen integrierten Reducer (Extension.R.Reducer), mit dem R-Code auf jedem Scheitelpunkt ausgeführt wird, der dem Reducer zugewiesen ist. 
* Verwenden von dedizierten benannten Datenrahmen („inputFromUSQL“ und „outputToUSQL“), um Daten zwischen USQL und R zu übergeben. Die Namen von Eingabe- und Ausgabedatenrahmen-IDs sind festgelegt (das heißt, Benutzer können diese vordefinierten Namen von Eingabe- und Ausgabedatenrahmen-IDs nicht ändern).

## <a name="embedding-r-code-in-the-u-sql-script"></a>Einbetten von R-Code in das U-SQL-Skript

Sie können den R-Code mithilfe des Befehlsparameters des Reducers „Extension.R.Reducer“ in Ihr U-SQL-Skript einbinden. Sie können das R-Skript beispielsweise als Zeichenfolgenvariable deklarieren und als Parameter an den Reducer übergeben.


    REFERENCE ASSEMBLY [ExtR];
    
    DECLARE @myRScript = @"
    inputFromUSQL$Species = as.factor(inputFromUSQL$Species)
    lm.fit=lm(unclass(Species)~.-Par, data=inputFromUSQL)
    #do not return readonly columns and make sure that the column names are the same in usql and r scripts,
    outputToUSQL=data.frame(summary(lm.fit)$coefficients)
    colnames(outputToUSQL) <- c(""Estimate"", ""StdError"", ""tValue"", ""Pr"")
    outputToUSQL
    ";
    
    @RScriptOutput = REDUCE … USING new Extension.R.Reducer(command:@myRScript, rReturnType:"dataframe");

## <a name="keep-the-r-code-in-a-separate-file-and-reference-it--the-u-sql-script"></a>Bewahren Sie den R-Codes in einer separaten Datei auf und verweisen Sie damit auf das U-SQL-Skript

Im folgenden Beispiel wird eine komplexere Verwendung veranschaulicht. In diesem Fall wird der R-Code als eine RESSOURCE bereitgestellt, bei der es sich um das U-SQL-Skript handelt.

Speichern Sie diesen R-Code als separate Datei.

    load("my_model_LM_Iris.rda")
    outputToUSQL=data.frame(predict(lm.fit, inputFromUSQL, interval="confidence")) 

Verwenden Sie ein U-SQL-Skript, um das R-Skript mit der Anweisung „DEPLOY RESOURCE“ bereitzustellen.

    REFERENCE ASSEMBLY [ExtR];

    DEPLOY RESOURCE @"/usqlext/samples/R/RinUSQL_PredictUsingLinearModelasDF.R";
    DEPLOY RESOURCE @"/usqlext/samples/R/my_model_LM_Iris.rda";
    DECLARE @IrisData string = @"/usqlext/samples/R/iris.csv";
    DECLARE @OutputFilePredictions string = @"/my/R/Output/LMPredictionsIris.txt";
    DECLARE @PartitionCount int = 10;

    @InputData =
        EXTRACT 
            SepalLength double,
            SepalWidth double,
            PetalLength double,
            PetalWidth double,
            Species string
        FROM @IrisData
        USING Extractors.Csv();

    @ExtendedData =
        SELECT 
            Extension.R.RandomNumberGenerator.GetRandomNumber(@PartitionCount) AS Par,
            SepalLength,
            SepalWidth,
            PetalLength,
            PetalWidth
        FROM @InputData;

    // Predict Species

    @RScriptOutput = REDUCE @ExtendedData ON Par
        PRODUCE Par, fit double, lwr double, upr double
        READONLY Par
        USING new Extension.R.Reducer(scriptFile:"RinUSQL_PredictUsingLinearModelasDF.R", rReturnType:"dataframe", stringsAsFactors:false);
        OUTPUT @RScriptOutput TO @OutputFilePredictions USING Outputters.Tsv();

## <a name="how-r-integrates-with-u-sql"></a>Integration von R in U-SQL

### <a name="datatypes"></a>Datentypen
* Zeichenfolgen- und numerische Spalten von U-SQL werden unmittelbar zwischen R-Datenrahmen (DataFrame) und U-SQL konvertiert [unterstützte Typen: double, string, bool, integer, byte].
* Der Datentyp „factor“ wird in U-SQL nicht unterstützt.
* „byte[]“ muss als Base64-codierte Zeichenfolge serialisiert werden.
* U-SQL-Zeichenfolgen können in Faktoren (factors) in R-Code konvertiert werden, nachdem U-SQL R-Eingabedatenrahmen erstellt oder indem Sie den Reducer-Parameter „stringsAsFactors“ auf „true“ festlegen.

### <a name="schemas"></a>Schemas
* U-SQL-Datasets dürfen keine doppelten Spaltennamen haben.
* U-SQL-Dataset-Spaltennamen müssen Zeichenfolgen sein.
* Spaltennamen müssen in U-SQL und R-Skripts identisch sein.
* Eine schreibgeschützte Spalte darf nicht Bestandteil des Ausgabedatenrahmens sein. Ursache hierfür ist, dass eine schreibgeschützte Spalte automatisch wieder in die U-SQL-Tabelle zurückgeschrieben wird, wenn sie Bestandteil des Ausgabeschemas von UDO ist.

### <a name="functional-limitations"></a>Funktionale Beschränkungen
* Das R-Modul kann nicht zweimal in demselben Prozess instanziiert werden. 
* Derzeit unterstützt U-SQL keine Combiner-UDOs für die Vorhersage mithilfe von partitionierten Modellen, die von Reducer-UDOs erzeugt wurden. Benutzer können die partitionierten Modelle als Ressource deklarieren und sie in ihrem R-Skript verwenden (siehe Beispielcode „ExtR_PredictUsingLMRawStringReducer.usql“)

### <a name="r-versions"></a>Versionen von R
Es wird nur R 3.2.2 unterstützt.

### <a name="standard-r-modules"></a>Standard-R-Module

    base
    boot
    Class
    Cluster
    codetools
    compiler
    datasets
    doParallel
    doRSR
    foreach
    foreign
    Graphics
    grDevices
    grid
    iterators
    KernSmooth
    lattice
    MASS
    Matrix
    Methods
    mgcv
    nlme
    Nnet
    Parallel
    pkgXMLBuilder
    RevoIOQ
    revoIpe
    RevoMods
    RevoPemaR
    RevoRpeConnector
    RevoRsrConnector
    RevoScaleR
    RevoTreeView
    RevoUtils
    RevoUtilsMath
    Rpart
    RUnit
    spatial
    splines
    Stats
    stats4
    survival
    Tcltk
    Tools
    translations
    utils
    XML

### <a name="input-and-output-size-limitations"></a>Einschränkungen der Ein- und Ausgabegröße
Jedem Scheitelpunkt ist eine begrenzte Menge an Arbeitsspeicher zugewiesen. Da die DataFrames für die Ein- und Ausgabe im Arbeitsspeicher im R-Code vorhanden sein müssen, darf die Gesamtgröße für die Ein- und Ausgabe nicht mehr als 500 GB betragen.

### <a name="sample-code"></a>Beispielcode
Weiterer Beispielcode ist in Ihrem Data Lake Store-Konto verfügbar, nachdem Sie die Erweiterungen U-SQL Advanced Analytics installiert haben. Der Pfad für weitere Beispielcodes ist: <ihre_konto_adresse>/usqlext/beispiele/R. 

## <a name="deploying-custom-r-modules-with-u-sql"></a>Bereitstellen von benutzerdefinierten R-Modulen mit U-SQL

Erstellen Sie zunächst ein benutzerdefiniertes R-Modul und zippen sie es. Laden Sie anschließende die ZIP-Datei mit dem benutzerdefinierten R-Modul in Ihren ADL-Store hoch. Im Beispiel laden Sie „magittr_1.5.zip“ in den Stamm des standardmäßigen ADLS-Kontos für das verwendete ADLA-Konto hoch. Deklarieren Sie das Modul, sobald Sie es in den ADL-Store hochladen haben, als „DEPLOY RESOURCE“, um es in Ihrem U-SQL-Skript verfügbar zu machen. Rufen Sie anschließend „install.packages“ auf, um es zu installieren.

    REFERENCE ASSEMBLY [ExtR];
    DEPLOY RESOURCE @"/magrittr_1.5.zip";

    DECLARE @IrisData string =  @"/usqlext/samples/R/iris.csv";
    DECLARE @OutputFileModelSummary string = @"/R/Output/CustomePackages.txt";

    // R script to run
    DECLARE @myRScript = @"
    # install the magrittr package,
    install.packages('magrittr_1.5.zip', repos = NULL),
    # load the magrittr package,
    require(magrittr),
    # demonstrate use of the magrittr package,
    2 %>% sqrt
    ";

    @InputData =
    EXTRACT SepalLength double,
    SepalWidth double,
    PetalLength double,
    PetalWidth double,
    Species string
    FROM @IrisData
    USING Extractors.Csv();

    @ExtendedData =
    SELECT 0 AS Par,
    *
    FROM @InputData;

    @RScriptOutput = REDUCE @ExtendedData ON Par
    PRODUCE Par, RowId int, ROutput string
    READONLY Par
    USING new Extension.R.Reducer(command:@myRScript, rReturnType:"charactermatrix");

    OUTPUT @RScriptOutput TO @OutputFileModelSummary USING Outputters.Tsv();


## <a name="next-steps"></a>Nächste Schritte
* [Übersicht über Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Entwickeln von U-SQL-Skripts mit Data Lake-Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
* [Verwenden von U-SQL-Funktionen für Azure Data Lake Analytics-Aufträge](data-lake-analytics-use-window-functions.md)

