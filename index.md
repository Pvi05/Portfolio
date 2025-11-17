---
layout: default
title: Home
---

<!-- # Portfolio
An overview of my student &amp; personnal projects  -->

* TOC
{:toc}

# Cleaning and classification of hand-written job titles

SypherAI is a French start-up selling a SaaS CRM management tool. Its system is capable of cleaning and retrieving missing information about potential clients, and sorting them using machine learning on previous sales to identify those most likely to buy.
The job title is one of the most critical pieces of information for the ML algorithm.

Our mission was to provide a pipeline capable of **cleaning and classifying hand-written job titles** previously extracted from LinkedIn and similar sources.

## The mission further explained

Job titles, when written as they appear on LinkedIn, can vary greatly even for the same or similar positions, sometimes to the point of being nearly obscure.
A simple example might be the possible job titles for an HR manager. People can describe themselves as "Head of HR", "Recruiting Manager", but also "Talent Acquisition" or even "Head Hunter". Although those variations might convey slightly different meanings, they are not relevant for our case (identifying prospects) and keeping them as-is can be detrimental to the efficiency of the ML algorithm.

Our solution should therefore categorize all of these titles under a single 'HR manager' label to be fed to the algorithm.
Since the variations for a single title can be almost endless, our pipeline should use a natural language processing model to extract the semantics of the title rather than doing simple matching, which would be tedious and prone to failure.

<div align='center'>
  <img alt="LinkedIn Exemple" src="images/LinkedIn1.png" />
  <img alt="LinkedIn Exemple" src="images/LinkedIn2.png" />
  <img alt="LinkedIn Exemple" src="images/LinkedIn3.png" />
  <img alt="LinkedIn Exemple" src="images/LinkedIn4.png" />
  <p><em>A variety of HR manager related jobs titles</em></p>
</div>

## How the mission went & my contribution

Among all the NLP models we tested, we selected DeBERTa-Zero-shot for its precision and its ability to handle rare or unique titles, thanks to semantic capabilities that make it significantly more resilient than a classic supervised classification model. An active learning approach was also considered to compensate for eventual models imprecisions over time, but early results were inconclusive compared to DeBERTa, so it was discontinued.

One of the main issues was the identification of target categories, initially approached using the International Labour Organization classification table; but as this turned out to be a much broader challenge than the original scope, it was agreed with SypherAI that categories would be supplied by the pipeline user. Indeed, interesting prospects usually belong to a specific job category already identified by the sales manager in the CRM.

One key technical solution that drastically improved model results was the choice to perform two classifications instead of one: one for the job level (Senior, Junior, Manager, Director, etc.) and one for the job department (HR, Sales, IT, Production, etc.). This greatly reduced the model's confusion between the level and department parts of the title, which could sometimes lead to cases like 'Head IT Manager' being associated with 'HR Manager.'

After training some basics models (FastText, BERT) to obtain a performance baseline, I personally chose performance criteria to design a benchmark that helped us select a model, and I presented and justified this choice to the client. I also proposed several of the ideas cited above to address limitations, including the ILO table and the two-level classification (originating from a tree-like classification idea, simplified into two levels).

This project allowed me to become familiar with main models and methods for learning natural language processing, as well as with communicating and delivering a project to a client, which was my first mission of this type.

## What we delivered

We delivered to SypherAI a fully working and ready-to-transfer Python codebase containing our model, paired with a GUI to make using our pipeline easier.

The main objective of this project was to reduce manual classification as much as possible, and it was successful: for a 1,000+ client list, only a handful of low-confidence job titles typically require correction.

Due to confidentiality, I am unable to disclose any code, dataset, or app produced in this mission.
You can, however, find below a short video presentation of our GUI.

<p align="center">
  <video style="width: 100%; height: auto;" controls>
    <source src="videos/demo.mp4">
  </video>
</p>

This mission was completed as part of a team of five people.

## Keywords

NLP, zero-shot, classification, ML

# Game of Hex Computer Player

Fully written in OCaml and built using Dune, I implemented my own Hex computer player. It uses various elements of game theory to achieve performance, including an optimized version of the Alpha-Beta algorithm with move ordering and pruning developed specifically for this game. This project was presented as a computer science research work (TIPE) during the competitive exams for entry into top French engineering schools.

## The game of Hex

The game of Hex has disputed origins: often credited to John Nash, who reportedly created it in 1948 during his time at Princeton. It is also attributed to the Danish mathematician Piet Hein in 1942 which could have found it while he was looking for a counterexample to the four-color theorem. Whoever the creator was, Hex is a highly mathematical game, and despite its simple rules it contains many elegant properties.

The rules are as follows:
Each player owns two opposite sides of the hexagonal board, and the goal is to connect them with stones placed in the hexes, alternating with the other player.

<div align='center'>
  <img alt="Hex board" src="images/hex.png" />
  <p><em>A 10x10 hex board where black has won</em></p>
</div>

We can prove that there exists a unique winner for each game of Hex (no draws or situations without a winner).

The game of Hex is very challenging to solve algorithmically due to its huge branching factor on large boards up to 19x19. It has often been compared to the game of Go (the branching factor calculation being the same), and has benefited from the research done to build efficient Go players. MoHex, the best player at the time of this project, is heavily inspired by MoGo and AlphaGo.

## The player

To develop this player, I took inspiration from the first performant computer Hex player, Hexy, developed in 1998 by Vadim V. Anshelevich. His approach, unlike subsequent computer players, doesn't solely rely on highly optimized Alpha-Beta or Monte-Carlo searches but rather on intrinsic game properties to gain information about future plays. His work teaches a lot about Hex game theory and paved the way for future computer players. Although he describes the general idea [here](https://www.sciencedirect.com/science/article/pii/S0004370201001540), there is still plenty of room for personal initiative, as was the case here.

My player therefore couples an Alpha-Beta search with a heuristic called H-SEARCH, which reveals future connections across the board. This information, which acts like predicting future plays without increasing the Alpha-Beta depth too far, is then used to evaluate the board position. The heuristic uses induction rules and properties regarding two hexes being connected, to build a network of possible future connections. An in-depth explanation of the theory is available in the dedicated repo.

This method gives the computer player strong insights, allowing it to make relevant plays and correctly identify critical regions of the board.

This information, however, comes at a significant time cost, so multiple optimizations were implemented to make the player usable, including a dynamic update of H-SEARCH, move ordering and situational pruning, and the implementation of iterative deepening depth-first search for Alpha-Beta to enforce time limits.

The technical implementation uses a variety of structures offered by OCaml to make the algorithm as efficient as possible, including sets and hash tables.

## Results & Code

My implementation did not match the one made by Vadim V. Anshelevich, as it suffered from long calculation times, even with the optimizations, and could then hardly work with the original parameters. However by the quality of its plays it showed how relevant searching for game properties can be to replace brute game-tree searching. 

The full code of the player is available in the following repo, along with a (french) report which goes in depth in the theoretical details of the implementation : 

[github.com/Pvi05/HexPlayer](https://github.com/Pvi05/HexPlayer)

I highly recommend the game of Hex for anyone interested in Computer and Game theory, as its properties makes it an excellent study case for computer player algorithms, and a great game to try overall.

This project was presented to the jury of the Tétraconcours (CentraleSupelec, Mines, CCINP) and the ENS de Paris, where it was graded respectively 18,5 and 16 out of 20 (both A+s).

# Python automata game with water physics

Inspired by cellular automaton games like the Game of Life, we built an automaton game in Python with a strong emphasis on water physics called "La-haut", inspired by Disney's mobile game "Where's My Water?". This was developed in a single week by a team of six.

## The Game

You are given a water source and a drain, and your goal—by placing or removing stones—is to direct the water from the source to the drain. The gameplay is further diversified by adding multiple elements with various properties, such as plants (which grow and absorb water), obsidian (indestructible with the basic tool), dynamite (to break obsidian in a limited area), and more.

<div align='center'>
  <img alt="Hex board" src="images/lahaut.png" />
  <p><em>One of the levels of the game</em></p>
</div>

Using Agile development, we created the basic gameplay elements first and then iteratively improved on the previous versions by adding new blocks, changing water physics, creating levels, etc.

The water physics is a highlight: it uses clever tricks with block types to achieve realistic behavior using a simple automaton (which does not remember history and only uses the previous state to determine the present state).

The game also includes a random-level option, which reuses a modified Game of Life automaton to generate block structures.

I was personally in charge of the graphical interface using Pygame, which included front-end elements for the player such as options and level visualization. I also created a visual level editor from scratch, which allowed us to be much more creative when designing levels than by editing a matrix directly. This functional level editor could, with minimal additional work, be given to players to create their own levels, but that was not implemented due to time constraints.

With this project I experienced Agile development in an intensive one-week sprint and became familiar with Python object-oriented programming, which I had previously used mainly in C or OCaml. The project also strengthened my communication skills for presenting our work and organizing the team, which was diverse and included members less familiar with development.

## The Code

As this was a one-week student sprint challenge, the game is not technically perfect for publishing, but it is close enough with the main mechanics in place and a presentable user interface.

The code is stored in the following repo:

[github.com/Pvi05/La-haut](https://github.com/Pvi05/La-haut)

# Smaller Side-projects

## Non-zenoness proving algorithm

When proving programs with timed automata, non-zenoness is a crucial property to ensure realistic behavior for our models.
An automaton (which models a program or system) is Zeno when there is the possibility of infinitely many actions in finite time—i.e., the automaton can loop infinitely fast between different states.
Currently UPPAAL, the mainstream tool for timed automata model checking, doesn't include a feature to prove non-zenoness. Non-zenoness is a delicate property to address, especially when using synchronization between multiple automata.

The most direct solution to synchronization would be to build the composition automaton of all automata, but this raises complexity concerns since the number of states of such a composition is exponential.
Our algorithm uses rather a sufficient condition for single-automaton systems which we extend to multiple-automaton synchronization by checking pairs of loops related by a single synchronization. It does not provide necessary and sufficient conditions, but it highlights loops at risk of being Zeno and can affirm the non-zenoness of a system if no such risky loops are found.

This program was developed as a side part of a different project about checking properties of an aircraft TCAS system; it was completed alone in one day as an algorithmic challenge. I enjoyed it—I'm always up for a coding challenge, especially when linked to computer theory (though this one involved a fair amount of XML parsing).

This project can be found here:

[github.com/Pvi05/Non-zenoness-checker](https://github.com/Pvi05/Non-zenoness-checker)

## Scraping Bot for Centrale student rooms

CentraleSupélec experiences a student housing shortage, and finding accommodation on campus can be challenging, especially for second-year students. To help with this problem, I built a bot capable of notifying the user via Discord when free rooms become available. This bot uses Selenium to scrape the student housing website (Césal), with careful cookie management to handle captchas, and, if needed, sends a private Discord message to warn of available rooms.

Project can be found here:

[github.com/Pvi05/Cesal_bot](https://github.com/Pvi05/Cesal_bot)

## Survival prediction for foetuses suffering from Congenital Diaphragmatic Hernia

Congenital Diaphragmatic Hernia is a rare disease affecting about 1 in 3,000 foetuses, characterized by a hole in the diaphragm that allows the stomach, liver and other organs to move into the thorax and compress the heart and lungs. Depending on the severity and timing of the condition, this can cause lung and heart development issues and, in the most serious cases, can be life-threatening.

Specialists use ultrasound and MRI to assess the disease and potentially schedule an operation to mitigate the impact on the lungs. They are usually confident about survival rates, but are less able to predict whether the foetus will require long-term respiratory support or only a period of close surveillance. Since this information is crucial for parents in their decision-making about continuing pregnancy, AP-HP and the Kremlin-Bicêtre hospital lab, France's reference center for the disease, asked us to train models on their data to see if prediction capability could be improved.

This project is currently underway and managed by a team of five.

Due to confidentiality I am unable to disclose any code or dataset regarding this mission.