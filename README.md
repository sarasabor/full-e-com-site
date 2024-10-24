# full-e-com-site


Étape 1 : Configuration de l'environnement de développement
Installer Node.js et MongoDB :

Téléchargez et installez Node.js et MongoDB.
Assurez-vous de vérifier l'installation en exécutant node -v et mongo --version dans le terminal.
Installer MongoDB Compass (facultatif) :

Pour une gestion graphique de vos bases de données, vous pouvez télécharger MongoDB Compass.
Étape 2 : Initialisation du projet
Créer un dossier de projet :

bash
mkdir ecommerce-app  
cd ecommerce-app  
Initialiser le projet backend :

bash
mkdir backend  
cd backend  
npm init -y  
npm install express mongoose cors dotenv jsonwebtoken bcryptjs  
Initialiser le projet frontend :

bash
cd ..  
npx create-react-app frontend  
cd frontend  
npm install axios react-router-dom  
Étape 3 : Développement du backend
Créer un fichier server.js dans le dossier backend :

javascript
const express = require('express');  
const mongoose = require('mongoose');  
const cors = require('cors');  
const dotenv = require('dotenv');  

dotenv.config();  

const app = express();  
app.use(cors());  
app.use(express.json());  

mongoose.connect(process.env.MONGO_URI, { useNewUrlParser: true, useUnifiedTopology: true })  
    .then(() => console.log('MongoDB connected'))  
    .catch(err => console.log(err));  

app.listen(process.env.PORT || 5000, () => {  
    console.log('Server is running on port 5000');  
});  
Créer les modèles de données :

Créez un dossier models et ajoutez par exemple un modèle de produit (Product.js).
javascript
const mongoose = require('mongoose');  

const productSchema = new mongoose.Schema({  
    name: { type: String, required: true },  
    price: { type: Number, required: true },  
    description: { type: String, required: true },  
    image: { type: String, required: true }  
});  

module.exports = mongoose.model('Product', productSchema);  
Créer des routes pour gérer les produits :

Créez un dossier routes et un fichier productRoutes.js.
javascript
const express = require('express');  
const router = express.Router();  
const Product = require('../models/Product');  

// Exemple de route pour récupérer les produits  
router.get('/', async (req, res) => {  
    try {  
        const products = await Product.find();  
        res.json(products);  
    } catch (error) {  
        res.status(500).send(error);  
    }  
});  

module.exports = router;  
Lier les routes dans server.js :

javascript
const productRoutes = require('./routes/productRoutes');  
app.use('/api/products', productRoutes);  
Étape 4 : Développement du frontend
Créer des composants pour l'application :

Dans src, créez des dossiers pour les composants, par exemple components/ProductList.js et components/ProductDetail.js.
Exemple de ProductList.js:

javascript
import React, { useEffect, useState } from 'react';  
import axios from 'axios';  

const ProductList = () => {  
    const [products, setProducts] = useState([]);  

    useEffect(() => {  
        const fetchProducts = async () => {  
            const res = await axios.get('http://localhost:5000/api/products');  
            setProducts(res.data);  
        };  
        fetchProducts();  
    }, []);  

    return (  
        <div>  
            <h1>Liste des Produits</h1>  
            <div>  
                {products.map(product => (  
                    <div key={product._id}>  
                        <h2>{product.name}</h2>  
                        <p>{product.price} $</p>  
                        <p>{product.description}</p>  
                    </div>  
                ))}  
            </div>  
        </div>  
    );  
};  

export default ProductList;  
Intégrer ProductList dans votre App.js:

javascript
import React from 'react';  
import ProductList from './components/ProductList';  

const App = () => {  
    return (  
        <div>  
            <ProductList />  
        </div>  
    );  
};  

export default App;  
Étape 5 : Exécution de l'application
Démarrer le backend :
Dans le dossier backend, exécutez :

bash
node server.js  
Démarrer le frontend :
Dans le dossier frontend, exécutez :

bash
npm start  
Étape 6 : Déploiement
Vous pouvez choisir d'héberger votre application sur un service cloud comme Heroku, AWS, ou Microsoft Azure. Chaque service a ses propres étapes et exigences. Pour un déploiement local, assurez-vous que le MongoDB est également hébergé sur une instance accessible.
