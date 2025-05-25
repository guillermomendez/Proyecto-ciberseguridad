# Proyecto-ciberseguridad
1 (uno)
# app.py
from flask import Flask, request, jsonify
import base64

app = Flask(__name__)

# Ruta pública accesible sin autenticación
@app.route('/public', methods=['GET'])
def public():
    return jsonify({'message': 'Esta es una ruta pública'})

# Función para verificar credenciales
def check_auth(username, password):
    return username == 'admin' and password == 'secret'

# Ruta protegida con autenticación básica
@app.route('/protected', methods=['GET'])
def protected():
    auth = request.authorization
    if not auth or not check_auth(auth.username, auth.password):
        return jsonify({'error': 'Autenticación requerida'}), 401, {'WWW-Authenticate': 'Basic realm="Login Required"'}
    return jsonify({'message': 'Acceso autorizado a la ruta protegida'})

if __name__ == '__main__':
    app.run(debug=True)
