# Projet
L'objectif de ce programme est de déocmpiler et d'analyser des apk.
Plusieurs models de LLM sont appelé pour générer les différentes analsyes.

# Configuration 
Dans le répertoire du proejt ajouter votre apk.jar (apksigner.jar déjà présent)
Dans le fichier `core/conf.py` modifier les params : 

```python
JAR_FILE = "apksigner.jar" # nom du fichier jar
DEEP_ANALYSE = True # Analyse profondes ou non 

OLLAMA_API_URL = "localhost:11434/api/generate" #URL du server ollama 
OLLAMA_API_TOKEN = "token" # token du serveur. 

DEBUG = False #Mode debug
```
Deep_analyse permet de sélectionner plus ou moins de classes dans la détection des classes pertinentes.
Le mode debug permet de sauter certains apelles de LLM, utile pour regénérer le rapports à partir de save dans le fichier `db/`

Ollama api url : si vous utiliser une instances GPU, ajouter l'url.
J'ai utilisé des instances gpu RTX 5090 sur vast.ai, coût d'une analyse d'apk environ 0,70€
Pour récupérer le OLLAMA API TOKEN sur vasi.ai -> echo $OPEN_BUTTON_TOKEN

## Configuration ordinateurs
Les tests ont été réalisé sur l'application apksigner.apk 
Une analyse complète environ 1h20, avec un GPU Nvidia 5090
40 Go RAM minimum 
45 Go de stockage min 

## Commande pour installer les models (à faire sur l'instance GPU ou en local)
```
curl -fsSL https://ollama.com/install.sh | sh #Installer ollama
pkill ollama
ollama pull qwen3-vl:32b #insallation des models
ollama pull qwen3:32b 
```

## Commande pour éxécuter le projet 
```
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python3 main.py
```

# Rapport 
Le rapport sera généré dans `db/Security_Analysis_Report.pdf` et `db/Security_Analysis_Report.md`
Le fichier .md permet de faire des modfification, puis avec le mode debug de regénérer le pdf à partir du md sant avoir à refaire toutes la génération.

# Doc supplémentaires 
Dans le répertoire `doc/`, vous retrouverez le graph utilisé pour choisir les classes pertinentes.

# Comparatif des models
## Analyse model glm-4.7-flash:bf16:
+ consome peu de GPU
- beaucoup de RAM nécéssaire, prendre plutôt un model avec poid faible 

STATS :
- 60GB
- RAM: 90Go minimum
- NVIDIA GeForce RTX 5090 : utilisation à 4% = 1.27 Go (100% = 31.8Gb)

## Analyse model qwen3-vl:32b:
+ assez rapide 

STATS :
- 26 Gb stockage
- RAM: 28 Go min 
- NVIDIA GeForce RTX 5090 à 28,94 Gb/31.8gb utilisé par model

TEMPS:
- Analyse macro : 50s/class
- Analyse méthodes : 12.48s/method
- Analyse crypto: X

## Analyse model qwen3:8b
+ assez rapide

STATS :
- 26 Gb stockage
- RAM: 33 Go min 
- NVIDIA GeForce RTX 5090 à 28,6 Gb/31.8gb utilisé par model

TEMPS:
- Analyse crypto : 4s/method
