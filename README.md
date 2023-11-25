# mini_app
#database
-- phpMyAdmin SQL Dump
-- version 5.2.0
-- https://www.phpmyadmin.net/
--
-- Hôte : 127.0.0.1
-- Généré le : sam. 25 nov. 2023 à 13:15
-- Version du serveur : 10.4.25-MariaDB
-- Version de PHP : 7.4.30

SET SQL_MODE = "NO_AUTO_VALUE_ON_ZERO";
START TRANSACTION;
SET time_zone = "+00:00";


/*!40101 SET @OLD_CHARACTER_SET_CLIENT=@@CHARACTER_SET_CLIENT */;
/*!40101 SET @OLD_CHARACTER_SET_RESULTS=@@CHARACTER_SET_RESULTS */;
/*!40101 SET @OLD_COLLATION_CONNECTION=@@COLLATION_CONNECTION */;
/*!40101 SET NAMES utf8mb4 */;

--
-- Base de données : `cuisine`
--
#login 
<fieldset>
	<legend>
		veillez remplire les champs suivant
	</legend>

<form method="post">

	<center>
	<table>
		<tr>
			<td>UTILISATEUR : </td>
			<td><input type="text" name="user"></td>
		</tr>
		<tr>
			<td>MOT DE PASSE : </td>
			<td><input type="password" name="mdp"></td>
		</tr>
		<tr>
			<td><input type="submit" name="envoyer"></td>
		</tr>
		</center>
	</table>
</form>
</fieldset>
<style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
        }

        fieldset {
            width: 50%;
            margin: 50px auto;
            padding: 20px;
            border: 2px solid #333;
            border-radius: 10px;
        }

        legend {
            font-size: 18px;
            font-weight: bold;
            color: #333;
        }

        table {
            width: 100%;
        }

        td {
            padding: 10px;
            text-align: left;
        }

        input[type="text"],
        input[type="password"] {
            width: 100%;
            padding: 8px;
            margin: 5px 0;
            box-sizing: border-box;
        }

        input[type="submit"] {
            padding: 10px;
            background-color: #4caf50;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }

        input[type="submit"]:hover {
            background-color: #45a049;
        }


    </style>
<?php
//3 - verification de login et mdp
include('connexiondb.php');
if(isset($_POST['envoyer'])){
	$req = $connexion->prepare("SELECT * FROM utilisateur WHERE user = ? AND mdp = ? ") ;
	$req->bindvalue(1 , $_POST["user"]);
	$req->bindvalue(2 , $_POST["mdp"]);
	$req->execute();
	$res = $req->fetchAll();
	if(empty($res))
		echo "verifier le nom ou le mot de passe";
	else{
		if($res[0][3] == "admin"){
			session_start();
			$getUser = $res[0][1];
			$getIdUtilisateur = $res[0][0] ;
			$_SESSION['user'] = $getUser;
			$_SESSION['role'] = $res[0][3] ;
			header('location:recetteaj.php');
			
		}
		else{
			session_start();
			$_SESSION['role'] = $res[0][3];
			$getUser = $res[0][1];
			$getIdUtilisateur = $res[0][0] ;
			$_SESSION['user'] = $getUser;
			header('location:modiferrec.php');
		}
	}
}
?>
#ajout de recette
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>Saveurs Maison</title>

	<h1>

<titre>Saveurs Maison</titre>
</h1>
<style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
            padding: 20px;
        }

        h1 {
            color: #333;
        }

        fieldset {
            margin-top: 20px;
            border: 1px solid #ccc;
            padding: 10px;
        }

        legend {
            font-weight: bold;
            color: #555;
        }

        table {
            width: 100%;
        }

        td {
            padding: 8px;
        }

        input[type="text"],
        textarea,
        input[type="time"] {
            width: 100%;
            padding: 8px;
            margin-bottom: 10px;
            box-sizing: border-box;
        }

        input[type="submit"] {
            background-color: #4CAF50;
            color: white;
            padding: 10px 15px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
        }

        input[type="submit"]:hover {
            background-color: #45a049;
        }
    </style>


</head>
<body>

<?php
	session_start();

	if (empty($_SESSION['user'])) {
		echo "empty";
	} elseif ($_SESSION['role'] == "utilisateur") {
		echo "vous avez pas le droit d'acceder a cette page";
	} else {
		include('masterPage.php');
	}
?>

<fieldset>
	<legend>Ajouter recette</legend>
	<form method="POST">
		<table>
			<tr>
				<td>
					
						<?php
							include('connexiondb.php');
							$req = $connexion->prepare("SELECT idnomrec,libelle FROM nomrec");
							$req->execute();
							$res = $req->fetchAll();
							foreach ($res as $item) {
								echo "<option value=" . $item['idnomrec'] . ">" . $item['libelle'] . "</option>";
							}
						?>
					</select>
				</td>
			</tr>
			<tr>
				<td>Nom :</td>
				<td><input type="text" name="titre"></td>
			</tr>
			<tr>
				<td>Liste d'ingrédients :</td>
				<td><textarea name="liste d'ingredients"></textarea></td>
			</tr>
			<tr>
				<td>Des étapes de préparation :</td>
				<td><textarea name="étapes de preparation"></textarea></td>
			</tr>
			<tr>
				<td>Durée de préparation :</td>
				<td><input type="time" name="time" id="time"></td>
			</tr>
			<tr>
				<td><input type="submit" name="ajouter" value="Ajouter"></td>
			</tr>
		</table>
	</form>
</fieldset>

<?php
	if (isset($_POST['ajouter'])) {
		try {
			include('connexiondb.php');
			$req = $connexion->prepare("INSERT INTO recette(idnomrec, nom, liste_ingredients, etapesdepreparation) VALUES (?, ?, ?, ?)");
			$req->bindValue(1, $_POST['idnomrec']);
			$req->bindValue(2, $_POST['titre']);
			$req->bindValue(3, $_POST['liste_ingredients']);
			$req->bindValue(4, $_POST['etapesdepreparation']);
			$req->execute();
			echo "Insertion terminée sans erreur" . "<br>";
		} catch (PDOException $e) {
			echo "Erreur : " . $e->getMessage();
		}
	}
?>

</body>
</html>
#supprimer
<!-- Add this section after your form for adding a recipe -->
<fieldset>
    <legend>Supprimer recette</legend>
    <form method="POST">
        <table>
            <tr>
                <th>Nom de recette</th>
                <th>Action</th>
            </tr>
            <?php
                // Modify the SQL query to fetch the recipes
                $req = $connexion->prepare("SELECT idrec, nom FROM recette");
                $req->execute();
                $res = $req->fetchAll();
                foreach ($res as $item) {
                    echo "<tr>";
                    echo "<td>" . $item['nom'] . "</td>";
                    echo "<td><button type='submit' name='delete' value='" . $item['idrec'] . "'>Supprimer</button></td>";
                    echo "</tr>";
                }
            ?>
        </table>
    </form>
</fieldset>
#connexion data base
<?php
	try{
			$chaine = 'mysql:host=localhost;dbname=cuisine;charset=utf8';
			$user = 'root' ;
			$password = '' ;
			$connexion = new PDO($chaine , $user , $password) ;
		}
		catch(PDOException $e){
			echo "erreur : ".$e->getMessage();
		}
?>
#modifier
<style>


/* Style de la liste des recettes */
table {
    width: 80%;
    margin: 20px auto;
    border-collapse: collapse;
    background-color: #fff;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

th, td {
    padding: 12px;
    border: 1px solid #ddd;
    text-align: left;
}

th {
    background-color: #4caf50;
    color: white;
}



</style>

<?php
session_start();
include('connexiondb.php');

function afficherTab($req){
    global $connexion;
    $req = $connexion->query($req);
    echo "liste des recettes";
    echo '<table border="1px">';


    while ($row = $req->fetch(PDO::FETCH_ASSOC)) {
        echo "<tr>";
        foreach ($row as $key => $value) {
            echo "<td>".$value."</td>";
        }
        echo "</tr>";
    }
    echo "</table>";
}

if(empty($_SESSION['user'])){
    // Si l'utilisateur n'est pas connecté, afficher le formulaire de connexion
    echo '<form action="authentification.php" method="POST" >
          <table align="">
          <tr><td>Username:</td><td><input type="text" name="username"></td></tr>
          <tr><td>Password:</td><td><input type="mdp" name="mdp"></td></tr>
          <tr><td></td><td><input type="submit" name="bouton" value="Connexion" class="bouton" ></td></tr>
          </table>
          </form>';
} else {
    // Si l'utilisateur est connecté, afficher les recettes
    afficherTab("SELECT * FROM recette");
}
?>
#logout 
<?php
session_start();
session_destroy();
header("Location: login.php");
exit();
?>

-- --------------------------------------------------------

--
-- Structure de la table `recette`
--

CREATE TABLE `recette` (
  `id` int(11) NOT NULL,
  `nom` varchar(20) NOT NULL,
  `liste d'ingrédients` text NOT NULL,
  `étapes de préparation` text NOT NULL,
  `duree` time NOT NULL DEFAULT current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

--
-- Déchargement des données de la table `recette`
--

INSERT INTO `recette` (`id`, `nom`, `liste d'ingrédients`, `étapes de préparation`, `duree`) VALUES
(1, 'Pâtes à la Carbonara', '400 g de spaghetti\r\n150 g de pancetta ou de lardons\r\n3 œufs\r\n1 tasse (environ 100 g) de parmesan râpé\r\nPoivre noir moulu, selon le goût\r\n2 gousses d\'ail (facultatif)\r\nPersil frais haché pour la garniture', 'Faire bouillir une grande casserole d\'eau salée. Cuire les pâtes selon les instructions sur l\'emballage jusqu\'à ce qu\'elles soient al dente.\r\n\r\nPendant ce temps, dans une poêle à feu moyen, faire revenir la pancetta ou les lardons jusqu\'à ce qu\'ils soient dorés et croustillants. Ajouter les gousses d\'ail émincées si vous les utilisez. Retirer du feu et réserver.\r\n\r\nDans un bol, battre les œufs. Ajouter le parmesan râpé et mélanger jusqu\'à obtenir une consistance crémeuse.\r\n\r\nUne fois que les pâtes sont cuites, égouttez-les rapidement, en réservant une tasse d\'eau de cuisson.\r\n\r\nAjoutez les pâtes égouttées à la poêle avec la pancetta/lardons. Mélangez bien pour que les pâtes soient enrobées du gras de la viande.\r\n\r\nRetirez la poêle du feu et ajoutez le mélange d\'œufs et de fromage. Remuez rapidement pour enrober les pâtes. Si la sauce semble trop épaisse, ajoutez un peu d\'eau de cuisson réservée, une cuillère à soupe à la fois, jusqu\'à ce que la consistance souhaitée soit atteinte.\r\n\r\nAssaisonnez avec du poivre noir moulu selon votre goût.\r\n\r\nGarnissez de persil frais haché et servez immédiatement.', '00:20:00'),
(2, 'Poulet rôti aux Herb', '1 poulet entier (environ 1,5 à 2 kg)\r\n2 citrons (1 pour le jus et l\'autre pour l\'intérieur du poulet)\r\n3 cuillères à soupe d\'huile d\'olive\r\n2 gousses d\'ail émincées\r\n1 cuillère à soupe de thym frais haché\r\n1 cuillère à soupe de romarin frais haché\r\nSel et poivre noir moulu, selon le goût\r\nLégumes pour accompagner (carottes, pommes de terre, oignons)', 'Préchauffez le four à 200°C.\r\n\r\nLavez le poulet à l\'intérieur et à l\'extérieur, puis séchez-le avec du papier absorbant.\r\n\r\nDans un bol, mélangez le jus d\'un citron, l\'huile d\'olive, l\'ail émincé, le thym et le romarin. Assaisonnez avec du sel et du poivre.\r\n\r\nPlacez le poulet dans un plat allant au four. Badigeonnez-le avec le mélange d\'huile d\'olive et d\'herbes, en vous assurant de bien enrober toute la surface.\r\n\r\nCoupez le deuxième citron en deux et placez les moitiés à l\'intérieur du poulet.\r\n\r\nAjoutez les légumes coupés en morceaux autour du poulet dans le plat.\r\n\r\nEnfournez le poulet et faites-le rôtir pendant environ 1 heure 30 minutes ou jusqu\'à ce qu\'un thermomètre inséré dans la partie la plus épaisse de la cuisse atteigne une température interne de 75°C.\r\n\r\nPendant la cuisson, arrosez le poulet toutes les 30 minutes avec les jus du plat pour qu\'il reste bien juteux.\r\n\r\nUne fois cuit, retirez le poulet du four et laissez-le reposer pendant quelques minutes avant de le découper.', '02:00:00'),
(3, 'Salade Méditerranéen', '200 g de quinoa\r\n1 concombre, coupé en dés\r\n250 g de tomates cerises, coupées en deux\r\n1 poivron rouge, coupé en dés\r\n1 oignon rouge, finement tranché\r\n100 g d\'olives noires, dénoyautées et coupées en deux\r\n200 g de fromage féta, émietté\r\n1/4 tasse (60 ml) d\'huile d\'olive extra vierge\r\nJus de 1 citron\r\n2 cuillères à soupe de vinaigre balsamique\r\n2 cuillères à café d\'origan séché\r\nSel et poivre noir moulu, selon le goût\r\nFeuilles de basilic frais pour la garniture', 'Rincer le quinoa sous l\'eau froide. Dans une casserole, cuire le quinoa selon les instructions sur l\'emballage. Laisser refroidir.\r\n\r\nDans un grand saladier, mélanger le quinoa cuit, le concombre, les tomates cerises, le poivron rouge, l\'oignon rouge, les olives noires et le fromage féta.\r\n\r\nDans un petit bol, préparer la vinaigrette en mélangeant l\'huile d\'olive, le jus de citron, le vinaigre balsamique, l\'origan séché, le sel et le poivre.\r\n\r\nVerser la vinaigrette sur la salade et mélanger délicatement pour bien enrober tous les ingrédients.\r\n\r\nGarnir la salade de feuilles de basilic frais.\r\n\r\nRéfrigérez la salade pendant au moins 30 minutes avant de servir pour permettre aux saveurs de se mélanger.', '00:40:00'),
(4, 'Brownies au Chocolat', '200 g de chocolat noir (70% de cacao)\r\n150 g de beurre\r\n200 g de sucre\r\n3 œufs\r\n1 cuillère à café d\'extrait de vanille\r\n100 g de farine tout usage\r\n1/4 de cuillère à café de sel\r\n100 g de noix ou de noisettes hachées (facultatif)', 'Préchauffez le four à 180°C. Graissez un moule carré de 20 cm ou tapissez-le de papier sulfurisé.\r\n\r\nFaites fondre le chocolat et le beurre au bain-marie ou au micro-ondes. Laissez refroidir légèrement.\r\n\r\nDans un grand bol, fouettez le sucre, les œufs et l\'extrait de vanille jusqu\'à ce que le mélange soit homogène.\r\n\r\nAjoutez le mélange de chocolat fondu au mélange d\'œufs et mélangez bien.\r\n\r\nTamisez la farine et le sel dans le bol et mélangez jusqu\'à ce que la pâte soit lisse.\r\n\r\nSi vous le souhaitez, ajoutez les noix ou les noisettes hachées et mélangez.\r\n\r\nVersez la pâte dans le moule préparé et étalez-la uniformément.\r\n\r\nFaites cuire au four préchauffé pendant environ 25-30 minutes ou jusqu\'à ce qu\'un cure-dent inséré au centre en ressorte avec quelques miettes (mais pas de pâte crue).\r\n\r\nLaissez refroidir dans le moule, puis coupez en carrés.', '00:45:00');

-- --------------------------------------------------------

--
-- Structure de la table `utilisateur`
--

CREATE TABLE `utilisateur` (
  `idUsers` int(11) NOT NULL,
  `user` varchar(50) NOT NULL,
  `mdp` varchar(50) NOT NULL,
  `role` varchar(50) NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

--
-- Déchargement des données de la table `utilisateur`
--

INSERT INTO `utilisateur` (`idUsers`, `user`, `mdp`, `role`) VALUES
(1, 'meryem', '1234', 'admin'),
(2, 'USER1', '0000', 'utilisateur'),
(3, 'USER2', '1111', 'utilisateur');

--
-- Index pour les tables déchargées
--

--
-- Index pour la table `recette`
--
ALTER TABLE `recette`
  ADD PRIMARY KEY (`id`);

--
-- Index pour la table `utilisateur`
--
ALTER TABLE `utilisateur`
  ADD PRIMARY KEY (`idUsers`);

--
-- AUTO_INCREMENT pour les tables déchargées
--

--
-- AUTO_INCREMENT pour la table `recette`
--
ALTER TABLE `recette`
  MODIFY `id` int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=5;

--
-- AUTO_INCREMENT pour la table `utilisateur`
--
ALTER TABLE `utilisateur`
  MODIFY `idUsers` int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=4;
COMMIT;

/*!40101 SET CHARACTER_SET_CLIENT=@OLD_CHARACTER_SET_CLIENT */;
/*!40101 SET CHARACTER_SET_RESULTS=@OLD_CHARACTER_SET_RESULTS */;
/*!40101 SET COLLATION_CONNECTION=@OLD_COLLATION_CONNECTION */;
