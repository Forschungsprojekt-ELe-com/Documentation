
# Dokumentation für das AI-Modell zur Ähnlichkeitsbewertung und Empfehlung

Diese Dokumentation beschreibt die Implementierung eines AI-Modells, das die Ähnlichkeit zwischen verschiedenen Textobjekten berechnet und darauf basierend Empfehlungen gibt. Der Workflow umfasst Datenvorbereitung, Datenbereinigung, Vektorisierung, Dimensionsreduktion und die Berechnung von Ähnlichkeitsmetriken.

## Anforderungen

Die folgenden Bibliotheken werden benötigt:
- `numpy`
- `pandas`
- `seaborn`
- `matplotlib`
- `re`
- `nltk`
- `scikit-learn`
- `joblib`

Installieren Sie die fehlenden Bibliotheken bei Bedarf, z.B. mit `pip install library_name`.

---

## Schritt-für-Schritt Anleitung

### 1. Laden der Daten
Laden Sie die CSV-Datei mit Metadaten und bereinigen Sie sie, um nur relevante Daten zu behalten.

```python
df_meta = pd.read_csv('Oktober2023/key_matrix_20230223_1.csv', sep=';')
df_meta.reset_index(drop=True, inplace=True)
```

### 2. Datenbereinigung
Leere Einträge und unerwünschte Zeichen werden aus den Daten entfernt. Die Datenspalte `merged_col` wird erstellt, die alle relevanten Informationen eines Eintrags zusammenführt.

```python
df['merged_col'] = df.apply(lambda row: row[row != 0].astype(str).str.cat(sep=', '), axis=1)
df['merged_col'] = df['merged_col'].str.replace(',', '').str.replace('\d+', '').str.lower()
```

### 3. Datenvorbereitung
Die Items werden durch IDs und Textlänge beschrieben. Die ID-Sätze (z.B. `ref_id1`, `obj_id1`) dienen zur späteren Zuordnung.

```python
df['itemId'] = range(0, len(df))
item_id = pd.read_csv('Oktober2023/item_ID_oktober2023_export.csv', sep=';')
item_id = item_id.groupby('description').agg(list).reset_index()
```

### 4. Text-Vektorisierung mit TF-IDF
Es wird ein `TfidfVectorizer` mit einer benutzerdefinierten Stopword-Liste erstellt, um eine TF-IDF-Matrix zu berechnen, die das Vokabular der Texte widerspiegelt.

```python
tfidf = TfidfVectorizer(max_features=250, analyzer='word', stop_words=stop)
vectorized_data = tfidf.fit_transform(df['merged_col'])
```

### 5. Dimensionsreduktion mit SVD
Die TF-IDF-Matrix wird mit `TruncatedSVD` auf 50 Dimensionen reduziert, um die Rechenleistung und Effizienz zu optimieren.

```python
svd = TruncatedSVD(n_components=50)
reduced_data = svd.fit_transform(count_matrix)
```

### 6. Ähnlichkeitsberechnung und Empfehlung
Der Ähnlichkeitswert zwischen den Einträgen wird mit `cosine_similarity` berechnet. Die Funktion `get_recommendations()` gibt basierend auf diesen Ähnlichkeitswerten die relevantesten Empfehlungen zurück.

```python
similarity = cosine_similarity(reduced_data)
result_df, _, error_code = get_recommendations(item_name, mediaPref, level, number_of_recommendations, done_list_ref_id)
```

### 7. Ergebnisdarstellung
Mit der Funktion `show_results()` werden die Top-Empfehlungen als Balkendiagramm angezeigt.

```python
show_results(item_name, result_df)
```

### 8. Speicherung des Modells und der Daten
Das Modell und die vorbereiteten Daten werden gespeichert, um sie in zukünftigen Schritten wiederverwenden zu können.

```python
joblib.dump(reduced_data, 'tfidf_model.pkl')
df.to_pickle('my_dataframe.pkl')
```

---

## Funktionsbeschreibung

- **`show_results(movie_name, top_titles_df)`**  
  Diese Funktion zeigt die Top-Empfehlungen für einen Eintrag als Balkendiagramm an.

- **`get_top_titles_and_categories(df, movie_indices, mediaPref)`**  
  Extrahiert die relevantesten Einträge basierend auf den Ähnlichkeitsmetriken.

- **`get_recommendations(item_name, mediaPref, level, n, done_list_ref_id)`**  
  Gibt eine Liste der am besten passenden Einträge zurück.

---

## Beispielaufruf

So kann das Modell für Empfehlungen aufgerufen werden:

```python
result_df, _, error_code = get_recommendations(item_name, mediaPref, level, number_of_recommendations, done_list_ref_id)
```

---

Diese Dokumentation deckt die wesentlichen Schritte zur Modellimplementierung und -anwendung ab und bietet eine Grundlage für eine einfache Übergabe und zukünftige Nutzung.
