Nous vous souhaitons la bienvenue au centre du code ouvert de L�emploi en 2030 de l�Institut Brookfield! Ce code cr�e, analyse et teste le mod�le qui a produit nos Pr�visions sur la croissance des professions au Canada (PCPC).  

Nous vous fournissons ici le code utilis� pour cr�er le mod�le, certaines des v�rifications de mod�le, et l�exercice d�analyse des �l�ments dont nous nous sommes servis pour d�terminer les caract�ristiques fondamentales, ainsi que les paires de caract�ristiques mentionn�es dans notre rapport. Ce code ne fournit pas le code R et les donn�es pour l�analyse d�mographique (largement en raison de la taille des tableaux personnalis�s de Statistique Canada), mais si vous avez la moindre question sur la m�thode utilis�e ou les m�canismes d�acc�s aux donn�es, veuillez communiquer avec Josh � jzachariah@ryerson.ca. Nous vous conseillons vivement de lire le rapport (et son annexe) si vous ne l�avez pas d�j� fait avant d�examiner le code. 

Anglais : *****Link
Fran�ais : ****** Link

Voici une description de chaque dossier et de son contenu. 

****raw_data****
Ce dossier renferme les donn�es recueillies dans le cadre de nos six ateliers nationaux, ainsi que le score d�importance de la comp�tence O*NET pour chaque code de la classification nationale des professions (CNP). Nous vous rappelons que nous avions demand� aux participants aux ateliers d��valuer chaque profession dans notre base d�apprentissage pour pr�dire si la part de l�emploi de ces professions allait augmenter, diminuer ou rester stable d�ici 2030.  

Les scores d�importance O*NET sont une mesure de 1 � 5 qui d�signe l�importance d�une comp�tence, d�une connaissance ou d�une aptitude pour l�ex�cution du travail associ� � une profession. Au total, il y a 120 comp�tences, caract�ristiques de connaissance et aptitudes. �tant donn� qu�O*NET est une base de donn�es am�ricaine, nous avons �galement inclus notre tableau de concordance entre les codes de profession am�ricains et canadiens (SOC et CNP respectivement). 

Enfin, les fichiers de donn�es d�O*NET et des ateliers renferment les descriptions des caract�ristiques et ainsi que celles de la CNP respectivement, en anglais et en fran�ais. 

****tables****
model input: Les deux tableaux utilis�s dans le mod�le. Le fichier noc_answers renferme les r�ponses compil�es de tous les participants � tous les ateliers, qui ont servi de base d�apprentissage. Le fichier noc_scores pr�sente les scores d�importance des caract�ristiques O*NET pour chaque profession. 

model output: Ce dossier contient nos projections issues des mod�les d�augmentation et de diminution pour chaque profession. 

testing output: Dossier renfermant les r�sultats pour notre script de test (voir ci-dessous). 

sffs output: Ces deux fichiers texte �num�rent les caract�ristiques retenues par notre processus de s�lection de caract�ristiques : Sequential Forward Floating Search.

feature analysis output: 
Le fichier 1run_non_conditional_influences pr�sente les influences de chaque caract�ristique non conditionnelle apr�s une ex�cution de l�analyse. � noter que puisqu�il s�agit d�une seule ex�cution, des caract�ristiques autres que les cinq caract�ristiques fondamentales sont susceptibles d�atteindre le seuil de 95 %. Or, les cinq caract�ristiques fondamentales sont les seules qui atteignent invariablement ce seuil, selon notre analyse de 10 ex�cutions du mod�le. 
sig_pairs �num�re toutes les importantes paires ordonn�es de caract�ristiques, avec leur part d�influence et d�occurrence.

****Final Model Scripts****
Ce dossier contient la version d�finitive de tous les scripts pertinents pour notre mod�le.

**model_construction.ipynb**
Le script le plus important! Il cr�e et ex�cute les mod�les d�augmentation et de diminution et produit les projections. 

**utils_rf.py**
Ensemble de fonctions utilis�es pour un certain nombre des scripts pour la for�t al�atoire. BON NOMBRE de ces fonctions sont utilis�es pour les scripts dans �?old files?� (vieux fichiers). Une description des fonctions les plus importantes est donn�e ci-dessous.

data_process(file,discrete)
Cette fonction traite les donn�es. Le drapeau discret permet � l�utilisateur de faire arrondir ou non les scores O*NET. Nous arrondissons les scores pour nos mod�les d�finitifs. 

init_params(model_type)
D�termine le param�tre pour le mod�le de for�t al�atoire. Le mod�le final utilise le param�tre cat. 

run_k_fold(x,y,params,index,binned,model_type)
Cette fonction ex�cute la m�thode group k-fold que nous avons utilis�e pour tester notre mod�le. .

param_search(x,y,model_type)
Nous avons utilis� cette fonction pour trouver les param�tres optimaux (� nouveau, en utilisant la m�thode group k fold). Nous avons utilis� diverses grilles de param�tres qui ne sont pas pr�sent�s dans le pr�sent dossier, ainsi qu�un processus it�ratif pour pr�ciser la zone d�limit�e par les param�tres. 

run_sfs(x,y,model_type,custom_score,increase_model)
Ex�cute la recherche de caract�ristiques SFFS. Le param�tre custom_score d�termine si nous utilisons ou non notre fonction personnalis�e d�erreur absolue moyenne (EAM) pour �valuer les ensembles de caract�ristiques (voir ci-dessous). 

custom_mae_increase(y_true,y_pred) and custom_mae_decrease(y_true,y_pred)
Notre mod�le apprend en fonction des donn�es au niveau du participant, mais effectue les tests en v�rifiant l�EAM pour la part pr�dite contre la part r�elle des experts qui donnent une r�ponse. Ces fonctions effectuent cette agr�gation et calculent le score.  

**testing**
Ces scripts ex�cutent les diverses m�thodes de test utilis�es dans l�annexe du rapport. Le script regional_models.ipynb teste la mesure dans laquelle le mod�le ayant appris d�un atelier est capable de pr�dire les r�ponses des autres. 

**sffs****
Ces deux scripts ex�cutent le mod�le SFFS 20 fois et inscrivent les r�sultats dans un fichier pickle. Si vous les ex�cutez, nous vous recommandons VIVEMENT de faire appel � un service d�infonuagique. L�ex�cution des scripts demande beaucoup de temps, mais ces derniers sont configur�s de fa�on � utiliser autant de fils que vous leur donnez, ce qui, en raison de la nature du processus, r�duit sensiblement le temps d�ex�cution. 

**Feature Analysis Files**
Le script dans ce dossier ex�cute la m�thode d�finitive utilis�e pour d�terminer l�influence d�une caract�ristique ou d�une paire de caract�ristiques. D�autres m�thodes et tentatives se trouvent dans old_files. Des d�tails sur le processus sont expos�s dans notre annexe. 

*find_trait_influences.ipynb*
Il s�agit du script utilis� pour notre exercice d�analyse des �l�ments. Il donne toutes les influences de caract�ristiques ainsi que les paires de caract�ristiques importantes. Voir notre annexe pour obtenir plus de d�tails. 

*basic feature analysis*
Ce script contient de nombreux tests pour d�couvrir une panoplie d�aspects que vous souhaiteriez conna�tre au sujet des caract�ristiques. 

****old files****
Ce dossier stocke un �ventail d�autres approches, tests et mod�les que nous avons essay�s. Nous vous invitons � l�explorer, mais gardez � l�esprit qu�ils ne fonctionnent pas n�cessairement tous. 

