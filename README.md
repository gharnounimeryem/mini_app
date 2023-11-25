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
