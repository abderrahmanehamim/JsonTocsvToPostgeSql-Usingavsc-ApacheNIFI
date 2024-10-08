1. Acquisition des Données (GetFile):

Le processus démarre avec l'acquisition du fichier JSON source ("company_data.json") depuis le répertoire spécifié ("C:\Users\H-R\Desktop\NifiTasks"). Le processeur GetFile est configuré pour lire ce fichier, avec des options permettant de gérer le filtrage, la récursivité des dossiers et la fréquence de scrutation.

2. Segmentation du JSON (SplitJson):

Le fichier JSON est divisé en enregistrements individuels grâce au processeur SplitJson. Ce processeur utilise une expression JsonPath "$.*" pour séparer chaque objet JSON du fichier source en un FlowFile distinct.

3. Extraction des Attributs (EvaluateJsonPath):

Le processeur EvaluateJsonPath analyse chaque enregistrement JSON et extrait les valeurs de champs spécifiques ("id", "company", "country", "countryregion", "product_categories", "brandnames", "link_details"). Ces valeurs sont ensuite stockées en tant qu'attributs du FlowFile.

4. Conversion en JSON (AttributesToJSON):

Les attributs extraits sont regroupés dans un objet JSON grâce au processeur AttributesToJSON. Cette étape prépare les données pour la conversion en format CSV.

5. Conversion en Avro (ConvertRecord):

Le processeur ConvertRecord, utilisant un service JsonTreeReader, transforme l'objet JSON en un enregistrement Avro. Cette étape est essentielle pour appliquer le schéma Avro aux données, garantissant ainsi une structure et un typage cohérents.

6. Conversion en CSV (ConvertRecord, MergeRecord):

Le processeur ConvertRecord, associé à un service AvroRecordSetWriter, convertit ensuite l'enregistrement Avro en un enregistrement CSV.

Le processeur MergeRecord fusionne les enregistrements CSV individuels en un seul fichier CSV. Un service CSVRecordSetWriter est utilisé pour appliquer un formatage standardisé au fichier CSV final.

7. Stockage du Fichier CSV (UpdateAttribute, PutFile):

Le processeur UpdateAttribute définit l'attribut "filename" du FlowFile résultant sur "company_data.csv", spécifiant le nom du fichier de sortie.

Enfin, le processeur PutFile stocke le fichier CSV final dans le répertoire spécifié ("C:\Users\H-R\Desktop\NifiTasks").

Avantages du Template:

Transformation Structurée: Le template utilise un schéma Avro pour garantir une structure de données cohérente lors de la conversion de JSON en CSV.

Flexibilité: La configuration du template permet de modifier facilement les champs extraits du JSON et le formatage du CSV final.

Scalabilité: Le template peut gérer des fichiers JSON de différentes tailles grâce à la segmentation et à la fusion des enregistrements.
