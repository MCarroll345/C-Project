# C-Project

This page is going to serve as logs/report for the C++ A* project.

The purpose of this project is to create an A* algorithm to find the shortest route through a maze.

The Geeks4Geeks article on A* algoritms was very useful for this as it provided alot of theory as to how it works.

<h2>04/03/2026</h2>

I have been trying to implement some of the work that we have been doing in our other classes. We worked on Matrices, and in doing so, we made overloaded operators. What I want to do is overload the = operator, to make a duplicate of the grid that was created. I want to do this so that I can print a new grid with the path taken mapped out using "\ - / |". I met with Michelle today to talk about what I have been working on. She recommended that I work on testing for the algorithm. She also recommened that I keep the overloaded operator in my code even if I end up not using it, as it shows initiative and understanding of the course materials.

The testing that I plan on doing will be testing that each function works, like the verfication and the destination arrival functions. I think that I should also implement exceptions and error handling. We did it recently in a lab and I believe that it would be good for the project, as it shows more innitiative and prudence.

<h2>11/03/2026</h2>

I am working on the testing functions. There will be one general test called that will run all other tests. To make the test grids, I didn't want to go through the process of setting up a new grid for every test so I made a default constructor. It works pretty well so I'm happy to use it. I may reformat the main grid to work off of something like this. I'd say I can get testing done today, leaving just the printing of the final grid and the path.

I have been working on the printing of the final grid. I've abandoned the idea of creating a second grid. I still have the overloaded operator for =. I'm keeping it there to show my thought process. I'm now printing the final grid depending on the character, as well as the cell details. I'm having an issue with "Expression: vector out of subscript". I'm using AI to diagnose the issue, as I know it is an issue with my validation. This is similar to the crowd stirke issue. My validation was set up poorly as it is trying to validate if coordinates are in the grid, but I believe it was checking the grid, but too much, trying to validate outside of the grid.

Nevermind. I was wrong. I had moved where the grid was being made, but I didn't bring the create with it. I wasn't even creating the grid. My validate was working fine now. I'd say I'm pretty much done. 

I have the main file initialising the destination and starting point.

<img width="185" height="229" alt="image" src="Screenshot 2026-03-17 155808.png" />

I changed how I generated the grid. I was talking to Nathan and he showed me how he generated his grid. As I was previously using the srand to make a completely random.

<img width="468" height="205" alt="image" src="Screenshot 2026-03-17 153436.png" />

Now I am using the random library, I get a uniform distribution of 1s and 0s.

<img width="359" height="402" alt="image" src="Screenshot 2026-03-17 155448.png" />

I have also changed the colour of the grid being printed, as it gives a better aesthetic, and is easier to read. This as well as the direction of the path being printed, makes the finished grid look quite good.

<img width="377" height="445" alt="image" src="Screenshot 2026-03-17 155347.png" />

