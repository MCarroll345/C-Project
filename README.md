<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>C-Project Devlog — Mark Carroll</title>
<link href="https://fonts.googleapis.com/css2?family=Share+Tech+Mono&family=Barlow:wght@400;500;600&display=swap" rel="stylesheet"/>
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --green: #39ff14;
    --dim-green: #1a7a06;
    --bg: #0a0f0a;
    --surface: #0f160f;
    --border: #1e2e1e;
    --text: #c8e6c8;
    --muted: #6a8f6a;
    --accent: #39ff14;
  }

  body {
    background: #060b06;
    color: var(--text);
    font-family: 'Barlow', sans-serif;
    min-height: 100vh;
    padding: 2rem 1rem;
  }

  .page {
    max-width: 780px;
    margin: 0 auto;
  }

  /* ── terminal chrome ── */
  .terminal-bar {
    background: var(--surface);
    border: 1px solid var(--border);
    border-bottom: none;
    border-radius: 10px 10px 0 0;
    padding: 10px 16px;
    display: flex;
    align-items: center;
    gap: 8px;
  }
  .dot { width: 12px; height: 12px; border-radius: 50%; }
  .dot.r { background: #ff5f57; }
  .dot.y { background: #ffbd2e; }
  .dot.g { background: #28c840; }
  .terminal-title {
    flex: 1;
    text-align: center;
    font-family: 'Share Tech Mono', monospace;
    font-size: 13px;
    color: var(--muted);
  }

  .log-wrap {
    background: var(--bg);
    border: 1px solid var(--border);
    border-radius: 0 0 12px 12px;
    overflow: hidden;
  }

  /* ── header ── */
  .log-header {
    padding: 2rem 2rem 1.5rem;
    border-bottom: 1px solid var(--border);
  }
  .project-tag {
    font-family: 'Share Tech Mono', monospace;
    font-size: 11px;
    color: var(--dim-green);
    letter-spacing: 2px;
    text-transform: uppercase;
    margin-bottom: 8px;
  }
  .project-title {
    font-family: 'Share Tech Mono', monospace;
    font-size: 30px;
    color: var(--accent);
    line-height: 1.2;
  }
  .project-sub {
    margin-top: 10px;
    font-size: 14px;
    color: var(--muted);
    line-height: 1.7;
    max-width: 580px;
  }
  .tag-row { display: flex; flex-wrap: wrap; gap: 6px; margin-top: 14px; }
  .tag {
    font-family: 'Share Tech Mono', monospace;
    font-size: 11px;
    padding: 3px 10px;
    border-radius: 4px;
    background: #0d2e0d;
    color: var(--accent);
    border: 1px solid var(--dim-green);
  }
  .tag.warn { background: #1e0d00; color: #ff8844; border-color: #aa4400; }
  .tag.info { background: #020d1e; color: #88bbee; border-color: #336699; }

  /* ── timeline ── */
  .entries { padding: 1rem 2rem 2rem; }

  .entry {
    border-left: 2px solid var(--border);
    margin-top: 2rem;
    padding-left: 1.5rem;
    position: relative;
  }
  .entry::before {
    content: '';
    width: 10px; height: 10px;
    border-radius: 50%;
    background: var(--dim-green);
    border: 2px solid var(--accent);
    position: absolute;
    left: -6px; top: 4px;
  }
  .entry-date {
    font-family: 'Share Tech Mono', monospace;
    font-size: 13px;
    color: var(--accent);
    letter-spacing: 1px;
    margin-bottom: 12px;
  }
  .entry-body { font-size: 14px; line-height: 1.8; color: var(--text); }
  .entry-body p { margin-bottom: 10px; }
  .entry-body p:last-child { margin-bottom: 0; }

  code {
    font-family: 'Share Tech Mono', monospace;
    font-size: 13px;
    background: #0d2e0d;
    color: var(--accent);
    padding: 1px 5px;
    border-radius: 3px;
  }

  .callout {
    border-radius: 6px;
    padding: 10px 14px;
    margin: 12px 0;
    font-family: 'Share Tech Mono', monospace;
    font-size: 13px;
    line-height: 1.6;
    border: 1px solid;
  }
  .callout.green  { background: #0d2e0d; color: var(--accent);  border-color: var(--dim-green); }
  .callout.orange { background: #1e0d00; color: #ff8844;         border-color: #aa4400; }
  .callout.blue   { background: #020d1e; color: #88bbee;         border-color: #336699; }

  /* ── images ── */
  .img-block {
    margin: 14px 0;
    border: 1px solid var(--border);
    border-radius: 6px;
    overflow: hidden;
    display: inline-block;
    max-width: 100%;
  }
  .img-block img {
    display: block;
    max-width: 100%;
    height: auto;
  }
  .img-caption {
    padding: 6px 10px;
    font-family: 'Share Tech Mono', monospace;
    font-size: 11px;
    color: var(--muted);
    background: var(--surface);
    border-top: 1px solid var(--border);
  }

  /* ── footer ── */
  .author-row {
    padding: 1rem 2rem;
    border-top: 1px solid var(--border);
    display: flex;
    align-items: center;
    gap: 12px;
  }
  .author-avatar {
    width: 36px; height: 36px;
    border-radius: 50%;
    background: #0d2e0d;
    border: 1px solid var(--dim-green);
    display: flex; align-items: center; justify-content: center;
    font-family: 'Share Tech Mono', monospace;
    font-size: 13px;
    color: var(--accent);
    flex-shrink: 0;
  }
  .author-name { font-size: 14px; font-weight: 500; color: var(--text); }
  .author-sub  { font-size: 12px; color: var(--muted); }
</style>
</head>
<body>
<div class="page">

  <div class="terminal-bar">
    <div class="dot r"></div>
    <div class="dot y"></div>
    <div class="dot g"></div>
    <div class="terminal-title">C++-Project / devlog.html — Mark Carroll</div>
  </div>

  <div class="log-wrap">

    <div class="log-header">
      <div class="project-tag">ATU &nbsp;·&nbsp; C++ Project &nbsp;·&nbsp; 2026</div>
      <div class="project-title"># C++-Project</div>
      <p class="project-sub">
        This will serve as a development log and report for the C++ A* pathfinding project.
        The goal is to implement an A* algorithm to find the shortest route through a randomly generated maze.
      </p>
    </div>

    <div class="entries">

 	    <!-- ── Entry 0 ── -->
      <div class="entry">
        <div class="entry-date">&gt; 25/02/2026</div>
        <div class="entry-body">
          <p>I used GeeksforGeeks to get a basis for the A* algorithm. I'm going to split the grid elements into it's own file so that the file layout is better.</p>
        </div>
      </div>

      <!-- ── Entry 1 ── -->
      <div class="entry">
        <div class="entry-date">&gt; 04/03/2026</div>
        <div class="entry-body">
          <p>Working on applying concepts from the Matrices lab — specifically overloaded operators. The plan is to overload the <code>=</code> operator to duplicate the grid, so a separate copy can be printed with the path mapped out using directional characters <code>\ - / |</code>.</p>
          <p>Met with Michelle today. She recommended focusing on testing for the algorithm, and keeping the overloaded operator in the codebase even if unused — it demonstrates initiative and understanding of the course material.</p>
        </div>
      </div>

      <!-- ── Entry 2 ── -->
      <div class="entry">
        <div class="entry-date">&gt; 11/03/2026</div>
        <div class="entry-body">
          <p>Working on the test functions. A single <code>runAllTests()</code> will run the tests. I didn't want to create a new grid for every test, so I made a default constructor. I just pass two values for an x and y axis. Making the default constructor shows that I have learned from the module and can implement my learning into my project.</p>
          <p>Started on the final grid print. I've abandoned the overloaded <code>=</code> operator. It didn't end up working, as I would have had to convert the grid to const char. I instead chose to print based off of the value, # for 0, * for 1. Then I replace the values of the path taken, with characters coresponding with the direction of the path.</p>

          <p>I'm having an error with, "Expression: vector subscript out of range". Currently working with AI to try and diagnose the issue. I believe there is an error with the validation function.</p>
          
          <p>Turns out the issue was alot simpler than I realised. When the grid initialisation to within the A* algorithm, I didn't move the grid creation into it though. That was the only real issue. I did readdress the validation to make sure it was within bounds anyways.</p>
          
          <div class="img-block">
            <img src="Screenshot 2026-03-17 155808.png" alt="Main file initialising destination and starting point" width="185" height="229"/>
            <div class="img-caption">// main.cpp — destination + start initialisation</div>
          </div>

          <p>I added colour to the output. This makes the output alot cleaner, and easier to read.</p>

          <div class="img-block">
            <img src="Screenshot 2026-03-17 155347.png" alt="Colour-coded console output" width="377" height="445"/>
            <div class="img-caption">// console output — colour-coded walls, path, and open cells</div>
          </div>
        </div>
      </div>
      
      <div class="entry">
        <div class="entry-date">&gt; 18/03/2026</div>
        <div class="entry-body">
          <p>I fixed the testing, as the test function for testing destination wasn't working. This means I have tests for validation, checking if the cell is a 1 or 0, and if the cell is the destination. I'm going to verify with Michelle that this is sufficient testing. I've got good tests. I'm using small unit tests which is better for catching bugs, as running the whole thing provides no granularity.</p>
          <p>Met with Michelle today. She recommended focusing on testing for the algorithm, and keeping the overloaded operator in the codebase even if unused — it demonstrates initiative and understanding of the course material.</p>
        </div>
      </div>

     <!-- ── Reports ── -->
      <div class="entry">
        <div class="entry-date">Report</div>
        <div class="entry-body">
          
          <p></p>

          <p>The way I was generating my grid, was by using <code>srand()</code> and modulo 2. This would give me either a 1 or a 0, which would be then used to fill the grid. This works, but it is "too random". It generates a grid that is very often impossible to navigate. This is why I chose to change the way the grid was generated.</p>

          <div class="img-block">
            <img src="Screenshot 2026-03-17 153436.png" alt="uniform_int_distribution code" width="468" height="205"/>
            <div class="img-caption">// createGrid() — std::uniform_int_distribution&lt;int&gt;(0,1)</div>
          </div>

          <p>I talked with Nathan about the grid generation, and he pointed me in the way of the <code>random</code> library. This library gave me access to <code>uniform_int_distribution</code>. This, in combination with <code>default_random_engine</code>, allows me to create a grid with a better distribution of 1s and 0s. This makes the grid far more solvable than the <code>srand</code> grid.</p>

          <div class="img-block">
            <img src="Screenshot 2026-03-17 153645.png" alt="Finished grid with path overlaid" width="359" height="402"/>
          </div>

          <div class="img-block">
            <img src="Screenshot 2026-03-17 155448.png" alt="Finished grid with path overlaid" width="359" height="402"/>
          </div>
          
          <p>In relation to issues that I currently have, and things that could be to make the project, there are a couple things. Within the A* function, I could've made a function to check the surrounding cells, instead of having it check each cell individually. This would reduce the stack size of the function, helping it run faster. I would also have liked to done more comprehensive unit tests. Testing the A* algorithm more, testing the calculations would have given me a smaller margin of error.</p>
          <p>As far as AI usage goes, I used it very minimally. I chose to not use it for writing the A* algorithm, as the GeeksforGeeks example was sufficient. I also wanted to show my abilities as a developer and show the knowledge I have gained throughout the module. I made the Grid class, all of the tests and the A* without the use of AI. I did however use it for debugging purposes and for printing the final grid. The printing of the final grid with the cell details was complicated, so I employed the use of AI for it. I had to debug it's work as it would write over itself and the characters for the pathing were inverted. I did also use AI to create the HTML and CSS for this GitHub page, as HTML is one of my least favourite things to do.</p>
        </div>
        <div class="img-block">
            <img src="Print.png" alt="Function replacing cells with characters" width="377" height="445"/>
            <div class="img-caption">// Function replacing cells with characters</div>
          </div>
      </div>
      <div class="entry">
        <div class="entry-date">References</div>
        <div class="entry-body">
          <p><a href="https://www.geeksforgeeks.org/dsa/a-search-algorithm/">A* Search Algorith, GeeksforGeeks</a></p>
          <p>Anthropic, Claude (AI)</p>
        </div>
      </div>
    </div><!-- /entries -->

    <div class="author-row">
      <div class="author-avatar">MC</div>
      <div>
        <div class="author-name">Mark Carroll</div>
        <div class="author-sub">Atlantic Technological University &nbsp;·&nbsp; C++ A* Project</div>
      </div>
    </div>

  </div><!-- /log-wrap -->
</div><!-- /page -->
</body>
</html>
