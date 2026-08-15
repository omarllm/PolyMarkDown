# Requirements
Hey Gemini, so these are my requirements:
1. Use at least one public Cloud service (e.g., from [https://publicapis.io/](https://publicapis.io/))  
2. Support multiple users with login and authentication  
3. Include some 2D graphics  
4. Use at least one sensor  
5. Use GPS  
6. Use the camera or perform image processing  
7. Include concurrency (async tasks, coroutines, etc.)  
8. Use at least one additional cloud feature (e.g., Google Cloud service)  
9. Implement a REST API running on a remote server (e.g., PythonAnywhere, Docker on a VM)  
10. Implement a storage service (e.g., a simple SQL database accessed via the REST API)

My idea would be to create an app which would resemble the famous Anki spaced repetition app, but with some twists to follow all the delineated requirements and to use Kotlin MultiPlatform to be able to run it on both Android and iOS. Don't do anything read the prompt and what is inside the folder. Step-by-step guide me, ok?

SQLite
**SQLite con Flask-SQLAlchemy**.

Obsidian to Google Doc
https://www.reddit.com/r/ObsidianMD/comments/1c4xfx0/obsidian_to_google_doc/

Free account features PythonAnywhere
https://help.pythonanywhere.com/pages/FreeAccountsFeatures

---
**No, non devi creare alcun account per usare SQLite.**

  

SQLite **non è un servizio cloud né un server** (come invece possono essere MySQL o PostgreSQL), ma una libreria software _file-based_:

  

- **È un semplice file:** L'intero database risiede in un singolo file (es. `database.db` o `app.db`) salvato direttamente nella cartella del tuo progetto.
    
      
    
- **È integrato in Python:** Python include già il modulo `sqlite3` nella sua libreria standard, quindi non devi installare nulla né sottoscrivere abbonamenti o creare credenziali.
    
      
    

### Come funziona su PythonAnywhere con Flask

Quando carichi la tua applicazione Flask su **PythonAnywhere**:

1. **Percorso assoluto del file:** Assicurati sempre di specificare il **percorso assoluto** del file di database, altrimenti Flask potrebbe non trovarlo quando viene avviato dal server web (WSGI).
    
```Python
    import os
    from flask import Flask
    from flask_sqlalchemy import SQLAlchemy
    
    app = Flask(__name__)
    
    # Ricava la cartella corrente in modo dinamico
    basedir = os.path.abspath(os.path.dirname(__file__))
    
    # Configura SQLite con percorso assoluto
    app.config['SQLALCHEMY_DATABASE_URI'] = f"sqlite:///{os.path.join(basedir, 'app.db')}"
```
    
1. **Nessuna configurazione extra:** Non serve toccare la scheda _Databases_ del pannello di controllo di PythonAnywhere (quella sezione serve solo per configurare database client-server come MySQL/Postgres).

---

Ecco un esempio completo, pronto e ottimizzato per **PythonAnywhere**, che utilizza Flask e **Flask-SQLAlchemy** con SQLite.

  

### 1. Il codice dell'app (`app.py` / `flask_app.py`)

Crea o modifica il file principale dell'applicazione:
```Python
import os
from flask import Flask, jsonify, request
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)

# --- CONFIGURAZIONE PERCORSO SQLITE ---
# Ricava il percorso assoluto della cartella dove si trova questo file
basedir = os.path.abspath(os.path.dirname(__file__))

# Percorso completo del file .db (es: /home/tuoutente/progetto/database.db)
app.config['SQLALCHEMY_DATABASE_URI'] = f"sqlite:///{os.path.join(basedir, 'database.db')}"
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False

db = SQLAlchemy(app)

# --- MODELLO DATI (TABELLA) ---
class Utente(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    nome = db.Column(db.String(80), nullable=False)
    email = db.Column(db.String(120), unique=True, nullable=False)

    def to_dict(self):
        return {"id": self.id, "nome": self.nome, "email": self.email}

# --- ROTTE FLASK ---
@app.route('/')
def home():
    return "App Flask con SQLite funzionante!"

# Rotta per leggere gli utenti
@app.route('/utenti', methods=['GET'])
def get_utenti():
    utenti = Utente.query.all()
    return jsonify([u.to_dict() for u in utenti])

# Rotta per aggiungere un utente di test
@app.route('/aggiungi-test', methods=['GET'])
def aggiungi_test():
    nuovo_utente = Utente(nome="Mario Rossi", email="mario@example.com")
    try:
        db.session.add(nuovo_utente)
        db.session.commit()
        return jsonify({"messaggio": "Utente aggiunto con successo!", "utente": nuovo_utente.to_dict()})
    except Exception as e:
        db.session.rollback()
        return jsonify({"errore": str(e)}), 400

# Creazione automatica delle tabelle al primo avvio
with app.app_context():
    db.create_all()

if __name__ == '__main__':
    app.run(debug=True)
```

### 2. Passaggi chiave su PythonAnywhere

1. **Installa la libreria:**
    
    Apri una **Bash Console** su PythonAnywhere ed esegui:
```Bash
pip install flask-sqlalchemy
```
    _(oppure all'interno del tuo virtualenv, se ne usi uno)_.
    
      
    
2. **File WSGI:**
    
    Nella scheda **Web** del pannello di controllo, assicurati che il file di configurazione WSGI punti correttamente alla tua app (es. `from app import app as application`).
    
      
    
3. **Ricarica l'app:**
    
    Premi il pulsante verde **Reload** nella scheda Web.
    
      
    

> **Nota:** Il file `database.db` verrà generato automaticamente nella stessa cartella dello script non appena il server si avvia o riceve la prima richiesta.