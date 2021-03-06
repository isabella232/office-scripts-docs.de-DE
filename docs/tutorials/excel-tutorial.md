---
title: Aufzeichnen, Bearbeiten und Erstellen von Office-Skripts in Excel im Web
description: Dies ist ein Lernprogramm zu den Grundlagen von Office-Skripts, einschließlich dem Aufzeichnen von Skripts mithilfe der Aktionsaufzeichnung und dem Schreiben von Daten in eine Arbeitsmappe.
ms.date: 07/21/2020
localization_priority: Priority
ms.openlocfilehash: 96bdc286883d87249de260666c7c8ffe2c94cc0f
ms.sourcegitcommit: ff7fde04ce5a66d8df06ed505951c8111e2e9833
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2020
ms.locfileid: "46616773"
---
# <a name="record-edit-and-create-office-scripts-in-excel-on-the-web"></a>Aufzeichnen, Bearbeiten und Erstellen von Office-Skripts in Excel im Web

In diesem Lernprogramm lernen Sie die Grundlagen zum Aufzeichnen, Bearbeiten und Schreiben eines Office-Skripts für Excel im Web kennen. Zuerst zeichnen Sie ein Skript auf, das ein Arbeitsblatt für Umsatzdatensätze formatiert. Dann bearbeiten Sie das aufgezeichnete Skript, um weitere Formatierungen anzuwenden, eine Tabelle zu erstellen und diese Tabelle zu sortieren. Dieses Aufzeichnen-und-Bearbeiten-Muster ist ein wichtiges Tool, um zu sehen, wie Ihre Excel-Aktionen als Code aussehen.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [Tutorial prerequisites](../includes/tutorial-prerequisites.md)]

> [!IMPORTANT]
> Dieses Lernprogramm richtet sich an Anfänger bis Fortgeschrittene mit JavaScript oder TypeScript. Wenn Sie noch nicht mit JavaScript vertraut sind, empfehlen wir Ihnen, mit dem [Mozilla-JavaScript-Lernprogramm](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Introduction) zu beginnen. Weitere Informationen über die Skriptumgebung finden Sie unter [ Umgebung des Code Editor für Office-Skripts](../overview/code-editor-environment.md).

## <a name="add-data-and-record-a-basic-script"></a>Hinzufügen von Daten und Aufzeichnen eines einfachen Skripts

Zuerst benötigen wir einige Daten und ein kleines Startskript.

1. Erstellen Sie eine neue Arbeitsmappe in Excel im Web.
2. Kopieren Sie die folgenden Obst-Umsatzdaten, und fügen Sie diese beginnend bei Zelle **A1** in das Arbeitsblatt ein.

    |Obstsorte |2018 |2019 |
    |:---|:---|:---|
    |Orangen |1000 |1200 |
    |Zitronen |800 |900 |
    |Limetten |600 |500 |
    |Grapefruits |900 |700 |

3. Öffnen Sie die Registerkarte **Automatisieren**. Falls die Registerkarte **Automatisieren** nicht angezeigt wird, überprüfen Sie den Menüband-Überlauf, indem Sie auf den Dropdownpfeil klicken.
4. Klicken Sie auf die Schaltfläche **Aktionen aufzeichnen**.
5. Wählen Sie Zellen **A2:C2** (die Zeile "Orangen") aus, und legen Sie die Füllfarbe auf Orange fest.
6. Beenden Sie die Aufzeichnung, indem Sie die Schaltfläche **Stopp** drücken.
7. Geben Sie in das Feld **Skriptnamen** einen einprägsamen Namen ein.
8. *Optional:* Füllen Sie das Feld **Beschreibung** mit einer aussagekräftigen Beschreibung aus. Diese wird verwendet, um den Kontext des Skripts bereitzustellen. Für dieses Lernprogramm können Sie "Farbcodierte Zeilen einer Tabelle" verwenden.

   > [!TIP]
   > Sie können die Beschreibung eines Skripts später im Bereich **Skriptdetails** bearbeiten, der sich im Menü **...** des Code-Editors befindet.

9. Klicken Sie auf die Schaltfläche **Speichern**, um das Skript zu speichern.

    Ihr Arbeitsblatt sollte wie folgt aussehen (machen Sie sich keine Sorgen, wenn die Farbe anders ist):

    ![Eine Zeile mit Obst-Umsatzdaten mit hervorgehobener orangefarbener Zeile "Orangen".](../images/tutorial-1.png)

## <a name="edit-an-existing-script"></a>Bearbeiten eines vorhandenen Skripts

Das vorherige Skript hat die Zeile "Orangen" orangefarben eingefärbt. Jetzt fügen wir eine gelbe Zeile für die "Zitronen" hinzu.

1. Klicken Sie im nun geöffneten **Detailbereich** auf die Schaltfläche **Bearbeiten**.
2. Folgendes sollte nun auf dem Bildschirm angezeigt werden:

    ```TypeScript
    function main(workbook: ExcelScript.Workbook) {
      // Set fill color to FFC000 for range Sheet1!A2:C2
      let selectedSheet = workbook.getActiveWorksheet();
      selectedSheet.getRange("A2:C2").getFormat().getFill().setColor("FFC000");
    }
    ```

    Dieser Code ruft das aktuelle Arbeitsblatt aus der Arbeitsmappe ab. Anschließend legt er die Füllfarbe des Bereichs **A2:C2** fest.

    Bereiche sind ein wesentliches Element von Office-Skripts in Excel im Web. Ein Bereich ist ein zusammenhängender, rechteckiger Block von Zellen, die Werte, Formeln und Formatierungen enthalten. Hierbei handelt es sich um die grundlegende Struktur von Zellen, durch die Sie die meisten Skript-Aufgaben ausführen werden.

3. Fügen Sie am Ende des Skripts die folgende Zeile ein (zwischen dem festgelegten `color` und der schließenden `}`):

    ```TypeScript
    selectedSheet.getRange("A3:C3").getFormat().getFill().setColor("yellow");
    ```

4. Testen Sie das Skript, indem Sie **Ausführen** drücken. Ihre Arbeitsmappe sollte nun wie folgt aussehen:

    ![Eine Zeile mit Obst-Umsatzdaten mit orangefarben hervorgehobener Zeile "Orangen" und einer gelb hervorgehobenen Zeile "Zitronen".](../images/tutorial-2.png)

## <a name="create-a-table"></a>Erstellen einer Tabelle

Wandeln wir diese Obst-Umsatzdaten in eine Tabelle um. Wir verwenden unser Skript für den gesamten Prozess.

1. Fügen Sie die folgende Zeile am Ende des Skripts hinzu (vor der schließenden `}`):

    ```TypeScript
    let table = selectedSheet.addTable("A1:C5", true);
    ```

2. Dieser Aufruf gibt ein `Table`-Objekt zurück. Verwenden wir diese Tabelle zum Sortieren der Daten. Wir werden die Daten basierend auf den Werten in der Spalte "Obstsorte" in aufsteigender Reihenfolge sortieren. Fügen Sie dann die folgende Zeile nach der Tabellenerstellung hinzu:

    ```TypeScript
    table.getSort().apply([{ key: 0, ascending: true }]);
    ```

    Das Skript sollte wie folgt aussehen:

    ```TypeScript
    function main(workbook: ExcelScript.Workbook) {
        // Set fill color to FFC000 for range Sheet12!A2:C2
        let selectedSheet = workbook.getActiveWorksheet();
        selectedSheet.getRange("A2:C2").getFormat().getFill().setColor("FFC000");
        selectedSheet.getRange("A3:C3").getFormat().getFill().setColor("yellow");
        let table = selectedSheet.addTable("A1:C5", true);
        table.getSort().apply([{ key: 0, ascending: true }]);
    }
    ```

    Tabellen beinhalten ein `TableSort`-Objekt, auf das über die `Table.getSort`-Methode zugegriffen wird. Sie können auf dieses Objekt Sortierkriterien anwenden. Die `apply`-Methode bezieht eine Reihe von `SortField`-Objekten ein. In diesem Fall gibt es nur ein Sortierkriterium, also verwenden wir nur ein `SortField`. `key: 0` legt für die Spalte mit den die Sortierung bestimmenden Werten "0" fest (dies ist die erste Spalte in der Tabelle, in diesem Fall **A**). `ascending: true` sortiert die Daten in aufsteigender Reihenfolge (statt in absteigender Reihenfolge).

3. Führen Sie das Skript aus. Es sollte eine Tabelle wie die folgende angezeigt werden:

    ![Eine Tabelle mit sortierten Obst-Umsatzdaten.](../images/tutorial-3.png)

    > [!NOTE]
    > Wenn Sie das Skript erneut ausführen, wird eine Fehlermeldung angezeigt. Der Grund dafür ist, dass Sie keine Tabelle über eine andere Tabelle erstellen können. Sie können das Skript jedoch auf ein anderes Arbeitsblatt oder eine andere Arbeitsmappe anwenden.

### <a name="re-run-the-script"></a>Das Skript erneut ausführen

1. Erstellen Sie ein neues Arbeitsblatt in der aktuellen Arbeitsmappe.
2. Kopieren Sie die Obstdaten am Anfang dieses Lernprogramms, und fügen Sie sie in das neue Arbeitsblatt ein, beginnend bei Zelle **A1**.
3. Führen Sie das Skript aus.

## <a name="next-steps"></a>Nächste Schritte

Führen Sie das Lernprogramm [Lesen von Arbeitsmappendaten mit Office-Skripts in Excel im Web](excel-read-tutorial.md) aus. Hier erfahren Sie, wie Sie mithilfe eines Office-Skripts Daten aus einer Arbeitsmappe lesen können.
