#! /home/galan/workspace/vlm/venv/bin/python
# -*- coding: utf-8 *-*

class LoadModule:
    """
    La classe LoadModule contient toutes les informations extraites du fichier
    des VLM File Manager.
    """

    def __init__(self):
        """
        Le constructeur de cette classe n'attend aucune valeur d'initialisation.
        """
        self.loadlib = ''
        """ le nom de la LOADLIB parent où est stocké le LOAD MODULE """
        self.module = ''
        """ le nom du LOAD MODULE """
        self.linked_date = ''
        """ la date à laquelle a été Linkédité le LOAD MODULE """
        self.linked_time = ''
        """ l'heure à laquelle a été Linkédité le LOAD MODULE """
        self.size = ''
        """ la taille totale, en octets, du LOAD MODULE """
        self.amode = ''
        """
        AMODE signifie Addressing Mode et est utilisé pour spécifier le mode
        d'adressage du module sous le contrôle du système d'exploitation z/OS.

        Ce paramètre peut prendre les valeurs 24, 31 ou 64 pour spécifier
        l'utilisation de 24 bits, 31 bits ou 64 bits pour les adresses.

        Ce mode d'adressage détermine la plage d'adresses que le programme
        peut utiliser pour stocker ses données et ses instructions.
        """
        self.rmode = ''
        """
        RMODE signifie Residency Mode et est utilisé pour spécifier le mode
        de résidence des instructions et des données pour un module dans la
        mémoire sous le contrôle du système d'exploitation z/OS.

        RMODE(24) : indique que le module doit être chargé dans la zone de
                    24 bits de la mémoire centrale. Ce mode limite la plage
                    d'adresses accessibles aux 16 Mo de la mémoire. Il est
                    utilisé pour les programmes plus petits qui ne nécessitent
                    pas beaucoup de mémoire.

        RMODE(31) : indique que le module doit être chargé dans la zone de
                    31 bits de la mémoire centrale. Ce mode permet d'accéder
                    à une plage d'adresses plus grande, allant jusqu'à 2 Go
                    de mémoire. Il est utilisé pour les programmes plus grands
                    qui nécessitent plus de mémoire.

        RMODE(ANY) : indique que le système doit charger le module dans
                    n'importe quelle zone de la mémoire, qu'elle soit de
                    24 bits ou de 31 bits. Ce mode permet de charger des
                    modules qui peuvent être placés dans n'importe quelle
                    zone de la mémoire et qui ne sont pas limités par une
                    plage d'adresses spécifique.

        RMODE(ANYLPA) : indique que le système doit charger le module dans
                    n'importe quelle zone de la mémoire à partir de l'adresse
                    0 (zéro). Ce mode permet de charger des modules qui peuvent
                    être placés dans n'importe quelle zone de la mémoire et qui
                    ne sont pas limités par une plage d'adresses spécifique,
                    mais ils ne peuvent pas être chargés à une adresse
                    supérieure à 16 Mo.
        """
        self.all_csect = []
        """
        Liste de dictionnaires contenant :
        - ['name'] le nom de la CSECT
        - ['size'] la taille du module de la CSECT
        - ['compiler'] le compilateur utilisé pour générer le module associé à la CSECT
        """
        self.compiler = ''
        """ le nom du compilateur ayant généré le LOAD MODULE """

    def __str__(self):
        """
        Affiche tous les attributs de la classe sur la sortie standard, stdout.
        Chaque attribut est séparé du précédent par un point-virgule.
        """
        output = ''
        for item in self.all_csect:
            output += (f"{self.loadlib};{self.module};"
                       f"{self.linked_date};"
                       f"{self.linked_time};"
                       f"{self.size};"
                       f"{self.amode};"
                       f"{self.rmode};"
                       f"{item['name']};"
                       f"{item['size']};"
                       f"{item['compiler']}")

        return output

    def reset(self):
        """
        Réinitialisez les attributs de classe à leurs valeurs par défaut.
        """
        self.__init__()


with open('vlm.txt', 'r', encoding='utf-8', errors='ignore') as file:

    count_module = 0       # nombre de LOAD Module détectés
    lm = LoadModule()      # instanciation d'un nouvel objet LoadModule
    # csect = {}             # nom, taille et compilateur de la csect détectée
    csect_table_start = '' # en-tête du tableau des csect est détecté
    csect_table_end = ''   # en-tête du tableau des csect est détecté

    for i, ligne in enumerate(file):
        # ignorer les 12 premières lignes du fichier des VLM
        if i < 2:
            continue

        # supprimer le caractère de contrôle ASA.
        #
        # Le caractère de contrôle ASA (American Standard Code for Information
        # Interchange Supplementary Control Character Set A) est un ensemble
        # de caractères de contrôle défini par IBM. Il fait partie du jeu de
        # caractères EBCDIC. Le caractère de contrôle ASA est souvent utilisé
        # pour représenter des actions spécifiques telles que le retour
        # chariot (CR), le saut de ligne (LF), l'effacement de l'écran (ECS),
        # l'effacement du champ (EBC), etc.
        if ligne[0] == '1' or ligne[0] == '0':
            ligne = ' ' + ligne[1:]

        # remplacer tous les tirets par un espace et supprimer tous
        # les espaces de début et de fin de la ligne
        ligne = ligne.replace("-", "").strip()

        # ignorer les lignes vides
        if not ligne:
            continue

        # ignorer les lignes de commandes FILE MANAGER
        if ligne.startswith('$$FILEM'):
            continue

        # ignorer les lignes qui débutent par: '1IBM File Manager for z/OS'
        if ligne.startswith('1IBM File Manager for z/OS'):
            continue

        # rupture sur LOAD MODULE 
        if ligne.startswith('Load Module Information'):
            if count_module > 0:
               print(lm)             # afficher le contenu du LOAD MODULE
            count_module += 1         # +1 LOAD MODULE traité
            csect_table_start = False # réinitialisation car nouveau LOAD MODULE 
            csect_table_end = False   # réinitialisation car nouveau LOAD MODULE 
            lm.reset()                # réinitialisation car nouveau LAOD MODULE
            continue

        # récupérer le nom de la LOADLIB parent où est stocké le LOAD MODULE
        if ligne.startswith('Load Library'):
            lm.loadlib = ligne.split()[2]
            continue

        # récupérer le nom du LOAD MODULE
        if ligne.startswith('Load Module'):
            mod = ligne.split()[2]
            lm.module = ligne.split()[2]
            continue

        # récupérer la date et heure de LINKEDIT du LOAD MODULE
        if ligne.startswith('Linked on'):
            lm.linked_date = ligne.split()[2]
            lm.linked_time = ligne.split()[4]
            continue

        # récupérer SIZE, AMODE et RMODE du LOAD MODULE
        #
        # dans cette ligne :
        #  EPA 0004E0 Size 00242D8 TTR 000004 SSI          AC 00 AM  31 RM ANY
        # SSI peut ne pas avoir de valeur. Donc il faut récupérer les valeurs
        # amode er rmode en partant de la fin de la chaîne
        if ligne.startswith('EPA '):
            lm.size = ligne.split()[3]
            lm.amode = ligne.split()[-3]
            lm.rmode = ligne.split()[-1]
            continue

        # ignorer les lignes qui débutent par: 'Name      Type'
        if ligne.startswith('Name      Type'):
            csect_table_start = True
            csect.clear()
            continue

        # ignorer les lignes qui débutent par :'FMNBA215'
        if ligne.startswith('FMNBA215'):
            csect_table_end = True
            continue

        # traitement des csect
        if (csect_table_start and not csect_table_end and len(ligne.split()) > 6):
            # récupérer le nom du compilateur ayant généré le LOAD MODULE
            mod = ligne.split()[0]
            if ligne.split()[0] == lm.module:
                csect['name'] = ligne.split()[0]
                csect['size'] = ligne.split()[3]
                csect['compiler'] = ligne[56:84]
                lm.all_csect.append(csect)
                continue

            # ignorer les CSECT de module compilé en C++ ou compilé en 'PL/X'
            if (ligne[56:84].startswith('C/C++') or ligne[56:84].startswith('PL/X')):
                continue

            # ignorer les CSCET débutant contenant les caractères '£' ou 'à' 
            if ('£' in ligne.split()[0] or 'à' in ligne.split()[0]
                and ligne.split()[1]):
                continue

            # récupérer le nom du compilateur ayant généré le LOAD MODULE
            # if ligne.split()[1] == 'SD': 
            #     csect.clear()
            #     csect['name'] = ligne.split()[0]
            #     csect['size'] = ligne.split()[3]
            #     csect['compiler'] = ligne[56:84]
            #     lm.all_csect.append(csect)
            #     continue
