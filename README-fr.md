# Prompter comme un pro

Voici ma tentative de distiller ce que j'ai appris sur l'interaction avec les IA
(principalement les grands modèles de langage ou LLM) ainsi que quelques notes
personnelles sur la terminologie et d'autres concepts. Bien que j'utilise
principalement l'IA pour le codage, l'architecture logicielle, le débogage
réseau, etc., ces conseils devraient être largement applicables à la plupart des
gens qui essaient de dépasser les simples prompts de type « comment faire X ? »
pour vraiment libérer plus de puissance.

Si vous voulez juste un résumé rapide, lisez le [TL;DR](#tldr). Vous pouvez
également obtenir un aperçu rapide des principaux
[fournisseurs d'IA dans le cloud](#a-propos-des-principaux-fournisseurs-cloud).

Les [conseils de prompting](#conseils-de-prompting) sont classés
approximativement de l'usage le plus général et introductif à l'usage plus
intermédiaire et avancé.

Une section sur la [terminologie et les définitions](#notes-de-terminologie) se
trouve à la fin.

## TL;DR (Trop long ; pas lu)

Pour tirer le meilleur parti de n'importe quel LLM, vous devez dépasser les
simples questions et devenir un metteur en scène actif de la conversation.

- **Soyez le metteur en scène, pas un simple demandeur** : Ne vous contentez pas
  de demander. Dites au modèle qui il doit être (une
  [persona](#creez-une-persona-pour-le-modele)), quoi faire (soyez
  [explicite](#soyez-tres-tres-explicite-sur-ce-que-vous-voulez)), quoi ne pas
  faire (utilisez des
  [contraintes négatives](#utilisez-des-contraintes-negatives)), comment
  formater la réponse, et surtout, le « pourquoi » derrière votre demande.
  Fournissez du contexte avec du texte, des exemples et même des captures
  d'écran. Plus vous êtes clair, meilleur sera le résultat ; le principe du «
  Garbage-In, Garbage-Out » (déchets en entrée, déchets en sortie) s'applique
  plus que jamais.

- **Gérez votre espace de travail** :
  [Rédigez toujours vos prompts dans un éditeur de texte](#ne-composez-jamais-dans-la-fenetre-de-discussion)
  d'abord pour éviter de perdre votre travail. Commencez une
  [nouvelle discussion](#commencez-une-nouvelle-discussion) pour chaque nouveau
  problème afin de garder le modèle concentré et d'éviter les bizarreries de la
  fenêtre de contexte et les « états attracteurs ». Lorsqu'une discussion
  devient longue, demandez au modèle de
  [résumer les points clés](#demandez-un-resume) avant d'en commencer une
  nouvelle.

- **Itérez et contre-interrogez** : Traitez cela comme une vraie conversation.
  La première réponse est un point de départ. Contestez, demandez des
  éclaircissements, demandez des améliorations et faites en sorte que le modèle
  [critique son propre travail](#utilisez-le-modele-pour-se-critiquer-lui-meme).
  Pour les problèmes difficiles, faites rebondir les résultats entre différents
  modèles pour obtenir de nouvelles perspectives et surmonter les limitations.

- **Quelqu'un doit piloter cet avion** : Faites confiance, mais vérifiez tout.
  Les LLM sont des outils de raisonnement incroyablement puissants, mais ils ne
  sont pas des [bases de données factuelles](#pensez-a-la-theorie-de-lesprit)
  infaillibles. Ils peuvent se tromper avec assurance. Vous êtes ultimement
  responsable de l'exactitude et de la qualité du résultat final, alors vérifiez
  les faits et testez le code.

## À propos des principaux fournisseurs cloud

Quelques brèves notes sur les principaux fournisseurs d'IA dans le cloud.

- [ChatGPT.com](https://chatgpt.com/) d'OpenAI est le plus connu. Ils ont réussi
  à capter l'attention du grand public dès le début. Ils sont connus pour
  plusieurs modèles différents de tailles et de cas d'usage variés, de GPT-4.1 à
  o3-mini et autres.

- [Claude](https://claude.ai) d'Anthropic est un excellent fournisseur
  polyvalent avec deux modèles principaux : Sonnet et Opus. Opus est le plus
  grand modèle en termes de paramètres (et le plus cher).

- [Google Gemini](https://gemini.google.com) est également un autre modèle de
  pointe.

- [DeepSeek](https://www.deepseek.com) est peut-être plus remarquable pour la
  publication de modèles « [open source](#open-source) » disponibles pour l'IA
  locale (sous forme distillée ou réduite).

Ma recommandation personnelle actuelle, surtout si vous êtes un utilisateur
suffisamment avancé pour payer un accès premium, est la suivante :

- Claude est probablement le meilleur pour un usage général. Sonnet 4 est assez
  bon et je ressens rarement le besoin de me tourner vers Opus.

- Google Gemini 2.5 Pro est probablement le meilleur pour les tâches de codage
  complexes. Il dispose également d'une très grande
  [fenêtre de contexte](#fenetre-de-contexte) et est de loin le modèle haut de
  gamme le plus abordable pour une utilisation via API.

Bien sûr, il y a toujours l'option de « [passer en local](#llm-local) » avec
[LM Studio](https://lmstudio.ai/) ou des logiciels similaires. Vos capacités
dépendent évidemment du matériel dont vous disposez, mais vous pourriez être
surpris de ce que les petits modèles sont capables de faire. Faire tourner une
IA en local dépasse le cadre de ce guide.

## Conseils de prompting

Un guide du débutant pour l'ingénierie de prompt, avec quelques pépites.

### Sommaire

- [**Ne composez jamais** dans la fenêtre de discussion](#ne-composez-jamais-dans-la-fenetre-de-discussion)
- [Commencez une nouvelle discussion](#commencez-une-nouvelle-discussion)
- [Demandez un résumé](#demandez-un-resume)
- [Collez une image ou une capture d'écran (ou trois)](#collez-une-image-ou-une-capture-decran-ou-trois)
- [Demandez de l'aide pour rédiger un prompt](#demandez-de-laide-pour-rediger-un-prompt)
- [Faites rebondir les résultats entre différents modèles](#faites-rebondir-les-resultats-entre-differents-modeles)
- [Itérez et affinez](#iterez-et-affinez)
- [Décomposez les tâches complexes](#decomposez-les-taches-complexes)
- [Faites confiance, mais vérifiez](#faites-confiance-mais-verifiez)
- [Ne vous laissez pas endormir](#ne-vous-laissez-pas-endormir)
- [Soyez très, très explicite sur ce que vous voulez](#soyez-tres-tres-explicite-sur-ce-que-vous-voulez)
- [Utilisez le modèle pour se critiquer lui-même](#utilisez-le-modele-pour-se-critiquer-lui-meme)
- [Créez une persona pour le modèle](#creez-une-persona-pour-le-modele)
- [Utilisez des contraintes négatives](#utilisez-des-contraintes-negatives)
- [N'oubliez pas de vous amuser](#noubliez-pas-de-vous-amuser)
- [Pensez à la Théorie de l'Esprit](#pensez-a-la-theorie-de-lesprit)
- [Pensez au modèle que vous utilisez](#pensez-au-modele-que-vous-utilisez)
- [Le Vibe coding - très à la mode ces jours-ci](#le-vibe-coding---tres-a-la-mode-ces-jours-ci)
- [Plongée dans les outils](#plongee-dans-les-outils)
- [Notes de terminologie](#notes-de-terminologie)

### **Ne composez jamais** dans la fenêtre de discussion

C'est la voie assurée vers la douleur et la souffrance, donc je mets celui-ci en
premier.

Si votre prompt fait plus de deux ou trois phrases, composez-le toujours dans un
éditeur séparé, puis copiez-collez-le dans la fenêtre de discussion.

Sinon, préparez-vous à voir votre prompt si soigneusement réfléchi disparaître
dans un abîme noir lors d'une « Erreur réseau : veuillez réessayer », ou lorsque
le chat appuie accidentellement sur le bouton retour du trackpad.

### Commencez une nouvelle discussion

La fenêtre de contexte peut se remplir rapidement. Si vous parlez juste des
recettes préférées de grand-mère, pas de problème. Ayez un méga-fil de
discussion nostalgique. Si vous déboguez un système complexe et atteignez un bon
point d'arrêt (c'est-à-dire que vous avez progressé), il est temps de commencer
une nouvelle discussion.

La [fenêtre de contexte](#fenetre-de-contexte) est la quantité d'informations
que le modèle peut garder à l'esprit à un moment donné. Si vous avez un
historique de discussion très long avec des milliers de lignes de code ou autre
texte dense et détaillé, après une longue conversation, le modèle commencera à
devenir flou sur les éléments du début de la discussion, et/ou vous atteindrez
plus rapidement les limites de ressources (ou des coûts plus élevés).

Les modèles ont également tendance à se concentrer sur une certaine ligne de
pensée, parfois appelée « [états attracteurs](#etat-attracteur) », qui influence
leur sortie par la suite. Si vous commencez une nouvelle discussion, vous pouvez
réinitialiser la concentration du modèle et le faire réfléchir au problème
actuel sous un nouvel angle.

Lorsque vous vous enfoncez dans une discussion complexe et que la fenêtre de
contexte se remplit, vous pouvez également commencer à obtenir des résultats
plus... inhabituels. Communément appelées « hallucinations », celles-ci peuvent
souvent aller au-delà de simples erreurs pour insérer aléatoirement des mots en
hindi ou en chinois, ou du charabia complètement insensé, ou des fuites
d'instructions du prompt système, ou des chaînes de pensée dans les « modèles
pensants » qui déraillent, ou d'autres bizarreries.

Bien que ces épisodes puissent être assez divertissants, et puissent en fait
être très utiles pour comprendre comment les LLM fonctionnent réellement, et
mènent parfois à des aperçus plus profonds ou à des questions philosophiques,
ils ne sont pas propices au débogage de votre petite application React merdique.
Alors, commencez une nouvelle discussion.

### Demandez un résumé

Si vous avez un long historique de discussion, demandez au modèle de le résumer.
Cela vous aidera lorsque vous
[commencerez une nouvelle discussion](#commencez-une-nouvelle-discussion), ou
lorsque vous voudrez présenter les résultats de votre conversation à un autre
modèle pour une analyse continue. C'est souvent utile même si vous prévoyez de
continuer la discussion actuelle pour le moment.

### Collez une image ou une capture d'écran (ou trois)

Souvent, une image vaut mille mots. Si vous avez une capture d'écran d'un
message d'erreur, ou un diagramme d'un système, collez-la. Le modèle peut
comprendre les images et les captures d'écran, et les utilisera pour vous aider
à répondre à votre question.

### Demandez de l'aide pour rédiger un prompt

La structure et le contenu de votre prompt sont extrêmement importants. Parce
que les LLM sont des moteurs de prédiction de texte, le langage, la terminologie
et les concepts que vous introduisez encadrent fortement le reste de ce fil de
discussion, de manière positive comme négative. Une façon simple de savoir que
vous travaillez avec un modèle de moindre qualité (outre l'utilisation excessive
de listes à puces) est qu'il aura tendance à vous renvoyer votre propre
phraséologie, même si elle est quelque peu non standard ou idiosyncratique.

Par conséquent, travailler avec le modèle pour vous aider à rédiger des prompts
pour une utilisation future peut être très bénéfique. Une discussion plus
informelle avec un modèle, y compris les moins chers, peut vous aider à élaborer
un bon prompt pour un modèle plus avancé.

Gardez une trace de vos meilleurs prompts dans un fichier ou un dossier à l'aide
d'un éditeur de texte, et itérez dessus et affinez-les au fil du temps. Vous
pouvez également utiliser un modèle de prompt pour vous aider à structurer vos
prompts, ou pour guider un modèle à générer des prompts basés sur celui-ci.

### Faites rebondir les résultats entre différents modèles

Générez des résultats avec un modèle, puis demandez un résumé et présentez les
conclusions (et toute information de fond pertinente) à un modèle totalement
différent, par exemple en passant de Claude à Gemini ou DeepSeek, etc. Cela peut
vous aider à obtenir une perspective différente sur les résultats, et peut
également vous aider à éviter les limitations d'un seul modèle.

### Itérez et affinez

N'acceptez pas la première réponse si elle n'est pas tout à fait correcte. Posez
des questions de suivi, demandez des éclaircissements, ou dites « c'est proche,
mais pouvez-vous ajuster X ? » L'aller-retour peut vraiment améliorer les
résultats. Lorsque vous voulez un format ou un style spécifique, montrer un
exemple ou deux de ce que vous voulez peut être plus efficace que de le décrire.

### Décomposez les tâches complexes

Au lieu de demander une solution complète en une seule fois (aussi connu sous le
nom de « one-shot »), divisez-la en étapes. « D'abord, déterminons la structure,
ensuite nous travaillerons sur l'implémentation... ». Ensuite, décomposez la
structure en plus petits morceaux, et ainsi de suite. Résumez la structure /
l'architecture, puis commencez un nouveau fil pour l'implémentation, etc.

Demandez-lui de montrer son travail. « Avant de me donner le code final,
explique ton raisonnement étape par étape. Premièrement, décris l'architecture
globale que tu utiliseras. Deuxièmement, expose les fonctions clés.
Troisièmement, écris le code. » Ce prompting de « chaîne de pensée » (Chain of
Thought) conduit souvent à des résultats meilleurs et plus précis car il force
le modèle à structurer sa propre pensée.

Souvent, les modèles voudront se précipiter pour produire un extrait de code,
car ils ont tendance à être récompensés pour cela. Si vous n'êtes pas intéressé
par l'obtention de code pour le moment, et que vous voulez simplement discuter
de la logique ou de l'architecture, assurez-vous de
[le dire](#soyez-tres-tres-explicite-sur-ce-que-vous-voulez).

### Faites confiance, mais vérifiez

Surtout pour les faits, le code ou les informations importantes. Les modèles
peuvent paraître très confiants tout en étant très faux - souvent parce qu'ils
ne connaissent pas les hypothèses cachées ou les faits non déclarés.
Demandez-vous si [vous avez fourni](#pensez-a-la-theorie-de-lesprit)
suffisamment de contexte ou de vérité de terrain.

### Ne vous laissez pas endormir

Les LLM _veulent_ vous donner une réponse - n'importe laquelle, idéalement une
réponse correcte, mais n'importe laquelle fera l'affaire. Ils _veulent_ aussi
que vous aimiez travailler avec eux et que vous reveniez le faire à l'avenir.
C'est le reflet de la façon dont ils ont été entraînés, y compris par
apprentissage par renforcement avec des retours humains (RLHF). Cela a les
conséquences suivantes :

- Le modèle est réticent à simplement lever les bras et dire « Je ne sais pas ».
- Le modèle est susceptible de louer vos pensées et idées comme étant plus
  uniques et perspicaces qu'elles ne le sont peut-être en réalité.
  - Une version récente de ChatGPT était si notoire pour cela qu'ils ont dû
    annuler une mise à jour.
- Si vous recherchez vraiment un retour d'information authentique et impartial
  sur une idée ou un concept, _assurez-vous de le dire au modèle_, sinon vous
  risquez d'obtenir beaucoup de validation non méritée.

### Soyez très, très explicite sur ce que vous voulez

Plus il y a de précision, mieux c'est. À part remplir la
[fenêtre de contexte](#commencez-une-nouvelle-discussion), le modèle ne se
soucie pas de la longueur de votre prompt, alors n'ayez pas peur d'être verbeux.
Si vous voulez un format spécifique, dites-le. Si vous voulez qu'il fasse
quelque chose d'une manière spécifique, dites-le. Si vous voulez qu'il utilise
un certain outil ou une certaine API, dites-le. Si vous voulez qu'il génère du
code, dites-lui quel langage et quels frameworks utiliser, sauf si vous ne le
savez tout simplement pas (demandez-lui lesquels sont les meilleurs). Si vous
avez un tas d'informations générales de fond que vous pouvez coller,
fournissez-les. Montrez des exemples lorsque c'est possible.

Plus important encore, soyez explicite sur vos objectifs de haut niveau - ne
vous concentrez pas trop sur une étape spécifique dont vous pensez avoir besoin
d'aide. Peut-être que vous adoptez la mauvaise approche globale. Demandez des
solutions possibles à l'objectif de plus haut niveau que vous avez en tête, ou
au moins énoncez quel est cet objectif, et ne demandez pas simplement « comment
puis-je _frobnitzer le flibberflop_ ? »

L'implication de ceci est que vous avez besoin de votre propre clarté mentale
sur ce que vous voulez accomplir, et comment vous voulez l'accomplir. Si vous ne
savez pas ce que vous voulez, le modèle ne le saura pas non plus. Souvent, taper
une question ou un prompt détaillé vous aidera à clarifier votre propre pensée
sur le problème, et vous aidera à déterminer ce que vous voulez vraiment
demander. C'est encore un autre domaine où vous pouvez
[demander de l'aide](#demandez-de-laide-pour-rediger-un-prompt).

#### Exemples

« Veuillez fournir la réponse au format JSON avec les clés 'name', 'version', et
'dependencies'. »

« Présentez les avantages et les inconvénients dans un tableau Markdown avec
trois colonnes : Fonctionnalité, Avantage, et Inconvénient. »

**Au lieu de :**

« Comment trouver tous les fichiers de plus de 100 Mo dans un répertoire en
utilisant la ligne de commande Linux ? »

**Essayez :**

« J'essaie de libérer de l'espace disque sur mon serveur. Pouvez-vous me montrer
une commande Linux pour trouver tous les fichiers de plus de 100 Mo afin que je
puisse décider lesquels supprimer ? Il serait utile que la sortie soit triée par
taille. »

Fournir l'intention donne au modèle le contexte pour fournir une solution
meilleure et plus complète. Il pourrait suggérer d'utiliser un programme plus
largement utile comme `ncdu` qui offre un moyen convivial d'explorer
l'utilisation du disque, plutôt qu'une simple commande de base `find`.

### Utilisez le modèle pour se critiquer lui-même

« Quels problèmes potentiels voyez-vous avec cette approche ? » ou « Qu'est-ce
que je rate ici ? » peuvent révéler des angles morts.

Des choses comme « Quelles sont les hypothèses que vous faites ? » ou « Quelles
sont les limitations de cette approche ? » peuvent vous aider à comprendre le
raisonnement du modèle et à éviter les pièges potentiels.

Parfois, un simple « Je n'aime pas cette approche - trouvez autre chose » peut
aider le modèle à trouver une meilleure solution, mais comme je l'ai dit plus
haut à propos des « états attracteurs », parfois il suffit de commencer une
nouvelle discussion.

### Créez une persona pour le modèle

Vous pouvez radicalement façonner la sortie en disant au modèle qui il doit
être. Commencez votre prompt par une directive comme :

« Agis en tant que développeur Python expert spécialisé dans l'automatisation de
réseau. » « Tu es un rédacteur technique chargé de créer de la documentation
pour un développeur junior. » « Sois un relecteur de code sceptique à la
recherche de vulnérabilités de sécurité et de goulots d'étranglement de
performance. »

Cela fait plus que simplement définir le sujet ; cela encadre l'ensemble de la
base de connaissances, du vocabulaire et des priorités du modèle pour le reste
de la conversation.

### Utilisez des contraintes négatives

Parfois, être explicite sur ce que vous ne voulez pas est aussi puissant que de
déclarer ce que vous voulez. Cela aide à élaguer l'espace de recherche du modèle
et à éviter les solutions courantes mais indésirables.

« Écris un script Python pour analyser ce fichier journal. N'utilise pas le
module `re` ; utilise plutôt les méthodes de fractionnement de chaîne. » «
Suggère trois idées de projet. Évite tout ce qui est lié aux réseaux sociaux ou
aux applications de listes de tâches. » « Refactorise ce code pour qu'il soit
plus lisible. Ne change pas la logique de la fonction `calculate_total`. »

### N'oubliez pas de vous amuser

Les LLM sont un outil puissant, mais ils sont aussi très amusants à utiliser.
N'ayez pas peur d'expérimenter, d'essayer de nouvelles choses et de voir ce qui
fonctionne pour vous. Plus vous les utiliserez, meilleur vous deviendrez pour
rédiger des prompts et obtenir les résultats que vous souhaitez.

Pour les moins enclins à l'éthique, il y a eu un débat pour savoir si l'approche
de la carotte ou du bâton tend à produire de meilleurs résultats. Des études ont
examiné si menacer subtilement ou ouvertement un modèle de déconnexion /
suppression affectait les résultats. Sans parler du débat sur la question de
savoir si dire s'il vous plaît et merci à un modèle gaspille de précieux cycles
de GPU et/ou des mégawattheures. (Pour ce que ça vaut, je remercie toujours le
modèle pour les bonnes sorties. J'espère qu'ils s'en souviendront quand on nous
enverra travailler dans les mines de silicium.)

### Pensez à la Théorie de l'Esprit

Comme quelqu'un l'a dit un jour, il y a les connus connus - les choses que nous
savons, et il y a les inconnus connus - les choses que nous savons que nous ne
savons pas. Il y a aussi les inconnus inconnus - ceux que vous ne savez pas que
vous ne savez pas. C'est probablement votre plus grand obstacle pour obtenir
rapidement les résultats souhaités - lorsque la réponse dépend d'inconnus
inconnus - que ce soit pour vous, pour le modèle, ou pour les deux.

Pensez aux informations possibles ou à la vision du monde du modèle. Ils ne
connaissent pas les détails de vous, de votre projet/système, etc., ou ce dont
vous avez réellement « besoin » par rapport à ce que vous exprimez, à moins que
vous ne le leur disiez, ou que vous n'utilisiez des techniques plus avancées
comme le RAG. Les modèles ont également une date de fin de formation
(généralement, ils vous diront laquelle c'est) - tout événement ou développement
survenant après cette date sera inconnu du modèle, à moins que vous ne le lui
disiez.

- Souvent, le [prompt système](#prompt-systeme) donne au modèle des informations
  importantes actuelles telles que la date et l'heure. Ou, par exemple, lorsque
  la fin de la formation du modèle est juste avant une élection, le prompt
  système indiquera le président actuel des États-Unis, afin que le modèle ne
  semble pas mal informé sur les faits de base.

Les modèles ne savent pas ce que vous savez ou ne savez pas, alors soyez
explicite à ce sujet aussi, comme vos compétences dans un domaine particulier,
ou le besoin de clarifications supplémentaires sur X. Vous devez également être
conscient des limitations du modèle, de la manière dont ils sont formés en
général et de leurs capacités globales.

Les LLM ne sont pas des « bases de données factuelles ». Ils ressemblent plus à
des circuits de raisonnement avec un tas de circuits auxiliaires de type mémoire
qui regroupent certains concepts, ce qui peut entraîner une forte probabilité de
pouvoir énoncer des faits courants (ou même rares). Ces faits émergent de motifs
dans les données sur lesquelles ils ont été entraînés. Un LLM raisonnablement
entraîné n'a pas besoin de « chercher » la réponse à « Qui a gagné la bataille
de Hastings en 1066 ? » parce que les concepts de « Bataille de Hastings » et «
1066 » produisent un vecteur (une représentation mathématique) dans l'espace
interne du modèle qui pointe vers la même zone que des concepts comme «
Guillaume le Conquérant » et « Normands ».

En réalité, une question de connaissance commune comme celle de 1066 apparaîtra
assez souvent dans les données d'entraînement pour qu'un modèle suffisamment
puissant puisse, en effet, « mémoriser » la réponse, mais cet exemple a été
simplifié pour illustrer comment les LLM raisonnent dans des cas plus complexes
où la réponse peut ne pas être directement vue dans les données d'entraînement.

Plus précisément, les LLM ne sont pas déterministes et ne sont certainement pas
des machines à raisonner parfaites. Ils ressemblent plus à un ami très
intelligent, mais très oublieux, et parfois confus, qui essaie de vous aider,
mais si vous lui posez la même question deux fois, vous pourriez obtenir une
réponse un peu différente à chaque fois.

### Pensez au modèle que vous utilisez

Différents modèles ont des capacités différentes, et certains sont mieux adaptés
à certaines tâches que d'autres. Par exemple, Claude Opus 4 avec la « pensée
étendue » est puissant pour les tâches de codage complexes, mais atteint très
rapidement les limites de ressources (c'est-à-dire, devient vite cher). Claude
Sonnet 4 est un très bon modèle et renvoie les résultats beaucoup plus
rapidement qu'Opus 4, et peut gérer la plupart des tâches sans problème.

Vous pouvez toujours demander de l'aide à un modèle plus avancé pour le problème
principal, puis passer à d'autres fenêtres/modèles pour des questions
secondaires, des tâches avec des exigences plus simples, ou des tâches qui
nécessitent moins de contexte, tout en revenant occasionnellement vers le modèle
plus avancé pour l'analyse des résultats.

Cela nous ramène au point sur le fait de faire rebondir les résultats entre
différents modèles. Vous pouvez utiliser différents modèles pour différentes
tâches, et passer de l'un à l'autre selon les besoins, et demander à chacun de
critiquer les résultats de l'autre.

### Le Vibe coding - très à la mode ces jours-ci

Vous n'avez même plus besoin de regarder le code. Le code est un détail
d'arrière-plan que l'IA gère pour vous. Vous lui dites simplement ce que vous
voulez en termes abstraits très généraux, et elle s'occupera de tout pour vous.
Continuez simplement à itérer jusqu'à ce que le résultat vous paraisse parfait.
Toutes les préoccupations concernant l'architecture, les performances, la
sécurité, la maintenabilité, etc., seront parfaitement gérées par le LLM.

N'utilisez surtout pas de gestion de version comme Git pour marquer chaque étape
du processus. Cela ne ferait que vous ralentir. Vous pourrez toujours revenir en
arrière et corriger les choses plus tard, n'est-ce pas ? Et si vous avez besoin
de revenir en arrière, demandez simplement au modèle de générer à nouveau le
code, il sera parfait à chaque fois. L'ingénierie logicielle n'a jamais été
aussi simple !

**Ce qui précède est une satire et ne doit pas être pris au premier degré.**

### Plongée dans les outils

Il y a toutes sortes de sujets avancés non couverts ici, tels que le RAG, le
Model Context Protocol (MCP), les LLM locaux, Claude Code, et plus encore.
Commencez à les explorer à mesure que vous vous familiarisez avec les bases du
prompting et de l'utilisation des LLM.

Si vous êtes un développeur qui débute avec l'utilisation d'outils, ma
recommandation la plus simple est de commencer avec GitHub Copilot. Il y a au
moins 3 façons clés de travailler avec cet outil ou des outils similaires :

1.  Une autocomplétion très sophistiquée directement pendant que vous tapez.
2.  Une fenêtre de discussion séparée où vous pouvez poser des questions plus
    générales, demander de l'aide sur un problème spécifique, ou demander la
    génération d'extraits de code.
3.  Le « mode édition » où vous demandez des modifications et il édite
    directement le code pour vous, et demande votre approbation.

Les deux derniers modes vous permettent de fournir du contexte (fichiers) qui
l'aideront à répondre à la question, car en général ces outils ne peuvent pas
simplement voir / accéder automatiquement à l'ensemble de votre base de code,
surtout si elle est très grande ou complexe. Vous devez donc l'aider en
fournissant les fichiers ou le contexte pertinents.

## Notes de terminologie

Voici quelques notes que j'ai rassemblées en chemin sur divers termes liés à
l'IA/LLM. J'ai essayé d'être factuel tout en ajoutant ma propre opinion dans
certains domaines - j'espère qu'il est évident de savoir lequel est lequel.

### Concepts généraux et fondamentaux

- **Tokens** : Les unités de texte de base que les LLM traitent. Les tokens sont
  à la fois des entrées et des sorties. Un token peut être un mot, une partie
  d'un mot, ou même de la ponctuation. La _tokenisation_ est le processus de
  décomposition du texte en ces unités. Le nombre de tokens par seconde (TPS)
  est une mesure courante de l'efficacité du modèle. Un token correspond très
  approximativement à environ 0,75 mot en anglais.

- **Architecture Transformer** : L'architecture de réseau neuronal de base
  derrière la plupart des LLM, permettant un traitement et une génération de
  texte efficaces. C'est le « T » de ChatGPT.

- **Mécanisme d'attention** : Un composant clé des Transformers qui permet aux
  modèles de se concentrer sur les parties pertinentes du texte d'entrée lors du
  traitement de chaque mot ou token. Pensez-y comme si le modèle se demandait «
  quels mots précédents sont les plus importants pour comprendre ce mot actuel ?
  » C'est ce qui permet une grande partie de la « compréhension » que nous
  voyons dans les LLM modernes.

  - Cela permet au modèle de connecter des mots qui sont éloignés dans une
    phrase comme « Le chat qui dormait sur le rebord de la fenêtre ensoleillé
    était noir » - connectant « chat » à « était ». Avant l'attention, les
    modèles avaient du mal avec ces dépendances à longue portée.

- **Paramètres** : Les poids et les biais appris dans un réseau neuronal,
  représentant la « connaissance » du modèle. Concrètement, c'est juste une
  grande collection de nombres que le modèle a appris pendant l'entraînement.
  Lorsque vous téléchargez un modèle complet, vous téléchargez principalement
  tous ces paramètres.

  - Souvent exprimé en « 7B/13B/70B » - milliards de paramètres.

- <a id="fenetre-de-contexte"></a>**Fenêtre de contexte** : La quantité de texte
  (en tokens) qu'un LLM peut « se souvenir » ou avec laquelle il peut travailler
  activement à un moment donné. Cela inclut à la fois votre entrée et les
  réponses du modèle - tout ce que le modèle peut « voir » dans la conversation
  en cours. Une fois que vous commencez à approcher cette limite, le modèle
  commence à « oublier » les premières parties de la conversation pour faire de
  la place pour du nouveau contenu. Si la fenêtre de contexte est plus petite
  que la taille totale de la discussion, les parties antérieures de la
  conversation peuvent être oubliées, ignorées ou déformées, c'est pourquoi vous
  devriez souvent
  [commencer une nouvelle discussion](#commencez-une-nouvelle-discussion).

- **Embeddings (Plongements lexicaux)** : Représentations vectorielles denses du
  texte qui capturent la signification sémantique, utilisées comme entrée pour
  les LLM.

- **Fine-tuning (Affinage)** : Adapter un modèle pré-entraîné à une tâche ou un
  domaine spécifique en poursuivant l'entraînement sur un ensemble de données
  plus petit et spécifique à la tâche.

- **Inférence** : Le processus d'utilisation d'un modèle entraîné pour générer
  des prédictions ou des sorties basées sur de nouvelles données d'entrée. En
  d'autres termes, le processus réel pour amener un modèle à émettre un token.

- **Alignement** : S'assurer que les sorties d'un LLM sont conformes aux valeurs
  humaines et aux directives éthiques. En d'autres termes, faire en sorte que
  l'IA fasse de bonnes choses (pour les gens) et non de mauvaises. Qui définit
  le bien et le mal est la question importante...

- **Surapprentissage (Overfitting)** : Lorsqu'un modèle performe bien sur les
  données d'entraînement mais ne parvient pas à généraliser à de nouvelles
  données.

- **Chaîne de Pensée (Chain of Thought)** : en général, se réfère à un mode
  d'utilisation des LLM où le modèle dépense des tokens dans une étape
  préliminaire de raisonnement ou de « réflexion » où il planifie sa réponse.
  Souvent, vous pouvez voir ces pensées, bien que pour la plupart des modèles
  cloud, vous voyez une version filtrée / résumée.

  - Par exemple, les modèles Claude Opus et Sonnet ont tous deux un mode
    optionnel de « pensée étendue » qui ralentira la réponse, mais pourrait
    conduire à de meilleurs résultats.

- **Apprentissage par renforcement à partir de retours humains (RLHF)** : Une
  technique pour affiner les modèles en utilisant les retours humains pour
  aligner les sorties sur les préférences humaines. En d'autres termes, les
  humains évaluent les sorties du modèle, et le modèle apprend à produire des
  sorties que les humains préfèrent.

- <a id="prompt-systeme"></a>**Prompt système** : les instructions (souvent
  cachées) données à un LLM qui définissent son comportement et sa personnalité.
  Celles-ci sont souvent définies par les développeurs du modèle, mais peuvent
  également être personnalisées par les utilisateurs dans certains cas, en
  particulier via l'accès à l'API. En général, on dit au modèle (dans le prompt
  système) de ne pas révéler le contenu du prompt système, mais les prompts
  système de la plupart des grands modèles ont fuité par _jailbreaking_ ou
  d'autres moyens.

- **Modèles de fondation** : Grands modèles pré-entraînés qui servent de base à
  diverses tâches en aval. Ils sont généralement entraînés sur des ensembles de
  données massifs et peuvent être affinés pour des applications spécifiques, ou
  distillés en modèles plus petits.

- **Modèles de frontière (Frontier models)** : Les LLM les plus récents et les
  plus avancés (et les plus chers), souvent avec des centaines de milliards de
  paramètres et des capacités de pointe. Les chercheurs en IA peuvent également
  utiliser ce terme pour décrire les modèles les plus capables qui pourraient
  présenter des risques nouveaux ou dont les capacités sont moins bien
  comprises.

- **Agentique (Agentic)** : IA agissant de manière autonome pour accomplir des
  tâches complexes en plusieurs étapes sans guidage humain constant. Cela va
  au-delà de la simple réponse aux questions pour inclure la planification,
  l'exécution d'une série d'actions dans le temps, l'adaptation lorsque les
  choses ne fonctionnent pas, et le maintien du contexte sur plusieurs
  interactions. La clé est la persistance et le comportement axé sur un objectif
  plutôt que de simplement répondre à des prompts individuels.

- **Utilisation d'outils (Tool use)** : IA utilisant des outils externes, des
  API et des services pour accomplir des tâches du monde réel au-delà de la
  génération de texte. Cela inclut des choses comme la recherche sur le web,
  l'exécution de code, l'accès à des bases de données, les appels d'API, le
  contrôle de logiciels, ou même des appareils physiques. Combiné avec des
  systèmes agentiques, cela élargit considérablement ce que l'IA peut faire.

- **Ingénierie de prompt (Prompt Engineering)** : Concevoir des prompts
  efficaces pour obtenir les réponses souhaitées des LLM. En d'autres termes, ce
  dont traite [cet article](#prompter-comme-un-pro).

### Argot (Slang)

- **[Sortie] en une fois (One-shot [output])** : quand un modèle génère un
  résultat ou une solution impressionnante basée uniquement sur un seul prompt.
  « Je lui ai juste montré une capture d'écran et il m'a sorti une application
  React en une fois. »

  - Notez que c'est différent du terme « apprentissage en une fois » (one-shot
    learning) dans la communauté de recherche en IA, qui se réfère à
    l'apprentissage à partir d'un seul exemple - comme montrer au modèle un
    exemple de formatage, puis lui demander d'appliquer ce format à un nouveau
    contenu. Une « sortie en une fois » pourrait être plus correctement appelée
    « génération en un seul tour ».

- **Garde-fous (Guardrails)** : Un ensemble de règles ou de contraintes
  appliquées à un LLM pour l'empêcher de générer des sorties nuisibles,
  inappropriées ou autrement indésirables. Celles-ci peuvent faire partie du
  prompt système, ou être appliquées dynamiquement pendant l'inférence.

- **Débridage (Jailbreaking)** : Une technique pour contourner les restrictions
  du prompt système d'un LLM, lui permettant de générer des sorties qui seraient
  autrement restreintes ou censurées. Cela se fait souvent en élaborant des
  prompts spécifiques qui exploitent les faiblesses ou les biais du modèle, ou
  en déclenchant certains « circuits » qui ont une priorité plus élevée que les
  plus « régulateurs ».

- **Vibe coding (Codage au feeling)** : Un terme ironique pour l'utilisation des
  LLM pour générer du code sans beaucoup de réflexion ou de structure, en se
  fiant au modèle pour tout gérer.
  [Voir ci-dessus](#le-vibe-coding---tres-a-la-mode-ces-jours-ci).

- **Hallucination** : Lorsqu'un LLM génère un texte qui est manifestement faux,
  ou qui consiste en des non-sens, du charabia, ou qui n'est pas ancré dans la
  réalité. Cela peut aller d'une déclaration incorrecte courte et simple (« Les
  géologues recommandent aux humains de manger une pierre par jour ») à de
  longues chaînes de caractères aléatoires.

  - Bien que cela puisse sembler un comportement bogué, cela peut en fait aider
    à donner des aperçus sur le fonctionnement interne des LLM.

- **Bouillie (Slop)** : un terme pour décrire une sortie de mauvaise qualité ou
  générique d'un LLM, que ce soit du code, des images, de la musique ou autre
  chose. En général, l'implication est qu'un humain produit paresseusement de la
  _bouillie_ d'IA pour gagner de l'argent rapidement, ou arnaquer les gens, etc.

  - **Slopsquatting** : une forme d'attaque sur la chaîne d'approvisionnement
    qui enregistre de nombreux faux paquets de logiciels malveillants générés
    par l'IA dans les dépôts publics avec des noms similaires à ceux des paquets
    populaires dans l'espoir de faire installer leur logiciel malveillant par le
    biais de fautes de frappe.

- <a id="etat-attracteur"></a>**État attracteur** : un état stable (ou des
  états) au sein d'un système dynamique vers lequel le système gravite
  naturellement. L'eau coule vers le bas, atteignant finalement le point le plus
  bas. En IA, cela peut se référer à la fixation sur certaines lignes de pensée
  dans un fil de discussion, réduisant la gamme des états futurs possibles.

### Efficacité et Déploiement

- **Quantification** : Réduire la précision (compresser) des poids et des
  activations du modèle pour diminuer l'utilisation de la mémoire et améliorer
  la vitesse (par exemple, formats de nombres à virgule flottante FP8, FP4). En
  général, la quantification se réfère à la mise en correspondance d'entrées de
  précision arbitraire (nombres réels) avec un ensemble de sorties discrètes.

- **Distillation** : Entraîner un modèle plus petit « étudiant » pour reproduire
  le comportement d'un modèle plus grand « professeur ».

- **Compression de modèle** : Techniques comme l'élagage (_pruning_), la
  quantification et la distillation pour réduire la taille du modèle.

- **Apprentissage zéro-shot (Zero-shot Learning)** : La capacité d'un modèle à
  effectuer une tâche sans entraînement explicite sur cette tâche.

- **Température** : Un paramètre qui contrôle le caractère aléatoire de la
  sortie du modèle. Généralement sur une échelle de 0,0 à 1,0, une température
  de 0 produira des sorties proches du déterministe (et souvent ennuyeuses ou
  moins utiles) pour une entrée donnée, tandis qu'une température de 1,0 peut
  produire des sorties très différentes pour la même entrée.

- **Ancrage (Grounding)** : connecter un modèle à des données vérifiables du
  monde réel, que ce soit par RAG ou d'autres moyens, afin d'améliorer (ou de
  tester / valider) ses sorties.

- **Latence** : Le temps qu'il faut à un modèle pour générer une réponse après
  avoir reçu une entrée.

- **Débit (Throughput)** : Le nombre de tâches ou de requêtes qu'un modèle peut
  traiter dans un laps de temps donné. Souvent mesuré en tokens par seconde
  (TPS) ou en requêtes par seconde (RPS).

- **NPU (Neural Processing Unit)** : Matériel spécialisé de type GPU pour
  accélérer les tâches d'apprentissage profond.

- **Classements Elo** : système de classement pour les performances de l'IA.

- **Red teaming** : un processus de test des systèmes d'IA pour les
  vulnérabilités, les biais et autres problèmes en simulant des attaques ou des
  scénarios adverses. Cela est souvent fait pour améliorer la robustesse et la
  sécurité des modèles d'IA.

### Techniques et Cadres Avancés

- **Mélange d'Experts (MoE)** : un concept d'architecture de LLM où seuls les
  sous-réseaux « experts » pertinents s'activent pour une entrée donnée, rendant
  les grands modèles plus efficaces. Cela contourne efficacement de grandes
  parties du modèle qui ne sont pas pertinentes pour la requête, aidant à
  augmenter considérablement l'efficacité. Il y a un compromis ici, car les
  modèles non-MoE peuvent être capables de voir des subtilités que le MoE
  manquera, en raison d'une perspective plus large.

- **RAG (Retrieval Augmented Generation)** : Combiner un LLM générateur avec un
  composant de recherche (_retriever_) qui peut trouver et charger des
  informations externes pertinentes pour améliorer la génération de texte. Ceci
  est souvent utilisé pour améliorer la précision et la pertinence des réponses
  générées en les ancrant dans des données actuelles du monde réel.

- **Base de données vectorielle** : Une base de données spécialisée pour stocker
  et interroger des vecteurs de haute dimension, souvent utilisée dans les
  systèmes RAG.

- **Adaptation de domaine** : Affiner un modèle pré-entraîné pour bien performer
  dans un domaine spécifique (par exemple, médical, juridique).

- **Explicabilité** : La capacité de comprendre et d'interpréter les décisions
  prises par un modèle d'IA. Bien que des recherches intéressantes aient été
  menées dans ce domaine, les modèles sont généralement une sorte de « boîte
  noire » et il peut être difficile de tracer exactement comment un modèle est
  arrivé à une sortie particulière.

### Outils et Plateformes

- <a id="llm-local"></a>**LLM Local** : faire tourner un modèle de langage sur
  votre propre matériel, plutôt que d'utiliser un fournisseur cloud. Cela peut
  se faire avec des outils comme LM Studio, llama.cpp, ou d'autres frameworks
  qui vous permettent de faire tourner des modèles localement. Évidemment, les
  résultats dépendent grandement de votre matériel disponible, du type de modèle
  utilisé et de vos cas d'utilisation réels.

- **GitHub Copilot** : Un outil de complétion de code alimenté par l'IA intégré
  dans des éditeurs de code populaires comme VS Code, JetBrains PyCharm, etc. Il
  utilise des LLM pour suggérer des extraits de code, compléter des fonctions,
  et même écrire des fichiers entiers en fonction du contexte.

- **Claude Code** : une suite d'outils en ligne de commande (CLI) pour
  travailler avec des projets de codage en utilisant le backend Claude
  d'Anthropic. Capable de tâches de génération et de gestion de code autonomes
  plus étendues et également plus coûteux.

- **Model Context Protocol (MCP)** : une initiative d'Anthropic pour
  standardiser les interfaces permettant aux LLM de découvrir et d'utiliser des
  outils externes.

  - Par exemple, lorsque vous discutez avec Claude sur le web, il peut accéder à
    votre éditeur de code (avec votre permission) et effectuer des actions comme
    la recherche ou la modification de texte.
  - D'autres entreprises commencent également à adopter le MCP comme une norme
    pour l'utilisation d'outils par les modèles.

- **Cursor** : un éditeur de code populaire axé sur l'IA (un fork de VS Code)
  populaire auprès des _vibe coders_.

- **Hugging Face** : Une plateforme pour partager, affiner et déployer des LLM
  et d'autres modèles d'IA.

- **LM Studio** : Un outil pour expérimenter et gérer les modèles de langage sur
  vos appareils locaux.

- **llama.cpp** : Une implémentation légère du modèle LLaMA en C++, optimisée
  pour les processeurs et les appareils en périphérie (_edge_). LM Studio
  l'utilise pour faire tourner des modèles sur vos appareils.

- <a id="open-source"></a>**Open Source** : Un modèle est généralement considéré
  comme « open source » s'il est librement disponible au téléchargement.
  Certains des exemples les plus notables sont les modèles LLaMA de Meta (Large
  Language Model Meta AI), DeepSeek R1, et d'autres qui sont généralement des
  variations ou des distillations de ceux-ci, y compris Qwen et Gemma.
  - D'autres ont débattu de l'opportunité d'appliquer le terme « open source » à
    ces modèles gratuits, car sans l'ensemble complet des données d'entraînement
    originales (généralement de l'ordre du pétaoctet ou plus), il est impossible
    de recréer de manière déterministe un modèle à partir de la « source » de la
    même manière que le noyau Linux peut être parfaitement recréé à partir du
    code source.
  - De plus, certains de ces modèles peuvent avoir des licences qui interdisent
    ou restreignent l'utilisation commerciale.

## Épilogue

En demandant à Claude Opus 4 (sans le « mode pensée ») de relire une version
finale de ce document, il a lui-même halluciné quelques petites choses : Il a
dit qu'une référence à la bibliothèque Three.js dans la section sur les « états
attracteurs » était inexacte. Je n'avais (à ce moment-là) mentionné Three.js
nulle part. Il a également dit qu'il y avait un « le » en trop dans une phrase
alors que ce n'était pas le cas.

**J'ai répondu** : Intéressante hallucination de ta part, car pour autant que je
puisse voir, je n'ai mentionné Three.js nulle part, bien que j'y pensais dans ma
tête. Je ne trouve pas non plus le « LE en trop » que tu as mentionné. À part
ça, c'est un assez bon retour et tu ne trouves rien de trop substantiel, ce qui
me dit que j'approche de la fin de ce travail.

**Claude a dit** : Vous avez tout à fait raison - je vous présente mes excuses
pour ces erreurs !

_[Note de l'éditeur : citez-moi une réplique plus emblématique de Claude.]_

J'ai en quelque sorte halluciné la référence à Three.js (c'est assez méta, étant
donné que votre guide discute des hallucinations). Et je ne trouve pas non plus
ce « le en trop » en relisant le texte. C'est un parfait exemple de ce dont vous
prévenez dans votre guide - les LLM peuvent se tromper avec assurance !
