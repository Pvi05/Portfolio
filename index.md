# Portfolio
An overview of my student &amp; personnal projects 

## Cleaning and classification of hand-written job titles

SypherAI is a french start-up selling a Saas CRM managing tool. Its tool is capable of cleaning and retriving missing info among potential clients, and sorting them using Machine Learning on previous sales to identify the ones most likely to buy.`
The job title is one of the most critical information for the ML algorithm. 

Our mission was to provide a pipeline capable of **cleaning and classifying hand-written job titles** previously extracted from LinkedIn and similars.

### The mission further explained

Job titles, when hand-written as they are on LinkedIn, can vary a lot even for same or similar work positions, sometimes to the point of being almost obscure.
A simple exemple might be the possible job titles for an HR manager. People can describe themselves as "Head of HR", "Recruiting manager", but also "Talent Acquisition" or even "Head hunter". Although thoses variations might convey slightly different meanings, they are not relevant for our case (identifying propects) and keeping them as is can be detrimental for the efficiency of the ML algorithm. 

Our solution should then categorize all of these titles as a single 'HR manager' in order to be fed to the algorithm.
Since the variations for a single title can be almost endless, our pipeline should use a natural language processing model in order to extract the semantics of the title rather than doing a simple matching, which would be fastidious and prone to fail.

<div align='center'>
  <img width="786" height="169" alt="LinkedIn Exemple" src="images/LinkedIn1.png" />
  <img width="775" height="93" alt="LinkedIn Exemple" src="images/LinkedIn1.png" />
  <img width="775" height="94" alt="LinkedIn Exemple" src="images/LinkedIn1.png" />
  <img width="778" height="115" alt="LinkedIn Exemple" src="images/LinkedIn1.png" />
  <p><em>A variety of HR manager related jobs titles</em></p>
</div>

### How the mission went 

Between all the NLP models we tested, we selected the DeBERTa-ZeroShot for its great precision and ability to deal well with rare or unique titles, thanks to its semantic capabilities which makes it significantly more resilient than a classic supervised classification model. An active learning approach was also considered as it would make up for the eventual imprecisions of a model over time, but early results showed it inconclusive compared to DeBERTa so it was discontinued.

One of the main issue was the identification of target categories, initialy dealt with by using the International Labour Organization classification table ; but as this turned out to be a much wider challenge than the original issue, it was decided in accordance with SypherIA that they would be supplied by the pipeline user. Indeed interesting prospects usually belong in a specific job category, already identified by the sales manager using the CRM manager.

One of the key technical solution which drasticly improved models results was the choice to perform two classifications instead of one. One on the job level (Senior, Junior, Manager, Director, etc) and one on the job departement (HR, Sales, IT, Production, etc). It greatly reduced models confusion between the level part of the title and the departement, which could sometimes lead things like 'Head IT manager' to be associated with 'HR manager'.

### What we delivered

We delivered to SypherIA a fully working and ready to transfert Python code containing our model, paired with a GUI to make using our pipeline easier.

I am unable to disclose any code, dataset or app produced in this mission.
You can however find below a short presentation video of our GUI.



This mission was done as part of a team of 5 people.

### Keywords 

NLP, ZeroShot, Classification, ML

## Joueur au Jeu de Hex 

## Dévellopement d'un jeu de type automate cellullaire, basé sur la physique de l'eau.
