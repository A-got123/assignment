import sqlite3
from flask import Flask, request, jsonify
app = Flask(__name__)
def get_db_connection():
    conn = sqlite3.connect('ice_cream_parlor.db')
    conn.row_factory = sqlite3.Row
    return conn
@app.route('/')
def home():
    return "Welcome to the Ice Cream Parlor!"
@app.route('/flavors', methods=['GET'])
def get_flavors():
    conn = get_db_connection()
    cursor = conn.cursor()
    cursor.execute('SELECT * FROM seasonal_flavors')
    flavors = cursor.fetchall()
    conn.close()

    return jsonify([dict(row) for row in flavors])
  @app.route('/ingredients', methods=['POST'])
def add_ingredient():
    data = request.get_json()
    name = data.get('name')
    quantity = data.get('quantity')
    is_allergen = data.get('is_allergen', 0)  # Default to 0 (non-allergen)

    conn = get_db_connection()
    cursor = conn.cursor()
    
    cursor.execute('''
        INSERT INTO ingredient_inventory (name, quantity, is_allergen)
        VALUES (?, ?, ?)''', (name, quantity, is_allergen))
    
    conn.commit()
    conn.close()

    return jsonify({"message": f"Ingredient {name} added successfully!"}), 201

@app.route('/allergens', methods=['GET'])
def search_allergens():
    conn = get_db_connection()
    cursor = conn.cursor()
    cursor.execute('SELECT * FROM ingredient_inventory WHERE is_allergen = 1')
    allergens = cursor.fetchall()
    conn.close()
 return jsonify([dict(row) for row in allergens])
@app.route('/cart', methods=['POST'])
def add_to_cart():
    data = request.get_json()
    customer_name = data.get('customer_name')
    flavor_name = data.get('flavor_name')

    conn = get_db_connection()
    cursor = conn.cursor()
    
    cursor.execute('''
        INSERT INTO cart (customer_name, flavor_name)
        VALUES (?, ?)''', (customer_name, flavor_name))
    
    conn.commit()
    conn.close()

    return jsonify({"message": f"{flavor_name} added to cart for {customer_name}!"}), 201
if __name__ == '__main__':
    app.run(debug=True)

import sqlite3
from flask import Flask, request, jsonify
app = Flask(__name__)
def get_db_connection():
    conn = sqlite3.connect('ice_cream_parlor.db')
    conn.row_factory = sqlite3.Row
    return conn
@app.route('/')
def home():
    return "Welcome to the Ice Cream Parlor!"
@app.route('/flavors', methods=['GET'])
def get_flavors():
    conn = get_db_connection()
    cursor = conn.cursor()
    cursor.execute('SELECT * FROM seasonal_flavors')
    flavors = cursor.fetchall()
    conn.close()

    return jsonify([dict(row) for row in flavors])
  @app.route('/ingredients', methods=['POST'])
def add_ingredient():
    data = request.get_json()
    name = data.get('name')
    quantity = data.get('quantity')
    is_allergen = data.get('is_allergen', 0)  # Default to 0 (non-allergen)

    conn = get_db_connection()
    cursor = conn.cursor()
    
    cursor.execute('''
        INSERT INTO ingredient_inventory (name, quantity, is_allergen)
        VALUES (?, ?, ?)''', (name, quantity, is_allergen))
    
    conn.commit()
    conn.close()

    return jsonify({"message": f"Ingredient {name} added successfully!"}), 201

@app.route('/allergens', methods=['GET'])
def search_allergens():
    conn = get_db_connection()
    cursor = conn.cursor()
    cursor.execute('SELECT * FROM ingredient_inventory WHERE is_allergen = 1')
    allergens = cursor.fetchall()
    conn.close()
 return jsonify([dict(row) for row in allergens])
@app.route('/cart', methods=['POST'])
def add_to_cart():
    data = request.get_json()
    customer_name = data.get('customer_name')
    flavor_name = data.get('flavor_name')

    conn = get_db_connection()
    cursor = conn.cursor()
    
    cursor.execute('''
        INSERT INTO cart (customer_name, flavor_name)
        VALUES (?, ?)''', (customer_name, flavor_name))
    
    conn.commit()
    conn.close()

    return jsonify({"message": f"{flavor_name} added to cart for {customer_name}!"}), 201
if __name__ == '__main__':
    app.run(debug=True)
docker build -t ice-cream-parlor .
docker run -p 5000:5000 ice-cream-parlor
