## RASA NLU Setup

### install pip if not installed
```
sudo apt-get update
sudo apt-get install python-pip
```

### clone the repo and install from setup.py
```
git clone https://github.com/golastmile/rasa_nlu.git
cd rasa_nlu
sudo python setup.py install
```

## Download backends options
### MITIE
Download and save total_word_feature_extractor.dat file. Copy total_word_feature_extractor.dat file from here :https://github.com/mit-nlp/MITIE/releases/download/v0.4/MITIE-models-v0.2.tar.bz2.
```
mkdir mitie
mv /path/to/total_word_feature_extractor.dat rasa_nlu/mitie/
pip install git+https://github.com/mit-nlp/MITIE.git
```
### spaCy and sklearn
```
pip install -U spacy
python -m spacy.en.download all
pip install numpy scipy scikit-learn
```
## Training
### Saving models
```
mkdir models
cd models
mkdir wit
mkdir rasa
mkdir data
cd data
mkdir wit
mkdir rasa
```
### Training data
You may use online rasa nlu trainer to create your training data here https://golastmile.github.io/rasa-nlu-trainer/
After adding your data, download the training data from the trainer. save the file in data/rasa/ folder.
### rasa
Create a config file called config_rasa.json with below contents
```
{
  "backend": "mitie_sklearn",
  "mitie_file": "/home/<user>/rasa/rasa_nlu/mitie/total_word_feature_extractor.dat",
  "path" : "/home/<user>/rasa_nlu/models/rasa/",
  "data" : "/home<user>/rasa_nlu/data/rasa/rasa.json"
}
```
### wit
Extract the data from your wit project and copy the expressions.json in data/wit folder
Create a config file called config_wit.json with below contents
```
{
  "backend": "mitie_sklearn",
  "mitie_file": "/home/<user>/rasa_nlu/mitie/total_word_feature_extractor.dat",
  "path" : "/home/<user>/rasa_nlu/models/wit/",
  "data" : "/home<user>/rasa_nlu/data/wit/expressions.json"
}
```
### Training
Firsts training rasa use config_rasa.json, then run the same command with config_wit.json
```
python -m rasa_nlu.train -c config_<rasa/wit>.json
```
After successful training, your models has been created in models folder for both the trainings

## Running Server
Create a config file config_server.json with following contents
```
{
  "mitie_file": "/home<user>/rasa_nlu/mitie/total_word_feature_extractor.dat",
  "server_model_dirs": {
    "wit" : "/home<user>/rasa_nlu/models/wit/<model_folder>/"
  },
  "token": <token>,
  "response_log":"/home<user>/rasa_nlu/logs/",
  "emulate" : "wit" 
}
```
### Start server
```
python -m rasa_nlu.server -c config_server.json
```

### Making requests
GET or Post. below is a GET request example
```
http://localhost:5000/parse?q=<text>&model=<model>&token=<token>
```

## Date parsing with Duckling-rest
### clone the repo
```
git clone https://github.com/ShubhankarS/duckling-rest.git
```
Install java version 6 or later if not installed. Copy lein script from here https://raw.githubusercontent.com/technomancy/leiningen/stable/bin/lein
Follow below steps to execute the script
```
cd /bin
sudo vi lein
Copy the contents of the script into lein
sudo chmod 755 lein
lein
```
### duckling-rest rest up
```
cd duckling-rest/duckling
lein jar
lein install
cd ..
lein deps
lein run
```
### Making requests
```
GET "http://localhost:5000/parse/time/:text"
```
