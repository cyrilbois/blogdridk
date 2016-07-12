Title: Le dépistage prénatal non-invasif de la trisomie 21
Slug: dpni
Date: 2016-07-12 18:59:16
Category: biologie
Tags: bioinformatique, génétique
Author: Sacha Schutz
Status: Draft
SIDEBARIMAGE:../images/common/caryotype.jpeg

La trisomie 21 est un syndrome polymalformatif avec un retard mental, lié dans la majorité des cas à la présence d'un chromosome 21 surnuméraire. On a tous en mémoire l'image de l'acteur [Pascal Duquenne](https://fr.wikipedia.org/wiki/Pascal_Duquenne) atteint de ce syndrome et lauréat du prix d'interprétation au festival de cannes pour le film le *[Huitème jour](https://fr.wikipedia.org/wiki/Le_Huiti%C3%A8me_Jour_(film,_1996))*.     
En France, le [dépistage de la trisomie 21](http://www.has-sante.fr/portail/jcms/c_1165790/fr/strategies-de-depistage-de-la-trisomie-21-impact-et-mise-en-oeuvre) est proposé à toutes les femmes enceintes au 1er trimestre de grossesse. Ce dépistage consiste à calculer un score en [fonction de marqueurs sanguins](http://acces.ens-lyon.fr/acces/ressources/sante/epidemiologie/depistage_trisomie21/Points/points_marqueurs_seriques) (AFP, Papp-A, beta HCG) et des signe d'appel échographique comme [la clarté nuccale](https://fr.wikipedia.org/wiki/Clart%C3%A9_nucale). Si ce score dépasse un certain seuil, un diagnostic cytogénétique est proposé pour confirmer la trisomie. Il consiste à prélever des cellules fœtales par des techniques invasives comme l'[amniocentèse](https://fr.wikipedia.org/wiki/Amniocent%C3%A8se) et de dénombrer le nombre de chromosome sur un [caryotype](https://fr.wikipedia.org/wiki/Caryotype) comme illustré ci-dessous. 

<div class="figure">
    <img src="../images/post18/caryotype.jpg" />
    <div class="legend">Caryotype présentant 3 chromosomes 21 au lieu de 2</div>
</div>


Malheureusement ces gestes invasifs ne sont pas anodin. On estime entre [0.5% et 1%](http://www.agence-biomedecine.fr/annexes/bilan2013/donnees/diag-prenat/01-diag_prenat/synthese.htm) le risque de fausse couche lié à l'amniocentèse.  
Cela peu sembler faible, mais le dépistage actuel souffre d'une très mauvaise spécificité. C'est à dire que beaucoup de femme réponde positif au dépistage alors que leurs fœtus est indemne. Par conséquence beaucoup trop d'amniocentèse sont réalisé inutilement avec le risque de fausse couche qui en découle.    
Mais depuis l’avènement récent du séquençage haut débit, un nouveau teste de dépistage beaucoup plus puissant en terme de sensibilité et de spécificité voit le jour. Il s'agit du DPNI pour Dépistage Prénatal Non Invasif. ( Vous lirez souvent *diagnostic* , mais il s'agit pour l'heure de dépistage). Ce nouveau teste consiste à quantifier sur une simple prise de sang un excès d'ADN fœtal circulant. 

## L'ADN fœtal
Il y a des fragments d'ADN double brin qui circule librement dans votre sang. En général ce sont les vôtres sauf si vous faite des expériences tordues dans votre laboratoire ou ... si vous êtes enceintes. En effet chez les femmes enceintes environs 10% de ces fragments proviennent du fœtus. Plus précisément ces fragments proviennent de la lyse des cellule trophoblastiques, un composant du placenta ayant la même origine embryologique que le fœtus.   
L'idée derrière le DPNI c'est quantifier l’excès d'ADN provenant du chromosome 21 à l'aide des nouvelles technologies de séquençage haut débit.    
La figure ci-dessous illustre la quantification d'ADN circulant chez une mère sans et avec fœtus trisomique. En mesurant une différence significative, il est possible de conclure à un excès de matérielle nucléaire provenant du chromosome 21.

<div class="figure">
    <img src="../images/post18/chromosomes.png" />
    <div class="legend">On considère que 90% de l'ADN circulant provient de la mère et 10% du foetus. L'excès du chromosome 21 chez le foetus est caractérisé par une différence significatif entre 2.1 et 2.0</div>
</div>

## Le Séquençage de nouvelle génération 
Le NGS (Next Generation Sequencing) est une technologie récente permettant le séquençage de l'ADN de façon très rapide grâce à un haut niveau de parallélisation. Sans entrer dans les détails ([super site ici](https://www.abmgood.com/marketing/knowledge_base/next_generation_sequencing_introduction.php#similarities)), les fragments d'ADN provenant du sang maternel,qu'on appellera *reads* à présent, sont séquencées puis alignées sur le génome de référence via des algorithmes de  bioinformatiques. 
On obtiens alors un fichier contenant la liste des reads associés à leurs positions sur le génome. C'est à dire qu'à chaque fragment séquencé, son chromosome lui est associé.   
La figure ci-dessous résume les étapes du séquençage ainsi que les différents formats de fichier. 

<div class="figure">
    <img src="../images/post18/ngs.png" />
    <div class="legend">Les reads sont séquencés et sauvegardés dans un fichier fastQ. Les reads sont ensuite alignés sur le génome de référence hg19. En vert les reads provenant du foetus. En violet les reads maternelles</div>
</div>

## Quantification et teste statistique

Une trisomie 21 se caractérise par un excès de reads s’alignant sur le chromosome 21. Pour mesurer cette excès ils nous faut des valeurs de références obtenues chez des femmes enceintes témoins dont le fœtus est sains. 
Avec un nombre suffisant de témoin, la moyenne et l'écart-type du nombre de reads par chromosome est calculés.      
Pour savoir si une patiente présente trop de reads, il suffit de recherche une différence significative à l'aide d'un Z-score en comparant les données de la patiente et les valeurs de référence.    
Le logiciel [RapidR](http://www.ncbi.nlm.nih.gov/pubmed/24990604) crée par le NHS utilise cette approche. Un exemple de résultats est présenté dans la figure suivante. 

<div class="figure">
    <img src="../images/post18/rapidR.png" />
    <div class="legend">En ordonnée le Z-score, en abscisse les chromosomes. Sur ce graphique, la patiente présente un excès significatif en read s’alignant sur le chromosome 21</div>
</div>

Un autre logiciel, [Wisecondor](http://www.ncbi.nlm.nih.gov/pubmed/?term=wisecondor) approche différent le problème et propose d'utiliser les autres chromosomes comme références au sein du même échantillon. C'est une approche plus complexe, mais peut se résumer ainsi.   
Tout d'abord, le génome est segmenté en région de 10 kilobase appelé *bin*. Le nombre de reads est comptabilisé par bin au lieu d'être comptabilisé par chromosome comme précédemment.       
A partir des mesures chez les témoins sains, les bins du chromosome 21 sont associés aux autres bins du génome lorsqu'il contienne plus ou moins le même nombre de reads. La figure suivante est une représentation de ces associations.  
<div class="figure">
    <img src="../images/post18/wisecondor.png" />
    <div class="legend">En ordonnée le Z-score, en absisse les chromosomes. Sur ce graphique, la patiente présente un excès significatif en read s'allignant sur le chromosome 21</div>
</div>

Ce logiciel permet donc de mesurer l'excès de reads mappant le chromosome 21 en le comparant aux autres chromosomes. Cette technique à l'avantage de se passer des biais de mesures, car le séquençage de la référence et du patient sont réalisé au en même temps. 

## Stratégie de dépistage 
En Novembre 2015, la Haute autorité de santé [a évalué positivement ce nouveau teste de dépistage](http://www.has-sante.fr/portail/jcms/c_2573090/fr/trisomie-21-de-nouveaux-tests-appellent-la-revision-des-modalites-de-depistage-actuelles). La sensibilité et la spécificité avoisine les 100%. 
Le risque de faux négatif est lié à l'absence d'ADN fœtal si le prélèvement est réalisé trop tôt. Un risque de faux positif est également possible dans les cas de [mosaïcisme fœtal](https://fr.wikipedia.org/wiki/Mosa%C3%AFque_(g%C3%A9n%C3%A9tique)).   
Aujourd'hui toutes les femmes enceintes, quel que soit leurs âges, sont informées de la possibilité de recourir à ce teste.    
La stratégie actuel est de proposer le dépistage standard. Si celui ci revient positif, au lieu de proposer directement un geste invasif, le DPNI est proposé. Si celui ci revient positif alors le geste invasif et le diagnostic cytogénétique est réalisé. 
A l'heure actuelle les laboratoire [Cerba](http://www.lab-cerba.com/) et [Biomnis](http://www.dpni-biomnis.com/) propose le DPNI. Mais depuis janvier 2016 certain centre hospitalier propose ce teste qui n'est malheureusement pas encore remboursé et avoisine les 800€.  

## Référence
* [Stratégies de dépistage de la trisomie 21](http://www.has-sante.fr/portail/jcms/c_1165790/fr/strategies-de-depistage-de-la-trisomie-21-impact-et-mise-en-oeuvre)
* [Les performances des tests ADN libre circulant ](https://www.has-sante.fr/portail/upload/.../recommandation_trisomie_21.pdf)
* [La valeur d'un dépistage préatal non invasif ( Illumina)](http://www.illumina.com/content/dam/.../intl-presentation-0042-vc-french.pdf)
* [RapidR](http://www.ncbi.nlm.nih.gov/pubmed/24990604)
* [Wisecondor](http://www.ncbi.nlm.nih.gov/pubmed/24170809)

## Remerciement 

