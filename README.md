<!DOCTYPE html>
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
    <div class="terminal-title">C-Project / devlog.html — Mark Carroll</div>
  </div>

  <div class="log-wrap">

    <div class="log-header">
      <div class="project-tag">ATU &nbsp;·&nbsp; C++ Project &nbsp;·&nbsp; 2026</div>
      <div class="project-title"># C-Project</div>
      <p class="project-sub">
        Development log and report for the C++ A* pathfinding project.
        The goal is to implement an A* algorithm to find the shortest route through a randomly generated maze.
        The Geeks4Geeks article on A* algorithms was a useful reference for the underlying theory.
      </p>
      <div class="tag-row">
        <span class="tag">C++</span>
        <span class="tag">A* algorithm</span>
        <span class="tag">pathfinding</span>
        <span class="tag info">Geeks4Geeks reference</span>
      </div>
    </div>

    <div class="entries">

      <!-- ── Entry 1 ── -->
      <div class="entry">
        <div class="entry-date">&gt; 04/03/2026</div>
        <div class="entry-body">
          <p>Working on applying concepts from the Matrices lab — specifically overloaded operators. The plan is to overload the <code>=</code> operator to duplicate the grid, so a separate copy can be printed with the path mapped out using directional characters <code>\ - / |</code>.</p>
          <p>Met with Michelle today. She recommended focusing on testing for the algorithm, and keeping the overloaded operator in the codebase even if unused — it demonstrates initiative and understanding of the course material.</p>
          <div class="callout blue">Plan: implement exceptions and error handling alongside unit tests for each core function (validation, destination arrival, etc.).</div>
          <div class="tag-row">
            <span class="tag">operator overloading</span>
            <span class="tag">testing</span>
            <span class="tag info">Michelle meeting</span>
          </div>
        </div>
      </div>

      <!-- ── Entry 2 ── -->
      <div class="entry">
        <div class="entry-date">&gt; 11/03/2026</div>
        <div class="entry-body">
          <p>Working on the test functions. A single <code>runAllTests()</code> will orchestrate everything. To avoid setting up a new grid for every test, a default constructor was added — works well enough that the main grid may be refactored to use it too.</p>
          <p>Started on the final grid print. Abandoned the second-grid approach; keeping the <code>=</code> operator for posterity to show the thought process. Now printing based on character type and cell details.</p>

          <div class="callout orange">
            Bug: "Expression: vector subscript out of range" — validation appeared to be checking coordinates outside the grid. Diagnosed with AI assistance; described as similar in nature to the CrowdStrike incident (a logic error in a boundary check).
          </div>

          <p>Turned out the root cause was simpler — <code>createGrid()</code> had been moved but never called at the new location. The grid was never being initialised. Validation logic was correct all along.</p>

          <p>I have the main file initialising the destination and starting point.</p>

          <div class="img-block">
            <img src="Screenshot 2026-03-17 155808.png" alt="Main file initialising destination and starting point" width="185" height="229"/>
            <div class="img-caption">// main.cpp — destination + start initialisation</div>
          </div>

          <p>Changed how the grid is generated. Previously using <code>srand()</code> for random generation. After talking with Nathan, switched to the <code>&lt;random&gt;</code> library.</p>

          <div class="img-block">
            <img src="Screenshot 2026-03-17 153436.png" alt="uniform_int_distribution code" width="468" height="205"/>
            <div class="img-caption">// createGrid() — std::uniform_int_distribution&lt;int&gt;(0,1)</div>
          </div>

          <div class="callout green">
            Now using <code>std::uniform_int_distribution</code> seeded with <code>std::random_device</code>, producing a proper uniform distribution of 1s and 0s each run.
          </div>

          <div class="img-block">
            <img src="Screenshot 2026-03-17 155448.png" alt="Finished grid with path overlaid" width="359" height="402"/>
            <div class="img-caption">// finished grid — path overlaid with directional characters</div>
          </div>

          <p>Colour output added to the console grid using <code>SetConsoleTextAttribute</code>. Directional path characters (<code>^ v &lt; &gt; / \</code>) now render on the finished grid. The result is clean and easy to read.</p>

          <div class="img-block">
            <img src="Screenshot 2026-03-17 155347.png" alt="Colour-coded console output" width="377" height="445"/>
            <div class="img-caption">// console output — colour-coded walls, path, and open cells</div>
          </div>

          <div class="tag-row">
            <span class="tag">testing</span>
            <span class="tag warn">bug fixed</span>
            <span class="tag">&lt;random&gt; library</span>
            <span class="tag">colour output</span>
            <span class="tag info">Nathan collab</span>
          </div>
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
