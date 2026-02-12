---
layout: default
title: Home
---

<!-- # Portfolio
An overview of my student &amp; personnal projects  -->

* TOC
{:toc}

# Introduction

In this Portfolio you may find IA/ML oriented projects (using mostly Python and main-stream librairies such as Scikit-learn, Pytorch), but also more low-level projects (using OCaml, C, VHDL). 

# Survival prediction for foetuses suffering from Congenital Diaphragmatic Hernia

Congenital Diaphragmatic Hernia is a rare disease affecting about 1 in 3,000 foetuses, characterized by a hole in the diaphragm that allows the stomach, liver and other organs to move into the thorax and compress the heart and lungs. Depending on the severity and timing of the condition, this can cause lung and heart development issues and, in the most serious cases, can be life-threatening. We built and trained a model capable of predicting oxygen requirement at 28 days post-birth with a significant accuracy , in order to further guide doctors' diagnosis.

## A deeper dive into the mission context

Mandated by APHP (Paris' hospital) reference center for the disease, we were asked to try and see if Machine Learning could help doctors make a better prediction whether one foetus would need life-long oxygen support or not. It is an important prediction, as it is made as soon as the disease is diagnosed (usually ~5 months of pregnancy), will guide the entire treatment process and whether IMG (termination of pregnancy) should be advised. Otherwise, a possible treatment is the insertion of a balloon in the lungs of the foetus to counteract the compression exerted by the other organs.

Specialists use ultrasound / MRI measurements as criterias (some of which are still being improved) to assess the foetus condition. They are usually confident about survival rates, but are less able to predict whether the foetus will require long-term respiratory support or only a period of close surveillance, which is our mission to make clearer.

## How the mission went & my contribution

In relation to APHP medical personnel, we proceeded first to make the medical data suitable for ML. This involved close inspection of data to remove irregularities, try to make the most of medical commentaries, uniformize data types (to float), and some more. In the end, we were left with a proper dataset of ~150 patients. I was at the origin of most innovation at this stage, most notably the conversion of medical commentaries as categorical data, the most we could do without requiring NLP.

Due to the small number of samples, simplest methods are best, and we proceeded to train the first models and perform data analysis (feature ranking, various preprocessing, new target columns, ...). At this stage the medical team feedback was crucial, it allowed us to keep our result medically relevant, while our models would also confirm well-installed metrics to rank a foetus condition.

For the final implementation, the key ideas were kept, but made robust and scaled with wider model testing, using of scikit-learn framework to build tailored models (Pipelines, Imputers, Scalers, Encoders, Recursive Feature Selection, Imblearn transformers, ...), and perform methodologically sound model scoring (NestedCrossValidation). Based on Accuracy and Recall (very important in a medical context), the most performant model families were identified to be Random Forest and Logistic Regression, the latter being finally chosen for a better overall trade-off. Scores on unseen data are about 82% accuracy and 86% recall.
I was the one responsible for this last implementation, which was delivered to the hospital.

Alongside the model selection, a graphical interface using Streamlit was developed to make the model easier to use in a medical context. One of the main features of this interface (and one of my ideas), was to include 5 closely related patients from the dataset (using KNN) alongside the prediction, allowing the doctor to reflect the current case with past ones.

## What we delivered

We delivered a complete model training and selection code, as a basis for our final choice, alongside the final model implemented in the interface, ready to use in a medical level. Early meetings with the medical team were very positive, and we are expected to have soon a complete presentation of our work to hospital personnel, and publication of a research article (in a medical journal).

Due to confidentiality I am unable to disclose any code or dataset regarding this mission. However, you can find below a screenshot of the delivered user interface.

<div align='center'>
  <img alt="Streamlit Interface" src="images/APHP.png" />
  <p><em>To the left, fields for the selected features. Results and related patients on the right</em></p>
</div>

## Keywords

Classification, Machine Learning, Ensemble Methods, Decision Tree, Data Treatment & Analysis, Preprocessing methods, Scikit-Learn.


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

NLP, Zero-shot Learning, Classification, Pipeline

# Building a fonctionnal minimal RISC architecture

Using VHDL and Intel's Quartus Suite, I built a fonctionnal RISC architecture, describing and linking the main components to form a fully-fonctionnal CPU. Alongside this hardware description, I designed an ISA and Assembly to go along and use this CPU.

## The project in detail 

The final architure implements all the essentials of a Von Neumann-like computer architecture, namely basic arithmetics and bit-wise operations, conditionnal jumps, RAM interaction, stack interactions, and function handling.

To achieve this it uses the following components :
- Decoder
- ALU providing operations such as ADD, SUB, AND, OR, Logical Shifts, and more
- Register File comprising of 14 general purpose registers, plus a stack pointer (sp) and link register (lr). The program counter is also accessible as a register (pc)
- Fetch unit that store the pc and updates when jumping
- ROM to store the program instructions
- RAM to store volatile data

To interact with the system, I provided an Assembly language with its Python Assembler, allowing easier program design. 
The Assembly notably supports the usage of labels for easier function and jump manipulations. 

<div align='center'>
  <img alt="CPU design" src="images/CPU.png" />
  <p><em>Overview of the CPU Architecture</em></p>
</div>

## Results and Sample program

The project was tested using ModelSim and testbenches, to ensure components and programs correct behavior. 

To showcase the final architecture, you may find an implementation of Russian Peasant Multiplication (multiplication not being natively supported) and Exponentiation by Squaring, which illustrate the function handling and recursive capabilities of the system.

Finally the code was synthesised for a MAX10 Altera FPGA, to test real-world behavior. However no interaction with the card was implemented, so it is not shown in the final version.

The complete code for the project is available here :

[github.com/Pvi05/RISC_CPU](https://github.com/Pvi05/RISC_CPU)

## Keywords
VHDL, Hardware Conception, RISC, Assembly, FPGA

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

My implementation did not match the one made by Vadim V. Anshelevich, as it suffered from long calculation times, and could hardly work with the original parameters. However by the quality of its plays it showed how relevant searching for game properties can be to replace brute game-tree searching. 

The full code of the player is available in the following repo, along with a (french) report which goes in depth in the theoretical details of the implementation : 

[github.com/Pvi05/HexPlayer](https://github.com/Pvi05/HexPlayer)

I highly recommend the game of Hex for anyone interested in Computer and Game theory, as its properties makes it an excellent study case for computer player algorithms, and a great game to try overall.

This project was presented to the jury of the Tétraconcours (CentraleSupelec, Mines, CCINP) and the ENS de Paris, where it was graded respectively 18,5 and 16 out of 20 (both A+s).

## Keywords

Game Theory, Computer Theory, Alpha-Beta, heuristic, Game of Hex

# Smaller Side-projects

## Scraping Bot for Centrale student rooms

CentraleSupélec experiences a student housing shortage, and finding accomodation on campus can be challenging, especially for second-year students. To help with this problem, I built a bot capable of notifying the user via Discord when free rooms become available. This bot uses Selenium to scrape the student housing website (Césal), with careful cookie management to handle captchas, and, if needed, sends a private Discord message to warn of available rooms. Additionaly, multiple options are accessible to the host to custom their experience with the bot (choose which building to track, which discord Ids should receive message, ...)

This bot was used by multiple classmates and students (including myself) to help them find the accomodation they needed.

Project can be found here:

[github.com/Pvi05/Cesal_bot](https://github.com/Pvi05/Cesal_bot)

## Building a frugal VLM

In partnership with LetxbeIA, a french deeptech specialised in document treatment with comuter vision, we are challenged to make a VLM with similar capacities to the current models used by the company, but much smaller, and therefore more sustainable. Current models are thought to be over-sized for their actual use case, and reduction from 7b parameters to ~4b is the ideal goal.

This project is underway with a team of 5 people.
Due to Intellectual Property, I an unable to release now any code, but this may change soon in the future, as the company is Open-Source oriented.

## Python automata game with water physics

Inspired by cellular automaton games like the Game of Life, we built an automaton game in Python with a strong emphasis on water physics called "La-haut", inspired by Disney's mobile game "Where's My Water?". This was developed in a single week by a team of six.

You are given a water source and a drain, and your goal—by placing or removing stones—is to direct the water from the source to the drain. The gameplay is further diversified by adding other elements with various properties, such as plants, obsidian (indestructible), dynamite, and more.

<div align='center'>
  <img alt="Hex board" src="images/lahaut.png" />
  <p><em>One of the levels of the game</em></p>
</div>

Using Agile development, we created the basic gameplay elements first and then iteratively improved on the previous versions by adding new blocks, changing water physics, creating levels, etc.

The water physics is a highlight: it uses clever tricks with block types to achieve realistic behavior simply using an automaton. The game also includes a random-level option, which reuses a modified Game of Life automaton to generate block structures.

In charge of the graphical interface using Pygame, I also created a visual level editor from scratch, which allowed my team to be much more creative. This functional level editor was not given to players due to time constraints.

With this project I experienced Agile development in an intensive one-week sprint and became familiar with Python object-oriented programming.

[github.com/Pvi05/La-haut](https://github.com/Pvi05/La-haut)

## Non-zenoness proving algorithm

When proving programs with timed automata, non-zenoness is a crucial property to ensure realistic behavior for our models.
An automaton (which models a program or system) is Zeno when there is the possibility of infinitely many actions in finite time—i.e., the automaton can loop infinitely fast between different states.
Currently UPPAAL, the mainstream tool for timed automata model checking, doesn't include a feature to prove non-zenoness. Non-zenoness is a delicate property to address, especially when using synchronization between multiple automata.

The most direct solution to synchronization would be to build the composition automaton of all automata, but this raises complexity concerns since the number of states of such a composition is exponential.
Our algorithm uses rather a sufficient condition for single-automaton systems which we extend to multiple-automaton synchronization by checking pairs of loops related by a single synchronization. It does not provide necessary and sufficient conditions, but it highlights loops at risk of being Zeno and can affirm the non-zenoness of a system if no such risky loops are found.

This program was developed as a side part of a different project about checking properties of an aircraft TCAS system; it was done completed alone in one day as an algorithmic challenge. I enjoyed it—I'm always up for a coding challenge, especially when linked to computer theory (though this one involved a fair amount of XML parsing).
The overall project was graded A+.

This project can be found here:

[github.com/Pvi05/Non-zenoness-checker](https://github.com/Pvi05/Non-zenoness-checker)

## For the future

I have several projects in mind. One of which would be to build a home-compiler for a language I have yet to decide. I hope to be able to bring updates soon on this project : stay tuned !