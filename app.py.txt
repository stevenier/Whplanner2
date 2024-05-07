from flask import Flask, request, jsonify
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///trucks.db'  # Verbindung zur SQLite-Datenbank
db = SQLAlchemy(app)

# Datenbankmodell für LKWs
class Truck(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    truckNumber = db.Column(db.String(100), nullable=False)
    parkingSpot = db.Column(db.String(100), nullable=False)
    loadingLocation = db.Column(db.String(100), nullable=False)
    startTime = db.Column(db.String(100), nullable=False)

# Route zum Anmelden eines LKWs
@app.route('/submit_truck', methods=['POST'])
def submit_truck():
    # Formulardaten abrufen
    truckNumber = request.form['truckNumber']
    parkingSpot = request.form['parkingSpot']
    loadingLocation = request.form['loadingLocation']
    startTime = request.form['startTime']
    
    # Neuen LKW-Eintrag in der Datenbank erstellen
    new_truck = Truck(truckNumber=truckNumber, parkingSpot=parkingSpot, loadingLocation=loadingLocation, startTime=startTime)
    db.session.add(new_truck)
    db.session.commit()
    
    return 'Truck submitted successfully'

# Route zum Abrufen der aktuellen LKW-Daten
@app.route('/get_trucks', methods=['GET'])
def get_trucks():
    trucks = Truck.query.all()  # Alle LKWs aus der Datenbank abrufen
    truck_list = []
    for truck in trucks:
        truck_list.append({'truckNumber': truck.truckNumber, 'parkingSpot': truck.parkingSpot, 'loadingLocation': truck.loadingLocation, 'startTime': truck.startTime})
    return jsonify(truck_list)

if __name__ == '__main__':
    db.create_all()  # Datenbanktabellen erstellen, falls sie noch nicht existieren
    app.run(debug=True)  # Starte die Flask-Anwendung im Debug-Modus
