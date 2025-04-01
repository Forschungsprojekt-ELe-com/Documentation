
# Dokumentation zur Datenverarbeitung und Skripte

Diese Dokumentation beschreibt die Schritte und die zugehörigen Dateien, die im Prozess der Datenverarbeitung verwendet werden. 

## Übersicht

Die folgenden Schritte beschreiben den Prozess zur Verarbeitung von CSV-Dateien und Python-Notebooks, die zur Datenvorbereitung und -analyse verwendet werden.

---

## Schritt 1: Datenvorbereitung und Matrix-Umwandlung

In diesem Schritt werden verschiedene CSV-Dateien vorbereitet und bearbeitet, um sie für weitere Analyse- und Berechnungsschritte nutzbar zu machen.

### Dateien:

1. **Book1.csv**  
   - **Verwendung**: Diese Datei dient nur für die Bearbeitung in Excel. Sie beinhaltet die manuell umgewandelte Matrix der **ZBB**-Daten.
   - **Speicherort**: Im Ordner `item_matrix`.
   - **Hinweis**: Umlaute müssen nach dem Import manuell in Excel korrigiert werden.

2. **exportForPandas.csv**  
   - **Verwendung**: Diese Datei ist die aus Excel exportierte Version der Matrix und dient zur weiteren Verarbeitung in Python.
   - **Formatierung**: Die Datei ist **semicolon (;) getrennt**, was die Weiterverarbeitung in Pandas (Python) ermöglicht.

3. **key_matrix_20230223.csv**  
   - **Verwendung**: Diese Datei ist das Ergebnis der Bereinigung durch das Skript `NullToZero_Generator.ipynb`.
   - **Funktion**: Enthält die bereinigte Matrix mit gefüllten leeren Zellen (durch `0` ersetzt), wodurch die Daten konsistenter und analysierbarer werden.

4. **NullToZero_Generator.ipynb**  
   - **Verwendung**: Jupyter Notebook zur automatisierten Datenbereinigung.
   - **Funktion**: Identifiziert und ersetzt alle leeren Zellen in der Matrix durch `0` und speichert das Ergebnis als `key_matrix_20230223.csv`.

5. **tfidf_python.ipynb**  
   - **Verwendung**: Jupyter Notebook zur Berechnung von AI-Werten (ohne Kategoriefilter).
   - **Funktion**: Führt Berechnungen zur Gewichtung und Analyse der Einheiten ohne Berücksichtigung von Kategorien (Level der Einheiten).

6. **tfidf_python_Category.ipynb**  
   - **Verwendung**: Jupyter Notebook zur Berechnung von AI-Werten (mit Kategoriefilter).
   - **Funktion**: Führt Berechnungen zur Gewichtung und Analyse der Einheiten unter Berücksichtigung der Kategorien (Level der Einheiten), was eine gezieltere Analyse ermöglicht.

---

## Schritt 2: Ergänzung der Items mit ID

In diesem Schritt werden die relevanten Items um eine eindeutige ID ergänzt, um sie besser identifizieren und verarbeiten zu können.

### Dateien:

1. **item_ID.csv**  
   - **Verwendung**: Diese Datei enthält die relevanten **item_ids**, basierend auf der **item_matrix**.
   - **Funktion**: Dient als Referenz zur Identifikation und Verknüpfung von Items in weiteren Analyseschritten.

---

## Anmerkungen zur Weiteren Verarbeitung

Die Dokumentation sollte regelmäßig aktualisiert werden, wenn Änderungen an den Dateistrukturen oder -namen vorgenommen werden. Ebenso ist es sinnvoll, zukünftige Anpassungen oder Ergänzungen in den einzelnen Jupyter Notebooks direkt zu kommentieren.
