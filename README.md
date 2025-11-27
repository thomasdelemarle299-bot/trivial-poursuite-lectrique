import React, { useState, useEffect } from 'react';
import { Zap, Trophy, RotateCcw, CheckCircle, XCircle, Shuffle } from 'lucide-react';

const TrivialPursuitElectricite = () => {
  const [currentQuestion, setCurrentQuestion] = useState(0);
  const [score, setScore] = useState(0);
  const [selectedAnswer, setSelectedAnswer] = useState(null);
  const [showResult, setShowResult] = useState(false);
  const [gameCompleted, setGameCompleted] = useState(false);
  const [shuffledQuestions, setShuffledQuestions] = useState([]);
  const [questionsToPlay, setQuestionsToPlay] = useState(20);

  const allQuestions = [
    // Loi d'Ohm
    {
      category: "Loi d'Ohm",
      question: "Quelle est la formule de la loi d'Ohm ?",
      answers: ["U = R × I", "I = U × R", "R = U × I", "P = U × I"],
      correct: 0,
      explanation: "La loi d'Ohm s'écrit U = R × I, où U est la tension, R la résistance et I l'intensité."
    },
    {
      category: "Loi d'Ohm",
      question: "Si U = 12V et R = 4Ω, alors I = ?",
      answers: ["48A", "3A", "8A", "16A"],
      correct: 1,
      explanation: "I = U/R = 12/4 = 3A"
    },
    {
      category: "Loi d'Ohm",
      question: "Si I = 2A et R = 6Ω, alors U = ?",
      answers: ["12V", "3V", "8V", "4V"],
      correct: 0,
      explanation: "U = R × I = 6 × 2 = 12V"
    },
    {
      category: "Loi d'Ohm",
      question: "Si U = 24V et I = 3A, alors R = ?",
      answers: ["8Ω", "72Ω", "21Ω", "27Ω"],
      correct: 0,
      explanation: "R = U/I = 24/3 = 8Ω"
    },
    {
      category: "Loi d'Ohm",
      question: "La résistance d'un conducteur ohmique est :",
      answers: ["Variable selon la tension", "Variable selon l'intensité", "Constante", "Nulle"],
      correct: 2,
      explanation: "Pour un conducteur ohmique, la résistance est constante, indépendante de U et I."
    },
    {
      category: "Loi d'Ohm",
      question: "Dans la relation U = R × I, si on double la tension :",
      answers: ["L'intensité double", "L'intensité diminue de moitié", "La résistance double", "Rien ne change"],
      correct: 0,
      explanation: "Si U double et R reste constante, alors I = U/R double aussi."
    },
    {
      category: "Loi d'Ohm",
      question: "Une résistance de 100Ω traversée par 0,5A dissipe une tension de :",
      answers: ["200V", "50V", "150V", "25V"],
      correct: 1,
      explanation: "U = R × I = 100 × 0,5 = 50V"
    },
    {
      category: "Loi d'Ohm",
      question: "La caractéristique U = f(I) d'un conducteur ohmique est :",
      answers: ["Une parabole", "Une droite passant par l'origine", "Une hyperbole", "Une sinusoïde"],
      correct: 1,
      explanation: "U = R × I est l'équation d'une droite de coefficient directeur R passant par l'origine."
    },
    {
      category: "Loi d'Ohm",
      question: "Si la résistance augmente et la tension reste constante :",
      answers: ["L'intensité augmente", "L'intensité diminue", "L'intensité reste constante", "La tension diminue"],
      correct: 1,
      explanation: "I = U/R, donc si R augmente et U reste constant, I diminue."
    },
    {
      category: "Loi d'Ohm",
      question: "L'unité de la résistance électrique est :",
      answers: ["L'ampère (A)", "Le volt (V)", "L'ohm (Ω)", "Le watt (W)"],
      correct: 2,
      explanation: "L'ohm (Ω) est l'unité de résistance électrique."
    },

    // Puissance
    {
      category: "Puissance",
      question: "Comment calcule-t-on la puissance électrique ?",
      answers: ["P = U / I", "P = U × I", "P = U + I", "P = U - I"],
      correct: 1,
      explanation: "La puissance électrique se calcule par P = U × I (tension × intensité)."
    },
    {
      category: "Puissance",
      question: "L'unité de la puissance électrique est :",
      answers: ["Le Joule (J)", "Le Watt (W)", "L'Ampère (A)", "Le Volt (V)"],
      correct: 1,
      explanation: "La puissance s'exprime en Watts (W)."
    },
    {
      category: "Puissance",
      question: "La puissance dissipée par une résistance peut s'écrire :",
      answers: ["P = R × I", "P = R × I²", "P = R + I²", "P = R / I²"],
      correct: 1,
      explanation: "P = R × I² (ou aussi P = U²/R grâce à la loi d'Ohm)"
    },
    {
      category: "Puissance",
      question: "Si U = 12V et I = 2A, alors P = ?",
      answers: ["6W", "14W", "24W", "10W"],
      correct: 2,
      explanation: "P = U × I = 12 × 2 = 24W"
    },
    {
      category: "Puissance",
      question: "Une résistance de 10Ω traversée par 3A dissipe :",
      answers: ["30W", "90W", "13W", "7W"],
      correct: 1,
      explanation: "P = R × I² = 10 × 3² = 10 × 9 = 90W"
    },
    {
      category: "Puissance",
      question: "Si P = 100W et U = 20V, alors I = ?",
      answers: ["5A", "80A", "120A", "2000A"],
      correct: 0,
      explanation: "I = P/U = 100/20 = 5A"
    },
    {
      category: "Puissance",
      question: "Une ampoule de 60W sous 230V est traversée par :",
      answers: ["0,26A", "2,6A", "13800A", "290A"],
      correct: 0,
      explanation: "I = P/U = 60/230 ≈ 0,26A"
    },
    {
      category: "Puissance",
      question: "Doubler la tension d'une résistance :",
      answers: ["Double la puissance", "Quadruple la puissance", "Divise la puissance par 2", "Ne change rien"],
      correct: 1,
      explanation: "P = U²/R, donc si U double, P devient 4 fois plus grande."
    },
    {
      category: "Puissance",
      question: "La formule P = U²/R s'obtient en combinant P = UI avec :",
      answers: ["I = U/R", "U = RI", "R = U/I", "Toutes les réponses"],
      correct: 3,
      explanation: "En remplaçant I par U/R dans P = UI, on obtient P = U²/R."
    },
    {
      category: "Puissance",
      question: "Un radiateur de 2000W sous 230V a une résistance de :",
      answers: ["26,45Ω", "115Ω", "460Ω", "4,6Ω"],
      correct: 0,
      explanation: "R = U²/P = 230²/2000 = 52900/2000 = 26,45Ω"
    },

    // Résistances
    {
      category: "Résistances",
      question: "Comment se comportent des résistances en série ?",
      answers: ["R_totale = R1 + R2 + R3", "1/R_totale = 1/R1 + 1/R2", "R_totale = R1 × R2", "R_totale = R1 / R2"],
      correct: 0,
      explanation: "En série, les résistances s'additionnent : R_totale = R1 + R2 + R3..."
    },
    {
      category: "Résistances",
      question: "Deux résistances de 10Ω en parallèle donnent une résistance équivalente de :",
      answers: ["20Ω", "10Ω", "5Ω", "1Ω"],
      correct: 2,
      explanation: "1/R = 1/10 + 1/10 = 2/10, donc R = 5Ω"
    },
    {
      category: "Résistances",
      question: "En parallèle, la résistance équivalente est :",
      answers: ["Plus grande que la plus grande", "Plus petite que la plus petite", "Égale à la moyenne", "Égale à la somme"],
      correct: 1,
      explanation: "En parallèle, R_eq est toujours inférieure à la plus petite des résistances."
    },
    {
      category: "Résistances",
      question: "Trois résistances de 6Ω en série donnent :",
      answers: ["2Ω", "6Ω", "18Ω", "36Ω"],
      correct: 2,
      explanation: "R_série = 6 + 6 + 6 = 18Ω"
    },
    {
      category: "Résistances",
      question: "Trois résistances de 9Ω en parallèle donnent :",
      answers: ["27Ω", "9Ω", "3Ω", "1Ω"],
      correct: 2,
      explanation: "1/R = 1/9 + 1/9 + 1/9 = 3/9, donc R = 3Ω"
    },
    {
      category: "Résistances",
      question: "Une résistance de 4Ω en série avec deux résistances de 2Ω en parallèle donne :",
      answers: ["8Ω", "5Ω", "6Ω", "4Ω"],
      correct: 1,
      explanation: "Les 2Ω en parallèle donnent 1Ω, puis 4 + 1 = 5Ω"
    },
    {
      category: "Résistances",
      question: "Le code couleur d'une résistance de 470Ω est :",
      answers: ["Jaune-Violet-Marron", "Jaune-Violet-Rouge", "Violet-Jaune-Marron", "Rouge-Violet-Jaune"],
      correct: 0,
      explanation: "470Ω : Jaune(4) - Violet(7) - Marron(×10)"
    },
    {
      category: "Résistances",
      question: "Une résistance Marron-Noir-Rouge a pour valeur :",
      answers: ["102Ω", "1000Ω", "120Ω", "10Ω"],
      correct: 1,
      explanation: "Marron(1) - Noir(0) - Rouge(×100) = 10 × 100 = 1000Ω = 1kΩ"
    },
    {
      category: "Résistances",
      question: "La tolérance d'une résistance indique :",
      answers: ["Sa température maximale", "Sa précision de fabrication", "Sa puissance maximale", "Sa tension maximale"],
      correct: 1,
      explanation: "La tolérance indique l'écart possible entre la valeur réelle et la valeur nominale."
    },
    {
      category: "Résistances",
      question: "Une bande dorée sur une résistance indique une tolérance de :",
      answers: ["1%", "5%", "10%", "20%"],
      correct: 1,
      explanation: "Doré = 5% de tolérance, Argenté = 10%"
    },

    // Circuits
    {
      category: "Circuits",
      question: "Dans un circuit en parallèle, que peut-on dire de la tension ?",
      answers: ["Elle diminue à chaque branche", "Elle est différente sur chaque branche", "Elle est identique sur toutes les branches", "Elle s'additionne"],
      correct: 2,
      explanation: "En parallèle, la tension est identique aux bornes de chaque branche."
    },
    {
      category: "Circuits",
      question: "Un court-circuit se produit quand :",
      answers: ["La résistance est très grande", "La résistance est proche de zéro", "Il n'y a pas de tension", "Il n'y a pas de courant"],
      correct: 1,
      explanation: "Un court-circuit correspond à une résistance très faible (proche de 0), ce qui peut créer un courant très intense et dangereux."
    },
    {
      category: "Circuits",
      question: "Dans un circuit série, si une résistance grille :",
      answers: ["Le circuit continue à fonctionner", "Tout le circuit s'arrête", "Seule cette résistance s'arrête", "La tension augmente"],
      correct: 1,
      explanation: "En série, tous les éléments sont sur le même chemin. Si un élément s'arrête, tout s'arrête."
    },
    {
      category: "Circuits",
      question: "Dans un circuit parallèle, si une branche s'arrête :",
      answers: ["Tout s'arrête", "Les autres branches continuent", "La tension chute", "L'intensité augmente partout"],
      correct: 1,
      explanation: "En parallèle, chaque branche est indépendante des autres."
    },
    {
      category: "Circuits",
      question: "La loi des nœuds stipule que :",
      answers: ["U1 + U2 = U_totale", "I_entrant = I_sortant", "R1 + R2 = R_totale", "P1 + P2 = P_totale"],
      correct: 1,
      explanation: "La somme des intensités qui arrivent à un nœud égale la somme des intensités qui en partent."
    },
    {
      category: "Circuits",
      question: "La loi des mailles stipule que :",
      answers: ["∑U = 0 dans une maille fermée", "∑I = 0 dans une maille", "∑R = 0 dans une maille", "∑P = 0 dans une maille"],
      correct: 0,
      explanation: "Dans une maille fermée, la somme algébrique des tensions est nulle."
    },
    {
      category: "Circuits",
      question: "Un voltmètre se branche :",
      answers: ["En série", "En parallèle", "N'importe comment", "Seulement aux bornes du générateur"],
      correct: 1,
      explanation: "Un voltmètre se branche en parallèle avec l'élément dont on veut mesurer la tension."
    },
    {
      category: "Circuits",
      question: "Un ampèremètre se branche :",
      answers: ["En série", "En parallèle", "N'importe comment", "Seulement aux bornes du générateur"],
      correct: 0,
      explanation: "Un ampèremètre se branche en série dans la branche où on veut mesurer l'intensité."
    },
    {
      category: "Circuits",
      question: "La résistance interne d'un voltmètre idéal est :",
      answers: ["Nulle", "Infinie", "Égale à 1Ω", "Variable"],
      correct: 1,
      explanation: "Un voltmètre idéal a une résistance infinie pour ne pas perturber le circuit."
    },
    {
      category: "Circuits",
      question: "La résistance interne d'un ampèremètre idéal est :",
      answers: ["Nulle", "Infinie", "Égale à 1Ω", "Variable"],
      correct: 0,
      explanation: "Un ampèremètre idéal a une résistance nulle pour ne pas perturber le circuit."
    },

    // Énergie
    {
      category: "Énergie",
      question: "L'unité de l'énergie électrique est :",
      answers: ["Le Watt (W)", "Le Joule (J)", "L'Ampère (A)", "Le Volt (V)"],
      correct: 1,
      explanation: "L'énergie s'exprime en Joules (J). Le Watt est l'unité de puissance."
    },
    {
      category: "Énergie",
      question: "1 kWh équivaut à :",
      answers: ["1000 J", "3600 J", "3600000 J", "1000000 J"],
      correct: 2,
      explanation: "1 kWh = 1000 W × 3600 s = 3 600 000 J = 3,6 MJ"
    },
    {
      category: "Énergie",
      question: "L'énergie dissipée par une résistance se calcule par :",
      answers: ["E = P × t", "E = U × I", "E = R × I", "E = U / t"],
      correct: 0,
      explanation: "L'énergie = Puissance × temps, donc E = P × t"
    },
    {
      category: "Énergie",
      question: "Un radiateur de 1500W fonctionne 2h. Il consomme :",
      answers: ["750 Wh", "3000 Wh", "3 kWh", "1,5 kWh"],
      correct: 2,
      explanation: "E = P × t = 1500W × 2h = 3000 Wh = 3 kWh"
    },
    {
      category: "Énergie",
      question: "Le compteur électrique mesure :",
      answers: ["La puissance", "L'intensité", "L'énergie", "La tension"],
      correct: 2,
      explanation: "Le compteur électrique mesure l'énergie consommée en kWh."
    },
    {
      category: "Énergie",
      question: "Un téléviseur de 200W fonctionne 4h par jour pendant un mois (30 jours). Sa consommation est :",
      answers: ["24 kWh", "200 kWh", "800 Wh", "6000 Wh"],
      correct: 0,
      explanation: "E = 200W × 4h × 30j = 24000 Wh = 24 kWh"
    },
    {
      category: "Énergie",
      question: "L'effet Joule transforme l'énergie électrique en :",
      answers: ["Énergie mécanique", "Énergie thermique", "Énergie lumineuse", "Énergie chimique"],
      correct: 1,
      explanation: "L'effet Joule convertit l'énergie électrique en chaleur (énergie thermique)."
    },
    {
      category: "Énergie",
      question: "1 Joule équivaut à :",
      answers: ["1 W·s", "1 kW·h", "1 V·A", "1 Ω·A"],
      correct: 0,
      explanation: "1 Joule = 1 Watt × 1 seconde = 1 W·s"
    },
    {
      category: "Énergie",
      question: "Une batterie de 12V débitant 5A pendant 2h fournit une énergie de :",
      answers: ["120 J", "432000 J", "60 J", "24 J"],
      correct: 1,
      explanation: "E = U × I × t = 12V × 5A × 7200s = 432000 J"
    },
    {
      category: "Énergie",
      question: "Le rendement d'un appareil est le rapport :",
      answers: ["Énergie consommée / Énergie utile", "Énergie utile / Énergie consommée", "Puissance / Temps", "Tension / Courant"],
      correct: 1,
      explanation: "Rendement = Énergie utile / Énergie consommée (toujours < 1)"
    },

    // Intensité
    {
      category: "Intensité",
      question: "L'intensité électrique correspond à :",
      answers: ["La force du courant", "Le débit de charges électriques", "La pression électrique", "La résistance du circuit"],
      correct: 1,
      explanation: "L'intensité mesure le débit de charges électriques (quantité de charges par seconde)."
    },
    {
      category: "Intensité",
      question: "L'unité de l'intensité électrique est :",
      answers: ["Le Volt (V)", "L'Ohm (Ω)", "L'Ampère (A)", "Le Watt (W)"],
      correct: 2,
      explanation: "L'intensité s'exprime en Ampères (A)."
    },
    {
      category: "Intensité",
      question: "1 Ampère correspond à :",
      answers: ["1 Coulomb par seconde", "1 Joule par seconde", "1 Volt par Ohm", "1 Watt par Volt"],
      correct: 0,
      explanation: "1 A = 1 C/s (1 Coulomb de charge par seconde)"
    },
    {
      category: "Intensité",
      question: "Dans un circuit série, l'intensité :",
      answers: ["Varie selon la résistance", "Est identique partout", "S'additionne", "Diminue"],
      correct: 1,
      explanation: "En série, l'intensité est la même en tout point du circuit."
    },
    {
      category: "Intensité",
      question: "Dans un circuit parallèle, l'intensité :",
      answers: ["Est identique dans chaque branche", "Se répartit selon les résistances", "S'annule", "Devient infinie"],
      correct: 1,
      explanation: "En parallèle, l'intensité se répartit dans chaque branche selon la loi d'Ohm."
    },
    {
      category: "Intensité",
      question: "Un courant de 500 mA correspond à :",
      answers: ["0,5 A", "5 A", "50 A", "0,05 A"],
      correct: 0,
      explanation: "500 mA = 500/1000 A = 0,5 A"
    },
    {
      category: "Intensité",
      question: "Le sens conventionnel du courant va :",
      answers: ["Du + vers le -", "Du - vers le +", "Dans les deux sens", "Dépend du circuit"],
      correct: 0,
      explanation: "Par convention, le courant va de la borne + vers la borne - du générateur."
    },
    {
      category: "Intensité",
      question: "Un ampèremètre affiche une valeur négative, cela signifie :",
      answers: ["Il est défaillant", "Le courant va dans l'autre sens", "Il n'y a pas de courant", "La tension est négative"],
      correct: 1,
      explanation: "Une valeur négative indique que le courant circule dans le sens opposé à celui prévu."
    },
    {
      category: "Intensité",
      question: "Pour mesurer l'intensité dans une branche, l'ampèremètre doit :",
      answers: ["Être branché en parallèle", "Être branché en série", "Être branché n'importe où", "Ne pas être branché"],
      correct: 1,
      explanation: "L'ampèremètre se branche toujours en série pour mesurer le courant qui traverse."
    },
    {
      category: "Intensité",
      question: "Si on inverse les bornes d'un ampèremètre :",
      answers: ["Il se casse", "La valeur devient négative", "Il ne fonctionne plus", "La valeur reste positive"],
      correct: 1,
      explanation: "Inverser les bornes change le signe de la mesure mais ne détruit pas l'appareil."
    },

    // Histoire de l'électricité - 25 questions
    {
      category: "Histoire",
      question: "Qui a découvert la loi qui porte son nom : U = R × I ?",
      answers: ["Ampère", "Volta", "Ohm", "Coulomb"],
      correct: 2,
      explanation: "Georg Simon Ohm (1789-1854) a découvert la relation entre tension, courant et résistance en 1827."
    },
    {
      category: "Histoire",
      question: "Le nom de l'unité 'Ampère' vient de :",
      answers: ["André-Marie Ampère", "Alessandro Volta", "Charles Coulomb", "Michael Faraday"],
      correct: 0,
      explanation: "André-Marie Ampère (1775-1836) est le physicien français qui a étudié l'électrodynamique."
    },
    {
      category: "Histoire",
      question: "Le 'Volt' tire son nom de :",
      answers: ["Thomas Edison", "Alessandro Volta", "Nikola Tesla", "Benjamin Franklin"],
      correct: 1,
      explanation: "Alessandro Volta (1745-1827) a inventé la première pile électrique en 1800."
    },
    {
      category: "Histoire",
      question: "Qui a inventé la première pile électrique ?",
      answers: ["Edison", "Volta", "Galvani", "Franklin"],
      correct: 1,
      explanation: "Alessandro Volta a créé la première pile (pile voltaïque) en 1800 avec des disques de cuivre et zinc."
    },
    {
      category: "Histoire",
      question: "Le Coulomb (unité de charge électrique) doit son nom à :",
      answers: ["Charles de Coulomb", "Marie Curie", "Isaac Newton", "Galileo Galilei"],
      correct: 0,
      explanation: "Charles-Augustin de Coulomb (1736-1806) a étudié les forces électrostatiques."
    },
    {
      category: "Histoire",
      question: "Qui a découvert l'induction électromagnétique ?",
      answers: ["Ampère", "Ohm", "Faraday", "Maxwell"],
      correct: 2,
      explanation: "Michael Faraday découvrit l'induction électromagnétique en 1831, base des générateurs électriques."
    },
    {
      category: "Histoire",
      question: "Le Watt (unité de puissance) honore :",
      answers: ["James Watt", "Thomas Edison", "Nikola Tesla", "Georg Ohm"],
      correct: 0,
      explanation: "James Watt (1736-1819) était un ingénieur écossais qui améliora la machine à vapeur."
    },
    {
      category: "Histoire",
      question: "Qui a inventé le paratonnerre ?",
      answers: ["Tesla", "Edison", "Franklin", "Volta"],
      correct: 2,
      explanation: "Benjamin Franklin inventa le paratonnerre en 1752 après ses expériences sur la foudre."
    },
    {
      category: "Histoire",
      question: "L'ampoule électrique à incandescence a été perfectionnée par :",
      answers: ["Tesla", "Edison", "Faraday", "Marconi"],
      correct: 1,
      explanation: "Thomas Edison perfectionna l'ampoule électrique en 1879 avec un filament de carbone."
    },
    {
      category: "Histoire",
      question: "Qui a découvert les rayons X ?",
      answers: ["Marie Curie", "Röntgen", "Becquerel", "Thomson"],
      correct: 1,
      explanation: "Wilhelm Röntgen découvrit les rayons X en 1895, révolutionnant la médecine."
    },
    {
      category: "Histoire",
      question: "Le premier générateur électrique industriel fut inventé par :",
      answers: ["Faraday", "Gramme", "Siemens", "Edison"],
      correct: 1,
      explanation: "Zénobe Gramme inventa la machine de Gramme en 1869, premier générateur industriel."
    },
    {
      category: "Histoire",
      question: "Qui a proposé le modèle de l'atome avec des électrons ?",
      answers: ["Rutherford", "Thomson", "Bohr", "Dalton"],
      correct: 1,
      explanation: "J.J. Thomson découvrit l'électron en 1897 et proposa le premier modèle atomique avec électrons."
    },
    {
      category: "Histoire",
      question: "La première centrale électrique commerciale fut construite par :",
      answers: ["Tesla", "Edison", "Westinghouse", "Siemens"],
      correct: 1,
      explanation: "Edison construisit la première centrale électrique commerciale à New York en 1882."
    },
    {
      category: "Histoire",
      question: "Qui a inventé le moteur électrique à courant alternatif ?",
      answers: ["Edison", "Tesla", "Faraday", "Westinghouse"],
      correct: 1,
      explanation: "Nikola Tesla inventa le moteur à induction (courant alternatif) en 1887."
    },
    {
      category: "Histoire",
      question: "Le galvanomètre (ancêtre de l'ampèremètre) fut inventé par :",
      answers: ["Galvani", "Oersted", "Schweigger", "Henry"],
      correct: 2,
      explanation: "Johann Schweigger inventa le galvanomètre en 1820 pour mesurer les courants faibles."
    },
    {
      category: "Histoire",
      question: "Qui a découvert que les courants électriques produisent des champs magnétiques ?",
      answers: ["Ampère", "Oersted", "Faraday", "Maxwell"],
      correct: 1,
      explanation: "Hans Christian Oersted découvrit en 1820 qu'un courant électrique dévie une aiguille magnétique."
    },
    {
      category: "Histoire",
      question: "La 'guerre des courants' opposait principalement :",
      answers: ["Tesla vs Marconi", "Edison vs Tesla", "Volta vs Galvani", "Ohm vs Ampère"],
      correct: 1,
      explanation: "La guerre des courants (1880s) opposait Edison (courant continu) à Tesla (courant alternatif)."
    },
    {
      category: "Histoire",
      question: "Qui a établi les équations fondamentales de l'électromagnétisme ?",
      answers: ["Faraday", "Maxwell", "Hertz", "Planck"],
      correct: 1,
      explanation: "James Clerk Maxwell unifia électricité et magnétisme dans ses équations (1864)."
    },
    {
      category: "Histoire",
      question: "Le premier voltmètre électrostatique fut développé par :",
      answers: ["Volta", "Thomson (Lord Kelvin)", "Galvani", "Henry"],
      correct: 1,
      explanation: "Lord Kelvin développa les premiers voltmètres électrostatiques précis vers 1867."
    },
    {
      category: "Histoire",
      question: "Qui a découvert la radioactivité naturelle ?",
      answers: ["Marie Curie", "Pierre Curie", "Becquerel", "Rutherford"],
      correct: 2,
      explanation: "Henri Becquerel découvrit la radioactivité naturelle en 1896 avec l'uranium."
    },
    {
      category: "Histoire",
      question: "La première pile à combustible fut inventée par :",
      answers: ["Grove", "Daniell", "Leclanché", "Planté"],
      correct: 0,
      explanation: "William Grove inventa la première pile à combustible (hydrogène-oxygène) en 1839."
    },
    {
      category: "Histoire",
      question: "Qui a démontré la nature électrique de la foudre ?",
      answers: ["Franklin", "Volta", "Galvani", "Coulomb"],
      correct: 0,
      explanation: "Benjamin Franklin prouva la nature électrique de la foudre avec son cerf-volant en 1752."
    },
    {
      category: "Histoire",
      question: "Le premier transformateur électrique fut inventé par :",
      answers: ["Faraday", "Gaulard et Gibbs", "Tesla", "Westinghouse"],
      correct: 1,
      explanation: "Lucien Gaulard et John Gibbs inventèrent le premier transformateur pratique en 1884."
    },
    {
      category: "Histoire",
      question: "Qui a inventé le premier accumulateur (batterie rechargeable) au plomb ?",
      answers: ["Volta", "Planté", "Leclanché", "Edison"],
      correct: 1,
      explanation: "Gaston Planté inventa l'accumulateur au plomb en 1859, encore utilisé aujourd'hui."
    },
    {
      category: "Histoire",
      question: "Le premier câble électrique sous-marin transatlantique fut posé en :",
      answers: ["1858", "1865", "1870", "1875"],
      correct: 0,
      explanation: "Le premier câble télégraphique transatlantique fut posé en 1858 (mais ne fonctionna que 3 semaines)."
    },

    // Questions bonus variées
    {
      category: "Loi d'Ohm",
      question: "Une tension de 9V aux bornes d'une résistance de 3Ω produit un courant de :",
      answers: ["27A", "3A", "6A", "12A"],
      correct: 1,
      explanation: "I = U/R = 9/3 = 3A"
    },
    {
      category: "Résistances",
      question: "Deux résistances 3Ω et 6Ω en parallèle donnent :",
      answers: ["9Ω", "4,5Ω", "2Ω", "1,5Ω"],
      correct: 2,
      explanation: "1/R = 1/3 + 1/6 = 2/6 + 1/6 = 3/6 = 1/2, donc R = 2Ω"
    },
    {
      category: "Puissance",
      question: "Si on divise la résistance par 2 à tension constante :",
      answers: ["P reste identique", "P est divisée par 2", "P est multipliée par 2", "P est divisée par 4"],
      correct: 2,
      explanation: "P = U²/R, donc si R est divisée par 2, P est multipliée par 2."
    },
    {
      category: "Circuits",
      question: "Un générateur idéal de tension :",
      answers: ["A une résistance interne nulle", "A une résistance interne infinie", "Fournit une intensité constante", "Ne peut pas débiter"],
      correct: 0,
      explanation: "Un générateur idéal de tension maintient une tension constante avec r = 0."
    },
    {
      category: "Énergie",
      question: "Un fer à repasser de 1200W fonctionne 30min. L'énergie consommée est :",
      answers: ["36 kJ", "2160 kJ", "600 kJ", "40 kJ"],
      correct: 1,
      explanation: "E = P × t = 1200W × 1800s = 2160000J = 2160kJ"
    }
  ];

  const categories = ["Loi d'Ohm", "Puissance", "Résistances", "Circuits", "Énergie", "Intensité", "Histoire"];
  const categoryColors = {
    "Loi d'Ohm": "bg-blue-500",
    "Puissance": "bg-red-500", 
    "Résistances": "bg-green-500",
    "Circuits": "bg-purple-500",
    "Énergie": "bg-yellow-500",
    "Intensité": "bg-pink-500",
    "Histoire": "bg-orange-500"
  };

  useEffect(() => {
    // Fonction pour mélanger un tableau (algorithme Fisher-Yates)
    const shuffleArray = (array) => {
      const shuffled = [...array];
      for (let i = shuffled.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [shuffled[i], shuffled[j]] = [shuffled[j], shuffled[i]];
      }
      return shuffled;
    };

    if (questionsToPlay > 0 && shuffledQuestions.length === 0) {
      const shuffled = shuffleArray(allQuestions);
      setShuffledQuestions(shuffled.slice(0, Math.min(questionsToPlay, allQuestions.length)));
    }
  }, [questionsToPlay]);

  const handleAnswerClick = (answerIndex) => {
    if (showResult) return;
    
    setSelectedAnswer(answerIndex);
    setShowResult(true);
    
    if (answerIndex === shuffledQuestions[currentQuestion].correct) {
      setScore(score + 1);
    }
  };

  const nextQuestion = () => {
    if (currentQuestion < shuffledQuestions.length - 1) {
      setCurrentQuestion(currentQuestion + 1);
      setSelectedAnswer(null);
      setShowResult(false);
    } else {
      setGameCompleted(true);
    }
  };

  const restartGame = () => {
    setCurrentQuestion(0);
    setScore(0);
    setSelectedAnswer(null);
    setShowResult(false);
    setGameCompleted(false);
    
    // Remélanger les questions pour une nouvelle partie
    const shuffleArray = (array) => {
      const shuffled = [...array];
      for (let i = shuffled.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [shuffled[i], shuffled[j]] = [shuffled[j], shuffled[i]];
      }
      return shuffled;
    };
    
    const shuffled = shuffleArray(allQuestions);
    setShuffledQuestions(shuffled.slice(0, Math.min(questionsToPlay, allQuestions.length)));
  };

  const getScoreMessage = () => {
    const percentage = (score / shuffledQuestions.length) * 100;
    if (percentage >= 80) return "🏆 Excellent ! Tu maîtrises parfaitement l'électricité !";
    if (percentage >= 60) return "👍 Bien joué ! Tu as de bonnes bases.";
    if (percentage >= 40) return "📚 Pas mal, mais il faut encore réviser un peu.";
    return "⚡ Il faut revoir tes cours d'électricité !";
  };

  if (gameCompleted) {
    return (
      <div className="max-w-2xl mx-auto p-6 bg-gradient-to-br from-blue-50 to-purple-50 rounded-xl shadow-lg">
        <div className="text-center">
          <Trophy className="w-16 h-16 text-yellow-500 mx-auto mb-4" />
          <h2 className="text-3xl font-bold text-gray-800 mb-4">Quiz Terminé !</h2>
          
          <div className="bg-white rounded-lg p-4 mb-6">
            <label className="block text-sm font-medium text-gray-700 mb-2">
              Nombre de questions pour la prochaine partie :
            </label>
            <select 
              value={questionsToPlay} 
              onChange={(e) => setQuestionsToPlay(Number(e.target.value))}
              className="border border-gray-300 rounded-lg px-3 py-2 text-center"
            >
              <option value={10}>10 questions</option>
              <option value={20}>20 questions</option>
              <option value={30}>30 questions</option>
              <option value={50}>50 questions</option>
              <option value={allQuestions.length}>Toutes les questions ({allQuestions.length})</option>
            </select>
          </div>

          <div className="bg-white rounded-lg p-6 mb-6">
            <p className="text-2xl font-bold text-blue-600 mb-2">
              Score : {score}/{shuffledQuestions.length}
            </p>
            <p className="text-lg text-gray-700">{getScoreMessage()}</p>
          </div>
          <button
            onClick={restartGame}
            className="bg-blue-500 hover:bg-blue-600 text-white px-6 py-3 rounded-lg font-semibold flex items-center gap-2 mx-auto transition-colors"
          >
            <RotateCcw className="w-5 h-5" />
            Rejouer
          </button>
        </div>
      </div>
    );
  }

  if (shuffledQuestions.length === 0) {
    return (
      <div className="max-w-2xl mx-auto p-6 bg-gradient-to-br from-blue-50 to-purple-50 rounded-xl shadow-lg">
        <div className="text-center">
          <Zap className="w-16 h-16 text-yellow-500 mx-auto mb-4" />
          <h1 className="text-3xl font-bold text-gray-800 mb-6">Trivial Pursuit Électricité</h1>
          <p className="text-lg text-gray-700 mb-6">
            Toutes les questions sont mélangées aléatoirement ! <br/>
            <span className="text-sm text-gray-500">({allQuestions.length} questions disponibles)</span>
          </p>
          
          <div className="bg-white rounded-lg p-6 mb-6">
            <label className="block text-sm font-medium text-gray-700 mb-4">
              Nombre de questions :
            </label>
            <div className="grid grid-cols-2 gap-3 mb-4">
              {[10, 20, 30, 50].map(num => (
                <button
                  key={num}
                  onClick={() => setQuestionsToPlay(num)}
                  className={`px-4 py-2 rounded-lg font-semibold transition-colors ${
                    questionsToPlay === num 
                      ? 'bg-blue-500 text-white' 
                      : 'bg-gray-100 hover:bg-gray-200 text-gray-700'
                  }`}
                >
                  {num} questions
                </button>
              ))}
            </div>
            <button
              onClick={() => setQuestionsToPlay(allQuestions.length)}
              className={`w-full px-4 py-2 rounded-lg font-semibold transition-colors ${
                questionsToPlay === allQuestions.length 
                  ? 'bg-blue-500 text-white' 
                  : 'bg-gray-100 hover:bg-gray-200 text-gray-700'
              }`}
            >
              Toutes les questions ({allQuestions.length})
            </button>
          </div>

          <button
            onClick={() => {
              // Fonction pour mélanger un tableau (algorithme Fisher-Yates)
              const shuffleArray = (array) => {
                const shuffled = [...array];
                for (let i = shuffled.length - 1; i > 0; i--) {
                  const j = Math.floor(Math.random() * (i + 1));
                  [shuffled[i], shuffled[j]] = [shuffled[j], shuffled[i]];
                }
                return shuffled;
              };
              
              const shuffled = shuffleArray(allQuestions);
              setShuffledQuestions(shuffled.slice(0, Math.min(questionsToPlay, allQuestions.length)));
            }}
            className="bg-blue-500 hover:bg-blue-600 text-white px-8 py-3 rounded-lg font-semibold flex items-center gap-2 mx-auto transition-colors"
          >
            <Shuffle className="w-5 h-5" />
            Commencer le Quiz
          </button>
        </div>
      </div>
    );
  }

  const currentQ = shuffledQuestions[currentQuestion];
  const categoryColor = categoryColors[currentQ.category];

  return (
    <div className="max-w-2xl mx-auto p-6 bg-gradient-to-br from-blue-50 to-purple-50 rounded-xl shadow-lg">
      {/* Header */}
      <div className="flex items-center justify-between mb-6">
        <div className="flex items-center gap-2">
          <Zap className="w-8 h-8 text-yellow-500" />
          <h1 className="text-2xl font-bold text-gray-800">Trivial Pursuit Électricité</h1>
        </div>
        <div className="text-right">
          <p className="text-sm text-gray-600">Question {currentQuestion + 1}/{shuffledQuestions.length}</p>
          <p className="text-lg font-semibold text-blue-600">Score: {score}</p>
        </div>
      </div>

      {/* Progress Bar */}
      <div className="w-full bg-gray-200 rounded-full h-2 mb-6">
        <div 
          className="bg-blue-500 h-2 rounded-full transition-all duration-300"
          style={{ width: `${((currentQuestion + 1) / shuffledQuestions.length) * 100}%` }}
        ></div>
      </div>

      {/* Category */}
      <div className="mb-4">
        <span className={`inline-block px-4 py-2 rounded-full text-white text-sm font-semibold ${categoryColor}`}>
          {currentQ.category}
        </span>
      </div>

      {/* Question */}
      <div className="bg-white rounded-lg p-6 mb-6 shadow-md">
        <h2 className="text-xl font-semibold text-gray-800 mb-6">{currentQ.question}</h2>
        
        {/* Answers */}
        <div className="space-y-3">
          {currentQ.answers.map((answer, index) => {
            let buttonClass = "w-full p-4 text-left border-2 rounded-lg transition-all duration-200 font-medium ";
            
            if (!showResult) {
              buttonClass += "border-gray-200 hover:border-blue-300 hover:bg-blue-50 cursor-pointer";
            } else {
              if (index === currentQ.correct) {
                buttonClass += "border-green-500 bg-green-100 text-green-800";
              } else if (index === selectedAnswer) {
                buttonClass += "border-red-500 bg-red-100 text-red-800";
              } else {
                buttonClass += "border-gray-200 bg-gray-50 text-gray-600";
              }
            }

            return (
              <button
                key={index}
                onClick={() => handleAnswerClick(index)}
                className={buttonClass}
                disabled={showResult}
              >
                <div className="flex items-center justify-between">
                  <span>{answer}</span>
                  {showResult && index === currentQ.correct && (
                    <CheckCircle className="w-5 h-5 text-green-600" />
                  )}
                  {showResult && index === selectedAnswer && index !== currentQ.correct && (
                    <XCircle className="w-5 h-5 text-red-600" />
                  )}
                </div>
              </button>
            );
          })}
        </div>

        {/* Explanation */}
        {showResult && (
          <div className="mt-6 p-4 bg-blue-50 border-l-4 border-blue-500 rounded">
            <p className="text-blue-800 font-medium">Explication :</p>
            <p className="text-blue-700 mt-1">{currentQ.explanation}</p>
          </div>
        )}
      </div>

      {/* Next Button */}
      {showResult && (
        <div className="text-center">
          <button
            onClick={nextQuestion}
            className="bg-blue-500 hover:bg-blue-600 text-white px-8 py-3 rounded-lg font-semibold transition-colors"
          >
            {currentQuestion < shuffledQuestions.length - 1 ? 'Question suivante' : 'Voir les résultats'}
          </button>
        </div>
      )}
    </div>
  );
};

export default TrivialPursuitElectricite;
