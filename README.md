# Flask-API-SETUP


### .env file
```
MYSQL_HOST=localhost
MYSQL_USER=root
MYSQL_PASSWORD=
MYSQL_DATABASE=your_database_name
```

### db_utils.py
```py
import os
import mysql.connector
from dotenv import load_dotenv

# Load environment variables from .env file
load_dotenv()

# Database configuration using environment variables
db_config = {
    'host': os.getenv('MYSQL_HOST'),
    'user': os.getenv('MYSQL_USER'),
    'password': os.getenv('MYSQL_PASSWORD'),
    'database': os.getenv('MYSQL_DATABASE')
}

def get_db_connection():
    """Return a new database connection."""
    return mysql.connector.connect(**db_config)

```

### app.py
```py
from flask import Flask, request, jsonify, render_template, redirect, url_for, abort, send_file
from werkzeug.utils import secure_filename
from db_utils import get_db_connection
from flask_cors import CORS
from flask_cors import cross_origin
import os
import shutil
import base64
import time
import config

app = Flask(__name__)
CORS(app, resources={r"/*": {"origins": "*"}})

# Configuration
app.config['UPLOAD_FOLDER'] = os.path.join(os.path.dirname(__file__), 'uploads/')
app.config['MAX_CONTENT_LENGTH'] = 100 * 1024 * 1024  # 100 MB limit

@app.route('/')
def index():
    return render_template('index.html')


#### YOUR CODE ####

### END OF YOUR CODE ###


if __name__ == "__main__":
    app.run(debug=True)
```

## Examples 

### Uploading Files 
```py
# Uploading Files
@app.route("/upload", methods=['POST'])
@cross_origin()
def upload():
  response_data = []
  for file in files:
    filename = secure_filename(file.filename)
    filepath = os.path.join(app.config['UPLOAD_FOLDER'], filename)
    file.save(filepath)
    response_data.append({ "filename": filename})

  if response_data:
          return jsonify({"message": "Files processed successfully", "data": response_data}), 200
  else:
        return jsonify({"error": "No files were processed successfully"}), 400
```


### Get Data From Database
```py
@app.route('/data', methods=['GET'])
@cross_origin()
def get_data():
    """API endpoint to fetch data from the database."""
    connection = get_db_connection()
    cursor = connection.cursor(dictionary=True)

    cursor.execute("SELECT * FROM student_data")
    data = cursor.fetchall()

    cursor.close()
    connection.close()
    return jsonify({"data": data}), 200
```
For Post use  methods=['POST'] & to insert data in database use Insert Query

### Get Input Field Value
```py
username = request.form.get('username') # Input Field name ( <input type='text' name='username'> )
```

### Get Value From Url
url example : your-domain.com/examdata?examid='1234'
```py
examid = request.args.get('examid', '')
```



